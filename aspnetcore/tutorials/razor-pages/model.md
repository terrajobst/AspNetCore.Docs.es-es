---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961180"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Agregar un modelo de datos

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:

Reemplace el contenido de la clase `Movie` por el siguiente código:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Aplicar scaffolding al modelo de película

En esta sección se aplica scaffolding al modelo de película; es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.

Cree una carpeta *Pages/Movies*:

* En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages* > **Agregar** > **Nueva carpeta**.
* Asigne a la carpeta el nombre *Movies*.

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scanffold**.

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:

* En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.
* En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.
* En la lista desplegable **Clase de contexto de datos**, seleccione **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Seleccione **Agregar**.

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

El proceso de scaffolding ha creado y cambiado los archivos siguientes:

### <a name="files-created"></a>Archivos creados

* *Pages/Movies* Create, Delete, Details, Edit, Index. Estas páginas se detallan en el tutorial siguiente.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Actualizaciones de archivos

* *Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.
* *appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Examinar el contexto registrado con la inserción de dependencias

ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación. Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor. El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.

La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.

Examine el método `Startup.ConfigureServices`. El proveedor de scaffolding ha agregado la línea resaltada:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos. El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos. En este proyecto, la clase se denomina `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

En el código anterior se crea una propiedad [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades. En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos. Una entidad se corresponde con una fila de la tabla.

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Realizar la migración inicial

En esta sección, usará la Consola del Administrador de paquetes (PMC) para:

* Agregar una migración inicial.
* Actualizar la base de datos con la migración inicial.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

En PCM, escriba los siguientes comandos:

```PMC
Add-Migration Initial
Update-Database
```

Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Pase por alto el siguiente mensaje de advertencia, lo subsanaremos en el próximo tutorial:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*). El argumento `Initial` se usa para asignar nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.

Si se produce un error:

SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-GUID" solicitada por el inicio de sesión. Error de inicio de sesión.
Error de inicio de sesión del usuario <nombre de usuario>.

Quiere decir que falta el [paso de migraciones](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Agregar un modelo de datos

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Agregar una cadena de conexión de base de datos

Agregue una cadena de conexión al archivo *appsettings.json*.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrar el contexto de base de datos

Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el [método ConfigureServices de la clase Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Compile el proyecto para comprobar que no contiene errores.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Agregar herramientas de scaffolding y realizar la migración inicial

En esta sección, usará la Consola del Administrador de paquetes (PMC) para:

* Agregar el paquete de generación de código de Visual Studio web. Este paquete es necesario para ejecutar el motor de scaffolding.
* Agregar una migración inicial.
* Actualizar la base de datos con la migración inicial.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

En PCM, escriba los siguientes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Se pueden usar los siguientes comandos de la CLI de .NET Core de forma alternativa:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Pase por alto el siguiente mensaje:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Lo subsanaremos en el siguiente tutorial.

El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*). El argumento `Initial` se usa para asignar un nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Prueba de la aplicación

* Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).
* Pruebe el vínculo **Crear**.

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.

Si obtiene una excepción SQL, confirme que ha ejecutado las migraciones y que la base de datos está actualizada.

En el tutorial siguiente se explican los archivos creados mediante scaffolding.

> [!div class="step-by-step"]
> [Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Páginas de Razor creadas mediante scaffolding](xref:tutorials/razor-pages/page)
