---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: Crear una IU dinámica con Knockout.js | Documentos de Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873815"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: Crear una IU dinámica con Knockout.js
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Crear una IU dinámica con Knockout.js

En esta sección, vamos a usar Knockout.js para agregar funcionalidad a la vista de administración.

[Knockout.js](http://knockoutjs.com/) es una biblioteca de Javascript que facilita el proceso de enlazar controles HTML a los datos. Knockout.js utiliza el modelo Model-View-ViewModel (MVVM).

- El *modelo* es la representación del lado del servidor de los datos en el dominio de negocio (en nuestro caso, productos y pedidos).
- El *vista* es la capa de presentación (HTML).
- El *modelo de vista* es un objeto de Javascript que contiene los datos del modelo. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación de HTML. En su lugar, representa las características abstractas de la vista, como "una lista de elementos".

La vista está enlazado a datos para el modelo de vista. Actualizaciones para el modelo de vista se reflejan automáticamente en la vista. El modelo de vista también obtiene los eventos de la vista, como clics del botón y realiza operaciones en el modelo, como la creación de un pedido.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

En primer lugar, se definirá el modelo de vista. Después de eso, se enlazará el marcado HTML para el modelo de vista.

Agregue la siguiente sección de Razor para Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Puede agregar esta sección en cualquier lugar en el archivo. Cuando se representa la vista, la sección aparece en la parte inferior de la página HTML, haga antes del cierre &lt;/cuerpo&gt; etiqueta.

Todo el script de esta página irá dentro de la etiqueta de secuencia de comandos indicada por el comentario:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

En primer lugar, defina una clase de modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** es un tipo especial de objeto de cobertura, llama a un *observable*. Desde el [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html): observable es un "objeto de JavaScript que puede notificar a los suscriptores sobre los cambios." Cuando se cambia el contenido de un objeto observable, la vista se actualiza automáticamente para que coincida con.

Para rellenar el `products` de matriz, realizar una solicitud AJAX en la API web. Recuerde que se almacena el identificador URI base para la API en el contenedor de la vista (vea [parte 4](using-web-api-with-entity-framework-part-4.md) del tutorial).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

A continuación, agregar funciones para el modelo de vista para crear, actualizar y eliminar productos. Estas funciones enviar llamadas AJAX a la API web y utilizan los resultados para actualizar el modelo de vista.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Ahora la parte más importante: cuando el DOM es fulled llamada cargado, el **ko.applyBindings** función y pase una nueva instancia de la `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

El **ko.applyBindings** método activa Knockout y conecta el modelo de vista a la vista.

Ahora que tenemos un modelo de vista, podemos crear los enlaces. En Knockout.js, hacerlo agregando `data-bind` atributos a elementos HTML. Por ejemplo, para enlazar una lista de HTML en una matriz, use el `foreach` enlace:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

El `foreach` enlace recorre en iteración la matriz y secundarios se crean elementos para cada objeto en la matriz. Enlaces en los elementos secundarios pueden hacer referencia a las propiedades de los objetos de la matriz.

Agregue los siguientes enlaces a la lista de "productos de actualización":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

El `<li>` elemento se produce dentro del ámbito de la **foreach** enlace. Que significa will Knockout representar el elemento una vez para cada producto en la `products` matriz. Todos los enlaces dentro de la `<li>` elemento hacer referencia a esa instancia de un producto. Por ejemplo, `$data.Name` hace referencia a la `Name` propiedad en el producto.

Para establecer los valores de las entradas de texto, utilice la `value` enlace. Los botones están enlazados a las funciones en la vista de modelo, mediante la `click` enlace. La instancia de un producto se pasa como un parámetro para cada función. Para obtener más información, la [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html) tiene buen descripciones de los distintos enlaces.

A continuación, agregue un enlace para el **enviar** eventos en el formulario Agregar producto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Llama a este enlace el `create` función en el modelo de vista para crear un nuevo producto.

Este es el código completo de la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Ejecute la aplicación, inicie sesión con la cuenta de administrador y haga clic en el vínculo "Admin". Debe ver la lista de productos y ser capaz de crear, actualizar o eliminar los productos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-4.md)
> [Siguiente](using-web-api-with-entity-framework-part-6.md)
