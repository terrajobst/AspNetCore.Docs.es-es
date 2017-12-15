# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Aplicación de ejemplo de proveedor de configuración de almacén de claves del prefijo (ASP.NET Core 2.x)

Este ejemplo ilustra el uso del proveedor de configuración de almacén de claves de Azure para ASP.NET Core 2.x con prefijos de nombre de clave. Para obtener el ejemplo de 1.x ASP.NET Core, vea [aplicación de ejemplo de proveedor de configuración de almacén de clave de prefijo (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x).

Para obtener más información sobre cómo funciona el ejemplo, vea el [proveedor de configuración de almacén de claves de Azure](xref:security/key-vault-configuration) tema.

## <a name="using-the-sample"></a>Utilizar el ejemplo
1. Crear un almacén de claves y configurar Azure Active Directory (Azure AD) para la aplicación siguiendo las instrucciones de [empezar a trabajar con el almacén de claves de Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Agregue los secretos en el almacén de claves con el módulo de PowerShell de Azure, la API de administración de Azure o el Portal de Azure. Los secretos se crean como *Manual* o *certificado* secretos. *Certificado* secretos están certificados para su uso por aplicaciones y servicios, pero no son compatibles con el proveedor de configuración. Debe utilizar el *Manual* opción para crear los secretos de par nombre / valor para su uso con el proveedor de configuración.
    * Usan valores jerárquicos (secciones de configuración) `--` (dos guiones) como separador.
    * Para la aplicación de ejemplo, cree dos *Manual* secretos con los siguientes pares de nombre y valor:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Registrar la aplicación de ejemplo con Azure Active Directory.
  * Autorizar la aplicación para tener acceso al almacén de claves. Cuando se usa el `Set-AzureRmKeyVaultAccessPolicy` cmdlet de PowerShell para autorizar la aplicación para tener acceso al almacén de claves, proporcionar `List` y `Get` acceso a los secretos con `-PermissionsToSecrets list,get`.
2. Actualización de la aplicación *appSettings.JSON que se* archivo con los valores de `Vault`, `ClientId`, y `ClientSecret`.
3. Ejecute la aplicación de ejemplo, que obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre de secreto con prefijo. En este ejemplo, el prefijo es la versión de la aplicación, que proporciona a los `PrefixKeyVaultSecretManager` cuando agrega el proveedor de configuración de almacén de claves de Azure. El valor de `AppSecret` se obtiene con `config["AppSecret"]`.
4. Cambiar la versión del ensamblado en el archivo de proyecto de aplicación `5.0.0.0` a `5.1.0.0` y vuelva a ejecutar la aplicación. En esta ocasión, el valor de secreto devuelto es `5.1.0.0_secret_value`.
