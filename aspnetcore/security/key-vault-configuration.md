---
title: Proveedor de configuración de almacén de claves de Azure en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre-valor que se cargan en tiempo de ejecución.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/22/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 2188929d6f380327465e8ce0fd8ad659188416d3
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743990"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Proveedor de configuración de almacén de claves de Azure en ASP.NET Core

Por [Halter](https://github.com/guardrex) y [Andrew Stanton-Nurse](https://github.com/anurse)

Este documento explica cómo usar el [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) proveedor de configuración para cargar los valores de configuración de aplicación de secretos de Azure Key Vault. Azure Key Vault es un servicio basado en la nube que ayuda a proteger claves criptográficas y secretos usados por aplicaciones y servicios. Escenarios comunes para usar Azure Key Vault con aplicaciones ASP.NET Core son:

* Controlar el acceso a datos confidenciales de la configuración.
* Cumple el requisito de FIPS 140-2 nivel 2 había validado módulos de seguridad de Hardware (HSM) al almacenar datos de configuración.

Este escenario está disponible para las aplicaciones que tienen como destino ASP.NET Core 2.1 o posterior.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Paquetes

Para usar el proveedor de configuración de Azure Key Vault, agregue una referencia de paquete para el [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paquete.

Adoptar la [administra las identidades de los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) escenario, agregue una referencia de paquete a la [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paquete.

> [!NOTE]
> En el momento de escribir la última versión estable de `Microsoft.Azure.Services.AppAuthentication`, versión `1.0.3`, proporciona compatibilidad para [asignado por el sistema administra identidades](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka). Compatibilidad con *asignada por el usuario administra las identidades* está disponible en el `1.0.2-preview` paquete. En este tema se muestra el uso de identidades administradas por el sistema y la aplicación de ejemplo proporcionada usa la versión `1.0.3` de la `Microsoft.Azure.Services.AppAuthentication` paquete.

## <a name="sample-app"></a>Aplicación de ejemplo

La aplicación de ejemplo se ejecuta en cualquiera de los dos modos según la `#define` instrucción en la parte superior de la *Program.cs* archivo:

* `Basic` &ndash; Muestra el uso de un identificador de la aplicación de Azure Key Vault y una contraseña (secreto de cliente) para tener acceso a los secretos almacenados en el almacén de claves. Implementar el `Basic` versión del ejemplo en cualquier host capaz de ofrecer servicio a una aplicación ASP.NET Core. Siga las instrucciones de la [Id. de aplicación de uso y el secreto de cliente para aplicaciones no hospedadas en Azure](#use-application-id-and-client-secret-for-non-azure-hosted-apps) sección.
* `Managed` &ndash; Muestra cómo usar [administra las identidades de los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación a Azure Key Vault con la autenticación de Azure AD sin credenciales almacenadas en el código o configuración de la aplicación. Cuando se usa identidades administradas para autenticar, un identificador de aplicación de Azure AD y una contraseña (secreto de cliente) no se requieren. El `Managed` versión del ejemplo debe implementarse en Azure. Siga las instrucciones de la [utilizar las identidades administrada para recursos de Azure](#use-managed-identities-for-azure-resources) sección.

Para obtener más información sobre cómo configurar una aplicación de ejemplo con las directivas de preprocesador (`#define`), consulte <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Almacenamiento de secreto en el entorno de desarrollo

Establecer secretos localmente mediante el [herramienta Secret Manager](xref:security/app-secrets). Cuando se ejecuta la aplicación de ejemplo en el equipo local en el entorno de desarrollo, se cargan los secretos del almacén local del Administrador de secretos.

La herramienta Secret Manager requiere un `<UserSecretsId>` propiedad en el archivo de proyecto de la aplicación. Establezca el valor de propiedad (`{GUID}`) a cualquier GUID único:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Los secretos se crean como pares nombre / valor. Usan valores jerárquicos (las secciones de configuración) un `:` (dos puntos) como separador en [configuración de ASP.NET Core](xref:fundamentals/configuration/index) los nombres de clave.

Se utiliza el Administrador de secretos desde un shell de comandos que se abrió a raíz del contenido del proyecto, donde `{SECRET NAME}` es el nombre y `{SECRET VALUE}` es el valor:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Ejecute los siguientes comandos en un shell de comandos desde la raíz del contenido del proyecto para establecer los secretos de la aplicación de ejemplo:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Cuando estos secretos se almacenan en Azure Key Vault en el [almacenamiento secreto en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección, el `_dev` sufijo se cambia a `_prod`. El sufijo proporciona una indicación visual en la salida de la aplicación que indica el origen de los valores de configuración.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Almacenamiento de secreto en el entorno de producción con Azure Key Vault

Las instrucciones proporcionadas por el [inicio rápido: Establecer y recuperar un secreto de Azure Key Vault mediante CLI de Azure](/azure/key-vault/quick-create-cli) tema se resumen a continuación para crear una instancia de Azure Key Vault y almacenar secretos usados por la aplicación de ejemplo. Consulte el tema para obtener más detalles.

1. Abrir shell en la nube de Azure mediante cualquiera de los métodos siguientes en el [portal Azure](https://portal.azure.com/):

   * Seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. Utilice la cadena de búsqueda "CLI de Azure" en el cuadro de texto.
   * Abra Cloud Shell en el explorador con el **iniciar Cloud Shell** botón.
   * Seleccione el **Cloud Shell** botón en el menú de la esquina superior derecha de Azure portal.

   Para obtener más información, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) y [información general de Azure Cloud Shell](/azure/cloud-shell/overview).

1. Si no están ya se ha autenticado, inicie sesión con la `az login` comando.

1. Cree un grupo de recursos con el comando siguiente, donde `{RESOURCE GROUP NAME}` es el nombre del grupo de recursos para el nuevo grupo de recursos y `{LOCATION}` es la región de Azure (centro de datos):

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Crear un almacén de claves en el grupo de recursos con el comando siguiente, donde `{KEY VAULT NAME}` es el nombre para el nuevo almacén de claves y `{LOCATION}` es la región de Azure (centro de datos):

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Cree los secretos del almacén de claves como pares nombre / valor.

   Nombres de secretos de Azure Key Vault se limitan a caracteres alfanuméricos y guiones. Usan valores jerárquicos (las secciones de configuración) `--` (dos guiones) como separador. Dos puntos, que normalmente se utilizan para delimitar una sección de una subclave en [configuración de ASP.NET Core](xref:fundamentals/configuration/index), no se permiten en los nombres de secretos del almacén de claves. Por lo tanto, son usa dos guiones y se intercambian en dos puntos cuando se cargan los secretos en la configuración de la aplicación.

   Los secretos siguientes son para su uso con la aplicación de ejemplo. Los valores incluyen un `_prod` sufijo para distinguirlos de los `_dev` sufijo valores cargados en el entorno de desarrollo de secretos del usuario. Reemplace `{KEY VAULT NAME}` con el nombre del almacén de claves que creó en el paso anterior:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a>Use el Id. de aplicación y secreto de cliente para aplicaciones no hospedadas en Azure

Configurar Azure AD, Azure Key Vault y la aplicación utiliza un identificador de la aplicación y una contraseña (secreto de cliente) para autenticarse en un almacén de claves **cuando la aplicación se hospeda fuera de Azure**.

> [!NOTE]
> Aunque se admite el uso de un identificador de la aplicación y una contraseña (secreto de cliente) para las aplicaciones hospedadas en Azure, se recomienda usar [administra las identidades de los recursos de Azure](#use-managed-identities-for-azure-resources) al hospedar una aplicación en Azure. Las identidades administradas no requiere almacenar las credenciales de la aplicación o su configuración, por lo que se considera como un enfoque suele ser más seguro.

La aplicación de ejemplo usa un identificador de la aplicación y una contraseña (secreto de cliente) cuando el `#define` instrucción en la parte superior de la *Program.cs* archivo se establece en `Basic`.

1. Registrar la aplicación con Azure AD y establecer una contraseña (secreto de cliente) para la identidad de la aplicación.
1. Store el nombre del almacén de claves, Id. de aplicación y secreto de cliente/contraseña en la aplicación *appsettings.json* archivo.
1. Vaya a **los almacenes de claves** en Azure portal.
1. Seleccione el almacén de claves que creó en el [almacenamiento secreto en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección.
1. Seleccione **las directivas de acceso**.
1. Seleccione **Agregar nuevo**.
1. Seleccione **seleccione entidad** y seleccione la aplicación registrada por nombre. Seleccione el **seleccione** botón.
1. Abra **permisos de secretos** y proporcione la aplicación con **obtener** y **lista** permisos.
1. Seleccione **Aceptar**.
1. Seleccione **Guardar**.
1. Implementar la aplicación.

El `Basic` aplicación de ejemplo obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre del secreto:

* Valores que no son jerárquicos: El valor de `SecretName` se obtiene con `config["SecretName"]`.
* Valores jerárquicos (secciones): Use `:` notación (dos puntos) o el `GetSection` método de extensión. Para obtener el valor de configuración, use cualquiera de estos enfoques:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Las llamadas de aplicación `AddAzureKeyVault` con los valores proporcionados por el *appsettings.json* archivo:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

Valores de ejemplo:

* Nombre de almacén de claves: `contosovault`
* Id. de aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`
* Contraseña: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

*appsettings.json*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Al ejecutar la aplicación, una página Web muestra los valores de secreto cargados. En el entorno de desarrollo, los valores de secreto de carga con el `_dev` sufijo. En el entorno de producción, los valores de carga con el `_prod` sufijo.

## <a name="use-managed-identities-for-azure-resources"></a>Usar identidades administrada para recursos de Azure

**Una aplicación implementada en Azure** puede sacar partido de [administra las identidades de los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview), lo que permite que la aplicación para autenticarse con Azure Key Vault mediante la autenticación de Azure AD sin credenciales (identificador de la aplicación y Password/Client secreto) almacenados en la aplicación.

La aplicación de ejemplo usa las identidades de administrada para recursos de Azure cuando la `#define` instrucción en la parte superior de la *Program.cs* archivo se establece en `Managed`.

Escriba el nombre del almacén en la aplicación *appsettings.json* archivo. La aplicación de ejemplo no requiere un identificador de la aplicación y una contraseña (secreto de cliente) cuando se establece en el `Managed` versión, por lo que puede pasar por alto esas entradas de configuración. La aplicación se implementa en Azure y Azure autentica la aplicación para acceder a Azure Key Vault usando solo el nombre del almacén se almacena en el *appsettings.json* archivo.

Implemente la aplicación de ejemplo en Azure App Service.

Una aplicación implementada en Azure App Service se registra automáticamente con Azure AD cuando se crea el servicio. Obtener el identificador de objeto de la implementación para su uso en el siguiente comando. El identificador de objeto se muestra en el portal de Azure en el **identidad** panel de App Service.

Uso de CLI de Azure y el Id. de objeto de la aplicación, proporcione la aplicación con `list` y `get` permisos para tener acceso al almacén de claves:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Reinicie la aplicación** mediante Azure portal, PowerShell o CLI de Azure.

La aplicación de ejemplo:

* Crea una instancia de la `AzureServiceTokenProvider` clase sin una cadena de conexión. Cuando no se proporciona una cadena de conexión, el proveedor intenta obtener un token de acceso de identidades de administrada para recursos de Azure.
* Un nuevo `KeyVaultClient` se crea con el `AzureServiceTokenProvider` devolución de llamada de token de instancia.
* El `KeyVaultClient` instancia se utiliza con una implementación predeterminada de `IKeyVaultSecretManager` que carga todos los valores de secreto y reemplaza dos guiones (`--`) con dos puntos (`:`) en los nombres de clave.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Al ejecutar la aplicación, una página Web muestra los valores de secreto cargados. En el entorno de desarrollo, los valores de secreto tienen el `_dev` sufijo porque le proporciona los secretos del usuario. En el entorno de producción, los valores de carga con el `_prod` sufijo porque le proporciona Azure Key Vault.

Si recibe un `Access denied` error, confirme que la aplicación está registrada con Azure AD y proporciona acceso al almacén de claves. Confirme que ha reiniciado el servicio en Azure.

## <a name="use-a-key-name-prefix"></a>Utilizar un prefijo de nombre de clave

`AddAzureKeyVault` Proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, que le permite controlar los secretos del almacén clave se convierten en las claves de configuración. Por ejemplo, puede implementar la interfaz para cargar los valores de secreto según un valor de prefijo que se proporcione al iniciar la aplicación. Esto permite, por ejemplo, para cargar los secretos en función de la versión de la aplicación.

> [!WARNING]
> No utilizar prefijos en secretos del almacén de claves para colocar los secretos de varias aplicaciones en el mismo almacén de claves o colocar los secretos del entorno (por ejemplo, *desarrollo* frente a *producción* secretos) en la misma almacén. Se recomienda que diferentes aplicaciones y entornos de desarrollo y producción usan almacenes de claves independientes para aislar los entornos de aplicación para el nivel más alto de seguridad.

En el ejemplo siguiente, se establece un secreto en la clave de almacén (y utilizar la herramienta Secret Manager para el entorno de desarrollo) para `5000-AppSecret` (no se permiten los períodos en los nombres de secretos del almacén de claves). Este secreto representa un secreto de la aplicación para la versión 5.0.0.0 de la aplicación. Por otra versión de la aplicación, 5.1.0.0, se agrega un secreto a la clave de almacén (y utilizar la herramienta Secret Manager) para `5100-AppSecret`. Cada versión de la aplicación carga su valor secreto con control de versiones en su configuración como `AppSecret`, extracción, la versión cuando se cargue el secreto.

`AddAzureKeyVault` se llama con una personalizada `IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

Proporcionan los valores de nombre de almacén de claves, el identificador de aplicación y contraseña (secreto de cliente) la *appsettings.json* archivo:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Valores de ejemplo:

* Nombre de almacén de claves: `contosovault`
* Id. de aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`
* Contraseña: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

El `IKeyVaultSecretManager` reacciona ante los prefijos de la versión de secretos para cargar el secreto adecuado en la configuración de implementación:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

El `Load` se llama al método mediante un algoritmo de proveedor que recorre en iteración los secretos del almacén para buscar las que tienen el prefijo de la versión. Cuando se encuentra un prefijo de la versión con `Load`, el algoritmo utiliza el `GetKey` método para devolver el nombre de la configuración del nombre del secreto. Quita el prefijo de la versión del nombre del secreto y devuelve el resto del nombre del secreto para la carga en la configuración de la aplicación de pares nombre / valor.

Cuando se implementa este enfoque:

1. Versión de la aplicación especificada en el archivo de proyecto de la aplicación. En el ejemplo siguiente, se establece la versión de la aplicación en `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Confirme que un `<UserSecretsId>` propiedad está presente en el archivo de proyecto de la aplicación, donde `{GUID}` es un GUID proporcionado por el usuario:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Guardar los secretos siguientes localmente con el [herramienta Secret Manager](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Los secretos se guardan en Azure Key Vault mediante los siguientes comandos de CLI de Azure:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Cuando se ejecuta la aplicación, se cargan los secretos del almacén de claves. El secreto de la cadena de `5000-AppSecret` se asocia a la versión de la aplicación especificada en el archivo de proyecto de la aplicación (`5.0.0.0`).

1. La versión, `5000` (con el guión), se elimina del nombre de clave. A lo largo de la aplicación, lee la configuración con la clave `AppSecret` carga el valor del secreto.

1. Si se cambia la versión de la aplicación en el archivo de proyecto `5.1.0.0` y se vuelve a ejecutar la aplicación, el valor secreto devuelto es `5.1.0.0_secret_value_dev` en el entorno de desarrollo y `5.1.0.0_secret_value_prod` en producción.

> [!NOTE]
> También puede proporcionar su propia `KeyVaultClient` implementación `AddAzureKeyVault`. Un cliente personalizado permite compartir una única instancia del cliente a través de la aplicación.

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a>Autenticarse en Azure Key Vault con un certificado X.509

Al desarrollar una aplicación de .NET Framework en un entorno que admite los certificados, puede autenticarse en Azure Key Vault con un certificado X.509. Clave privada del certificado X.509 es administrada por el sistema operativo. Para obtener más información, consulte [autenticar con un certificado en lugar de un secreto de cliente](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Use la `AddAzureKeyVault` sobrecarga que acepta un `X509Certificate2` (`_env` en el ejemplo siguiente:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["KeyVaultName"],
    builtConfig["AzureADApplicationId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a>Enlace de una matriz a una clase

El proveedor es capaz de leer los valores de configuración en una matriz para el enlace a una matriz POCO.

Al leer desde un origen de configuración que permite que las claves que contiene dos puntos (`:`) separadores, un segmento clave numérico se usa para distinguir las claves que constituyen una matriz (`:0:`, `:1:`,... `:{n}:`). Para obtener más información, consulte [configuración: Enlazar una matriz a una clase](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Claves de Azure Key Vault no pueden usar un coma como separador. El enfoque descrito en este tema usa los guiones dobles (`--`) como separador para valores jerárquicos (secciones). Las claves de matriz se almacenan en Azure Key Vault con doble guiones y segmentos de clave numéricas (`--0--`, `--1--`,... `--{n}--`).

Examine el siguiente [Serilog](https://serilog.net/) proporcionado por un archivo JSON de configuración de proveedor de registro. Hay dos objetos literales definidos en el `WriteTo` matriz que se reflejan dos Serilog *receptores*, que describen los destinos de salida del registro:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

La configuración se muestra en el archivo JSON anterior se almacena en Azure Key Vault con guión doble (`--`) numérica y notación de segmentos:

| Key | Valor |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Volver a cargar los secretos

Los secretos se almacenan en caché hasta que `IConfigurationRoot.Reload()` se llama. Ha expirado, deshabilitado, y actualizado los secretos del almacén de claves no se respeten la aplicación hasta que `Reload` se ejecuta.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secretos deshabilitados y expirados

Generar secretos expirados y deshabilitados una `KeyVaultClientException`. Para evitar que la aplicación genere, reemplace la aplicación o actualizar el secreto deshabilitado o expirado.

## <a name="troubleshoot"></a>Solucionar problemas

Cuando no se puede cargar la configuración mediante el proveedor de la aplicación, se escribe un mensaje de error en la [infraestructura de registro de ASP.NET Core](xref:fundamentals/logging/index). Configuración de carga evitará que las condiciones siguientes:

* La aplicación no está configurada correctamente en Azure Active Directory.
* El almacén de claves no existe en Azure Key Vault.
* La aplicación no está autorizada para acceder al almacén de claves.
* La directiva de acceso no incluye `Get` y `List` permisos.
* En el almacén de claves, los datos de configuración (par nombre-valor) denominados incorrectamente, falta, deshabilitado o expirado.
* La aplicación tiene el nombre de almacén de claves incorrecto (`KeyVaultName`), Id. de aplicación de Azure AD (`AzureADApplicationId`), o la contraseña (secreto de cliente) de Azure AD (`AzureADPassword`).
* La contraseña (secreto de cliente) de Azure AD (`AzureADPassword`) ha expirado.
* La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentación de Key Vault](/azure/key-vault/)
* [Para generar y transferir protegidas con HSM de claves para Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Clase KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
