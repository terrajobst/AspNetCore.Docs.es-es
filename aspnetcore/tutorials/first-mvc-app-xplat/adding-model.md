---
title: "Adición de un modelo a una aplicación de ASP.NET Core MVC."
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, API web, REST, Mac, Linux, HTTP, servicio, servicio HTTP, VS Code
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.
* Agregue el código siguiente al archivo *Modelos/Movie.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

La base de datos requiere el campo `ID` para la clave principal. 

Compile la aplicación para comprobar que no tiene ningún error y que ha agregado un **m**odelo a la aplicación **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparar el proyecto para la técnica scaffolding

- Agregue los siguientes paquetes de NuGet resaltados al archivo *MvcMovie.csproj*:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Guarde el archivo y seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".
- Cree un archivo *Modelos/MvcMovieContext.cs* y agregue la siguiente clase `MvcMovieContext`:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Abra el archivo *Startup.cs* y agregue dos instrucciones using:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Agregue el contexto de base de datos al archivo *Startup.cs*:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Esto indica a Entity Framework las clases de modelo que se incluyen en el modelo de datos. Se va a definir un *conjunto de entidades* de objetos Movie, que se representarán en la base de datos como una tabla de películas.

- Compile el proyecto para comprobar que no hay ningún error.

## <a name="scaffold-the-moviecontroller"></a>Aplicar la técnica scaffolding a MovieController

Abra una ventana de terminal en la carpeta del proyecto y ejecute los siguientes comandos:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> Si recibe un error al ejecutar el comando de la técnica scaffolding, vea [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) (Error 444 en el repositorio de scaffolding) para encontrar una solución.

El motor de scaffolding crea lo siguiente:

* Un controlador de películas (*Controllers/MoviesController.cs*)
* Archivos de vistas de Razor para las páginas Crear, Eliminar, Detalles, Editar e Índice (*Views/Movies/\*.cshtml*)

La creación automática de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*. Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos. En el siguiente tutorial trabajaremos con la base de datos.

### <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Globalización y localización](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQLite](working-with-sql.md)
