---
title: "Proveedor de configuración de almacén de claves de Azure"
author: guardrex
description: "Obtenga información acerca de cómo usar el proveedor de configuración de almacén de claves de Azure para configurar una aplicación con pares de nombre / valor que se carga en tiempo de ejecución."
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: 1a91a87fb90d4d4651e07f32415e4364c8e2d993
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2018
---
# <a name="azure-key-vault-configuration-provider"></a>Proveedor de configuración de almacén de claves de Azure

Por [Luke Latham](https://github.com/guardrex) y [Andrew Stanton-enfermera](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ver o descargar el código de ejemplo para 2.x:

* [Ejemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))-lee los valores de secreto en una aplicación.
* [Ejemplo de prefijo de nombre de clave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)): valores secretos lecturas utilizando un prefijo de nombre de la clave que representa la versión de una aplicación, lo que permite cargar un conjunto diferente de valores secretos para cada versión de la aplicación.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ver o descargar el código de ejemplo para 1.x:

* [Ejemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))-lee los valores de secreto en una aplicación.
* [Ejemplo de prefijo de nombre de clave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)): valores secretos lecturas utilizando un prefijo de nombre de la clave que representa la versión de una aplicación, lo que permite cargar un conjunto diferente de valores secretos para cada versión de la aplicación. 

---

Este documento explica cómo utilizar el [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) proveedor de configuración para cargar valores de configuración de aplicación de secretos del almacén de claves de Azure. Almacén de claves de Azure es un servicio basado en la nube que le ayuda a proteger las claves criptográficas y secretos usados por aplicaciones y servicios. Escenarios comunes incluyen controlar el acceso a datos confidenciales de la configuración y satisfacer el requisito para FIPS 140-2 nivel 2 validar módulos de seguridad de Hardware (HSM) al almacenar los datos de configuración. Esta característica está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.

## <a name="package"></a>Package
Para usar el proveedor, agregue una referencia a la [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paquete.

## <a name="application-configuration"></a>Configuración de aplicación
Puede explorar el proveedor con el [aplicaciones de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Una vez que establezca un almacén de claves y crear secretos en el almacén, las aplicaciones de ejemplo segura cargar los valores de secreto en sus configuraciones y mostrarlos en las páginas Web.

El proveedor se agrega a la `ConfigurationBuilder` con el `AddAzureKeyVault` extensión. En las aplicaciones de ejemplo, la extensión utiliza tres valores de configuración cargados desde el *appSettings.JSON que se* archivo.

| Configuración de la aplicación    | Descripción                    | Ejemplo                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nombre del almacén de claves de Azure           | contosovault                                 |
| `ClientId`     | Id. de aplicación de Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Clave de aplicación de Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Creación de secretos del almacén de claves y cargar los valores de configuración (ejemplo de basic)
1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con el almacén de claves de Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Agregar secretos en el almacén de claves mediante el [módulo de PowerShell de almacén de claves AzureRM](/powershell/module/azurerm.keyvault) disponibles desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), el [API de REST del almacén de claves de Azure](/rest/api/keyvault/), o la [Portal de azure](https://portal.azure.com/). Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe utilizar el *Manual* opción para crear los secretos de par nombre / valor para su uso con el proveedor de configuración.
    * Secretos simples se crean como pares nombre / valor. Nombres de secreto de almacén de claves Azure están limitados a caracteres alfanuméricos y guiones.
    * Usan valores jerárquicos (secciones de configuración) `--` (dos guiones) como separador en el ejemplo. Caracteres de dos puntos, que normalmente se utilizan para delimitar una sección de una subclave en [configuración de ASP.NET Core](xref:fundamentals/configuration/index), no están permitidos en los nombres de secreto. Por lo tanto, se usa dos guiones y se intercambian en dos puntos cuando se cargan los secretos en la configuración de la aplicación.
    * Cree dos *Manual* secretos con los siguientes pares de nombre-valor. El secreto del primer es un nombre simple y el valor y el secreto del segundo crea un valor secreto con una sección y la subclave en el nombre de secreto:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Registrar la aplicación de ejemplo con Azure Active Directory.
  * Autorizar la aplicación para tener acceso al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` cmdlet de PowerShell para autorizar la aplicación para tener acceso al almacén de claves, proporcionar `List` y `Get` acceso a los secretos con `-PermissionsToSecrets list,get`.
2. Actualización de la aplicación *appSettings.JSON que se* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo, que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre de secreto.
  * Valores no son jerárquicos: el valor de `SecretName` se obtiene con `config["SecretName"]`.
  * Valores jerárquicos (secciones): Use `:` notación (coma) o el `GetSection` método de extensión. Use cualquiera de estos métodos para obtener el valor de configuración:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

Cuando se ejecuta la aplicación, una página Web muestra los valores cargados secretos:

![Ventana del explorador que muestra valores secretos carga mediante el proveedor de configuración de almacén de claves de Azure](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Crear el almacén de claves con prefijo secretos y cargar los valores de configuración (clave-nombre-ejemplo de prefijo)
`AddAzureKeyVault`También proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, que le permite controlar cómo clave secretos del almacén se convierten en las claves de configuración. Por ejemplo, puede implementar la interfaz para cargar valores secretos basándose en un valor de prefijo que proporcione al iniciar la aplicación. Esto le permite, por ejemplo, para cargar los secretos en función de la versión de la aplicación.

> [!WARNING]
> No utilizar prefijos en secretos del almacén de claves colocar secretos para varias aplicaciones en el mismo almacén de claves o colocar secretos entorno (por ejemplo, *desarrollo* frente a *producción* secretos) en el mismo almacén. Se recomienda que diferentes aplicaciones y entornos de desarrollo o de producción usan almacenes claves independientes para aislar los entornos de aplicación para el nivel más alto de seguridad.

Con la segunda aplicación de ejemplo, crear un secreto en el almacén de claves para `5000-AppSecret` (períodos no están permitidos en los nombres de secreto de almacén de claves) que representa un secreto de la aplicación para la versión 5.0.0.0 de la aplicación. Para obtener otra versión, 5.1.0.0, crear un secreto para `5100-AppSecret`. Cada versión de la aplicación carga su propio valor secreto en su configuración como `AppSecret`, extracción desactivar la versión cuando se cargue el secreto. Implementación del ejemplo se muestra a continuación:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

El `Load` método se llama mediante un algoritmo de proveedor que recorre en iteración los secretos del almacén para buscar los que tienen el prefijo de la versión. Cuando se encuentra un prefijo de versión con `Load`, el algoritmo utiliza el `GetKey` método para devolver el nombre de configuración del nombre del secreto. Quita el prefijo de la versión del nombre del secreto y devuelve el resto del nombre del secreto para cargar en la configuración de la aplicación de pares nombre / valor.

Al implementar este enfoque:

1. Se cargan los secretos del almacén de claves.
2. El secreto de cadena para `5000-AppSecret` se encuentran coincidencias.
3. La versión `5000` (con el guión), se quitan el nombre de clave dejando `AppSecret` para cargar con el valor de secreto en configuración de la aplicación.

> [!NOTE]
> También puede proporcionar sus propios `KeyVaultClient` implementación `AddAzureKeyVault`. Proporciona a un cliente personalizado le permite compartir una única instancia del cliente entre el proveedor de configuración y otras partes de la aplicación.

1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con el almacén de claves de Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Agregar secretos en el almacén de claves mediante el [módulo de PowerShell de almacén de claves AzureRM](/powershell/module/azurerm.keyvault) disponibles desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), el [API de REST del almacén de claves de Azure](/rest/api/keyvault/), o la [Portal de azure](https://portal.azure.com/). Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe utilizar el *Manual* opción para crear los secretos de par nombre / valor para su uso con el proveedor de configuración.
    * Usan valores jerárquicos (secciones de configuración) `--` (dos guiones) como separador.
    * Cree dos *Manual* secretos con los siguientes pares de nombre y valor:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Registrar la aplicación de ejemplo con Azure Active Directory.
  * Autorizar la aplicación para tener acceso al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` cmdlet de PowerShell para autorizar la aplicación para tener acceso al almacén de claves, proporcionar `List` y `Get` acceso a los secretos con `-PermissionsToSecrets list,get`.
2. Actualización de la aplicación *appSettings.JSON que se* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo, que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre de secreto con prefijo. En este ejemplo, el prefijo es la versión de la aplicación, que proporciona a los `PrefixKeyVaultSecretManager` cuando agrega el proveedor de configuración de almacén de claves de Azure. El valor de `AppSecret` se obtiene con `config["AppSecret"]`. La página Web generada por la aplicación muestra el valor cargado:

   ![Ventana del explorador que muestra un valor secreto que se carga mediante el proveedor de configuración de almacén de claves de Azure versión de la aplicación una vez 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Cambiar la versión del ensamblado en el archivo de proyecto de aplicación `5.0.0.0` a `5.1.0.0` y vuelva a ejecutar la aplicación. En esta ocasión, el valor de secreto devuelto es `5.1.0.0_secret_value`. La página Web generada por la aplicación muestra el valor cargado:

   ![Ventana del explorador que muestra un valor secreto que se carga mediante el proveedor de configuración de almacén de claves de Azure versión de la aplicación una vez 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Controlar el acceso a la ClientSecret
Use la [herramienta Administrador de secreto](xref:security/app-secrets) para mantener la `ClientSecret` fuera de su árbol de código fuente del proyecto. Con el Administrador de secreto, para asocia los secretos de aplicación a un proyecto específico y compartirlos en varios proyectos.

Al desarrollar una aplicación de .NET Framework en un entorno que admite certificados, puede autenticarse en el almacén de claves de Azure con un certificado X.509. Clave privada del certificado X.509 es administrada por el sistema operativo. Para obtener más información, consulte [autenticar con un certificado en lugar de un secreto de cliente](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Use la `AddAzureKeyVault` sobrecarga que acepta un `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Volver a cargar secretos
Los secretos se almacenan en caché hasta que `IConfigurationRoot.Reload()` se llama. Caducado, deshabilitado, y por la aplicación hasta que no se respeten la secretos actualizados en el almacén de claves `Reload` se ejecuta.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Secretos deshabilitados y expirados
Secretos deshabilitados y expirados producen un `KeyVaultClientException`. Para evitar que la aplicación genere, reemplace la aplicación o actualizar el secreto deshabilitado o expirado.

## <a name="troubleshooting"></a>Solución de problemas
Cuando no se puede cargar la configuración mediante el proveedor de la aplicación, se escribe un mensaje de error en la [infraestructura del registro de ASP.NET](xref:fundamentals/logging/index). Las siguientes condiciones se evita que la configuración de carga:
* La aplicación no está configurada correctamente en Azure Active Directory.
* El almacén de claves no existe en el almacén de claves de Azure.
* La aplicación no está autorizada para tener acceso al almacén de claves.
* No incluye la directiva de acceso `Get` y `List` permisos.
* En el almacén de claves, los datos de configuración (par nombre / valor) se denominó incorrectamente, falta, deshabilitado o expirado.
* La aplicación tiene el nombre de almacén de claves incorrecto (`Vault`), Id. de aplicación de Azure AD (`ClientId`), o la clave de AD de Azure (`ClientSecret`).
* La clave de AD de Azure (`ClientSecret`) ha expirado.
* La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.

## <a name="additional-resources"></a>Recursos adicionales
* [Configuración](xref:fundamentals/configuration/index)
* [Microsoft Azure: Almacén de claves](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentación de almacén de claves](https://docs.microsoft.com/azure/key-vault/)
* [Cómo generar y transferir protegidas con HSM de claves para el almacén de claves de Azure](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Clase KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
