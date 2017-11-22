---
title: "Agregar un modelo a una aplicación de ASP.NET Core MVC"
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
keywords: ASP.NET Core,MVC,scaffold,scaffolding
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: ff0b262bdf2685bd1bc410c30c12aa2d16f6dcda
ms.sourcegitcommit: 7d092cd99057bad9246c472a8a0a8cbc7ab9fa9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* Haga clic con el botón derecho en la carpeta *Modelos* y, luego, seleccione **Agregar** > **Nuevo archivo**. 
* En el cuadro de diálogo **Nuevo archivo**:

  * Seleccione **General** en el panel izquierdo.
  * Seleccione **Clase vacía** en el panel central.
  * Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.

Agregue las propiedades siguientes a la clase `Movie`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

La base de datos requiere el campo `ID` para la clave principal.

Compile el proyecto para comprobar que no contiene errores. Ahora tiene un **m**odelo en su aplicación **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparar el proyecto para la técnica scaffolding

- Haga clic con el botón derecho en el archivo del proyecto y, luego, seleccione **Herramientas > Editar archivo**.

  ![vista del paso anterior](adding-model/_static/1.png)

- Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Guarde el archivo.

- Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Abra el archivo *Startup.cs* y agregue dos instrucciones using:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Agregue el contexto de base de datos al archivo *Startup.cs*:

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos. Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.

- Compile el proyecto para comprobar que no hay ningún error.

## <a name="scaffold-the-moviecontroller"></a>Aplicar la técnica scaffolding a MovieController

Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Si recibe el error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:

 * Se encuentra en el directorio del proyecto. El directorio del proyecto tiene los archivos *Program.cs*, *Startup.cs* y *.csproj*.
 * La versión de dotnet es la 1.1 o posterior. Ejecute `dotnet` para obtener la versión.
 * Ha agregado el elemento `<DotNetCliToolReference>` al [archivo MvcMovie.csproj](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

El motor de scaffolding crea lo siguiente:

* Un controlador de películas (*Controllers/MoviesController.cs*)
* Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)

La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*. Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.

### <a name="add-the-files-to-visual-studio"></a>Agregar los archivos a Visual Studio

* Agregue el archivo *MovieController.cs* al proyecto de Visual Studio:

  * Haga clic con el botón derecho en la carpeta *Controllers* y seleccione **Agregar > Agregar archivos**.
  * Seleccione el archivo *MovieController.cs*.

* Agregue las vistas y la carpeta *Películas*:

  * Haga clic con el botón derecho en la carpeta *Vistas* y seleccione **Agregar > Agregar carpeta existente**.
  * Vaya a la carpeta *Vistas*, seleccione *Vistas\Películas* y seleccione **Abrir**.
  * En el cuadro de diálogo **Seleccione los archivos que se deben agregar de Películas**, seleccione **Incluir todo** y **Aceptar**.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos. En el siguiente tutorial trabajaremos con la base de datos.

## <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Globalización y localización](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQL](working-with-sql.md)  
