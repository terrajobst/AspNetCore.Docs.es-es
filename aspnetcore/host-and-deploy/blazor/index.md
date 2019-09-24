---
title: Hospedaje e implementación de ASP.NET Core Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 1cfe87c7194b34c2461429225c560f9e689168ae
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211700"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="5ae35-103">Hospedaje e implementación de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="5ae35-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="5ae35-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="5ae35-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="5ae35-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="5ae35-105">Publish the app</span></span>

<span data-ttu-id="5ae35-106">Las aplicaciones se publican para implementación en la configuración de versión.</span><span class="sxs-lookup"><span data-stu-id="5ae35-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ae35-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ae35-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5ae35-108">Seleccione **Compilar** > **Publicar {aplicación}** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="5ae35-109">Seleccione el *destino de publicación*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-109">Select the *publish target*.</span></span> <span data-ttu-id="5ae35-110">Para publicar localmente, seleccione **Carpeta**.</span><span class="sxs-lookup"><span data-stu-id="5ae35-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="5ae35-111">Acepte la ubicación predeterminada del campo **Elegir una carpeta** o especifique una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="5ae35-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="5ae35-112">Seleccione el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="5ae35-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5ae35-113">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="5ae35-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5ae35-114">Use el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) para publicar la aplicación con una configuración de versión:</span><span class="sxs-lookup"><span data-stu-id="5ae35-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="5ae35-115">Al publicar la aplicación se desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y se [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="5ae35-116">Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="5ae35-117">Una aplicación WebAssembly de Blazor se publica en la carpeta */bin/Release/{RED DE DESTINO}/publish/{NOMBRE DE ENSAMBLADO}/dist*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-117">A Blazor WebAssembly app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="5ae35-118">Una aplicación de servidor Blazor se publica en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-118">A Blazor Server app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="5ae35-119">Los recursos de la carpeta se implementan en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="5ae35-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="5ae35-120">La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="5ae35-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="5ae35-121">Ruta de acceso base de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5ae35-121">App base path</span></span>

<span data-ttu-id="5ae35-122">La *ruta de acceso base de la aplicación* es la de la dirección URL raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="5ae35-123">Tenga en cuenta la siguiente aplicación principal y la de Blazor:</span><span class="sxs-lookup"><span data-stu-id="5ae35-123">Consider the following main app and Blazor app:</span></span>

* <span data-ttu-id="5ae35-124">La aplicación principal se llama `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="5ae35-124">The main app is called `MyApp`:</span></span>
  * <span data-ttu-id="5ae35-125">La aplicación reside físicamente en *d:\\MyApp*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-125">The app physically resides at *d:\\MyApp*.</span></span>
  * <span data-ttu-id="5ae35-126">Las solicitudes se reciben en `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="5ae35-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="5ae35-127">Una aplicación Blazor llamada `CoolApp` es una subaplicación de `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="5ae35-127">A Blazor app called `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="5ae35-128">La subaplicación reside físicamente en *d:\\MyApp\\CoolApp*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-128">The sub-app physically resides at *d:\\MyApp\\CoolApp*.</span></span>
  * <span data-ttu-id="5ae35-129">Las solicitudes se reciben en `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="5ae35-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="5ae35-130">Sin especificar una configuración adicional para `CoolApp`, la subaplicación de este escenario desconoce dónde reside en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5ae35-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="5ae35-131">Por ejemplo, la aplicación no puede construir URL relativas correctas para sus recursos sin saber que reside en la ruta de acceso URL relativa `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="5ae35-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="5ae35-132">Para proporcionar la configuración de la ruta de acceso base de la aplicación Blazor de `https://www.contoso.com/CoolApp/`, el atributo `href` de la etiqueta `<base>` se establece en la ruta de acceso raíz relativa en el archivo *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="5ae35-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *wwwroot/index.html* file:</span></span>

```html
<base href="/CoolApp/">
```

<span data-ttu-id="5ae35-133">Al proporcionar la ruta de acceso URL relativa, un componente que no se encuentre en el directorio raíz puede construir direcciones URL relativas a la ruta de acceso raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-133">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="5ae35-134">Los componentes que se encuentran en diferentes niveles de la estructura de directorios pueden compilar vínculos a otros recursos en ubicaciones de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-134">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="5ae35-135">La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.</span><span class="sxs-lookup"><span data-stu-id="5ae35-135">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="5ae35-136">En muchos escenarios de hospedaje, la ruta de acceso URL relativa a la aplicación es la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-136">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="5ae35-137">En estos casos, la ruta de acceso URL relativa de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="5ae35-137">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="5ae35-138">En otros escenarios de hospedaje, como las subaplicaciones de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso URL relativa del servidor a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-138">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path to the app.</span></span>

<span data-ttu-id="5ae35-139">Para establecer la ruta de acceso base de la aplicación, actualice la etiqueta `<base>` dentro de los elementos de etiqueta `<head>` del archivo *wwwroot/index.HTML*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-139">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="5ae35-140">Establezca el valor del atributo `href` en `/{RELATIVE URL PATH}/` (la barra diagonal final es necesaria), donde `{RELATIVE URL PATH}` es la ruta de acceso URL relativa completa de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ae35-140">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="5ae35-141">En el caso de una aplicación con una ruta de acceso URL relativa que no sea raíz (por ejemplo, `<base href="/CoolApp/">`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*.</span><span class="sxs-lookup"><span data-stu-id="5ae35-141">For an app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="5ae35-142">Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5ae35-142">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="5ae35-143">Para pasar el argumento de ruta de acceso base al ejecutar la aplicación de forma local, ejecute el comando `dotnet run` desde el directorio de la aplicación con la opción `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="5ae35-143">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="5ae35-144">Para una aplicación con una ruta de acceso URL relativa de `/CoolApp/` (`<base href="/CoolApp/">`), el comando es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="5ae35-144">For an app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="5ae35-145">La aplicación responde de forma local en `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="5ae35-145">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="5ae35-146">Implementación</span><span class="sxs-lookup"><span data-stu-id="5ae35-146">Deployment</span></span>

<span data-ttu-id="5ae35-147">Para una guía sobre la implementación, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="5ae35-147">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
