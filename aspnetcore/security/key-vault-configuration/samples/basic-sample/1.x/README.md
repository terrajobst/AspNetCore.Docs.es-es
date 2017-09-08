# <a name="key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Aplicación de ejemplo de proveedor de configuración de almacén de claves (ASP.NET Core 1.x)

Este ejemplo ilustra el uso del proveedor de configuración de almacén de claves de Azure para ASP.NET Core 1.x. Para obtener el ejemplo de 2.x ASP.NET Core, vea [aplicación de ejemplo de proveedor de configuración de almacén de claves (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x).

> [!NOTE]
> El proveedor de configuración no está disponible para ASP.NET Core 1.0. Si va a implementar el proveedor de configuración y la aplicación es una aplicación de ASP.NET Core 1.0, actualizar la aplicación a la primera 1.1 o posterior.

Para obtener más información sobre cómo funciona el ejemplo, vea el [proveedor de configuración de almacén de claves de Azure](xref:security/key-vault-configuration) tema.

## <a name="using-the-sample"></a>Utilizar el ejemplo
1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con el almacén de claves de Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Agregar secretos en el almacén de claves mediante el [módulo de PowerShell de almacén de claves AzureRM](/powershell/module/azurerm.keyvault) disponibles desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), el [API de REST del almacén de claves de Azure](/rest/api/keyvault/), o la [Portal de azure](https://portal.azure.com/). Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe utilizar el *Manual* opción para crear los secretos de par nombre / valor para su uso con el proveedor de configuración.
    * Secretos simples se crean como pares nombre / valor. Nombres de secreto de almacén de claves Azure están limitados a caracteres alfanuméricos y guiones.
    * Usan valores jerárquicos (secciones de configuración) `--` (dos guiones) como separador en el ejemplo. Caracteres de dos puntos, que normalmente se utilizan para delimitar una sección de una subclave en [configuración de ASP.NET Core](xref:fundamentals/configuration), no están permitidos en los nombres de secreto. Por lo tanto, se usa dos guiones y se intercambian en dos puntos cuando se cargan los secretos en la configuración de la aplicación.
    * Cree dos *Manual* secretos con los siguientes pares de nombre-valor. El secreto del primer es un nombre simple y el valor y el secreto del segundo crea un valor secreto con una sección y la subclave en el nombre de secreto:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Registrar la aplicación de ejemplo con Azure Active Directory.
  * Autorizar la aplicación para tener acceso al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` cmdlet de PowerShell para autorizar la aplicación para tener acceso al almacén de claves, proporcionar `List` y `Get` acceso a los secretos con `-PermissionsToKeys list,get`.
2. Actualización de la aplicación *appSettings.JSON que se* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo, que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre de secreto.
  * Valores no son jerárquicos: el valor de `SecretName` se obtiene con `config["SecretName"]`.
  * Valores jerárquicos (secciones): Use `:` notación (coma) o el `GetSection` método de extensión. Use cualquiera de estos métodos para obtener el valor de configuración:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`
