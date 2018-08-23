---
title: Migrar de autenticación e identidad a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar de autenticación e identidad de un proyecto ASP.NET MVC a un proyecto de ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828876"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="b7b52-103">Migrar de autenticación e identidad a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7b52-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="b7b52-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b7b52-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b7b52-105">En el artículo anterior, hemos [migrar configuración de un proyecto de ASP.NET MVC a ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="b7b52-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="b7b52-106">En este artículo, nos migrar las características de administración de registro, inicio de sesión y usuario.</span><span class="sxs-lookup"><span data-stu-id="b7b52-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="b7b52-107">Configurar la identidad y pertenencia</span><span class="sxs-lookup"><span data-stu-id="b7b52-107">Configure Identity and Membership</span></span>

<span data-ttu-id="b7b52-108">En ASP.NET MVC, las características de autenticación e identidad se configuran mediante ASP.NET Identity en *Startup.Auth.cs* y *IdentityConfig.cs*, que se encuentra en la *App_Start* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b7b52-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="b7b52-109">En ASP.NET Core MVC, estas características se configuran en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b7b52-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="b7b52-110">Instalar el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` y `Microsoft.AspNetCore.Authentication.Cookies` paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b7b52-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="b7b52-111">A continuación, abra *Startup.cs* y actualizar la `Startup.ConfigureServices` método para usar los servicios de identidad y Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="b7b52-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="b7b52-112">En este momento, hay dos tipos que se hace referencia en el código anterior que hemos aún no hemos migrado desde el proyecto de ASP.NET MVC: `ApplicationDbContext` y `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b7b52-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="b7b52-113">Cree un nuevo *modelos* carpeta en el núcleo de ASP.NET del proyecto y agregarle dos clases correspondientes a estos tipos.</span><span class="sxs-lookup"><span data-stu-id="b7b52-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="b7b52-114">Encontrará el ASP.NET MVC versiones de estas clases en */Models/IdentityModels.cs*, pero vamos a utilizar un archivo por cada clase en el proyecto migrado, ya que es más clara.</span><span class="sxs-lookup"><span data-stu-id="b7b52-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="b7b52-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="b7b52-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="b7b52-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="b7b52-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
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

<span data-ttu-id="b7b52-117">El proyecto Web de ASP.NET Core MVC Starter no incluye mucha personalización de los usuarios, o la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b7b52-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="b7b52-118">Al migrar una aplicación real, también deberá migrar todas las propiedades personalizadas y los métodos de usuario de la aplicación y `DbContext` clases, así como otras clases de modelo usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b7b52-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="b7b52-119">Por ejemplo, si su `DbContext` tiene un `DbSet<Album>`, tiene que migrar la `Album` clase.</span><span class="sxs-lookup"><span data-stu-id="b7b52-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="b7b52-120">Con estos archivos en su lugar, el *Startup.cs* archivo se puede realizar para compilar mediante la actualización de su `using` instrucciones:</span><span class="sxs-lookup"><span data-stu-id="b7b52-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="b7b52-121">Nuestra aplicación ahora está lista para admitir la autenticación y los servicios de identidad.</span><span class="sxs-lookup"><span data-stu-id="b7b52-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="b7b52-122">Basta con que tenga estas características que se exponen a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="b7b52-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="b7b52-123">Migrar la lógica de inicio de sesión y registro</span><span class="sxs-lookup"><span data-stu-id="b7b52-123">Migrate registration and login logic</span></span>

<span data-ttu-id="b7b52-124">Con la configurada para la aplicación de servicios de identidad y acceso a datos configurado mediante Entity Framework y SQL Server, ya estamos listos para agregar compatibilidad con inicio de sesión y registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b7b52-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="b7b52-125">Recuerde que [anteriormente en el proceso de migración](xref:migration/mvc#migrate-the-layout-file) nos comentadas una referencia a *_LoginPartial* en *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b7b52-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="b7b52-126">Ahora es momento de volver a ese código, quite los comentarios y agregue en las vistas para admitir la funcionalidad de inicio de sesión y los controladores necesarios.</span><span class="sxs-lookup"><span data-stu-id="b7b52-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="b7b52-127">Quite el `@Html.Partial` línea *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b7b52-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="b7b52-128">Ahora, agregue una nueva vista de Razor denominada *_LoginPartial* a la *Views/Shared* carpeta:</span><span class="sxs-lookup"><span data-stu-id="b7b52-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="b7b52-129">Actualización *_LoginPartial.cshtml* con el código siguiente (reemplace todo su contenido):</span><span class="sxs-lookup"><span data-stu-id="b7b52-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="b7b52-130">En este momento, podrá actualizar el sitio en el explorador.</span><span class="sxs-lookup"><span data-stu-id="b7b52-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="b7b52-131">Resumen</span><span class="sxs-lookup"><span data-stu-id="b7b52-131">Summary</span></span>

<span data-ttu-id="b7b52-132">ASP.NET Core presenta cambios en las características de ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="b7b52-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="b7b52-133">En este artículo, ha visto cómo migrar las características de administración de usuario y autenticación de ASP.NET Identity a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7b52-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
