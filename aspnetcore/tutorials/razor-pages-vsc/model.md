---
title: Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Obtenga información sobre cómo agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 20282b491162e9f35e40702655532a78edceb89a
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="c273f-103">Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c273f-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c273f-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c273f-104">Add a data model</span></span>

* <span data-ttu-id="c273f-105">Agregue una carpeta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="c273f-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="c273f-106">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="c273f-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="c273f-107">Agregue el código siguiente al archivo *Modelos/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="c273f-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="c273f-108">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="c273f-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c273f-109">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="c273f-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c273f-110">Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c273f-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c273f-111">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c273f-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="c273f-112">**Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="c273f-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="c273f-113">Edite el archivo *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c273f-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="c273f-114">Seleccione **Archivo** > **Abrir archivo** y, después, el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c273f-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="c273f-115">Agregue la referencia de herramientas de `Microsoft.EntityFrameworkCore.Tools.DotNet` al segundo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="c273f-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="c273f-116">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="c273f-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="c273f-117">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c273f-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c273f-118">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c273f-118">Run the following command:</span></span>

<span data-ttu-id="c273f-119">**Nota: ejecute el siguiente comando en Windows. Para MacOS y Linux, vea el siguiente comando**</span><span class="sxs-lookup"><span data-stu-id="c273f-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="c273f-120">En MacOS y Linux, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c273f-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="c273f-121">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="c273f-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="c273f-122">Salga de Visual Studio y vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="c273f-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="c273f-123">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c273f-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c273f-124">[Anterior: Introducción](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="c273f-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
