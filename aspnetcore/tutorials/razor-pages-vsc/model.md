---
title: "Adición de un modelo a una aplicación de páginas de Razor con Visual Studio para Mac"
author: rick-anderson
description: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio para Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 9600392b47fb8b1dded06faefaff1bf87d67af4e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-code"></a>Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core con Visual Studio Code

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

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

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
[Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages-vsc/page)
