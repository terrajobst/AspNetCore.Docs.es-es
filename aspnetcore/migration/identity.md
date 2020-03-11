---
title: Migración de la autenticación y la identidad a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar la autenticación y la identidad de un proyecto de MVC de ASP.NET a un proyecto de MVC de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: f821930dbd36de18db31104cddf34c563009a506
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653015"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migración de la autenticación y la identidad a ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

En el artículo anterior, se [migró la configuración de un proyecto de ASP.NET MVC a ASP.net Core MVC](xref:migration/configuration). En este artículo, migraremos las características de registro, Inicio de sesión y administración de usuarios.

## <a name="configure-identity-and-membership"></a>Configurar la identidad y la pertenencia

En ASP.NET MVC, las características de autenticación e identidad se configuran mediante ASP.NET Identity en *Startup.auth.CS* y *IdentityConfig.CS*, que se encuentra en la carpeta *App_Start* . En ASP.NET Core MVC, estas características se configuran en *Startup.CS*.

Instale los paquetes de NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` y `Microsoft.AspNetCore.Authentication.Cookies`.

A continuación, Abra *Startup.CS* y actualice el método `Startup.ConfigureServices` para usar Entity Framework e Identity Services:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

En este punto, hay dos tipos a los que se hace referencia en el código anterior que todavía no se han migrado desde el proyecto ASP.NET MVC: `ApplicationDbContext` y `ApplicationUser`. Cree una nueva carpeta *Models* en el proyecto de ASP.net Core y agregue dos clases a ella que se correspondan con estos tipos. Encontrará las versiones de ASP.NET MVC de estas clases en */Models/IdentityModels.CS*, pero usaremos un archivo por clase en el proyecto migrado, ya que eso es más claro.

*ApplicationUser.CS*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.CS*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

El proyecto Web de inicio de MVC de ASP.NET Core no incluye gran personalización de usuarios o la `ApplicationDbContext`. Al migrar una aplicación real, también debe migrar todas las propiedades y métodos personalizados del usuario y las clases de `DbContext` de la aplicación, así como cualquier otra clase de modelo que use la aplicación. Por ejemplo, si el `DbContext` tiene un `DbSet<Album>`, debe migrar la clase `Album`.

Una vez realizados estos archivos, el archivo *Startup.CS* se puede compilar mediante la actualización de sus instrucciones `using`:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Nuestra aplicación ya está lista para admitir servicios de autenticación e identidad. Solo es necesario que estas características se expongan a los usuarios.

## <a name="migrate-registration-and-login-logic"></a>Migración de la lógica de inicio de sesión y registro

Con los servicios de identidad configurados para la aplicación y el acceso a los datos configurados con Entity Framework y SQL Server, estamos preparados para agregar compatibilidad con el registro y el inicio de sesión en la aplicación. Recuerde que [anteriormente en el proceso de migración](xref:migration/mvc#migrate-the-layout-file) hemos comentado una referencia a *_LoginPartial* en *_Layout. cshtml*. Ahora es el momento de volver a ese código, quitar su comentario y agregar los controladores y las vistas necesarios para admitir la funcionalidad de inicio de sesión.

Quite la marca de comentario de la línea de `@Html.Partial` en *_Layout. cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Ahora, agregue una nueva vista de Razor denominada *_LoginPartial* a la carpeta *views/Shared* :

Actualice *_LoginPartial. cshtml* con el código siguiente (reemplace todo su contenido):

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

En este punto, debería poder actualizar el sitio en el explorador.

## <a name="summary"></a>Resumen

ASP.NET Core introduce cambios en las características de ASP.NET Identity. En este artículo, ha aprendido a migrar las características de autenticación y administración de usuarios de ASP.NET Identity a ASP.NET Core.
