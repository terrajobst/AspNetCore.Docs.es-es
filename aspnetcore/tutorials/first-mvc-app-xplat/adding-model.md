---
title: Agregar un modelo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Agregue un modelo a una aplicación sencilla de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 77750ba0df7775d6a0e4744811848bfe9782d995
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Agregar un modelo a una aplicación de ASP.NET Core MVC

[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.
* Agregue el código siguiente al archivo *Modelos/Movie.cs*:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

La base de datos requiere el campo `ID` para la clave principal. 

Compile la aplicación para comprobar que no tiene ningún error y que ha agregado un **m**odelo a la aplicación **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparar el proyecto para la técnica scaffolding

- Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Guarde el archivo y seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".
- Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Abra el archivo *Startup.cs* y agregue dos instrucciones using:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Agregue el contexto de base de datos al archivo *Startup.cs*:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos. Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.

- Compile el proyecto para comprobar que no hay ningún error.

## <a name="scaffold-the-moviecontroller"></a>Aplicar la técnica scaffolding a MovieController

Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
El motor de scaffolding crea lo siguiente:

* Un controlador de películas (*Controllers/MoviesController.cs*)
* Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)

La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*. Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos. En el siguiente tutorial trabajaremos con la base de datos.

### <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Globalización y localización](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con SQLite](working-with-sql.md)
