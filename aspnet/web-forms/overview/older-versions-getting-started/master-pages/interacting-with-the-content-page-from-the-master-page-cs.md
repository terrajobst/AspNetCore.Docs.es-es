---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interactuar con la página de contenido desde la página maestra (C#) | Microsoft Docs
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código en la página maestra.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 815752ee70eb761d7f9da24c9eada9d4c0c833a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829938"
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interactuar con la página de contenido desde la página maestra (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) o [descargar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código en la página maestra.


## <a name="introduction"></a>Introducción

El tutorial anterior examina cómo hacer que la página de contenido interactuar mediante programación con su página principal. Recuerde que hemos actualizado la página maestra para incluir un control GridView que muestra los cinco más recientemente agregado productos. A continuación, creamos una página de contenido desde la que el usuario podría agregar un nuevo producto. Al agregar un nuevo producto, la página de contenido necesario para indicar a la página maestra para actualizar su GridView por lo que incluiría el producto recién agregados. Esta funcionalidad se logra agregando un método público a la página maestra que actualiza los datos enlazados en el control GridView, y, a continuación, invocar ese método desde la página de contenido.

La forma más común de contenido y la interacción de la página principal se origina en la página de contenido. Sin embargo, es posible que la página maestra para rouse la página de contenido actual en acción, y esta funcionalidad puede ser necesaria si la página maestra contiene elementos de interfaz de usuario que permiten a los usuarios modificar datos que también se muestran en la página de contenido. Considere la posibilidad de una página de contenido que muestra el control de la información de productos en un control GridView y control de una página maestra que incluye un botón que, al hacer clic en, a doblar los precios de todos los productos. Igual que el ejemplo en el tutorial anterior, debe actualizarse después el precio doble se hace clic en botón de modo que muestre los nuevos precios GridView, pero en este escenario es la página maestra que se deba rouse la página de contenido en una acción.

Este tutorial describe cómo hacer que la página maestra invocar funcionalidad definida en la página de contenido.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Impongan interacción mediante programación a través de un evento y los controladores de eventos

Invocar la funcionalidad de la página de contenido de una página maestra es más difícil que al revés. Dado que una página de contenido tiene una única página principal, cuando se impongan la interacción mediante programación desde la página de contenido que sabemos qué propiedades y métodos públicos son a nuestra disposición. Sin embargo, una página maestra, puede tener muchas páginas de contenido diferentes, cada uno con su propio conjunto de propiedades y métodos. ¿Cómo, a continuación, podemos escribir código en la página maestra para realizar alguna acción en su página de contenido cuando no sabemos qué página de contenido que se va a invocar hasta el tiempo de ejecución?

Considere la posibilidad de un control Web de ASP.NET, como el control de botón. Un control de botón puede aparecer en cualquier número de páginas ASP.NET y necesita un mecanismo por el que se puede alertar la página que se hizo clic. Esto se logra mediante *eventos*. En concreto, el control de botón genera su `Click` eventos cuando se hace clic en; la página ASP.NET que contiene el botón, opcionalmente, puede responder a esa notificación a través de un *controlador de eventos*.

Este mismo patrón puede usarse para tener una función de desencadenador de la página maestra en las páginas de contenido:

1. Agregar un evento a la página maestra.
2. Genera el evento cada vez que la página maestra que se necesita para comunicarse con su página de contenido. Por ejemplo, si necesita la página maestra alertar a su página de contenido que el usuario ha duplicado los precios, su evento podría generarse inmediatamente después de que los precios se han duplicado.
3. Cree un controlador de eventos en las páginas de contenido que necesitan realizar alguna acción.

En el resto de este tutorial implementa el ejemplo descrito en la introducción; es decir, una página de contenido que se enumera los productos en la base de datos y una página maestra que incluye un botón de control para doblar los precios.

## <a name="step-1-displaying-products-in-a-content-page"></a>Paso 1: Visualización de productos en una página de contenido

Nuestra primera regla de negocio consiste en crear una página de contenido que se enumera los productos de la base de datos Northwind. (Se agrega la base de datos de Northwind al proyecto en el tutorial anterior, [ *interactuar con la página maestra desde la página de contenido*](interacting-with-the-master-page-from-the-content-page-cs.md).) Empiece por agregar una nueva página ASP.NET para la `~/Admin` carpeta denominada `Products.aspx`y asegúrese que se va a enlazar la `Site.master` página maestra. Figura 1 muestra el Explorador de soluciones después de esta página se ha agregado al sitio Web.


[![Agregue una nueva página de ASP.NET a la carpeta Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Figura 01**: agregar una nueva página de ASP.NET para la `Admin` carpeta ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Recuerde que en el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que genera el título de la página si no lo está establecer explícitamente. Vaya a la `Products.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualice el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` para el contenido de la lección de interacción de página maestra:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

La adición de este `<siteMapNode>` elemento se refleja en las lecciones de lista (consulte la figura 5).

Vuelva a `Products.aspx`. En el control de contenido para `MainContent`, agregue un control GridView y asígnele el nombre `ProductsGrid`. Enlazar el control GridView a un nuevo control SqlDataSource denominado `ProductsDataSource`.


[![Enlazar el control GridView a un nuevo Control SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Figura 02**: enlazar el control GridView a un nuevo SqlDataSource Control ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Configurar al Asistente para que utilice la base de datos Northwind. Si ha trabajado en el tutorial anterior, ya debe tener una cadena de conexión denominada `NorthwindConnectionString` en `Web.config`. Elija esta cadena de conexión en la lista desplegable, como se muestra en la figura 3.


[![Configurar SqlDataSource para usar la base de datos Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Figura 03**: configurar SqlDataSource para usar la base de datos Northwind ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


A continuación, especifique el control de origen de datos `SELECT` instrucción mediante la elección de la tabla Products en la lista desplegable y devolver el `ProductName` y `UnitPrice` columnas (consulte la figura 4). Haga clic en siguiente y después en Finalizar para completar al Asistente para configurar orígenes de datos.


[![Devolver los campos de UnitPrice y ProductName de la tabla de productos](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Figura 04**: devolver el `ProductName` y `UnitPrice` campos desde la `Products` tabla ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Eso es todo esto! Después de completar al Asistente para Visual Studio agrega dos BoundFields en el control GridView para reflejar los dos campos devueltos por el control SqlDataSource. Marcado de los controles GridView y SqlDataSource sigue. Figura 5 muestra los resultados cuando se ve mediante un explorador.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Cada producto y su precio se muestra en el control GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Figura 05**: cada producto y su precio se muestra en el control GridView ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> No dude en limpiar la apariencia del control GridView. Algunas sugerencias incluyen dar formato al valor mostrado de UnitPrice como moneda y el uso de fuentes y colores de fondo para mejorar la apariencia de la cuadrícula. Para obtener más información sobre cómo mostrar y dar formato a datos en ASP.NET, consulte mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Paso 2: Agregar un botón de precios dobles a la página maestra

La siguiente tarea es agregar un control de botón Web al maestro de página que, al hacer clic, doble el precio de todos los productos de la base de datos le. Abrir el `Site.master` página principal y arrastre un botón del cuadro de herramientas hasta el diseñador, colocarlo bajo la `RecentProductsDataSource` control SqlDataSource se ha agregado en el tutorial anterior. Establezca el botón `ID` propiedad `DoublePrice` y su `Text` propiedad en "Precios de productos dobles".

A continuación, agregue un control SqlDataSource para la página maestra, asígnele el nombre `DoublePricesDataSource`. Este SqlDataSource se usará para ejecutar el `UPDATE` instrucción duplicar todos los precios. En concreto, es necesario establecer su `ConnectionString` y `UpdateCommand` propiedades a la cadena de conexión adecuada y `UPDATE` instrucción. A continuación, necesitamos llamar a este control SqlDataSource `Update` método cuando el `DoublePrice` se hace clic en el botón. Para establecer el `ConnectionString` y `UpdateCommand` propiedades, seleccione el control SqlDataSource y, a continuación, vaya a la ventana Propiedades. El `ConnectionString` esas cadenas de conexión ya están almacenadas en listas de propiedades `Web.config` en una lista desplegable; Elija el `NorthwindConnectionString` opción tal como se muestra en la figura 6.


[![Configurar para usar el NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Figura 06**: configurar SqlDataSource para usar el `NorthwindConnectionString` ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Para establecer el `UpdateCommand` propiedad, busque la opción UpdateQuery en la ventana Propiedades. Esta propiedad, cuando se selecciona, muestra un botón con puntos suspensivos; Haga clic en este botón para mostrar el cuadro de diálogo Editor de parámetros y comandos que se muestra en la figura 7. Escriba lo siguiente `UPDATE` instrucción en el cuadro de texto del cuadro de diálogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Cuando se ejecuta, esta instrucción se duplicará el `UnitPrice` valor para cada registro en el `Products` tabla.


[![Establecer propiedad de UpdateCommand de SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Figura 07**: establecer SqlDataSource `UpdateCommand` propiedad ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Después de establecer estas propiedades, el marcado declarativo de los controles SqlDataSource y botón debe ser similar al siguiente:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Todo lo que queda es llamar a su `Update` método cuando el `DoublePrice` se hace clic en el botón. Crear un `Click` controlador de eventos para el `DoublePrice` botón y agregue el código siguiente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Para probar esta funcionalidad, visite la `~/Admin/Products.aspx` página se creó en el paso 1 y haga clic en el botón "Doble precios de productos". Al hacer clic en el botón provoca una devolución de datos y ejecuta el `DoublePrice` del botón `Click` controlador de eventos, lo que duplica los precios de todos los productos. A continuación, volver a representar la página y se devuelve el marcado y se volverá a aparecer en el explorador. El control GridView en la página de contenido, sin embargo, muestra los mismos precios como antes de "Double precios de productos" botón se hizo clic. Esto es porque los datos que se cargó inicialmente en el control GridView tenían su estado almacenado en estado de vista, por lo que no se recarga en devoluciones de datos a menos que se indique lo contrario. Si visita una página distinta y, a continuación, volver a la `~/Admin/Products.aspx` página podrá ver los precios actualizados.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Paso 3: Generar un evento cuando el precios se duplican

Dado que el control GridView en la `~/Admin/Products.aspx` página no refleja inmediatamente lo que duplica el precio, un usuario puede considerar como es lógico que no hizo clic en el botón "Precios de productos Double", o que no funciona. Puede intentar hacer clic en el botón algunas más veces, lo que duplica los precios y otra vez. Para solucionarlo necesitamos tener la cuadrícula del contenido de página muestra los nuevos precios inmediatamente después de que se duplican.

Como se explicó anteriormente en este tutorial, tenemos que generar un evento en la página principal cada vez que el usuario hace clic en el `DoublePrice` botón. Un evento es una forma de una clase (un publicador de eventos) notifique a otro, un conjunto de otras clases (los suscriptores de eventos) que ha sucedido algo interesante. En este ejemplo, la página maestra es el publicador de eventos; las páginas que preocupan cuando de contenido la `DoublePrice` se hace clic en botón son los suscriptores.

Una clase se suscribe a un evento mediante la creación de un *controlador de eventos*, que es un método que se ejecuta en respuesta al evento que se generen. El publicador define los eventos que provoca definiendo un *delegado de evento*. El delegado de eventos especifica qué parámetros de entrada debe aceptar el controlador de eventos. En .NET Framework, los delegados de eventos no devuelve ningún valor y acepte dos parámetros de entrada:

- Un `Object`, que identifica el origen del evento, y
- Una clase derivada de `System.EventArgs`

El segundo parámetro pasado a un controlador de eventos puede incluir información adicional sobre el evento. Mientras la base de `EventArgs` no pasa la clase a lo largo de toda la información, .NET Framework incluye una serie de clases que extienden `EventArgs` y abarcar propiedades adicionales. Por ejemplo, un `CommandEventArgs` instancia se pasa a los controladores de eventos que respondan a la `Command` eventos e incluye dos propiedades informativas: `CommandArgument` y `CommandName`.

> [!NOTE]
> Para obtener más información sobre cómo crear, generar y controlar eventos, vea [eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) y [delegados de eventos simples de inglés](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Para definir un evento use la sintaxis siguiente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Dado que solo debemos a la página de contenido de la alerta cuando el usuario ha hecho clic el `DoublePrice` botón y no es necesario pasar información adicional, podemos utilizar el delegado de eventos `EventHandler`, que define un controlador de eventos que acepta como su segundo un objeto de tipo de parámetro `System.EventArgs`. Para crear el evento en la página principal, agregue la siguiente línea de código a la clase de código subyacente de la página maestra:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

El código anterior agrega un evento público a la página maestra denominada `PricesDoubled`. Ahora es necesario generar este evento después de que los precios se han duplicado. Para provocar un evento utilice la sintaxis siguiente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Donde *remitente* y *eventArgs* son los valores que van a pasar al controlador de eventos del suscriptor.

Actualización de la `DoublePrice` `Click` controlador de eventos con el código siguiente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Como antes, el `Click` se inicia el controlador de eventos mediante una llamada a la `DoublePricesDataSource` del control SqlDataSource `Update` método duplicar los precios de todos los productos. Después de que hay dos elementos en el controlador de eventos. En primer lugar, el `RecentProducts` se actualizan los datos de GridView. Este control GridView se ha agregado a la página maestra en el tutorial anterior y muestra los cinco productos agregado más recientemente. Es necesario actualizar esta cuadrícula para que muestre los precios duplica just para estos cinco productos. A continuación, el `PricesDoubled` provoca el evento. Una referencia a la propia página maestra (`this`) se envía al controlador de eventos como el origen del evento y un valor vacío `EventArgs` objeto enviado como los argumentos del evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Paso 4: Controlar el evento en la página de contenido

En este momento la página maestra genera su `PricesDoubled` evento cada vez que el `DoublePrice` se hace clic en el control de botón. Sin embargo, esto es sólo la mitad de la batalla, todavía es necesario controlar el evento en el suscriptor. Esto implica dos pasos: crear el controlador de eventos y agregar código de cableado de eventos para que cuando se provoca el evento se ejecuta el controlador de eventos.

Empiece por crear un controlador de eventos denominado `Master_PricesDoubled`. Debido a cómo se definen los `PricesDoubled` eventos en la página maestra dos parámetros de entrada del controlador de eventos deben ser de tipos `Object` y `EventArgs`, respectivamente. En la llamada al controlador de eventos el `ProductsGrid` de GridView `DataBind` método para volver a enlazar los datos a la cuadrícula.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

El código para el controlador de eventos está completo, pero aún no hemos conectar la página maestra `PricesDoubled` eventos a este controlador de eventos. El suscriptor conecta un evento a un controlador de eventos a través de la sintaxis siguiente:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*publicador* es una referencia al objeto que ofrece el evento *eventName*, y *methodName* es el nombre del controlador de eventos definido en el suscriptor que tenga una firma correspondiente a el *eventDelegate*. En otras palabras, si el evento delegado es `EventHandler`, a continuación, *methodName* debe ser el nombre de un método en el suscriptor que no devuelve un valor y acepta dos parámetros de tipos de entrada `Object` y `EventArgs`, respectivamente.

Este código de cableado del evento debe ejecutarse en la primera visita de página y los postbacks subsiguientes y debe realizarse en un punto en el ciclo de vida de la página que precede al que se puede generar el evento. Un buen momento para agregar código de cableado del evento está en la fase de PreInit, lo que se produce una etapa muy temprana del ciclo de vida de página.

Abra `~/Admin/Products.aspx` y cree un `Page_PreInit` controlador de eventos:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Para completar este código de la conexión se necesita una referencia mediante programación a la página maestra desde la página de contenido. Como se indicó en el tutorial anterior, hay dos maneras de hacerlo:

- Convirtiendo débilmente tipadas `Page.Master` propiedad al tipo adecuado de página maestra, o
- Agregando un `@MasterType` la directiva en el `.aspx` página y, a continuación, usar fuertemente tipadas `Master` propiedad.

Vamos a usar el último enfoque. Agregue las siguientes `@MasterType` la directiva a la parte superior de marcado declarativo de la página:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

A continuación, agregue el siguiente código de cableado de eventos en el `Page_PreInit` controlador de eventos:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Con este código en su lugar, el control GridView en la página de contenido se actualiza cada vez que el `DoublePrice` se hace clic en el botón.

Las figuras 8 y 9 muestran este comportamiento. Figura 8 muestra la página cuando se visita por primera vez. Tenga en cuenta que los valores de precio en ambos el `RecentProducts` GridView (en la columna izquierda de la página maestra) y el `ProductsGrid` GridView (en la página de contenido). Figura 9 se muestra en la misma pantalla inmediatamente después de la `DoublePrice` se ha hecho clic de botón. Como puede ver, los nuevos precios al instante se reflejarán en ambos GridView.


[![Los valores de precio inicial](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Figura 08**: los valores de precio inicial ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Los precios Just-Doubled se muestran en la GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Figura 09**: The Just-Doubled precios se muestran en la GridView ([haga clic aquí para ver imagen en tamaño completo](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Resumen

Lo ideal es que, una página maestra y las páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Sin embargo, si tiene una página maestra o una página de contenido que muestra los datos que se pueden modificar desde la página maestra o una página de contenido, debe tener la página principal la página de contenido (o viceversa un) de la alerta cuando se modifican los datos para que se puede actualizar la presentación. En el tutorial anterior, vimos cómo hacer que una página de contenido interactuar mediante programación con su página maestra; en este tutorial hemos examinado cómo hacer que una página maestra: iniciar la interacción.

Mientras que la interacción mediante programación entre una página maestra y el contenido puede originarse en el contenido o la página maestra, usa el patrón de interacción depende el origen. Las diferencias son debido al hecho de que una página de contenido tiene una sola página maestra, pero una página maestra puede tener muchas páginas de contenido diferentes. En lugar de tener una página maestra interactuar directamente con una página de contenido, un mejor enfoque es que la página principal genere un evento para indicar que ha realizado alguna acción. Las páginas de contenido que interesan a la acción pueden crear controladores de eventos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Acceder y actualizar datos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Pasar información entre el contenido y páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en los tutoriales ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Suchi Banerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Siguiente](master-pages-and-asp-net-ajax-cs.md)
