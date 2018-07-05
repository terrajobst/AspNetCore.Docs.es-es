---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usar el Inspector de página en ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y el Inspector de página i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 032be78e744bdf2c74337c72afb4efb7471ae4f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399148"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Usar el Inspector de página en ASP.NET MVC
====================
por Tim Ammann

> Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y el Inspector de página al instante resalta del elemento origen y CSS. Puede examinar cualquier vista MVC, rápidamente buscar los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.
> 
> [Vea el vídeo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Este tutorial muestra cómo habilitar el modo de inspección y, a continuación, buscar y editar rápidamente marcado y CSS dentro del proyecto web. Este tutorial usa un proyecto de MVC, pero también puede usar el Inspector de página para [formularios Web Forms](https://go.microsoft.com/?linkid=9802001) y otras aplicaciones de ASP.NET.
> 
> El tutorial tiene las siguientes secciones:
> 
> - [Requisitos previos](#_1_prerequisites)
> - [Crear una aplicación Web](#_2_creating_a)
> - [Usar Inspector de página para examinar a una vista](#_3_using_page)
> - [Habilitar el modo de inspección](#_4_inspection_mode)
> - [Usar el Inspector de página para realizar cambios en el marcado](#_5_using_page)
> - [Modo de inspección y la ventana de HTML](#_6_inspection_mode)
> - [Vista previa de cambios en la ventana estilos CSS](#_7_previewing_css)
> - [Sincronización automática de CSS](#css_auto_sync)
> - [Con el selector de Color CSS](#css_color_picker)
> - [Asignación de elementos de página dinámica a JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obtener la versión más reciente de Inspector de página, use [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Windows Azure para .NET 2.0.


Inspector de página se incluye con Microsoft Web Developer Tools. La versión más reciente es 1.3. Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** desde el **ayuda** menú.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Crear una aplicación Web

En primer lugar, cree una aplicación web que va a utilizar con el Inspector de página. En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**.

![Nueva aplicación MVC de ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Haga clic en **Aceptar**.

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet**. Deje **Razor** como el motor de vistas predeterminado.

![Nuevo proyecto de MVC de ASP.NET: aplicación de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Se abre la aplicación en **origen** vista.

![Nueva aplicación MVC de ASP.NET en la vista de origen](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Ahora que tiene una aplicación para que funcione con, puede usar el Inspector de página para examinar y modificarlo.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Usar Inspector de página para examinar a una vista

En Visual Studio 2012, puede hacer clic en cualquier vista en el proyecto, seleccione **ver en Inspector de página**, y lo averiguar la ruta y se mostrará la página de Inspector de página.

En **el Explorador de soluciones**, expanda el **vistas** carpeta y, a continuación, el **inicio** carpeta. Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.

![Ver Index.cshtml en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

De forma predeterminada, el Inspector de página está acoplado como una ventana en el lado izquierdo del entorno de Visual Studio. Si lo prefiere, puede acoplarlo en otro lugar o desacoplar la ventana. Consulte [Cómo: organizar y acoplar Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).

El panel superior de la ventana del Inspector de página muestra la página actual en una ventana del explorador. El panel inferior muestra la página en el marcado HTML, junto con algunas de las pestañas que le permiten consultar distintos aspectos de la página. El panel inferior es similar a la [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.

![Aplicación de ASP.NET MVC en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

En este tutorial, usará el **HTML** y **estilos** pestañas para navegar rápidamente y realizar cambios en la aplicación.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modo EnableInspection

Para poner Inspector de página en modo de inspección, haga clic en el **inspeccionar** botón. En el modo de inspección, cuando se mantiene el puntero del mouse sobre cualquier parte de la página presentada, el marcado del código fuente correspondiente o el código se resalta.

![Alternar el modo de inspección](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Ahora, mueva el mouse sobre las distintas partes de la página en el Inspector de página. Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:

![Mantener el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Al mover el puntero del mouse, Visual Studio resalta la sintaxis de Razor correspondiente en el archivo de origen. Si el elemento HTML procede de otro archivo de código fuente, Visual Studio abre automáticamente el archivo.

En el Inspector de página, el **HTML** pestaña muestra el código HTML que se generó a partir de la sintaxis de Razor. A medida que mueve el puntero del mouse, se resaltan los elementos HTML. El **estilos** ficha muestra las reglas CSS para el elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar el Inspector de página para realizar cambios en el marcado

Inspector de página permite buscar el marcado cuya ubicación puede no ser obvio. A continuación, puede modificar el marcado y ver los cambios resultantes.

Para ver esto, haga clic en **inspeccionar** y, a continuación, desplácese hasta la parte inferior de la página en la ventana del Inspector de página.

Cuando se mueve el puntero del mouse en el área de pie de página, Inspector de página abre el \_archivo Layout.cshtml y resalta la sección de la página de diseño que ha seleccionado. Como puede ver, el pie de página son se define en el archivo de diseño y no la propia vista.

![Pie de página](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Ahora se mueva el puntero del mouse sobre la línea con los derechos de autor <a id="a"> </a>tenga en cuenta. En el \_Layout.cshtml página, la línea correspondiente se resalta.

![Línea de copyright del pie de página resaltada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Agregue texto al final de la línea en el \_Layout.cshtml archivo.

&lt;p&gt;&amp;copien; @DateTime.Now.Year -Mi aplicación ASP.NET MVC es impresionante! &lt;/p&gt;

Ahora, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización para ver los resultados en la ventana del explorador de Inspector de página.

![Mi Rocks de aplicación de ASP.NET.](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Es posible que haya pensado que el pie de página definido en Index.cshtml, pero, resultó para ser en el \_Layout.cshtml y resultó que el Inspector de página.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspección y la ventana de HTML

A continuación, tendrá una visión rápida de la ventana de HTML y cómo se asigna elementos por usted.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

Haga clic en la parte superior de la página que dice "El logohere". Si está examinando un elemento determinado con más detalle, por lo que ya no se cambia la presentación en la ventana del explorador al mover el puntero del mouse.

Ahora, mueva el puntero del mouse en el **HTML** ventana. Al mover el puntero del mouse, Inspector de página se describe el elemento dentro de la **HTML** ventana y resalta el elemento correspondiente en la ventana del explorador.

![Ventana de HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Como antes, Inspector de página abre el \_archivo Layout.cshtml automáticamente en una pestaña temporal. Haga clic en el \_Layout.cshtml de pestaña temporal y el marcado correspondiente se resaltará en el &lt;encabezado&gt; sección para usted:

![Marcado resaltado](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vista previa de cambios en la ventana estilos CSS

A continuación, usará el Inspector de página **estilos** ventana para obtener una vista previa de los cambios de CSS.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

En la ventana de explorador del Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que el **div.content contenedor** etiqueta aparece.

![Mantener el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Haga clic en la sección div.content contenedor una vez y, a continuación, mueva el puntero del mouse hasta el **estilos** ventana. El **Syles** ventana muestra todas las reglas CSS para este elemento. Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase. Ahora, desactive la casilla de la propiedad de color de fondo.

![Color de fondo claro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Observe cómo el cambio muestra una vista previa al instante en la ventana del explorador de Inspector de página.

Vuelva a seleccionar la casilla de verificación, a continuación, haga doble clic en el valor de propiedad y cámbielo a rojo. El cambio aparece inmediatamente:

![Color de fondo rojo](using-page-inspector-in-aspnet-mvc/_static/image30.png)

El **estilos** facilita la ventana que resulte fácil probar y obtener una vista previa de CSS cambia antes de confirmar los cambios en el estilo de hojas de sí mismo.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronización automática de CSS

> [!NOTE]
> Esta característica requiere la versión 1.3 de Inspector de página.


La característica de sincronización automática de CSS le permite editar directamente un archivo CSS y ver los cambios inmediatamente en el Explorador de Inspector de página.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

En el Explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que el **div.content contenedor** etiqueta aparece. Haga clic una vez para seleccionar este elemento.

El **Syles** ventana muestra todas las reglas CSS para este elemento. Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase. Haga clic en ".featured .content-contenedor". Inspector de página abre el archivo CSS que define este estilo (Site.css) y resalta el estilo CSS correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Ahora, cambie el valor de `background-color` a "rojo". El cambio aparece inmediatamente en el Explorador de Inspector de página.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Con el selector de Color CSS

El editor de CSS en Visual Studio 2012 tiene un selector de colores que resulta muy fácil elegir e insertar los colores. El selector de colores incluye una paleta de colores estándar, es compatible con nombres de colores estándar, códigos hash, los colores RGB, RGBA, HSL y HSLA y mantiene una lista de los colores que se ha usado más recientemente en el documento.

En la sección anterior, ha cambiado el valor de la `background-color` propiedad. Para invocar el selector de colores, sitúe el cursor después del nombre de la propiedad y escriba **#** o **rgb (**.

![La barra de selector de colores CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Haga clic en un color para seleccionarlo, o presione la tecla flecha abajo y, a continuación, use las teclas de flecha derecha e izquierda para atravesar los colores. Cuando visita un color, el valor hexadecimal correspondiente es una vista previa:

![valor de propiedad de color de fondo muestra una vista previa](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Si la barra de colores no tiene el color exacto que desea, puede usar el pop desplegable de selector de color. Para abrirlo, haga clic en el cheurón doble en el extremo derecho de la barra de colores o presione la flecha abajo de una o dos veces en el teclado.

![Selector de Color CSS Pop-abajo](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Haga clic en un color en la barra vertical a la derecha. Esto muestra un degradado para ese color en la ventana principal. Elegir un color directamente desde la barra vertical, presione ENTRAR o haga clic en cualquier punto en la ventana principal para elegir con mayor precisión.

Si hay un color en la pantalla del equipo que desea utilizar (que no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta de cuentagotas en la esquina inferior derecha.

También puede cambiar la opacidad de un color moviendo el control deslizante en la parte inferior del selector de colores. Hacerlo cambia al color valores a los valores RGBA, porque el formato RGBA puede representar la opacidad.

Después de elegir un color, presione ENTRAR y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el *Site.css* archivo.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra de actualización de Inspector de página

Inspector de página detecta inmediatamente el cambio en el *Site.css* de archivo y muestra una alerta en una barra de actualización.

![Barra de actualización](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Para guardar todos los archivos y actualice el Explorador de Inspector de página, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización. El cambio en el color de resaltado aparece en el explorador.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Asignación de elementos de página dinámica a JavaScript

En las aplicaciones web modernas, los elementos de la página a menudo se generan dinámicamente con JavaScript. Esto significa que no hay ningún marcado estático (HTML o Razor) que corresponde a estos elementos de la página.

Con la versión 1.3, Inspector de página ahora puede asignar los elementos que se han agregado dinámicamente a la página al código JavaScript correspondiente. Para demostrar esta característica, usaremos el [plantilla de aplicación de página única (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> La plantilla SPA requiere la [ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) actualizar.


En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**. Haga clic en **Aceptar**.

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de página única**.

En el Explorador de soluciones, expanda el **vistas** carpeta y, a continuación, el **inicio** carpeta. Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.

Lo primero que se muestran en el Explorador de Inspector de página es una página de inicio de sesión. Haga clic en "Registrarse" y cree un nombre de usuario y una contraseña. Una vez registrado, la aplicación inicia la sesión y crea una lista de tareas pendientes con algunos elementos de ejemplo.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección. En el explorador del Inspector de página, haga clic en uno de los elementos de tareas pendientes. Tenga en cuenta que, en lugar de que se resaltan en azul, se resalta el elemento en color naranja, con "JS" junto al nombre del elemento. Esto indica que el elemento se creó dinámicamente a través del script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Además, aparece un subrayado de color naranja en la **pila de llamadas** ficha. Esto indica que el **pila de llamadas** panel tiene más información sobre el elemento.

Haga clic en el **pila de llamadas** ficha. El **pila de llamadas** panel muestra la pila de llamadas de la llamada de JavaScript que crea el elemento. Las llamadas a bibliotecas externas, como jQuery se contraen, por lo que puede ver fácilmente las llamadas a la secuencia de comandos de la aplicación.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Para ver la pila completa, incluidas las llamadas a bibliotecas externas, puede expandir los nodos con la etiqueta "Bibliotecas externas":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Si hace clic en un elemento en la pila de llamadas, Visual Studio abre el archivo de código y resalta la secuencia de comandos correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Vea también

[Introducción a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sitio Web de ASP.net)

[Presentación de Inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo de Channel 9)

[Los mensajes de Error del Inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)
