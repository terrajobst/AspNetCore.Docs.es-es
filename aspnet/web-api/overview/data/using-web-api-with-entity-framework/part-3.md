---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar migraciones de Code First para inicializar la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 533d3a6e9a69584fb2523b2c0a604755e372f031
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839196"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Usar migraciones de Code First para inicializar la base de datos
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, usará [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) en EF para inicializar la base de datos con datos de prueba.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](part-3/samples/sample1.cmd)]

Este comando agrega una carpeta denominada migraciones a su proyecto, además de un archivo de código denominado Configuration.cs en la carpeta de migraciones.

![](part-3/_static/image1.png)

Abra el archivo Configuration.cs. Agregue el siguiente **mediante** instrucción.

[!code-csharp[Main](part-3/samples/sample2.cs)]

A continuación, agregue el código siguiente a la **Configuration.Seed** método:

[!code-csharp[Main](part-3/samples/sample3.cs)]

En la ventana de consola de administrador de paquetes, escriba los siguientes comandos:

[!code-console[Main](part-3/samples/sample4.cmd)]

El primer comando genera código que crea la base de datos y el segundo comando ejecuta ese código. La base de datos se crea localmente, mediante [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorar la API (opcional)

Presione F5 para ejecutar la aplicación en modo de depuración. Visual Studio inicia IIS Express y ejecuta la aplicación web. A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación.

Cuando Visual Studio se ejecuta un proyecto web, asigna a un número de puerto. En la imagen siguiente, el número de puerto es 50524. Al ejecutar la aplicación, verá un número de puerto diferente.

![](part-3/_static/image3.png)

La página principal se implementa mediante ASP.NET MVC. En la parte superior de la página, hay un vínculo que dice "API". Este vínculo lleva a una página de ayuda generada automáticamente para la API web. (Para obtener información sobre cómo se genera esta página de ayuda, y cómo puede agregar su propia documentación a la página, vea [crear páginas de ayuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Puede hacer clic en la Ayuda de vínculos de página para ver detalles acerca de la API, incluido el formato de solicitud y respuesta.

![](part-3/_static/image4.png)

La API permite operaciones CRUD en la base de datos. A continuación resume la API.

| Authors |  |
| --- | -- |
| OBTENER api/authors | Obtener a todos los autores. |
| GET api/authors / {id} | Obtener a un autor por Id. |
| Autores/api/POST | Crear a un nuevo autor. |
| PUT/API/authors / {id} | Actualizar a un autor existente. |
| Eliminar/API/authors / {id} | Eliminar a un autor. |

| Libros |  |
| --- | -- |
| OBTENER /api/books | Obtener todos los libros. |
| OBTENER/API/libros / {id} | Obtenga un libro por identificador. |
| PUBLICAR libros / api / | Cree un nuevo libro. |
| PUT/API/libros / {id} | Actualizar un libro existente. |
| Eliminar/API/libros / {id} | Eliminar un libro. |

## <a name="view-the-database-optional"></a>Vista de la base de datos (opcional)

Cuando ejecutó el comando Update-Database, EF crea la base de datos y se llama el `Seed` método. Cuando se ejecuta la aplicación localmente, EF usa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Puede ver la base de datos en Visual Studio. Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server**.

![](part-3/_static/image5.png)

En el **conectar al servidor** cuadro de diálogo, en el **nombre del servidor** cuadro de edición, escriba "(localdb) \v11.0". Deje el **autenticación** opción como "Autenticación de Windows". Haga clic en **Conectar**.

![](part-3/_static/image6.png)

Visual Studio se conecta a LocalDB y muestra las bases de datos existentes en la ventana del explorador de objetos de SQL Server. Puede expandir los nodos para ver las tablas que EF creado.

![](part-3/_static/image7.png)

Para ver los datos, haga clic en una tabla y seleccione **ver datos**.

![](part-3/_static/image8.png)

Captura de pantalla siguiente muestra los resultados de la tabla de los libros en pantalla. Tenga en cuenta que EF rellena la base de datos con los datos de inicialización y la tabla contiene la clave externa a la tabla Authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Anterior](part-2.md)
> [Siguiente](part-4.md)
