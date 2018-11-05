---
title: Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.All no se recomienda para ASP.NET Core 2.1 y versiones posteriores.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148855"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="e41ed-103">Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="e41ed-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="e41ed-104">Se recomienda que las aplicaciones que tengan como destino ASP.NET Core 2.1 y versiones posteriores usen el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en lugar de este paquete.</span><span class="sxs-lookup"><span data-stu-id="e41ed-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="e41ed-105">Consulte [Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) en este artículo.</span><span class="sxs-lookup"><span data-stu-id="e41ed-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="e41ed-106">Esta característica requiere ASP.NET Core 2.x con .NET Core 2.x como destino.</span><span class="sxs-lookup"><span data-stu-id="e41ed-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="e41ed-107">El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e41ed-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="e41ed-108">Todos los paquetes admitidos por el equipo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e41ed-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="e41ed-109">Todos los paquetes admitidos por Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e41ed-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="e41ed-110">Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e41ed-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="e41ed-111">Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x están incluidas en el paquete `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="e41ed-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="e41ed-112">Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.0 usan este paquete.</span><span class="sxs-lookup"><span data-stu-id="e41ed-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="e41ed-113">El número de versión del metapaquete `Microsoft.AspNetCore.All` representa la versión de ASP.NET Core y la versión de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e41ed-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="e41ed-114">Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.All` pueden aprovechar automáticamente el [almacén en tiempo de ejecución de .NET Core](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="e41ed-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="e41ed-115">El almacén en tiempo de ejecución contiene todos los recursos en tiempo de ejecución necesarios para ejecutar aplicaciones de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="e41ed-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="e41ed-116">Al usar el metapaquete `Microsoft.AspNetCore.All`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el almacén en tiempo de ejecución de .NET Core ya contiene esos recursos.</span><span class="sxs-lookup"><span data-stu-id="e41ed-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="e41ed-117">Los recursos del almacén en tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e41ed-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="e41ed-118">Puede usar el proceso de recorte de paquetes para quitar los paquetes que no se usan.</span><span class="sxs-lookup"><span data-stu-id="e41ed-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="e41ed-119">Los paquetes recortados se excluyen de la salida de la aplicación publicada.</span><span class="sxs-lookup"><span data-stu-id="e41ed-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="e41ed-120">El siguiente archivo *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.All` de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e41ed-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="e41ed-121">Control de versiones implícitas</span><span class="sxs-lookup"><span data-stu-id="e41ed-121">Implicit versioning</span></span>

<span data-ttu-id="e41ed-122">En ASP.NET Core 2.1 o una versión posterior, puede especificar la referencia de paquete `Microsoft.AspNetCore.All` sin una versión.</span><span class="sxs-lookup"><span data-stu-id="e41ed-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="e41ed-123">Si no se especifica la versión, el SDK define una versión implícita (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="e41ed-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="e41ed-124">Es recomendable confiar en la versión implícita especificada por el SDK y no establecer de forma explícita el número de versión en la referencia del paquete.</span><span class="sxs-lookup"><span data-stu-id="e41ed-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="e41ed-125">Si tiene alguna pregunta sobre este enfoque, deje un comentario de GitHub en [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Debate sobre la versión implícita de Microsoft.AspNetCore.App).</span><span class="sxs-lookup"><span data-stu-id="e41ed-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="e41ed-126">La versión implícita se establece en `major.minor.0` para las aplicaciones portátiles.</span><span class="sxs-lookup"><span data-stu-id="e41ed-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="e41ed-127">El mecanismo de puesta al día del marco de uso compartido ejecuta la aplicación en la versión más reciente compatible entre los marcos de uso compartidos instalados.</span><span class="sxs-lookup"><span data-stu-id="e41ed-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="e41ed-128">Para garantizar que se use la misma versión en el desarrollo, las pruebas y la producción, asegúrese de que en todos los entornos esté instalada la misma versión del marco de uso compartido.</span><span class="sxs-lookup"><span data-stu-id="e41ed-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="e41ed-129">Para las aplicaciones autocontenidas, el número de versión implícita se establece en el valor `major.minor.patch` del marco de uso compartido incluido en el SDK instalado.</span><span class="sxs-lookup"><span data-stu-id="e41ed-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="e41ed-130">Especificar un número de versión en la referencia de paquete `Microsoft.AspNetCore.All` **no** garantiza que se seleccione la versión del marco de uso compartido.</span><span class="sxs-lookup"><span data-stu-id="e41ed-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="e41ed-131">Por ejemplo, suponga que se especifica la versión "2.1.1", pero está instalada la "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="e41ed-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="e41ed-132">En ese caso, la aplicación usará el valor "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="e41ed-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="e41ed-133">Aunque no se recomienda, puede deshabilitar la puesta al día (revisión o secundaria).</span><span class="sxs-lookup"><span data-stu-id="e41ed-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="e41ed-134">Para obtener más información sobre la puesta al día del host de dotnet y cómo configurar su comportamiento, vea [Dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Puesta al día del host de dotnet).</span><span class="sxs-lookup"><span data-stu-id="e41ed-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="e41ed-135">El SDK del proyecto se debe establecer en `Microsoft.NET.Sdk.Web` en el archivo de proyecto para usar la versión implícita de `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="e41ed-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="e41ed-136">Cuando se especifica el SDK `Microsoft.NET.Sdk` (`<Project Sdk="Microsoft.NET.Sdk">` en la parte superior del archivo del proyecto), se genera la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="e41ed-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="e41ed-137">*Advertencia NU1604: La dependencia del proyecto Microsoft.AspNetCore.All no contiene un límite inferior inclusivo. Incluya un límite inferior en la versión de dependencia para garantizar que los resultados de restauración son coherentes.*</span><span class="sxs-lookup"><span data-stu-id="e41ed-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="e41ed-138">Este es un problema conocido con el SDK de .NET Core 2.1 y se corregirá en el SDK de .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="e41ed-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="e41ed-139">Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="e41ed-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="e41ed-140">En `Microsoft.AspNetCore.All` se incluyen los siguientes paquetes, pero no el paquete `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e41ed-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="e41ed-141">Para pasar de `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, si su aplicación usa las API de los paquetes anteriores, o bien paquetes incluidos en ellos, agregue las referencias correspondientes a dichos paquetes en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e41ed-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="e41ed-142">No se incluye implícitamente ninguna dependencia de los paquetes anteriores que no sea, de otro modo, una dependencia de `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e41ed-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="e41ed-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e41ed-143">For example:</span></span>

* <span data-ttu-id="e41ed-144">`StackExchange.Redis` como dependencia de `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="e41ed-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="e41ed-145">`Microsoft.ApplicationInsights` como dependencia de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="e41ed-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="e41ed-146">Actualización de ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e41ed-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="e41ed-147">Se recomienda migrar al metapaquete `Microsoft.AspNetCore.App` con la versión 2.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e41ed-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="e41ed-148">Para seguir usando el metapaquete `Microsoft.AspNetCore.All` y asegurarse de que se ha implementado la última versión de la revisión:</span><span class="sxs-lookup"><span data-stu-id="e41ed-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="e41ed-149">En los equipos de desarrollo y los servidores de compilación: instale el último [SDK de .NET Core](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="e41ed-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="e41ed-150">En los servidores de implementación: instale el último [.NET Core Runtime](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="e41ed-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="e41ed-151">Su aplicación se pondrá al día con la última versión instalada al reiniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e41ed-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
