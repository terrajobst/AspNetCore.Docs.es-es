# <a name="gdpr-sample"></a>Ejemplo de RGPD

* En *appsettings.json*, establezca `CheckNotConsentNeeded` a `false` a requieren el consentimiento; en caso contrario, se establece en true o se omite. Probar la aplicación con `CheckNotConsentNeeded` establecido en `false` y establezca en `true`.
* Creación de cookies esenciales y no esenciales con cada variación de `CheckConsentNeeded` y consentimiento concedido.
* Registrar un usuario.
* Eliminar las cookies.
