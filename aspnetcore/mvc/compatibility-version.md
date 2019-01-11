---
title: Versión de compatibilidad para ASP.NET Core MVC
author: rick-anderson
description: Descubra cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: mvc/compatibility-version
ms.openlocfilehash: 63243d99c7cb74a7e594cd309a808455c6611fc0
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577856"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="96986-103">Versión de compatibilidad para ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="96986-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="96986-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="96986-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="96986-105">El método <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permite a una aplicación participar o no en los cambios de comportamiento importantes incorporados en ASP.NET Core MVC 2.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="96986-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="96986-106">Estos cambios de comportamiento importantes suelen estar relacionados con cómo se comporta el subsistema de MVC y cómo el tiempo de ejecución llama al **código**.</span><span class="sxs-lookup"><span data-stu-id="96986-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="96986-107">Si la aplicación participa, obtendrá el comportamiento más reciente y a largo plazo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96986-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="96986-108">El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2:</span><span class="sxs-lookup"><span data-stu-id="96986-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="96986-109">Le recomendamos que pruebe la aplicación con la versión más reciente (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="96986-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="96986-110">Prevemos que la mayoría de las aplicaciones no tendrán cambios de comportamiento importantes usando la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="96986-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="96986-111">Las aplicaciones que llaman a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` están protegidas frente a los cambios de comportamiento importantes incorporados en ASP.NET Core 2.1 MVC y versiones 2.x posteriores.</span><span class="sxs-lookup"><span data-stu-id="96986-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="96986-112">Esta protección:</span><span class="sxs-lookup"><span data-stu-id="96986-112">This protection:</span></span>

* <span data-ttu-id="96986-113">No es aplicable a todos los cambios de 2.1 y versiones posteriores, sino que tiene como destino los cambios importantes de comportamiento en tiempo de ejecución de ASP.NET Core en el subsistema de MVC.</span><span class="sxs-lookup"><span data-stu-id="96986-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="96986-114">No se extiende a la siguiente versión principal.</span><span class="sxs-lookup"><span data-stu-id="96986-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="96986-115">La compatibilidad predeterminada de las aplicaciones ASP.NET Core 2.1 y versiones 2.x posteriores que **no** llaman a `SetCompatibilityVersion` es la compatibilidad 2.0.</span><span class="sxs-lookup"><span data-stu-id="96986-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="96986-116">Es decir, no llamar a `SetCompatibilityVersion` es igual que llamar a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="96986-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="96986-117">El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2, salvo en los siguientes comportamientos:</span><span class="sxs-lookup"><span data-stu-id="96986-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* [<span data-ttu-id="96986-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="96986-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="96986-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="96986-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="96986-120">En el caso de las aplicaciones que encuentran cambios de comportamiento importantes, si se usan los modificadores de compatibilidad adecuados:</span><span class="sxs-lookup"><span data-stu-id="96986-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="96986-121">Se podrá usar la versión más reciente y descartar cambios de comportamiento importantes específicos.</span><span class="sxs-lookup"><span data-stu-id="96986-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="96986-122">Se dispondrá de tiempo para actualizar la aplicación para que funcione con los cambios más recientes.</span><span class="sxs-lookup"><span data-stu-id="96986-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="96986-123">En los comentarios de código fuente de [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) encontrará bien explicado qué ha cambiado y por qué estos cambios son una mejora para la mayoría de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="96986-123">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="96986-124">Próximamente habrá una [versión ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="96986-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="96986-125">Los comportamientos anteriores admitidos por los modificadores de compatibilidad se quitarán en esta versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="96986-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="96986-126">Estamos convencidos de que estos son cambios positivos que beneficiarán a prácticamente todos los usuarios.</span><span class="sxs-lookup"><span data-stu-id="96986-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="96986-127">Al presentarlos ahora, la mayoría de las aplicaciones podrán empezar a sacar partido de ellos ya y los demás tendrán tiempo suficiente para actualizar sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="96986-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
