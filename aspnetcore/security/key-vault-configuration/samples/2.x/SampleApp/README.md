# <a name="key-vault-configuration-provider-sample-app"></a>Aplicación de ejemplo del proveedor de configuración de Key Vault

En este ejemplo se muestra el uso del proveedor de configuración de Azure Key Vault.

El ejemplo se ejecuta en uno de los dos modos determinados por la instrucción `#define` en la parte superior del archivo *Program.CS* . Para obtener instrucciones, vea [directivas de preprocesador en código de ejemplo](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; muestra el uso de un identificador de cliente de Azure Key Vault y un certificado X. 509 para tener acceso a los secretos almacenados en Azure Key Vault. Esta versión del ejemplo se puede ejecutar desde cualquier ubicación, implementada en Azure App Service o cualquier host capaz de servir a una aplicación ASP.NET Core.
* en `Managed` &ndash; se muestra cómo usar [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) de Azure para autenticar la aplicación en Azure Key Vault con Azure ad autenticación sin credenciales en el código o la configuración de la aplicación. No es necesario un identificador de cliente de Azure AD y un secreto para que la aplicación se autentique con Azure Key Vault. Este ejemplo debe implementarse en Azure App Service para explorar la identidad administrada scearnio.

Para obtener más información, vea [Azure Key Vault proveedor de configuración](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
