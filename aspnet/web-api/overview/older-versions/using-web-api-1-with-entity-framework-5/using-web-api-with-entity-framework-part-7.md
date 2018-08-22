---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creación de los principales página | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829400"
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Creación de los principales página
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Creación de los principales página

En esta sección, creará la página principal de la aplicación. Esta página será más compleja que la página de administración, por lo que se alcanzará en varios pasos. En el camino, verá algunas técnicas más avanzadas de Knockout.js. Este es el diseño básico de la página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Productos" contiene una matriz de productos.
- "Cesta" contiene una matriz de productos con cantidades. Al hacer clic en "Add to Cart" actualiza el carro de compra.
- "Orders" contiene una matriz de identificadores de pedido.
- "Detalles" contiene un detalle de pedido, que es una matriz de elementos (productos con cantidades)

Comenzaremos por definir algunas diseño básico en HTML, con ningún enlace de datos o la secuencia de comandos. Abra el archivo Views/Home/Index.cshtml y reemplace todo el contenido por lo siguiente:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

A continuación, agregue una sección de Scripts y crear un modelo de vista vacío:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

En función del diseño esbozado anteriormente, nuestro modelo de vista debe observables para los productos, carro, pedidos y detalles. Agregue las siguientes variables para el `AppViewModel` objeto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Los usuarios pueden agregar elementos de la lista de productos en el carro de compra y quitar elementos del carro de la. Para encapsular estas funciones, vamos a crear otra clase de modelo de vista que representa un producto. Agrega el código siguiente a `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

El `ProductViewModel` clase contiene dos funciones que se usan para mover el producto hacia y desde el carro de compra: `addItemToCart` agrega una unidad del producto al carro de compra, y `removeAllFromCart` quita todas las cantidades del producto.

Los usuarios pueden seleccionar un pedido existente y obtener los detalles del pedido. Encapsularemos esta funcionalidad en otro modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

El `OrderDetailsViewModel` se inicializa con un pedido, y captura los detalles del pedido mediante el envío de una solicitud AJAX al servidor.

Además, tenga en cuenta la `total` propiedad en el `OrderDetailsViewModel`. Esta propiedad es un tipo especial de objeto observable que se llama a un [calcula observable](http://knockoutjs.com/documentation/computedObservables.html). Como el nombre implica, un objeto observable calculado permite enlazar datos a un valor calculado&#8212;en este caso, el costo total del pedido.

A continuación, agregue estas funciones para `AppViewModel`:

- `resetCart` Quita todos los elementos del carro de compra.
- `getDetails` Obtiene los detalles de un pedido (por pusing un nuevo `OrderDetailsViewModel` hasta la `details` lista).
- `createOrder` crea un nuevo pedido y vacía el carro de compra.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Por último, inicialice el modelo de vista realizando solicitudes AJAX para los productos y pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Bien, que es una gran cantidad de código, pero lo construimos seguridad paso a paso, por lo que espero que el diseño está desactivado. Ahora podemos agregar algunos enlaces de Knockout.js para el código HTML.

**Productos**

Estos son los enlaces de la lista de productos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Esto se recorre en iteración la matriz de productos y muestra el nombre y el precio. El botón "Agregar a pedido" sólo es visible cuando el usuario ha iniciado sesión.

Las llamadas del botón "Agregar a pedido" `addItemToCart` en el `ProductViewModel` instancia para el producto. Esto muestra una interesante característica de Knockout.js: cuando un modelo de vista contiene otros modelos de vista, puede aplicar los enlaces para el modelo interno. En este ejemplo, los enlaces dentro de la `foreach` se aplican a cada uno de los `ProductViewModel` instancias. Este enfoque es mucho más limpio que poner toda la funcionalidad en un único modelo de vista.

**Carro de compra**

Estos son los enlaces para el carro de compra:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Esto se recorre en iteración la matriz de carro y muestra el nombre, precio y cantidad. Tenga en cuenta que el vínculo "Remove" y el botón "Crear un pedido" están enlazados a las funciones del modelo de vista.

**Pedidos**

Estos son los enlaces de la lista de pedidos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Se recorre en iteración los pedidos y muestra el identificador de pedido. El evento click en el vínculo está enlazado a la `getDetails` función.

**Detalles del pedido**

Estos son los enlaces para los detalles del pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Esto se recorre en iteración los elementos en el orden y muestra el producto, precio y cantidad. La etiqueta div circundante sólo está visible si la matriz de detalles contiene uno o varios elementos.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado una aplicación que utiliza Entity Framework para comunicarse con la base de datos y ASP.NET Web API para proporcionar una interfaz pública encima de la capa de datos. ASP.NET MVC 4 se usa para representar las páginas HTML y Knockout.js además de jQuery para proporcionar interacciones dinámicas sin las recargas de página.

Recursos adicionales:

- [Mapa de contenido de acceso de datos de ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro para desarrolladores de Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
