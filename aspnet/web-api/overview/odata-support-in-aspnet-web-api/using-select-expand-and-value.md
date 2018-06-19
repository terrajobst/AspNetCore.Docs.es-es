---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Con $select, $expand y $value en ASP.NET Web API 2 OData | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508064"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Con $select, $expand y $value en ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2 agrega compatibilidad para expandir $expand, $select y las opciones de $value de OData. Estas opciones permiten un cliente controlar la representación que se recibe desde el servidor.

- **$expand** hace que las entidades relacionadas pueden insertar en la respuesta.
- **$select** selecciona un subconjunto de propiedades para incluir en la respuesta.
- **$value** Obtiene el valor sin formato de una propiedad.

## <a name="example-schema"></a>Esquema de ejemplo

En este artículo, utilizará un servicio de OData que define tres entidades: categoría, proveedor y producto. Cada producto tiene una categoría y un proveedor.

![](using-select-expand-and-value/_static/image1.png)

Estas son las clases de C# que definen los modelos de entidad:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Tenga en cuenta que la `Product` clase define propiedades de navegación para la `Supplier` y `Category`. La `Category` clase define una propiedad de navegación para los productos en cada categoría.

Para crear un extremo de OData para este esquema, usar el scaffolding de Visual Studio 2013, como se describe en [creación de un extremo de OData en ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Agregar controladores independientes para los productos, categorías y proveedores.

## <a name="enabling-expand-and-select"></a>Habilitar $expanda y $select

En Visual Studio 2013, el scaffolding de Web API OData crea un controlador que automáticamente admite $expand y $select. Como referencia, estos son los requisitos para admitir $expanda y $select en un controlador.

Para de las colecciones, el controlador `Get` método debe devolver un **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Para entidades únicas, devolver un **SingleResult&lt;T&gt;**, donde T es un **IQueryable** que contiene cero entidades o una entidad.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Además, decorar la `Get` métodos con el **[Queryable]** atributo, como se muestra en los fragmentos de código anterior. O bien, llamar a **EnableQuerySupport** en el **HttpConfiguration** objeto en el inicio. (Para obtener más información, consulte [habilitar opciones de consulta de OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>$Expanda

Al consultar una entidad OData o una colección, la respuesta predeterminada no incluye las entidades relacionadas. Por ejemplo, aquí está la respuesta predeterminada para el conjunto de entidades de categorías:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Como puede ver, la respuesta no incluye los productos, aunque la entidad Category tiene un vínculo de navegación de productos. Sin embargo, el cliente puede utilizar $ se expanden para obtener la lista de productos de cada categoría. La $expand opción va en la cadena de consulta de la solicitud:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Ahora el servidor incluirá los productos para cada categoría, en línea con las categorías. Esta es la carga de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Tenga en cuenta que cada entrada de la matriz de "value" contiene una lista de productos.

La $expand lista toma separados por comas de opciones de propiedades de navegación para expandir. La siguiente solicitud expande la categoría y el proveedor de un producto.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Puede expandir más de un nivel de propiedad de navegación. En el ejemplo siguiente se incluye todos los productos para una categoría así como el proveedor de cada producto.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

De forma predeterminada, Web API limita la profundidad de expansión máxima a 2. Que impide que el cliente envía solicitudes complejas como `$expand=Orders/OrderDetails/Product/Supplier/Region`, que podría ser ineficaz para consultar y crear respuestas de gran tamaño. Para reemplazar el valor predeterminado, establezca el **MaxExpansionDepth** propiedad en el **[Queryable]** atributo.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Para obtener más información sobre la $expandir opción, consulte [expanda System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) en la documentación oficial de OData.

## <a name="using-select"></a>Uso de $select

La opción $select especifica un subconjunto de propiedades para incluir en el cuerpo de respuesta. Por ejemplo, para obtener solo el nombre y el precio de cada producto, use la siguiente consulta:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Puede combinar $select y $expand en la misma consulta. Asegúrese de incluir la propiedad expandida en la opción $select. Por ejemplo, la siguiente solicitud obtiene el nombre de producto y el proveedor.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

También puede seleccionar las propiedades dentro de una propiedad expandida. La solicitud siguiente amplía productos y selecciona el nombre de categoría de producto.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Este es el cuerpo de respuesta:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Para obtener más información acerca de la opción $select, consulte [Seleccionar opción de consulta de sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) en la documentación oficial de OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Obtener las propiedades individuales de una entidad ($value)

Hay dos formas para que un cliente de OData y obtener una propiedad individual de una entidad. El cliente puede obtener el valor de formato de OData u obtener el valor sin formato de la propiedad.

La siguiente solicitud obtiene una propiedad en formato de OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Aquí es una respuesta de ejemplo en formato JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Para obtener el valor sin formato de la propiedad, anexe $value al URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Aquí está la respuesta. Tenga en cuenta que el tipo de contenido "text/plain", no es JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Para admitir estas consultas en el controlador de OData, agregue un método denominado `GetProperty`, donde `Property` es el nombre de la propiedad. Por ejemplo, el método para obtener la propiedad Name se denominará `GetName`. El método debe devolver el valor de esa propiedad:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
