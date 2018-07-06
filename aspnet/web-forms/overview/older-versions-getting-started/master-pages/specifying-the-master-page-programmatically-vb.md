---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Especificar la página maestra mediante programación (VB) | Microsoft Docs
author: rick-anderson
description: Examina la configuración de la página principal de la página de contenido mediante programación con el controlador de eventos PreInit.
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c84462487426fc50888708baeedd2fb50983ce3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814846"
---
<a name="specifying-the-master-page-programmatically-vb"></a>Especificar la página maestra mediante programación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) o [descargar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Examina la configuración de la página principal de la página de contenido mediante programación con el controlador de eventos PreInit.


## <a name="introduction"></a>Introducción

Desde el ejemplo inaugural de [ *crear un diseño de todo el sitio usar páginas maestra*](creating-a-site-wide-layout-using-master-pages-vb.md), todo el contenido páginas han hecho referencia a su página maestra mediante declaración a través de la `MasterPageFile` atributo en el `@Page`directiva. Por ejemplo, la siguiente `@Page` directiva vincula la página de contenido a la página maestra `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

El [ `Page` clase](https://msdn.microsoft.com/library/system.web.ui.page.aspx) en el `System.Web.UI` espacio de nombres incluye una [ `MasterPageFile` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que devuelve la ruta de acceso a la página principal de la página de contenido; es esta propiedad establecido por el `@Page` directiva. Esta propiedad también puede utilizarse para especificar mediante programación la página principal de la página de contenido. Este enfoque es útil si desea asignar dinámicamente la página maestra en función de factores externos, como el usuario visita la página.

En este tutorial se agregará una segunda página maestra a nuestro sitio Web y decide qué página maestra que se usa en tiempo de ejecución de forma dinámica.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Paso 1: Un vistazo el ciclo de vida de página

Cada vez que llega una solicitud en el servidor web para una página ASP.NET que es una página de contenido, el motor de ASP.NET debe fuse la página contenido controles ContentPlaceHolder correspondiente del controles en la página maestra. Esta fusión crea una jerarquía de control único que, a continuación, puede realizarse a través del ciclo de vida de página típico.

Figura 1 ilustra esta fusión. Paso 1 en la figura 1 muestra el contenido inicial y las jerarquías de control de página maestra. En la parte final de la fase de PreInit el contenido controles en la página se agregan a la ContentPlaceHolders correspondiente en la página maestra (paso 2). Después de esta fusión, la página principal sirve como la raíz de la jerarquía de controles fusionadas. Esto fusionan control jerarquía, a continuación, se agrega a la página para generar la jerarquía de control finalizados (paso 3). El resultado neto es que la jerarquía de controles de la página incluye la jerarquía de controles fusionadas.


[![La página maestra y las jerarquías de Control de la página de contenido son fusionan juntos durante la fase de PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figura 01**: jerarquías de Control de la página maestra y la página de contenido son fusionan juntos durante la fase de PreInit ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Paso 2: Establecer el`MasterPageFile`propiedad desde el código

¿En qué página maestra partakes en esta fusión depende del valor de la `Page` del objeto `MasterPageFile` propiedad. Establecer el `MasterPageFile` atributo en el `@Page` directiva tiene el efecto neto de la asignación de la `Page`del `MasterPageFile` propiedad durante la fase de inicialización, que es la primera etapa del ciclo de vida de la página. También podemos establecer esta propiedad mediante programación. Sin embargo, es imperativo que esta propiedad se establece antes de la fusión en la figura 1.

Al principio de la fase de PreInit el `Page` objeto genera su [ `PreInit` eventos](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) y llama a su [ `OnPreInit` método](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para establecer la página maestra mediante programación, a continuación, nos podemos crear un controlador de eventos para el `PreInit` eventos o invalidar el `OnPreInit` método. Echemos un vistazo a ambos enfoques.

Comience abriendo `Default.aspx.vb`, el archivo de clase de código subyacente para la página principal de nuestro sitio. Agregar un controlador de eventos de la página `PreInit` eventos escribiendo en el código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Desde aquí podemos establecer el `MasterPageFile` propiedad. Actualice el código para que asigna el valor "~ / Site.master" para el `MasterPageFile` propiedad.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Si establece un punto de interrupción y de inicio con la depuración, verá que cada vez que el `Default.aspx` se visita la página o cada vez que hay una devolución de datos a esta página, el `Page_PreInit` ejecuta el controlador de eventos y el `MasterPageFile` propiedad se asigna a "~ / Site.master".

Como alternativa, puede invalidar el `Page` la clase `OnPreInit` método y establezca el `MasterPageFile` propiedad no existe. En este ejemplo, vamos a no establecer la página maestra en una página determinada, sino desde `BasePage`. Recuerde que hemos creado una clase de página base personalizada (`BasePage`) en el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial. Actualmente `BasePage` invalida la `Page` la clase `OnLoadComplete` método, que define la página `Title` propiedad según los datos de mapa del sitio. Vamos a actualizar `BasePage` también invalidar el `OnPreInit` método para especificar mediante programación la página maestra.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Dado que todas nuestras páginas de contenido que se derivan de `BasePage`, todos ellos tienen ahora su página maestra mediante programación asignada. En este momento la `PreInit` controlador de eventos en `Default.aspx.vb` es superflua; no dude en quitarlo.

### <a name="what-about-thepagedirective"></a>¿Qué sucede el`@Page`directiva?

Lo que puede ser un poco confuso es que las páginas del contenido `MasterPageFile` ahora se especifican las propiedades en dos lugares: mediante programación en el `BasePage` la clase `OnPreInit` método así como a través del `MasterPageFile` atributo en cada página de contenido `@Page` directiva.

La primera fase del ciclo de vida de página es la fase de inicialización. Durante esta fase el `Page` del objeto `MasterPageFile` se asigna el valor de propiedad el `MasterPageFile` atributo el `@Page` directiva (si se proporciona). La fase de PreInit sigue a la fase de inicialización y aquí es donde se establece mediante programación el `Page` del objeto `MasterPageFile` propiedad, con lo que se sobrescriba el valor asignado desde el `@Page` directiva. Porque estamos configurando la `Page` del objeto `MasterPageFile` propiedad mediante programación, podríamos Quitamos el `MasterPageFile` de atributo de la `@Page` directiva sin afectar a la experiencia del usuario final. Para convencer a usted mismo de esto, continúe y quitar el `MasterPageFile` de atributo de la `@Page` directiva `Default.aspx` y, a continuación, visite la página a través de un explorador. Como cabría esperar, el resultado es el mismo que antes de que se ha quitado el atributo.

Si el `MasterPageFile` propiedad se establece a través de la `@Page` directiva o mediante programación es no afectan a la experiencia del usuario final. Sin embargo, el `MasterPageFile` atributo el `@Page` directiva se usa Visual Studio durante el tiempo de diseño para generar el WYSIWYG vista en el diseñador. Si vuelve a `Default.aspx` en Visual Studio y navegue hasta el diseñador verá el mensaje, "error de página maestra: la página tiene controles que requieren una referencia de página maestra, pero no se especifica ninguna" (consulte la figura 2).

En resumen, debe dejar el `MasterPageFile` atributo el `@Page` directiva para disfrutar de una experiencia de tiempo de diseño en Visual Studio.


[![Visual Studio usa el @Page MasterPageFile (atributo) de la directiva para representar la vista de diseño](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figura 02**: Visual Studio usa el `@Page` la directiva `MasterPageFile` atributo para representar la vista de diseño ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Paso 3: Creación de una página maestra alternativa

Porque la página principal de la página de contenido se puede establecer mediante programación en tiempo de ejecución es posible cargar dinámicamente una página maestra determinada según algunos criterios externos. Esta funcionalidad puede ser útil en situaciones donde el diseño de la carpeta del sitio debe variar en función del usuario. Por ejemplo, una aplicación web de motor de blog es posible que su les permitirá elegir un diseño para su blog, donde cada diseño está asociada con una página maestra diferente. En tiempo de ejecución, cuando un visitante está viendo el blog de un usuario, la aplicación web deberá determinar el diseño de blog y asociar dinámicamente la página maestra correspondiente a la página de contenido.

Examinemos cómo cargar dinámicamente una página maestra en tiempo de ejecución según algunos criterios externos. Actualmente, nuestro sitio Web contiene una sola página maestra (`Site.master`). Se necesita otra página maestra para ilustrar la elección de una página maestra en tiempo de ejecución. Este paso se centra en crear y configurar la nueva página maestra. Paso 4 examina determinar qué página maestra que se usa en tiempo de ejecución.

Crear una nueva página principal en la carpeta raíz denominada `Alternate.master`. También, agregar una nueva hoja de estilos para el sitio Web denominado `AlternateStyles.css`.


[![Agregue otro archivo CSS y página maestra al sitio Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figura 03**: agregar otra página maestra y archivo CSS para el sitio Web ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image9.png))


He diseñado el `Alternate.master` página maestra para que el título que se muestra en la parte superior de la página, centrado y sobre un fondo azul marino. He dispensar de la columna izquierda y mover ese contenido bajo el `MainContent` control ContentPlaceHolder, que ahora abarca todo el ancho de la página. Además, nixed la lista de las lecciones desordenada y lo reemplazamos por una lista horizontal anterior `MainContent`. También actualizan las fuentes y colores usados por la página principal (y, por extensión, las páginas de contenido). La figura 4 muestra `Default.aspx` cuando se usa el `Alternate.master` página maestra.

> [!NOTE]
> ASP.NET incluye la capacidad para definir *temas*. Un tema es una colección de imágenes, archivos CSS y relacionadas con el estilo Web propiedad configuración del control que se puede aplicar a una página en tiempo de ejecución. Los temas son la mejor opción si los diseños de su sitio difieren solo en las imágenes mostradas y sus reglas de CSS. Si el diseño difiere notablemente más, como el uso de controles Web diferentes o tiene un diseño radicalmente diferente, a continuación, deberá utilizar páginas principales independientes. Consulte la sección Lecturas adicionales al final de este tutorial para obtener más información acerca de los temas.


[![Nuestras páginas de contenido pueden usar ahora un nuevo aspecto](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figura 04**: nuestras páginas de contenido pueden usar ahora un nuevo aspecto ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image12.png))


Cuando se fusionan el patrón y el marcado de las páginas de contenido, el `MasterPage` clase comprobaciones para asegurarse de que el contenido de cada control en la página de contenido hace referencia a un control ContentPlaceHolder en la página maestra. Se produce una excepción si se encuentra un control de contenido que hace referencia a un ContentPlaceHolder inexistentes. En otras palabras, es imperativo que la página maestra que se asigna a la página de contenido tiene un ContentPlaceHolder para cada control en la página de contenido de contenido.

El `Site.master` página principal incluye cuatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algunas de las páginas de contenido en nuestro sitio Web incluyen uno o dos controles de contenido; otros incluyen un control de contenido para cada uno de los disponibles ContentPlaceHolders. Si nuestra nueva página maestra (`Alternate.master`) nunca se pueden asignar a las páginas de contenido que tienen controles de contenido para todos los ContentPlaceHolders en `Site.master` es fundamental que `Alternate.master` también incluyen los mismos controles ContentPlaceHolder como `Site.master`.

Para obtener su `Alternate.master` página maestra para tener un aspecto similar al mío (consulte la figura 4), empiece por definir estilos de la página maestra en el `AlternateStyles.css` hoja de estilos. Agregue las siguientes reglas en `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

A continuación, agregue el siguiente marcado declarativo para `Alternate.master`. Como puede ver, `Alternate.master` contiene cuatro controles ContentPlaceHolder con el mismo `ID` valores como los controles ContentPlaceHolder `Site.master`. Además, incluye un control ScriptManager, que es necesario para las páginas en nuestro sitio Web que utilizan el marco de AJAX de ASP.NET.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Probar la nueva página maestra

Para probar esta nueva actualización de la página principal la `BasePage` la clase `OnPreInit` método para que el `MasterPageFile` se asigna el valor de propiedad `"~/Alternate.maser"` y, a continuación, visite el sitio Web. Cada página debería funcionar sin errores excepto dos: `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx`. Adición de un producto a DetailsView en `~/Admin/AddProduct.aspx` da como resultado un `NullReferenceException` desde la línea de código que intenta establecer la página maestra `GridMessageText` propiedad. Cuando se visita `~/Admin/Products.aspx` una `InvalidCastException` se produce al cargar la página con el mensaje: "no se puede convertir el objeto de tipo ' ASP.alternate\_maestra ' al tipo ' ASP.site\_maestra '."

Estos errores se producen porque el `Site.master` clase de código subyacente incluye eventos públicos, propiedades y métodos que no estén definidos en `Alternate.master`. La parte del marcado de estas dos páginas tienen un `@MasterType` directiva que hace referencia el `Site.master` página maestra.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Además, ovládacího prvku DetailsView `ItemInserted` controlador de eventos en `~/Admin/AddProduct.aspx` incluye código que convierte débilmente tipadas `Page.Master` propiedad a un objeto de tipo `Site`. El `@MasterType` directiva (de este modo se usa) y la conversión en el `ItemInserted` controlador de eventos se acopla estrechamente la `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas a la `Site.master` página maestra.

Para interrumpir este estrecho acoplamiento que tengamos `Site.master` y `Alternate.master` derivan de una clase base común que contiene las definiciones de los miembros públicos. A continuación, podemos actualizar el `@MasterType` directiva para hacer referencia a este tipo base común.

### <a name="creating-a-custom-base-master-page-class"></a>Creación de una clase de página maestra de Base personalizada

Agregue un nuevo archivo de clase para el `App_Code` carpeta denominada `BaseMasterPage.vb` y hacer que se derivan de `System.Web.UI.MasterPage`. Es necesario definir la `RefreshRecentProductsGrid` método y el `GridMessageText` propiedad en `BaseMasterPage`, pero simplemente no podemos mover ellos desde `Site.master` dado que estos miembros funcionan con los controles Web que son específicos de la `Site.master` página maestra (el `RecentProducts` GridView y `GridMessage` etiqueta).

Lo que debemos hacer es configurar `BaseMasterPage` de tal manera que estos miembros se definen existe, pero se implementan en realidad por `BaseMasterPage`de las clases derivadas (`Site.master` y `Alternate.master`). Este tipo de herencia es posible al marcar la clase como `MustInherit` y sus miembros como `MustOverride`. En resumen, agregar estas palabras clave a la clase y sus dos miembros anuncia que `BaseMasterPage` no implementado `RefreshRecentProductsGrid` y `GridMessageText`, pero que quede de sus clases derivadas.

También es necesario definir la `PricesDoubled` eventos en `BaseMasterPage` y proporcionan un medio por las clases derivadas para generar el evento. El patrón que se usa en .NET Framework para facilitar este comportamiento es crear un evento público en la clase base y agregue un método reemplazable, protegido denominado `OnEventName`. Las clases derivadas, a continuación, pueden llamar a este método para generar el evento o pueden invalidarla para ejecutar código inmediatamente antes o después de que se genera el evento.

Actualización de su `BaseMasterPage` clase para que contenga el código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

A continuación, vaya a la `Site.master` código subyacente de clase y hacer que se derivan de `BaseMasterPage`. Dado que `BaseMasterPage` contiene los miembros marcados `MustOverride` es necesario invalidar esos miembros aquí en `Site.master`. Agregar el `Overrides` palabra clave a las definiciones de método y propiedad. Actualizar también el código que genera el `PricesDoubled` eventos en el `DoublePrice` del botón `Click` controlador de eventos con una llamada a la clase base `OnPricesDoubled` método.

Después de estas modificaciones la `Site.master` clase de código subyacente debe contener el código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

También necesitamos actualizar `Alternate.master`de clase de código subyacente para derivar de `BaseMasterPage` e invalide los dos `MustOverride` miembros. Sin embargo, dado `Alternate.master` no contiene un control GridView que se enumeran los productos más recientes ni una etiqueta que muestra un mensaje después de un producto nuevo se agrega a la base de datos, estos métodos no es necesario hacer nada.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Hacer referencia a la clase de página maestra de Base

Ahora que hemos completado la `BaseMasterPage` clase y tiene nuestros dos páginas maestras ampliándolo, el último paso consiste en actualizar el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas para hacer referencia a este tipo común. Empiece por cambiar la `@MasterType` la directiva en ambas páginas desde:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

A:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

En lugar de hacer referencia a una ruta de acceso de archivo, el `@MasterType` propiedad ahora hace referencia el tipo base (`BaseMasterPage`). Por lo tanto, fuertemente tipadas `Master` propiedad utilizada en las clases de código subyacente de las dos páginas ahora es de tipo `BaseMasterPage` (en lugar de tipo `Site`). Con este cambio en lugar de volver a visitar `~/Admin/Products.aspx`. Anteriormente, Esto resultó en un error de conversión porque la página está configurada para usar el `Alternate.master` página maestra, pero la `@MasterType` directiva hace referencia a la `Site.master` archivo. Pero ahora la página se representa sin errores. Esto es porque el `Alternate.master` página maestra se puede convertir en un objeto de tipo `BaseMasterPage` (ya que extiende).

Hay un pequeño cambio que debe realizarse en `~/Admin/AddProduct.aspx`. El control DetailsView `ItemInserted` controlador de eventos usa ambos fuertemente tipadas `Master` propiedad y débilmente tipadas `Page.Master` propiedad. Se ha corregido la referencia fuertemente tipada cuando se actualiza el `@MasterType` directiva, pero aun así deberá actualizar la referencia fuertemente tipado. Reemplace la línea de código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Por lo siguiente, que convierte `Page.Master` al tipo base:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Paso 4: Determinar qué página maestra para enlazar a las páginas de contenido

Nuestro `BasePage` clase actualmente establece todas las páginas de contenido `MasterPageFile` propiedades en un valor codificado de forma rígida en la fase de PreInit del ciclo de vida de página. Podemos actualizar este código para basar la página maestra en algún factor externo. Quizás la página principal para cargar depende de las preferencias del usuario que ha iniciado sesión actualmente. En ese caso, se tendría que escribir código el `OnPreInit` método `BasePage` que busca las preferencias de página principal del usuario está visitando.

Vamos a crear una página web que permite al usuario elegir qué página maestra que se usará - `Site.master` o `Alternate.master` - y guarde esta opción en una variable de sesión. Empiece por crear una nueva página web en el directorio raíz denominado `ChooseMasterPage.aspx`. Al crear esta página (o cualquier otras contenidas páginas aquí en adelante) no es necesario enlazar a una página maestra porque la página principal se establece mediante programación en `BasePage`. Sin embargo, si no se enlaza la nueva página a una página maestra, a continuación, marcado declarativo de la página nueva de forma predeterminada contiene un formulario Web Forms y otro contenido proporcionado por la página maestra. Deberá reemplazar manualmente este marcado con los controles de contenido adecuados. Por ese motivo, le resulte más fácil enlazar la nueva página ASP.NET a una página maestra.

> [!NOTE]
> Dado que `Site.master` y `Alternate.master` tener el mismo conjunto de controles ContentPlaceHolder no importa qué página maestra que elige al crear la nueva página de contenido. Para mantener la coherencia, sugiero usar `Site.master`.


[![Agregue una nueva página de contenido al sitio Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figura 05**: agregar una nueva página de contenido al sitio Web ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image15.png))


Actualización de la `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` de la lección páginas maestras y ASP.NET AJAX:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Antes de agregar contenido a la `ChooseMasterPage.aspx` página dedique unos minutos a actualizar la clase de código subyacente de la página para que se derive de `BasePage` (en lugar de `System.Web.UI.Page`). A continuación, agregue un control DropDownList para la página, establezca su `ID` propiedad `MasterPageChoice`, y agregue dos ListItems con el `Text` valores de "~ / Site.master" y "~ / Alternate.master".

Agregue un control Button Web a la página y establezca su `ID` y `Text` propiedades a `SaveLayout` y "Guardar alternativas de diseño", respectivamente. En este punto marcado declarativo de la página debe ser similar al siguiente:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Cuando primero se visita la página, es necesario mostrar la elección del usuario actualmente seleccionado de página maestra. Crear un `Page_Load` controlador de eventos y agregue el código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

El código anterior se ejecuta solo en la primera visita de página (y no en los postbacks subsiguientes). En primer lugar comprueba si la variable de sesión `MyMasterPage` existe. Si es así, intenta buscar la coincidencia ListItem en el `MasterPageChoice` DropDownList. Si se encuentra un coincidencia ListItem, su `Selected` propiedad está establecida en `True`.

También necesitamos código que guarda la elección del usuario en el `MyMasterPage` variable de sesión. Crear un controlador de eventos para el `SaveLayout` del botón `Click` eventos y agregue el código siguiente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> En el momento en el `Click` controlador de eventos que se ejecuta en el postback, ya se ha seleccionado la página maestra. Por lo tanto, la selección del usuario lista desplegable no entrarán en vigor hasta que se visite la página siguiente. El `Response.Redirect` obliga al explorador para volver a solicitar `ChooseMasterPage.aspx`.


Con el `ChooseMasterPage.aspx` página completa, nuestra última tarea consiste en tener `BasePage` asignar el `MasterPageFile` en función del valor de propiedad el `MyMasterPage` variable de sesión. Si no se establece la variable de sesión tienen `BasePage` de forma predeterminada `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> He movido el código que asigna el `Page` del objeto `MasterPageFile` propiedad fuera de la `OnPreInit` controlador de eventos y en dos métodos independientes. Este primer método, `SetMasterPageFile`, asigna la `MasterPageFile` propiedad en el valor devuelto por el segundo método, `GetMasterPageFileFromSession`. Se ha marcado el `SetMasterPageFile` método `Overridable` para que futuras clases que extienden `BasePage` opcionalmente, puede invalidar para implementar la lógica personalizada, si es necesario. Vamos a ver un ejemplo de invalidación `BasePage`del `SetMasterPageFile` propiedad en el siguiente tutorial.


Con este código en su lugar, visite la `ChooseMasterPage.aspx` página. Inicialmente, el `Site.master` página maestra está seleccionado (vea la figura 6), pero el usuario puede seleccionar una página maestra diferente en la lista desplegable.


[![Se muestran las páginas de contenido mediante la página maestra Site.master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figura 06**: páginas de contenido son muestra utilizando el `Site.master` página maestra ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Ahora se muestran las páginas de contenido mediante la página maestra Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figura 07**: páginas de contenido son ahora se muestran con el `Alternate.master` página maestra ([haga clic aquí para ver imagen en tamaño completo](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Resumen

Cuando se visita una página de contenido, sus controles de contenido se fusionan con los controles ContentPlaceHolder de su página principal. Página principal de la página de contenido se indica mediante el `Page` la clase `MasterPageFile` propiedad, que se asigna a la `@Page` la directiva `MasterPageFile` atributo durante la fase de inicialización. Como este tutorial se ha mostrado, podemos asignamos un valor para el `MasterPageFile` propiedad siempre que hacerlo antes del final de la fase de PreInit. Posibilidad de especificar mediante programación la página maestra, abre la puerta para escenarios más avanzados, como enlazar dinámicamente una página de contenido a una página maestra en función de factores externos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Diagrama de ciclo de vida de la página ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Información general sobre el ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Información general de máscaras y temas de ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas maestras: Sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [Temas en ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Suchi Banerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-asp-net-ajax-vb.md)
> [Siguiente](nested-master-pages-vb.md)
