---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Controlar las relaciones de entidad | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 7759b828068d99f9975d56671e427ccf6e94aef6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400958"
---
<a name="handling-entity-relations"></a>Relaciones de entidad de control
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

Esta sección describen algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circular en las clases de modelo. (Esta sección proporciona los conocimientos y no es necesario para completar el tutorial. Si lo prefiere, vaya a [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Diligente frente a la carga diferida

Cuando se usa EF con una base de datos relacional, es importante comprender cómo EF carga los datos relacionados.

También es útil ver las consultas SQL que genera EF. Para realizar el seguimiento de SQL, agregue la siguiente línea de código para el `BookServiceContext` constructor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Si envía una solicitud GET a /api/books, devuelve JSON similar al siguiente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Puede ver que la propiedad Author es null, aunque el libro contiene un AuthorId válido. Eso es porque EF no carga las entidades relacionadas del autor. Esto confirma que el registro de seguimiento de la consulta SQL:

[!code-console[Main](part-4/samples/sample3.sql)]

La instrucción SELECT toma de la tabla de los libros en pantalla y no hace referencia a la tabla de autor.

Como referencia, este es el método en el `BooksController` clase que devuelve la lista de los libros en pantalla.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Veamos cómo podemos volver al autor como parte de los datos JSON. Hay tres formas de cargar datos relacionados en Entity Framework: la carga diligente, la carga diferida y la carga explícita. Existen ventajas y desventajas con cada una de ellas, por lo que es importante comprender cómo funcionan.

### <a name="eager-loading"></a>Carga diligente

Con *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial. Para realizar la carga diligente, utilice el **System.Data.Entity.Include** método de extensión.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Esto indica a EF para incluir los datos del autor de la consulta. Si realiza este cambio y ejecutar la aplicación, ahora los datos JSON tendrá este aspecto:

[!code-console[Main](part-4/samples/sample6.cmd)]

El registro de seguimiento muestra que EF realiza una combinación en las tablas del libro y autor.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carga diferida

Con la carga diferida, EF carga automáticamente una entidad relacionada cuando la propiedad de navegación para esa entidad se desreferencia. Para habilitar la carga diferida, que la propiedad de navegación virtual. Por ejemplo, en la clase Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Ahora, considere el siguiente código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Cuando se habilita la carga diferida, obtener acceso a la `Author` propiedad `books[0]` hace que EF consultar la base de datos para el autor.

La carga diferida requiere varios viajes de base de datos, dado que EF envía una consulta cada vez que recupera una entidad relacionada. Por lo general, desea deshabilitar para los objetos que se serializa la carga diferida. El serializador tiene que leer todas las propiedades en el modelo, que se desencadena al cargar las entidades relacionadas. Por ejemplo, presentamos las consultas SQL que EF serializa la lista de libros con la carga diferida habilitada. Puede ver que EF realiza tres consultas independientes para los autores de tres.

[!code-console[Main](part-4/samples/sample10.sql)]

Todavía hay veces cuando desea utilizar la carga diferida. Para hacer que la carga diligente EF generar una combinación muy compleja. O podría necesitar las entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería más eficaz.

Es una forma de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad. Este enfoque le mostraré más adelante en el artículo.

### <a name="explicit-loading"></a>Carga explícita

Carga explícita es similar a la carga diferida, salvo que obtener explícitamente los datos relacionados en el código; no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Carga explícita le ofrece más control sobre cuándo se debe cargar los datos relacionados, pero requiere código adicional. Para obtener más información acerca de la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Las propiedades de navegación y las referencias circulares

Cuando definen los modelos de libro y autor, definí una propiedad de navegación en el `Book` clase para la relación del autor del libro, pero no he definido una propiedad de navegación en la otra dirección.

¿Qué ocurre si agrega la propiedad de navegación correspondiente a la `Author` clase?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Lamentablemente, esto crea un problema al serializar los modelos. Si carga los datos relacionados, crea un gráfico circular de objeto.

![](part-4/_static/image1.png)

Cuando se intente el formateador JSON o XML serializar el gráfico, producirá una excepción. Los dos formateadores generar mensajes de excepción diferente. Este es un ejemplo para el formateador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Este es el formateador XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Una solución consiste en usar dto, que describe en la sección siguiente. Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos de gráfico. Para obtener más información, consulte [referencias a objetos de control Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Para este tutorial, no necesita la `Author.Book` propiedad de navegación, por lo que puede dejarlo.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Siguiente](part-5.md)
