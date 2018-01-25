---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "Características para móviles de ASP.NET MVC 4 | Documentos de Microsoft"
author: Rick-Anderson
description: "Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código al implementar una aplicación de ASP.NET MVC 5 Mobile Web en sitios Web Azure."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: d47d8f61dc7af6e1dc5887338be862ea81d7bb17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Características para móviles de ASP.NET MVC 4
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código en [implementar una aplicación de ASP.NET MVC 5 Mobile Web en sitios Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Este tutorial le enseñará los aspectos básicos de cómo trabajar con características para móviles en una aplicación Web de ASP.NET MVC 4. Para este tutorial, puede usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer o VWD&quot;). Si ya tienes puede usar la versión professional de Visual Studio.

Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Si usas Visual Web Developer 2010, debe instalar [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

También necesitará un emulador de explorador móvil. Funcionarán cualquiera de las siguientes acciones:

- [Emulador de teléfono de Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Esto es el emulador que se utiliza en la mayoría de las capturas de pantalla en este tutorial).
- Cambie la cadena de agente de usuario para emular un iPhone. Vea [esto](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Emulador de dispositivos móviles opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con el agente de usuario establecido en iPhone. Para obtener instrucciones sobre cómo configurar el agente de usuario en Safari para "iPhone", consulte [cómo permiten Safari Finjan es IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) en el blog de David Alison.

Proyectos de Visual Studio con código fuente de C# están disponibles para acompañar este tema:

- [Descarga de proyecto de inicio](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Completar la descarga de proyecto](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Lo que vamos a compilar

Para este tutorial, agregará características para móviles para la aplicación de lista de conferencia simple que se proporciona en el [proyecto inicial](https://go.microsoft.com/fwlink/?LinkId=228307). Captura de pantalla siguiente muestra la página de etiquetas de la aplicación completa tal como se muestra en el [emulador de teléfono de Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Vea [emulador de teclado de asignación para la de Windows Phone](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar la entrada de teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Puede usar Internet Explorer versión 9 o 10, FireFox o Chrome para desarrollar su aplicación móvil estableciendo la [cadena de agente de usuario](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). La siguiente imagen muestra el tutorial completado con Internet Explorer emulando un iPhone. Puede usar las herramientas de desarrollo de Internet Explorer F-12 y [herramienta Fiddler](http://www.fiddler2.com/fiddler2/) para ayudar a depurar la aplicación.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Usan de las plantillas de ASP.NET MVC 4 el HTML5 `viewport` atributo y representación adaptable para mejorar se muestran en los dispositivos móviles.
- Cómo crear vistas específicas de mobile.
- Cómo crear a un modificador de vista que los usuarios de permite activar o desactivar entre una vista móvil y una vista de escritorio de la aplicación.

### <a name="getting-started"></a>Introducción

Descargue la aplicación de lista de conferencia para el proyecto de inicio mediante el siguiente vínculo: [descargar](https://go.microsoft.com/fwlink/?LinkId=228307). A continuación, en el Explorador de Windows, haga clic en el *MvcMobile.zip* de archivos y elija **propiedades**. En el **MvcMobile.zip propiedades** diálogo cuadro, elija la **Unblock** botón. (Desbloqueo impide que una advertencia de seguridad que se produce cuando intenta utilizar un *.zip* archivo que ha descargado desde el web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Haga clic en el *MvcMobile.zip* de archivos y seleccione **extraer todo** para descomprimir el archivo. En Visual Studio, abra el *MvcMobile.sln* archivo.

Presione CTRL + F5 para ejecutar la aplicación, que se mostrará en el Explorador de escritorio. Inicie el emulador de explorador móvil, copie la dirección URL de la aplicación de conferencia en el emulador y, a continuación, haga clic en el **Examinar por etiqueta** vínculo. Si usas el emulador de Windows Phone, haga clic en la barra de dirección URL y presione la tecla Pausa para obtener acceso mediante teclado. La imagen siguiente muestra el *AllTags* vista (desde elegir **Examinar por etiqueta**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

La pantalla es muy legible en un dispositivo móvil. Elija el vínculo ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La vista de la etiqueta ASP.NET es muy confuso. Por ejemplo, el **fecha** columna es muy difícil de leer. Más adelante en el tutorial creará una versión de la *AllTags* vista que está específicamente diseñado para los exploradores móviles y que hará que la presentación sea legible.

Nota: Actualmente hay un error en el motor de almacenamiento en caché móvil. Para las aplicaciones de producción, debe instalar la [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete dato valioso. Vea [ASP.NET MVC 4 Mobile almacenamiento en caché errores fijo](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

## <a name="css-media-queries"></a>Consultas de medios CSS

[Las consultas de medios CSS](http://www.w3.org/TR/css3-mediaqueries/) son una extensión CSS para los tipos de medios. Le permiten crear reglas que invalidan las reglas de CSS de forma predeterminada para exploradores concretos (agentes de usuario). Una regla común de CSS que tenga como destino exploradores móviles consiste en definir el tamaño máximo de pantalla. El *Content\Site.css* archivo que se crea cuando se crea un nuevo proyecto de ASP.NET MVC 4 Internet contiene la siguiente consulta de medios:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Si la ventana del explorador es 850 píxeles o menos, utilizará las reglas de CSS dentro de este bloque de medios. Puede usar las consultas de medios CSS como esta para proporcionar una mejor visualización de contenido HTML en exploradores pequeños (por ejemplo, los exploradores móviles) que las reglas CSS predeterminadas que están diseñados para las pantallas más amplia de los exploradores de escritorio.

## <a name="the-viewport-meta-tag"></a>La etiqueta Meta de la ventanilla

Exploradores móviles más definen un ancho de la ventana de explorador virtual (la *ventanilla*) que es mucho mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles para ajustarse a toda la página web dentro de la pantalla virtual. Los usuarios, a continuación, pueden acercar en contenido interesante. Sin embargo, si establece el ancho de la ventanilla para el ancho de un dispositivo real, sin zoom es necesario, porque el contenido se ajusta en el explorador móvil.

La ventanilla `<meta>` etiqueta en el archivo de diseño de MVC de ASP.NET 4 establece la ventanilla para el ancho del dispositivo. La siguiente línea muestra la ventanilla `<meta>` etiqueta en el archivo de diseño de MVC de ASP.NET 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinar el efecto de las consultas de medios CSS y la etiqueta Meta de la ventanilla

Abra la *Views\Shared\\_Layout.cshtml* archivo en el editor y marque como comentario la ventanilla `<meta>` etiqueta. El marcado siguiente muestra la línea comentada.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra la *MvcMobile\Content\Site.css* en el editor y cambie el ancho máximo de la consulta de medios a cero píxeles. Esto impedirá que las reglas de CSS que se va a usar en exploradores móviles. La siguiente línea muestra la consulta de medios modificado:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Guarde los cambios y vaya a la aplicación de conferencias en un emulador de explorador móvil. El minúsculo texto en la siguiente imagen es el resultado de la eliminación de la ventanilla `<meta>` etiqueta. Con ningún ventanilla `<meta>` etiqueta, el explorador se aleja el ancho de la ventanilla predeterminado (850 píxeles o más amplia de exploradores móviles más.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Deshacer los cambios, quite el comentario de la ventanilla `<meta>` etiqueta en el archivo de diseño y restaurar la consulta de medios a 850 píxeles en el *Site.css* archivo. Guarde los cambios y actualice el explorador móvil para comprobar que se ha restaurado la pantalla preparado para dispositivos móviles.

La ventanilla `<meta>` etiqueta y la consulta de medios CSS no son específicos de ASP.NET MVC 4, y puede aprovechar estas características en cualquier aplicación web. Pero que ahora se compilan en los archivos que se generan cuando se crea un nuevo proyecto de ASP.NET MVC 4.

Para obtener más información acerca de la ventanilla `<meta>` de etiquetas, consulte [un indicador de dos puntos de visión: segunda parte](http://www.quirksmode.org/mobile/viewports2.html).

En la siguiente sección verá cómo proporcionar vistas específicas del explorador de mobile.

## <a name="overriding-views-layouts-and-partial-views"></a>Reemplazar vistas, diseños y las vistas parciales

Una nueva característica importante de ASP.NET MVC 4 es un mecanismo sencillo que le permite invalidar cualquier vista (incluyendo los esquemas y las vistas parciales) para los exploradores móviles en general, para un explorador móvil individual o para cualquier explorador específico. Para proporcionar una vista específica para equipos móviles, puede copiar un archivo de vista y agregar *. Mobile* al nombre de archivo. Por ejemplo, para crear un teléfono móvil *índice* ver, copiar *Views\Home\Index.cshtml* a *Views\Home\Index.Mobile.cshtml*.

En esta sección, creará un archivo de diseño específica para equipos móviles.

Para empezar, copie *Views\Shared\\_Layout.cshtml* a *Views\Shared\\_Layout.Mobile.cshtml*. Abra  *\_Layout.Mobile.cshtml* y cambie el título de **MVC4 conferencia** a **conferencia (móvil)**.

En cada uno de ellos `Html.ActionLink` llamada, quitar "Buscar por" en cada vínculo *ActionLink*. El código siguiente muestra la sección de cuerpo completado del archivo de diseño móvil.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copia la *Views\Home\AllTags.cshtml* del archivo a *Views\Home\AllTags.Mobile.cshtml*. Abra el nuevo archivo y cambie el `<h2>` elemento de "Etiquetas" a "etiquetas (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Ir a la página de etiquetas mediante un explorador de escritorio y el emulador de explorador móvil. El emulador de explorador móvil muestra los dos cambios realizados.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

En cambio, no ha cambiado la presentación del escritorio.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Vistas específicas del explorador

Además de las vistas específicas de escritorio y móviles, puede crear vistas para un explorador individual. Por ejemplo, puede crear vistas que son específicos para el Explorador de iPhone. En esta sección, creará un diseño para el Explorador de iPhone y una versión de iPhone de la *AllTags* vista.

Abra la *Global.asax* de archivos y agregue el código siguiente a la `Application_Start` (método).

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Este código define un nuevo modo de presentación con el nombre "iPhone" que se comparará con cada solicitud entrante. Si la solicitud entrante coincide con la condición definida (es decir, si el agente de usuario contiene la cadena "iPhone"), ASP.NET MVC buscará vistas cuyo nombre contiene el sufijo "iPhone".

En el código, haga clic en `DefaultDisplayMode`, elija **resolver**y, a continuación, elija `using System.Web.WebPages;`. Esto agrega una referencia a la `System.Web.WebPages` espacio de nombres, que es donde el `DisplayModes` y `DefaultDisplayMode` se definen los tipos.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, puede agregar simplemente manualmente la siguiente línea a la `using` sección del archivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

El contenido completo de la *Global.asax* archivo se muestra a continuación.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Guarde los cambios. Copia la *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* del archivo a *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Abra el nuevo archivo y, a continuación, cambie la `h1` encabezado de `Conference (Mobile)` a `Conference (iPhone)`.

Copia la *MvcMobile\Views\Home\AllTags.Mobile.cshtml* del archivo a *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. En el nuevo archivo, cambie la `<h2>` elemento de "etiquetas (M)" para "Tags (iPhone)".

Ejecute la aplicación. Ejecutar un emulador de explorador móvil, asegúrese de que el agente de usuario se establece en "iPhone" y examine el *AllTags* vista. La siguiente captura de pantalla muestra la *AllTags* vista se representa en el [Safari](http://www.apple.com/safari/download/) explorador. Puede descargar Safari para Windows [aquí](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

En esta sección hemos visto cómo crear vistas y diseños de dispositivos móviles y cómo crear diseños y las vistas para dispositivos específicos, como iPhone. En la siguiente sección verá cómo aprovechar jQuery Mobile más vistas móviles atractivos.

## <a name="using-jquery-mobile"></a>Uso de jQuery Mobile

El [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteca proporciona un marco de interfaz de usuario que funciona en todos los principales exploradores móviles. aplica de jQuery Mobile *mejora progresiva* a los exploradores móviles que admiten CSS y JavaScript. Mejora progresiva permite todos los exploradores mostrar el contenido básico de una página web, mientras que permite a los exploradores más eficaces y dispositivos para tener una presentación más completa. Los archivos JavaScript y CSS que se incluyen con jQuery Mobile aplicar estilo a muchos elementos para ajustarse a los exploradores móviles sin realizar ningún cambio de marcado.

En esta sección instalará el *jQuery.Mobile.MVC* paquete de NuGet, que se instala un widget de modificador de vista y móviles de jQuery.

Para iniciar, eliminar el *Shared\\_Layout.Mobile.cshtml* y *Shared\\_Layout.iPhone.cshtml* archivos que creó anteriormente.

Cambiar el nombre de *Views\Home\AllTags.Mobile.cshtml* y *Views\Home\AllTags.iPhone.cshtml* archivos *Views\Home\AllTags.iPhone.cshtml.hide* y  *Views\Home\AllTags.Mobile.cshtml.Hide*. Dado que los archivos ya no tienen un *.cshtml* extensión, no se usará el tiempo de ejecución de ASP.NET MVC para representar la *AllTags* vista.

Instalar el *jQuery.Mobile.MVC* paquete de NuGet haciendo lo siguiente:

1. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**y, a continuación, seleccione **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. En el **Package Manager Console**, escriba`Install-Package jQuery.Mobile.MVC -version 1.0.0`

La siguiente imagen muestra los archivos que se agregan y cambian al proyecto MvcMobile el paquete de NuGet jQuery.Mobile.MVC. Archivos que se agregan [Agregar] ha anexado tras el nombre de archivo. La imagen no muestra el formato GIF y PNG (archivos) que se agregan a la *Content\images* carpeta.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

El paquete de NuGet jQuery.Mobile.MVC instala lo siguiente:

- El *aplicación\_Start\BundleMobileConfig.cs* archivo, que es necesario para hacer referencia a los archivos de JavaScript y CSS de jQuery agregados. Debe seguir las instrucciones siguientes y hacer referencia a la agrupación de móvil definida en ese archivo.
- archivos de jQuery Mobile CSS.
- A `ViewSwitcher` widget de controlador (*Controllers\ViewSwitcherController.cs*).
- archivos de jQuery Mobile JavaScript.
- Un archivo de diseño de un estilo Mobile jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Una vista parcial de modificador de vista *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) que proporciona un vínculo en la parte superior de cada página para cambiar de vista de escritorio a la vista móvil y viceversa.
- Varios*.png* y *.gif* archivos de imagen en el *Content\images* carpeta.

Abra la *Global.asax* de archivos y agregue el código siguiente como la última línea de la `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

El código siguiente muestra la sección completa *Global.asax* archivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Si está utilizando Internet Explorer 9 y no ve el `BundleMobileConfig` línea anteriormente en amarillo resaltado, haga clic en el [botón de vista de compatibilidad](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![imagen del botón de vista de compatibilidad (desactivado)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Imagen del botón de vista de compatibilidad (desactivado)") en Internet Explorer para cambiar el icono de un esquema ![imagen del botón de vista de compatibilidad (desactivado)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "imagen del botón de vista de compatibilidad (desactivado) ") en un color sólido ![imagen del botón de vista de compatibilidad (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "imagen del botón de vista de compatibilidad (on)"). También puede ver este tutorial en FireFox o Chrome.


Abra la *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* de archivos y agregue el siguiente marcado directamente después de la `Html.Partial` llamar a:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

La sección completa *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* archivo se muestra a continuación:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compile la aplicación y en el emulador de explorador móvil, vaya a la *AllTags* vista. Vea lo siguiente:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Puede depurar el código móvil específico por [establecer la cadena de agente de usuario](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para Internet Explorer o Chrome para iPhone y, a continuación, usar las herramientas de desarrollo F-12. Si no se muestra el explorador móvil la **inicio**, **altavoces**, **etiqueta**, y **fecha** vínculos como botones, las referencias a jQuery Mobile scripts y archivos CSS probablemente no son correctos.


Además de los cambios de estilo, consulte **Mostrar vista móvil** y un vínculo que permite cambiar de vista móvil a la vista de escritorio. Elija la **vista de escritorio** se muestra el vínculo y la vista de escritorio.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La vista de escritorio no proporciona una manera para navegar directamente a la vista móvil. Lo corregirá ahora. Abra la *Views\Shared\\_Layout.cshtml* archivo. Justo debajo de la página `body` elemento, agregue el código siguiente, que representa el widget de modificador de vista:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Actualizar el *AllTags* vista en el explorador móvil. Ahora puede desplazarse entre las vistas de escritorio y móviles.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Depurar Nota: puede agregar el código siguiente al final de la Views\Shared\\_ViewSwitcher.cshtml para ayudar a depurar vistas cuando mediante un explorador la cadena de agente de usuario establecido en un dispositivo móvil.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  y agregar el encabezado siguiente para el *Views\Shared\\_Layout.cshtml* archivo.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Vaya a la *AllTags* página en un explorador de escritorio. El widget de modificador de vista no se muestra en un explorador de escritorio, dado que solo se agrega a la página de diseño móviles. Más adelante en el tutorial verá cómo puede agregar el widget de modificador de vista a la vista de escritorio.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Mejora de la lista de los altavoces

En el explorador móvil, seleccione la **altavoces** vínculo. Porque no hay ninguna vista móvil (*AllSpeakers.Mobile.cshtml*), mostrar los altavoces predeterminado (*AllSpeakers.cshtml*) se representa mediante la vista de diseño móvil ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Global puede deshabilitar una vista de (no son móviles) predeterminado de representación dentro de un diseño móvil estableciendo `RequireConsistentDisplayMode` a `true` en el *vistas\\_ViewStart.cshtml* archivo, similar al siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Cuando `RequireConsistentDisplayMode` está establecido en `true`, el diseño móvil (*\_Layout.Mobile.cshtml*) se utiliza únicamente para las vistas móviles. (Es decir, el archivo de vista tiene el formato ***ViewName**. Mobile.cshtml*.) Conviene establecer `RequireConsistentDisplayMode` a `true` si su diseño móvil no funciona bien con las vistas no son móviles. La captura de pantalla siguiente muestra cómo el *altavoces* la página presenta cuando `RequireConsistentDisplayMode` está establecido en `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Puede deshabilitar el modo de presentación coherente en una vista estableciendo `RequireConsistentDisplayMode` a `false` en el archivo de vista. El siguiente marcado en el *Views\Home\AllSpeakers.cshtml* archivo establece `RequireConsistentDisplayMode` a `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Crear una vista móvil altavoces

Como ha visto, el *altavoces* vista es legible, pero los vínculos son pequeños y son difíciles de puntear en un dispositivo móvil. En esta sección, creará un determinado mobile *altavoces* vista que parece una moderna aplicación móvil: muestra grandes, fácil de tap vincula y contiene un cuadro de búsqueda para encontrar rápidamente los altavoces.

Copia *AllSpeakers.cshtml* a *AllSpeakers.Mobile.cshtml*. Abra la *AllSpeakers.Mobile.cshtml* de archivos y quitar el `<h2>` elemento de encabezado.

En el `<ul>` etiqueta, agregue el `data-role` de atributo y establezca su valor en `listview`. Al igual que otros [ `data-*` atributos](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` hace más fácilmente los elementos de lista grande a derivar. Este es el aspecto que tiene el marcado completado:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Actualice el explorador móvil. La vista actualizada tiene el siguiente aspecto:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Aunque se ha mejorado la vista móvil, es difícil la navegación de la larga lista de altavoces. Para solucionar esto, en la `<ul>` etiqueta, agregue el `data-filter` atributo y establézcalo en `true`. El código siguiente muestra el `ul` marcado.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

La siguiente imagen muestra el cuadro de filtro de búsqueda en la parte superior de la página resultante de la `data-filter` atributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Tal y como se escribe cada letra en el cuadro de búsqueda, jQuery Mobile filtra la lista que aparece tal y como se muestra en la imagen siguiente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Mejora de la lista de etiquetas

Al igual que el valor predeterminado *altavoces* vista, la *etiquetas* vista es legible, pero los vínculos son pequeños y difíciles de puntear en un dispositivo móvil. En esta sección, corregirá el *etiquetas* ver la misma manera que se corrigió el *altavoces* vista.

Quitar el &quot;ocultar&quot; sufijo a la la *Views\Home\AllTags.Mobile.cshtml.hide* por lo que es el nombre de archivo *Views\Home\AllTags.Mobile.cshtml*. Abra el archivo cuyo nombre ha cambiado y quite el `<h2>` elemento.

Agregar el `data-role` y `data-filter` atributos a la `<ul>` etiqueta, como se muestra aquí:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

La imagen siguiente muestra la página de etiquetas filtrado por la letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Mejora de la lista de fechas

Puede mejorar la *fechas* ver como ha mejorado la *altavoces* y *etiquetas* vistas, para que resulte más fácil de usar en un dispositivo móvil.

Copia la *Views\Home\AllDates.cshtml* del archivo a *Views\Home\AllDates.Mobile.cshtml*. Abra el nuevo archivo y quitar la `<h2>` elemento.

Agregar `data-role="listview"` a la `<ul>` etiqueta, similar al siguiente:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

La imagen siguiente muestra lo que hace el **fecha** aspecto de página con el `data-role` atributo en su lugar.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) reemplazar el contenido de la *Views\Home\AllDates.Mobile.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Este código agrupa todas las sesiones por días. Crea un separador de lista para cada nuevo día y se muestran todas las sesiones para cada día en un divisor. Este es su aspecto cuando se ejecuta este código:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Mejora de la vista de SessionsTable

En esta sección, creará una vista específica para equipos móviles de sesiones. Los cambios que se realizan será más amplios que en otras vistas que hemos creado.

En el explorador móvil, pulse el **altavoces** , a continuación, escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Pulse la **Scott Hanselman** vínculo.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como puede ver, la pantalla es difícil de leer en un explorador móvil. La columna de fecha es difícil de leer y la columna de etiquetas está fuera de la vista. Para solucionar este problema, copie *Views\Home\SessionsTable.cshtml* a *Views\Home\SessionsTable.Mobile.cshtml*y, a continuación, reemplace el contenido del archivo con el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

El código quita el salón y etiquetas de columnas y formatos verticalmente, el título, altavoces y fecha para que toda esta información es legible en un explorador móvil. La imagen siguiente refleja los cambios de código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Mejora de la vista de SessionByCode

Por último, creará una vista específica para equipos móviles de la *SessionByCode* vista. En el explorador móvil, pulse el **altavoces** , a continuación, escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Pulse la **Scott Hanselman** vínculo. Se muestran las sesiones de Scott Hanselman.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Elija la **una visión general de la pila de amor MS Web** vínculo.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La vista de escritorio de forma predeterminada es correcto, pero se puede mejorar.

Copia la *Views\Home\SessionByCode.cshtml* a *Views\Home\SessionByCode.Mobile.cshtml* y reemplace el contenido de la *Views\Home\SessionByCode.Mobile.cshtml*archivo con el siguiente marcado:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Usa la nueva marca el `data-role` atributo para mejorar el diseño de la vista.

Actualice el explorador móvil. La siguiente imagen refleja los cambios de código que acaba de crear:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup y revisión

Este tutorial presenta las nuevas características de dispositivos móviles de ASP.NET MVC 4 Developer Preview. Las características móviles incluyen:

- La capacidad de invalidar el diseño, vistas y las vistas parciales, tanto globalmente como para obtener una vista individual.
- Control sobre el diseño y el cumplimiento de reemplazo parcial mediante la `RequireConsistentDisplayMode` propiedad.
- Un widget de modificador de vista para dispositivos móviles ve que también se pueden mostrar en vistas de escritorio.
- Compatibilidad con la compatibilidad con exploradores específicos, como el Explorador de iPhone.

## <a name="see-also"></a>Vea también

- [jQuery Mobile](http://jquerymobile.com) sitio.
- [información general sobre Mobile jQuery](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Prácticas recomendadas de la aplicación del Web móvil de W3C recomendación](http://www.w3.org/TR/mwabp/)
- [Recomendación de candidatos de W3C para las consultas de medios](http://www.w3.org/TR/css3-mediaqueries/)
