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
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio para Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Agregar un modelo de datos

* Agregue una carpeta denominada *Modelos*.
* Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.
* Agregue el código siguiente al archivo *Modelos/Movie.cs*:

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Compile el proyecto para comprobar que no contiene errores.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Paquetes de Entity Framework Core NuGet para migraciones

Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*. **Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.

Edite el archivo *RazorPagesMovie.csproj*:

* Seleccione **Archivo** > **Abrir archivo** y, después, el archivo *RazorPagesMovie.csproj*.
* Agregue la referencia de herramientas de `Microsoft.EntityFrameworkCore.Tools.DotNet` al segundo **\<ItemGroup>**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Aplicar scaffolding al modelo de película

* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute el siguiente comando:

**Nota: ejecute el siguiente comando en Windows. Para MacOS y Linux, vea el siguiente comando**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* En MacOS y Linux, ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Si se produce un error:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Salga de Visual Studio y vuelva a ejecutar el comando.

[!INCLUDE[model 4](../../includes/RP/model4.md)] En el tutorial siguiente se explican los archivos creados mediante scaffolding.

>[!div class="step-by-step"]
[Anterior: Introducción](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)
