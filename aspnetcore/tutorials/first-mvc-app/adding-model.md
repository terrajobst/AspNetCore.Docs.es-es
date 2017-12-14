---
title: "Adición de un modelo a una aplicación MVC de ASP.NET"
author: rick-anderson
description: "Agregue un modelo a una aplicación sencilla de ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: a29bab9cf0712936fa9c3f2b4bb3b275a46fe6f6
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Nota: Las plantillas de ASP.NET Core 2.0 contienen la carpeta *Models*.

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **MvcMovie** > **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**. Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

La base de datos requiere el campo `ID` para la clave principal. 

Compile el proyecto para comprobar que no contiene errores. Ahora tiene un **m**odelo en la aplicación **M**VC.

## <a name="scaffolding-a-controller"></a>Scaffolding de un controlador

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.

![vista del paso anterior](adding-model/_static/add_controller.png)

En el cuadro de diálogo **Agregar dependencias de MVC**, seleccione **Dependencias mínimas** y **Agregar**.

![vista del paso anterior](adding-model/_static/add_depend.png)

Visual Studio agrega las dependencias necesarias para aplicar la técnica de scaffolding a un controlador, pero no se crea el controlador. La siguiente invocación de **> Agregar > Controlador** crea el controlador. 

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.

![vista del paso anterior](adding-model/_static/add_controller.png)

En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold2.png)

Rellene el cuadro de diálogo **Agregar controlador**:

* **Clase de modelo:** *Movie (MvcMovie.Models)*.
* **Clase de contexto de datos:** seleccione el icono **+** y agregue el valor predeterminado **MvcMovie.Models.MvcMovieContext**.

![Adición de contexto de datos](adding-model/_static/dc.png)

* **Vistas:** conserve el valor predeterminado de cada opción activada.
* **Nombre del controlador:** conserve el valor predeterminado *MoviesController*.
* Pulse en **Agregar**.

![Cuadro de diálogo Agregar controlador](adding-model/_static/add_controller2.png)

Visual Studio crea:

* Una [clase de contexto de base de datos](xref:data/ef-mvc/intro#create-the-database-context) de Entity Framework Core (*Data/MvcMovieContext.cs*)
* Un controlador de películas (*Controllers/MoviesController.cs*)
* Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (*Views/Movies/&ast;.cshtml*)

La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*. Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.

Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Debe crear la base de datos y para ello usar la característica [Migraciones](xref:data/ef-mvc/migrations) de EF Core. Las migraciones permiten crear una base de datos que coincide con el modelo de datos y actualizan el esquema de base de datos cuando cambia el modelo de datos.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Adición de herramientas de EF y migración inicial

En esta sección se usa la Consola del Administrador de paquetes (PMC) para:

* Agregar el paquete de herramientas de Entity Framework Core. Este paquete es necesario para agregar migraciones y actualizar la base de datos.
* Agregar una migración inicial.
* Actualizar la base de datos con la migración inicial.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menú de PMC](adding-model/_static/pmc.png)

En PCM, escriba los siguientes comandos:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Nota:** Si recibe un error con el comando `Install-Package`, abra el administrador de paquetes NuGet y busque el paquete `Microsoft.EntityFrameworkCore.Tools`. De esta forma, podrá instalar el paquete o comprobar si ya está instalado. Como alternativa, vea el [enfoque de la CLI](#cli) si tiene problemas con la PMC.

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Data/MvcMovieContext.cs*). El argumento `Initial` se usa para asignar nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<marca_de_tiempo>_Initial.cs*, con lo que se crea la base de datos.

<a name="cli"></a> Puede realizar los pasos anteriores mediante la interfaz de línea de comandos (CLI) en lugar de la PMC:

* Agregue las [herramientas de EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) al archivo *.csproj*.
* Ejecute los siguientes comandos desde la consola (en el directorio del proyecto):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menú contextual de IntelliSense en un elemento Modelo que enumera las propiedades disponibles para Identificador, Precio, Fecha de lanzamiento y Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Globalización y localización](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Anterior: Agregar una vista](adding-view.md)
[Siguiente: Trabajar con SQL](working-with-sql.md)  
