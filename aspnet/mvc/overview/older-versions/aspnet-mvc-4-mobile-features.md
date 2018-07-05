---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Las características móviles de ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código al implementar una aplicación de ASP.NET MVC 5 Mobile Web en Azure WebSites.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5b029aa7e87f064622d72feacaf7e97ea4da5cca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384821"
---
<a name="aspnet-mvc-4-mobile-features"></a>Características para móviles de ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código en [implementar una aplicación de ASP.NET MVC 5 Mobile Web en Azure websites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Este tutorial le enseñará los aspectos básicos de cómo trabajar con características móviles en una aplicación Web de ASP.NET MVC 4. Para este tutorial, puede usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer o VWD&quot;). Si ya tiene puede usar la versión de Visual Studio professional.

Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Si usas Visual Web Developer 2010, debe instalar [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

También necesitará un emulador de explorador móvil. Funcionará cualquiera de las siguientes acciones:

- [Emulador de teléfono de Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Este es el emulador que se usa en la mayoría de las capturas de pantalla en este tutorial).
- Cambie la cadena de agente de usuario para emular un iPhone. Consulte [esto](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Emulador de opera Mobile](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con el agente de usuario establecido en iPhone. Para obtener instrucciones sobre cómo configurar el agente de usuario en Safari en "iPhone", consulte [cómo permitir que Safari simule que es el IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) en el blog de David Alison.

Proyectos de Visual Studio con código fuente de C# están disponibles para este tema:

- [Descarga del proyecto inicial](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Descarga del proyecto completado](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>¿Qué va a crear

Para este tutorial, agregará características móviles a la aplicación simple de lista de conferencias que se proporciona en el [proyecto inicial](https://go.microsoft.com/fwlink/?LinkId=228307). Captura de pantalla siguiente muestra la página de etiquetas de la aplicación completada, tal como se muestra en el [emulador de teléfono de Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Consulte [emulador de teclado de asignación para la de Windows Phone](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar la entrada de teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Puede usar Internet Explorer versión 9 o 10, FireFox o Chrome para desarrollar su aplicación móvil estableciendo el [cadena user agent](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). La siguiente imagen muestra el tutorial completado mediante Internet Explorer que emula un iPhone. Puede usar las herramientas de desarrollo de Internet Explorer F-12 y [herramienta Fiddler](http://www.fiddler2.com/fiddler2/) para ayudar a depurar la aplicación.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aquí es lo que aprenderá:

- Usan de las plantillas de ASP.NET MVC 4 el HTML5 `viewport` Mostrar atributo y procesamiento adaptable para mejorar en los dispositivos móviles.
- Cómo crear vistas específicas de dispositivos móviles.
- Cómo crear a un modificador de vista ese botón de alternancia de permite a los usuarios entre una vista móvil y una vista de la aplicación de escritorio.

### <a name="getting-started"></a>Introducción

Descargue la aplicación de lista de conferencias para el proyecto de inicio mediante el siguiente vínculo: [descargar](https://go.microsoft.com/fwlink/?LinkId=228307). A continuación, en el Explorador de Windows, haga clic en el *MvcMobile.zip* de archivo y elija **propiedades**. En el **MvcMobile.zip propiedades** diálogo cuadro, elija el **desbloquear** botón. (Desbloqueo evita recibir una advertencia de seguridad que se produce cuando se intenta usar un *.zip* archivo que ha descargado desde el web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Haga clic en el *MvcMobile.zip* de archivo y seleccione **extraer todo** para descomprimir el archivo. En Visual Studio, abra el *MvcMobile.sln* archivo.

Presione CTRL + F5 para ejecutar la aplicación, que se mostrará en el Explorador de escritorio. Iniciar el emulador de explorador móvil, copie la dirección URL de la aplicación de conferencia en el emulador y, a continuación, haga clic en el **explorar por etiqueta** vínculo. Si usa el emulador de Windows Phone, haga clic en la barra de dirección URL y presione la tecla Pausa para obtener acceso mediante el teclado. La imagen siguiente muestra el *AllTags* vista (desde elegir **explorar por etiqueta**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

La pantalla es muy legible en un dispositivo móvil. Elija el vínculo ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La vista de la etiqueta ASP.NET es muy confuso. Por ejemplo, el **fecha** columna es muy difícil de leer. Más adelante en este tutorial creará una versión de la *AllTags* vista que es específicamente para los exploradores móviles y que hará que la presentación legible.

Nota: Actualmente hay un error en el motor de almacenamiento en caché móvil. Para las aplicaciones de producción, debe instalar la [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete de NuGet. Consulte [ASP.NET MVC 4 Mobile almacenamiento en caché errores fijo](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

## <a name="css-media-queries"></a>Consultas de medios CSS

[Las consultas de medios CSS](http://www.w3.org/TR/css3-mediaqueries/) son una extensión a CSS para los tipos de medios. Le permiten crear reglas que invalidan las reglas de CSS de forma predeterminada para exploradores concretos (agentes de usuario). Una regla para CSS que tenga como destino los exploradores móviles común consiste en definir el tamaño máximo de pantalla. El *Content\Site.css* archivo que se crea cuando se crea un nuevo proyecto de ASP.NET MVC 4 Internet contiene la siguiente consulta de medios:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Si la ventana del explorador es 850 píxeles píxeles o menos, utilizará las reglas de CSS dentro de este bloque de medios. Puede usar las consultas de medios CSS como este para ofrecer una mejor visualización de contenido HTML en exploradores pequeño (por ejemplo, los exploradores móviles) a las reglas CSS predeterminadas que están diseñadas para las pantallas más amplias de exploradores de escritorio.

## <a name="the-viewport-meta-tag"></a>La etiqueta Meta de ventanilla

Los exploradores móviles más definen un ancho de la ventana de explorador virtual (la *ventanilla*) que es mucho mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles para ajustarse a la página web completa dentro de la pantalla virtual. Los usuarios pueden, a continuación, acercar contenido interesante. Sin embargo, si establece el ancho de la ventanilla en el ancho de un dispositivo real, sin zoom es necesario, porque el contenido se ajusta en el explorador móvil.

La ventanilla `<meta>` etiqueta en el archivo de diseño de ASP.NET MVC 4 establece la ventanilla en el ancho del dispositivo. La siguiente línea muestra la ventanilla `<meta>` etiqueta en el archivo de diseño de ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinar el efecto de las consultas de medios CSS y la etiqueta Meta de ventanilla

Abra el *Views\Shared\\_Layout.cshtml* archivo en el editor y marque como comentario la ventanilla `<meta>` etiqueta. El marcado siguiente muestra la línea comentada.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra el *MvcMobile\Content\Site.css* en el editor de archivos y cambiar el ancho máximo de la consulta de medios a cero píxeles. Esto impedirá que las reglas de CSS que se usan en los exploradores móviles. La siguiente línea muestra la consulta de medios modificado:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Guarde los cambios y vaya a la aplicación de conferencia en un emulador de explorador móvil. El minúsculo texto en la siguiente imagen es el resultado de la eliminación de la ventanilla `<meta>` etiqueta. Con ningún ventanilla `<meta>` etiqueta, el explorador se aleja el ancho de ventanilla predeterminado (850 píxeles o más ancha para exploradores móviles más.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Deshacer los cambios, quite el comentario de la ventanilla `<meta>` etiquetar en el archivo de diseño y la consulta de medios de restauración a 850 píxeles en la *Site.css* archivo. Guarde los cambios y actualice el explorador móvil para comprobar que se ha restaurado la visualización móvil.

La ventanilla `<meta>` etiqueta y la consulta de medios CSS no son específicos de ASP.NET MVC 4, y puede aprovechar estas características en cualquier aplicación web. Pero ahora están integradas en los archivos que se generan cuando se crea un nuevo proyecto de ASP.NET MVC 4.

Para obtener más información acerca de la ventanilla `<meta>` etiquetar, vea [un relato de dos puntos de visión, la segunda parte](http://www.quirksmode.org/mobile/viewports2.html).

En la sección siguiente verá cómo proporcionar vistas específicas de explorador móvil.

## <a name="overriding-views-layouts-and-partial-views"></a>Invalidación de vistas, diseños y vistas parciales

Una nueva característica significativa en ASP.NET MVC 4 es un mecanismo sencillo que le permite reemplazar cualquier vista (incluidos los diseños y vistas parciales) para los exploradores móviles en general, para un explorador móvil individual o para cualquier explorador específico. Para proporcionar una vista específica para móviles, puede copiar un archivo de vista y agregar *. Mobile* al nombre de archivo. Por ejemplo, para crear un mobile *índice* ver, copiar *Views\Home\Index.cshtml* a *Views\Home\Index.Mobile.cshtml*.

En esta sección, creará un archivo de diseño específicas de dispositivos móviles.

Para empezar, copie *Views\Shared\\_Layout.cshtml* a *Views\Shared\\_Layout.Mobile.cshtml*. Abra  *\_Layout.Mobile.cshtml* y cambie el título de **MVC4 conferencia** a **conferencia (móvil)**.

En cada `Html.ActionLink` llamada, "explorar por" en cada vínculo *ActionLink*. El código siguiente muestra la sección de cuerpo completado del archivo de diseño para dispositivos móviles.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copia el *Views\Home\AllTags.cshtml* archivo *Views\Home\AllTags.Mobile.cshtml*. Abra el archivo nuevo y cambie el `<h2>` elemento de "Tags" a "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Vaya a la página de etiquetas mediante un explorador de escritorio y mediante el emulador de explorador móvil. El emulador de explorador móvil muestra los dos cambios realizados.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

En cambio, la pantalla del escritorio no ha cambiado.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Vistas específicas de explorador

Además de las vistas específicas de escritorio y móviles, puede crear vistas para un explorador individual. Por ejemplo, puede crear vistas que están diseñadas específicamente para el Explorador de iPhone. En esta sección, creará un diseño para el Explorador de iPhone y una versión de iPhone de la *AllTags* vista.

Abra el *Global.asax* archivo y agregue el código siguiente a la `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Este código define un nuevo modo de presentación llamado "iPhone" que se comparará con cada solicitud entrante. Si la solicitud entrante coincide con la condición definida (es decir, si el agente de usuario contiene la cadena "iPhone"), ASP.NET MVC buscará vistas cuyo nombre contiene el sufijo "iPhone".

En el código, haga clic en `DefaultDisplayMode`, elija **resolver**y, a continuación, elija `using System.Web.WebPages;`. Esto agrega una referencia a la `System.Web.WebPages` espacio de nombres, que es donde el `DisplayModes` y `DefaultDisplayMode` se definen los tipos.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, puede agregar manualmente la siguiente línea a la `using` sección del archivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Todo el contenido de la *Global.asax* archivo se muestra a continuación.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Guarde los cambios. Copia el *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* archivo *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Abra el nuevo archivo y, a continuación, cambie el `h1` encabezado desde `Conference (Mobile)` a `Conference (iPhone)`.

Copia el *MvcMobile\Views\Home\AllTags.Mobile.cshtml* archivo *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. En el nuevo archivo, cambie el `<h2>` elemento de "Tags (M)" a "Tags (iPhone)".

Ejecute la aplicación. Ejecutar un emulador de explorador móvil, asegúrese de que su agente de usuario se establece en "iPhone" y vaya a la *AllTags* vista. La siguiente captura de pantalla muestra la *AllTags* representa la vista en el [Safari](http://www.apple.com/safari/download/) explorador. Puede descargar Safari para Windows [aquí](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

En esta sección, hemos visto cómo crear vistas y diseños móviles y cómo crear vistas y diseños para dispositivos específicos, como el iPhone. En la sección siguiente verá cómo sacar provecho de jQuery Mobile más vistas móviles atractivas.

## <a name="using-jquery-mobile"></a>Uso de jQuery Mobile

El [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteca proporciona un marco de interfaz de usuario que funciona en todos los principales exploradores móviles. jQuery Mobile aplica *mejora progresiva* a los exploradores móviles que admiten CSS y JavaScript. Mejora progresiva permite que todos los exploradores mostrar el contenido básico de una página web, mientras que permite que los exploradores más eficaces y los dispositivos tengan una presentación más completa. Muchos elementos para ajustarse a los exploradores móviles sin realizar ningún cambio de marcado de estilo de los archivos JavaScript y CSS que se incluyen con jQuery Mobile.

En esta sección instalará el *jQuery.Mobile.MVC* paquete NuGet, que instala jQuery Mobile y un widget de modificador de vista.

Para empezar, elimine el *Shared\\_Layout.Mobile.cshtml* y *Shared\\_Layout.iPhone.cshtml* archivos que creó anteriormente.

Cambiar el nombre de *Views\Home\AllTags.Mobile.cshtml* y *Views\Home\AllTags.iPhone.cshtml* archivos *Views\Home\AllTags.iPhone.cshtml.hide* y  *Views\Home\AllTags.Mobile.cshtml.Hide*. Dado que los archivos ya no tienen un *.cshtml* extensión, no usará el tiempo de ejecución de ASP.NET MVC para representar el *AllTags* vista.

Instalar el *jQuery.Mobile.MVC* paquete de NuGet haciendo lo siguiente:

1. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**y, a continuación, seleccione **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. En el **Package Manager Console**, escriba `Install-Package jQuery.Mobile.MVC -version 1.0.0`

La siguiente imagen muestra los archivos que se agregan y cambian al proyecto MvcMobile el paquete de NuGet jQuery.Mobile.MVC. Archivos que se han agregado [add] ha anexado tras el nombre de archivo. La imagen no muestra el formato GIF y los archivos PNG se agregan a la *Content\images* carpeta.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

El paquete de NuGet jQuery.Mobile.MVC instala lo siguiente:

- El *aplicación\_Start\BundleMobileConfig.cs* archivo, que es necesario para hacer referencia a los archivos CSS y JavaScript jQuery agregados. Debe seguir las instrucciones siguientes y hacer referencia a la agrupación de móvil definida en este archivo.
- archivos de jQuery Mobile CSS.
- Un `ViewSwitcher` widget del controlador (*Controllers\ViewSwitcherController.cs*).
- archivos de jQuery Mobile JavaScript.
- Un archivo de diseño móvil de estilo de jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Una vista parcial del modificador de vista *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) que proporciona un vínculo en la parte superior de cada página para cambiar de vista de escritorio a la vista móvil y viceversa.
- Varios<em>.png</em> y <em>.gif</em> archivos de imagen en el <em>Content\images</em> carpeta.

Abra el *Global.asax* archivo y agregue el código siguiente como la última línea de la `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

El código siguiente muestra la completa *Global.asax* archivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Si usa Internet Explorer 9 y no ve el `BundleMobileConfig` línea por encima de resaltado en amarillo, haga clic en el [botón de vista de compatibilidad](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![imagen del botón de vista de compatibilidad (desactivado)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Imagen del botón de vista de compatibilidad (desactivado)") en Internet Explorer para que el icono de cambiar de un esquema ![imagen del botón de vista de compatibilidad (desactivado)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "imagen del botón de vista de compatibilidad (desactivado) ") a un color sólido ![imagen del botón de vista de compatibilidad (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "imagen del botón de vista de compatibilidad (on)"). También puede ver este tutorial en FireFox o Chrome.


Abrir el *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* archivo y agregue el marcado siguiente directamente después de la `Html.Partial` llamar:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

La completa *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* archivo se muestra a continuación:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilar la aplicación y en el emulador de explorador móvil, vaya a la *AllTags* vista. Verá lo siguiente:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Puede depurar el código específico de dispositivos móvil por [establecer la cadena user agent](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para Internet Explorer o Chrome para iPhone y, a continuación, usar las herramientas de desarrollo F-12. Si no se muestra el explorador móvil el **inicio**, **altavoz**, **etiqueta**, y **fecha** vínculos como botones, las referencias a jQuery Mobile los scripts y archivos CSS probablemente no son correctos.


Además de los cambios de estilo, verá **Mostrar vista móvil** y un vínculo que le permite cambiar de vista móvil a la vista de escritorio. Elija la **vista de escritorio** se muestra el vínculo y la vista de escritorio.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La vista de escritorio no proporciona una forma de navegar directamente a la vista móvil. Para corregir esto ahora. Abra el *Views\Shared\\_Layout.cshtml* archivo. Justo debajo de la página `body` elemento, agregue el código siguiente, que representa el widget de modificador de vista:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Actualizar el *AllTags* vista en el explorador móvil. Ahora puede desplazarse entre las vistas de escritorio y móviles.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Depurar Nota: puede agregar el código siguiente al final de la Views\Shared\\_ViewSwitcher.cshtml para ayudar a depurar las vistas cuando mediante un explorador, la cadena de agente de usuario establecido en un dispositivo móvil.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  y agregar el encabezado siguiente a la *Views\Shared\\_Layout.cshtml* archivo.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Vaya a la *AllTags* página en un explorador de escritorio. El widget de modificador de vista no se muestra en un explorador de escritorio ya que solo se agrega a la página de diseño para dispositivos móviles. Más adelante en el tutorial verá cómo puede agregar el widget de modificador de vista a la vista de escritorio.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Mejora de la lista de oradores

En el explorador móvil, seleccione la **altavoces** vínculo. Porque no hay ninguna vista móvil (*AllSpeakers.Mobile.cshtml*), mostrar los oradores predeterminada (*AllSpeakers.cshtml*) se representa mediante la vista de diseño móvil ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Puede deshabilitar globalmente una vista de predeterminada (no móvil) dentro de un diseño móvil estableciendo `RequireConsistentDisplayMode` a `true` en el *vistas\\_ViewStart.cshtml* archivo similar al siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Cuando `RequireConsistentDisplayMode` está establecido en `true`, el diseño móvil (<em>\_Layout.Mobile.cshtml</em>) se usa solo para vistas móviles. (Es decir, el archivo de vista de formulario <em>** ViewName</em><em>. Mobile.cshtml</em>.) Desea establecer `RequireConsistentDisplayMode` a `true` si el diseño móvil no funciona bien con las vistas que no son móviles. La captura de pantalla siguiente muestra cómo el <em>altavoces</em> página presenta cuando `RequireConsistentDisplayMode` está establecido en `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Puede deshabilitar el modo de presentación coherente en una vista estableciendo `RequireConsistentDisplayMode` a `false` en el archivo de vista. El siguiente marcado en el *Views\Home\AllSpeakers.cshtml* archivo establece `RequireConsistentDisplayMode` a `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Creación de una vista móvil altavoces

Como acabamos de ver, el *altavoces* vista es legible, pero los vínculos son pequeños y difíciles de pulsar en un dispositivo móvil. En esta sección, creará un específicas de dispositivos móviles *altavoces* vista similar a una aplicación móvil moderna: muestra grandes, fáciles de pulsar vincula y contiene un cuadro de búsqueda para buscar oradores rápidamente.

Copia *AllSpeakers.cshtml* a *AllSpeakers.Mobile.cshtml*. Abra el *AllSpeakers.Mobile.cshtml* de archivos y quitar el `<h2>` elemento de encabezado.

En el `<ul>` etiqueta, agregue el `data-role` de atributo y establezca su valor en `listview`. Al igual que otros [ `data-*` atributos](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` hace que los elementos de lista grande sea más fácil puntear en. Este es el aspecto que tiene el marcado completado:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Actualice el explorador móvil. La vista actualizada tiene este aspecto:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Aunque la vista móvil ha mejorado, es difícil navegar por la larga lista de oradores. Para solucionar esto, en el `<ul>` etiqueta, agregue el `data-filter` atributo y establézcalo en `true`. El código siguiente muestra el `ul` marcado.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

La siguiente imagen muestra el cuadro de filtro de búsqueda en la parte superior de la página que se origina el `data-filter` atributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Mientras escribe cada letra en el cuadro de búsqueda, jQuery Mobile filtra la lista que aparece como se muestra en la imagen siguiente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Mejora de la lista de etiquetas

Al igual que el valor predeterminado *altavoces* vista, el *etiquetas* vista es legible, pero los vínculos son pequeños y difíciles de pulsar en un dispositivo móvil. En esta sección, corregirá el *etiquetas* ver la misma manera que se ha corregido la *altavoces* vista.

Quitar el &quot;ocultar&quot; sufijo a la *Views\Home\AllTags.Mobile.cshtml.hide* por lo que es el nombre de archivo *Views\Home\AllTags.Mobile.cshtml*. Abra el archivo renombrado y quite el `<h2>` elemento.

Agregar el `data-role` y `data-filter` atributos a la `<ul>` etiqueta, como se muestra aquí:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

La imagen siguiente muestra la página de etiquetas filtrando por la letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Mejora de la lista de fechas

Se puede mejorar la *fechas* ver como ha mejorado la *altavoces* y *etiquetas* vistas para que resulte más fácil de usar en un dispositivo móvil.

Copia el *Views\Home\AllDates.cshtml* archivo *Views\Home\AllDates.Mobile.cshtml*. Abra el nuevo archivo y quite el `<h2>` elemento.

Agregar `data-role="listview"` a la `<ul>` etiqueta, similar al siguiente:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

La imagen siguiente muestra lo que el **fecha** aspecto de página con el `data-role` atributo en su lugar.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) reemplace el contenido de la *Views\Home\AllDates.Mobile.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Este código agrupa todas las sesiones por días. Crea un separador de lista para cada día nuevo y enumera todas las sesiones para cada día en un divisor. Este es su aspecto cuando se ejecuta este código:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Mejora de la vista SessionsTable

En esta sección, creará una vista específica para móviles de sesiones. Los cambios que realice será más amplias que en otras vistas que hemos creado.

En el explorador móvil, pulse el **altavoz** botón y, después, escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Pulse el **Scott Hanselman** vínculo.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como puede ver, la pantalla es difícil de leer en un explorador móvil. Es difícil de leer la columna de fecha y la columna de etiquetas está fuera de la vista. Para solucionar este problema, copie *Views\Home\SessionsTable.cshtml* a *Views\Home\SessionsTable.Mobile.cshtml*y, a continuación, reemplace el contenido del archivo con el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

El código quita la sala de reuniones y etiquetas de columnas y formatos verticalmente, el título, orador y fecha para que toda esta información es legible en un explorador móvil. La imagen siguiente refleja los cambios de código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Mejora de la vista SessionByCode

Por último, creará una vista específica para móviles de la *SessionByCode* vista. En el explorador móvil, pulse el **altavoz** botón y, después, escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Pulse el **Scott Hanselman** vínculo. Se muestran las sesiones de Scott Hanselman.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Elija la **una visión general de la pila de amor MS Web** vínculo.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La vista de escritorio predeterminada está bien, pero se puede mejorar.

Copia el *Views\Home\SessionByCode.cshtml* a *Views\Home\SessionByCode.Mobile.cshtml* y reemplace el contenido de la *Views\Home\SessionByCode.Mobile.cshtml*archivo con el siguiente marcado:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

El nuevo marcado usa el `data-role` atributo para mejorar el diseño de la vista.

Actualice el explorador móvil. La siguiente imagen refleja los cambios de código que acaba de crear:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Revisión y Wrapup

Este tutorial presenta las nuevas características móviles de ASP.NET MVC 4 Developer Preview. Las características móviles incluyen:

- La capacidad de invalidar el diseño, vistas y las vistas parciales, tanto globalmente como para una vista individual.
- Control sobre el diseño y la aplicación de invalidación parcial mediante la `RequireConsistentDisplayMode` propiedad.
- Un widget de modificador de vista para dispositivos móviles vistas que se pueden mostrar en vistas de escritorio.
- Soporte técnico para la compatibilidad con exploradores específicos, como el Explorador de iPhone.

## <a name="see-also"></a>Vea también

- [jQuery Mobile](http://jquerymobile.com) sitio.
- [jQuery Mobile Overview](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Recomendaciones de W3C recomendación Web móvil aplicaciones](http://www.w3.org/TR/mwabp/)
- [Recomendación de candidatos de W3C para consultas multimedia](http://www.w3.org/TR/css3-mediaqueries/)
