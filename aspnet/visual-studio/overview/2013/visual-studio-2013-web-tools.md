---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "Laboratorio de prácticas: Visual Studio 2013 Web Tools | Documentos de Microsoft"
author: rick-anderson
description: "Visual Studio es un entorno de desarrollo excelente. Ventanas basadas en NET y proyectos web. Incluye un editor de texto eficaz que puede utilizarse fácilmente para..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratorio de prácticas: Visual Studio 2013 Web Tools
====================
por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](http://aka.ms/webcamps-training-kit)

> Visual Studio es un entorno de desarrollo excelente. Ventanas basadas en NET y proyectos web. Incluye un editor de texto eficaz que puede utilizar fácilmente para editar archivos independientes sin un proyecto.
> 
> Visual Studio mantiene un árbol de análisis completos cuando se edita cada archivo. Esto permite a Visual Studio proporcionar características de Autocompletar sin parangón y acciones basadas en el documento al realizar la experiencia de desarrollo más agradable y mucho más rápida. Estas características son especialmente eficaces en documentos HTML y CSS.
> 
> Toda esta potencia también está disponible para las extensiones, de forma que sea fácil ampliar los editores con características nuevas y eficaces para satisfacer sus necesidades. Web Essentials es una colección de mejoras (principalmente) basadas en la web de Visual Studio. Incluye una gran cantidad de nuevos finalizaciones de IntelliSense (especialmente para CSS), nuevas características de vínculo de explorador, automático de archivos JSHint para JavaScript, nuevas advertencias para HTML, CSS y muchas otras características que son esenciales para el desarrollo web moderna.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar las nuevas características del editor HTML Web Essentials incluidas como fragmentos de código de HTML5 enriquecidos y codificación Zen
- Usar las nuevas características del editor de CSS Web Essentials incluidas como el selector de colores y la información sobre herramientas de explorador matriz
- Usar las nuevas características de editor de JavaScript incluidas en la Web Essentials, como extraer a archivo e IntelliSense de todos los elementos HTML
- Intercambiar datos entre el explorador y Visual Studio mediante el vínculo de explorador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio de prácticas, es necesario lo siguiente:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o superior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para ejecutar los ejercicios en este laboratorio práctico, debe configurar primero el entorno.

1. Abra una ventana del explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que configure el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo de Control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha activado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso de los fragmentos de código

En este documento de laboratorio, le indicará que insertar bloques de código. Para su comodidad, la mayor parte de este código se proporciona como fragmentos de código de Visual Studio, puede tener acceso desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.

> [!NOTE]
> Cada ejercicio viene acompañado por una solución de inicia ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de las otras. Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio se encuentran desde estas a partir de soluciones y no pueden funcionar hasta que haya completado el ejercicio. En el código fuente de un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos en el ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional cuando se trabaja a través de este laboratorio práctico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Trabajar con elementos esenciales de Web y de vínculo de explorador](#Exercise1)
2. [Sacar partido de IntelliSense y de fragmentos de código](#Exercise2)

> [!NOTE]
> Cuando se inicia Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos descritos en este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ejercicio 1: Trabajar con elementos esenciales de Web y de vínculo de explorador

**Web Essentials** es una extensión de Visual Studio que agrega una variedad de características útiles para el desarrollo web moderna, se centra principalmente en hacer que la experiencia de desarrollo web mucho más rápida y más agradable. Puede instalar Essentials Web desde la Galería de extensiones de Visual Studio.

**Vínculo de explorador** es una característica nueva incluida en Visual Studio 2013 que proporciona un canal entre el IDE de Visual Studio y cualquier explorador abierto para intercambiar datos entre la aplicación web y Visual Studio. Web Essentials amplía vínculo de explorador con herramientas para manipular el modelo de objetos DOM y los estilos CSS de las páginas web directamente desde el explorador.

En este ejercicio, explorará algunas de las características admitidas por **Web Essentials** y **vínculo de explorador** para mejorar una página de prueba simple.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarea 1: ejecutar el proyecto en varios exploradores

En esta tarea, configurará la aplicación web para que se ejecute en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.

1. Abra **Microsoft Visual Studio**.
2. En el **archivo** menú, seleccione **abrir | Proyecto o solución...**  y vaya a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** en el **origen** carpeta del laboratorio (C:\WebCampsTK\HOL\VSWebTooling\Source). Seleccione **Begin.sln** y haga clic en **abiertos**.
3. En la barra de herramientas de Visual Studio, expanda el menú del explorador y seleccione **examinar con...** .

    ![Opción del menú Examinar con](visual-studio-2013-web-tools/_static/image1.png "examinar con... en el menú del explorador")

    *Explorar con opción de menú*
4. En el **explorar con** cuadro de diálogo, seleccione **Google Chrome** y **Internet Explorer** , mantenga presionada la **CTRL** clave y haga clic en  **Establecer como predeterminado**.

    ![Examinar con el cuadro de diálogo](visual-studio-2013-web-tools/_static/image2.png "examinar con el cuadro de diálogo")

    *Al seleccionar varios exploradores predeterminados*
5. Internet Explorer y Google Chrome deberían aparecer ahora como los exploradores de manera predeterminada. Haga clic en **cancelar** para cerrar el cuadro de diálogo.

    ![Google Chrome e Internet Explorer como exploradores predeterminados](visual-studio-2013-web-tools/_static/image3.png "exploradores predeterminados de Internet Explorer y Google Chrome")

    *Google Chrome e Internet Explorer como exploradores predeterminados*

    > [!NOTE]
    > Después de configurar los exploradores de manera predeterminada, el **varios exploradores** opción está seleccionada en el menú del explorador.
    > 
    > ![Varios exploradores](visual-studio-2013-web-tools/_static/image4.png "varios exploradores")
6. Presione **CTRL** + **F5** para ejecutar la aplicación sin depuración.
7. Cuando abren dos ventanas del explorador, coloque uno de ellos por encima de la otra con el fin de ver las actualizaciones en los exploradores al mismo tiempo. Los exploradores deben mostrar una página web con un rectángulo de color azul claro.

    ![Colocación de un explorador encima de otro](visual-studio-2013-web-tools/_static/image5.png "colocar uno explorador encima de otro")

    *Colocación de un explorador encima de otro*
8. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarea 2: usar Zen de codificación para crear elementos HTML

**Codificación Zen** es un editor de código de complemento de alta velocidad HTML, XML, XSL (o cualquier otro formato de código estructurado) y de edición. El núcleo de este complemento es un motor de abreviatura eficaz que permite expandir expresiones - similares a los selectores CSS - en código HTML. Codificación Zen es una forma rápida de escribir la sintaxis del selector de estilo HTML mediante CSS.

En este ejercicio, utilizará la característica de codificación Zen proporcionada por Web Essentials para generar los botones HTML que representan las opciones de la pregunta.

1. Cambie a Visual Studio.
2. Abra la **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.
3. Reemplace el  **&lt;!--TODO: agregar aquí--opciones&gt;**  comentario con el código siguiente y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. El código debe ampliarse a HTML.

    ![Expandir HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")

    *HTML expandido*

    > [!NOTE]
    > Para obtener más información sobre sintaxis Zen de codificación, vea la siguiente [artículo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Haga clic en el **Actualizar exploradores vinculados** botón para actualizar dos exploradores.

    ![Actualizar exploradores vinculados](visual-studio-2013-web-tools/_static/image7.png "Actualizar exploradores vinculados")

    *Actualizar exploradores vinculados*

    ![Internet Explorer - página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página se actualiza con cuatro botones")

    *Internet Explorer - página se actualiza con cuatro botones*

    ![Google Chrome - página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página se actualiza con cuatro botones")

    *Google Chrome - página se actualiza con cuatro botones*
6. Cambie a Visual Studio.
7. Ha agregado los botones a la página, pero necesite agregar una pregunta de la plantilla. Para ello, usará una nueva característica en Essentials Web denominado **Lorem Ipsum generador**. Busque la **div** elemento con la **clase** atributo **frontal**.
8. Agregue el siguiente código como el primer elemento secundario de la **div**y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. El código debe ampliarse a HTML.

    ![Genera automáticamente Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerado")

    *Lorem Ipsum autogenerado*

    > [!NOTE]
    > Como parte de codificación Zen, ahora pueden generar código Lorem Ipsum directamente en el editor de HTML. Basta con que escriba **lorem** y posicionamiento **ficha** y un 30 Lorem Ipsum se insertará texto de word. P. ej. *lorem10* inserta 10 palabras Lorem Ipsum.
10. Agregará un logotipo en la parte superior de la pregunta utilizando otra característica nueva en Essentials Web denominado **generador Lorem píxeles**. Agregue el siguiente código como el primer elemento secundario de la **div** elemento con **contenedor** como **clase** valor y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. El código debe expandir en HTML.

    ![lorem Píxel autogenerado](visual-studio-2013-web-tools/_static/image11.png "autogenerado Lorem píxeles")

    *Genera automáticamente lorem píxeles*

    > [!NOTE]
    > Como parte de codificación Zen, también puede generar código Lorem píxeles directamente en el editor de HTML. Basta con que escriba **foto-200 x 200-animales** y posicionamiento **ficha** y **img** etiqueta con una imagen de 200 x 200 de un animal que se van a insertar. Para obtener más información, consulte [Lorem píxeles](http://www.lorempixel.com).
12. Haga clic en el **Actualizar exploradores vinculados** botón para actualizar dos exploradores.

    ![Internet Explorer - autogenerado imagen y texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - autogenerado imagen y texto")

    *Internet Explorer - autogenerado imagen y texto*

    ![Google Chrome - autogenerado imagen y texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - autogenerado imagen y texto")

    *Google Chrome - autogenerado imagen y texto*

    > [!NOTE]
    > Dado que la imagen se selecciona aleatoriamente al agregar el fragmento de código, puede diferir la imagen que se muestra en los exploradores.
13. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarea 3: actualizar una propiedad de estilo

En esta tarea, va a usar el vínculo de explorador **inspeccionar modo** característica para detectar la ubicación exacta donde se genera el elemento DOM específico y, a continuación, actualice la propiedad de color de ese elemento mediante un selector de colores proporcionado por Web Essentials.

1. En el Explorador de Internet Explorer, presione **CTRL** + **ALT** + **I** para habilitar el modo de inspeccionar.
2. Mueva el puntero sobre el borde de color azul claro y haga clic en.

    ![Mueva el puntero sobre el borde de color azul claro](visual-studio-2013-web-tools/_static/image14.png "mover el puntero sobre el borde de color azul claro")

    *Mueva el puntero sobre el borde de color azul claro*
3. Cambie a Visual Studio. Observe cómo el elemento HTML que seleccionó en el explorador también está seleccionado en el editor HTML de Visual Studio.

    ![Elemento HTML seleccionado en el editor HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML seleccionado en el editor HTML de Visual Studio")

    *Elemento HTML seleccionado en el editor HTML de Visual Studio*
4. A continuación, actualizaremos la **frontal** clase CSS para cambiar el estilo del elemento seleccionado. Para ello, presione **CTRL** + **,** para abrir el **navegar a** cuadro de búsqueda. Tipo de **site.css** y presione **ENTRAR** para abrir el archivo.

    ![Abrir archivo Site.css](visual-studio-2013-web-tools/_static/image16.png "al abrir el archivo Site.css")

    *Abrir el archivo Site.css*
5. Presione **CTRL** + **F** y tipo **.front .flip-Container** para encontrar el selector de CSS.
6. Haga clic en el cuadrado azul claro en la propiedad de borde de la clase para abrir el selector de colores.

    ![Abrir el selector de colores](visual-studio-2013-web-tools/_static/image17.png "abrir el selector de colores")

    *Abrir el selector de colores*
7. Expanda el selector de colores, haga clic en el botón de contenido y seleccione un color nuevo.

    ![Al expandir el selector de colores](visual-studio-2013-web-tools/_static/image18.png "expandiendo el selector de colores")

    *Al expandir el selector de colores*
8. Presione **CTRL** + **ALT** + **ENTRAR** para actualizar los exploradores vinculados.
9. Cambie a Internet Explorer y observe cómo ha cambiado el color del borde.

    ![Internet Explorer - actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - actualizarlo el color del borde")

    *Internet Explorer - actualizarlo el color del borde*
10. Cambie a Google Chrome y observe cómo ha cambiado el color del borde.

    ![Google Chrome - actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - actualizarlo el color del borde")

    *Google Chrome - actualizarlo el color del borde*
11. Cambie a Visual Studio.
12. Vaya al final de la **Site.css** archivo y presione **CTRL** + **F** para buscar la **.btn** selector.
13. Tenga en cuenta que la **- webkit-border-radius** propiedad subrayada en verde.

    ![propiedad - webkit-border-radius del botón selector de](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius propiedad del selector de botón")

    *propiedad - webkit-border-radius del selector de botón*
14. Colocar el símbolo de intercalación en el **- webkit-border-radius** propiedad. Debe aparecer una línea azul debajo de la primera letra de la primera palabra de la propiedad. Se trata de la **etiqueta inteligente**.
15. Presione **CTRL** + **.** Para abrir el menú de sugerencias y haga clic en **agregar falta la propiedad estándar (border-radius)**.

    ![Agregar falta sugerencia de propiedad estándar](visual-studio-2013-web-tools/_static/image22.png "agregar falta sugerencias de propiedades estándar")

    *Agregar falta sugerencias de propiedades estándar*
16. El **border-radius** propiedad se agrega automáticamente a la regla CSS.

    ![Falta la propiedad estándar agregado](visual-studio-2013-web-tools/_static/image23.png "falta la propiedad estándar agregado")

    *Falta la propiedad estándar agregado*
17. Mueva el puntero sobre la **border-radius** propiedad para mostrar el **información sobre herramientas de explorador matriz**. El **información sobre herramientas de explorador matriz** muestra la disponibilidad de la propiedad en cada explorador.

    ![Información sobre herramientas de explorador matriz](visual-studio-2013-web-tools/_static/image24.png "información sobre herramientas de explorador matriz")

    *Información sobre herramientas de explorador matriz*
18. Tenga en cuenta que el valor de la **border-radius** propiedad aparece subrayada todavía. Mueva el puntero sobre el valor para ver el mensaje de advertencia.

    ![Advertencia de valor de propiedad Border-radius](visual-studio-2013-web-tools/_static/image25.png "advertencia de valor de propiedad Border-radius")

    *Advertencia de valor de propiedad de radio del borde*
19. Quite la unidad de la **border-radius** valor de la propiedad tal como se sugiere la información sobre herramientas.
20. Como **border-radius** es la propiedad estándar para definir cómo redondeado borde esquinas son, puede quitar el **- webkit-border-radius** propiedad y valor de la regla CSS.
21. Colocar el símbolo de intercalación en el **ajuste** propiedad y observe que la etiqueta inteligente también aparece a continuación.
22. Abra el menú y haga clic en **añadir falta información específica del proveedor**.

    ![Agregar sugerencias de datos específicos de proveedor falta](visual-studio-2013-web-tools/_static/image26.png "agregar sugerencias de datos específicos de proveedor que faltan")

    *Agregar sugerencias de datos específicos de proveedor que faltan*
23. El **-ms-word-wrap** propiedad se agrega automáticamente a la regla CSS.

    ![Propiedad específica de proveedor agregado](visual-studio-2013-web-tools/_static/image27.png "propiedad específica de proveedor agregado")

    *Propiedad específica de proveedor agregado*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarea 4: actualizar el código HTML desde el explorador

En esta tarea, va a usar el vínculo de explorador **el modo de diseño** característica para editar el objeto DOM desde el explorador y transferir los cambios en el archivo de código fuente HTML en Visual Studio.

1. En Google Chrome, presione **CTRL** + **ALT** + **d.** para habilitar el modo de diseño.
2. Mueva el puntero sobre la **Lorem Ipsum dolor sit AME** etiquetar y haga clic en.

    ![Editar la pregunta](visual-studio-2013-web-tools/_static/image28.png "editar la pregunta")

    *Edición de la pregunta*
3. Debería aparecer un cursor. Reemplace el texto original con *lo que parece al igual que cuando escribe una pregunta más?*y, a continuación, presione **ESC** para salir del modo de diseño.

    ![Pregunta editado](visual-studio-2013-web-tools/_static/image29.png "pregunta editado")

    *Pregunta editado*
4. Vuelva a Visual Studio y abra switch **Index.cshtml**, si aún no está abierto. Tenga en cuenta que el texto interno de la  **&lt;p&gt;**  se ha actualizado el elemento.

    ![Pregunta de actualizados en la página HTML](visual-studio-2013-web-tools/_static/image30.png "pregunta actualizados en la página HTML")

    *Pregunta actualizada en la página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Advertencias relacionadas con la tarea 5: revisión SEO

**Optimización de motor de búsqueda** (SEO) es el proceso de realizar un rango de sitio Web superior en la lista de un motor de búsqueda de resultados. Cuanto mayor sea el sitio clasifica y más coherente aparece, los visitantes más obtendrán el sitio de ese motor de búsqueda. Web Essentials incorpora una herramienta de análisis que examina HTML, los problemas de informes encontró y proporciona ayuda para corregirlas.

1. Vaya a la **vista** menú y haga clic en **lista de errores** para abrir el **lista de errores** ventana.

    ![Lista de errores en la vista menú](visual-studio-2013-web-tools/_static/image31.png "lista de errores en el menú Ver")

    *Lista de errores en la vista menú*
2. Observe que hay una advertencia de SEO le notifica que un  **&lt;meta&gt;**  etiqueta para la descripción de la página falta. Haga doble clic en la entrada de advertencia de SEO para corregirlo.

    ![Ventana Lista de errores](visual-studio-2013-web-tools/_static/image32.png "ventana Lista de errores")

    *Ventana Lista de errores*
3. En el **Web Essentials** cuadro de diálogo, haga clic en **Sí** para insertar una descripción &lt;meta&gt; etiqueta.

    ![Cuadro de diálogo de Web Essentials](visual-studio-2013-web-tools/_static/image33.png "cuadro de diálogo Web Essentials")

    *Cuadro de diálogo de Web Essentials*
4. El editor para  **\_Layout.cshtml** abre y  **&lt;meta&gt;**  etiqueta se agrega automáticamente a la **head** sección de la Archivo HTML.

    ![Etiqueta META que agrega automáticamente en la página de _Layout](visual-studio-2013-web-tools/_static/image34.png "Meta etiqueta agregado automáticamente en la página de _Layout")

    *Etiqueta META que agrega automáticamente a \_página de diseño*
5. Cambie el valor de la **contenido** atribuir a *GeekQuiz* y guarde el archivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ejercicio 2: Sacar partido de fragmentos de código y de IntelliSense

Con Essentials de Web, el editor HTML se ha ampliado con una funcionalidad adicional. En este ejercicio, verá algunas características nuevas que resultan útiles al desarrollar aplicaciones web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarea 1: utilizar IntelliSense en documentos HTML

La primera función nueva que aparece en esta tarea se llama **IntelliSense dinámico**. IntelliSense dinámica lee otras etiquetas y atributos para deducir los posibles identificadores que va a usar.

En esta tarea, creará un nuevo elemento de formulario HTML que contiene una etiqueta y un campo de entrada. A continuación, agregará un **para** de atributo a la etiqueta y enlazarla a la entrada, y verá sugerencias de IntelliSense en función de los identificadores de las entradas en el ámbito.

1. Abra **Visual Studio Express 2013 para Web** y **Begin.sln** soluciones se encuentran en la **origen/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **el Explorador de soluciones**, abra el **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.
3. Agregue el siguiente formulario dentro de la  **&lt;sección&gt;**  elemento.

    (Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *formulario*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. La etiqueta de entrada debe estar precedida de una etiqueta con una descripción del campo. Agregue la etiqueta siguiente antes de la etiqueta de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. El **para** atributo de un  **&lt;etiqueta&gt;**  especifica qué elemento form con una etiqueta que está enlazado. El valor del atributo debe ser igual que el identificador del elemento relacionado. Agregar el **para** atribuir a la  **&lt;etiqueta&gt;**  elemento. Como se muestra en la ilustración siguiente, la &quot;nombre&quot; valor aparece en el cuadro de IntelliSense, basándose en el identificador de los elementos dentro del mismo ámbito (incluye  **&lt;formulario&gt;**).

    ![Que muestra el Id. de IntelliSense](visual-studio-2013-web-tools/_static/image35.png "que muestra el Id. de IntelliSense")

    *Que muestra el Id. de IntelliSense*
6. Eliminar agregadas recientemente  **&lt;formulario&gt;**  elemento y su contenido.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarea 2: uso de fragmentos de código de HTML

HTML5 introdujo más de 25 etiquetas semánticas de nuevo. Visual Studio ya tenía compatibilidad con IntelliSense para estas etiquetas, pero Visual Studio 2013 hace que sea más rápida y más fácil de escribir marcado mediante la adición de nuevos fragmentos de código. Aunque estas etiquetas no son complejas, vienen con unos matices pequeños, como la adición de las reservas de códec correcto para el *audio* etiqueta. En esta tarea, verá los fragmentos de código HTML para la etiqueta de audio.

1. En el **Index.cshtml** de archivo, escriba  **&lt;aud** dentro de la  **&lt;sección&gt;**  elemento tal como se muestra en la ilustración siguiente.

    ![Inserción de un elemento audio](visual-studio-2013-web-tools/_static/image36.png "insertar un elemento de audio")

    *Insertar un elemento de audio*
2. Presione **ficha** dos veces y observe cómo se agrega el código siguiente en la página y el cursor se coloca en el **src** atributo del primer origen.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Presionando el **ficha** clave dos veces, el fragmento de código se inserta. El fragmento de audio muestra el uso estándar de la *audio* etiqueta, con dos archivos de origen para la compatibilidad mejorada.
3. Elimine la segunda línea y actualizar el origen de la primera línea con el siguiente vínculo para la presentación de WebCampsTV Katana: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). El código resultante se muestra a continuación.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > El archivo de origen *KatanaProject.mp3* se utiliza como ejemplo. Puede usar otro origen, si lo prefiere.
4. Presione **CTRL** + **S** para guardar el archivo.
5. Presione **CTRL** + **F5** para iniciar la aplicación.
6. Tenga en cuenta que un Reproductor de audio se agregó a la aplicación.

    ![Reproductor de audio en Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Reproductor de Audio en Internet Explorer")

    *Reproductor de audio en Internet Explorer*

    ![Reproductor de audio en Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Reproductor de Audio en Google Chrome")

    *Reproductor de audio en Google Chrome*
7. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tarea 3: utilizar IntelliSense en documentos de JavaScript

Con Web Essentials 2013, hojas de estilos y páginas HTML generan una lista de identificadores y nombres de clase. En esta tarea, obtendrá información sobre cómo mejoran la compatibilidad con IntelliSense para JavaScript en Web Essentials 2013 esas listas.

1. En el **Index.cshtml** de archivos, agregue el código siguiente para definir un **script** etiqueta para el código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Agregue el código siguiente dentro de la **script** etiqueta para definir la función de devolución de llamada listo.

    (Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Colocar el símbolo de intercalación en el **script** etiqueta y presione **CTRL** + **.** Para abrir el menú de sugerencias.
4. Haga clic en **extraer al archivo**.

    ![Extraer JavaScript para sugerencias de archivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extraer a sugerencias de archivo")

    *Extraer JavaScript para sugerencias de archivo*
5. En el **Guardar como** ventana, seleccione la **Scripts** carpeta, asigne nombre al archivo **init.js** y haga clic en **guardar**.

    ![Guardar como ventana](visual-studio-2013-web-tools/_static/image40.png "ventana Guardar como")

    *Guardar como ventana*

    > [!NOTE]
    > El **init.js** archivo se crea y se mueve el contenido de la secuencia de comandos al archivo.
    > 
    > ![Archivo de init.js creado con el contenido incluido](visual-studio-2013-web-tools/_static/image41.png "archivo Init.js creado con el contenido incluido")
    > 
    > *Archivo de init.js creado con el contenido incluido*
6. Abra la **Index.cshtml** de archivo y compruebe que la etiqueta de script se ha reemplazado por una referencia a la **init.js** archivo.

    ![Referencia de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referencia de html")

    *Referencia de html init.js*
7. Vaya a la **el Explorador de soluciones** y tenga en cuenta que la **init.js** archivo se incluye automáticamente en la solución.

    ![Archivo de init.js incluido en la solución](visual-studio-2013-web-tools/_static/image43.png "archivo Init.js incluido en la solución")

    *Archivo de init.js incluido en la solución*
8. Vuelva a la **init.js** archivo para actualizar la **listo** función de devolución de llamada.
9. Dentro de la definición de devolución de llamada de función que se pasa a *listo*, agregue el código siguiente para obtener todos los elementos mediante un atributo de clase específica.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Presione **CTRL** + **espacio** entre las comillas dentro de la **getElementsByClassName** llamada de función.

    ![Con IntelliSense para la función getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "IntelliSense que muestra para la función getElementsByClassName")

    *IntelliSense que muestra para la función getElementsByClassName*

    > [!NOTE]
    > Tenga en cuenta que IntelliSense muestra las clases definidas en las hojas de estilos del proyecto.
11. Reemplace la línea que ha creado con el código siguiente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Coloque el cursor después de **au** dentro de las comillas en el **getElementsByTagName** función del sistema y presione **CTRL** + **espacio**.

    ![Con IntelliSense para el método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "que muestra IntelliSense para el método getElementByTagName")

    *Que muestra IntelliSense para el método getElementsByTagName*
13. Seleccione  **&quot;audio&quot;**  en la lista y presione **ENTRAR**. El resultado se muestra en la ilustración siguiente.

    ![Recuperar los elementos de Audio](visual-studio-2013-web-tools/_static/image46.png "recuperar los elementos de Audio")

    *Recuperar los elementos de Audio*
14. En **el Explorador de soluciones**, haga clic en el **init.js** un archivo en el **Scripts** carpeta y seleccione **archivos JavaScript Minificar** desde el **Web Essentials** menú.

    ![Los archivos JavaScript de minificar](visual-studio-2013-web-tools/_static/image47.png "archivos JavaScript Minificar")

    *Minificar los archivos de JavaScript*
15. Cuando se le solicita que habilite la reducción automática cuando cambia el archivo de origen, haga clic en **Sí**.

    ![Habilitar la advertencia de reducción automática](visual-studio-2013-web-tools/_static/image48.png "habilitar la advertencia de reducción automática")

    *Habilitar la advertencia de reducción automática*

    > [!NOTE]
    > El **init.min.js** se crea y se agrega como una dependencia de la **init.js** archivo.
    > 
    > ![Archivo init.min.js creado](visual-studio-2013-web-tools/_static/image49.png "archivo Init.min.js creado")
    > 
    > *Archivo init.min.js creado*
16. Abra la **init.min.js** de archivos y observe que el archivo se reduce.

    ![Contenido del archivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenido del archivo")

    *Contenido del archivo init.min.js*
17. En el **init.js** , agregue el código siguiente la **getElementsByTagName** llamada para reproducir todos los elementos de audio de función.

    (Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Haga clic en **CTRL** + **S** para guardar el archivo. Puesto que ya está abierto el archivo reducido, verá un cuadro de diálogo que dice que el archivo se ha modificado fuera del editor de origen. Haga clic en **Sí**.

    ![Advertencia de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "advertencia de Microsoft Visual Studio")

    *Advertencia de Microsoft Visual Studio*
19. Vuelva a la **init.min.js** archivo para comprobar que el archivo se ha actualizado con el nuevo código.

    ![Actualizado el archivo init.min.js](visual-studio-2013-web-tools/_static/image52.png "archivo Init.min.js actualizado")

    *Archivo init.min.js actualizado*
20. Haga clic en el **actualizar de vínculo de explorador** botón.
21. Una vez que se actualizan dos exploradores los reproductores de audio que vio en la tarea anterior empezará a reproducirse automáticamente.

    ![Reproductor de audio incluido en la vista](visual-studio-2013-web-tools/_static/image53.png "incluido en la vista de Reproductor de Audio")

    *Reproductor de audio incluido en la vista*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Usar las nuevas características del editor HTML Web Essentials incluidas como fragmentos de código de HTML5 enriquecidos y codificación Zen
- Usar las nuevas características del editor de CSS Web Essentials incluidas como el selector de colores y la información sobre herramientas de explorador matriz
- Usar las nuevas características de editor de JavaScript incluidas en la Web Essentials, como extraer a archivo e IntelliSense de todos los elementos HTML
- Intercambiar datos entre el explorador y Visual Studio mediante el vínculo de explorador
