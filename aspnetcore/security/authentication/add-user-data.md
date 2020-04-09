---
title: Agregue, descargue y elimine datos de usuario a Identity en un proyecto de ASP.NET Core
author: rick-anderson
description: Aprenda a agregar datos de usuario personalizados a Identity en un proyecto de ASP.NET Core. Eliminar datos por RGPD.
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501226"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Agregue, descargue y elimine datos de usuario personalizados a Identity en un proyecto de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este artículo se muestra cómo:

* Agregue datos de usuario personalizados a una aplicación web de ASP.NET Core.
* Marque el modelo de <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> datos de usuario personalizado con el atributo para que esté disponible automáticamente para su descarga y eliminación. Hacer que los datos puedan descargarse y eliminarse ayuda a cumplir los requisitos [del RGPD.](xref:security/gdpr)

El ejemplo de proyecto se crea a partir de una aplicación web razor Pages, pero las instrucciones son similares para una aplicación web de ASP.NET Core MVC.

[Ver o descargar código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ( cómo[descargar](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerrequisitos

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de ejemplo de [descarga.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)
* Seleccione **ASP.NET aplicación** > web principal **ACEPTAR**
* Seleccione **ASP.NET Core 3.0** en el menú desplegable
* Seleccione **Aplicación** > web **OK**
* Compile y ejecute el proyecto.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**. Asigne al proyecto el nombre **WebApp1** si desea que coincida con el espacio de nombres del código de ejemplo de [descarga.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)
* Seleccione **ASP.NET aplicación** > web principal **ACEPTAR**
* Seleccione **ASP.NET Core 2.2** en el menú desplegable
* Seleccione **Aplicación** > web **OK**
* Compile y ejecute el proyecto.

::: moniker-end


# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Ejecute el scaffolder Identity

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* En **el Explorador**de soluciones , haga clic con el botón derecho en el proyecto > **Agregar** > **nuevo elemento con scaffolding**.
* En el panel izquierdo del cuadro de diálogo **Agregar scaffold,** seleccione**Agregar** **identidad** > .
* En el cuadro de diálogo **Agregar identidad,** las siguientes opciones:
  * Seleccione el archivo de diseño existente *./Pages/Shared/_Layout.cshtml*
  * Seleccione los siguientes archivos que desea anular:
    * **Cuenta/Registro**
    * **Cuenta/Administrar/Índice**
  * Seleccione **+** el botón para crear una nueva clase de **contexto de**datos. Acepte el tipo (**WebApp1.Models.WebApp1Context** si el proyecto se denomina **WebApp1**).
  * Seleccione **+** el botón para crear una nueva **clase User**. Acepte el tipo (**WebApp1User** si el proyecto se denomina **WebApp1**) > **Agregar**.
* Seleccione **Agregar**.

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si no ha instalado previamente el scaffolder ASP.NET Core, instálelo ahora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj). Ejecute el siguiente comando en el directorio del proyecto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Ejecute el siguiente comando para enumerar las opciones de Scaffolder identity:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

En la carpeta del proyecto, ejecute el scaffolder Identity:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Siga las instrucciones de [Migraciones, UseAuthentication y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:

* Cree una migración y actualice la base de datos.
* Agregue `UseAuthentication` a `Startup.Configure`.
* Agregar `<partial name="_LoginPartial" />` al archivo de diseño.
* Pruebe la aplicación:
  * Registrar un usuario
  * Seleccione el nuevo nombre de usuario (junto al vínculo **Cerrar sesión).** Es posible que deba expandir la ventana o seleccionar el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.
  * Seleccione la pestaña **Datos personales.**
  * Seleccione el botón **Descargar** y examine el archivo *PersonalData.json.*
  * Pruebe el botón **Eliminar,** que elimina el usuario que ha iniciado sesión.

## <a name="add-custom-user-data-to-the-identity-db"></a>Agregue datos de usuario personalizados a la base de datos de identidades

Actualice `IdentityUser` la clase derivada con propiedades personalizadas. Si ha denominado el proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*. Actualice el archivo con el siguiente código:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Las propiedades con el atributo [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) son:

* Se elimina cuando la página de Razor *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* llama `UserManager.Delete`a la página .
* Incluido en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.

### <a name="update-the-accountmanageindexcshtml-page"></a>Actualizar la página Account/Manage/Index.cshtml

Actualice `InputModel` el en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con el siguiente código resaltado:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Actualice *areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Actualice *areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Actualizar la página Account/Register.cshtml

Actualice `InputModel` el en *Areas/Identity/Pages/Account/Register.cshtml.cs* con el siguiente código resaltado:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Actualice *areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Actualice *areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Compile el proyecto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Agregar una migración para los datos de usuario personalizados

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

En la consola del **Administrador de paquetes**de Visual Studio:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Probar crear, ver, descargar, eliminar datos de usuario personalizados

Pruebe la aplicación:

* Registre un nuevo usuario.
* Vea los datos de `/Identity/Account/Manage` usuario personalizados en la página.
* Descargue y vea los datos `/Identity/Account/Manage/PersonalData` personales de los usuarios desde la página.

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a>Agregar notificaciones a Identity mediante IUserClaimsPrincipalFactory<ApplicationUser>

Se pueden agregar notificaciones adicionales a `IUserClaimsPrincipalFactory<T>` ASP.NET identidad principal mediante la interfaz. Esta clase se puede agregar `Startup.ConfigureServices` a la aplicación en el método. Agregue la implementación personalizada de la clase de la siguiente manera:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

El código de `ApplicationUser` demostración utiliza la clase. Esta clase agrega `IsAdmin` una propiedad que se utiliza para agregar la notificación adicional.

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

`AdditionalUserClaimsPrincipalFactory` implementa la interfaz `UserClaimsPrincipalFactory`. Se agrega una nueva `ClaimsPrincipal`notificación de rol al archivo .

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

La notificación adicional se puede usar en la aplicación. En una página `IAuthorizationService` de Razor, la instancia se puede usar para tener acceso al valor de notificación.

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
