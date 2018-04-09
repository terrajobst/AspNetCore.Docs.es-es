---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Páginas maestras y ASP.NET AJAX (C#) | Documentos de Microsoft
author: rick-anderson
description: Describe las opciones para el uso de AJAX de ASP.NET y las páginas maestras. Si se examinan mediante la clase ScriptManagerProxy; Describe cómo los distintos archivos JS se cargan dependi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 87e5855354610723823da88ec961e7391c3f705f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="master-pages-and-aspnet-ajax-c"></a>Páginas maestras y ASP.NET AJAX (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Describe las opciones para el uso de AJAX de ASP.NET y las páginas maestras. Si se examinan mediante la clase ScriptManagerProxy; Describe cómo se cargan los diversos archivos JS dependiendo de si se utiliza el objeto ScriptManager en el maestro de página o página de contenido.


## <a name="introduction"></a>Introducción

En los últimos años, se han ido acumulando cada vez más desarrolladores [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-aplicaciones web habilitadas. Un sitio Web con AJAX habilitado usa una serie de tecnologías web relacionadas para ofrecer una experiencia de usuario más sensible. La creación de aplicaciones habilitadas para AJAX de ASP.NET es sorprendentemente fáciles gracias a las de Microsoft [marco de AJAX de ASP.NET](../../../../ajax/index.md). AJAX de ASP.NET está organizado en ASP.NET 3.5 y Visual Studio 2008; También está disponible como descarga independiente para las aplicaciones de ASP.NET 2.0.

Al compilar con AJAX habilitado páginas web con el marco de AJAX de ASP.NET, debe agregar exactamente una [control ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) a cada página que utiliza el marco de trabajo. Como su nombre implica, el control ScriptManager administra el script de cliente utilizado en las páginas web con AJAX habilitado. Como mínimo, el objeto ScriptManager emite código HTML que indica al explorador que descargue los archivos JavaScript que utilizan la biblioteca de cliente de AJAX de ASP.NET. También se puede utilizar para registrar los archivos personalizados de JavaScript, los servicios web habilitados para escritura y funcionalidad de servicio de aplicación personalizada.

Si el patrón de sitio usa páginas (como debería), no necesariamente necesitará agregar un control ScriptManager a cada página de contenido única; en su lugar, puede agregar un control ScriptManager a la página maestra. Este tutorial muestra cómo agregar el control ScriptManager a la página maestra. También examina cómo usar el control ScriptManagerProxy para registrar scripts personalizados y servicios de script en una página de contenido específico.

> [!NOTE]
> Este tutorial no explorar diseñar o crear aplicaciones web habilitadas para AJAX con el marco de AJAX de ASP.NET. Para obtener más información sobre el uso de AJAX, consulte el [vídeos de AJAX de ASP.NET](../../../videos/aspnet-ajax/index.md) y [tutoriales](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), así como los recursos enumerados en la sección más información al final de este tutorial.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Estudie el marcado emitido por el Control ScriptManager

El control ScriptManager emite marcado que indica al explorador que descargue los archivos JavaScript que utilizan la biblioteca de cliente de AJAX de ASP.NET. También agrega un poco de JavaScript alineado a la página que inicializa esta biblioteca. El marcado siguiente muestra el contenido que se agrega a la salida representada de una página que incluye un control ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

El `<script src="url"></script>` etiquetas indicar al explorador que descargue y ejecute el archivo de JavaScript en *url*. El ScriptManager emite tres etiquetas de este tipo; uno hace referencia al archivo `WebResource.axd`, mientras que los otros dos hacen referencia al archivo `ScriptResource.axd`. Estos archivos no existen realmente como archivos en el sitio Web. En su lugar, cuando llega una solicitud para cualquiera de estos archivos en el servidor web, el motor ASP.NET examina la cadena de consulta y devuelve el contenido de JavaScript adecuado. La secuencia de comandos proporcionada por estos tres archivos JavaScript externos constituyen la biblioteca de cliente del marco de ASP.NET AJAX. El otro `<script>` etiquetas emitidas por el objeto ScriptManager incluyen script en línea que inicializa esta biblioteca.

Las referencias de script externo y el script en línea emitidos por el objeto ScriptManager son fundamentales para una página que usa el marco de AJAX de ASP.NET, pero no es necesario para las páginas que no utilizan el marco de trabajo. Por lo tanto, podría motivo que resulta ideal para agregar solo un ScriptManager a dichas páginas que utilizan el marco de AJAX de ASP.NET. Y esto es suficiente, pero si tiene muchas páginas que utilizan el marco de trabajo terminará agregar el control ScriptManager a todas las páginas: una tarea repetitiva, al menos. Como alternativa, puede agregar un ScriptManager a la página maestra, que, a continuación, inserta esta secuencia de comandos necesario en todas las páginas de contenido. Con este enfoque, no es necesario recordar agregar un ScriptManager a una nueva página que usa el marco de AJAX de ASP.NET porque ya está incluido en la página maestra. Paso 1 explica cómo agregar un ScriptManager a la página maestra.

> [!NOTE]
> Si tiene previsto incluir la funcionalidad de AJAX dentro de la interfaz de usuario de la página maestra, no tiene ninguna opción en la materia: debe incluir el objeto ScriptManager en la página maestra.


Un inconveniente de agregar el objeto ScriptManager a la página principal es que el script anterior se emite en *cada* página, independientemente de si su necesarios. Claramente Esto conduce al ancho de banda desaprovechado de dichas páginas que obtiene el objeto ScriptManager incluido (a través de la página maestra) todavía no usar ninguna característica del marco de AJAX de ASP.NET. ¿Pero simplemente cuánto se desperdicia ancho de banda?

- El contenido real emitido por el objeto ScriptManager (que se muestra arriba) que suma un poco más de 1KB.
- Los tres archivos de script externo al que hace referencia el `<script>` elemento, sin embargo, componen aproximadamente 450 KB de datos sin comprimir; en un sitio Web que usa la compresión gzip, se puede reducir este ancho de banda total cerca de 100 KB. Sin embargo, estos archivos de script se almacenan en caché por el explorador durante un año, lo que significa que sólo deben descargarse una vez y, a continuación, se pueden reutilizar en otras páginas en el sitio.

En el mejor de los casos, a continuación, cuando se almacenan en caché los archivos de script, el costo total es de 1KB, que es insignificante. En el peor de los casos, sin embargo - que es cuando los archivos de script aún no se han descargado y el servidor web no usa ningún tipo de compresión, el posicionamiento de ancho de banda es alrededor de 450KB, lo que puede agregar en cualquier parte de un segundo o dos a través de una conexión de banda ancha a hasta un minuto  usuarios a través de módems de acceso telefónico. Las buenas noticias son que dado que se almacenan en caché los archivos de script externo mediante el explorador, este escenario de caso peor se produce con poca frecuencia.

> [!NOTE]
> Si todavía se siente incómodo colocando el control ScriptManager en la página maestra, tenga en cuenta el formulario Web Forms (el `<form runat="server">` marcado en la página maestra). Todas las páginas ASP.NET que usa el modelo de postback deben incluir exactamente un formulario Web Forms. Al agregar un formulario Web Forms agrega contenido adicional: un número de campos de formulario oculto, el `<form>` , etiqueta y, si es necesario, una función JavaScript para iniciar una devolución de datos desde un script. Este marcado es necesaria para las páginas que no devuelve datos. Este marcado extraño podría eliminarse quitando el formulario Web Forms de la página maestra y agregarlo manualmente a cada página de contenido que la necesite. Sin embargo, los beneficios de tener el formulario Web Forms en la página maestra compensan las desventajas de tener que agregar innecesariamente a ciertas páginas de contenido.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Paso 1: Agregar un Control ScriptManager a la página maestra

Todas las páginas web que usa el marco de AJAX de ASP.NET debe contener exactamente un control ScriptManager. Debido a este requisito, normalmente tiene sentido colocar un solo control ScriptManager en la página maestra para que todas las páginas de contenido tienen el control ScriptManager incluyen automáticamente. Además, el objeto ScriptManager debe preceder a cualquiera de los controles de servidor ASP.NET AJAX, como los controles UpdatePanel y UpdateProgress. Por lo tanto, es mejor ubicar el objeto ScriptManager antes que los controles ContentPlaceHolder dentro del formulario Web.

Abra la `Site.master` página principal y agregar un control ScriptManager a la página en el formulario Web Forms, pero antes del `<div id="topContent">` elemento (consulte la figura 1). Si está utilizando Visual Web Developer 2008 o Visual Studio 2008, el control ScriptManager se encuentra en el cuadro de herramientas en la pestaña Extensiones de AJAX. Si está utilizando Visual Studio 2005, debe primero instalar el marco de AJAX de ASP.NET y agregar los controles al cuadro de herramientas. Visite la [Wiki de AJAX de ASP.NET](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obtener el marco de trabajo de ASP.NET 2.0.

Después de agregar el objeto ScriptManager a la página, cambiar su `ID` de `ScriptManager1` a `MyManager`.


[![Agregue el objeto ScriptManager a la página maestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: agregar el objeto ScriptManager a la página maestra ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Paso 2: Mediante el marco de AJAX de ASP.NET de una página de contenido

Con el control ScriptManager agregado a la página maestra ahora podemos agregar funcionalidad de AJAX de ASP.NET framework a cualquier página de contenido. Vamos a crear una nueva página ASP.NET que muestra un producto seleccionado aleatoriamente de la base de datos Northwind. Vamos a usar el control de temporizador del marco de ASP.NET AJAX para actualizar esta pantalla cada 15 segundos, que muestra un nuevo producto.

Empiece por crear una nueva página en el directorio raíz denominado `ShowRandomProduct.aspx`. No olvide enlazar esta nueva página para la `Site.master` página maestra.


[![Agregue una nueva página de ASP.NET para el sitio Web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: agregar una nueva página de ASP.NET para el sitio Web ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Recuerde que en la [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que generó el título de la página si fue no se establece explícitamente. Vaya a la `ShowRandomProduct.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualizaremos el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` para el patrón de la lección de interacción de página de contenido:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

La adición de este `<siteMapNode>` elemento se refleja en las lecciones lista (Véase la figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Mostrar un producto seleccionado aleatoriamente

Vuelva a `ShowRandomProduct.aspx`. En el diseñador, arrastre un control UpdatePanel en el cuadro de herramientas en el `MainContent` control de contenido y establecer su `ID` propiedad `ProductPanel`. UpdatePanel representa una región en la pantalla que se pueden actualizar de forma asincrónica a través de un postback de página parcial.

La primera tarea es mostrar información acerca de un producto seleccionado aleatoriamente dentro de UpdatePanel. Iniciar, arrastre un control DetailsView en UpdatePanel. Establecer el control DetailsView `ID` propiedad `ProductInfo` y borrar su `Height` y `Width` propiedades. Expanda la etiqueta inteligente de DetailsView y, en la lista desplegable Elegir origen de datos, decide enlazar DetailsView a un nuevo control SqlDataSource denominado `RandomProductDataSource`.


[![Enlazar el DetailsView a un nuevo Control SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: enlazar DetailsView a un nuevo SqlDataSource Control ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurar el control SqlDataSource para conectarse a la base de datos de Northwind a través de la `NorthwindConnectionString` (que se crea en el [ *interactuar con la página principal de la página de contenido* ](interacting-with-the-content-page-from-the-master-page-cs.md) tutorial). Cuando elige la configuración de la instrucción select especificar una instrucción SQL personalizada y, a continuación, escriba la siguiente consulta:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

El `TOP 1` palabra clave en el `SELECT` cláusula devuelve solo el primer registro devuelto por la consulta. El [ `NEWID()` función](https://msdn.microsoft.com/library/ms190348.aspx) genera un nuevo [valor de identificador único global (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) y puede usarse en un `ORDER BY` cláusula para devolver registros de la tabla en orden aleatorio.


[![Configurar el SqlDataSource para devolver un único registro seleccionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurar la SqlDataSource para devolver un único registro seleccionado aleatoriamente ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Después de completar al asistente, Visual Studio crea un BoundField de las dos columnas devueltas por la consulta anterior. En este momento marcado declarativo de la página debe ser similar al siguiente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Figura 5 se muestra la `ShowRandomProduct.aspx` página cuando se ve mediante un explorador. Haga clic en el botón de actualización de su explorador para volver a cargar la página; debería ver el `ProductName` y `UnitPrice` valores para un nuevo registro seleccionado aleatoriamente.


[![Se muestra el nombre y el precio de un producto aleatorio](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: se muestra el nombre y el precio del producto aleatorio A ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Mostrar automáticamente un nuevo producto cada 15 segundos

El marco de ASP.NET AJAX incluye un control Timer que realiza una devolución de datos en un momento determinado; en el temporizador de devolución de datos `Tick` evento se desencadena. Si el control Timer se coloca dentro de un UpdatePanel desencadena un postback de página parcial, durante el cual se pueden enlazar los datos a DetailsView para mostrar un nuevo producto seleccionado aleatoriamente.

Para ello, arrastre un control Timer desde el cuadro de herramientas y colóquelo en el control UpdatePanel. Cambiar el temporizador `ID` de `Timer1` a `ProductTimer` y su `Interval` propiedad de 60000 a 15000. El `Interval` propiedad indica el número de milisegundos entre las devoluciones de datos; si se establece en 15000 hace que el temporizador desencadenar un postback de página parcial cada 15 segundos. En este momento marcado declarativo del temporizador debe ser similar al siguiente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Crear un controlador de eventos del temporizador `Tick` eventos. En este controlador de eventos es necesario volver a enlazar los datos a DetailsView mediante una llamada a la DetailsView `DataBind` método. Si lo hace, indica a DetailsView para volver a recuperar los datos de su control de origen de datos, que se seleccione y mostrará un nuevo aleatoriamente seleccionado registro (al igual que al volver a cargar la página, haga clic en el botón Actualizar del explorador).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Eso es todo lo! Volver a visitar la página a través de un explorador. Inicialmente, se muestra información de un producto aleatorio. Si observa la pantalla pacientemente observará que, después de 15 segundos, obtener información acerca de un producto nuevo por arte de magia reemplaza la presentación existente.

Para ver mejor lo que está sucediendo aquí, vamos a agregar un control de etiqueta para el UpdatePanel que muestra la hora a la que se actualizó por última vez la presentación. Agregar un control Web Label en UpdatePanel, establezca su `ID` a `LastUpdateTime`y borrar su `Text` propiedad. A continuación, cree un controlador de eventos para el UpdatePanel `Load` eventos y mostrar la hora actual en la etiqueta. (El UpdatePanel `Load` evento se desencadena en cada postback de página completa o parcial.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Con este cambio completo, la página incluye la hora en que se cargó el producto mostrado actualmente. Figura 6 se muestra la página cuando se visita por primera vez. Figura 7 muestra la página 15 segundos más tarde después de que el control Timer ha "marcada" y se haya actualizado el UpdatePanel para mostrar información acerca de un producto nuevo.


[![Se muestra un producto seleccionado aleatoriamente al cargar la página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: producto de una forma aleatoria seleccionado se muestra en la carga de la página ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Cada 15 segundos, se muestra un nuevo aleatoriamente seleccionado producto](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: cada 15 segundos se muestra un nuevo aleatoriamente seleccionado producto ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Paso 3: Usar el Control ScriptManagerProxy

Junto con inclusión de la secuencia de comandos necesario para el marco de AJAX de ASP.NET biblioteca de cliente, el objeto ScriptManager también puede registrar archivos JavaScript personalizados, referencias a servicios Web habilitados para escritura y autenticación personalizada, autorización y servicios de perfiles. Normalmente estas personalizaciones son específicas de una página determinada. Sin embargo, si un script personalizado archivos, las referencias de servicio Web o la autenticación, autorización o servicios de perfiles se hace referencia en el control ScriptManager en la página maestra, a continuación, se incluyen en *todos los* páginas en el sitio Web.

Para agregar las personalizaciones de ScriptManager según una página a página usa el control ScriptManagerProxy. Puede agregar un ScriptManagerProxy a una página de contenido y, a continuación, registrar el archivo JavaScript personalizado, referencia de servicio Web, o la autenticación, autorización o servicio de perfiles de ScriptManagerProxy; Esto tiene el efecto de registro de estos servicios para la página de contenido determinado.

> [!NOTE]
> Una página ASP.NET solo puede tener el control ScriptManager de no más de uno presente. Por lo tanto, no se puede agregar un control ScriptManager a una página de contenido si el control ScriptManager ya está definido en la página maestra. El único propósito de la ScriptManagerProxy es proporcionar una manera para que los desarrolladores definen el objeto ScriptManager en la página maestra, pero seguir aprovechando la capacidad de agregar personalizaciones de ScriptManager según una página a página.


Para ver el control ScriptManagerProxy en acción, vamos a aumentar el UpdatePanel en `ShowRandomProduct.aspx` para incluir un botón que utiliza el script de cliente para pausar o reanudar el control Timer. El control Timer tiene tres métodos de lado cliente que podemos usar para lograr esta funcionalidad deseada:

- `_startTimer()` -inicia el control Timer
- `_raiseTick()` -hace que el control Timer a "otro", con lo que se devolución y provocar su `Tick` eventos en el servidor
- `_stopTimer()` -detiene el control Timer

Vamos a crear un archivo JavaScript a una variable denominada `timerEnabled` y una función denominada `ToggleTimer`. El `timerEnabled` variable indica si el control Timer está actualmente habilitado o deshabilitado; el valor predeterminado es true. El `ToggleTimer` función acepta dos parámetros de entrada: una referencia para el botón de pausa/reanudar y el cliente `id` valor del control Timer. Esta función alterna el valor de `timerEnabled`, obtiene una referencia al control de temporizador, se inicia o detiene el temporizador (dependiendo del valor de `timerEnabled`) y actualiza el texto del botón Mostrar "Pausa" o "Reanudar". Esta función se llamará cada vez que se hace clic en el botón de pausa/reanudar.

Empiece por crear una nueva carpeta en el sitio Web denominado `Scripts`. A continuación, agregue un nuevo archivo en la carpeta de secuencias de comandos denominada `TimerScript.js` de tipo de archivo de JScript.


[![Agregar un nuevo archivo de JavaScript a la carpeta de Scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: agregar un nuevo archivo de JavaScript a la `Scripts` carpeta ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Se ha agregado un nuevo archivo de JavaScript en el sitio Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: se ha agregado un nuevo archivo de JavaScript en el sitio Web ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image27.png))


A continuación, agregue el siguiente script en el archivo TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Ahora necesitamos registrar este archivo JavaScript personalizado en `ShowRandomProduct.aspx`. Vuelva a `ShowRandomProduct.aspx` y agregar un control ScriptManagerProxy a la página; establezca su `ID` a `MyManagerProxy`. Para registrar un JavaScript personalizado archivo seleccionar el control ScriptManagerProxy en el diseñador y, a continuación, vaya a la ventana Propiedades. Una de las propiedades se titula secuencias de comandos. Al seleccionar esta propiedad, se muestra el Editor de la colección ScriptReference se muestra en la figura 10. Haga clic en el botón Agregar para incluir una referencia de script nuevo y, a continuación, escriba la ruta de acceso al archivo de script en la propiedad de ruta de acceso: `~/Scripts/TimerScript.js`.


[![Agregue una referencia de Script para el Control ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: agregar una referencia de Script para el ScriptManagerProxy Control ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Después de agregar la referencia de script el control ScriptManagerProxy's declarativo marcado se actualiza para incluir un `<Scripts>` colección con una sola `ScriptReference` entrada, como el siguiente fragmento de marcado de muestra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

El `ScriptReference` entrada indica la ScriptManagerProxy debe incluir una referencia al archivo JavaScript en el marcado representado. Es decir, mediante el registro personalizado script en el ScriptManagerProxy el `ShowRandomProduct.aspx` salida de la página representada ahora incluye otra `<script src="url"></script>` etiqueta: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Ahora podemos llamar el `ToggleTimer` función definida en `TimerScript.js` desde el script de cliente en la `ShowRandomProduct.aspx` página. Agregue el siguiente código HTML en el UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Esto muestra un botón con el texto "Pausa". Cada vez que se hace clic, la función de JavaScript `ToggleTimer` se llama pasando una referencia para el botón y el valor de identificador del control Timer (`ProductTimer`). Tenga en cuenta la sintaxis para obtener la `id` valor del control Timer. `<%=ProductTimer.ClientID%>` emite el valor de la `ProductTimer` del control Timer `ClientID` propiedad. En el [ *nombres de identificador de Control en páginas de contenido* ](control-id-naming-in-content-pages-cs.md) tutorial analizamos las diferencias entre el lado del servidor `ID` valor y el lado de cliente resultante `id` valor y cómo `ClientID` devuelve el cliente `id`.

La figura 11 muestra esta página cuando visita por primera vez a través de un explorador. El temporizador se está ejecutando actualmente y actualiza la información de producto mostrado cada 15 segundos. La figura 12 muestra la pantalla después de que se ha hecho clic el botón de pausa. Haga clic en el botón de pausa se detiene el temporizador y se actualiza el texto del botón para "Reanudar". La información de producto se actualice (y continuar actualizar cada 15 segundos) una vez que el usuario hace clic en Reanudar.


[![Haga clic en el botón de pausa para detener el Control Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: haga clic en el botón de pausa para detener el Control Timer ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Haga clic en el botón de reanudación para reiniciar el temporizador](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: haga clic en el botón de reanudación para reiniciar el temporizador ([haga clic aquí para ver la imagen a tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Resumen

Al compilar aplicaciones web habilitadas para AJAX mediante el marco de AJAX de ASP.NET es imperativo que todas las páginas web con AJAX habilitado incluyen un control ScriptManager. Para facilitar este proceso, podemos agregar un ScriptManager a la página maestra, en lugar de tener que recordar agregar un ScriptManager a cada página de contenido. Paso 1 se ha explicado cómo agregar el control ScriptManager a la página maestra al paso 2 examinando implementar funcionalidad de AJAX en una página de contenido.

Si necesita agregar scripts personalizados, las referencias a los servicios Web habilitados para escritura, o personalizada autenticación, autorización o servicios de perfiles a una página de contenido determinado, agregue un control ScriptManagerProxy a la página contenida y, a continuación, configurar el personalizaciones no existe. Paso 3 se examina cómo usar el ScriptManagerProxy para registrar un archivo JavaScript personalizado en una página de contenido específico.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [Tutoriales de AJAX de ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos de AJAX de ASP.NET](../../../videos/aspnet-ajax/index.md)
- [Creación de interfaz de usuario interactiva con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilizar NEWID para ordenar los registros de forma aleatoria](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usar el Control Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Siguiente](specifying-the-master-page-programmatically-cs.md)
