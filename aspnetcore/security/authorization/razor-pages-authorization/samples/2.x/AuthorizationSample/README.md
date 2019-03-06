# <a name="aspnet-core-authorization-sample"></a>Ejemplo de autorización de ASP.NET Core

Este ejemplo ilustra el uso de la autorización de las páginas de Razor mediante convenciones. Este ejemplo muestra las características descritas en la [convenciones de autorización de las páginas de Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tema.

Autorización de usuario en este ejemplo usa características que se describen en la autenticación con cookies la [utilizar autenticación de cookies sin ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tema. Los conceptos y ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core Identity. Para obtener información sobre el uso de ASP.NET Core Identity, vea [Introducción a la identidad en ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Use la dirección de correo electrónico **maria.rodriguez@contoso.com** para autenticar al usuario con cualquier contraseña. El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo. En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.

## <a name="examples-in-this-sample"></a>Ejemplos que se incluyen

| Característica | Descripción |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página con la ruta de acceso especificada. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Agrega un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta con la ruta de acceso especificada. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página con la ruta de acceso especificada. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Agrega un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta con la ruta de acceso especificada. |
