---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creación del producto y los controladores de orden | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 08abc66624526f4b1931231d114158afe8b63247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382853"
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Creación de producto y los controladores de orden
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Agregar un controlador de productos

El controlador de administración es para los usuarios que tienen privilegios de administrador. Los clientes, por otro lado, pueden ver productos pero no se puede crear, actualizar o eliminarlos.

Fácilmente, podemos restringimos el acceso a los métodos Post, Put y Delete, dejando los métodos Get abierto. Pero veamos los datos que se devuelven para un producto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

El `ActualCost` propiedad no debe ser visible para los clientes. La solución consiste en definir un *objeto de transferencia de datos* (DTO) que incluye un subconjunto de las propiedades que deben ser visibles para los clientes. Se va a usar LINQ para proyecto `Product` instancias `ProductDTO` instancias.

Agregue una clase denominada `ProductDTO` a la carpeta Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Ahora, agregue el controlador. En el Explorador de soluciones, haga clic en la carpeta Controllers. Seleccione **agregar**, a continuación, seleccione **controlador**. En el **Agregar controlador** cuadro de diálogo, el nombre del controlador &quot;ProductsController&quot;. En **plantilla**, seleccione **controlador de API vacía**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Reemplace todo el contenido del archivo de origen con el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

El controlador sigue usando el `OrdersContext` para consultar la base de datos. Pero en lugar de devolver `Product` instancias directamente, llamamos a `MapProducts` para ellos en proyectar `ProductDTO` instancias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

El `MapProducts` método devuelve un **IQueryable**, por lo que nos podemos componer el resultado con otros parámetros de consulta. Puede ver esto en el `GetProduct` método, que agrega un **donde** cláusula a la consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Agregar un controlador de pedidos

A continuación, agregue un controlador que permite a los usuarios crear y ver los pedidos.

Comenzaremos con otro DTO. En el Explorador de soluciones, haga clic en la carpeta Models y agregue una clase denominada `OrderDTO` utilice la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Ahora, agregue el controlador. En el Explorador de soluciones, haga clic en la carpeta Controllers. Seleccione **agregar**, a continuación, seleccione **controlador**. En el **Agregar controlador** cuadro de diálogo, establezca las siguientes opciones:

- En **nombre del controlador**, escriba "OrdersController".
- En **plantilla**, seleccione "Controlador de API con acciones de lectura/escritura, usando Entity Framework".
- En **clase modelo**, seleccione &quot;orden (ProductStore.Models)&quot;.
- En **clase de contexto de datos**, seleccione &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Haga clic en **Agregar**. Esto agrega un archivo denominado OrdersController.cs. A continuación, se debe modificar la implementación predeterminada del controlador.

En primer lugar, elimine el `PutOrder` y `DeleteOrder` métodos. Para este ejemplo, los clientes no se pueden modificar ni eliminar pedidos existentes. En una aplicación real, necesitaría una gran cantidad de lógica de back-end para controlar estos casos. (Por ejemplo, se ha enviado el pedido ya?)

Cambiar el `GetOrders` método para devolver sólo los pedidos que pertenecen al usuario:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Cambiar el `GetOrder` método como sigue:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Estos son los cambios que realizamos en el método:

- El valor devuelto es un `OrderDTO` instancia, en lugar de un `Order`.
- Cuando se consulta la base de datos para el pedido, usamos el [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) método capturar relacionado `OrderDetail` y `Product` entidades.
- El resultado se aplana con una proyección.

La respuesta HTTP contendrá una matriz de productos con cantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Este formato es más fácil para los clientes consuman que el gráfico de objeto original, que contiene entidades anidadas (orden, detalles y productos).

El último método puede considerar `PostOrder`. En este momento, este método toma un `Order` instancia. Pero tenga en cuenta qué sucede si un cliente envía un cuerpo de solicitud similar al siguiente:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Se trata de un orden bien estructurado y Entity Framework, la insertará en la base de datos. Pero contiene una entidad de producto que no existía anteriormente. El cliente que acaba de crear un nuevo producto en nuestra base de datos. Esto será una sorpresa al departamento de asesoría de orden, cuando vean un pedido para osos koala. La moraleja es, tenga cuidado realmente los datos que acepte en una solicitud POST o PUT.

Para evitar este problema, cambie el `PostOrder` método tomar un `OrderDTO` instancia. Use la `OrderDTO` para crear el `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Tenga en cuenta que utilizamos la `ProductID` y `Quantity` propiedades y se omiten los valores que se envía al cliente para el nombre de producto o precio. Si el identificador de producto no es válido, infringirán la restricción foreign key en la base de datos y se producirá un error la inserción, como debería.

Aquí es el conjunto completo `PostOrder` método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Por último, agregue el **Authorize** atributo al controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Ahora sólo los usuarios registrados pueden crear o ver los pedidos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-5.md)
> [Siguiente](using-web-api-with-entity-framework-part-7.md)
