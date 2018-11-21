---
title: Uso de analizadores de API web
author: pranavkm
description: Obtenga más información sobre los analizadores de API web en Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635405"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="71078-103">Uso de analizadores de API web</span><span class="sxs-lookup"><span data-stu-id="71078-103">Use web API analyzers</span></span>

<span data-ttu-id="71078-104">ASP.NET Core 2.2 presenta el paquete NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) que contiene analizadores para las API web.</span><span class="sxs-lookup"><span data-stu-id="71078-104">ASP.NET Core 2.2 introduces the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="71078-105">Los analizadores funcionan con los controladores anotados con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> y se compilan con [convenciones de la API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="71078-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="71078-106">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="71078-106">Package installation</span></span>

<span data-ttu-id="71078-107">Se puede agregar `Microsoft.AspNetCore.Mvc.Api.Analyzers` con uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="71078-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71078-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71078-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71078-109">En la ventana **Consola del Administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="71078-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="71078-110">Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="71078-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="71078-111">Vaya al directorio en el que esté el archivo *ApiConventions.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71078-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="71078-112">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="71078-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="71078-113">En el cuadro de diálogo **Administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="71078-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="71078-114">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="71078-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="71078-115">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="71078-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="71078-116">En el cuadro de búsqueda, escriba "Microsoft.AspNetCore.Mvc.Api.Analyzers".</span><span class="sxs-lookup"><span data-stu-id="71078-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="71078-117">Seleccione el paquete "Microsoft.AspNetCore.Mvc.Api.Analyzers" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="71078-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71078-118">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="71078-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="71078-119">Haga clic con el botón derecho en la carpeta *Paquetes*, en **Panel de solución** > **Agregar paquetes...**.</span><span class="sxs-lookup"><span data-stu-id="71078-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="71078-120">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="71078-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="71078-121">En el cuadro de búsqueda, escriba "Microsoft.AspNetCore.Mvc.Api.Analyzers".</span><span class="sxs-lookup"><span data-stu-id="71078-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="71078-122">Seleccione el paquete "Microsoft.AspNetCore.Mvc.Api.Analyzers" en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="71078-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71078-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71078-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="71078-124">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="71078-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="71078-125">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="71078-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="71078-126">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="71078-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="71078-127">Analizadores para las convenciones de API</span><span class="sxs-lookup"><span data-stu-id="71078-127">Analyzers for API conventions</span></span>

<span data-ttu-id="71078-128">Los documentos de Open API contienen los códigos de estado y los tipos de respuesta que puede devolver una acción.</span><span class="sxs-lookup"><span data-stu-id="71078-128">Open API documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="71078-129">En ASP.NET Core MVC, los atributos como <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> y <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> se usan para documentar una acción.</span><span class="sxs-lookup"><span data-stu-id="71078-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="71078-130"><xref:tutorials/web-api-help-pages-using-swagger> ofrece información más detallada sobre cómo documentar su API.</span><span class="sxs-lookup"><span data-stu-id="71078-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="71078-131">Uno de los analizadores del paquete inspecciona los controladores anotados con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> e identifica las acciones que no documentan completamente sus respuestas.</span><span class="sxs-lookup"><span data-stu-id="71078-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="71078-132">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="71078-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="71078-133">La acción anterior documenta el tipo de valor devuelto correcto HTTP 200, pero no documenta el código de estado de error HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="71078-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="71078-134">El analizador informa de la documentación que falta para el código de estado 404 de HTTP como una advertencia.</span><span class="sxs-lookup"><span data-stu-id="71078-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="71078-135">Se proporciona una opción para corregir el problema.</span><span class="sxs-lookup"><span data-stu-id="71078-135">An option to fix the problem is provided.</span></span>
