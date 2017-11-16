---
title: "Adición de un modelo a una aplicación de páginas de Razor con Visual Studio para Mac"
author: rick-anderson
description: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio para Mac"
keywords: "ASP.NET Core,páginas de Razor,Razor,MVC,modelo"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: f2f09909f4c307ce3e1a0c8571b82a709e1f88f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="2f0ec-104">Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2f0ec-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="2f0ec-105">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="2f0ec-105">Add a data model</span></span>

* <span data-ttu-id="2f0ec-106">Agregue una carpeta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-106">Add a folder named *Models*.</span></span>
* <span data-ttu-id="2f0ec-107">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-107">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="2f0ec-108">Agregue el código siguiente al archivo *Modelos/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-108">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="2f0ec-109">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2f0ec-110">Paquetes de Entity Framework Core NuGet para migraciones</span><span class="sxs-lookup"><span data-stu-id="2f0ec-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2f0ec-111">Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="2f0ec-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2f0ec-112">Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="2f0ec-113">**Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="2f0ec-114">Edite el archivo *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="2f0ec-115">Seleccione **Archivo** > **Abrir archivo** y, después, el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-115">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="2f0ec-116">Agregue la referencia de herramientas de `Microsoft.EntityFrameworkCore.Tools.DotNet` al segundo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-116">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="2f0ec-117">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="2f0ec-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="2f0ec-118">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2f0ec-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="2f0ec-119">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-119">Run the following command:</span></span>

<span data-ttu-id="2f0ec-120">**Nota: ejecute el siguiente comando en Windows. Para MacOS y Linux, vea el siguiente comando**</span><span class="sxs-lookup"><span data-stu-id="2f0ec-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="2f0ec-121">En MacOS y Linux, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="2f0ec-122">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="2f0ec-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="2f0ec-123">Salga de Visual Studio y vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="2f0ec-124"> En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2f0ec-124"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2f0ec-125">[Anterior: Introducción](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="2f0ec-125">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
