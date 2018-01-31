---
title: "Adición de un modelo a una aplicación de páginas de Razor con Visual Studio para Mac"
author: rick-anderson
description: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: f4fb4fc3402c866fa9f956341c06be34ca9f4763
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="9d465-103">Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="9d465-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9d465-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="9d465-104">Add a data model</span></span>

* <span data-ttu-id="9d465-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**.</span><span class="sxs-lookup"><span data-stu-id="9d465-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="9d465-106">Asigne un nombre a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="9d465-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="9d465-107">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.</span><span class="sxs-lookup"><span data-stu-id="9d465-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="9d465-108">En el cuadro de diálogo **Nuevo archivo**:</span><span class="sxs-lookup"><span data-stu-id="9d465-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="9d465-109">Seleccione **General** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="9d465-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="9d465-110">Seleccione **Clase vacía** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="9d465-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="9d465-111">Asigne un nombre a la clase **Película** y seleccione **Nuevo**.</span><span class="sxs-lookup"><span data-stu-id="9d465-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="9d465-112">Haga clic con el botón derecho en una línea ondulada roja, como `MovieContext` en la línea `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="9d465-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="9d465-113">Seleccione **Corrección rápida > con RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="9d465-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="9d465-114">Visual Studio agregará la instrucción de uso.</span><span class="sxs-lookup"><span data-stu-id="9d465-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="9d465-115">Compile el proyecto para comprobar que no contenga errores.</span><span class="sxs-lookup"><span data-stu-id="9d465-115">Build the project to verify you don't have any errors.</span></span>

![Página Crear](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="9d465-117">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="9d465-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="9d465-118">Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="9d465-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="9d465-119">Haga clic en el vínculo [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) para obtener el número de versión que debe usar.</span><span class="sxs-lookup"><span data-stu-id="9d465-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="9d465-120">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9d465-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="9d465-121">**Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="9d465-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="9d465-122">Para editar un archivo *.csproj*, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9d465-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="9d465-123">Seleccione **Archivo** > **Abrir** y, después, seleccione el archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9d465-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="9d465-124">Seleccione **Opciones**.</span><span class="sxs-lookup"><span data-stu-id="9d465-124">Select **Options**.</span></span>
* <span data-ttu-id="9d465-125">Cambie **Abrir con** a **Editor de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="9d465-125">Change **Open with** to **Source Code Editor**.</span></span>

![Editar el archivo .csproj](model/csproj.png)

<span data-ttu-id="9d465-127">Agregue la referencia de herramientas de `Microsoft.EntityFrameworkCore.Tools.DotNet` al segundo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="9d465-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="9d465-128">Los números de versión que se muestran en el código que hay a continuación eran correctos en el momento de escribir este artículo.</span><span class="sxs-lookup"><span data-stu-id="9d465-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="9d465-129">Agregar los archivos de página o película al proyecto</span><span class="sxs-lookup"><span data-stu-id="9d465-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="9d465-130">En Visual Studio, haga clic con el botón derecho en la carpeta *Páginas* y seleccione **Agregar > Agregar carpeta existente**.</span><span class="sxs-lookup"><span data-stu-id="9d465-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="9d465-131">Seleccione la carpeta *Películas*.</span><span class="sxs-lookup"><span data-stu-id="9d465-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="9d465-132">En el cuadro de diálogo *Elegir los archivos que se incluirán en el proyecto*, seleccione **Incluir todos**.</span><span class="sxs-lookup"><span data-stu-id="9d465-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="9d465-133">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9d465-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9d465-134">[Anterior: Introducción](xref:tutorials/razor-pages-mac/razor-pages-start)
[Siguiente: Scaffolded Razor Pages (Páginas de Razor creadas mediante scaffolding)](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9d465-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
