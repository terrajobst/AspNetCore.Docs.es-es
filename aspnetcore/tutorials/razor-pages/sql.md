---
title: Trabajar con SQL Server LocalDB y ASP.NET Core
author: rick-anderson
description: Explica cómo trabajar con SQL Server LocalDB y ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 20e2353eb2e453235c2fb04c696a7e3d27bed5bf
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011279"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Trabajar con SQL Server LocalDB y ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette) 

El objeto `MovieContext` controla la tarea de conexión a la base de datos y de asignación de objetos `Movie` a los registros de la base de datos. El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Para más información sobre los modelos empleados en `ConfigureServices`, vea:

* [Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr) para `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:mvc/compatibility-version)

::: moniker-end

El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`. Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*. El valor de nombre de la base de datos (`Database={Database name}`) será distinto en su código generado. El valor de nombre es arbitrario.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server. Para más información, vea [Configuración](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas. LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja. De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.

<a name="ssox"></a>
* En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).

  ![Menú Ver](sql/_static/ssox.png)

* Haga clic con el botón derecho en la tabla `Movie` y seleccione **Diseñador de vistas**:

  ![Menú contextual abierto en la tabla Movie](sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](sql/_static/dv.png)

Observe el icono de llave junto a `ID`. De forma predeterminada, EF crea una propiedad denominada `ID` para la clave principal.

* Haga clic con el botón derecho en la tabla `Movie` y seleccione **Ver datos**:

  ![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicialización de la base de datos

Cree una nueva clase denominada `SeedData` en la carpeta *Models*. Reemplace el código generado con el siguiente:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Agregar el inicializador

En *Program.cs*, modifique el método `Main` para que haga lo siguiente:

* Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.
* Llamar al método de inicialización, pasándolo al contexto.
* Eliminar el contexto cuando el método de inicialización finalice.

En el código siguiente se muestra el archivo *Program.cs* actualizado.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Una aplicación de producción no llamaría a `Database.Migrate`. Se agrega al código anterior para evitar que se produzca la siguiente excepción cuando `Update-Database` no se ha ejecutado:

SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-21" solicitada por el inicio de sesión. Error de inicio de sesión.
Error de inicio de sesión del usuario <nombre de usuario>.

### <a name="test-the-app"></a>Prueba de la aplicación

* Elimine todos los registros de la base de datos. Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).
* Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización. Para forzar la inicialización, se debe detener y reiniciar IIS Express. Puede hacerlo con cualquiera de los siguientes enfoques:

  * Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**:

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

    * Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.
    * Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.
   
La aplicación muestra los datos inicializados:

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

El siguiente tutorial limpia la presentación de los datos.

> [!div class="step-by-step"]
> [Anterior: Páginas de Razor con scaffolding](xref:tutorials/razor-pages/page)
> [Siguiente: Actualización de las páginas](xref:tutorials/razor-pages/da1)
