---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar migraciones de Code First para inicializar la base de datos | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Usar migraciones de Code First para inicializar la base de datos
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, va a usar [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) en EF para inicializar la base de datos con datos de prueba.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](part-3/samples/sample1.cmd)]

Este comando agrega una carpeta denominada migraciones a su proyecto, además de un archivo de código con el nombre de archivo Configuration.cs que en la carpeta Migrations.

![](part-3/_static/image1.png)

Abra el archivo del archivo Configuration.cs que. Agregue el siguiente **con** instrucción.

[!code-csharp[Main](part-3/samples/sample2.cs)]

A continuación, agregue el código siguiente a la **Configuration.Seed** método:

[!code-csharp[Main](part-3/samples/sample3.cs)]

En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos:

[!code-console[Main](part-3/samples/sample4.cmd)]

El primer comando genera código que crea la base de datos, y el segundo comando ejecuta ese código. La base de datos se crea de forma local, mediante [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorar la API (opcional)

Presione F5 para ejecutar la aplicación en modo de depuración. Visual Studio inicia IIS Express y ejecuta la aplicación web. Visual Studio, a continuación, inicia un explorador y abre la página principal de la aplicación.

Cuando Visual Studio se ejecuta un proyecto web, le asigna a un número de puerto. En la imagen siguiente, el número de puerto es 50524. Al ejecutar la aplicación, verá un número de puerto diferente.

![](part-3/_static/image3.png)

La página de inicio se implementa mediante ASP.NET MVC. En la parte superior de la página, hay un vínculo que dice "API". Este vínculo abre una página de ayuda generada automáticamente para la API web. (Para obtener información sobre cómo se genera esta página de ayuda, y cómo puede agregar su propia documentación a la página, vea [crear páginas de ayuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Puede hacer clic en la Ayuda de vínculos de página para ver detalles acerca de la API, incluido el formato de solicitud y respuesta.

![](part-3/_static/image4.png)

La API permite operaciones CRUD en la base de datos. A continuación resume la API.

| Authors |  |
| --- | -- |
| OBTENER api/autores | Obtener a todos los autores. |
| Api GET/autores / {id} | Obtener a un autor por identificador. |
| Autores/api/POST | Crear a un nuevo autor. |
| PUT /api/authors/{id} | Actualizar a un autor existente. |
| Eliminar/API/autores / {id} | Eliminar a un autor. |

| Libros |  |
| --- | -- |
| OBTENER /api/books | Obtener todos los libros. |
| GET /api/books/{id} | Obtener un libro por identificador. |
| REGISTRAR/api/libros | Crear un nuevo libro. |
| PUT /api/books/{id} | Actualizar un libro existente. |
| Eliminar/API/libros / {id} | Eliminar un libro. |

## <a name="view-the-database-optional"></a>Vista de la base de datos (opcional)

Cuando ejecutó el comando Update-Database, EF crea la base de datos y se llama el `Seed` método. Cuando se ejecuta la aplicación localmente, usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Puede ver la base de datos en Visual Studio. Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server**.

![](part-3/_static/image5.png)

En el **conectar al servidor** cuadro de diálogo, en la **nombre del servidor** cuadro de edición, escriba "(localdb) \v11.0". Deje el **autenticación** opción como "Autenticación de Windows". Haga clic en **conectar**.

![](part-3/_static/image6.png)

Visual Studio se conecta a LocalDB y muestra las bases de datos existentes en la ventana del explorador de objetos de SQL Server. Puede expandir los nodos para ver las tablas que EF creado.

![](part-3/_static/image7.png)

Para ver los datos, haga clic en una tabla y seleccione **ver datos**.

![](part-3/_static/image8.png)

Captura de pantalla siguiente muestra los resultados de la tabla de libros. Tenga en cuenta que EF rellena la base de datos con los datos de inicialización y la tabla contiene la clave externa a la tabla Authors.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Anterior](part-2.md)
[Siguiente](part-4.md)
