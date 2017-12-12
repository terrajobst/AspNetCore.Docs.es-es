---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Parte 6: Creación de producto y controladores de orden | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Creación de producto y controladores de orden
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Agregar un controlador de productos

El controlador de administración es para los usuarios que tienen privilegios de administrador. Los clientes, por otro lado, pueden ver productos, pero no se puede crear, actualizar o eliminarlos.

Se podemos restringir fácilmente el acceso a los métodos Post, Put y Delete, mientras que los métodos Get se deja abierta. Pero veamos los datos que se devuelven para un producto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

El `ActualCost` propiedad no debe ser visible para otros clientes. La solución consiste en definir un *objeto de transferencia de datos* (DTO) que incluye un subconjunto de propiedades que deben estar visibles para los clientes. Se va a usar LINQ al proyecto `Product` instancias de `ProductDTO` instancias.

Agregue una clase denominada `ProductDTO` en la carpeta Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Ahora agregue el controlador. En el Explorador de soluciones, haga clic en la carpeta de controladores. Seleccione **agregar**, a continuación, seleccione **controlador**. En el **Agregar controlador** cuadro de diálogo, el nombre del controlador &quot;ProductsController&quot;. En **plantilla**, seleccione **controlador API vacía**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Reemplace todo el contenido del archivo de origen con el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

El controlador sigue usando el `OrdersContext` para consultar la base de datos. Pero en lugar de devolver `Product` instancias directamente, llamamos a `MapProducts` a ellos en proyectos `ProductDTO` instancias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

El `MapProducts` método devuelve un **IQueryable**, por lo que se puede crear el resultado con otros parámetros de consulta. Puede verlo en el `GetProduct` método, que agrega un **donde** cláusula a la consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Agregar un controlador de pedidos

A continuación, agregue un controlador que permite a los usuarios crear y ver los pedidos.

Comenzaremos con otro DTO. En el Explorador de soluciones, haga clic en la carpeta Models y agregue una clase denominada `OrderDTO` usar la implementación siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Ahora agregue el controlador. En el Explorador de soluciones, haga clic en la carpeta de controladores. Seleccione **agregar**, a continuación, seleccione **controlador**. En el **Agregar controlador** cuadro de diálogo, establezca las siguientes opciones:

- En **nombre del controlador**, escriba "OrdersController".
- En **plantilla**, seleccione "Controlador de API con acciones de lectura/escritura, mediante Entity Framework".
- En **clase modelo**, seleccione &quot;orden (ProductStore.Models)&quot;.
- En **clase de contexto de datos**, seleccione &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Haga clic en **Agregar**. Esto agrega un archivo denominado OrdersController.cs. A continuación, es necesario modificar la implementación predeterminada del controlador.

En primer lugar, elimine la `PutOrder` y `DeleteOrder` métodos. En este ejemplo, los clientes no se pueden modificar ni eliminar pedidos existentes. En una aplicación real, necesitaría una gran cantidad de lógica de back-end para controlar estos casos. (Por ejemplo, se envía el pedido ya?)

Cambiar el `GetOrders` método para devolver sólo los pedidos que pertenecen al usuario:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Cambiar el `GetOrder` método tal como se indica a continuación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Estos son los cambios que hemos realizado al método:

- El valor devuelto es un `OrderDTO` instancia, en lugar de un `Order`.
- Cuando se consulta la base de datos para el pedido, usamos el [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) método capturar relacionado `OrderDetail` y `Product` entidades.
- El resultado se recopila mediante el uso de una proyección.

La respuesta HTTP contiene una matriz de productos con cantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Este formato es más fácil para los clientes consumir que el gráfico de objetos original, que contiene entidades anidadas (orden, los detalles y productos).

El último método para tener en cuenta `PostOrder`. En este momento, este método toma un `Order` instancia. Pero tenga en cuenta lo que sucede si un cliente envía un cuerpo de solicitud similar al siguiente:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Se trata de un orden bien estructurado y Entity Framework, inserta, en la base de datos. Pero contiene una entidad de producto que no existía anteriormente. El cliente que acaba de crear un nuevo producto en nuestra base de datos. Se trata una sorpresa al departamento de fullfilment orden, cuando vean un pedido para osos koala. Es la moraleja, tenga cuidado realmente los datos que se aceptan en una solicitud POST o PUT.

Para evitar este problema, cambie la `PostOrder` método tomar un `OrderDTO` instancia. Use la `OrderDTO` para crear la `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Observe que utilizamos la `ProductID` y `Quantity` propiedades y se omiten los valores que el cliente envía para su nombre de producto o precio. Si el identificador de producto no es válido, infringirán la restricción foreign key en la base de datos y se producirá un error la inserción, como debería.

Aquí es el conjunto completo `PostOrder` método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Por último, agregue el **Authorize** atributo al controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Ahora sólo los usuarios registrados pueden crear o ver los pedidos.

>[!div class="step-by-step"]
[Anterior](using-web-api-with-entity-framework-part-5.md)
[Siguiente](using-web-api-with-entity-framework-part-7.md)
