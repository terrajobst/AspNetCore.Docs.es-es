---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Con el Inspector de página para Visual Studio 2012 en ASP.NET Web Forms | Documentos de Microsoft"
author: rick-anderson
description: "Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Con el Inspector de página para Visual Studio 2012 en formularios Web Forms ASP.NET
====================
por Tim Ammann

> Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página al instante resalta el elemento de la fuente y CSS. Puede examinar cualquier página en la aplicación, rápidamente buscar los orígenes del marcado representado y utilizar las herramientas de explorador en el entorno de Visual Studio.
> 
> Este tutorial shwos cómo habilitar el modo de inspección y, a continuación, busque y editar rápidamente las reglas de CSS y texto en el proyecto web. El tutorial utiliza un proyecto de aplicación de formularios Web, pero también puede utilizar el Inspector de página para los proyectos de sitio Web y [MVC](https://go.microsoft.com/?linkid=9802002) aplicaciones.
> 
> El tutorial consta de las siguientes secciones:
> 
> [Requisitos previos](#_1_prerequisites)
> 
> [Crear una aplicación Web](#_2_creating_a)
> 
> [Usar Inspector de página para ver la aplicación](#_3_using_page)
> 
> [Habilitar el modo de inspección](#_4_inspection_mode)
> 
> [Usar Inspector de página para realizar cambios en el marcado](#_5_using_page)
> 
> [Modo de inspección y la ventana de HTML](#_6_inspection_mode)
> 
> [Vista previa de cambios en la ventana Estilos de CSS](#_7_previewing_css)
> 
> [Sincronización automática de CSS](#css_auto_sync)
> 
> [Mediante el selector de Color CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obtener la versión más reciente de Inspector de página, use [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Azure para .NET 2.0.


Inspector de página está integrado en Microsoft Web Developer Tools. La versión más reciente es 1.3. Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** desde el **ayuda** menú.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Crear una aplicación Web

En primer lugar, creará una aplicación web que se va a usar Inspector de página con. En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación de ASP.NET Web Forms**.

![Nueva aplicación de formularios Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Haga clic en **Aceptar**.

Se abre la aplicación en **origen** vista.

![Nueva aplicación de formularios Web en la vista de origen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Ahora que tiene una aplicación para que funcione con, puede utilizar el Inspector de página para examinar y modificarlo.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Usar Inspector de página para ver la aplicación

A continuación, verá la aplicación con el Inspector de página. En **el Explorador de soluciones**, haga clic en el proyecto y, a continuación, elija **ver en Inspector de página**.

![Ver en Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

De forma predeterminada, al Inspector de página se inicia por primera vez, se está acoplado como una ventana estrecha en el lado izquierdo del entorno de Visual Studio. Dejarla acoplada en el lado izquierdo y establézcalo en un valor de ancho que resulte cómodo para usted, o cuando acopla en una de las áreas de la herramienta en la parte superior, inferior o derecho:

![Posiciones de acoplamiento de Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Si desacoplar la ventana del Inspector de página, se puede colocar fuera de Visual Studio, o incluso en un segundo monitor si dispone de uno. Sin embargo, en orden a ALT + TAB entre Visual Studio y el Inspector de página cuando la ventana de Inspector de página está desacoplada, se vaya a **herramientas** &gt; **opciones** &gt;  **Entorno** &gt; **pestañas y ventanas**y en **ficha también**, desactive la casilla de verificación denominada **las ventanas de herramientas flotante siempre está en la parte superior de la ventana principal**:

![Desactive la casilla de windows de herramienta flotante a ALT + TAB entre Visual Studio y la ventana de Inspector de página desacoplada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

El panel superior de la ventana de Inspector de página muestra la página actual en una ventana del explorador. El panel inferior muestra la página en el marcado HTML de la izquierda, y algunas fichas a la derecha que le permiten inspeccionar los distintos aspectos de la página. El panel inferior es similar a la [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer. (Sin embargo, a diferencia de las herramientas de desarrollo, puede utilizar el Inspector de página en Visual Studio.)

![Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

En este tutorial, va a usar el panel Explorador de Inspector de página y el **HTML** y **estilos** pestañas para ayudarle rápidamente navegar y realizar cambios en la aplicación.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Habilitar el modo de inspección

A continuación, verá cómo funciona el modo de inspección del Inspector de página. En la ventana del Inspector de página, haga clic en el **inspeccionar** botón.

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Para ver el modo de inspección en acción, mueva el mouse sobre las distintas partes de la página en la ventana del explorador de Inspector de página. Tal y como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:

![Mantiene el mouse sobre el contenedor de div.content](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Cuando mueve el puntero del mouse, tenga en cuenta que

- El contenido de **origen** ver cambia para mostrar el marcado correspondiente al elemento seleccionado en la página. El marcado correspondiente se resalta. Si el origen está en otro archivo, ese archivo se abre en la vista de origen con el resaltado de marcado pertinentes.

- El marcado que se muestra en el **HTML** ficha en Inspector de página también se cambia para que correspondan al elemento seleccionado en la página. En el **HTML** ficha, se muestra el marcado correspondiente.

- El **estilos** pestaña muestra las reglas de CSS relevantes para la selección actual.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspector de página para realizar cambios en el marcado

Ahora verá cómo se puede usar el Inspector de página para buscar y realizar cambios en el marcado o el texto cuya ubicación podría no detectarse inmediatamente.

Poner Inspector de página en modo de inspección y, a continuación, desplácese hasta la parte inferior de la página principal.

En cuanto escriba el área de pie de página, Inspector de página abre los *Site.Master* archivo de diseño en **origen** vista en una pestaña temporal a la derecha de las otras pestañas y resalta la sección del maestro de página que ha seleccionado. Esto muestra cómo puede buscar y mostrar el contenido en una página que realmente podría proceder de un archivo diferente a la que se abrió originalmente Inspector de página.

![Aspectos destacados de pie de página en modo de inspección](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre la línea con los derechos de autor <a id="a"> </a>tenga en cuenta.

En el *Site.Master* página, la línea correspondiente se resalta.

![Línea de propiedad intelectual de pie de página resaltada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Agregar texto al final de la línea en la *Site.Master* archivo.

&lt;p&gt;&amp;copien; &lt;%: DateTime.Now.Year %&gt; -mi rocas de aplicación de ASP.NET!&lt; /p&gt;

Ahora, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización para ver los resultados en la ventana del explorador de Inspector de página.

![Mi rocas de aplicación de ASP.NET.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Es posible que haya pensado que era el pie de página en el *Default.aspx* página, pero resultó en la página de diseño maestra y Inspector de página se encuentra para usted.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspección y la ventana de HTML

A continuación, tendrá un vistazo rápido a la ventana HTML y cómo se asigna elementos automáticamente.

Poner Inspector de página en modo de inspección.

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Haga clic en la parte superior de la página que dice "su logotipo aquí". Se examina un elemento determinado con más detalle, por lo que ya no se cambia la visualización en la ventana del explorador cuando mueve el puntero del mouse.

Ahora, mueva el puntero del mouse hasta el **HTML** ventana. Cuando mueve el puntero del mouse, Inspector de página se describen el elemento en el **HTML** ventana y resalta el elemento correspondiente en la ventana del explorador.

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Como antes, Inspector de página abre los *Site.Master* archivo automáticamente en una pestaña temporal. Haga clic en la pestaña Site.Master y el marcado correspondiente se resalta en el &lt;encabezado&gt; sección:

![Marcado resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vista previa de cambios en la ventana Estilos de CSS

A continuación, verá cómo se puede utilizar el Inspector de página **estilos** ventana obtener una vista previa de los cambios de CSS.

Haga clic en el **inspeccionar** botón para poner Inspector de página en modo de inspección.

En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece.

![Al mantener el mouse sobre los elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Haga clic en la sección div.content contenedor una vez y, a continuación, mueva el puntero del mouse hasta el **estilos** ventana. En el selector de clase de contenedor .content .featured, desactive y Active la casilla de verificación para la propiedad de color de fondo.

![Color de fondo claro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Observe cómo el cambio muestra una vista previa al instante en la ventana del explorador de Inspector de página.

Vuelva a seleccionar la casilla de verificación, a continuación, haga doble clic en el valor de propiedad y cámbiela a `red`. El cambio se muestra inmediatamente:

![Color de fondo rojo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

El **estilos** hace ventana fáciles de probar y obtener una vista previa de CSS cambia antes de confirmar los cambios en el estilo de hojas de sí mismo.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronización automática de CSS

> [!NOTE]
> Esta característica requiere la versión 1.3 de Inspector de página.


La característica de sincronización automática de CSS permite editar directamente un archivo CSS y ver los cambios inmediatamente en el Explorador de Inspector de página.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

En el Explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece. Haga clic una vez para seleccionar este elemento.

El **Syles** ventana muestra todas las reglas CSS para este elemento. Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase. Haga clic en ".featured .content-contenedor". Inspector de página se abre el archivo CSS que define este estilo (Site.css) y resalta el estilo CSS correspondiente.

![Archivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Ahora, cambie el valor de `background-color` a "rojo". El cambio aparece inmediatamente en el Explorador de Inspector de página.

![Explorador de Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Mediante el selector de Color CSS

A continuación, aprenderá cómo utilizar el Inspector de página para buscar rápidamente y cambiar el código CSS para el texto resaltado en la aplicación predeterminada. En este ejemplo, ha decidido que no desee el resaltado azul y desea cambiar a otro color.

Haga clic en el **inspeccionar** botón.

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre el resaltado "vídeos, tutoriales y ejemplos" aparece de forma que el código CSS "marcar" etiqueta de texto.

![Al mantener el mouse sobre el elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Haga clic en el texto para seleccionarlo. El selector de marca CSS correspondiente aparece en la parte inferior de la **estilos** ventana.

![selector de marca en la ventana estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Haga clic en el selector de marca. Se abrirá la *Site.css* archivo para la aplicación web. Haga clic en la pestaña Site.css y se resalta el correspondiente código CSS para el selector:

![selector de marca en la hoja de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Seleccione y quite la línea con la propiedad de color de fondo.

Ahora utilizará el selector de colores de Visual Studio 2012 CSS nuevo para elegir un color nuevo para la **marcar** propiedad de color de fondo.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Mediante el selector de Color de CSS de Visual Studio 2012

El editor de CSS en Visual Studio 2012 tiene un selector de color que hace más fácil elegir e insertar otros colores. Tiene una barra de colores simple y un selector "pop desplegable" que ofrece un control más preciso.

El selector de colores incluye una paleta de colores estándar, es compatible con nombres de color estándar, códigos hash, los colores RGB, RGBA, HSL y HSLA y mantiene una lista de los colores que se haya utilizado más recientemente en el documento.

En la línea donde fue la propiedad de color de fondo, escriba "bc" y pulse la flecha hacia abajo una vez.

Cuando se escribe el primer carácter de cada palabra en una propiedad separados por guión como "background-color", IntelliSense filtra la lista para mostrar solo las propiedades que coinciden con:

![Valores de filtrado de IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

A continuación, escriba un signo de dos puntos. Al hacerlo, se inserta el nombre de propiedad de color de fondo completo. Tipo de  **#**  o **rgb (**, y aparece la barra de selector de color:

![La barra de selector de color CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Para ver cómo funciona la barra de selector de color, haga clic en los colores con el puntero del mouse, o presione la tecla flecha abajo y, a continuación, use las teclas de flecha derecha e izquierda para atravesar los colores. Cuando visita un color, el valor correspondiente de la propiedad de color de fondo es una vista previa:

![valor de propiedad de color de fondo que muestra una vista previa](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

En este punto, puede presionar ENTRAR para seleccionar el valor y, a continuación, un punto y coma (;) para completar la entrada CSS. Por ahora, vaya a la sección siguiente para que pueda ver cómo funciona la profundidad de pop de selector de color.

#### <a name="using-the-color-picker-pop-down"></a>Usar el selector de Color Pop-abajo

Cuando la barra de colores no tiene exactamente el color que está buscando, puede usar el selector de colores emergente abajo.

Para abrirlo, haga clic en las comillas angulares dobles en el extremo derecho de la barra de colores o presione la tecla flecha abajo una o dos veces en el teclado.

![Selector de Color CSS Pop-abajo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Haga clic en un color en la barra vertical a la derecha. Esto muestra un degradado para ese color en la ventana principal. Elija un color directamente desde la barra vertical, presione ENTRAR o haga clic en cualquier momento en la ventana principal para elegir con mayor precisión.

Si hay un color en la pantalla del equipo que desea utilizar (que no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta de cuentagotas en la esquina inferior derecha.

También puede cambiar la opacidad de un color moviendo el control deslizante en la parte inferior del selector de color. Si lo hace, cambios de color valores con los valores RGBA porque el formato RGBA puede representar la opacidad.

Después de elegir un color, presione ENTRAR y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el *Site.css* archivo.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra de actualización de Inspector de página

Inspector de página detecta inmediatamente el cambio a la *Site.css* archivo (o a cualquier archivo en la aplicación) y muestra una alerta en una barra de actualización.

![Barra de actualización](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Para guardar todos los archivos y actualizar el Explorador de Inspector de página, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización. El cambio en el color de resaltado aparece en el explorador:

![Cambiar el color de resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Tenga en cuenta que cómodamente actualice el Explorador de Inspector de página directamente desde dentro del entorno de Visual Studio. Con el Inspector de página en lugar de un explorador externo le mantendrá en el editor cuando se desarrollan las aplicaciones web.

## <a name="see-also"></a>Vea también

[Introducción a Inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo de Channel 9)

[Mensajes de Error del Inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)
