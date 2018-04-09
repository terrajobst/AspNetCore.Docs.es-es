---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interactuar con la página de contenido de la página maestra (VB) | Documentos de Microsoft
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código en la página maestra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 9274924b441cb21e33eb57de06ff374428fa036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interactuar con la página de contenido de la página maestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código en la página maestra.


## <a name="introduction"></a>Introducción

El tutorial anterior examina cómo hacer que la página de contenido interactuar mediante programación con su página maestra. Recuerde que se han actualizado la página maestra para incluir un control GridView que muestra los cinco más recientemente agregado productos. A continuación, se crea una página de contenido desde la que el usuario puede agregar un nuevo producto. Tras agregar un nuevo producto, la página de contenido necesario para indicar a la página maestra para actualizar su GridView, por lo que incluiría el producto recién agregados. Esta funcionalidad se consigue agregando un método público a la página maestra que actualiza los datos enlazados a la GridView, y, a continuación, invocar ese método desde la página de contenido.

La forma más habitual de contenido e interacción de página maestra se origina en la página de contenido. Sin embargo, es posible que la página maestra para rouse la página de contenido actual en acción, y esta funcionalidad puede ser necesaria si la página maestra contiene elementos de la interfaz de usuario que permiten a los usuarios modificar datos que también se muestran en la página de contenido. Considere la posibilidad de una página de contenido que muestra la información de productos en un GridView control y una página maestra que incluye un botón de control que, al hacer clic en, dobla los precios de todos los productos. Al igual que el ejemplo en el tutorial anterior, GridView deba actualizarse después el precio doble que se utilice este botón para que muestre los nuevos precios, pero en este escenario es la página maestra que se deba rouse la página de contenido en acción.

Este tutorial explora cómo hacer que la página maestra invocar funcionalidad definida en la página de contenido.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Impongan interacción mediante programación a través de un evento y controladores de eventos

Invocar la funcionalidad de la página de contenido de una página maestra es más complejo que al revés. Dado que una página de contenido tiene una sola página maestra, cuando se impongan la interacción mediante programación desde la página de contenido sabemos qué propiedades y métodos públicos son a nuestra disposición. Sin embargo, una página maestra, puede tener muchas páginas de contenido diferentes, cada uno con su propio conjunto de propiedades y métodos. ¿Cómo, a continuación, podemos es escribir código en la página maestra para realizar alguna acción en su página de contenido cuando no sabemos qué página de contenido se invocará hasta el tiempo de ejecución?

Considere la posibilidad de un control Web de ASP.NET, como el control de botón. Un control de botón puede aparecer en cualquier número de páginas de ASP.NET y necesita un mecanismo mediante el cual se puede alertar la página que se ha hecho clic. Esto se logra mediante *eventos*. En concreto, el control de botón genera su `Click` eventos cuando se hace clic en; la página ASP.NET que contiene el botón, opcionalmente, puede responder a esa notificación a través de un *controlador de eventos*.

Este mismo patrón puede utilizarse para tener una funcionalidad de desencadenador de página maestra en las páginas de contenido:

1. Agregar un evento a la página maestra.
2. Generar el evento cada vez que la página maestra necesita comunicarse con su página de contenido. Por ejemplo, si la página maestra debe alertar a su página de contenido que el usuario ha duplicado los precios, se generará el evento inmediatamente después de que los precios se han duplicado.
3. Crear un controlador de eventos en las páginas de contenido que necesitan realizar alguna acción.

Este resto de este tutorial implementa el ejemplo que se describen en la introducción; es decir, una página de contenido que se enumera los productos en la base de datos y una página maestra que incluye un botón control para doblar los precios.

## <a name="step-1-displaying-products-in-a-content-page"></a>Paso 1: Mostrar productos en una página de contenido

La primera regla de negocio consiste en crear una página de contenido que se enumera los productos de la base de datos Northwind. (Se agrega la base de datos de Northwind al proyecto en el tutorial anterior, [ *interactuar con la página principal de la página de contenido*](interacting-with-the-master-page-from-the-content-page-vb.md).) Empiece por agregar una nueva página ASP.NET para la `~/Admin` carpeta denominada `Products.aspx`, lo que permite seguro para enlazarla a la `Site.master` página maestra. La figura 1 muestra el Explorador de soluciones después de esta página se ha agregado al sitio Web.


[![Agregue una nueva página de ASP.NET a la carpeta Admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figura 01**: agregar una nueva página de ASP.NET para la `Admin` carpeta ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Recuerde que en la [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que genera el título de la página si no lo es establecer explícitamente. Vaya a la `Products.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualizaremos el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` para el contenido a lección de interacción de página maestra:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

La adición de este `<siteMapNode>` elemento se refleja en las lecciones lista (Véase la figura 5).

Vuelva a `Products.aspx`. En el control de contenido para `MainContent`, agregue un control GridView y asígnele el nombre `ProductsGrid`. Enlazar el control GridView a un nuevo control SqlDataSource denominado `ProductsDataSource`.


[![Enlazar el control GridView a un nuevo Control SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figura 02**: enlazar el control GridView a un nuevo SqlDataSource Control ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Configurar al Asistente para que utilice la base de datos Northwind. Si ha trabajado en el tutorial anterior, ya debe tener una cadena de conexión denominada `NorthwindConnectionString` en `Web.config`. Elija esta cadena de conexión en la lista desplegable, como se muestra en la figura 3.


[![Configurar el SqlDataSource para usar la base de datos Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figura 03**: configurar la SqlDataSource para usar la base de datos Northwind ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


A continuación, especifique el control de origen de datos `SELECT` instrucción eligiendo la tabla Products de la lista desplegable y devolver el `ProductName` y `UnitPrice` columnas (consulte la figura 4). Haga clic en siguiente y, a continuación, Finalizar para completar al Asistente para configurar orígenes de datos.


[![Devolver el ProductName y UnitPrice campos de la tabla Products](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figura 04**: devolver el `ProductName` y `UnitPrice` campos desde el `Products` tabla ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


Eso es todo lo! Después de completar al Asistente para Visual Studio agrega dos BoundFields a GridView para reflejar los dos campos devueltos por el control SqlDataSource. Marcado de los controles GridView y SqlDataSource sigue. La figura 5 muestra los resultados cuando se ven a través de un explorador.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Cada producto y su precio se muestra en el control GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figura 05**: cada producto y su precio se muestra en el control GridView ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> No dude en limpiar la apariencia del control GridView. Algunas sugerencias incluyen dé formato al valor UnitPrice mostrado como una moneda y el uso de fuentes y colores de fondo para mejorar la apariencia de la cuadrícula. Para obtener más información sobre cómo mostrar y dar formato a datos en ASP.NET, consulte mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Paso 2: Agregar un botón de precios dobles a la página principal

La siguiente tarea es agregar un control de botón Web al maestro de página que, al hacer clic, le duplique el precio de todos los productos de la base de datos. Abra la `Site.master` página principal y arrastre un botón del cuadro de herramientas hasta el diseñador, colocarlo bajo la `RecentProductsDataSource` control SqlDataSource hemos agregado en el tutorial anterior. Establecer el botón `ID` propiedad `DoublePrice` y su `Text` propiedad en "Dobles precios de productos".

A continuación, agregue un control SqlDataSource a la página maestra, asígnele el nombre `DoublePricesDataSource`. Este SqlDataSource se usará para ejecutar el `UPDATE` instrucción duplicar todos los precios. En concreto, es necesario establecer su `ConnectionString` y `UpdateCommand` propiedades en la cadena de conexión adecuada y `UPDATE` instrucción. A continuación, se necesita llamar a este control SqlDataSource `Update` método cuando el `DoublePrice` se hace clic en el botón. Para establecer el `ConnectionString` y `UpdateCommand` propiedades, seleccione el control SqlDataSource y, a continuación, vaya a la ventana Propiedades. El `ConnectionString` listas de propiedades de esas cadenas de conexión ya almacenadas en `Web.config` en una lista desplegable; Elija la `NorthwindConnectionString` opción tal como se muestra en la figura 6.


[![Configurar el SqlDataSource para usar la conexión NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figura 06**: configurar la SqlDataSource que se utilizan los `NorthwindConnectionString` ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Para establecer el `UpdateCommand` propiedad, busque la opción UpdateQuery en la ventana Propiedades. Esta propiedad, cuando se selecciona, muestra un botón con puntos suspensivos; Haga clic en este botón para mostrar el cuadro de diálogo Editor de parámetros y comandos que se muestra en la figura 7. Escriba lo siguiente `UPDATE` instrucción en el cuadro de texto del cuadro de diálogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Cuando se ejecuta, esta instrucción se duplicará el `UnitPrice` valor para cada registro de la `Products` tabla.


[![Establecer la propiedad UpdateCommand del SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figura 07**: establecer SqlDataSource `UpdateCommand` propiedad ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Después de establecer estas propiedades, marcado declarativo de los controles botón y SqlDataSource debe ser similar al siguiente:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Todo lo que queda es llamar a su `Update` método cuando el `DoublePrice` se hace clic en el botón. Crear un `Click` controlador de eventos para el `DoublePrice` botón y agregue el código siguiente:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Para probar esta funcionalidad, visite la `~/Admin/Products.aspx` página se creó en el paso 1 y haga clic en el botón "Dobles precios de productos". Haga clic en el botón provoca una devolución de datos y ejecuta la `DoublePrice` del botón `Click` controlador de eventos, lo que duplica los precios de todos los productos. A continuación, volver a representar la página y se devuelve el marcado y se volverá a aparecer en el explorador. GridView en la página de contenido, sin embargo, enumera los mismos precios como antes de "Dobles precios de productos" botón se hizo clic. Esto es porque los datos se cargan inicialmente en el control GridView tenían su estado almacenado en estado de vista, por lo que no se recarga cuando las devoluciones de datos a menos que se indique lo contrario. Si visita una página diferente y, a continuación, volver a la `~/Admin/Products.aspx` página, verá los precios actualizados.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Paso 3: Cuando se genera un evento cuando el precios se duplican

Dado que el control GridView en la `~/Admin/Products.aspx` página no reflejan inmediatamente la duplicación de precio, un usuario como es lógico puede pensar que no hizo clic en el botón "Dobles precios de productos", o que no ha funcionado. Pueden intentar al hacer clic en el botón más algunas veces, lo que duplica los precios y otra vez. Para corregir esto es necesario que la cuadrícula en el contenido de presentación de página nuevos precios inmediatamente después de que se duplican.

Según lo descrito anteriormente en este tutorial, es necesario generar un evento en la página maestra cada vez que el usuario hace clic en el `DoublePrice` botón. Un evento es una manera de una clase (un publicador de eventos) notifique a otro un conjunto de otras clases (los suscriptores de eventos) que se ha producido algo interesante. En este ejemplo, la página maestra es el publicador de eventos; las páginas que interesan cuando de contenido la `DoublePrice` hace clic en botón son los suscriptores.

Una clase se suscribe a un evento mediante la creación de un *controlador de eventos*, que es un método que se ejecuta en respuesta al evento que se está generando. El publicador define los eventos que genera definiendo un *delegado de evento*. El delegado de eventos especifica qué parámetros de entrada debe aceptar el controlador de eventos. En .NET Framework, los delegados de eventos no devolver ningún valor y acepta dos parámetros de entrada:

- Un `Object`, que identifica el origen del evento, y
- Una clase derivada de `System.EventArgs`

El segundo parámetro pasado a un controlador de eventos puede incluir información adicional sobre el evento. Mientras que la base de `EventArgs` no pasa la clase a lo largo de toda la información, .NET Framework incluye una serie de clases que extienden `EventArgs` e incluir propiedades adicionales. Por ejemplo, un `CommandEventArgs` instancia se pasa a los controladores de eventos que responden a la `Command` eventos e incluye dos propiedades informativas: `CommandArgument` y `CommandName`.

> [!NOTE]
> Para obtener más información acerca de cómo crear, generar y controlar eventos, vea [eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) y [los delegados de eventos simples de inglés](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Para definir un evento usa la sintaxis siguiente:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Dado que únicamente se necesita la página de contenido de la alerta cuando el usuario hace clic en el `DoublePrice` botón y no es necesario pasar información adicional, se puede usar el delegado de eventos `EventHandler`, que define un controlador de eventos que acepta como su segundo un objeto de tipo de parámetro `System.EventArgs`. Para crear el evento en la página principal, agregue la siguiente línea de código a la clase de código subyacente de la página maestra:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

El código anterior agrega un evento público a la página maestra denominada `PricesDoubled`. Ahora necesitamos generar este evento después de que los precios se han duplicado. Para generar un evento usa la sintaxis siguiente:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Donde *remitente* y *eventArgs* son los valores que se van a pasar al controlador de eventos del suscriptor.

Actualización de la `DoublePrice` `Click` controlador de eventos con el código siguiente:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Al igual que antes, el `Click` controlador de eventos se inicia mediante una llamada a la `DoublePricesDataSource` del control SqlDataSource `Update` método duplicar los precios de todos los productos. Después de que hay dos elementos en el controlador de eventos. En primer lugar, el `RecentProducts` se actualizan los datos de GridView. Este GridView se agregó a la página maestra en el tutorial anterior y muestra los cinco productos agregado más recientemente. Es necesario actualizar esta cuadrícula para que muestre los precios just-duplica para estos cinco productos. A continuación, el `PricesDoubled` evento se desencadena. Una referencia a la propia página maestra (`Me`) se envía al controlador de eventos como el origen del evento y vacío `EventArgs` objeto se envía como argumentos del evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Paso 4: Controlar el evento en la página de contenido

En este momento la página maestra genera su `PricesDoubled` evento cada vez que la `DoublePrice` se hace clic en el control de botón. Sin embargo, esto es sólo la mitad de la batalla, necesitamos controlar el evento en el suscriptor. Esto implica dos pasos: crear el controlador de eventos y agregar código de cableado de eventos para que cuando se produce el evento se ejecuta el controlador de eventos.

Empiece por crear un controlador de eventos denominado `Master_PricesDoubled`. Debido a cómo se definen los `PricesDoubled` evento en la página maestra dos parámetros de entrada del controlador de eventos deben ser de tipos `Object` y `EventArgs`, respectivamente. En la llamada al controlador de eventos el `ProductsGrid` de GridView `DataBind` método volver a enlazar los datos a la cuadrícula.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

El código del controlador de eventos se completa, pero hemos aún a la página principal de la conexión `PricesDoubled` evento al controlador de eventos. El suscriptor conecta un evento con un controlador de eventos a través de la sintaxis siguiente:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Publisher* es una referencia al objeto que ofrece el evento *eventName*, y *methodName* es el nombre del controlador de eventos definido en el suscriptor.

Este código de cableado de eventos debe ejecutarse en la primera página visita y postbacks posteriores y debe realizarse en un punto en el ciclo de vida de la página que precede al que se puede generar el evento. Es un buen momento para agregar código de conexión de eventos en la fase de PreInit, lo que sucede muy pronto en el ciclo de vida de la página.

Abra `~/Admin/Products.aspx` y crear un `Page_PreInit` controlador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Para poder completar este código de cableado se necesita una referencia a la página maestra mediante programación desde la página de contenido. Como se indicó en el tutorial anterior, hay dos maneras de hacerlo:

- Convirtiendo débilmente tipadas `Page.Master` propiedad al tipo adecuado de página maestra, o
- Mediante la adición de un `@MasterType` la directiva en el `.aspx` página y, a continuación, usar fuertemente tipadas `Master` propiedad.

Vamos a usar el último enfoque. Agregue las siguientes `@MasterType` directiva al principio del marcado declarativo de la página:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

A continuación, agregue el siguiente código de conexión de eventos en el `Page_PreInit` controlador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Con este código en su lugar, se actualiza el control GridView en la página de contenido cada vez que la `DoublePrice` se hace clic en el botón.

Las figuras 8 y 9 muestran este comportamiento. La figura 8 muestra la página cuando se visita por primera vez. Tenga en cuenta que los valores de precio en ambos el `RecentProducts` GridView (en la columna izquierda de la página maestra) y la `ProductsGrid` GridView (en la página de contenido). Figura 9 se muestra la misma pantalla inmediatamente después de la `DoublePrice` se ha hecho clic de botón. Como puede ver, los nuevos precios instantáneamente se reflejarán en ambos GridView.


[![Los valores de precio inicial](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figura 08**: los valores iniciales de precio ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Los precios de Just-Doubled se muestran en la GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figura 09**: The Just-Doubled precios se muestran en la GridView ([haga clic aquí para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Resumen

Lo ideal es que, una página maestra y las páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Sin embargo, si tiene una página maestra o la página de contenido que muestra los datos que se pueden modificar desde la página maestra o la página de contenido, a continuación, se que necesite tener la página maestra alertar a la página de contenido (o viceversa un) cuando se modifican los datos para que se puede actualizar la presentación. En el tutorial anterior, hemos visto cómo hacer que una página de contenido interactuar mediante programación con su página principal; en este tutorial explicamos cómo hacer que una página maestra: iniciar la interacción.

Mientras interacción mediante programación entre una página maestra y el contenido puede originarse en el contenido o una página maestra, el patrón de interacción utilizado depende de la creación. Las diferencias son debido al hecho de que una página de contenido tiene una sola página maestra, pero una página maestra puede tener muchas páginas de contenido diferentes. En lugar de tener una página maestra interactuar directamente con una página de contenido, un mejor enfoque es que la página maestra genere un evento para indicar que ha llevado a cabo alguna acción. Las páginas de contenido que desea ocuparse de la acción puede crear controladores de eventos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Obtener acceso y actualizar los datos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Pasar información entre el contenido y páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en los tutoriales ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Suchi Banerjee. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Siguiente](master-pages-and-asp-net-ajax-vb.md)
