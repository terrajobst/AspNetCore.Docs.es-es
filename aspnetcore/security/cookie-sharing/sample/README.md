# <a name="cookie-sharing-sample-app"></a>Aplicación de ejemplo de uso compartido de cookies

En el ejemplo muestra el uso compartido entre tres aplicaciones que usan la autenticación con cookies de cookie:

| Proyecto                             | Descripción |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplicación ASP.NET Core Razor Pages sin usar ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Aplicación MVC de ASP.NET Core con ASP.NET Core Identity |
| CookieAuthWithIdentity.NETFramework | Aplicación MVC de ASP.NET Framework con ASP.NET Identity |

Instrucciones:

1. Ejecute la aplicación CookieAuth.Core. Registrar un usuario. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. En la misma sesión del explorador, ejecute la aplicación CookieAuthWithIdentity.Core. Registrar el mismo usuario que se utiliza en la aplicación principal. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. En la misma sesión del explorador, ejecute la aplicación CookieAuthWithIdentity.NETFramework. Registrar el mismo usuario que se usan con las otras aplicaciones. La aplicación autentica al usuario cuando el usuario está registrado. Cierre la sesión del usuario.
1. El usuario inicie sesión en cualquiera de las tres aplicaciones. La cookie de autenticación se comparte entre las aplicaciones. Tenga en cuenta que el usuario ha iniciado automáticamente en las dos aplicaciones.
1. Inicie la sesión del usuario de cualquiera de las aplicaciones. Tenga en cuenta que el usuario ha iniciado automáticamente fuera de las dos aplicaciones.

Este ejemplo muestra las características descritas en la [compartir cookies entre aplicaciones](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tema.
