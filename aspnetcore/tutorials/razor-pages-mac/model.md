---
title: "Agregar un modelo a una aplicación de páginas de Razor con Visual Studio para Mac"
author: rick-anderson
description: "Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio para Mac"
keywords: "ASP.NET Core,páginas Razor,Razor,MVC,modelo"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="c61ed-104">Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c61ed-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c61ed-105">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c61ed-105">Add a data model</span></span>

* <span data-ttu-id="c61ed-106">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="c61ed-107">Asigne un nombre a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="c61ed-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="c61ed-108">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="c61ed-109">En el cuadro de diálogo **Archivo nuevo**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c61ed-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="c61ed-110">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="c61ed-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="c61ed-111">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="c61ed-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="c61ed-112">Asigne un nombre a la clase **Película** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="c61ed-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="c61ed-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="c61ed-114">Haga clic con el botón derecho en una línea ondulada roja, como `MovieContext` en la línea `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="c61ed-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="c61ed-115">Seleccione **Corrección rápida > con RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="c61ed-116">Visual Studio agregará la instrucción de uso.</span><span class="sxs-lookup"><span data-stu-id="c61ed-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="c61ed-117">Compile el proyecto para comprobar que no contenga errores.</span><span class="sxs-lookup"><span data-stu-id="c61ed-117">Build the project to verify you don't have any errors.</span></span>

![Página Crear](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c61ed-119">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="c61ed-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c61ed-120">Las herramientas de EF para la interfaz de nivel de llamada (CLI) se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c61ed-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c61ed-121">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c61ed-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="c61ed-122">**Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="c61ed-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="c61ed-123">Para editar un archivo *.csproj*, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c61ed-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="c61ed-124">Seleccione **Archivo > Abrir** y elija el archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c61ed-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="c61ed-125">Seleccione **Opciones**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-125">Select **Options**.</span></span>
* <span data-ttu-id="c61ed-126">Cambie **Abrir con** a **Editor de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-126">Change **Open with** to **Source Code Editor**.</span></span>

![Editar el archivo .csproj](model/csproj.png)

<span data-ttu-id="c61ed-128">El código siguiente muestra el archivo *csproj* actualizado.</span><span class="sxs-lookup"><span data-stu-id="c61ed-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="c61ed-129">Agregar los archivos de página o película al proyecto</span><span class="sxs-lookup"><span data-stu-id="c61ed-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="c61ed-130">En Visual Studio, haga clic con el botón derecho en la carpeta *Páginas* y seleccione **Agregar > Agregar carpeta existente**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="c61ed-131">Seleccione la carpeta *Películas*.</span><span class="sxs-lookup"><span data-stu-id="c61ed-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="c61ed-132">En el cuadro de diálogo *Elegir los archivos que se incluirán en el proyecto*, seleccione **Incluir todos**.</span><span class="sxs-lookup"><span data-stu-id="c61ed-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="c61ed-133">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c61ed-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c61ed-134">[Anterior: Introducción](xref:tutorials/razor-pages-mac/razor-pages-start)
[Siguiente: Scaffolded Razor Pages (Páginas de Razor creadas mediante scaffolding)](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="c61ed-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
