---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Control de relaciones de entidad | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-entity-relations"></a>Relaciones de entidad de control
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección se describe algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circular en las clases de modelo. (Esta sección ofrece información de segundo plano y no es necesario para completar el tutorial. Si lo prefiere, vaya a [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Carga frente a la carga diferida diligente

Al usar EF con una base de datos relacional, es importante entender la forma en que EF carga los datos relacionados.

También es útil ver las consultas SQL que genera EF. Para realizar el seguimiento de SQL, agregue la siguiente línea de código para el `BookServiceContext` constructor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Si se envía una solicitud GET a /api/books, devuelve JSON similar al siguiente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Puede ver que la propiedad Author es null, incluso si el libro contiene un AuthorId válido. Eso es porque EF no está cargando las entidades relacionadas de autor. Esto confirma que el registro de seguimiento de la consulta SQL:

[!code-console[Main](part-4/samples/sample3.sql)]

La instrucción SELECT toma de la tabla de libros y no hace referencia a la tabla de autor.

Como referencia, éste es el método en el `BooksController` clase que devuelve la lista de libros.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Veamos cómo podemos devolvemos al autor como parte de los datos JSON. Hay tres maneras de cargar los datos relacionados en Entity Framework: carga diligente, la carga diferida y carga explícita. Hay ventajas y desventajas con cada una de ellas, por lo que es importante comprender cómo funcionan.

### <a name="eager-loading"></a>Carga diligente

Con *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial. Para realizar la carga diligente, utilice la **System.Data.Entity.Include** método de extensión.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Esto indica a EF para incluir los datos de autor en la consulta. Si realiza este cambio y ejecutar la aplicación, ahora los datos JSON tendrá este aspecto:

[!code-console[Main](part-4/samples/sample6.cmd)]

El registro de seguimiento muestra que EF realiza una combinación en las tablas del libro y el autor.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carga diferida

Con la carga diferida, EF carga automáticamente una entidad relacionada cuando se deshace la referencia de la propiedad de navegación para esa entidad. Para habilitar la carga diferida, convierta la propiedad de navegación virtual. Por ejemplo, en la clase de libro:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Ahora, considere el siguiente código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Cuando está habilitada la carga diferida, obteniendo acceso a la `Author` propiedad `books[0]` hace EF consultar la base de datos para el autor.

Carga diferida requiere varios viajes de base de datos, porque EF envía una consulta cada vez que recupera una entidad relacionada. En general, es conveniente deshabilitada para los objetos que se serializa la carga diferida. El serializador tiene que leer todas las propiedades en el modelo, lo que desencadena la carga de las entidades relacionadas. Por ejemplo, cuando estas son las consultas SQL EF serializa la lista de libros con carga diferida habilitada. Puede ver que EF hace tres consultas independientes para los autores de tres.

[!code-console[Main](part-4/samples/sample10.sql)]

Hay veces cuando desea usar la carga diferida. Carga diligente puede provocar EF generar una combinación muy compleja. O podría necesitar las entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería mucho más eficaz.

Es una manera de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad. Voy a explicar este enfoque más adelante en el artículo.

### <a name="explicit-loading"></a>Carga explícita

Carga explícita es similar a la carga diferida, salvo que obtener explícitamente los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Carga explícita ofrece un mayor control sobre cuándo se debe cargar los datos relacionados, pero requiere código adicional. Para obtener más información acerca de la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propiedades de navegación y las referencias circulares

Cuando definen los modelos del libro y el autor, definir una propiedad de navegación en la `Book` clase para la relación del autor del libro, pero no ha definido una propiedad de navegación en la otra dirección.

¿Qué ocurre si agrega la propiedad de navegación correspondiente a la `Author` clase?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Por desgracia, surge un problema al serializar los modelos. Si carga los datos relacionados, crea un gráfico de objetos circulares.

![](part-4/_static/image1.png)

Cuando el formateador XML o JSON intenta serializar el gráfico, producirá una excepción. Los dos formateadores producen mensajes de excepción diferentes. Este es un ejemplo para el formateador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Este es el formateador de XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Una solución consiste en usar dto, que describe en la sección siguiente. Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos de gráfico. Para obtener más información, consulte [control Circular referencias a objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Para este tutorial, no es necesario el `Author.Book` propiedad de navegación, por lo que puede dejar.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Siguiente](part-5.md)
