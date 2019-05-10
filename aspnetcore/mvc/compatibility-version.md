---
title: Versión de compatibilidad para ASP.NET Core MVC
author: rick-anderson
description: Descubra cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085884"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="354f0-103">Versión de compatibilidad para ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="354f0-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="354f0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="354f0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="354f0-105">El método <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> permite a una aplicación participar o no en los cambios de comportamiento importantes incorporados en ASP.NET Core MVC 2.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="354f0-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="354f0-106">Estos cambios de comportamiento importantes suelen estar relacionados con cómo se comporta el subsistema de MVC y cómo el tiempo de ejecución llama al **código**.</span><span class="sxs-lookup"><span data-stu-id="354f0-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="354f0-107">Si la aplicación participa, obtendrá el comportamiento más reciente y a largo plazo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="354f0-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="354f0-108">El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2:</span><span class="sxs-lookup"><span data-stu-id="354f0-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="354f0-109">Le recomendamos que pruebe la aplicación con la versión más reciente (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="354f0-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="354f0-110">Prevemos que la mayoría de las aplicaciones no tendrán cambios de comportamiento importantes usando la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="354f0-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="354f0-111">Las aplicaciones que llaman a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` están protegidas frente a los cambios de comportamiento importantes incorporados en ASP.NET Core 2.1 MVC y versiones 2.x posteriores.</span><span class="sxs-lookup"><span data-stu-id="354f0-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="354f0-112">Esta protección:</span><span class="sxs-lookup"><span data-stu-id="354f0-112">This protection:</span></span>

* <span data-ttu-id="354f0-113">No es aplicable a todos los cambios de 2.1 y versiones posteriores, sino que tiene como destino los cambios importantes de comportamiento en tiempo de ejecución de ASP.NET Core en el subsistema de MVC.</span><span class="sxs-lookup"><span data-stu-id="354f0-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="354f0-114">No se extiende a la siguiente versión principal.</span><span class="sxs-lookup"><span data-stu-id="354f0-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="354f0-115">La compatibilidad predeterminada de las aplicaciones ASP.NET Core 2.1 y versiones 2.x posteriores que **no** llaman a `SetCompatibilityVersion` es la compatibilidad 2.0.</span><span class="sxs-lookup"><span data-stu-id="354f0-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="354f0-116">Es decir, no llamar a `SetCompatibilityVersion` es igual que llamar a `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="354f0-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="354f0-117">El siguiente código establece el modo de compatibilidad en ASP.NET Core 2.2, salvo en los siguientes comportamientos:</span><span class="sxs-lookup"><span data-stu-id="354f0-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="354f0-118">En el caso de las aplicaciones que encuentran cambios de comportamiento importantes, si se usan los modificadores de compatibilidad adecuados:</span><span class="sxs-lookup"><span data-stu-id="354f0-118">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="354f0-119">Se podrá usar la versión más reciente y descartar cambios de comportamiento importantes específicos.</span><span class="sxs-lookup"><span data-stu-id="354f0-119">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="354f0-120">Se dispondrá de tiempo para actualizar la aplicación para que funcione con los cambios más recientes.</span><span class="sxs-lookup"><span data-stu-id="354f0-120">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="354f0-121">En la documentación de <xref:Microsoft.AspNetCore.Mvc.MvcOptions> se incluye una completa explicación de los cambios y por qué son una mejora para la mayoría de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="354f0-121">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions> documentation has a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="354f0-122">Próximamente habrá una [versión ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="354f0-122">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="354f0-123">Los comportamientos anteriores admitidos por los modificadores de compatibilidad se quitarán en esta versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="354f0-123">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="354f0-124">Estamos convencidos de que estos son cambios positivos que beneficiarán a prácticamente todos los usuarios.</span><span class="sxs-lookup"><span data-stu-id="354f0-124">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="354f0-125">Al presentarlos ahora, la mayoría de las aplicaciones podrán empezar a sacar partido de ellos ya y los demás tendrán tiempo suficiente para actualizar sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="354f0-125">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
