---
title: Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar clases para administrar películas en una base de datos con Entity Framework Core (EF Core).
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: c7341430e8e2ace7eb04faa308020095139d5b94
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410237"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Agregar un modelo a una aplicación de páginas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

En esta sección, se agregan clases para administrar películas en una base de datos. Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos. EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos.

Las clases de modelo se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core. Definen las propiedades de los datos que se almacenan en la base de datos.

[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) un ejemplo.

## <a name="add-a-data-model"></a>Agregar un modelo de datos

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **Película**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Agregue una carpeta denominada *Models*.
* Agregue una clase a la carpeta *Modelos* denominada *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** y seleccione **Agregar** > **Carpeta nueva**. Asigne un nombre a la carpeta *Modelos*.
* Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Archivo nuevo**.
* En el cuadro de diálogo **Nuevo archivo**:

  * Seleccione **General** en el panel izquierdo.
  * Seleccione **Clase vacía** en el panel central.
  * Asigne a la clase el nombre **Películas** y seleccione **Nuevo**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

Compile el proyecto para comprobar que no haya errores de compilación.

## <a name="scaffold-the-movie-model"></a>Aplicar scaffolding al modelo de película

En esta sección se aplica scaffolding al modelo de película; es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de película.

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cree una carpeta *Pages/Movies*:

* Haga clic con el botón derecho en la carpeta *Páginas* > **Agregar** > **Nueva carpeta**.
* Asigne a la carpeta el nombre *Movies*.

Haga clic con el botón derecho en la carpeta *Pages/Movies* > **Agregar** > **Nuevo elemento con scaffolding**.

![Imagen de las instrucciones anteriores.](model/_static/sca.png)

En el cuadro de diálogo **Agregar scaffold**, seleccione **Páginas de Razor que usan Entity Framework (CRUD)** > **Agregar**.

![Imagen de las instrucciones anteriores.](model/_static/add_scaffold.png)

Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:

* En la lista desplegable **Clase de modelo**, seleccione **Movie (RazorPagesMovie.Models)**.
* En la fila **Clase de contexto de datos**, seleccione el signo más **+**, inicie sesión y acepte el nombre generado **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Seleccione **Agregar**.

![Imagen de las instrucciones anteriores.](model/_static/arp.png)

El archivo *appsettings.json* se actualiza con la cadena de conexión que se usa para conectarse a una base de datos local.

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Instale la herramienta de scaffolding:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **En Windows**: Ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **En macOS y Linux**: Ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Instale la herramienta de scaffolding:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* Ejecute el siguiente comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

Los comandos anteriores generan la advertencia siguiente: "No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." ("No se ha especificado ningún tipo en la columna decimal 'Price' en el tipo de entidad 'Movie'. Esto hará que los valores se trunquen inadvertidamente si no caben según la precisión y escala predeterminados. Especifique expresamente el tipo de columna de SQL Server que tenga cabida para todos los valores usando 'HasColumnType()'.")

Puede omitir dicha advertencia, ya que se corregirá en un tutorial posterior.

El proceso de scaffolding crea y actualiza los archivos siguientes:

### <a name="files-created"></a>Archivos creados

* *Pages/Movies*: Create, Delete, Details, Edit e Index.
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>Archivo actualizado

* *Startup.cs*

Los archivos creados y actualizados se explican en la sección siguiente.

<a name="pmc"></a>

## <a name="initial-migration"></a>Migración inicial

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<!-- VS -------------------------->

En esta sección, la Consola del administrador de paquetes (PMC) se utiliza para:

* Agregar una migración inicial.
* Actualizar la base de datos con la migración inicial.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

En PCM, escriba los siguientes comandos:

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

El comando `ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext`, en el archivo *RazorPagesMovieContext.cs*. El argumento `InitialCreate` se usa para asignar nombre a las migraciones. Se puede usar cualquier nombre, pero, por convención, se selecciona uno que describa la migración.

El comando `ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*. El método `Up` crea la base de datos.

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Examinar el contexto registrado con la inserción de dependencias

ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación. Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor. El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.

La herramienta de scaffolding ha creado un contexto de base de datos de forma automática y lo ha registrado con el contenedor de inserción de dependencias.

Examine el método `Startup.ConfigureServices`. El proveedor de scaffolding ha agregado la línea resaltada:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

El elemento `RazorPagesMovieContext` coordina la funcionalidad de EF Core (creación, lectura, actualización, eliminación, etc.) para el modelo `Movie`. El contexto de datos (`RazorPagesMovieContext`) se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

El código anterior crea una propiedad [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para el conjunto de entidades. En la terminología de Entity Framework, un conjunto de entidades suele corresponder a una tabla de base de datos. Una entidad se corresponde con una fila de la tabla.

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `RazorPagesMovieContext` (en el archivo *Data/RazorPagesMovieContext.cs*). El argumento `Initial` se usa para asignar nombre a las migraciones. Se puede usar cualquier nombre, pero, por convención, se utiliza uno que describa la migración. Para obtener más información, vea <xref:data/ef-mvc/migrations>.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/{time-stamp}_InitialCreate.cs*, con lo que se crea la base de datos.

<a name="test"></a>

### <a name="test-the-app"></a>Prueba de la aplicación

* Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador ( `http://localhost:port/movies` ).

Si se produce un error:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Quiere decir que falta el [paso de migraciones](#pmc).

* Pruebe el vínculo **Crear**.

  ![Página Crear](model/_static/conan.png)
  
  > [!NOTE]
  > Es posible que no pueda escribir comas decimales en el campo `Price`. La aplicación debe globalizarse para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos. Para obtener instrucciones sobre la globalización, consulte [esta cuestión en GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).

* Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.

En el tutorial siguiente se explican los archivos creados mediante scaffolding.

> [!div class="step-by-step"]
> [Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
> [Siguiente: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)
