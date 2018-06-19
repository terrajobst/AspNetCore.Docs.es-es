---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creación de los principales página | Documentos de Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30869873"
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Creación de los principales página
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Creación de los principales página

En esta sección, creará la página de aplicación principal. Esta página será más compleja que la página de administración, por lo que se alcanzará en varios pasos. Al hacerlo, podrá ver algunas técnicas más avanzadas de Knockout.js. Este es el diseño básico de la página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Productos" contiene una matriz de productos.
- "Cart" contiene una matriz de productos con cantidades. Al hacer clic en "Add to Cart", se actualiza el carro.
- "Orders" contiene una matriz de identificadores de pedido.
- "Detalles" contiene un detalle de pedido, que es una matriz de elementos (productos con cantidades)

Comenzaremos definiendo algunos diseño básico en formato HTML, con ningún enlace de datos o la secuencia de comandos. Abra el archivo Views/Home/Index.cshtml y reemplace todo el contenido con lo siguiente:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

A continuación, agregue una sección de secuencias de comandos y crear un modelo vacío de la vista:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Según el diseño que se elabora anteriormente, nuestro modelo de vista debe observables para los productos, carro, pedidos y detalles. Agregue las siguientes variables a la `AppViewModel` objeto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Los usuarios pueden agregar elementos de la lista de productos en el carro de la y quitar elementos del carro. Para encapsular estas funciones, vamos a crear otra clase de modelo de vista que representa un producto. Agrega el código siguiente a `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

El `ProductViewModel` clase contiene dos funciones que se usan para mover el producto desde y hacia el carro: `addItemToCart` agrega una unidad del producto al carro de la y `removeAllFromCart` quita todas las cantidades del producto.

Los usuarios pueden seleccionar un pedido existente y obtener los detalles del pedido. Va a encapsular esta funcionalidad en otro modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

El `OrderDetailsViewModel` se inicializa con un pedido, y captura los detalles del pedido mediante el envío de una solicitud AJAX al servidor.

Además, tenga en cuenta el `total` propiedad en el `OrderDetailsViewModel`. Esta propiedad es un tipo especial de objeto observable que se llama a un [calcula observable](http://knockoutjs.com/documentation/computedObservables.html). Como su nombre indica, un objeto observable calculada le permite enlazar los datos con un valor calculado&#8212;en este caso, el coste total del pedido.

A continuación, agregue estas funciones para `AppViewModel`:

- `resetCart` Quita todos los elementos del carro.
- `getDetails` Obtiene los detalles de un pedido (por pusing un nuevo `OrderDetailsViewModel` en el `details` lista).
- `createOrder` crea un nuevo pedido y vacía el carro.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Por último, inicializar el modelo de vista realizando solicitudes de AJAX de los productos y pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Bien, que es una gran cantidad de código, pero se compiló seguridad paso a paso, espero que el diseño está desactivado. Ahora podemos agregar algunos enlaces Knockout.js en el código HTML.

**Productos**

Estos son los enlaces de la lista de productos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Se recorre en iteración la matriz de productos y muestra el nombre y el precio. El botón "Agregar a pedido" sólo es visible cuando el usuario ha iniciado sesión.

Las llamadas de botón "Agregar a pedido" `addItemToCart` en el `ProductViewModel` instancia para el producto. Esto muestra una interesante característica de Knockout.js: cuando un modelo de vista contiene otros modelos de vista, puede aplicar los enlaces para el modelo interno. En este ejemplo, los enlaces dentro de la `foreach` se aplican a cada uno de los `ProductViewModel` instancias. Este enfoque es mucho más limpio que colocar toda la funcionalidad en un único modelo de vista.

**Cart**

Estos son los enlaces para el carro:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Se recorre en iteración la matriz de carro y muestran el nombre, el precio y la cantidad. Tenga en cuenta que el vínculo "Quitar" y el botón "Crear pedido" se enlazan a las funciones de modelo de vista.

**Pedidos**

Estos son los enlaces de la lista de pedidos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Se recorre en iteración los pedidos y muestra el identificador de pedido. El evento de clic en el vínculo se enlaza a la `getDetails` (función).

**Detalles del pedido**

Estos son los enlaces para los detalles del pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Esto se recorre en iteración los elementos en el orden y muestra el producto, precio y cantidad. Div adyacente sólo está visible si la matriz de detalles contiene uno o varios elementos.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado una aplicación que utiliza Entity Framework para comunicarse con la base de datos y ASP.NET Web API para proporcionar una interfaz pública encima de la capa de datos. ASP.NET MVC 4 se usa para representar las páginas HTML y Knockout.js además de jQuery para proporcionar interacciones dinámicas sin recargas de página.

Recursos adicionales:

- [Mapa de contenido de acceso de datos de ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro para desarrolladores de Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
