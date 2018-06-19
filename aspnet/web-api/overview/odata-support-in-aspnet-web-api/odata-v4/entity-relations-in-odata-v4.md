---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Las relaciones de entidad en OData v4 mediante ASP.NET Web API 2.2 | Documentos de Microsoft
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508104"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relaciones de entidad en OData v4 mediante ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por relaciones de entidad. Dado un producto, puede encontrar el proveedor. También puede crear o eliminar las relaciones. Por ejemplo, puede establecer el proveedor de un producto.
> 
> Este tutorial muestra cómo admitir estas operaciones en OData v4 mediante ASP.NET Web API. El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2.1
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para la versión 3 de OData, vea [admiten relaciones de entidad en OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Agregar una entidad de proveedor

> [!NOTE]
> El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


En primer lugar, se necesita una entidad relacionada. Agregue una clase denominada `Supplier` en la carpeta de modelos.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Agregar una propiedad de navegación a la `Product` clase:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Agregue un nuevo **DbSet** a la `ProductsContext` de la clase, lo que Entity Framework incluirá la tabla de proveedor en la base de datos.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

En WebApiConfig.cs, agregue un &quot;proveedores&quot; del conjunto de entidades en el entity data model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Agregar un controlador de proveedores

Agregar un `SuppliersController` clase a la carpeta de controladores.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

No se muestra cómo agregar operaciones CRUD para este controlador. Los pasos son los mismos que el controlador de productos (consulte [crear un punto de conexión de OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtener entidades relacionadas

Para obtener el proveedor de un producto, el cliente envía una solicitud GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Este método usa una convención de nomenclatura predeterminado

- Nombre del método: GetX, donde X es la propiedad de navegación.
- Nombre del parámetro: *clave*

Si sigue esta convención de nomenclatura, Web API asigna automáticamente la solicitud HTTP para el método del controlador.

Solicitud de ejemplo HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Respuesta de ejemplo HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtener una colección relacionados

En el ejemplo anterior, un producto tiene un proveedor. Una propiedad de navegación también puede devolver una colección. El código siguiente obtiene los productos de un proveedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

En este caso, el método devuelve un **IQueryable** en lugar de un **SingleResult&lt;T&gt;**

Solicitud de ejemplo HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Respuesta de ejemplo HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Crear una relación entre entidades

OData admite la creación o eliminación de relaciones entre dos entidades existentes. En la terminología de OData v4, la relación es una &quot;referencia&quot;. (En OData v3, la relación se denomina un *vínculo*. Las diferencias de protocolo no importan para este tutorial.)

Una referencia tiene su propio URI, con el formato `/Entity/NavigationProperty/$ref`. Por ejemplo, este es el URI para resolver la referencia entre un producto y su proveedor:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Para agregar una relación, el cliente envía una solicitud POST o PUT a esta dirección.

- Si la propiedad de navegación es una entidad única, como `Product.Supplier`.
- Muestra si la propiedad de navegación es una colección, como `Supplier.Products`.

El cuerpo de la solicitud contiene el URI de la otra entidad en la relación. Esta es una solicitud de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

En este ejemplo, el cliente envía una solicitud PUT para `/Products(6)/Supplier/$ref`, que es el URI de $ref para el `Supplier` del producto con el Id. = 6. Si la solicitud se realiza correctamente, el servidor envía una respuesta 204 (sin contenido):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Éste es el método de controlador para agregar una relación con un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

El *navigationProperty* parámetro especifica qué relación establezca. (Si hay más de una propiedad de navegación en la entidad, puede agregar más `case` instrucciones.)

El *vínculo* parámetro contiene el URI del proveedor. API Web analiza automáticamente el cuerpo de la solicitud para obtener el valor de este parámetro.

Para buscar el proveedor, es necesario el identificador (o clave), que forma parte de la *vínculo* parámetro. Para ello, utilice el siguiente método auxiliar:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Básicamente, este método usa la biblioteca de OData para dividir la ruta de acceso URI en segmentos, busque el segmento que contiene la clave y convertir la clave en el tipo correcto.

## <a name="deleting-a-relationship-between-entities"></a>Eliminar una relación entre entidades

Para eliminar una relación, el cliente envía una solicitud HTTP DELETE al URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Éste es el método de controlador para eliminar la relación entre un producto y un proveedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

En este caso, `Product.Supplier` es el &quot;1&quot; final de una relación de 1 a muchos, por lo que puede eliminar la relación simplemente estableciendo `Product.Supplier` a `null`.

En el &quot;muchos&quot; final de una relación, el cliente debe especificar que relacionados con la entidad que se va a quitar. Para ello, el cliente envía el URI de la entidad relacionada en la cadena de consulta de la solicitud. Por ejemplo, para quitar "Producto 1" de "proveedor 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Para admitir esto en API Web, es necesario incluir un parámetro adicional en el `DeleteRef` método. Este es el método de controlador para quitar un producto de la `Supplier.Products` relación.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

El *clave* parámetro es la clave para el proveedor y el *relatedKey* parámetro es la clave para el producto que desea quitar de la `Products` relación. Tenga en cuenta que API Web obtiene automáticamente la clave de la cadena de consulta.
