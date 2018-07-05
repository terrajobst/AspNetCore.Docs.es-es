---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usar $select, $expand y $value en ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f7e70bd76668f2af9215d57ee1cc9e7d41948c67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384769"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Usar $select, $expand y $value en ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2 agrega compatibilidad para expandir el $, $select y las opciones de $value de OData. Estas opciones permiten que un cliente controlar la representación que recibe desde el servidor.

- **$expand** hace que las entidades relacionadas ser incluido en línea en la respuesta.
- **$select** selecciona un subconjunto de propiedades para incluir en la respuesta.
- **$value** Obtiene el valor de una propiedad sin formato.

## <a name="example-schema"></a>Esquema de ejemplo

En este artículo, usaré un servicio de OData que define tres entidades: categoría, proveedor y producto. Cada producto tiene una categoría y un proveedor.

![](using-select-expand-and-value/_static/image1.png)

Estas son las clases de C# que definen los modelos de entidad:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Tenga en cuenta que el `Product` clase define las propiedades de navegación para el `Supplier` y `Category`. La `Category` clase define una propiedad de navegación para los productos de cada categoría.

Para crear un extremo de OData para este esquema, usar el scaffolding de Visual Studio 2013, como se describe en [creación de un extremo de OData en ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Agregar controladores independientes para el proveedor, categoría y producto.

## <a name="enabling-expand-and-select"></a>Habilitación de $expanda y $select

En Visual Studio 2013, el scaffolding de Web API OData crea un controlador que admite automáticamente la $expand y $select. Como referencia, estos son los requisitos para admitir $expanda y $select en un controlador.

Para de las colecciones, el controlador `Get` método debe devolver un **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Para entidades únicas, devolver un **SingleResult&lt;T&gt;**, donde T es un **IQueryable** que contiene cero entidades o una entidad.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Además, decore el `Get` métodos con el **[Queryable]** de atributo, como se muestra en los fragmentos de código anterior. Como alternativa, llame a **EnableQuerySupport** en el **HttpConfiguration** objeto en el inicio. (Para obtener más información, consulte [habilitar opciones de consulta de OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>$Expanda

Al consultar una entidad de OData o una colección, la respuesta predeterminada no incluye las entidades relacionadas. Por ejemplo, aquí está la respuesta predeterminada para el conjunto de entidades de categorías:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Como puede ver, la respuesta no incluye ningún producto, aunque la entidad Category tiene un vínculo de navegación de productos. Sin embargo, el cliente puede utilizar $expanda esta opción para obtener la lista de productos para cada categoría. Opción de expansión de los $ se incluye en la cadena de consulta de la solicitud:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Ahora el servidor incluirá los productos para cada categoría, en línea con las categorías. Esta es la carga de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Tenga en cuenta que cada entrada de la matriz de "value" contiene una lista de productos.

El $expandir la lista de opciones toma separados por comas de las propiedades de navegación para expandir. La solicitud siguiente amplía la categoría y el proveedor de un producto.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Puede expandir más de un nivel de propiedad de navegación. El ejemplo siguiente incluye todos los productos para una categoría y también el proveedor de cada producto.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

De forma predeterminada, Web API limita la profundidad de expansión máxima a 2. Que evita que el cliente envíe solicitudes complejas, como `$expand=Orders/OrderDetails/Product/Supplier/Region`, lo que podría resultar ineficaz para consultar y crear las respuestas de gran tamaño. Para reemplazar el valor predeterminado, establezca el **MaxExpansionDepth** propiedad en el **[Queryable]** atributo.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Para obtener más información acerca de los $ opción $expand, consulte [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) en la documentación oficial de OData.

## <a name="using-select"></a>Usar $select

La opción $select especifica un subconjunto de propiedades para incluir en el cuerpo de respuesta. Por ejemplo, para obtener solamente el nombre y el precio de cada producto, use la siguiente consulta:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Puede combinar $select y $expand en la misma consulta. Asegúrese de incluir la propiedad expandida en la opción $select. Por ejemplo, la siguiente solicitud obtiene el nombre de producto y el proveedor.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

También puede seleccionar las propiedades dentro de una propiedad expandida. La solicitud siguiente amplía los productos y selecciona el nombre de la categoría junto con el nombre de producto.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Para obtener más información acerca de la opción $select, consulte [seleccione System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) en la documentación oficial de OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Introducción a las propiedades individuales de una entidad ($value)

Hay dos formas para que un cliente de OData obtener una propiedad individual de una entidad. El cliente puede obtener el valor en formato de OData u obtener el valor sin formato de la propiedad.

La siguiente solicitud obtiene una propiedad en formato de OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Este es un ejemplo de respuesta en formato JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Para obtener el valor de la propiedad sin formato, anexe $value al URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Esta es la respuesta. Tenga en cuenta que el tipo de contenido "text/plain", no es JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Para admitir estas consultas en el controlador de OData, agregue un método denominado `GetProperty`, donde `Property` es el nombre de la propiedad. Por ejemplo, el método para obtener la propiedad Name se denominará `GetName`. El método debe devolver el valor de esa propiedad:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
