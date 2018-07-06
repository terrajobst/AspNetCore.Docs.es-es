---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData v3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores. Uso de OData, los clientes pueden navegar a través de...'
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: b24e3ca4e3d39b424bec6bb408bb0f85825c6761
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810749"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Compatibilidad con las relaciones de entidad en OData v3 con Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por las relaciones de entidad. Dado un producto, puede encontrar el proveedor. También puede crear o eliminar las relaciones. Por ejemplo, puede establecer el proveedor de un producto.
> 
> Este tutorial muestra cómo admitir estas operaciones en ASP.NET Web API. El tutorial se basa en el tutorial [crear un punto de conexión de OData v3 con Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2
> - OData versión 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Agregar una entidad de proveedor

En primer lugar, necesitamos agregar un nuevo tipo de entidad a nuestra fuente de OData. Vamos a agregar un `Supplier` clase.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Esta clase utiliza una cadena para la clave de entidad. En la práctica, que podría ser menos frecuentes que usar una clave de tipo entero. Pero merece la pena viendo cómo OData controla otros tipos de clave además de enteros.

A continuación, vamos a crear una relación mediante la adición de un `Supplier` propiedad a la `Product` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Agregue un nuevo **DbSet** a la `ProductServiceContext` clase, para que Entity Framework incluirá el `Supplier` tabla en la base de datos.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

En WebApiConfig.cs, agregue una entidad "Proveedores" para el modelo EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propiedades de navegación

Para obtener el proveedor de un producto, el cliente envía una solicitud GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aquí "Proveedor" es una propiedad de navegación en el `Product` tipo. En este caso, `Supplier` hace referencia a un único elemento, pero una navegación propiedad también puede devolver una colección (relación de uno a varios o varios a varios).

Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

El *clave* parámetro es la clave del producto. El método devuelve la entidad relacionada & #8212 en este caso, un `Supplier` instancia. El nombre del método y el nombre de parámetro son importantes. En general, si la propiedad de navegación se denomina "X", deberá agregar un método denominado "GetX". El método debe tomar un parámetro denominado "*clave*" que coincida con el tipo de datos de clave del elemento primario.

También es importante incluir la **[FromOdataUri]** atributo en el *clave* parámetro. Este atributo indica a Web API para usar las reglas de sintaxis de OData cuando analiza la clave desde el URI de solicitud.

## <a name="creating-and-deleting-links"></a>Creación y eliminación de vínculos

OData admite la creación o eliminación de las relaciones entre dos entidades. En la terminología de OData, la relación es un "vínculo". Cada vínculo tiene un URI con el formulario *entidad*/$links /*entidad*. Por ejemplo, el vínculo del producto a proveedor tiene este aspecto:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para crear un nuevo vínculo, el cliente envía una solicitud POST al URI de vínculo. El cuerpo de la solicitud es el URI de la entidad de destino. Por ejemplo, suponga que hay un proveedor con la clave "CTSO". Para crear un vínculo de "Product(1)" a "Supplier('CTSO')", el cliente envía una solicitud similar al siguiente:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para eliminar un vínculo, el cliente envía una solicitud DELETE al URI de vínculo.

**Creación de vínculos**

Para habilitar un cliente crear vínculos de proveedor del producto, agregue el código siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Este método toma tres parámetros:

- *clave*: la clave para la entidad primaria (el producto)
- *navigationProperty*: el nombre de la propiedad de navegación. En este ejemplo, la propiedad de navegación solo es válido es "Proveedor".
- *vínculo*: el URI de OData de la entidad relacionada. Este valor se toma del cuerpo de la solicitud. Por ejemplo, el vínculo que URI podría ser "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con el Id. = 'CTSO'.

El método utiliza el vínculo para buscar el proveedor. Si se encuentra el proveedor de búsqueda de coincidencias, el método establece el `Product.Supplier` propiedad y guarda el resultado en la base de datos.

La parte más difícil es analizar el URI del vínculo. Básicamente, deberá simular el resultado de enviar una solicitud GET a ese URI. El método auxiliar siguiente muestra cómo hacerlo. El método invoca el proceso de enrutamiento de Web API y recibe un **ODataPath** instancia que representa la ruta de acceso de OData analizado. Para un URI de vínculo, uno de los segmentos debe ser la clave de entidad. (Si no, el cliente envió un URI incorrecto.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Eliminar vínculos**

Para eliminar un vínculo, agregue el código siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

En este ejemplo, la propiedad de navegación es una sola `Supplier` entidad. Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada. Por ejemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Esta solicitud quita el pedido 1 cliente 1. En este caso, el método DeleteLink tendrá la siguiente firma:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

El *relatedKey* parámetro proporciona la clave para la entidad relacionada. Por lo tanto en su `DeleteLink` método, buscar la entidad principal mediante el *clave* parámetro, busque la entidad relacionada por la *relatedKey* parámetro y, a continuación, quite la asociación. Dependiendo del modelo de datos, es posible que deba implementar ambas versiones de `DeleteLink`. API Web llamará a la versión correcta según el URI de solicitud.
