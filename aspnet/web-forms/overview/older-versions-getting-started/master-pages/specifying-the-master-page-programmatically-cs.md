---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Especificar la página maestra mediante programación (C#) | Documentos de Microsoft
author: rick-anderson
description: Examina la configuración de página maestra de la página de contenido mediante programación a través del controlador de eventos PreInit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2294ee2e58e55901d77958e7cf45dd74fc2a1187
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891157"
---
<a name="specifying-the-master-page-programmatically-c"></a>Especificar la página maestra mediante programación (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Examina la configuración de página maestra de la página de contenido mediante programación a través del controlador de eventos PreInit.


## <a name="introduction"></a>Introducción

Desde el ejemplo inaugural de [ *crear una páginas de todo el sitio diseño utilizando Master*](creating-a-site-wide-layout-using-master-pages-cs.md), todo el contenido páginas han hecho referencia a su página maestra mediante declaración a través de la `MasterPageFile` atributo en el `@Page`directiva. Por ejemplo, la siguiente `@Page` directiva vincula la página de contenido a la página maestra `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

El [ `Page` clase](https://msdn.microsoft.com/library/system.web.ui.page.aspx) en el `System.Web.UI` espacio de nombres incluye una [ `MasterPageFile` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que devuelve la ruta de acceso a la página principal de la página de contenido; es esta propiedad establecido por el `@Page` directiva. Esta propiedad también puede utilizarse para especificar mediante programación el contenido de página principal. Este enfoque es útil si desea asignar dinámicamente la página maestra en función de factores externos, como el usuario visita la página.

En este tutorial se agregará una segunda página maestra a nuestro sitio Web y decide qué página maestra que se usa en tiempo de ejecución de forma dinámica.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Paso 1: Un vistazo el ciclo de vida de la página

Cada vez que llega una solicitud en el servidor web de una página ASP.NET que es una página de contenido, el motor ASP.NET debe fusible la página contenido controles ContentPlaceHolder correspondiente del controles en la página maestra. Esta fusión crea una jerarquía de control único que, a continuación, puede continuar durante el ciclo de vida de página típico.

La figura 1 muestra esta fusión. Paso 1 en la figura 1 muestra el contenido inicial y las jerarquías de control de página maestra. En la parte final de la fase de PreInit el contenido en la página se agregan a la ContentPlaceHolders correspondiente en la página maestra (paso 2). Después de este fusion, la página maestra actúa como la raíz de la jerarquía de control combinados. Esto fusionada control jerarquía, a continuación, se agrega a la página para generar la jerarquía de control finalizadas (paso 3). El resultado neto es que la jerarquía de control de la página incluye la jerarquía de control combinados.


[![La página maestra y las jerarquías de Control de la página de contenido están fusionada próximas durante la fase de PreInit](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Figura 01**: jerarquías de Control de la página maestra y la página de contenido están fusionada próximas durante la fase de PreInit ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Paso 2: Establecer el`MasterPageFile`propiedad desde el código

¿Qué página maestra partakes en esta fusión depende del valor de la `Page` del objeto `MasterPageFile` propiedad. Establecer el `MasterPageFile` de atributo en el `@Page` directiva tiene el efecto neto de asignar el `Page`del `MasterPageFile` propiedad durante la fase de inicialización, que es la primera etapa del ciclo de vida de la página. También podemos establecer esta propiedad mediante programación. Sin embargo, es imperativo que esta propiedad se establece antes de la fusión en la figura 1 realiza.

Al principio de la fase de PreInit el `Page` objeto genera su [ `PreInit` eventos](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) y llama a su [ `OnPreInit` método](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para establecer la página maestra mediante programación, a continuación, podemos o crear un controlador de eventos para el `PreInit` eventos o invalidar la `OnPreInit` método. Echemos un vistazo a ambos enfoques.

Comience abriendo `Default.aspx.cs`, el archivo de clase de código subyacente para la página principal de nuestro sitio. Agregar un controlador de eventos de la página `PreInit` eventos escribiendo en el código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Desde aquí podemos establecer la `MasterPageFile` propiedad. Actualice el código para que asigna el valor "~ / Site.master" para el `MasterPageFile` propiedad.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Si establece un punto de interrupción y de inicio con la depuración, verá que cada vez que la `Default.aspx` se visita la página o cada vez que hay una devolución de datos a esta página, el `Page_PreInit` ejecuta el controlador de eventos y el `MasterPageFile` propiedad se asigna a "~ / Site.master".

Como alternativa, puede invalidar la `Page` la clase `OnPreInit` método y establezca la `MasterPageFile` propiedad no existe. En este ejemplo, vamos a no establezca la página maestra en una página determinada, sino que de `BasePage`. Recuerde que hemos creado una clase de página base personalizada (`BasePage`) en el [ *especificar el título, etiquetas Meta y otros encabezados de HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial. Actualmente `BasePage` invalida la `Page` la clase `OnLoadComplete` método, que define la página `Title` propiedad en función de los datos del mapa del sitio. Vamos a actualizar `BasePage` para invalidar el `OnPreInit` método para especificar mediante programación la página maestra.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Dado que todas las páginas de nuestro contenido derivan de `BasePage`, todos ellos tengan ahora su página maestra que se asigna mediante programación. En este momento la `PreInit` controlador de eventos de `Default.aspx.cs` es superflua; no dude en quitarlo.

### <a name="what-about-thepagedirective"></a>¿Qué hay de la`@Page`directiva?

Lo que puede resultar un poco confuso es que las páginas de contenido `MasterPageFile` ahora se especifican propiedades en dos lugares: mediante programación en el `BasePage` la clase `OnPreInit` método así como a través del `MasterPageFile` atributo en cada página de contenido `@Page` directiva.

La primera fase del ciclo de vida de la página es la fase de inicialización. Durante esta fase la `Page` del objeto `MasterPageFile` propiedad se asigna el valor de la `MasterPageFile` atributo el `@Page` directiva (si se proporciona). La fase de PreInit sigue a la fase de inicialización y aquí es donde se establece mediante programación el `Page` del objeto `MasterPageFile` propiedad, con lo que se sobrescriba el valor asignado de la `@Page` directiva. Dado que estamos estableciendo el `Page` del objeto `MasterPageFile` propiedad mediante programación, se puede quitar el `MasterPageFile` de atributo de la `@Page` directivas sin afectar a la experiencia del usuario final. Para convencer a sí mismo de esto, voy a quitar el `MasterPageFile` de atributo de la `@Page` directiva `Default.aspx` y, a continuación, visite la página a través de un explorador. Como cabría esperar, el resultado es el mismo que antes de que se ha quitado el atributo.

Si el `MasterPageFile` propiedad se establece a través de la `@Page` directiva o mediante programación es insignificante en experiencia del usuario final. Sin embargo, el `MasterPageFile` de atributo en el `@Page` directiva se usa Visual Studio en tiempo de diseño para generar la WYSIWYG vista en el diseñador. Si vuelve a `Default.aspx` en Visual Studio y navegue hasta el diseñador verá el mensaje "error de página maestra: la página tiene controles que requieren una referencia de página maestra, pero no se especifica ninguno" (consulte la figura 2).

En resumen, debe dejar el `MasterPageFile` de atributo en el `@Page` directiva para disfrutar de una experiencia de tiempo de diseño en Visual Studio.


[![Visual Studio usa el @Page MasterPageFile atributo de la directiva para representar la vista de diseño](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Figura 02**: Visual Studio usa el `@Page` la directiva `MasterPageFile` atributo para representar la vista de diseño ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Paso 3: Crear una página maestra alternativa

Dado un contenido de página principal puede establecerse mediante programación en tiempo de ejecución es posible cargar dinámicamente una página maestra determinada en función de algunos criterios externos. Esta funcionalidad puede ser útil en situaciones donde diseño del sitio necesita varían en función de usuario. Por ejemplo, una aplicación web de motor de blog puede permitir que sus usuarios elegir un diseño para su blog, donde cada diseño está asociada con una página maestra diferente. En tiempo de ejecución, cuando un visitante está viendo el blog de un usuario, la aplicación web necesitaría determinar el diseño del blog y asociar dinámicamente la página maestra correspondiente a la página de contenido.

Vamos a examinar cómo cargar dinámicamente una página maestra en tiempo de ejecución en función de algunos criterios externos. Actualmente, nuestro sitio Web contiene una sola página maestra (`Site.master`). Necesitamos otra página maestra para ilustrar la elección de una página maestra en tiempo de ejecución. Este paso se centra en crear y configurar la nueva página maestra. Paso 4 examina determinar qué página maestra que se usa en tiempo de ejecución.

Crear una nueva página maestra en la carpeta raíz con el nombre `Alternate.master`. Agregar una nueva hoja de estilos para el sitio Web denominado `AlternateStyles.css`.


[![Agregar otra página maestra y CSS de archivos al sitio Web](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Figura 03**: agregar otra página maestra y archivo CSS para el sitio Web ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image9.png))


He diseñado el `Alternate.master` página maestra tenga el título que se muestra en la parte superior de la página, centrado y sobre un fondo azul marino. He suprimirse de la columna izquierda y mover ese contenido bajo el `MainContent` control ContentPlaceHolder, que ahora abarca todo el ancho de la página. Además, nixed la lista de lecciones sin ordenar y sustituirla por otra con una lista horizontal anterior `MainContent`. También actualizan las fuentes y los colores utilizados en la página maestra (y, por extensión, las páginas de contenido). La figura 4 muestra `Default.aspx` cuando se usa el `Alternate.master` página maestra.

> [!NOTE]
> ASP.NET incluye la capacidad para definir *temas*. Un tema es una colección de imágenes, archivos CSS y relacionadas con el estilo Web control valores de propiedades que pueden aplicarse a una página en tiempo de ejecución. Los temas son la mejor opción si los diseños de su sitio difieren solo en las imágenes mostradas y sus reglas de CSS. Si los diseños de diferencian más considerablemente, como el uso de controles Web diferentes o tiene un diseño totalmente diferente, deberá usar páginas maestras independientes. Consulte la sección más información al final de este tutorial para obtener más información acerca de los temas.


[![Nuestras páginas de contenido pueden usar ahora un nuevo aspecto y diseño](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Figura 04**: nuestras páginas de contenido pueden usar ahora un nuevo aspecto ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image12.png))


Cuando se fusionan el maestro y marcado de páginas de contenido, la `MasterPage` clase comprobaciones para asegurarse de que el contenido de cada control en la página de contenido hace referencia a un control ContentPlaceHolder en la página maestra. Se produce una excepción si se encuentra un control de contenido que hace referencia a un ContentPlaceHolder inexistente. En otras palabras, es imperativo que la página maestra que se asigna a la página de contenido tiene un ContentPlaceHolder para cada control en la página de contenido de contenido.

La `Site.master` página maestra incluye cuatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algunas de las páginas de contenido en nuestro sitio Web son simplemente uno o dos controles de contenido; otros incluyen un control de contenido para cada uno de los ContentPlaceHolders disponibles. Si la nueva página maestra (`Alternate.master`) nunca estén asignados a esas páginas de contenido que tienen controles de contenido para todos los ContentPlaceHolders en `Site.master` es fundamental que `Alternate.master` también incluyen los mismos controles ContentPlaceHolder como `Site.master`.

Para obtener la `Alternate.master` página maestra para tener un aspecto similar para extraer (consulte la figura 4), de inicio mediante la definición de estilos de la página maestra en la `AlternateStyles.css` hoja de estilos. Agregue las siguientes reglas en `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

A continuación, agregue el siguiente marcado declarativo para `Alternate.master`. Como puede ver, `Alternate.master` contiene cuatro controles ContentPlaceHolder con el mismo `ID` valores como los controles ContentPlaceHolder en `Site.master`. Además, incluye un control ScriptManager, que es necesario para esas páginas en nuestro sitio Web que utilizan el marco de AJAX de ASP.NET.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Probar la nueva página maestra

Para probar esta nueva actualización de página maestra el `BasePage` la clase `OnPreInit` método para que la `MasterPageFile` propiedad se asigna el valor "~ / Alternate.master" y, a continuación, visite el sitio Web. Todas las páginas deben funcionar sin errores excepto dos: `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx`. Agregar un producto a DetailsView en `~/Admin/AddProduct.aspx` da como resultado un `NullReferenceException` desde la línea de código que intenta establecer la página maestra `GridMessageText` propiedad. Al visitar `~/Admin/Products.aspx` una `InvalidCastException` se produce al cargar la página con el mensaje: "no se puede convertir un objeto de tipo ' ASP.alternate\_maestra ' al tipo ' ASP.site\_maestra '."

Estos errores se producen porque el `Site.master` clase de código subyacente incluye eventos públicos, propiedades y métodos que no estén definidos en `Alternate.master`. La parte de marcado de estas dos páginas tienen un `@MasterType` directiva que hace referencia la `Site.master` página maestra.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Del Además, DetailsView `ItemInserted` controlador de eventos de `~/Admin/AddProduct.aspx` incluye código que convierte débilmente tipadas `Page.Master` propiedad a un objeto de tipo `Site`. El `@MasterType` directiva (usado de este modo) y la conversión en el `ItemInserted` controlador de eventos estrechamente la `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas a la `Site.master` página maestra.

Para interrumpir este podemos tenemos el acoplamiento apretado `Site.master` y `Alternate.master` derivan de una clase base común que contiene las definiciones de los miembros públicos. A continuación, podemos actualizar el `@MasterType` directiva para hacer referencia a este tipo base común.

### <a name="creating-a-custom-base-master-page-class"></a>Crear una clase de página maestra de Base personalizada

Agregue un nuevo archivo de clase para la `App_Code` carpeta denominada `BaseMasterPage.cs` y hacer que se derivan de `System.Web.UI.MasterPage`. Es necesario definir la `RefreshRecentProductsGrid` (método) y la `GridMessageText` propiedad en `BaseMasterPage`, pero simplemente no mover ellos desde `Site.master` dado que estos miembros funcionan con los controles Web que son específicos de la `Site.master` página maestra (el `RecentProducts` GridView y `GridMessage` etiqueta).

Lo que debemos hacer es configurar `BaseMasterPage` de tal manera que estos miembros se definen no existe, pero se implementan en realidad por `BaseMasterPage`de las clases derivadas (`Site.master` y `Alternate.master`). Este tipo de herencia es posible al marcar la clase y sus miembros como `abstract`. En resumen, agregar el `abstract` palabra clave a estos dos miembros anuncia que `BaseMasterPage` no implementado `RefreshRecentProductsGrid` y `GridMessageText`, pero dicha sus clases derivadas.

También es necesario definir la `PricesDoubled` evento en `BaseMasterPage` y proporcionan un medio por las clases derivadas para generar el evento. El patrón que se usa en .NET Framework para facilitar este comportamiento consiste en crear un evento público en la clase base y agregar protegido, `virtual` método denominado `OnEventName`. Las clases derivadas, a continuación, pueden llamar a este método para generar el evento o pueden invalidar para ejecutar código inmediatamente antes o después de que se genera el evento.

Actualización de su `BaseMasterPage` clase para que contenga el código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

A continuación, vaya a la `Site.master` código subyacente de clase y hacer que se derivan de `BaseMasterPage`. Dado que `BaseMasterPage` es `abstract` tenemos que las reemplazan a `abstract` miembros aquí en `Site.master`. Agregar el `override` palabra clave a las definiciones de método y propiedad. Actualizar el código que genera el `PricesDoubled` evento en el `DoublePrice` del botón `Click` controlador de eventos con una llamada a la clase base `OnPricesDoubled` método.

Después de estas modificaciones la `Site.master` clase de código subyacente debe contener el código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

También es necesario actualizar `Alternate.master`de clase de código subyacente para derivar de `BaseMasterPage` e invalide los dos `abstract` miembros. Sin embargo, dado `Alternate.master` no contiene una GridView que enumera los productos más recientes ni una etiqueta que se muestra un mensaje después de un nuevo producto se agrega a la base de datos, estos métodos no es necesario hacer nada.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Hacer referencia a la clase de página maestra de Base

Ahora que hemos completado la `BaseMasterPage` clase y tiene nuestro dos páginas maestras ampliarlo, el último paso consiste en actualizar el `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` páginas para hacer referencia a este tipo común. Empiece por cambiar la `@MasterType` la directiva en ambas páginas de:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

A:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

En lugar de hacer referencia a una ruta de acceso de archivo, el `@MasterType` propiedad ahora hace referencia al tipo base (`BaseMasterPage`). Por lo tanto, el fuertemente tipado `Master` propiedad utilizado en las clases de código subyacente de ambas páginas ahora es de tipo `BaseMasterPage` (en lugar de tipo `Site`). Con este cambio en su lugar, volver a visitar `~/Admin/Products.aspx`. Anteriormente, esto ha provocado un error de conversión debido a la página está configurada para usar el `Alternate.master` página maestra, pero la `@MasterType` directiva hace referencia a la `Site.master` archivo. Pero ahora la página se representa sin errores. Esto es porque el `Alternate.master` página maestra puede convertirse a un objeto de tipo `BaseMasterPage` (desde que extiende).

Hay un pequeño cambio que debe hacerse en `~/Admin/AddProduct.aspx`. El control DetailsView `ItemInserted` controlador de eventos utiliza tanto la fuertemente tipado `Master` propiedad y débilmente tipadas `Page.Master` propiedad. Se ha corregido la referencia fuertemente tipada al se han actualizado la `@MasterType` directiva, pero que aún necesite actualizar la referencia imprecisa. Reemplace la línea de código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Con los siguientes valores, que convierte `Page.Master` al tipo base:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Paso 4: Determinar qué página maestra para enlazar a las páginas de contenido

Nuestro `BasePage` clase actualmente establece todas las páginas de contenido `MasterPageFile` propiedades en un valor codificado de forma rígida en la fase de PreInit del ciclo de vida de página. Podemos actualizar este código para basar la página maestra en algún factor externo. Quizás la página maestra para cargar depende de las preferencias del usuario que ha iniciado sesión actualmente. En ese caso, se tendría que escribir código el `OnPreInit` método `BasePage` que busca las preferencias de página principal del usuario actualmente visitando.

Vamos a crear una página web que permite al usuario elegir qué página maestra que se usará - `Site.master` o `Alternate.master` - y guarde esta opción en una variable de sesión. Empiece por crear una nueva página web en el directorio raíz denominado `ChooseMasterPage.aspx`. Al crear esta página (o cualquier otros contenidos páginas de ahora en adelante) no es necesario enlazarla a una página maestra porque la página maestra se establece mediante programación `BasePage`. Sin embargo, si no enlaza la nueva página a una página maestra, a continuación, marcado declarativo de la página nueva predeterminado contiene un formulario Web Forms y otro contenido proporcionado por la página maestra. Debe reemplazar manualmente este marcado con los controles de contenido adecuados. Por esta razón, si le resulta más fácil enlazar la nueva página ASP.NET a una página maestra.

> [!NOTE]
> Dado que `Site.master` y `Alternate.master` tener el mismo conjunto de controles ContentPlaceHolder no importa qué página maestra que elegir al crear la nueva página de contenido. Para mantener la coherencia, indicaría utilizando `Site.master`.


[![Agregue una nueva página de contenido para el sitio Web](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Figura 05**: agregar una nueva página de contenido para el sitio Web ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image15.png))


Actualización de la `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` para la lección páginas maestras y AJAX de ASP.NET:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Antes de agregar cualquier contenido en el `ChooseMasterPage.aspx` página Tómese un momento para actualizar la clase de código subyacente de la página para que se derive de `BasePage` (en lugar de `System.Web.UI.Page`). A continuación, agregue un control DropDownList a la página, establezca su `ID` propiedad `MasterPageChoice`, y agregar dos ListItems con el `Text` valores de "~ / Site.master" y "~ / Alternate.master".

Agregue un control Button Web a la página y establezca su `ID` y `Text` propiedades para `SaveLayout` y "Guardar alternativas de diseño", respectivamente. En este momento marcado declarativo de la página debe ser similar al siguiente:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Cuando se visita la página por primera vez es necesario mostrar la elección del usuario actualmente seleccionado de página maestra. Crear un `Page_Load` controlador de eventos y agregue el código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

El código anterior se ejecuta sólo en la primera visita de página (y no en postbacks posteriores). En primer lugar comprueba para ver si la variable de sesión `MyMasterPage` existe. Si es así, intenta buscar la coincidencia ListItem en el `MasterPageChoice` DropDownList. Si se encuentra un coincidencia ListItem, su `Selected` propiedad está establecida en `true`.

También es necesario código que guarda la elección del usuario en el `MyMasterPage` variable de sesión. Crear un controlador de eventos para el `SaveLayout` del botón `Click` eventos y agregue el código siguiente:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> En el momento en el `Click` se ejecuta el controlador de eventos en la devolución de datos, ya se ha seleccionado la página maestra. Por lo tanto, la selección del usuario lista desplegable no estará en vigor hasta que se visite la página siguiente. El `Response.Redirect` obliga al explorador volverá a solicitar `ChooseMasterPage.aspx`.


Con el `ChooseMasterPage.aspx` página completa, la última tarea consiste en tener `BasePage` asignar la `MasterPageFile` propiedad basándose en el valor de la `MyMasterPage` variable de sesión. Si no se establece la variable de sesión tienen `BasePage` como valor predeterminado `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Mover el código que asigna el `Page` del objeto `MasterPageFile` propiedad fuera de la `OnPreInit` controlador de eventos y en dos métodos independientes. Este primer método, `SetMasterPageFile`, asigna la `MasterPageFile` propiedad en el valor devuelto por el segundo método, `GetMasterPageFileFromSession`. He realizado los `SetMasterPageFile` método `virtual` para que las clases futuras se extienden `BasePage` opcionalmente, puede invalidar para implementar la lógica personalizada, si es necesario. Veremos un ejemplo de cómo reemplazar `BasePage`del `SetMasterPageFile` propiedad en el tutorial siguiente.


Con este código en su lugar, visite la `ChooseMasterPage.aspx` página. Inicialmente, el `Site.master` página maestra esté seleccionado (vea la figura 6), pero el usuario puede seleccionar una página maestra diferente en la lista desplegable.


[![Páginas de contenido se muestran con la página maestra Site.master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Figura 06**: páginas de contenido son muestra utilizando el `Site.master` página maestra ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![Páginas de contenido se muestran ahora con la página principal de Alternate.master](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Figura 07**: páginas de contenido son ahora muestra con el `Alternate.master` página maestra ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Resumen

Cuando se visita una página de contenido, sus controles de contenido se fusionan con su maestro ContentPlaceHolder controles de la página. Página principal de la página de contenido se indica mediante el `Page` la clase `MasterPageFile` propiedad, que se asigna a la `@Page` la directiva `MasterPageFile` atributo durante la fase de inicialización. Como este tutorial se ha explicado, se puede asignar un valor a la `MasterPageFile` propiedad siempre que hacerlo antes del final de la fase de PreInit. Posibilidad de especificar mediante programación la página maestra, abre la puerta a escenarios más avanzados, como enlazar dinámicamente una página de contenido a una página maestra en función de factores externos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Diagrama de ciclo de vida de la página ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Información general sobre el ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Información general de máscaras y temas de ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas principales: Sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [Temas en ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Suchi Banerjee. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-asp-net-ajax-cs.md)
> [Siguiente](nested-master-pages-cs.md)
