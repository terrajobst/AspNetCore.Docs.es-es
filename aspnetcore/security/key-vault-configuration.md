---
title: Azure Key Vault proveedor de configuración en ASP.NET Core
author: guardrex
description: Aprenda a usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre y valor cargados en tiempo de ejecución.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/key-vault-configuration
ms.openlocfilehash: cc3894df4df169d941f54ef3dfad5d3e6f798aad
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007408"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Azure Key Vault proveedor de configuración en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [Andrew Stanton-enfermera](https://github.com/anurse)

En este documento se explica cómo usar el proveedor de configuración de [key Vault de Microsoft Azure](https://azure.microsoft.com/services/key-vault/) para cargar los valores de configuración de la aplicación de Azure Key Vault secretos. Azure Key Vault es un servicio basado en la nube que ayuda a proteger las claves criptográficas y los secretos que usan las aplicaciones y los servicios. Entre los escenarios comunes para usar Azure Key Vault con ASP.NET Core aplicaciones se incluyen:

* Controlar el acceso a los datos de configuración confidenciales.
* Cumplir los requisitos de los módulos de seguridad de hardware (HSM) de FIPS 140-2 nivel 2 validados al almacenar los datos de configuración.

Este escenario está disponible para las aplicaciones que tienen como destino ASP.NET Core 2,1 o posterior.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Paquetes

Para usar el proveedor de configuración de Azure Key Vault, agregue una referencia de paquete al paquete [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .

Para adoptar el escenario [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) , agregue una referencia de paquete al paquete [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .

> [!NOTE]
> En el momento de escribir este documento, la versión estable más reciente de `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, proporciona compatibilidad con [identidades administradas asignadas por el sistema](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work). La compatibilidad con *identidades administradas asignadas* por el usuario está disponible en el paquete `1.2.0-preview2`. En este tema se muestra el uso de identidades administradas por el sistema y la aplicación de ejemplo proporcionada usa la versión `1.0.3` del paquete `Microsoft.Azure.Services.AppAuthentication`.

## <a name="sample-app"></a>Aplicación de ejemplo

La aplicación de ejemplo se ejecuta en uno de los dos modos determinados por la instrucción `#define` en la parte superior del archivo *Program.CS* :

* `Certificate` &ndash; muestra el uso de un identificador de cliente Azure Key Vault y un certificado X. 509 para tener acceso a los secretos almacenados en Azure Key Vault. Esta versión del ejemplo se puede ejecutar desde cualquier ubicación, implementada en Azure App Service o cualquier host capaz de servir a una aplicación ASP.NET Core.
* `Managed` &ndash; muestra cómo usar [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación en Azure Key Vault con Azure ad autenticación sin credenciales almacenadas en el código o la configuración de la aplicación. Al usar identidades administradas para autenticar, no se requieren un identificador de aplicación Azure AD y una contraseña (secreto de cliente). La versión `Managed` del ejemplo debe implementarse en Azure. Siga las instrucciones de la sección [uso de las identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) .

Para obtener más información sobre cómo configurar una aplicación de ejemplo mediante las directivas de preprocesador (`#define`), consulte <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Almacenamiento de secretos en el entorno de desarrollo

Establezca secretos localmente mediante la [herramienta de administrador de secretos](xref:security/app-secrets). Cuando la aplicación de ejemplo se ejecuta en el equipo local en el entorno de desarrollo, los secretos se cargan desde el almacén del administrador secreto local.

La herramienta Administrador de secretos requiere una propiedad `<UserSecretsId>` en el archivo de proyecto de la aplicación. Establezca el valor de propiedad (`{GUID}`) en cualquier GUID único:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Los secretos se crean como pares de nombre y valor. Los valores jerárquicos (secciones de configuración) usan un `:` (dos puntos) como separador en ASP.NET Core nombres de clave de [configuración](xref:fundamentals/configuration/index) .

El administrador de secretos se usa desde un shell de comandos abierto a la [raíz del contenido](xref:fundamentals/index#content-root)del proyecto, donde `{SECRET NAME}` es el nombre y `{SECRET VALUE}` es el valor:

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Ejecute los siguientes comandos en un shell de comandos desde la [raíz del contenido](xref:fundamentals/index#content-root) del proyecto para establecer los secretos de la aplicación de ejemplo:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Cuando estos secretos se almacenan en Azure Key Vault en el [almacenamiento Secret en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección, el sufijo `_dev` se cambia a `_prod`. El sufijo proporciona una indicación visual en la salida de la aplicación que indica el origen de los valores de configuración.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Almacenamiento secreto en el entorno de producción con Azure Key Vault

Las instrucciones proporcionadas por el @no__t 0Quickstart: Establecer y recuperar un secreto de Azure Key Vault mediante el tema CLI de Azure @ no__t-0 se resumen aquí para crear un Azure Key Vault y almacenar secretos usados por la aplicación de ejemplo. Consulte el tema para obtener más detalles.

1. Abra Azure Cloud Shell con cualquiera de los métodos siguientes en el [Azure portal](https://portal.azure.com/):

   * Seleccione **pruébelo** en la esquina superior derecha de un bloque de código. Use la cadena de búsqueda "CLI de Azure" en el cuadro de texto.
   * Abra Cloud Shell en el explorador con el botón **iniciar Cloud Shell** .
   * Seleccione el botón **Cloud Shell** en el menú de la esquina superior derecha del Azure portal.

   Para obtener más información, consulte [interfaz de la línea de comandos de Azure (CLI)](/cli/azure/) e [información general de Azure Cloud Shell](/azure/cloud-shell/overview).

1. Si aún no está autenticado, inicie sesión con el comando `az login`.

1. Cree un grupo de recursos con el siguiente comando, donde `{RESOURCE GROUP NAME}` es el nombre del grupo de recursos para el nuevo grupo de recursos y `{LOCATION}` es la región de Azure (Datacenter):

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Cree un almacén de claves en el grupo de recursos con el siguiente comando, donde `{KEY VAULT NAME}` es el nombre del nuevo almacén de claves y `{LOCATION}` es la región de Azure (Datacenter):

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Cree secretos en el almacén de claves como pares de nombre y valor.

   Azure Key Vault los nombres de los secretos se limitan a caracteres alfanuméricos y guiones. Los valores jerárquicos (secciones de configuración) usan `--` (dos guiones) como separador. Los dos puntos, que se suelen usar para delimitar una sección de una subclave en [ASP.net Core configuración](xref:fundamentals/configuration/index), no se permiten en los nombres de secreto del almacén de claves. Por lo tanto, se usan dos guiones y se intercambian por dos puntos cuando se cargan los secretos en la configuración de la aplicación.

   Los siguientes secretos son para su uso con la aplicación de ejemplo. Los valores incluyen un sufijo `_prod` para distinguirlos de los valores de sufijo `_dev` cargados en el entorno de desarrollo de los secretos de usuario. Reemplace `{KEY VAULT NAME}` por el nombre del almacén de claves que creó en el paso anterior:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Usar el identificador de aplicación y el certificado X. 509 para aplicaciones no hospedadas en Azure

Configure Azure AD, Azure Key Vault y la aplicación para que use un identificador de aplicación Azure Active Directory y un certificado X. 509 para autenticarse en un almacén de claves **cuando la aplicación se hospeda fuera de Azure**. Para obtener más información, vea [acerca de las claves, los secretos y los certificados](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Aunque el uso de un identificador de aplicación y un certificado X. 509 es compatible con las aplicaciones hospedadas en Azure, se recomienda usar [identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) al hospedar una aplicación en Azure. Las identidades administradas no requieren el almacenamiento de un certificado en la aplicación o en el entorno de desarrollo.

La aplicación de ejemplo usa un identificador de aplicación y un certificado X. 509 cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Certificate`.

1. Cree un certificado de archivo PKCS # 12 ( *. pfx*). Las opciones para crear certificados incluyen [Makecert en Windows](/windows/desktop/seccrypto/makecert) y [OpenSSL](https://www.openssl.org/).
1. Instale el certificado en el almacén de certificados personales del usuario actual. Marcar la clave como exportable es opcional. Anote la huella digital del certificado, que se usa más adelante en este proceso.
1. Exporte el certificado de archivo PKCS # 12 ( *. pfx*) como un certificado codificado en der ( *. cer*).
1. Registre la aplicación con Azure AD (**registros de aplicaciones**).
1. Cargue el certificado codificado en DER ( *. cer*) en Azure ad:
   1. Seleccione la aplicación en Azure AD.
   1. Vaya a **certificados & Secrets**.
   1. Seleccione **upload Certificate (cargar certificado** ) para cargar el certificado, que contiene la clave pública. Un certificado *. cer*, *. pem*o *. CRT* es aceptable.
1. Almacene el nombre del almacén de claves, el identificador de la aplicación y la huella digital del certificado en el archivo *appSettings. JSON* de la aplicación.
1. Vaya a **almacenes de claves** en el Azure portal.
1. Seleccione el almacén de claves que creó en el [almacenamiento de secretos en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección.
1. Seleccione **directivas de acceso**.
1. Seleccione **Agregar nuevo**.
1. Seleccione **seleccionar entidad** de seguridad y seleccione la aplicación registrada por nombre. Seleccione el botón **seleccionar** .
1. Abra los **permisos de secreto** y proporcione la aplicación con los permisos **Get** y **List** .
1. Seleccione **Aceptar**.
1. Seleccione **Guardar**.
1. Implemente la aplicación.

La aplicación de ejemplo `Certificate` obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el secreto:

* Valores no jerárquicos: El valor de `SecretName` se obtiene con `config["SecretName"]`.
* Valores jerárquicos (secciones): Use la notación `:` (dos puntos) o el método de extensión `GetSection`. Use cualquiera de estos métodos para obtener el valor de configuración:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

El certificado X. 509 se administra mediante el sistema operativo. La aplicación llama a `AddAzureKeyVault` con los valores proporcionados por el archivo *appSettings. JSON* :

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

Valores de ejemplo:

* Nombre del almacén de claves: `contosovault`
* IDENTIFICADOR de la aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`
* Huella digital del certificado: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appsettings.json*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web. En el entorno de desarrollo, los valores secretos se cargan con el sufijo `_dev`. En el entorno de producción, los valores se cargan con el sufijo `_prod`.

## <a name="use-managed-identities-for-azure-resources"></a>Uso de identidades administradas para los recursos de Azure

**Una aplicación implementada en Azure** puede aprovechar las [identidades administradas de los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview), lo que permite que la aplicación se autentique con Azure Key Vault mediante la autenticación de Azure ad sin credenciales (identificador de aplicación y contraseña/secreto de cliente). almacenado en la aplicación.

La aplicación de ejemplo usa identidades administradas para los recursos de Azure cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Managed`.

Escriba el nombre del almacén en el archivo *appSettings. JSON* de la aplicación. La aplicación de ejemplo no requiere un identificador de aplicación y una contraseña (secreto de cliente) cuando se establece en la versión `Managed`, por lo que puede omitir esas entradas de configuración. La aplicación se implementa en Azure y Azure autentica la aplicación para acceder a Azure Key Vault solo mediante el nombre del almacén almacenado en el archivo *appSettings. JSON* .

Implemente la aplicación de ejemplo en Azure App Service.

Una aplicación implementada en Azure App Service se registra automáticamente con Azure AD cuando se crea el servicio. Obtenga el identificador de objeto de la implementación para usarlo en el siguiente comando. El identificador de objeto se muestra en el Azure Portal en el panel **identidad** del App Service.

Con CLI de Azure y el identificador de objeto de la aplicación, proporcione la aplicación con los permisos `list` y `get` para acceder al almacén de claves:

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Reinicie la aplicación** con CLI de Azure, PowerShell o el Azure portal.

La aplicación de ejemplo:

* Crea una instancia de la clase `AzureServiceTokenProvider` sin una cadena de conexión. Cuando no se proporciona una cadena de conexión, el proveedor intenta obtener un token de acceso de identidades administradas para los recursos de Azure.
* Se crea un nuevo `KeyVaultClient` con la devolución de llamada del token de instancia `AzureServiceTokenProvider`.
* La instancia de `KeyVaultClient` se utiliza con una implementación predeterminada de `IKeyVaultSecretManager` que carga todos los valores secretos y reemplaza los dobles guiones (`--`) por dos puntos (`:`) en los nombres de clave.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web. En el entorno de desarrollo, los valores secretos tienen el sufijo `_dev` porque son proporcionados por los secretos del usuario. En el entorno de producción, los valores se cargan con el sufijo `_prod` porque los proporciona Azure Key Vault.

Si recibe un error `Access denied`, confirme que la aplicación está registrada con Azure AD y que proporciona acceso al almacén de claves. Confirme que ha reiniciado el servicio en Azure.

## <a name="use-a-key-name-prefix"></a>Usar un prefijo de nombre de clave

`AddAzureKeyVault` proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, lo que permite controlar cómo se convierten los secretos del almacén de claves en claves de configuración. Por ejemplo, puede implementar la interfaz para cargar valores de secreto basados en un valor de prefijo que se proporciona al inicio de la aplicación. Esto le permite, por ejemplo, cargar secretos en función de la versión de la aplicación.

> [!WARNING]
> No use prefijos en secretos del almacén de claves para colocar secretos para varias aplicaciones en el mismo almacén de claves o para colocar secretos de entorno (por ejemplo, *desarrollo* frente a secretos de *producción* ) en el mismo almacén. Se recomienda que las distintas aplicaciones y entornos de desarrollo y producción usen almacenes de claves independientes para aislar los entornos de aplicación para obtener el máximo nivel de seguridad.

En el siguiente ejemplo, se establece un secreto en el almacén de claves (y el uso de la herramienta de administrador de secretos para el entorno de desarrollo) para `5000-AppSecret` (no se permiten puntos en los nombres de secreto del almacén de claves). Este secreto representa un secreto de aplicación para la versión 5.0.0.0 de la aplicación. Para otra versión de la aplicación, 5.1.0.0, se agrega un secreto al almacén de claves (y con la herramienta de administración de secretos) para `5100-AppSecret`. Cada versión de la aplicación carga su valor de secreto con versión en su configuración como `AppSecret`, eliminando la versión mientras carga el secreto.

se llama a `AddAzureKeyVault` con un @no__t personalizado-1:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

La implementación de `IKeyVaultSecretManager` reacciona a los prefijos de versión de los secretos para cargar el secreto adecuado en la configuración:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

Un algoritmo de proveedor llama al método `Load`, que recorre en iteración los secretos del almacén para encontrar los que tienen el prefijo de versión. Cuando se encuentra un prefijo de versión con `Load`, el algoritmo utiliza el método `GetKey` para devolver el nombre de la configuración del nombre de secreto. Elimina el prefijo de versión del nombre del secreto y devuelve el resto del nombre de secreto para cargarlo en los pares de nombre y valor de configuración de la aplicación.

Cuando se implementa este enfoque:

1. La versión de la aplicación especificada en el archivo de proyecto de la aplicación. En el ejemplo siguiente, la versión de la aplicación se establece en `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Confirme que hay una propiedad `<UserSecretsId>` en el archivo de proyecto de la aplicación, donde `{GUID}` es un GUID proporcionado por el usuario:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Guarde los siguientes secretos localmente con la [herramienta de administración de secretos](xref:security/app-secrets):

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Los secretos se guardan en Azure Key Vault mediante los siguientes comandos de CLI de Azure:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Cuando se ejecuta la aplicación, se cargan los secretos del almacén de claves. El secreto de cadena para `5000-AppSecret` coincide con la versión de la aplicación especificada en el archivo de proyecto de la aplicación (`5.0.0.0`).

1. La versión, `5000` (con el guión), se elimina del nombre de la clave. En toda la aplicación, la lectura de la configuración con la clave `AppSecret` carga el valor del secreto.

1. Si se cambia la versión de la aplicación en el archivo de proyecto a `5.1.0.0` y se vuelve a ejecutar la aplicación, el valor de secreto devuelto es `5.1.0.0_secret_value_dev` en el entorno de desarrollo y @no__t 2 en producción.

> [!NOTE]
> También puede proporcionar su propia implementación `KeyVaultClient` a `AddAzureKeyVault`. Un cliente personalizado permite compartir una única instancia del cliente a través de la aplicación.

## <a name="bind-an-array-to-a-class"></a>Enlace de una matriz a una clase

El proveedor es capaz de leer valores de configuración en una matriz para enlazar a una matriz POCO.

Al leer desde un origen de configuración que permite que las claves contengan separadores de dos puntos (`:`), se usa un segmento de clave numérica para distinguir las claves que componen una matriz (`:0:`, `:1:`,... `:{n}:`). Para obtener más información, vea [Configuration: Enlazar una matriz a una clase @ no__t-0.

Azure Key Vault teclas no pueden usar un signo de dos puntos como separador. El enfoque que se describe en este tema utiliza los guiones dobles (`--`) como separador de valores jerárquicos (secciones). Las claves de matriz se almacenan en Azure Key Vault con dobles guiones y segmentos de clave numérica (`--0--`, `--1--`, &hellip; `--{n}--`).

Examine la siguiente configuración de proveedor de registro de [Serilog](https://serilog.net/) proporcionada por un archivo JSON. Hay dos literales de objeto definidos en la matriz `WriteTo` que reflejan dos *receptores*Serilog, que describen los destinos para el registro de la salida:

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

La configuración que se muestra en el archivo JSON anterior se almacena en Azure Key Vault mediante la notación de doble guión (`--`) y los segmentos numéricos:

| Key | Valor |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Volver a cargar los secretos

Los secretos se almacenan en caché hasta que se llama a `IConfigurationRoot.Reload()`. La aplicación no respeta los secretos expirados, deshabilitados y actualizados en el almacén de claves hasta que se ejecute `Reload`.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secretos deshabilitados y expirados

Los secretos deshabilitados y expirados inician una `KeyVaultClientException` en tiempo de ejecución. Para evitar que se inicie la aplicación, proporcione la configuración mediante un proveedor de configuración diferente o actualice el secreto deshabilitado o expirado.

## <a name="troubleshoot"></a>Solucionar problemas

Cuando la aplicación no carga la configuración mediante el proveedor, se escribe un mensaje de error en la [infraestructura de registro de ASP.net Core](xref:fundamentals/logging/index). Las siguientes condiciones impedirán que se cargue la configuración:

* La aplicación o el certificado no están configurados correctamente en Azure Active Directory.
* El almacén de claves no existe en Azure Key Vault.
* La aplicación no está autorizada para acceder al almacén de claves.
* La Directiva de acceso no incluye los permisos `Get` y `List`.
* En el almacén de claves, los datos de configuración (par nombre-valor) tienen un nombre incorrecto, faltan, están deshabilitados o han expirado.
* La aplicación tiene un nombre de almacén de claves incorrecto (`KeyVaultName`), Azure AD ID. de aplicación (`AzureADApplicationId`) o Azure AD huella digital del certificado (`AzureADCertThumbprint`).
* La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault @ no__t-0
* [Microsoft Azure: Key Vault documentación @ no__t-0
* [Cómo generar y transferir claves protegidas con HSM para Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Clase KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Inicio rápido: Establecimiento y recuperación de un secreto desde Azure Key Vault mediante una aplicación Web de .NET @ no__t-0
* [Tutorial: Cómo usar Azure Key Vault con máquinas virtuales Windows de Azure en. NET @ no__t-0
