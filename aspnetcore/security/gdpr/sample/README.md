# <a name="gdpr-sample"></a>Ejemplo GDPR

* En *appSettings.JSON que se*, establezca `CheckNotConsentNeeded` a `false` para requerir el consentimiento; de lo contrario se establece en true o se omite. Probar la aplicación con `CheckNotConsentNeeded` establecido en `false` y establezca en `true`.
* Crear las cookies o esenciales y no esenciales con cada variación de `CheckConsentNeeded` y consentimiento concedido.
* Registrar un usuario.
* Eliminar las cookies.
