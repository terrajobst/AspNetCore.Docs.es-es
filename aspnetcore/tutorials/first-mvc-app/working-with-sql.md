---
title: Trabajar con SQL Server LocalDB
author: rick-anderson
description: "Empleo de SQL Server LocalDB con una aplicación MVC sencilla"
keywords: ASP.NET Core,SQL Server LocalDB,SQL Server,LocalDB
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: d06a957f41d4d8928f825c7a795b3d1518ab4421
ms.sourcegitcommit: c496ec8d5483e7de79b20d06127da7a6cbdac108
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2017
---
# <a name="working-with-sql-server-localdb"></a>Trabajar con SQL Server LocalDB

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos. El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

El sistema [Configuración](xref:fundamentals/configuration) de ASP.NET Core lee el elemento `ConnectionString`. Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:

[!code-javascript[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server. Para más información, vea [Configuración](xref:fundamentals/configuration).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas. LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja. De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.

* En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).

  ![Menú Ver](working-with-sql/_static/ssox.png)

* Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**.

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](working-with-sql/_static/dv.png)

Observe el icono de llave junto a `ID`. De forma predeterminada, EF convierte una propiedad denominada `ID` en la clave principal.

* Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/ssox2.png)

  ![Tabla Movie abierta mostrando datos de la tabla](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicialización de la base de datos

Cree una nueva clase denominada `SeedData` en la carpeta *Models*. Reemplace el código generado por el siguiente:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Adición del inicializador

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Agregue el inicializador al método `Main` del archivo *Program.cs*:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Agregue el inicializador al final del método `Configure` del archivo *Startup.cs*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

Prueba de la aplicación

* Elimine todos los registros de la base de datos. Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.
* Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización. Para forzar la inicialización, se debe detener y reiniciar IIS Express. Puede hacerlo con cualquiera de los siguientes enfoques:

  * Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.

    ![Icono Bandeja del sistema de IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](working-with-sql/_static/stopIIS.png)

   * Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración
   * Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5
   
La aplicación muestra los datos de inicialización.

![La aplicación Movie de MVC se abre en Microsoft Edge y muestra datos de la película](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
[Anterior](adding-model.md)
[Siguiente](controller-methods-views.md)  
