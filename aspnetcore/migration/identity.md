---
title: "Migrar de autenticación e identidad"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: 6972329e3d434e1b67131843118f2f18229807b9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity"></a>Migrar de autenticación e identidad

<a name="migration-identity"></a>

Por [Steve Smith](https://ardalis.com/)

En la versión anterior del artículo se [migrar configuración desde un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo](configuration.md). En este artículo, nos migrar las características de administración de registro, el inicio de sesión y el usuario.

## <a name="configure-identity-and-membership"></a>Configure la identidad y pertenencia

En ASP.NET MVC, características de autenticación e identidad se configuran mediante ASP.NET Identity en Startup.Auth.cs y IdentityConfig.cs, que se encuentra en la carpeta App_Start. En el núcleo de ASP.NET MVC, estas características se configuran en *Startup.cs*.

Instalar el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` y `Microsoft.AspNetCore.Authentication.Cookies` paquetes de NuGet.

A continuación, abra Startup.cs y actualizar la `ConfigureServices()` método se debe utilizar servicios de identidad y de Entity Framework:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

En este momento, hay dos tipos que se hace referencia en el código anterior que hemos aún no hemos migrado desde el proyecto de MVC de ASP.NET: `ApplicationDbContext` y `ApplicationUser`. Crear un nuevo *modelos* carpeta en el núcleo de ASP.NET del proyecto y agregarle dos clases correspondientes a estos tipos. Encontrará ASP.NET MVC versiones de estas clases en `/Models/IdentityModels.cs`, pero vamos a utilizar un archivo por cada clase en el proyecto migrado ya que es más nítido.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

No incluye el proyecto Web de inicio de MVC de ASP.NET Core mucho personalización de los usuarios o lo ApplicationDbContext. Al migrar una aplicación real, también necesitará migrar todas las propiedades personalizadas y los métodos de las clases de DbContext y usuario de la aplicación, así como cualquier otra clase de modelo que la aplicación utiliza (por ejemplo, si su DbContext tiene un DbSet<Album>, evidentemente debe migrar la clase de álbum).

Con estos archivos en su lugar, puede convertirse en el archivo Startup.cs para compilar mediante la actualización de su uso de instrucciones:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Nuestra aplicación ahora está preparado para admitir la autenticación y los servicios de identidad: basta con que tienen estas características expuestas a los usuarios.

## <a name="migrate-registration-and-login-logic"></a>Migrar el registro y la lógica de inicio de sesión

Con servicios de identidad configurados para la aplicación y el acceso a datos configurado mediante Entity Framework y SQL Server, ahora estamos listos agregar compatibilidad con inicio de sesión y de registro a la aplicación. Recuerde que [anteriormente en el proceso de migración](mvc.md#migrate-layout-file) se comentada una referencia a _LoginPartial en _Layout.cshtml. Ahora es el momento de volver a ese código, quitar los comentarios y agregar en las vistas para admitir la funcionalidad de inicio de sesión y los controladores necesarios.

Actualizar _Layout.cshtml; Quite el @Html.Partial línea:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Ahora, agregue una nueva página de vista de MVC llama _LoginPartial a la carpeta vistas/compartida:

Actualizar _LoginPartial.cshtml con el código siguiente (reemplazar todo su contenido):

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

En este momento, podrá actualizar el sitio en el explorador.

## <a name="summary"></a>Resumen

ASP.NET Core incluye cambios en las características de ASP.NET Identity. En este artículo, ha visto cómo migrar las características de administración de autenticación y usuario de una identidad de ASP.NET a ASP.NET Core.
