---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Novedades de ASP.NET y desarrollo Web en Visual Studio 2012 | Documentos de Microsoft
author: rick-anderson
description: La nueva versión de Visual Studio presenta a una serie de mejoras que se centra en la mejora de la experiencia y el rendimiento cuando se trabaja con tecnologías Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f447dc0108dffb36ed6d627fb83b3117fd22c94c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306837"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Novedades de ASP.NET y desarrollo Web en Visual Studio 2012
====================
Por [Web colonias equipo](https://twitter.com/webcamps)

> La nueva versión de Visual Studio presenta a una serie de mejoras que se centra en la mejora de la experiencia y el rendimiento cuando se trabaja con tecnologías Web. Editores de Visual Studio para CSS, JavaScript y HTML se ha reformado completamente para incluir muchas de las ayudas de código más solicitados, como IntelliSense y la sangría automática. Con respecto al rendimiento, las agrupar y minificar ahora se integran como tiempo de carga de características integradas para reducir fácilmente la página.
> 
> Visual Studio le permite trabajar con las tecnologías más recientes del sitio Web. Puede usar fragmentos de código de varios exploradores CSS3 para asegurarse de que el sitio funciona independientemente de la plataforma de cliente al también sacar partido de los nuevos elementos de HTML5 y características.
> 
> Escribir y generación de perfiles de código de JavaScript deben ser más fáciles con esta versión de Visual Studio. Listas de IntelliSense, características de navegación y la documentación de XML integradas ahora están disponibles para el código JavaScript. Ahora tiene el catálogo de JavaScript a su alcance. Además, puede comprobar el cumplimiento de ECMAScript5 con las secuencias de comandos y detectar errores de sintaxis en una fase temprana.
> 
> Por último, pero no menos importante, esta versión de Visual Studio implementa integrados agrupar y minificar. Los archivos de script y hojas de estilos se empaquetan y comprimir de forma que el sitio se realiza con mayor rapidez.
> 
> Esta práctica le guiará a través de las mejoras y nuevas características descritos anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio de prácticas, obtendrá información sobre cómo:

- Use las nuevas características y mejoras en el editor de CSS
- Use las nuevas características y mejoras en el editor de HTML
- Use las nuevas características y mejoras en el editor de JavaScript
- Configurar y usar agrupar y minificar

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (para los scripts de instalación - ya instalados en Windows 8 y Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - o un explorador compatible con HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Ejercicio 1: ¿Qué es nuevo en el Editor de CSS](#Exercise1)
2. [Ejercicio 2: Novedades en el Editor de HTML](#Exercise2)
3. [Ejercicio 3: Novedades en el Editor de JavaScript](#Exercise3)
4. [Ejercicio 4: Agrupación y Minificación](#Exercise4)

Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Ejercicio 1: ¿Qué es nuevo en el Editor de CSS

Los desarrolladores Web deben estar familiarizados con muchas de las dificultades relacionadas con la edición de CSS. Uno de los problemas más importantes de estilos CSS es la compatibilidad de varios exploradores. A menudo sucede que, después de aplicar estilos a su sitio, tenga en cuenta que tiene un aspecto diferente si se abre en otro explorador o dispositivo. Por lo tanto, puede dedicar un tiempo considerable sobre cómo solucionar los problemas visuales para que tenga en cuenta que, cuando se finalmente que funcione en un explorador, se divide en los demás.

Ahora, Visual Studio incorpora características que ayudan a los desarrolladores tener acceso a, trabajo y organizar las hojas de estilos CSS eficazmente. En este ejercicio, se reunirá las nuevas características para una organización eficaz y la edición, así como los fragmentos de código CSS3 para la compatibilidad de varios exploradores.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tarea 1: nuevas características del Editor

En esta tarea, descubrirá las nuevas características del Editor de CSS. Este nuevo editor le ayudará a aumentar su productividad, aprovechando las ventajas de la sangría inteligente nueva, los comentarios de código mejorada y la lista de IntelliSense mejorada.

1. Iniciar **Visual Studio** y abra el **WhatsNewASPNET.sln** soluciones se encuentran en la **Source\WhatsNewASPNET** carpeta de este laboratorio.
2. En el Explorador de soluciones, abra el **Site.css** archivo situado bajo el **estilos** carpeta. Asegúrese de que el **Editor de texto** herramientas están visibles en la barra de herramientas. Para ello, seleccione la **vista** | **las barras de herramientas** opción de menú y compruebe el **Editor de texto** opciones. Observará que, desde esta nueva versión, la **comentario** botón (![comment (botón)-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) y la **sin comentarios** botón (![quite-botón](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) también están habilitadas para el editor de CSS.

    ![Habilitación de Editor y las herramientas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "habilitar Editor y las herramientas CSS")

    *Habilitación de Editor y las herramientas CSS*
3. Desplazar el código y seleccione cualquier definición de clase CSS. Haga clic en el **comentario** (![comment (botón)-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) botón para acotar como comentario las líneas seleccionadas. A continuación, haga clic en el **sin comentarios** (![botón quite](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) botón para deshacer los cambios.
4. Haga clic en el **contraer** (![contraer](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) y **expandir** (![expanda](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) los botones situados en el margen izquierdo del texto. Observe que ahora puede ocultar los estilos que no use disponer de una vista más limpia.

    ![Contraer clases CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "clases CSS contraer")

    *Contraer clases CSS*
5. Asegúrese de que la característica de sangría inteligente está habilitada. Seleccione el **herramientas** | **opciones** opción de menú y, a continuación, seleccione la **Editor de texto** | **CSS**  |  **Formato** página en el panel izquierdo de la pantalla. Compruebe el **sangría jerárquica** opción.

    ![Habilitar sangría jerárquica](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "habilitar sangría jerárquica")

    *Habilitar sangría jerárquica*
6. Busque la definición de clase principal (.main) y anexar un estilo a los elementos div. Observará que el código alinea automáticamente, ayudar a los usuarios para buscar al elemento primario de las clases de un vistazo.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alineación jerárquica en CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "alineación jerárquica en CSS")

    *Alineación jerárquica en CSS*
7. Dentro de **.main div** de clases, colocar el cursor al final de **borde: 0px;** y presione **ENTRAR** para mostrar la lista de IntelliSense. Comience a escribir **arriba** y observe cómo se filtra la lista a medida que escribe. La lista mostrará los elementos que contienen **arriba** en cualquier parte de la palabra (en las versiones anteriores de Visual Studio, la lista se filtra los elementos que *comenzar* con el término).

    ![Mejoras de IntelliSense en CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "mejoras de IntelliSense de CSS")

    *Mejoras de IntelliSense de CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tarea 2: el selector de colores

En esta tarea, se detectará que el nuevo selector de colores de CSS integrado en Visual Studio IntelliSense.

1. En **Site.css,** encontrar la definición de clase de encabezado (.header) y coloque el cursor junto a **color de fondo** atributo, entre el &quot;:&quot; y &quot; # &quot; caracteres en esa línea de código **.**

    ![Buscar el cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "localizar el cursor")

    *Buscar el cursor*
2. Eliminar el **dos puntos** (:) y escribirla de nuevo para mostrar el selector de colores. Observe que los colores de primer que verá los colores más frecuentes de su sitio. Si hace clic en el color blanco, el código de color HTML (#fff) reemplazará el código de color actual en la hoja de estilos.

    ![Selector de colores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selector de colores")

    *Selector de colores*
3. Presione el **expandir** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) botón en el selector de colores para mostrar el degradado de color y, a continuación, arrastre el cursor de degradado para seleccionar un color diferente. A continuación, haga clic en el **Cuentagotas** botón y seleccionar cualquier color de la pantalla. Tenga en cuenta que el valor del color de fondo cambia dinámicamente mientras se mueve el cursor.

    ![Degradado de selector de color](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "degradado de selector de Color")

    *Degradado de selector de color*
4. En el **opacidad** control deslizante, mueva el selector al centro de la barra para reducir la opacidad. Tenga en cuenta que el valor de color de fondo cambia su escala RGBA ahora.

    ![Selector de colores opacidad](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "opacidad del selector de colores")

    *Selector de color de opacidad*

    > [!NOTE]
    > La definición de color RGBA (rojo, verde, azul, alfa) en CSS3 le permite definir el valor de opacidad del color de un solo elemento. A diferencia de **opacidad -** un atributo similar de CSS **-** colores RGBA también son compatibles con los exploradores más recientes.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tarea 3: fragmentos de código Compatible de CSS

En esta tarea, obtendrá información sobre cómo usar los fragmentos de CSS3 de varios exploradores compatibles con el fin de implementar algunas características en el sitio Web.

1. En el **Site.css** de archivos, busque el **encabezado** CSS (.header) de la definición de clase y coloque el cursor debajo del **/ \*radius de borde\* /** marcador de posición para agregar un nuevo fragmento de código. Presione **ENTRAR** para mostrar la lista de IntelliSense y escribe **radius** para filtrar la lista. Seleccione el **border-radius** opción en la lista con un doble clic y, a continuación, presione la **ficha** clave para insertar el fragmento de código. A continuación, escriba un tamaño de radius en píxeles y presione **ENTRAR**. Por ejemplo, escriba **15px**.

    Los atributos de CSS3 agregados por el fragmento de código representarán bordes redondeados en la mayoría de los exploradores de compatibilidad de HTML5, incluidos Mozilla y los exploradores basados en WebKit.

    ![Usar un fragmento de radio del borde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando un fragmento de radio del borde")

    *Usar un fragmento de radio del borde*
2. Aplicar el mismo **borde** fragmentos de código en el estilo de la página (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Presione **F5** para ejecutar la solución. Tenga en cuenta que cada página ahora se redondea los bordes.

    ![Esquinas redondeadas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "esquinas redondeadas")

    *Esquinas redondeadas*
4. Cierre el explorador y vuelva a Visual Studio.
5. Abra la **Custom.css** archivo situado bajo el **estilos** carpeta y coloque el cursor dentro de **div.images ul li img** definición de la clase.
6. Presione ENTRAR para mostrar la lista de IntelliSense, tipo **sombra del cuadro** y presione la **ficha** tecla dos veces para insertar el fragmento de código de instantáneas predeterminada dentro de la definición de clase. Establezca los valores de sombra **10px 10px 5px 888 #**. A continuación, escriba **border-radius** e insertar el fragmento de código. Tipo de **15px** para establecer el tamaño de radius y presione **ENTRAR**.

    ![Redondear las esquinas con sombra](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "redondear las esquinas con sombra")

    *Esquinas redondeadas con sombra*

    > [!NOTE]
    > En este momento, se inserta el atributo de instantáneas con el prefijo correspondiente (moz, webkit, o) para admitir Mozilla y los exploradores Webkit (Chrome, Safari, Konkeror).
7. Cree una nueva clase **div.images ul li img:hover** a continuación el **div.images ul li img** definición de clase y coloque el cursor dentro de los corchetes **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tipo de **transformar** y presione la **ficha** clave dos veces para insertar el fragmento de código de transformación. A continuación, escriba **rotate(-15deg)** para cambiar el valor de ángulo de rotación cuando se mantiene el mouse en imágenes.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Presione **F5** para ejecutar la solución y vaya a la página de CSS3. Tenga en cuenta que las imágenes tienen esquinas redondeadas y cuadro de sombras. Mantenga el mouse sobre las imágenes y observen cómo girar.

    ![Transformar el fragmento de rotación de una imagen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragmento de código de transformación de rotación de una imagen")

    *Transformar el fragmento de rotación de una imagen*

    > [!NOTE]
    > Si está usando Internet Explorer 10 y no puede ver las sombras, asegúrese de que el modo de documento se establece en estándares de IE10. Presione **F12** para abrir herramientas de desarrollo de Internet Explorer y haga clic en **modo de documento** para cambiar a los estándares de IE10.

    ![sobre-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Ejercicio 2: Novedades en el Editor de HTML

Visual Studio tiene un editor de HTML mejorado. Algunas de las mejoras incluidas en esta versión son inteligente sangría en documentos HTML, fragmentos de código de HTML5, inicio HTML y la búsqueda de coincidencias de etiqueta de finalización y validación HTML. En este ejercicio, verá cómo estos cambios mejoran su composición cuando se trabaja en el código de sitio Web.

Al igual que el editor de CSS, también se ha mejorado el editor HTML. La mayoría de estas mejoras son pequeños que facilitan el trabajo del desarrollador de Web. Elementos como fragmentos de código más HTML5, sangría inteligente, la coincidencia de etiquetas de inicio y finalización al editar y validación como destino el documento HTML DOCTYPE son algunas de estas mejoras.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tarea 1: validación de DOCTYPE mejorada

El editor HTML ahora tiene la capacidad de comprobar el tipo de documento de la página, aunque es posible la definición en la página maestra. Según el tipo de documento de la página, el editor HTML se validará con el conjunto correcto de reglas y filtrará la lista de IntelliSense teniendo en cuenta los elementos de tipo de documento.

En esta tarea, cambiará el tipo de documento de una página para ver cómo cambia el comportamiento del editor HTML.

1. Si aún no está abierto, inicie **Visual Studio** y abra el **WhatsNewASPNET.sln** soluciones se encuentran en la **Source\WhatsNewASPNET** carpeta de este laboratorio.
2. Abra la **Site.Master** página.
3. Observe el esquema de destino de la barra de herramientas de validación. La manera en que comporta el editor HTML (validación, IntelliSense, etc.) correctamente cambiará para ajustar el tipo de documento seleccionado.

    ![Usar tipo de documento en la barra de herramientas Edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype de uso en la barra de herramientas Edición de código fuente de HTML")

    *Usar tipo de documento en la barra de herramientas Edición de código fuente de HTML*
4. Cambiar el esquema de destino en HTML 4.01.

    ![Cambiar tipo de documento en la barra de herramientas Edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "cambiar el tipo de documento en la barra de herramientas Edición de código fuente de HTML")

    *Cambiar tipo de documento en la barra de herramientas Edición de código fuente de HTML*
5. Coloque el cursor en el **cuerpo** elemento y empiece a escribir el nombre de un elemento de HTML5 (por ejemplo, **vídeo**). Tenga en cuenta que el elemento no está disponible en la lista de IntelliSense.

    ![Elementos de HTML5 no aparece](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementos de HTML5 no enumerados")

    *Elementos de HTML5 no enumerados*
6. Deshacer los cambios en el esquema de destino para la barra de herramientas de validación, eligiendo DOCTYPE: XHTML5 en la lista desplegable.

    ![Usar tipo de documento en la barra de herramientas Edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype de uso en la barra de herramientas Edición de código fuente de HTML")

    *Restablecer el tipo de documento en la barra de herramientas Edición de código fuente HTML*
7. Coloque el cursor en el **cuerpo** elemento y comience a escribir un elemento de HTML5 nuevo (por ejemplo, como **vídeo**). Observe que los elementos de HTML5 ahora están disponibles en la lista de IntelliSense.

    ![Elementos de HTML5 se enumeran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "aparezca en la lista de elementos de HTML5")

    *Elementos de HTML5 que se muestran*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Actualización automática de etiquetas de la tarea 2 - inicio/fin

Ahora, Visual Studio actualiza el código HTML de apertura o cierre de las etiquetas del elemento que va a editar para coincidir entre sí. Esta nueva característica mejorará la productividad al editar etiquetas HTML.

1. En el **Default.aspx** página, agregue un **H3** elemento con un título (por ejemplo, Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Cambiar el **H3** etiqueta y tipo **H2** o **H1.**

    Observe que la etiqueta de cierre se actualiza automáticamente. También puede modificar la etiqueta de cierre para ver que la etiqueta de apertura se actualiza en consecuencia demasiado.

    ![Actualización automática de la etiqueta de cierre](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "la actualización automática de la etiqueta de cierre")

    *Actualización automática de la etiqueta de cierre*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tarea 3: nuevos fragmentos de código de HTML5

Visual Studio ahora incluye varios fragmentos de código de HTML5. En esta tarea, usará algunos de estos fragmentos de código.

1. Agregar una nueva carpeta denominada **audio** a la raíz de la carpeta del sitio web. Abra el Explorador de Windows y copiar cualquier archivo de audio en el **audio** carpeta de la **WhatsNewASPNET.sln** solución.
2. En el **Default.aspx** página, busque el cursor en el rocas Web11!! Encabezado. Tipo de **audio** y presione la tecla TAB.

    El nuevo editor de HTML incluye fragmentos de código para el contenido de HTML5. Recuerde que debe usar la definición de tipo de documento apropiada para habilitar los fragmentos de código de HTML5.

    ![Insertar fragmentos de código de HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "insertar fragmentos de código de HTML5")

    *Insertar fragmentos de código de HTML5*
3. Actualizar el origen de audio para que señale a un archivo de audio existente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Debe agregar el archivo de audio a la solución.
4. Presione **F5** ejecute el sitio Web y reproduzca el audio.

    ![Ejecuta el control de audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "ejecutando el control de audio")

    *Ejecuta el control de audio*

    > [!NOTE]
    > También puede intentar más fragmentos de código incluidos en Visual Studio, como vídeo, figura, etcetera.
5. Ahora, intente insertar un control en alguna parte de la página. Por ejemplo, al intentar insertar un **GridView** (control), pero en lugar de escribir  **&lt;cuadrícula,** comience a escribir  **&lt;GV**. Tenga en cuenta que la lista de IntelliSense muestra la **asp: GridView** control.

    IntelliSense en el Editor de HTML proporciona ahora la búsqueda de mayúsculas y minúsculas de título, así como parcial coincidente (recuperar todos los elementos que contiene el término).

    ![Insertar un control GridView con listas de IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "insertar un control GridView con listas de IntelliSense")

    *Insertar un control GridView con listas de IntelliSense*

    Si escribe  **&lt;cuadrícula** obtendrá todos los elementos que coinciden con el término, pero Visual Studio le sugerirá la **gridview** control:

    ![Insertar un control GridView con listas de IntelliSense y la búsqueda de coincidencias parciales](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "insertar un control GridView con listas de IntelliSense y la búsqueda de coincidencias parciales")

    *Insertar un control GridView con listas de IntelliSense y la búsqueda de coincidencias parciales*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tarea 4: etiquetas inteligentes de Editor HTML

Otra mejora en el Editor de HTML es la característica de etiquetas inteligentes. Las etiquetas inteligentes facilitan el realizar tareas de desarrollo comunes o repetitivas de forma por control. Esta característica ya estaba disponible en el Diseñador HTML, pero no en el Editor de HTML.

1. Abra **Site.Master** y busque el **asp: menú** elemento. Coloque el cursor en la etiqueta de apertura y observe que el pequeño glifo mostrado en la parte inferior del elemento, haga clic en él para abrir el menú tareas inteligentes. Tenga en cuenta que tiene acceso rápido a algunas tareas relacionadas con el control de menú.

    ![Las tareas para el control de menú de Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentes las tareas para el control de menú")

    *Tareas inteligentes para el control de menú*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tarea 5: sangría inteligente

Uno de los procedimientos recomendados en HTML es aplicar sangría a los elementos anidados para mantener el código legible. En Visual Studio 2012, observará que el editor aplica sangría automáticamente a los elementos mientras escribe el código.

> [!NOTE]
> En la versión anterior de Visual Studio, sangría inteligente estaba disponible en el editor XML, pero no en el editor de HTML.


1. Asegúrese de que la configuración de la sangría en el Editor de HTML se establece en sangría inteligente. Para ello, seleccione la **herramientas | Opciones de** opción de menú y, a continuación, seleccione la **Editor de texto | HTML | Pestañas** página en el panel izquierdo de la pantalla. Seleccione la opción de sangría inteligente.

    ![Configuración del Editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "configuración del Editor de HTML")

    *Configuración del Editor de HTML*
2. En el **Default.aspx** página, elimine todo el contenido en el elemento de audio.
3. Coloque el cursor al final de la apertura **audio** elemento y presione **ENTRAR**.

    Observe que la nueva posición del cursor tiene un nivel de sangría adicional.

    ![Inteligentes sangría en el Editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentes sangría en el Editor de HTML")

    *Sangría inteligente en el Editor de HTML*
4. Restaurar la etiqueta de audio con el contenido se ha quitado o cerrar **Default.aspx** sin guardar los cambios.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tarea 6: extraer a Control de usuario

Las herramientas de refactorización incluidas en Visual Studio, como extraer una parte del código a una función, son estupendas características que facilitan la mejora y refactorizar el código existente. El equivalente para las páginas ASP.NET sería la extracción de código HTML para un Control de usuario. Hacerlo manualmente, implicaría varios pasos, como crear un nuevo Control de usuario, mover la sección de código para el Control de usuario, registrar un prefijo de etiqueta para el Control de usuario y, por último, crear una instancia del Control de usuario en las páginas. Ahora, la nueva *extraer a Control de usuario* herramienta automáticamente realiza todos estos pasos automáticamente.

En esta tarea, utilizará la extracción nueva operación contextual del Control de usuario para generar un nuevo control de usuario desde el código seleccionado.

1. En el **Default.aspx** página, seleccione la **H2** y **audio** elementos.
2. Haga clic en y seleccione **extraer a Control de usuario**.

    ![Extraer a la opción de menú de Control de usuario](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extraer a la opción de menú de Control de usuario")

    *Extraer a la opción de menú de Control de usuario*
3. Escriba un nombre para el nuevo control de usuario. Por ejemplo, **Jukebox.ascx**y, a continuación, haga clic en **Aceptar**.

    ![Guardar el control de usuario extraídos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "guardar el control de usuario extraídos")

    *Guardar el control de usuario extraídos*
4. Observe que el código seleccionado se ha extraído en un control de usuario y la ubicación original del código seleccionado se ha reemplazado por una instancia del nuevo control de usuario.

    ![Página se actualiza automáticamente para utilizar el nuevo control de usuario](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "página se actualiza automáticamente para utilizar el nuevo control de usuario")

    *Página se actualiza automáticamente para utilizar el nuevo control de usuario*
5. Presione **F5** para ejecutar la página y compruebe que funciona el control.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Ejercicio 3: Novedades en el Editor de JavaScript

Escribir o editar código de JavaScript no es una tarea fácil, especialmente cuando la aplicación empieza a crecer en tamaño y encuentre ocuparse de archivos largos y cientos de funciones. Los desarrolladores de script normalmente tienen que hacer alguna tarea adicional para mantener la legibilidad del código y desplazarse a través de archivos. Con la inclusión de las bibliotecas de JavaScript como jQuery, navegación de la secuencia de comandos se ha convertido en un desafío debido a la longitud del código.

Visual Studio ha renovado el editor de JavaScript con el compromiso de que el modo de código accesible y organizada. Muchas características de Visual Studio que ya existían en el Editor de código C# o VB ahora se implementan en el editor de JavaScript: Ir a definición, la sangría automática, la documentación y la validación cuando se escribe. Con la lista de IntelliSense renovada tendrá el catálogo de la función JavaScript a su alcance.

En este ejercicio, obtendrá información sobre algunas de las nuevas características y mejoras del editor de JavaScript. Podrá examinar los archivos de ejemplo y detectar cada una de las nuevas características que harán que la programación de JavaScript más eficaz dentro de Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tarea 1: nuevas características de Editor de JavaScript

Esta tarea se incorporará a algunas de las nuevas características de editor de JavaScript, que se centran en organizar el código y volver a poner una mejor experiencia de usuario.

1. Si aún no está abierto, inicie **Visual Studio** y abra el **WhatsNewASPNET.sln** soluciones se encuentran en la **Source\WhatsNewASPNET** carpeta de este laboratorio.
2. Presione **F5** para ejecutar la aplicación, a continuación, haga clic en el vínculo de JavaScript en la barra de navegación. Actualice la página varias veces y compruebe cómo incrementa el contador.

    ![Contador de páginas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contador de páginas")

    *Contador de páginas*
3. Cierre el explorador y vuelva a Visual Studio.
4. Abra la **JavaScript.aspx** página y busque el **&lt;script&gt;** bloque (se muestra a continuación).

    El código siguiente usa el almacenamiento local de HTML5 para almacenar un *pageLoadCount* variable que almacena el número de veces que el usuario actual ha visitado la página. El almacenamiento local es una base de datos de clave y valor de cliente introducido con el estándar de HTML5. Los datos se guardan en el equipo local, en el explorador del usuario.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Asegúrese de que el tipo de documento se establece en XHTML5 antes de continuar con los siguientes pasos.
5. Modificar el código y tenga en cuenta que IntelliSense para JavaScript incluye características de HTML5, como el almacenamiento local y sus métodos internos.

    ![Características de JavaScript de HTML5 en JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "características de JavaScript de HTML5 en JavaScript")

    *Características de JavaScript de HTML5 en JavaScript*
6. Haga clic en cualquier corchete de apertura (**{**) de la secuencia de comandos de código y tenga en cuenta que se resaltan los corchetes.

    ![Se resaltan los corchetes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "corchetes aparecen resaltados")

    *Se resaltan los corchetes*
7. Quite la función **testAutoAlign()** (seleccione las tres líneas y se puede usar **CTRL** + **K**; **CTRL** + **U**) y busque el cursor dentro del código de función. Presione ENTRAR para agregar una segunda línea. Tenga en cuenta que el código está **alineado** y **sangría automática**.

    ![Código de JavaScript es auto alineado](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "código de JavaScript es auto alineado")

    *Código de JavaScript es auto alineado*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tarea 2: validación de JavaScript

En esta tarea, verá la nueva validación de JavaScript para el estándar de ECMAScript5. Esta característica le ayudará a escribir código JavaScript compatible, al evitar problemas de secuencias de comandos antes de la implementación de sitio.

> [!NOTE]
> Visual Studio 2010 implementa la compatibilidad con ECMAStript3, mientras que Visual Studio 2012 proporciona compatibilidad de ECMAScript5.


1. Abra **ECMA5script5.js** situado bajo el **Scripts\custom** carpeta del proyecto. Ahora va a probar la validación de ECMAScript5 estándar.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Puede consultar la &quot; **uso de strict** &quot; dirección en la primera línea del archivo, lo que permite ECMAScript5 **modo strict**. Este modo se compone de un subconjunto del lenguaje que clarifica las ambigüedades de la edición anterior y agrega algunas características nuevas, como los captadores y establecedores, compatibilidad con bibliotecas de JSON y reflexión más completa de propiedades del objeto.
2. Abra la **lista de errores** si aún no está abierto (**vista** menú | **Lista de errores**). Observe el **función** declaración está subrayada. Esto es porque en ECMA5 funciones estándar no se pueden anidar dentro de las estructuras del lenguaje. En el error lista debajo de usted verá los detalles de la advertencia.

    ![Mensaje de error de validación de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "mensaje de error de validación de JavaScript")

    *Mensaje de error de validación de JavaScript*
3. Convierta en comentario la **&quot;uso de strict&quot;** dirección y observe que desaparecen de errores, pero permanecen las advertencias.
4. En la última línea del archivo, escriba cualquier cadena como **&quot;probar&quot;** (incluir las comillas para indicar es como cadena). Escribir un período junto a la cadena para mostrar la lista de IntelliSense y seleccionar la **recortar** opción.

    En el estándar de ECMAScript5, variables y valores de cadena también tienen métodos de cadena definidos, como recortar, mayúsculas, buscar y reemplazar.

    ![Lista de IntelliSense de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "lista IntelliSense de JavaScript")

    *Lista de IntelliSense de JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tarea 3: documentación XML para JavaScript

En esta tarea, explorará las características de Visual Studio para la documentación de XML de JavaScript. Verá que la lista de IntelliSense para JavaScript muestra la documentación de XML de cada función. Además, detectará la característica de navegación en JavaScript.

1. Abra **XMLDoc.js** archivo se encuentra en **secuencias de comandos/custom** carpeta del proyecto. Este archivo contiene la documentación de XML en cada una de las funciones de JavaScript.

    ![Documentación de XML de JavaScript integrados en IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "integrado de documentación XML de JavaScript IntelliSense")

    *Documentación de XML de JavaScript integrada en IntelliSense*
2. A continuación **agregar** funcionando en **XMLDoc.js** , cree una nueva función denominada **probar**.
3. En el **probar** función, llame a la **multiplicar** función que recibe dos parámetros. Tenga en cuenta que se muestra el cuadro de información sobre herramientas la **multiplicar** función documentación.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentación XML para las funciones de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentación XML para las funciones de JavaScript")

    *Documentación XML para las funciones de JavaScript*
4. Completar la instrucción de llamada de función y el tipo de un *punto* para abrir la lista de IntelliSense en el valor devuelto. Tenga en cuenta que Visual Studio está detectando el **devolver** valor en la documentación, tratar el valor como un número.

    ![Documentación XML para tipos de valor devuelto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentación XML para tipos de valor devuelto")

    *Documentación XML para tipos de valor devuelto*
5. Ahora, inserte una llamada a add (función). Tenga en cuenta que el editor de JavaScript ahora es compatible con las sobrecargas de función. Cuando se escribe un nombre de función, podrá seleccionar cualquiera de las sobrecargas disponibles que se especifica en la documentación.

    ![Documentación XML para las sobrecargas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentación XML para las sobrecargas")

    *Documentación XML para las sobrecargas*
6. Abra **GotoDefinition.js** de archivos y busque la **$().html()** llamada de función. Busque el cursor en **html**.
7. Presione **F12** y navegar a la definición. Observe que ahora puede obtener acceso y examinar el código de JavaScript sin usar la **buscar** herramienta.
8. Busque el cursor en la instrucción de jQuery antes el bloque de firma en la parte inferior del archivo de código. Presione **F12**. Se le remitirá al archivo de biblioteca de jQuery. Tenga en cuenta también puede desplazarse a través de los archivos de jQuery mediante **F12**.

    ![Desplazarse a las definiciones de jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "desplazarse a las definiciones de jQuery")

    *Desplazarse a las definiciones de jQuery*

> [!NOTE]
> Asegúrese de que GotoDefinition.js no tiene ningún error de sintaxis antes de guardar el archivo.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Ejercicio 4: Agrupación y Minificación

¿Cuántas veces ¿sus sitios Web incluyen JavaScript o CSS de más de un archivo? Se trata de un escenario muy común donde agrupar y minificar pueden ayudar a reducir el tamaño de archivo y que el sitio se ejecute con mayor rapidez. La nueva característica de agrupación de ASP.NET 4.5 los módulos de un conjunto de archivos Javascript o CSS en un único elemento y reduce su tamaño al minificar el contenido (es decir, quitar los espacios en blanco no son necesarios, quitar comentarios, lo que reduce los identificadores).

Agrupar y minificar en ASP.NET 4.5 se realiza en tiempo de ejecución, para que el proceso pueda identificar el agente de usuario (por ejemplo, Internet Explorer, Mozilla etcetera) y, por lo tanto, mejorar la compresión estableciendo como destino el explorador del usuario (por ejemplo, quitar material que es específico de Mozilla Cuando la solicitud proviene de Internet Explorer).

En este ejercicio, obtendrá información sobre cómo habilitar y utilizar los distintos tipos de agrupación y minificación en ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tarea 1: instalar la agrupación y Minificación paquete de NuGet

1. Si aún no está abierto, inicie **Visual Studio** y abra el **WhatsNewASPNET.sln** soluciones se encuentran en la **Source\WhatsNewASPNET** carpeta de este laboratorio.
2. Abra la consola de administrador de paquetes de NuGet. Para ello, use el menú **vista** | **otras ventanas** | **Package Manager Console**.

    ![Abrir la file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole manager paquete](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "abrir la consola de administrador de paquetes")

    *Abra la consola de administrador de paquetes*
3. En el **consola de administrador de paquetes,** tipo **Install-Package Microsoft.Web.Optimization** y presione **ENTRAR**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tarea 2: paquetes de forma predeterminada

Es la manera más sencilla de usar agrupar y minificar habilitar las agrupaciones de forma predeterminada. Este método usa las convenciones que le permite hacer referencia a la versión reducida e integrada para los archivos Javascript y CSS en una carpeta.

En esta tarea, obtendrá información sobre cómo habilitar y hacer referencia a los archivos Javascript y CSS agrupados y reducidos y ver el resultado.

1. Si aún no está abierto, inicie **Visual Studio** y abra el **WhatsNewASPNET.sln** soluciones se encuentran en la **Source\WhatsNewASPNET** carpeta de este laboratorio.
2. En el **el Explorador de soluciones**, expanda la **estilos**, **Scripts\custom** y **Scripts\bundle** carpetas.

    Tenga en cuenta que la aplicación está usando un archivo de más de CSS y Javascript.

    ![Archivos de varias hojas de estilos y JavaScript de la aplicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "archivos varias hojas de estilos y JavaScript en la aplicación")

    *Varios archivos de hojas de estilos y JavaScript de la aplicación*
3. Abra la **Global.asax.cs** archivo.

    Tenga en cuenta que la nueva **Microsoft.Web.Optimization** espacio de nombres está comentada al principio del archivo. Quite el uso de la directiva para incluir las características de agrupación y minificación.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Busque la **aplicación\_iniciar** método.

    En este método, quite la marca de comentario la llamada EnableDefaultBundles tal y como se muestra en el siguiente fragmento. Esto nos permite hacer referencia a una colección de archivos CSS en una carpeta integrada mediante el uso de la ruta de acceso a esa carpeta, además de la interfaz &quot;CSS&quot; o &quot;JS&quot; sufijo.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Abra la **Optimization.aspx** de archivos y busque el control de contenido para **HeadContent**.

    Tenga en cuenta los archivos CSS y los archivos JS tiene una única etiqueta que se hace referencia.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Este código es para fines de demostración. Idealmente, se hará referencia a los paquetes en el archivo Site.Master. En este ejemplo de código, encontrará algunos de los archivos integrados también se hace referencia mediante el archivo Site.Master, facilitan esta última referencia redundantes.
6. Tenga en cuenta que los vínculos están usando las convenciones de agrupación en el **href** atributo para obtener archivos CSS o Javascript desde los estilos y Scripts\custom carpeta respectivamente.

    Puede usar la ruta de acceso **secuencias de comandos/personalizado/JS** tal y como se muestra a continuación para agrupar y minificar todos los archivos JS dentro de un **secuencias de comandos/custom** carpeta. Éste es el comportamiento predeterminado con las agrupaciones de forma predeterminada.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Abra la **Styles\Site.css** archivo.

    Observe que el archivo original de CSS contiene código con sangría, espacios en blanco y los comentarios que aumentan el archivo. (También el archivo JavaScript contiene comentarios y espacios en blanco).

    ![Una de las reglas de CSS original archivos en la carpeta Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "una de las reglas de CSS original archivos en la carpeta de Scripts")

    *Uno de los archivos CSS originales en la carpeta de Scripts*
8. Presione **F5** para ejecutar la aplicación y navegar hasta el **optimización** página.
9. Haga clic en el **agrupación de CSS** vínculo para descargar y abrir el archivo.

    Desproteger el archivo agrupado reducido. Observará que se han quitado todos los espacios en blanco, comentarios y caracteres de sangría, generar un archivo más pequeño.

    ![Los archivos CSS agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "archivos CSS agrupadas")

    *Archivos CSS agrupados*
10. Ahora haga clic en el **JS agrupación** vínculo para abrir el archivo JavaScript agrupada. Puede omitir de forma segura el Explorador de advertencia. Tenga en cuenta los archivos JavaScript en el **personalizado** carpeta también se empaquetan y reducido.

    ![Los archivos JavaScript agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "archivos JavaScript agrupadas")

    *Archivos JavaScript agrupados*

    Al habilitar la compresión de archivos CSS o JS era mucho más complicado en la versión anterior de ASP.NET. Ahora, como se ha visto, solo necesita agregar una línea en el *Global.asax* para habilitar la agrupación de archivos y, a continuación, hacer referencia a los archivos integrados desde su sitio.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tarea 3: paquetes estáticos

El enfoque de agrupación estática permite personalizar el conjunto de archivos para la agrupación, la referencia y el método de reducción que va a utilizar.

En esta tarea, configurará una agrupación estática para definir un conjunto específico de archivos para agrupar y minificar.

1. Cierre el explorador.
2. Abra la **Global.asax.cs** de archivos y busque la **aplicación\_iniciar** método.
3. Quite el código de agrupación estática tal como se muestra en el código siguiente.

    Define una agrupación estática que se hará referencia a la &quot; **~/StaticBundle** &quot; ruta de acceso virtual y el uso **JsMinify** para minificación de todos los archivos especificados con el **AddFile** método. Por último, va a agregar la agrupación estática a la **BundleTable** y habilitarlo.

    Tenga en cuenta que los archivos no se encuentran en el mismo lugar; Esta es otra ventaja con respecto a la agrupación de forma predeterminada.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Abra la **Optimization.aspx** archivo.

    Tenga en cuenta que el vínculo a **estático agrupación JS** está usando la ruta de acceso que se ha declarado cuando se configura la agrupación estática en el archivo Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Presione **F5** para ejecutar la aplicación y, a continuación, navegue hasta la **optimización** página.
6. Haga clic en el **estático agrupación JS** vínculo para abrir el archivo.

    Tenga en cuenta que la reducida agrupadas archivo JavaScript es el resultado para todos los archivos de JavaScript que se configura en el archivo de agrupación estática en la ruta de acceso &quot;/StaticBundle&quot;.

    ![Agrupación de archivos JavaScript estático](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "agrupación de archivos JavaScript estáticos")

    *Agrupación archivos JavaScript estáticos*
7. Cierre el explorador y vuelva a Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tarea 4: paquetes de la carpeta dinámica

En esta tarea, aprenderá a configurar los paquetes de la carpeta dinámica. La capacidad de agrupación dinámica es que puede incluir JavaScript estático, así como otros archivos en idiomas que se compila en JavaScript y, por lo tanto, requieren algún procesamiento antes de que se ejecuta la agrupación.

En este ejemplo, obtendrá información sobre cómo utilizar el **DynamicFolderBundle** clase para crear una agrupación dinámica para los archivos escritos en CofeeScript. CofeeScript es un lenguaje de programación que se compila en JavaScript y proporciona una sintaxis más sencilla para escribir código JavaScript, mejora la legibilidad y la brevedad de JavaScript.

1. Abra la **Global.asax.cs** de archivos y busque la **aplicación\_iniciar** método.
2. Quite el código de agrupación dinámica tal como se muestra en el código siguiente.

    Define una agrupación de carpetas dinámica que utilizará el **CoffeeMinify** procesador minificación personalizado que únicamente se aplicará a los archivos con la &quot; **.coffee** &quot; (extensión Archivos de CoffeeScript). Observe que puede utilizar un patrón de búsqueda para seleccionar los archivos para empaquetar en una carpeta, como '\*.coffee'.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Abra la consola de administrador de paquetes de NuGet. Para ello, use el menú **vista** | **otras ventanas** | **Package Manager Console**.
4. En el **consola de administrador de paquetes,** tipo **Install-Package CoffeeSharp** y presione **ENTRAR**.
5. Haga clic en el **mostrar todos los archivos** botón en el **el Explorador de soluciones** ventana

    ![Mostrar todos los archivos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "mostrar todos los archivos")

    *Mostrar todos los archivos*
6. Haga clic con el **CoffeeMinify.cs** un archivo en el **el Explorador de soluciones** y seleccione **incluir en el proyecto**

    ![Incluir el archivo CoffeeMinify.cs en el proyecto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "incluir el archivo CoffeeMinify.cs en el proyecto")

    *Incluir el archivo CoffeeMinify.cs en el proyecto*
7. Abra la **CoffeeMinify.cs** archivo.

    Esta clase hereda de JsMinify a minificar la salida de JavaScript resultante de la compilación de código de CoffeeScript. Lo llama el compilador de CoffeeScript para generar el código de JavaScript primero y, a continuación, envía al método JsMinify.Process para minificar el código resultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Abra la **Script1.coffee** y **Script2.coffee** archivos desde el **Scripts/agrupación** carpeta.

    Estos archivos incluirá el código CoffeScript a compilarse al realizar la unión con la clase CoffeeMinify.

    Por motivos de simplicidad, los archivos de CoffeeScript proporcionados solo incluye código de ejemplo de CoffeeScript. Los comentarios se excluyen mediante el proceso de JsMinify.

    ![Archivos de CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "archivos de CoffeeScript")

    *Archivos de CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) proporciona una sintaxis más sencilla para escribir código JavaScript, mejora la legibilidad y la brevedad de JavaScript, así como agregar otras características, como la comprensión de la matriz y la coincidencia de patrones.
9. Abra la **Optimization.aspx** de archivos y busque los vínculos de agrupación.

    Tenga en cuenta que el vínculo a **agrupación dinámica de JS** hace referencia a la **secuencias de comandos/agrupación** carpeta utilizando el **/café** sufijo configurado para la agrupación de carpetas dinámica.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Presione **F5** para ejecutar la aplicación y, a continuación, navegue hasta la **optimización** página.
11. Haga clic en el **agrupación dinámica de JS** vínculo para abrir el archivo generado.

    Tenga en cuenta que solo contiene el contenido que se incluye en este paquete **.coffee** archivos. También puede ver que el código de CoffeeScript se compiló con JavaScript y se ha quitado las líneas de comentarios.

    ![Agrupación archivos JS dinámicos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dinámica archivos agrupación")

    *Agrupación archivos JS dinámicos*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Resumen

Esta práctica le ayudará a comprender lo que es nuevo en ASP.NET y desarrollo Web en Visual Studio 2012 y cómo aprovechar las ventajas de la serie de mejoras en Visual Studio 2012.

Tras completar este laboratorio práctico, que aprendió cómo usar las nuevas características y mejoras en el Editor de Visual Studio 2012 de CSS, JavaScript y HTML. Además, que aprendió cómo Visual Studio 2012 implementa integrados agrupar y minificar.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Express de VS para icono Web*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.

    ![Descargar el sitio web de perfil de publicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto. Escriba un nombre para la regla y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) botón.

    ![Agregar dirección IP del cliente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmar cambios*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configurar la cadena de conexión de destino](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "configurar la cadena de conexión de destino")

     *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplicación se publicó en Windows Azure")

    *Aplicación que se publican en Windows Azure*
