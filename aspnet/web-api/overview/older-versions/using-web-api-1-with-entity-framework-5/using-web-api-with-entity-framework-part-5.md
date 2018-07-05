---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: Crear una IU dinámica con Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7d8bd4732ecf73c14251ceebfd667310f6e197b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801698"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: Crear una IU dinámica con Knockout.js
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Crear una IU dinámica con Knockout.js

En esta sección, vamos a usar Knockout.js para agregar funcionalidad a la vista de administración.

[Knockout.js](http://knockoutjs.com/) es una biblioteca de Javascript que facilita la tarea enlazar controles HTML a los datos. Knockout.js usa el patrón Model-View-ViewModel (MVVM).

- El *modelo* es la representación del lado servidor de los datos en el dominio de negocio (en nuestro caso, productos y pedidos).
- El *vista* es la capa de presentación (HTML).
- El *vista-modelo* es un objeto de Javascript que contiene los datos del modelo. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación de HTML. En su lugar, representa las características abstractas de la vista, como "una lista de elementos".

La vista está enlazada a datos al modelo de vista. Actualizaciones para el modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como los clics de botón y realiza operaciones en el modelo, como la creación de un pedido.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

En primer lugar, definiremos el modelo de vista. Después de eso, se enlazará el marcado HTML para el modelo de vista.

Agregue la siguiente sección de Razor para Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Puede agregar esta sección en cualquier lugar en el archivo. Cuando se representa la vista, la sección que aparece en la parte inferior de la página HTML, justo antes del cierre &lt;corporal&gt; etiqueta.

Todo el script de esta página se irá dentro de la etiqueta de secuencia de comandos indicada por el comentario:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

En primer lugar, defina una clase de modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** es un tipo especial de objeto de Knockout, llama a un *observable*. Desde el [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html): un objeto observable es un "objeto de JavaScript que puede notificar a los suscriptores sobre los cambios." Cuando se cambia el contenido de un objeto observable, la vista se actualiza automáticamente para que coincida con.

Para rellenar el `products` de matriz, realice una solicitud AJAX a la API web. Recuerde que se almacena el identificador URI base para la API en el contenedor de vista (consulte [4ª](using-web-api-with-entity-framework-part-4.md) del tutorial).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

A continuación, agregar funciones para el modelo de vista para crear, actualizar y eliminar productos. Estas funciones enviar las llamadas AJAX a la API web y usar los resultados para actualizar el modelo de vista.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Ahora la parte más importante: cuando el DOM es fulled llamada cargado, el **ko.applyBindings** de función y pasar una nueva instancia de la `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

El **ko.applyBindings** método Knockout se activa y se conecta el modelo de vista a la vista.

Ahora que tenemos un modelo de vista, podemos crear los enlaces. En Knockout.js, hacerlo agregando `data-bind` atributos a elementos HTML. Por ejemplo, para enlazar una lista de HTML a una matriz, use el `foreach` enlace:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

El `foreach` enlace recorre en iteración la matriz y crea elementos para cada objeto de secundarios de la matriz. Los enlaces en los elementos secundarios pueden hacer referencia a las propiedades de los objetos de la matriz.

Agregue los siguientes enlaces a la lista de "productos de actualización":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

El `<li>` se produce un elemento dentro del ámbito de la **foreach** enlace. Que significa que tendrá Knockout representar el elemento una vez para cada producto en el `products` matriz. Todos los enlaces dentro de la `<li>` elemento hacen referencia a esa instancia del producto. Por ejemplo, `$data.Name` hace referencia a la `Name` propiedad sobre el producto.

Para establecer los valores de las entradas de texto, use la `value` enlace. Los botones están enlazados a las funciones en la vista de modelo, utilizando el `click` enlace. La instancia de un producto se pasa como un parámetro para cada función. Para obtener más información, el [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html) tiene buena descripciones de los distintos enlaces.

A continuación, agregue un enlace para el **enviar** eventos en el formulario Agregar producto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Llama este enlace el `create` función en el modelo de vista para crear un nuevo producto.

Este es el código completo de la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Ejecute la aplicación, inicie sesión con la cuenta de administrador y haga clic en el vínculo "Admin". Debería ver la lista de productos y ser capaz de crear, actualizar o eliminar los productos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-4.md)
> [Siguiente](using-web-api-with-entity-framework-part-6.md)
