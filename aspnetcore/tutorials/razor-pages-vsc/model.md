---
title: Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Obtenga información sobre cómo agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244728"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="f962e-103">Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f962e-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f962e-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="f962e-104">Add a data model</span></span>

* <span data-ttu-id="f962e-105">Agregue una carpeta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="f962e-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="f962e-106">Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="f962e-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="f962e-107">Agregue el código siguiente al archivo *Modelos/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="f962e-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="f962e-108">Paquetes NuGet de Entity Framework Core para SQLite</span><span class="sxs-lookup"><span data-stu-id="f962e-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="f962e-109">Desde la línea de comandos, ejecute el siguiente comando de la CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="f962e-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="f962e-110">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="f962e-110">Register the database context</span></span>

<span data-ttu-id="f962e-111">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f962e-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="f962e-112">Agregue las instrucciones `using` siguientes en la parte superior de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f962e-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="f962e-113">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="f962e-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="f962e-114">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="f962e-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="f962e-115">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f962e-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f962e-116">**En Windows**, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f962e-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="f962e-117">**En macOS y Linux**, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f962e-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="f962e-118">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f962e-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f962e-119">[Anterior: Introducción](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="f962e-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
