---
title: Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio para Mac
author: rick-anderson
description: Obtenga información sobre cómo agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core usando Visual Studio para Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186763"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Agregar un modelo a una aplicación de páginas de Razor de ASP.NET Core con Visual Studio para Mac

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Agregar un modelo de datos

* En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**. Asigne un nombre a la carpeta *Modelos*.
* Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.
* En el cuadro de diálogo **Nuevo archivo**:

  * Seleccione **General** en el panel izquierdo.
  * Seleccione **Clase vacía** en el panel central.
  * Asigne un nombre a la clase **Película** y seleccione **Nuevo**.

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Haga clic con el botón derecho en una línea ondulada roja, como `MovieContext` en la línea `services.AddDbContext<MovieContext>(options =>`. Seleccione **Corrección rápida > con RazorPagesMovie.Models;**. Visual Studio agregará la instrucción de uso.

Compile el proyecto para comprobar que no contenga errores.

![Página Crear](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Paquetes de Entity Framework Core NuGet para migraciones

Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Haga clic en el vínculo [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) para obtener el número de versión que debe usar. Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*. **Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes.

Para editar un archivo *.csproj*, haga lo siguiente:

* Seleccione **Archivo** > **Abrir** y, después, seleccione el archivo *.csproj*.
* Seleccione **Opciones**.
* Cambie **Abrir con** a **Editor de código fuente**.

![Editar el archivo .csproj](model/csproj.png)

Agregue la referencia de herramientas de `Microsoft.EntityFrameworkCore.Tools.DotNet` al segundo **\<ItemGroup>**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

Los números de versión que se muestran en el código que hay a continuación eran correctos en el momento de escribir este artículo.

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Agregar los archivos de página o película al proyecto

* En Visual Studio, haga clic con el botón derecho en la carpeta *Páginas* y seleccione **Agregar > Agregar carpeta existente**.
* Seleccione la carpeta *Películas*.
* En el cuadro de diálogo *Elegir los archivos que se incluirán en el proyecto*, seleccione **Incluir todos**.

En el tutorial siguiente se explican los archivos creados mediante scaffolding.

> [!div class="step-by-step"]
> [Anterior: Introducción](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages-mac/page)
