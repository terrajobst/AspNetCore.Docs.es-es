---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData v3 con Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Compatibilidad con las relaciones de entidad en OData v3 con Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por relaciones de entidad. Dado un producto, puede encontrar el proveedor. También puede crear o eliminar las relaciones. Por ejemplo, puede establecer el proveedor de un producto.
> 
> Este tutorial muestra cómo admitir estas operaciones en ASP.NET Web API. El tutorial se basa en el tutorial [crear un punto de conexión de OData v3 con API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2
> - Versión 3 de OData
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Agregar una entidad de proveedor

Primero se debe agregar un nuevo tipo de entidad a nuestra fuente de OData. Vamos a agregar un `Supplier` clase.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Esta clase utiliza una cadena para la clave de entidad. En la práctica, que podría ser menos común que usar una clave de tipo entero. Pero vale la pena ver cómo OData controla otros tipos de clave además de números enteros.

A continuación, vamos a crear una relación mediante la adición de un `Supplier` propiedad a la `Product` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Agregue un nuevo **DbSet** a la `ProductServiceContext` de la clase, lo que Entity Framework incluirá el `Supplier` tabla en la base de datos.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

En WebApiConfig.cs, agregar una entidad de "Proveedores" en el modelo EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propiedades de navegación

Para obtener el proveedor de un producto, el cliente envía una solicitud GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aquí "Proveedor" es una propiedad de navegación en la `Product` tipo. En este caso, `Supplier` hace referencia a un único elemento, pero una navegación propiedad también puede devolver una colección (relación de uno a varios o varios a varios).

Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

El *clave* parámetro es la clave del producto. El método devuelve la entidad relacionada & #8212 en este caso, un `Supplier` instancia. El nombre del método y el nombre de parámetro son importantes. En general, si la propiedad de navegación se denomina "X", debe agregar un método denominado "GetX". El método debe tomar un parámetro denominado "*clave*" que coincide con el tipo de datos de clave del elemento primario.

También es importante incluir la **[FromOdataUri]** de atributo en el *clave* parámetro. Este atributo indica a API Web para usar las reglas de sintaxis de OData cuando analiza la clave de URI de la solicitud.

## <a name="creating-and-deleting-links"></a>Crear y eliminar vínculos

OData admite la creación o eliminación de las relaciones entre dos entidades. En la terminología de OData, la relación es un "enlace". Cada vínculo tiene un URI con el formulario *entidad*/$links /*entidad*. Por ejemplo, el vínculo de producto al proveedor tiene este aspecto:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para crear un nuevo vínculo, el cliente envía una solicitud POST para el identificador URI del vínculo. El cuerpo de la solicitud es el URI de la entidad de destino. Por ejemplo, suponga que hay un proveedor con la clave "CTSO". Para crear un vínculo de "Product(1)" a "Supplier('CTSO')", el cliente envía una solicitud similar al siguiente:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para eliminar un vínculo, el cliente envía una solicitud de eliminación para el identificador URI del vínculo.

**Crear vínculos**

Para habilitar al cliente para crear vínculos de proveedor del producto, agregue el código siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Este método toma tres parámetros:

- *clave*: la clave para la entidad primaria (el producto)
- *navigationProperty*: el nombre de la propiedad de navegación. En este ejemplo, la propiedad de navegación solo es válido es "Proveedor".
- *vínculo*: el URI de OData de la entidad relacionada. Este valor se toma del cuerpo de la solicitud. Por ejemplo, podría ser los URI del vínculo "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con el Id. = 'CTSO'.

El método utiliza el vínculo para buscar el proveedor. Si se encuentra el proveedor de búsqueda de coincidencias, el método establece la `Product.Supplier` propiedad y guarda el resultado en la base de datos.

La parte más difícil es analizar el identificador URI del vínculo. Básicamente, se necesita simular el resultado de enviar una solicitud GET a ese URI. El método auxiliar siguiente muestra cómo hacerlo. El método se invoca el proceso de enrutamiento de Web API y vuelve una **ODataPath** instancia que representa la ruta de acceso de OData analizado. Para un identificador URI del vínculo, uno de los segmentos debe ser la clave de entidad. (Si no, el cliente envió un URI incorrecto.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Eliminar vínculos**

Para eliminar un vínculo, agregue el código siguiente a la `ProductsController` clase:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

En este ejemplo, la propiedad de navegación es una sola `Supplier` entidad. Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada. Por ejemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Esta solicitud quita el orden 1 de cliente 1. En este caso, el método DeleteLink tendrá la siguiente firma:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

El *relatedKey* parámetro proporciona la clave para la entidad relacionada. Por lo tanto en el `DeleteLink` método, buscar la entidad principal mediante la *clave* parámetro, buscar la entidad relacionada con el *relatedKey* parámetro y, después, quite la asociación. Dependiendo del modelo de datos, tendrá que implementar ambas versiones de `DeleteLink`. API Web llamará a la versión correcta según el URI de solicitud.
