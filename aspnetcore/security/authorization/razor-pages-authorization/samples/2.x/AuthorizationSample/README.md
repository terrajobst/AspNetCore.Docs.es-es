# <a name="aspnet-core-authorization-sample"></a>Ejemplo de autorización de ASP.NET Core

En este ejemplo se muestra el uso de la autorización de Razor Pages por convenciones. En este ejemplo se muestran las características descritas en el tema [Razor pages convenciones de autorización](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) .

La autorización del usuario en este ejemplo usa las características de autenticación de cookies descritas en el tema uso de la [autenticación de cookies sin ASP.net Core identidad](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) . Los conceptos y los ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core identidad. Para obtener información sobre el uso de ASP.NET Core identidad, vea [Introducción a la identidad en ASP.net Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Use la dirección de correo electrónico **maria.rodriguez@contoso.com** para autenticar al usuario con cualquier contraseña. El usuario se autentica en el método `AuthenticateUser` del archivo *pages/Account/login. cshtml. CS* . En un ejemplo real, el usuario se autenticaría en una base de datos.

## <a name="examples-in-this-sample"></a>Ejemplos incluidos

| Característica | Descripción |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página con la ruta de acceso especificada. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas de una carpeta con la ruta de acceso especificada. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página con la ruta de acceso especificada. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas de una carpeta con la ruta de acceso especificada. |
