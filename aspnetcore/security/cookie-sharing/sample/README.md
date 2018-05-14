# <a name="cookie-sharing-sample-app"></a>Aplicación de ejemplo de uso compartido de cookie

El ejemplo ilustra la cookie compartir entre tres aplicaciones que usan autenticación con cookies:

| Proyecto                             | Descripción |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplicación ASP.NET Core 2.0 Razor páginas sin utilizar ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Aplicación MVC de ASP.NET Core 2.0 con la identidad de ASP.NET Core |
| CookieAuthWithIdentity.NETFramework | Aplicación MVC de ASP.NET Framework 4.6.1 con la identidad de ASP.NET |

Instrucciones:

1. Ejecutar la aplicación CookieAuth.Core. Registrar un usuario. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. En la misma sesión del explorador, ejecute la aplicación CookieAuthWithIdentity.Core. Registrar el mismo usuario que se utiliza en la aplicación principal. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. En la misma sesión del explorador, ejecute la aplicación CookieAuthWithIdentity.NETFramework. Registrar el mismo usuario que se usan con las otras aplicaciones. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. Inicie sesión en el usuario a cualquiera de las tres aplicaciones. La cookie de autenticación se comparte entre las aplicaciones. Tenga en cuenta que el usuario ha iniciado automáticamente en las otras dos aplicaciones.
1. Cierre la sesión del usuario desde cualquiera de las aplicaciones. Tenga en cuenta que el usuario ha iniciado automáticamente fuera de las otras dos aplicaciones.

Este ejemplo muestra las características descritas en la [compartir cookies entre aplicaciones](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tema.
