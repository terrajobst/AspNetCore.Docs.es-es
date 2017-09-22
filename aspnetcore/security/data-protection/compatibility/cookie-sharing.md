---
title: Uso compartido de las cookies entre aplicaciones
author: rick-anderson
description: 
keywords: Uso compartido de Core,ASP.NET,cookies,Interop,cookie de ASP.NET
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: dbf52b0a990a3627b8eded22db033c45d51ba6ad
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="1b071-103">Uso compartido de las cookies entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="1b071-103">Sharing cookies between applications</span></span>

<span data-ttu-id="1b071-104">Sitios Web normalmente consisten en muchas aplicaciones web individuales, todas funcionan juntos armonía.</span><span class="sxs-lookup"><span data-stu-id="1b071-104">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="1b071-105">Si desea que un desarrollador de aplicaciones proporcionar una buena experiencia de inicio de sesión único, a menudo tendrán todas las aplicaciones web diferente dentro del sitio para compartir los vales de autenticación entre sí.</span><span class="sxs-lookup"><span data-stu-id="1b071-105">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="1b071-106">Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="1b071-106">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="1b071-107">Cookies de autenticación para compartir entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="1b071-107">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="1b071-108">Para compartir las cookies de autenticación entre dos aplicaciones diferentes que ASP.NET Core, configure cada aplicación que se debe compartir las cookies como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="1b071-108">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="1b071-109">En su configurar los métodos que utilizan el CookieAuthenticationOptions para configurar el servicio de protección de datos para las cookies y el AuthenticationScheme para que coincida con ASP.NET 4.X.</span><span class="sxs-lookup"><span data-stu-id="1b071-109">In your configure method use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.X.</span></span>

<span data-ttu-id="1b071-110">Si usas identidad:</span><span class="sxs-lookup"><span data-stu-id="1b071-110">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
   {
       options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
       var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
       options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
       options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
   });
   ```

<span data-ttu-id="1b071-111">Si usa cookies directamente:</span><span class="sxs-lookup"><span data-stu-id="1b071-111">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
   {
       DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
   });
   ```
   
<span data-ttu-id="1b071-112">El `DataProtectionProvider` requiere el `Microsoft.AspNetCore.DataProtection.Extensions` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="1b071-112">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="1b071-113">Cuando se utiliza de esta manera, DirectoryInfo debe apuntar a una ubicación de almacenamiento de claves que se reservan específicamente para las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1b071-113">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="1b071-114">El middleware de autenticación de la cookie utiliza la implementación proporcionada explícitamente de la DataProtectionProvider, que ahora está aislada del sistema de protección de datos utilizada por otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1b071-114">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="1b071-115">Se omite el nombre de la aplicación (intencionadamente por lo tanto, puesto que está intentando obtener varias aplicaciones compartan cargas).</span><span class="sxs-lookup"><span data-stu-id="1b071-115">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="1b071-116">Puede configurar el DataProtectionProvider de modo que las claves se cifran en reposo, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="1b071-116">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="1b071-117">Uso compartido de las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b071-117">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="1b071-118">Las aplicaciones de ASP.NET 4.x que usar el middleware de autenticación de cookie de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de la cookie de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b071-118">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1b071-119">Esto permite actualizar las aplicaciones individuales de un sitio grande por etapas mientras sigue proporcionando un inicio de sesión único sin problemas en la experiencia en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="1b071-119">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="1b071-120">Puede indicar si la aplicación existente usa middleware de autenticación de cookie de Katana por la existencia de una llamada a UseCookieAuthentication en Startup.Auth.cs del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1b071-120">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="1b071-121">Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y utilizan el middleware de autenticación de la cookie de Katana de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1b071-121">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="1b071-122">La aplicación de ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior, en caso contrario, los paquetes de NuGet necesarios no se instala.</span><span class="sxs-lookup"><span data-stu-id="1b071-122">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="1b071-123">Para compartir las cookies de autenticación entre las aplicaciones de ASP.NET 4.x y las aplicaciones de ASP.NET Core, configure la aplicación de ASP.NET Core tal y como se ha indicado anteriormente, a continuación, configure las aplicaciones de ASP.NET 4.x siguiendo los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="1b071-123">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="1b071-124">Instale el paquete Microsoft.Owin.Security.Interop en cada una de las aplicaciones de ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="1b071-124">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="1b071-125">En Startup.Auth.cs, busque la llamada a UseCookieAuthentication, que normalmente tendrá un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="1b071-125">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="1b071-126">Modifique la llamada a UseCookieAuthentication como se indica a continuación, cambiar el CookieName para que coincida con el nombre utilizado por el middleware de autenticación de la cookie de ASP.NET Core, proporcionar una instancia de un DataProtectionProvider que se ha inicializado a una ubicación de almacenamiento de claves, y establece CookieManager en interoperabilidad ChunkingCookieManager de modo que el formato fragmentado no es compatible.</span><span class="sxs-lookup"><span data-stu-id="1b071-126">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
       {
           AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
           CookieName = ".AspNetCore.Cookies",
           // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
           // CookiePath = "...", (if necessary)
           // ...
           TicketDataFormat = new AspNetTicketDataFormat(
               new DataProtectorShim(
                   DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                   .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                   "Cookies", "v2"))),
           CookieManager = new ChunkingCookieManager()
       });
       ```
    <span data-ttu-id="1b071-127">DirectoryInfo debe apuntar a la misma ubicación de almacenamiento que apunta la aplicación de ASP.NET Core para y debe configurarse con la misma configuración.</span><span class="sxs-lookup"><span data-stu-id="1b071-127">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="1b071-128">ASP.NET 4.x y aplicaciones de ASP.NET Core ahora están configurados para compartir las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1b071-128">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="1b071-129">Debe asegurarse de que el sistema de identidad para cada aplicación apunta a la misma base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="1b071-129">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="1b071-130">En caso contrario, el sistema de identidades producirá errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.</span><span class="sxs-lookup"><span data-stu-id="1b071-130">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
