---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: "Con el Inspector de página en Visual Studio 2012 | Documentos de Microsoft"
author: rick-anderson
description: "En este laboratorio práctico, verá una nueva herramienta para buscar y corregir problemas de la página web en Visual Studio, el Inspector de página. Inspector de página es una nueva herramienta que b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Con el Inspector de página en Visual Studio 2012
====================
por [Web colonias equipo](https://twitter.com/webcamps)

> En este laboratorio práctico, verá una nueva herramienta para buscar y corregir problemas de la página web en Visual Studio, el Inspector de página.
> 
> Inspector de página es una herramienta nueva que ofrece herramientas de diagnóstico de explorador a Visual Studio y proporciona una experiencia integrada entre el explorador, ASP.NET y código fuente. Presenta una página web (HTML, formularios Web Forms, ASP.NET MVC o Web Pages) directamente en el IDE de Visual Studio y le permite examinar el código fuente y el resultado. Inspector de página permite descomponer fácilmente un sitio Web, generar páginas rápidamente desde el principio y diagnosticar los problemas rápidamente.
> 
> Hoy en día contamos con un número de marcos de trabajo Web que cree sitios Web flexible y escalable de manera oportuna, como ASP.NET MVC y formularios Web Forms. Por otro lado, obtiene más difícil encontrar problemas en las páginas porque el IDE no es compatible con la vista del diseñador en páginas basadas en plantillas y contenido dinámico. Por lo tanto, estos sitios Web deben estar abiertos en un explorador para ver cómo aparecen para un usuario.
> 
> Los desarrolladores Web use herramientas externas para detectar problemas que se ejecutan con regularidad en el explorador. A continuación, se volver al IDE y empezar a reparar. Esto y hacia atrás actividad entre el IDE, el explorador y las herramientas de generación de perfiles puede ser ineficaz y a veces requiere una implementación nueva y cada vez que desee reproducir un problema de limpieza de caché.
> 
> Inspector de página llena una discontinuidad en desarrollo Web entre el cliente (herramientas del explorador) y el servidor (ASP.NET y código fuente), ya que reúne lo mejor de ambos mundos mediante el uso de un conjunto combinado de características.
> 
> Inspector de página puede ver qué elementos de los archivos de origen (incluido el código de servidor) han creado el marcado HTML que se representará en el explorador. Inspector de página también le permite modificar las propiedades CSS y los atributos del elemento DOM para ver los cambios reflejados inmediatamente en el explorador.
> 
> Este laboratorio práctico se le guían a través de las características de Inspector de página y muestran cómo se puede usar para solucionar problemas en aplicaciones Web. **Este laboratorio contiene dos ejercicios flujos similares pero diferentes tecnologías de destino. Si es un desarrollador de MVC de ASP.NET, siga ejercicio uno; Si es un ejercicio de seguimiento para desarrolladores de formularios Web Forms dos**.
> 
> Esta práctica le guiará a través de las mejoras y nuevas características descritos anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Descomponer un sitio Web mediante el Inspector de página
- Inspeccionar y obtener una vista previa de los cambios de estilos CSS con el Inspector de página
- Detectar y corregir problemas en las páginas web con el Inspector de página

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).
- Internet Explorer 9 o superior

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Ejercicio 1: Usar Inspector de página en proyectos de ASP.NET MVC](#Exercise1)
2. [Ejercicio 2: Usar Inspector de página en los proyectos de formularios Web Forms](#Exercise2)

> [!NOTE]
> Cada ejercicio está acompañado por una solución inicial, que se encuentra en la carpeta de inicio del ejercicio, que le permite seguir cada ejercicio independientemente de las otras. En el código fuente de un ejercicio, también encontrará una carpeta final que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos en el ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional cuando se trabaja a través de este laboratorio práctico.


Tiempo estimado para completar esta práctica: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Ejercicio 1: Usar Inspector de página en proyectos de ASP.NET MVC

En este ejercicio, obtendrá información sobre cómo obtener una vista previa y depurar un **ASP.NET MVC 4** solución mediante **Inspector de página**. En primer lugar, se llevará a cabo una breve aspectos básicos relativos a la herramienta para obtener información sobre las características que facilitan el proceso de depuración de Web. A continuación, trabajará en una página web que contiene los problemas de aplicación de estilos. Obtendrá información sobre cómo usar Inspector de página para buscar el código fuente que genera el problema y solucionarlo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarea 1: explorar el Inspector de página

En esta tarea, aprenderá cómo utilizar el Inspector de página en el contexto de un proyecto de ASP.NET MVC 4 que muestra una galería de fotografías.

1. Abra la **comenzar** solución ubicado en **origen/Ex1-MVC4/Begin/** carpeta.

    1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. En el Explorador de soluciones, busque **Index.cshtml** ver en el **/vistas/inicio** carpeta del proyecto, haga clic en él y seleccione **ver en Inspector de página**.

    ![Al seleccionar un archivo para obtener una vista previa en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image1.png "al seleccionar un archivo para obtener una vista previa en Inspector de página")

    *Al seleccionar un archivo para obtener una vista previa en Inspector de página*
3. La ventana de Inspector de página mostrará el */principal/índice* dirección URL asignada al origen de vista que ha seleccionado.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *El primer contacto con el Inspector de página*

    La herramienta de Inspector de página está integrada en el entorno de Visual Studio. El inspector contiene un explorador integrado, junto con un generador de perfiles HTML eficaz. Tenga en cuenta que no es necesario ejecutar la solución para ver el aspecto de las páginas.

    > [!NOTE]
    > Cuando el ancho del explorador de Inspector de página es menor que el ancho de la página abierta, no verá la página correctamente. Si esto sucede, ajuste el ancho del Inspector de página.
4. Haga clic en el **archivos** ficha en Inspector de página.

    Verá todos los archivos de origen que están creando la página de índice. Esta característica ayuda a identificar todos los elementos de una ojeada, especialmente cuando se trabaja con las vistas parciales y plantillas. Tenga en cuenta que también puede abrir cada uno de los archivos si hace clic en los vínculos.

    ![La ficha de archivos](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *La pestaña archivos*
5. Haga clic en el **pasar al modo de inspección** situado a la izquierda de las pestañas.

    Esta herramienta le permitirá seleccionar cualquier elemento de la página y ver el código HTML y Razor.

    ![Inspección-modo-botón de alternancia](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Botón de alternancia modo de inspección*
6. En el Explorador de Inspector de página, mueva el puntero del mouse sobre los elementos de la página. Mientras se mueve el puntero del mouse sobre alguna parte de la página presentada, se muestra el tipo de elemento y el marcado del código fuente o el código correspondiente se resalta en el editor de Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modo de inspección en acción*

    > [!NOTE]
    > No se maximice la ventana del Inspector de página o no podrá ver la pestaña de vista previa que muestra el código fuente. Si hace clic en el elemento en el Inspector de página cuando está maximizado, aparecerá el código fuente de la selección, pero ocultará la ventana del Inspector de página.

    Si se preste atención a la **Index.cshtml** archivo, observará que la parte del código fuente que genera el elemento seleccionado se resalta. Esta característica facilita la edición de archivos de origen largo, proporcionan una manera directa y rápida de obtener acceso al código.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspeccionar elementos*
7. Haga clic en el **pasar al modo de inspección** botón (![seleccione la pestaña HTML para mostrar el código HTML que se representa en el Explorador de Inspector de página.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Seleccione la pestaña HTML para mostrar el código HTML que se representa en el Explorador de Inspector de página.") ) para deshabilitar el cursor.
8. Seleccione el **HTML** ficha para mostrar el código HTML que se representa en el Explorador de Inspector de página.
9. En el código HTML, busque el elemento de lista con el vínculo Koala y selecciónelo.

    Tenga en cuenta que, cuando se selecciona el código, el resultado correspondiente se resalta automáticamente en el explorador. Esta característica es útil para ver cómo se representa un bloque HTML en la página.

    ![Si selecciona el elemento de HTML en la página](using-page-inspector-in-visual-studio-2012/_static/image8.png "elemento de selección HTML en la página")

    *Seleccionar elemento HTML en la página*
10. Haga clic en el **pasar al modo de inspección** botón Habilitar *modo de inspección* y haga clic en la barra de navegación. A la derecha del código HTML, en el panel Estilos, verá una lista con los estilos CSS que se aplican al elemento seleccionado.

    > [!NOTE]
    > Dado que el encabezado es una parte del diseño del sitio, también se abrirá el Inspector de página \_Layout.cshtml archivo y resaltar afecta el segmento de código.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Detección de estilos y los archivos de código fuente de un elemento seleccionado*
11. Con el puntero de inspección de alternancia habilitado, mueva el puntero del mouse (ratón) debajo de la barra azul destacado y haga clic en el semicírculo.

    ![Selección de un elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "selección de un elemento")

    *Selección de un elemento*
12. En el panel Estilos, busque el **imagen de fondo** de elemento en el **.main-content** grupo. **Desactive la opción** el **imagen de fondo** y vea Qué sucede. Observará que el explorador reflejará los cambios inmediatamente y se oculta el círculo.

    > [!NOTE]
    > Los cambios que se apliquen en la pestaña estilos de Inspector de página no afectan a la hoja de estilos original. Puede desactivar estilos o cambiar sus valores tantas veces como desee, pero se restaurarán después de actualizar la página.

    ![Habilitar y deshabilitar estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "habilitar y deshabilitar estilos CSS")

    *Habilitar y deshabilitar estilos CSS*
13. Ahora, haga clic en el '**su logotipo aquí**' texto en el encabezado mediante el modo de inspección.
14. En el **estilos** ficha, busque el **tamaño de fuente** atributo CSS en el **.site título** grupo. Haga doble clic en el valor del atributo y reemplace el valor de 2.3 em con **em 3**y, a continuación, presione **ENTRAR**. Tenga en cuenta que el título tiene un aspecto más grande.

    ![Cambiar valores CSS en el Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image12.png "valores de cambio de CSS en el Inspector de página")

    *Cambiar valores CSS en el Inspector de página*
15. Haga clic en el **realizar seguimiento de estilos** ficha, que se encuentra en el panel derecho del Inspector de página. Se trata de una manera alternativa para ver todos los estilos aplicados a la selección, ordenada por nombre de atributo.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Seguimiento de estilos CSS del elemento seleccionado*
16. Otra característica del Inspector de página es el panel de diseño. Con el modo de inspección, seleccione la barra de navegación y, a continuación, haga clic en el **diseño** ficha en el panel derecho. Verá el tamaño exacto del elemento seleccionado, así como su tamaño de desplazamiento, margen, relleno y borde. Tenga en cuenta que también puede modificar los valores de esta vista.

    ![Diseño de un elemento en el Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image14.png "diseño de un elemento en el Inspector de página")

    *Diseño de un elemento en el Inspector de página*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarea 2: buscar y corregir problemas de estilo en la Galería fotográfica

¿Forma de diagnosticar problemas de páginas Web con versiones anteriores de Visual Studio? Probablemente esté familiarizado con las herramientas de depuración que se ejecutan fuera del IDE de Visual Studio, como herramientas de desarrollo de Internet Explorer o Firebug de web. Exploradores solo comprenden HTML, secuencias de comandos y estilos, mientras que un marco de trabajo subyacente genera el código HTML que se representará. Por esta razón, a menudo necesitará implementar todo el sitio para ver el aspecto de las páginas web como.

Probablemente tenía seguido estos pasos si desea detectar y corregir un problema en el sitio web:

1. Ejecutar la solución desde Visual Studio, o implementar la página en el servidor web.
2. En el explorador, abra las herramientas de desarrollo que se utiliza, o simplemente abra el código fuente y los estilos e intente coincide con el problema. Para buscar los archivos implicados, que se ha utilizado el &quot;búsqueda&quot; o &quot;buscar en archivos&quot; características con el nombre de las clases de estilo.
3. Una vez detectado el error, detenga el explorador Web y el servidor.
4. Borre la memoria caché de exploración.
5. Vuelva a Visual Studio para aplicar una corrección. Repita todos los pasos para probar.

Como no existe ningún WYSIWYG real en ASP.NET MVC 4, la mayoría de los problemas de estilo se detecta en una fase posterior, después de ejecutar o implementar la aplicación web. Ahora, con el Inspector de página, es posible obtener una vista previa de cualquier página sin ejecutar la solución.

En esta tarea, utilizará el inspector de página y corregir algunos problemas de la aplicación de la Galería fotográfica.

1. Con el Inspector de página, busque el **registrar** y el **sesión** vínculos en la parte izquierda del encabezado.

    Observe que los vínculos no aparecen en el lugar esperado a la derecha, y se muestran como una lista con viñetas. Ahora podrá alinear los vínculos a la derecha y cambiar el estilo de ellos como corresponda.

    ![Buscar el registro y el registro en vínculos](using-page-inspector-in-visual-studio-2012/_static/image15.png "buscar el registro y el registro de vínculos")

    *Buscar el registro y el registro de vínculos*
2. Con pasar al modo de inspección seleccionado, haga clic en Cerrar para, pero no en el vínculo de registro para abrir su código.

    Tenga en cuenta que el código fuente de los vínculos se encuentra en la  **\_LoginPartial.cshtml** de archivos, no el Index.cshtml ni \_Layout.cshtml, que son los lugares podría ser en primer lugar. Han colocado directamente en el archivo de código fuente correcto.
3. En el **estilos** ficha, busque y haga clic en el  **<section> #login</section>**  elemento, que es el contenedor HTML para que estos vínculos.

    Tenga en cuenta que la **#login** estilo automáticamente se encuentra en **Site.css** tras hacer clic en. Además, ahora se resalta el código.

    ![Seleccionar los estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "al seleccionar los estilos CSS")

    *Seleccionar los estilos CSS*
4. Quite el **alinear texto** mediante la eliminación de los caracteres de cierre y apertura de atributo en el código resaltado y guarde el **Site.css** archivo.

    Inspector de página es consciente de todos los archivos que componen la página actual y puede detectar cuándo cambia cualquiera de estos archivos. Le avisa cuando la página actual en el explorador no está sincronizada con los archivos de origen.
5. En el Explorador de Inspector de página, haga clic en la barra situada debajo de la barra de direcciones para volver a cargar la página.

    ![Volver a cargar la página](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Volver a cargar la página*

    Los vínculos son ahora a la derecha, pero todavía quedarán una lista con viñetas. Ahora, debe quitar las viñetas y Alinear horizontalmente los vínculos.

    ![Página actualizada](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Página actualizada*
6. Con el modo de inspección, seleccione cualquiera de los  **&lt;li&gt;**  elementos que contienen el &quot;registrar&quot; y &quot;iniciar sesión&quot; vínculos. A continuación, haga clic en el  **&lt;sección&gt; #login** de elemento para tener acceso a **Styles.css** código.

    ![Buscar el estilo](using-page-inspector-in-visual-studio-2012/_static/image19.png "buscar el estilo")

    *Buscar el estilo*
7. En **Style.css**, quite el código de **#login li** elementos. El estilo que se va a agregar se oculte la viñeta y mostrar los elementos horizontalmente.

    ![Cambiar el estilo de los vínculos de inicio de sesión](using-page-inspector-in-visual-studio-2012/_static/image20.png "cambiar el estilo de los vínculos de inicio de sesión")

    *Cambiar el estilo de los vínculos de inicio de sesión*
8. Guardar **Style.css** de archivo y haga clic una vez en la barra situada debajo de la dirección a cargar la página. Observe que los vínculos aparecen correctamente.

    ![Vínculos alinean a la derecha](using-page-inspector-in-visual-studio-2012/_static/image21.png "vínculos alinean a la derecha")

    *Vínculos alineado a la derecha*
9. Por último, cambiará el título de encabezado. Haga clic con el modo de inspección **su logotipo aquí** texto y get en el código fuente que lo genera.
10. Ahora está en  **\_Layout.cshtml**, reemplace '**su logotipo aquí**'text con'**Galería fotográfica**'. Guarde y actualice el Explorador de Inspector de página.

    ![Asignar un nuevo título](using-page-inspector-in-visual-studio-2012/_static/image22.png "asignar un nuevo título")

    *Asignar un nuevo título*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Página de la Galería fotográfica actualizado*
11. Por último, elija la **PhotoGallery** proyecto y presione **F5** para ejecutar la aplicación. Compruebe todos los cambios funcionan según lo esperado.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Ejercicio 2: Usar Inspector de página en los proyectos de formularios Web Forms

En este ejercicio, obtendrá información sobre cómo obtener una vista previa y depurar una solución de formularios Web Forms con el Inspector de página. En primer lugar se llevará a cabo una breve aspectos básicos relativos a la herramienta para obtener información sobre las características de Inspector de página que facilitan el proceso de depuración de Web. A continuación, trabajará en una página web que contiene los problemas de aplicación de estilos. Obtendrá información sobre cómo usar Inspector de página para buscar el código fuente que genera el problema y solucionarlo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarea 1: explorar el Inspector de página

En esta tarea, aprenderá a usar las características de Inspector de página en el contexto de un proyecto de formularios Web Forms que muestra una galería de fotografías.

1. Abra la **comenzar** solución ubicado en **origen/Ex2-WebForms/Begin/** carpeta.

    1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. En el Explorador de soluciones, busque **Default.aspx** página, haga clic en él y seleccione **ver en Inspector de página**.

    ![Abrir Default.aspx con el Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image24.png "abrir Default.aspx con el Inspector de página")

    *Abrir Default.aspx con el Inspector de página*
3. La ventana de Inspector de página mostrará Default.aspx.

    ![Ver Default.aspx en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image25.png "ver Default.aspx en Inspector de página")

    *Ver Default.aspx en Inspector de página*

    La herramienta de Inspector de página está integrada en el entorno de Visual Studio. El inspector contiene un explorador integrado, junto con un generador de perfiles eficaz de HTML que muestra el código seleccionado. Tenga en cuenta que no es necesario ejecutar la solución para ver el aspecto de las páginas.

    > [!NOTE]
    > Cuando el ancho del explorador de Inspector de página es menor que el ancho de la página abierta, no verá la página correctamente. Si esto sucede, ajuste el ancho del Inspector de página.
4. Haga clic en el **archivos** ficha en Inspector de página.

    Verá todos los archivos de origen que están creando la página predeterminada representada. Se trata de una característica útil para identificar todos los elementos de una ojeada, especialmente cuando se trabaja con controles de usuario y las páginas maestras. Tenga en cuenta que también puede desplazarse a cada uno de los archivos.

    ![La pestaña archivos](using-page-inspector-in-visual-studio-2012/_static/image26.png "los archivos (ficha)")

    *La pestaña archivos*
5. Haga clic en el **pasar al modo de inspección** situado a la izquierda de las pestañas.

    Esta herramienta le permitirá seleccionar cualquier elemento de la página y ver su fuente .aspx y de código HTML.

    ![Botón de alternancia modo de inspección](using-page-inspector-in-visual-studio-2012/_static/image27.png "botón de alternancia modo de inspección")

    *Botón de alternancia modo de inspección*
6. En el Explorador de Inspector de página, mueva el mouse sobre los elementos de la página. Mientras se mueve el puntero del mouse sobre alguna parte de la página presentada, se muestra el tipo de elemento y el marcado del código fuente o el código correspondiente se resalta en el editor de Visual Studio.

    ![Modo de inspección en acción](using-page-inspector-in-visual-studio-2012/_static/image28.png "modo de inspección en acción")

    *Modo de inspección en acción*

    > [!NOTE]
    > No se maximice la ventana del Inspector de página o no podrá ver la pestaña de vista previa que muestra el código fuente. Si hace clic en el elemento en el Inspector de página cuando está maximizado, aparecerá el código fuente de la selección, pero ocultará la ventana del Inspector de página.

    Si se preste atención a **Default.aspx** archivo, observará que la parte del código fuente que genera el elemento seleccionado se resalta. Esta característica facilita la edición de archivos de origen largo, proporcionan una manera directa y rápida de obtener acceso al código.

    ![Inspeccionar elementos](using-page-inspector-in-visual-studio-2012/_static/image29.png "inspeccionar elementos")

    *Inspeccionar elementos*
7. Haga clic en el **pasar al modo de inspección** botón (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), ubicado en las pestañas del Inspector de página, para deshabilitar el cursor.
8. Seleccione el **HTML** ficha para mostrar el código HTML que se representa en el Explorador de Inspector de página.
9. En el código HTML, busque el elemento de lista con el vínculo Koala y selecciónelo.

    Tenga en cuenta que, cuando se selecciona el código, el resultado correspondiente es explorador automáticamente resaltado. Esta característica es útil para ver cómo se representa un bloque HTML en la página.

    ![Seleccionar un elemento HTML en la página](using-page-inspector-in-visual-studio-2012/_static/image31.png "seleccionar un elemento HTML en la página")

    *Seleccionar un elemento HTML en la página*
10. Haga clic en el **pasar al modo de inspección** botón Habilitar *modo de inspección* y haga clic en la barra de navegación. A la derecha del código HTML, en el panel Estilos, verá una lista con los estilos CSS que se aplican al elemento seleccionado.

    > [!NOTE]
    > como el encabezado es una parte del diseño del sitio, Inspector de página también se abra el archivo Site.Master y resalte el segmento de código que se ve afectado.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "detección de estilos y los archivos de código fuente de un elemento seleccionado")

    *Detección de estilos y los archivos de código fuente de un elemento seleccionado*
11. Con el puntero de inspección de alternancia habilitado, mueva el puntero del mouse por debajo de la barra de menús y haga clic en el círculo de medio en blanco.

    ![Selección de un elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "selección de un elemento")

    *Selección de un elemento*
12. En el panel Estilos, busque el **imagen de fondo** de elemento en el **.main-content** grupo. **Desactive la opción** el **imagen de fondo** y vea Qué sucede. Observará que el explorador reflejará los cambios inmediatamente y se oculta el círculo.

    > [!NOTE]
    > Los cambios que se apliquen en la pestaña estilos de Inspector de página no afectan a la hoja de estilos original. Puede desactivar estilos o cambiar sus valores tantas veces como desee, pero se restaurarán después de actualizar la página.

    ![Habilitación y deshabilitación de CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "habilitar y deshabilitar estilos CSS")

    *Habilitar y deshabilitar estilos CSS*
13. Ahora, haga clic en el '**su** **logotipo aquí '** texto en el encabezado mediante el modo de inspección.
14. En el **estilos** ficha, busque el **tamaño de fuente** atributo CSS en el **.site título** grupo. Haga doble clic en el atributo una vez para editar su valor. Valor de reemplazo del 2.3em con **3em**, y, a continuación, presione ENTRAR. Tenga en cuenta que el título tiene un aspecto más grande.

    ![Cambiar valores CSS en el Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "valores de cambio de CSS en el Inspector de página")

    *Cambiar valores CSS en el Inspector de página*
15. Haga clic en el **realizar seguimiento de estilos** ficha, que se encuentra en el panel derecho del Inspector de página. Se trata de una manera alternativa para ver todos los estilos aplicados a la selección, ordenada por nombre de atributo.

    ![Seguimiento de estilos CSS del elemento seleccionado](using-page-inspector-in-visual-studio-2012/_static/image36.png "seguimiento de estilos CSS del elemento seleccionado")

    *Seguimiento de estilos CSS del elemento seleccionado*
16. Otra característica del Inspector de página es el panel de diseño. Con el modo de inspección, seleccione la barra de navegación y, a continuación, haga clic en el **diseño** ficha en el panel derecho. Verá el tamaño exacto del elemento seleccionado, así como su tamaño de desplazamiento, margen, relleno y borde. Tenga en cuenta que también puede modificar los valores de esta vista.

    ![Diseño de un elemento en el Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image37.png "diseño de un elemento en el Inspector de página")

    *Diseño de un elemento en el Inspector de página*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarea 2: buscar y corregir problemas de estilo en la Galería fotográfica

¿Forma de diagnosticar problemas de páginas Web con versiones anteriores de Visual Studio? Probablemente esté familiarizado con las herramientas de depuración que se ejecutan fuera del IDE de Visual Studio, como herramientas de desarrollo de Internet Explorer o Firebug de web. Exploradores solo comprenden HTML, secuencias de comandos y estilos, mientras que un marco de trabajo subyacente genera el código HTML que se representará. Por esta razón, a menudo necesitará implementar todo el sitio para ver el aspecto de las páginas web como.

Probablemente tenía seguido estos pasos si desea detectar y corregir un problema en el sitio web:

1. Ejecutar la solución desde Visual Studio, o implementar la página en el servidor web.
2. En el explorador, abra las herramientas de desarrollo que se utiliza, o simplemente abra el código fuente y los estilos e intente coincide con el problema. Para buscar los archivos implicados, que se ha utilizado el &quot;búsqueda&quot; o &quot;buscar en archivos&quot; características con el nombre de las clases de estilo.
3. Una vez detectado el error, detenga el explorador Web y el servidor.
4. Borre la memoria caché de exploración.
5. Vuelva a Visual Studio para aplicar una corrección. Repita todos los pasos para probar.

Como no existe ya no real WYSIWYG en ASP.NET WebForms, algunos problemas de estilo se detectan en una fase posterior, después de ejecutar o implementar. Ahora, con el Inspector de página, es posible obtener una vista previa de cualquier página sin ejecutar la solución.

En esta tarea, utilizará el inspector de página para solucionar algunos problemas de la aplicación de la Galería fotográfica. En los pasos siguientes, va a detectar y corregir rápidamente algún problema de estilo simple en el encabezado.

1. Mediante la inspección de página, busque el **registrar** y **inicio de sesión** vínculos en la parte izquierda del encabezado.

    Tenga en cuenta que el vínculo no se muestra en el lugar esperado a la derecha. Ahora podrá alinear el vínculo a la derecha y cambiar el estilo en consecuencia.

    ![Inicie sesión en el vínculo situado en la parte izquierda](using-page-inspector-in-visual-studio-2012/_static/image38.png "inicie sesión en el vínculo situado a la izquierda")

    *Registro en el vínculo situado a la izquierda*
2. Con el modo de inspección de alternancia seleccionado, seleccione el vínculo de inicio de sesión para abrir su código.

    Tenga en cuenta que el código de origen del vínculo se encuentra en la **Site.Master** archivos, no en la página Default.aspx que es el lugar donde puede buscar en primer lugar; se han colocado directamente en el archivo de código fuente correcto.
3. En el **estilos** ficha, busque y haga clic en el  **&lt;sección&gt; #login** elemento, que es el contenedor HTML para que estos vínculos.

    Tenga en cuenta que la **#login** estilo automáticamente se encuentra en **Site.css** tras hacer clic en. Además, ahora se resalta el código.

    ![Seleccionar los estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "al seleccionar los estilos CSS")

    *Seleccionar los estilos CSS*
4. Quite el **alinear texto** mediante la eliminación de los caracteres de cierre y apertura de atributo en el código resaltado y guarde el **Site.css** archivo.

    Inspector de página es consciente de todos los archivos que componen la página actual y puede detectar cuándo cambia cualquiera de estos archivos. Le avisa cuando la página actual en el explorador no está sincronizada con los archivos de origen.
5. En el Explorador de Inspector de página, haga clic en la barra situada debajo de la barra de direcciones para guardar los cambios y volver a cargar la página.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Volver a cargar la página*

    Los vínculos son ahora a la derecha, pero todavía quedarán una lista con viñetas. Ahora, debe quitar las viñetas y Alinear horizontalmente los vínculos.

    ![Página actualizada](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Página actualizada*
6. Con el modo de inspección, seleccione cualquiera de los  **&lt;li&gt;**  elementos que contienen el &quot;registrar&quot; y &quot;iniciar sesión&quot; vínculos. A continuación, haga clic en el  **&lt;sección&gt; #login** de elemento para tener acceso a **Styles.css** código.

    ![Buscar el estilo](using-page-inspector-in-visual-studio-2012/_static/image42.png "buscar el estilo")

    *Buscar el estilo*
7. En **Style.css**, quite el código de **#login li** elementos. El estilo que se va a agregar se oculte la viñeta y mostrar los elementos horizontalmente.

    ![Cambiar el estilo de los vínculos de inicio de sesión](using-page-inspector-in-visual-studio-2012/_static/image43.png "cambiar el estilo de los vínculos de inicio de sesión")

    *Cambiar el estilo de los vínculos de inicio de sesión*
8. Guardar **Style.css** de archivo y haga clic una vez en la barra situada debajo de la dirección a cargar la página. Observe que los vínculos aparecen correctamente.

    ![Vínculos alinean a la derecha](using-page-inspector-in-visual-studio-2012/_static/image44.png "vínculos alinean a la derecha")

    *Vínculos alineado a la derecha*
9. Por último, cambiará el título de encabezado. En lugar de buscar el '**su logotipo aquí '** texto en todos los archivos, use el modo de inspección para haga clic en el texto y obtener el código de origen que lo genera.

    ![Buscar el título del sitio](using-page-inspector-in-visual-studio-2012/_static/image45.png "buscar el título del sitio")

    *Buscar el título del sitio*
10. Ahora está en **Site.Master**, reemplace el '**su logotipo aquí**'text con'**Galería fotográfica**'. Guarde y actualice el Explorador de Inspector de página.

    ![Página de la Galería fotográfica actualizado](using-page-inspector-in-visual-studio-2012/_static/image46.png "página de la Galería fotográfica actualizado")

    *Página de la Galería fotográfica actualizado*
11. Por último, presione **F5** para ejecutar la aplicación en la comprobación de todos los cambios funcionan según lo esperado.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Tras completar este laboratorio práctico, que aprendió cómo usar Inspector de página para obtener una vista previa de la aplicación Web sin tener que volver a generar y ejecutar el sitio Web en un explorador. Además, aprendió cómo buscar y corregir errores accediendo directamente desde la salida representada en el código fuente rápidamente.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Express de VS para icono Web*
