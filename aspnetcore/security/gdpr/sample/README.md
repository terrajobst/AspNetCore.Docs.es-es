# <a name="gdpr-sample"></a>Ejemplo de RGPD

* En *appSettings. JSON*, establezca `CheckNotConsentNeeded` en `false` para que requiera el consentimiento. de lo contrario, se establece en true u omitir. Pruebe la aplicación con `CheckNotConsentNeeded` establecida en `false` y establezca en `true`.
* Cree cookies esenciales y no esenciales con cada variación de `CheckConsentNeeded` y consentimiento concedidos.
* Registrar un usuario.
* Eliminar cookies.
