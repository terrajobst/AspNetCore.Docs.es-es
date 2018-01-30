---
title: "Migrar de autenticación e identidad"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="04c43-102">Migrar de autenticación e identidad</span><span class="sxs-lookup"><span data-stu-id="04c43-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="04c43-103">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="04c43-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="04c43-104">En la versión anterior del artículo se [migrar configuración desde un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="04c43-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="04c43-105">En este artículo, nos migrar las características de administración de registro, el inicio de sesión y el usuario.</span><span class="sxs-lookup"><span data-stu-id="04c43-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="04c43-106">Configure la identidad y pertenencia</span><span class="sxs-lookup"><span data-stu-id="04c43-106">Configure Identity and Membership</span></span>

<span data-ttu-id="04c43-107">En ASP.NET MVC, características de autenticación e identidad se configuran mediante ASP.NET Identity en Startup.Auth.cs y IdentityConfig.cs, que se encuentra en la carpeta App_Start.</span><span class="sxs-lookup"><span data-stu-id="04c43-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="04c43-108">En el núcleo de ASP.NET MVC, estas características se configuran en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="04c43-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="04c43-109">Instalar el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` y `Microsoft.AspNetCore.Authentication.Cookies` paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="04c43-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="04c43-110">A continuación, abra Startup.cs y actualizar la `ConfigureServices()` método se debe utilizar servicios de identidad y de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="04c43-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="04c43-111">En este momento, hay dos tipos que se hace referencia en el código anterior que hemos aún no hemos migrado desde el proyecto de MVC de ASP.NET: `ApplicationDbContext` y `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="04c43-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="04c43-112">Crear un nuevo *modelos* carpeta en el núcleo de ASP.NET del proyecto y agregarle dos clases correspondientes a estos tipos.</span><span class="sxs-lookup"><span data-stu-id="04c43-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="04c43-113">Encontrará ASP.NET MVC versiones de estas clases en `/Models/IdentityModels.cs`, pero vamos a utilizar un archivo por cada clase en el proyecto migrado ya que es más nítido.</span><span class="sxs-lookup"><span data-stu-id="04c43-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="04c43-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="04c43-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="04c43-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="04c43-115">ApplicationDbContext.cs:</span></span>

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

<span data-ttu-id="04c43-116">No incluye el proyecto Web de inicio de MVC de ASP.NET Core mucho personalización de los usuarios o lo ApplicationDbContext.</span><span class="sxs-lookup"><span data-stu-id="04c43-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="04c43-117">Al migrar una aplicación real, también necesitará migrar todas las propiedades personalizadas y los métodos de las clases de DbContext y usuario de la aplicación, así como cualquier otra clase de modelo que la aplicación utiliza (por ejemplo, si su DbContext tiene un DbSet<Album>, evidentemente debe migrar la clase de álbum).</span><span class="sxs-lookup"><span data-stu-id="04c43-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="04c43-118">Con estos archivos en su lugar, puede convertirse en el archivo Startup.cs para compilar mediante la actualización de su uso de instrucciones:</span><span class="sxs-lookup"><span data-stu-id="04c43-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="04c43-119">Nuestra aplicación ahora está preparado para admitir la autenticación y los servicios de identidad: basta con que tienen estas características expuestas a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="04c43-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="04c43-120">Migrar el registro y la lógica de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="04c43-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="04c43-121">Con servicios de identidad configurados para la aplicación y el acceso a datos configurado mediante Entity Framework y SQL Server, ahora estamos listos agregar compatibilidad con inicio de sesión y de registro a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04c43-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="04c43-122">Recuerde que [anteriormente en el proceso de migración](mvc.md#migrate-layout-file) se comentada una referencia a _LoginPartial en _Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="04c43-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="04c43-123">Ahora es el momento de volver a ese código, quitar los comentarios y agregar en las vistas para admitir la funcionalidad de inicio de sesión y los controladores necesarios.</span><span class="sxs-lookup"><span data-stu-id="04c43-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="04c43-124">Actualizar _Layout.cshtml; Quite el @Html.Partial línea:</span><span class="sxs-lookup"><span data-stu-id="04c43-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="04c43-125">Ahora, agregue una nueva página de vista de MVC llama _LoginPartial a la carpeta vistas/compartida:</span><span class="sxs-lookup"><span data-stu-id="04c43-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="04c43-126">Actualizar _LoginPartial.cshtml con el código siguiente (reemplazar todo su contenido):</span><span class="sxs-lookup"><span data-stu-id="04c43-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="04c43-127">En este momento, podrá actualizar el sitio en el explorador.</span><span class="sxs-lookup"><span data-stu-id="04c43-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="04c43-128">Resumen</span><span class="sxs-lookup"><span data-stu-id="04c43-128">Summary</span></span>

<span data-ttu-id="04c43-129">ASP.NET Core incluye cambios en las características de ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="04c43-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="04c43-130">En este artículo, ha visto cómo migrar las características de administración de autenticación y usuario de una identidad de ASP.NET a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04c43-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
