---
title: Agregar un modelo a una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Agregue un modelo a una aplicación sencilla de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 802cb458cb05579b97256022b56d6f97a03d2f1a
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687797"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Agregar un modelo a una aplicación de ASP.NET Core MVC

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Haga clic con el botón derecho en la carpeta *Models* > **Agregar** > **Clase**. Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

La base de datos requiere el campo `ID` para la clave principal. 

Compile el proyecto para comprobar que no contiene errores. Ahora tiene un **m**odelo en la aplicación **M**VC.

## <a name="scaffolding-a-controller"></a>Scaffolding de un controlador

::: moniker range=">= aspnetcore-2.1"

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Nuevo elemento con scaffold**.

![vista del paso anterior](adding-model/_static/add_controller21.png)

En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Controladores* **> Agregar > Controlador**.

![vista del paso anterior](adding-model/_static/add_controller.png)

Si aparece el cuadro de diálogo **Agregar dependencias de MVC**:

* [Actualice Visual Studio a la última versión](https://www.visualstudio.com/downloads/). La versiones de Visual Studio anteriores a la 15.5 muestran este cuadro de diálogo.
* Si no puede actualizar, seleccione **AGREGAR** y luego siga los pasos para agregar el controlador de nuevo.

En el cuadro de diálogo **Agregar scaffold**, pulse en **Controlador de MVC con vistas que usan Entity Framework > Agregar**.

![Cuadro de diálogo Agregar scaffold](adding-model/_static/add_scaffold2.png)

::: moniker-end

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
* Archivos de vistas Razor para las páginas de creación, eliminación, detalles, edición e índice (<em>Views/Movies/&ast;.cshtml</em>)

La creación automática del contexto de base de datos y de vistas y métodos de acción [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) se conoce como *scaffolding*. Pronto contará con una aplicación web totalmente funcional que le permitirá administrar una base de datos de películas.

Si ejecuta la aplicación y hace clic en el vínculo **Mvc Movie**, aparece un error similar al siguiente:

``` error
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

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

Pase por alto el siguiente mensaje de error, lo subsanaremos en el próximo tutorial:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.* (No se ha especificado ningún tipo en la columna decimal "Price" en el tipo de entidad "Movie". Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando "ForHasColumnType()". Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados.)

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Nota:** Si recibe un error con el comando `Install-Package`, abra el administrador de paquetes NuGet y busque el paquete `Microsoft.EntityFrameworkCore.Tools`. De esta forma, podrá instalar el paquete o comprobar si ya está instalado. Como alternativa, vea el [enfoque de la CLI](#cli) si tiene problemas con la PMC.

::: moniker-end

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Data/MvcMovieContext.cs*). El argumento `Initial` se usa para asignar nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<marca_de_tiempo>_Initial.cs*, con lo que se crea la base de datos.

<a name="cli"></a> Puede realizar los pasos anteriores mediante la interfaz de línea de comandos (CLI) en lugar de la PMC:

* Agregue las [herramientas de EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) al archivo *.csproj*.
* Ejecute los siguientes comandos desde la consola (en el directorio del proyecto):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Si ejecuta la aplicación y aparece el error:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Probablemente se deba a que no ha ejecutado `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menú contextual de IntelliSense en un elemento Modelo que enumera las propiedades disponibles para Identificador, Precio, Fecha de lanzamiento y Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [Globalización y localización](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Anterior: Agregar una vista](adding-view.md)
> [Siguiente: Trabajar con SQL](working-with-sql.md)  
