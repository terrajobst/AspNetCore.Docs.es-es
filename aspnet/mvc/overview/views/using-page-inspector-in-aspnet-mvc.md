---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "Usar Inspector de página en ASP.NET MVC | Documentos de Microsoft"
author: rick-anderson
description: "Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6aa9f16f166ecf5529ae33a17951eb5ea425e7af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Usar el Inspector de página en ASP.NET MVC
====================
por Tim Ammann

> Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página al instante resalta el elemento de la fuente y CSS. Puede examinar cualquier vista MVC, rápidamente buscar los orígenes del marcado representado y utilizar las herramientas de explorador en el entorno de Visual Studio.
> 
> [Vea el vídeo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Este tutorial muestra cómo habilitar el modo de inspección y, a continuación, busque y editar rápidamente CSS y marcado dentro del proyecto web. El tutorial utiliza un proyecto de MVC, pero también puede utilizar el Inspector de página para [formularios Web Forms](https://go.microsoft.com/?linkid=9802001) y otras aplicaciones de ASP.NET.
> 
> El tutorial consta de las siguientes secciones:
> 
> - [Requisitos previos](#_1_prerequisites)
> - [Crear una aplicación Web](#_2_creating_a)
> - [Usar Inspector de página que vaya a una vista](#_3_using_page)
> - [Habilitar el modo de inspección](#_4_inspection_mode)
> - [Usar Inspector de página para realizar cambios en el marcado](#_5_using_page)
> - [Modo de inspección y la ventana de HTML](#_6_inspection_mode)
> - [Vista previa de cambios en la ventana Estilos de CSS](#_7_previewing_css)
> - [Sincronización automática de CSS](#css_auto_sync)
> - [Mediante el selector de Color CSS](#css_color_picker)
> - [Asignación de elementos de página dinámicos para JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).

> [!NOTE]
> Para obtener la versión más reciente de Inspector de página, use [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Windows Azure para .NET 2.0.


Inspector de página está integrado en Microsoft Web Developer Tools. La versión más reciente es 1.3. Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** desde el **ayuda** menú.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Crear una aplicación Web

En primer lugar, cree una aplicación web que se va a usar Inspector de página con. En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC4**.

![Nueva aplicación MVC de ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Haga clic en **Aceptar**.

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![Nuevo proyecto de ASP.NET MVC - aplicación de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Se abre la aplicación en **origen** vista.

![Nueva aplicación de MVC de ASP.NET en la vista de origen](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Ahora que tiene una aplicación para que funcione con, puede utilizar el Inspector de página para examinar y modificarlo.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Usar Inspector de página que vaya a una vista

En Visual Studio 2012, puede hacer clic cualquier vista en el proyecto, seleccione **ver en Inspector de página**, y se pueda determinar la ruta y mostrar la página de Inspector de página.

En **el Explorador de soluciones**, expanda la **vistas** carpeta y, a continuación, el **inicio** carpeta. Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.

![Ver Index.cshtml en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

De forma predeterminada, el Inspector de página está acoplado como una ventana en el lado izquierdo del entorno de Visual Studio. Si lo prefiere, puede acoplarlo en otra ubicación o desacoplar la ventana. Vea [Cómo: organizar y acoplar ventanas](https://msdn.microsoft.com/en-us/library/z4y0hsax.aspx).

El panel superior de la ventana de Inspector de página muestra la página actual en una ventana del explorador. El panel inferior muestra la página en el marcado HTML, junto con algunas fichas que le permiten inspeccionar los distintos aspectos de la página. El panel inferior es similar a la [herramientas de desarrollo F12](https://msdn.microsoft.com/en-us/ie/aa740478) en Internet Explorer.

![Aplicación de ASP.NET MVC en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

En este tutorial, utilizará el **HTML** y **estilos** pestañas para navegar rápidamente y realizar cambios en la aplicación.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modo de EnableInspection

Para poner Inspector de página en modo de inspección, haga clic en el **inspeccionar** botón. En el modo de inspección, cuando mantiene el puntero del mouse sobre alguna parte de la página presentada, el formato del código fuente correspondiente o el código se resalta.

![Pasar al modo de inspección](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Ahora, mueva el mouse sobre las distintas partes de la página en el Inspector de página. Tal y como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:

![Mantiene el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image14.png)

A medida que mueve el puntero del mouse, Visual Studio resalta la sintaxis de Razor correspondiente en el archivo de origen. Si el elemento HTML procede de otro archivo de código fuente, Visual Studio abre automáticamente el archivo.

En el Inspector de página, el **HTML** pestaña muestra el código HTML que se generó a partir de la sintaxis de Razor. A medida que mueve el puntero del mouse, se resaltan los elementos HTML. El **estilos** pestaña muestra las reglas de CSS para el elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspector de página para realizar cambios en el marcado

Inspector de página le permite encontrar marcado cuya ubicación podría no ser obvia. A continuación, puede modificar el marcado y ver los cambios resultantes.

Para ver esto, haga clic en **inspeccionar** y, a continuación, desplácese hasta la parte inferior de la página en la ventana de Inspector de página.

Cuando se mueve el puntero del mouse en el área de pie de página, Inspector de página abre los \_Layout.cshtml archivo y resalta la sección de la página de diseño que ha seleccionado. Como puede ver, el pie de página son se define en el archivo de diseño y no la propia vista.

![Pie de página](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Ahora mueva el puntero del mouse sobre la línea con los derechos de autor <a id="a"> </a>tenga en cuenta. En el \_Layout.cshtml página, la línea correspondiente se resalta.

![Línea de propiedad intelectual de pie de página resaltada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Agregar texto al final de la línea en el \_Layout.cshtml archivo.

&lt;p&gt;&amp;copien; @DateTime.Now.Year -Mi aplicación ASP.NET MVC Rocks! &lt;/p&gt;

Ahora, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización para ver los resultados en la ventana del explorador de Inspector de página.

![Mi rocas de aplicación de ASP.NET.](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Podría haber pensado que el pie de página definido en Index.cshtml, pero resultó para ser en el \_Layout.cshtml e Inspector de página que se encuentra por usted.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspección y la ventana de HTML

A continuación, tendrá un vistazo rápido a la ventana HTML y cómo se asigna elementos automáticamente.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

Haga clic en la parte superior de la página que dice "El logohere". Se examina un elemento determinado con más detalle, por lo que ya no se cambia la visualización en la ventana del explorador cuando mueve el puntero del mouse.

Ahora, mueva el puntero del mouse hasta el **HTML** ventana. Cuando mueve el puntero del mouse, Inspector de página se describen el elemento en el **HTML** ventana y resalta el elemento correspondiente en la ventana del explorador.

![Ventana HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Como antes, Inspector de página abre los \_Layout.cshtml archivo en una pestaña temporal. Haga clic en el \_Layout.cshtml temporal pestaña y el marcado correspondiente se resaltará en el &lt;encabezado&gt; sección automáticamente:

![Marcado resaltado](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vista previa de cambios en la ventana Estilos de CSS

A continuación, usará el Inspector de página **estilos** ventana obtener una vista previa de los cambios de CSS.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece.

![Mantiene el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Haga clic en la sección div.content contenedor una vez y, a continuación, mueva el puntero del mouse hasta el **estilos** ventana. El **Syles** ventana muestra todas las reglas CSS para este elemento. Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase. Ahora, desactive la casilla de la propiedad de color de fondo.

![Color de fondo claro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Observe cómo el cambio muestra una vista previa al instante en la ventana del explorador de Inspector de página.

Vuelva a seleccionar la casilla de verificación, a continuación, haga doble clic en el valor de propiedad y cambiar a rojo. El cambio se muestra inmediatamente:

![Color de fondo rojo](using-page-inspector-in-aspnet-mvc/_static/image30.png)

El **estilos** hace ventana fáciles de probar y obtener una vista previa de CSS cambia antes de confirmar los cambios en el estilo de hojas de sí mismo.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronización automática de CSS

> [!NOTE]
> Esta característica requiere la versión 1.3 de Inspector de página.


La característica de sincronización automática de CSS permite editar directamente un archivo CSS y ver los cambios inmediatamente en el Explorador de Inspector de página.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.

En el Explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece. Haga clic una vez para seleccionar este elemento.

El **Syles** ventana muestra todas las reglas CSS para este elemento. Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase. Haga clic en ".featured .content-contenedor". Inspector de página se abre el archivo CSS que define este estilo (Site.css) y resalta el estilo CSS correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Ahora, cambie el valor de `background-color` a "rojo". El cambio aparece inmediatamente en el Explorador de Inspector de página.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Mediante el selector de Color CSS

El editor de CSS en Visual Studio 2012 tiene un selector de color que hace más fácil elegir e insertar otros colores. El selector de colores incluye una paleta de colores estándar, es compatible con nombres de color estándar, códigos hash, los colores RGB, RGBA, HSL y HSLA y mantiene una lista de los colores que se haya utilizado más recientemente en el documento.

En la sección anterior, ha cambiado el valor de la `background-color` propiedad. Para invocar el selector de colores, sitúe el cursor después del nombre de la propiedad y escriba  **#**  o **rgb (**.

![La barra de selector de color CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Haga clic en un color para seleccionarlo, o presione la tecla flecha abajo y, a continuación, use las teclas de flecha derecha e izquierda para atravesar los colores. Cuando visita un color, el valor hexadecimal correspondiente es una vista previa:

![valor de propiedad de color de fondo que muestra una vista previa](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Si la barra de colores no tiene exactamente el color que desea, puede usar el selector de color pop-abajo. Para abrirlo, haga clic en las comillas angulares dobles en el extremo derecho de la barra de colores o presione la tecla flecha abajo una o dos veces en el teclado.

![Selector de Color CSS Pop-abajo](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Haga clic en un color en la barra vertical a la derecha. Esto muestra un degradado para ese color en la ventana principal. Elija un color directamente desde la barra vertical, presione ENTRAR o haga clic en cualquier momento en la ventana principal para elegir con mayor precisión.

Si hay un color en la pantalla del equipo que desea utilizar (que no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta de cuentagotas en la esquina inferior derecha.

También puede cambiar la opacidad de un color moviendo el control deslizante en la parte inferior del selector de color. Si lo hace, cambios de color valores con los valores RGBA, puesto que el formato RGBA puede representar la opacidad.

Después de elegir un color, presione ENTRAR y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el *Site.css* archivo.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra de actualización de Inspector de página

Inspector de página detecta inmediatamente el cambio a la *Site.css* de archivos y mostrará una alerta en una barra de actualización.

![Barra de actualización](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Para guardar todos los archivos y actualizar el Explorador de Inspector de página, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización. El cambio en el color de resaltado aparece en el explorador.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Asignación de elementos de página dinámicos para JavaScript

En aplicaciones web modernas, los elementos de la página a menudo se generan dinámicamente con JavaScript. Esto significa que no hay ningún marcado estático (HTML o Razor) que corresponde a estos elementos de la página.

Con la versión 1.3, Inspector de página ahora puede asignar los elementos que se han agregado dinámicamente a la página de vuelta al código de JavaScript correspondiente. Para demostrar esta característica, usaremos el [plantilla de aplicación de página única (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> La plantilla de SPA requiere la [ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) actualizar.


En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC4**. Haga clic en **Aceptar**.

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de una sola página**.

En el Explorador de soluciones, expanda la **vistas** carpeta y, a continuación, el **inicio** carpeta. Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.

Lo primero que se muestre en el Explorador de Inspector de página es una página de inicio de sesión. Haga clic en "Inicio de sesión de seguridad" y cree un nombre de usuario y una contraseña. Una vez que se registra, la aplicación la sesión y crea una lista de tareas con algunos elementos de ejemplo.

Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección. En el Explorador de Inspector de página, haga clic en uno de los elementos de tareas pendientes. Tenga en cuenta que, en lugar de que se resalta en azul, se resalta el elemento en color naranja, con "JS" junto al nombre del elemento. Esto indica que el elemento se ha creado dinámicamente a través de la secuencia de comandos.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Además, aparece un subrayado de color naranja en la **pila de llamadas** ficha. Esto indica que la **pila de llamadas** panel tiene más información sobre el elemento.

Haga clic en el **pila de llamadas** ficha. El **pila de llamadas** panel muestra la pila de llamadas para la llamada de JavaScript que creó el elemento. Las llamadas a las bibliotecas externas como jQuery están contraídas, por lo que puede ver fácilmente las llamadas a la secuencia de comandos de la aplicación.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Para ver la pila completa, incluidas las llamadas a las bibliotecas externas, puede expandir los nodos con la etiqueta "Bibliotecas externas":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Si hace clic en un elemento en la pila de llamadas, Visual Studio abre el archivo de código y resalta la secuencia de comandos correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Vea también

[Introducción a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sitio Web de ASP.net)

[Introducción a Inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo de Channel 9)

[Mensajes de Error del Inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)
