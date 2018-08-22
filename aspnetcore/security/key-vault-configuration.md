---
title: Proveedor de configuración de Azure Key Vault en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre-valor que se cargan en tiempo de ejecución.
ms.author: riande
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 829c6c7e2750879b51bf3ce8225c6e472900f2ad
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828516"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Proveedor de configuración de Azure Key Vault en ASP.NET Core

Por [Halter](https://github.com/guardrex) y [Andrew Stanton-Nurse](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ver o descargar el código de ejemplo para 2.x:

* [Ejemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([descarga](xref:tutorials/index#how-to-download-a-sample))-lee los valores de secreto en una aplicación.
* [Ejemplo de prefijo de nombre de clave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([descarga](xref:tutorials/index#how-to-download-a-sample)): los valores de secreto de lecturas con un prefijo de nombre de clave que representa la versión de una aplicación, que permite cargar un conjunto diferente de los valores de secreto para cada versión de la aplicación.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ver o descargar el código de ejemplo para 1.x:

* [Ejemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([descarga](xref:tutorials/index#how-to-download-a-sample))-lee los valores de secreto en una aplicación.
* [Ejemplo de prefijo de nombre de clave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([descarga](xref:tutorials/index#how-to-download-a-sample)): los valores de secreto de lecturas con un prefijo de nombre de clave que representa la versión de una aplicación, que permite cargar un conjunto diferente de los valores de secreto para cada versión de la aplicación.

---

Este documento explica cómo usar el [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) proveedor de configuración para cargar los valores de configuración de aplicación de secretos de Azure Key Vault. Azure Key Vault es un servicio basado en la nube que le ayuda a proteger claves criptográficas y secretos usados por aplicaciones y servicios. Escenarios comunes incluyen control de acceso a datos confidenciales de la configuración y cumple el requisito de FIPS 140-2 nivel 2 validado módulos de seguridad de Hardware (HSM) al almacenar datos de configuración. Esta característica está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.

## <a name="package"></a>Package

Para usar el proveedor, agregue una referencia a la [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paquete.

## <a name="app-configuration"></a>Configuración de la aplicación

Puede explorar el proveedor con el [aplicaciones de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Después de establecer un almacén de claves y cree los secretos en el almacén, las aplicaciones de ejemplo carga los valores de secreto en sus configuraciones de forma segura y mostrarlos en las páginas Web.

El proveedor se agrega a la configuración de la aplicación con el `AddAzureKeyVault` extensión. En las aplicaciones de ejemplo, la extensión usa tres valores de configuración cargados desde el *appsettings.json* archivo.

| Configuración de la aplicación    | Descripción                    | Ejemplo                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nombre del almacén de claves de Azure           | contosovault                                 |
| `ClientId`     | Identificador de aplicación de Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Clave de la aplicación de Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Creación de los secretos del almacén de claves y cargar los valores de configuración (ejemplo de basic)

1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Agregue secretos al almacén de claves con el [módulo de PowerShell AzureRM Key Vault](/powershell/module/azurerm.keyvault) disponibles desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), el [API de REST de Azure Key Vault](/rest/api/keyvault/), o el [Portal azure](https://portal.azure.com/). Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe usar el *Manual* opción para crear los secretos de par nombre-valor para su uso con el proveedor de configuración.
     * Secretos simples se crean como pares nombre / valor. Nombres de secretos de Azure Key Vault se limitan a caracteres alfanuméricos y guiones.
     * Usan valores jerárquicos (las secciones de configuración) `--` (dos guiones) como separador en el ejemplo. Dos puntos, que normalmente se utilizan para delimitar una sección de una subclave en [configuración de ASP.NET Core](xref:fundamentals/configuration/index), no se permiten en los nombres de secretos. Por lo tanto, son usa dos guiones y se intercambian en dos puntos cuando se cargan los secretos en la configuración de la aplicación.
     * Cree dos *Manual* secretos con los siguientes pares de nombre-valor. El secreto de la primera es un nombre simple y el valor y el secreto de la segunda crea un valor secreto con una sección y la subclave en el nombre del secreto:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registrar la aplicación de ejemplo con Azure Active Directory.
   * Autorizar a la aplicación para acceder al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` proporcionan el cmdlet de PowerShell para autorizar a la aplicación para tener acceso al almacén de claves `List` y `Get` acceso a los secretos con `-PermissionsToSecrets list,get`.

2. Actualización de la aplicación *appsettings.json* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre del secreto.
   * Valores que no son jerárquicos: el valor de `SecretName` se obtiene con `config["SecretName"]`.
   * Los valores jerárquicos (secciones): Use `:` notación (dos puntos) o el `GetSection` método de extensión. Para obtener el valor de configuración, use cualquiera de estos enfoques:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Al ejecutar la aplicación, una página Web muestra los valores de secreto cargados:

![Ventana del explorador que muestra los valores de secreto cargado mediante el proveedor de configuración de Azure Key Vault](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Creación de los secretos del almacén de claves con prefijo y cargar los valores de configuración (clave de nombre de prefijo ejemplo)

`AddAzureKeyVault` También proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, que le permite controlar los secretos del almacén clave se convierten en las claves de configuración. Por ejemplo, puede implementar la interfaz para cargar los valores de secreto según un valor de prefijo que se proporcione al iniciar la aplicación. Esto permite, por ejemplo, para cargar los secretos en función de la versión de la aplicación.

> [!WARNING]
> No utilizar prefijos en secretos del almacén de claves para colocar los secretos de varias aplicaciones en el mismo almacén de claves o colocar los secretos del entorno (por ejemplo, *desarrollo* frente a *producción* secretos) en la misma almacén. Se recomienda que diferentes aplicaciones y entornos de desarrollo y producción usan almacenes de claves independientes para aislar los entornos de aplicación para el nivel más alto de seguridad.

Con la segunda aplicación de ejemplo, crear un secreto en el almacén de claves para `5000-AppSecret` (no se permiten los períodos en los nombres de secretos del almacén de claves) que representa un secreto de la aplicación para la versión 5.0.0.0 de la aplicación. Otra versión, 5.1.0.0, creará un secreto para `5100-AppSecret`. Cada versión de la aplicación carga su propio valor secreto en su configuración como `AppSecret`, extracción, la versión cuando se cargue el secreto. La implementación del ejemplo se muestra a continuación:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

El `Load` se llama al método mediante un algoritmo de proveedor que recorre en iteración los secretos del almacén para buscar las que tienen el prefijo de la versión. Cuando se encuentra un prefijo de la versión con `Load`, el algoritmo utiliza el `GetKey` método para devolver el nombre de la configuración del nombre del secreto. Quita el prefijo de la versión del nombre del secreto y devuelve el resto del nombre del secreto para la carga en la configuración de la aplicación de pares nombre / valor.

Al implementar este enfoque:

1. Se cargan los secretos del almacén de claves.
2. El secreto de la cadena de `5000-AppSecret` coincide.
3. La versión, `5000` (con el guión), se quitan el nombre de clave dejando `AppSecret` carga con el valor del secreto en la configuración de la aplicación.

> [!NOTE]
> También puede proporcionar su propia `KeyVaultClient` implementación `AddAzureKeyVault`. Proporciona a un cliente personalizado le permite compartir una única instancia del cliente entre el proveedor de configuración y otras partes de la aplicación.

1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Agregue secretos al almacén de claves con el [módulo de PowerShell AzureRM Key Vault](/powershell/module/azurerm.keyvault) disponibles desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), el [API de REST de Azure Key Vault](/rest/api/keyvault/), o el [Portal azure](https://portal.azure.com/). Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe usar el *Manual* opción para crear los secretos de par nombre-valor para su uso con el proveedor de configuración.
     * Usan valores jerárquicos (las secciones de configuración) `--` (dos guiones) como separador.
     * Cree dos *Manual* secretos con los siguientes pares de nombre y valor:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrar la aplicación de ejemplo con Azure Active Directory.
   * Autorizar a la aplicación para acceder al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` proporcionan el cmdlet de PowerShell para autorizar a la aplicación para tener acceso al almacén de claves `List` y `Get` acceso a los secretos con `-PermissionsToSecrets list,get`.

2. Actualización de la aplicación *appsettings.json* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre de secreto con prefijo. En este ejemplo, el prefijo es la versión de la aplicación, que proporciona a los `PrefixKeyVaultSecretManager` cuando agregó el proveedor de configuración de Azure Key Vault. El valor de `AppSecret` se obtiene con `config["AppSecret"]`. La página Web generada por la aplicación muestra el valor cargado:

   ![Ventana del explorador que muestra un valor secreto cargado a través del proveedor de configuración de Azure Key Vault cuando la versión de la aplicación es 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Cambiar la versión del ensamblado en el archivo de proyecto de aplicación `5.0.0.0` a `5.1.0.0` y vuelva a ejecutar la aplicación. Esta vez, devuelve el valor del secreto es `5.1.0.0_secret_value`. La página Web generada por la aplicación muestra el valor cargado:

   ![Ventana del explorador que muestra un valor secreto cargado a través del proveedor de configuración de Azure Key Vault cuando la versión de la aplicación es 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Controlar el acceso a la ClientSecret

Use la [herramienta Secret Manager](xref:security/app-secrets) para mantener la `ClientSecret` fuera de un árbol de origen del proyecto. Con el Administrador de secretos, asocie los secretos de aplicación con un proyecto específico y compartirlos entre varios proyectos.

Al desarrollar una aplicación de .NET Framework en un entorno que admite los certificados, puede autenticarse en Azure Key Vault con un certificado X.509. Clave privada del certificado X.509 es administrada por el sistema operativo. Para obtener más información, consulte [autenticar con un certificado en lugar de un secreto de cliente](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Use la `AddAzureKeyVault` sobrecarga que acepta un `X509Certificate2` (`_env` en el ejemplo siguiente:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reloading-secrets"></a>Volver a cargar los secretos

Los secretos se almacenan en caché hasta que `IConfigurationRoot.Reload()` se llama. Ha expirado, deshabilitado, y actualizado los secretos del almacén de claves no se respeten la aplicación hasta que `Reload` se ejecuta.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secretos deshabilitados y expirados

Generar secretos expirados y deshabilitados una `KeyVaultClientException`. Para evitar que la aplicación genere, reemplace la aplicación o actualizar el secreto deshabilitado o expirado.

## <a name="troubleshooting"></a>Solución de problemas

Cuando no se puede cargar la configuración mediante el proveedor de la aplicación, se escribe un mensaje de error en la [infraestructura de registro de ASP.NET Core](xref:fundamentals/logging/index). Configuración de carga evitará que las condiciones siguientes:

* La aplicación no está configurada correctamente en Azure Active Directory.
* El almacén de claves no existe en Azure Key Vault.
* La aplicación no está autorizada para acceder al almacén de claves.
* La directiva de acceso no incluye `Get` y `List` permisos.
* En el almacén de claves, los datos de configuración (par nombre-valor) denominados incorrectamente, falta, deshabilitado o expirado.
* La aplicación tiene el nombre de almacén de claves incorrecto (`Vault`), Id. de aplicación de Azure AD (`ClientId`), o la clave de Azure AD (`ClientSecret`).
* La clave de Azure AD (`ClientSecret`) ha expirado.
* La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Almacén de claves](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentación del almacén de claves](/azure/key-vault/)
* [Para generar y transferir protegidas con HSM de claves para Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Clase KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
