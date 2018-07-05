---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio práctico: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: Visual Studio es un entorno de desarrollo excelente. Windows basada en NET y proyectos web. Incluye un editor de texto eficaz que puede usarse fácilmente para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 8f0f19a184d50cdc6e8e5a2da0e55a0e8ca44dc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399405"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratorio práctico: Visual Studio 2013 Web Tools
====================
por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](http://aka.ms/webcamps-training-kit)

> Visual Studio es un entorno de desarrollo excelente. Windows basada en NET y proyectos web. Incluye un editor de texto eficaz que puede usarse fácilmente para editar archivos independientes sin un proyecto.
> 
> Visual Studio mantiene un árbol de análisis completo, como modificar cada archivo. Esto permite que Visual Studio proporcionar sin precedentes de autocompletado y las acciones basadas en el documento al mismo tiempo que la experiencia de desarrollo más agradable y mucho más rápida. Estas características son especialmente eficaces en documentos HTML y CSS.
> 
> Toda esta potencia también está disponible para las extensiones, de forma que sea fácil de ampliar los editores con características nuevas y eficaces para satisfacer sus necesidades. Web Essentials es una colección (principalmente) mejoras relacionadas con la web para Visual Studio. Incluye una gran cantidad de nuevas finalizaciones de IntelliSense (especialmente para CSS), nuevas características de vínculo de explorador, automatic JSHint para JavaScript, archivos, nuevas advertencias para HTML y CSS y muchas otras características que son esenciales para el desarrollo web moderno.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar las nuevas características del editor HTML incluidas en Web Essentials como fragmentos de código enriquecidos de HTML5 y el Zen de codificación
- Usar las nuevas características del editor CSS incluidas en Web Essentials como el selector de colores y la información sobre herramientas de explorador matriz
- Usar las nuevas características del editor de JavaScript incluidas en Web Essentials, como extraer al archivo e IntelliSense para todos los elementos HTML
- Intercambiar datos entre su explorador y Visual Studio mediante el vínculo de explorador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o superior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para poder ejecutar los ejercicios en este laboratorio práctico, deberá configurar primero el entorno.

1. Abra una ventana del explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que se configure su entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo Control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso de los fragmentos de código

En todo el documento de laboratorio, se le pedirá que inserte los bloques de código. Para su comodidad, la mayoría de este código se proporciona como fragmentos de código de Visual Studio, puede acceder desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.

> [!NOTE]
> Cada ejercicio viene acompañado por una solución inicial ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de los demás. Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio faltan en estos a partir de las soluciones y es posible que no funcione hasta que haya completado el ejercicio. En el código fuente para un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos descritos en el ejercicio correspondiente. Puede usar estas soluciones como instrucciones si necesita más ayuda mientras se trabaja a través de este laboratorio práctico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los ejercicios siguientes:

1. [Trabajar con el vínculo con exploradores y Web Essentials](#Exercise1)
2. [Sacar partido de IntelliSense y fragmentos de código](#Exercise2)

> [!NOTE]
> Primera vez que inicie Visual Studio, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos de este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ejercicio 1: Trabajar con vínculo con exploradores y Web Essentials

**Web Essentials** es una extensión de Visual Studio que se agrega una variedad de características útiles para el desarrollo web moderno, centrado principalmente en hacer que la experiencia de desarrollo web mucho más rápida y más agradable. Web Essentials puede instalar desde la Galería de extensiones de Visual Studio.

**Vínculo de explorador** es una característica nueva incluida en Visual Studio 2013 que proporciona un canal entre el IDE de Visual Studio y en cualquier explorador abierto para intercambiar datos entre su aplicación web y Visual Studio. Web Essentials amplía el vínculo de explorador con herramientas para manipular el modelo de objetos DOM y los estilos CSS de las páginas web directamente desde el explorador.

En este ejercicio, explorará algunas de las características admitidas por **Web Essentials** y **Browser Link** para mejorar una página de prueba simple.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarea 1: ejecutar el proyecto en varios exploradores

En esta tarea, configurará la aplicación web para ejecutar en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.

1. Abra **Microsoft Visual Studio**.
2. En el **archivo** menú, seleccione **abiertos | Proyecto o solución...**  y vaya a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** en el **origen** carpeta del laboratorio (C:\WebCampsTK\HOL\VSWebTooling\Source). Seleccione **Begin.sln** y haga clic en **abierto**.
3. En la barra de herramientas de Visual Studio, expanda el menú del explorador y seleccione **examinar con...** .

    ![Opción del menú Examinar con](visual-studio-2013-web-tools/_static/image1.png "examinar con... en el menú del explorador")

    *Explorar con opción de menú*
4. En el **explorar con** cuadro de diálogo, seleccione **Google Chrome** y **Internet Explorer** manteniendo presionada la **CTRL** clave y haga clic en  **Establecer como predeterminado**.

    ![Examinar con el cuadro de diálogo](visual-studio-2013-web-tools/_static/image2.png "examinar con el cuadro de diálogo")

    *Selección de varios exploradores predeterminados*
5. Google Chrome e Internet Explorer deberían aparecer ahora como los exploradores predeterminados. Haga clic en **cancelar** para cerrar el cuadro de diálogo.

    ![Google Chrome e Internet Explorer como exploradores predeterminados](visual-studio-2013-web-tools/_static/image3.png "exploradores predeterminados de Internet Explorer y Google Chrome")

    *Google Chrome e Internet Explorer como exploradores predeterminados*

    > [!NOTE]
    > Después de configurar los exploradores de forma predeterminada, el **varios exploradores** opción está seleccionada en el menú del explorador.
    > 
    > ![Varios exploradores](visual-studio-2013-web-tools/_static/image4.png "varios exploradores")
6. Presione **CTRL** + **F5** para ejecutar la aplicación sin depuración.
7. Al abrirán dos ventanas del explorador, colocar uno de ellos encima del otro para ver las actualizaciones en los dos exploradores simultáneamente. Los exploradores deben mostrar una página web con un rectángulo azul claro.

    ![Colocación de un explorador encima del otro](visual-studio-2013-web-tools/_static/image5.png "colocar encima de los otros exploradores")

    *Colocación de un explorador encima del otro*
8. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarea 2: usar Zen de codificación para crear elementos HTML

**Codificación Zen** es un editor de código de complemento de alta velocidad HTML, XML, XSL (o cualquier otro formato de código estructurado) y editar. El núcleo de este complemento es un motor de abreviatura eficaz que permite expandir expresiones - similares a los selectores CSS - en código HTML. Zen de codificación es una forma rápida para escribir la sintaxis del selector de estilo HTML mediante CSS.

En este ejercicio, usará la característica de codificación Zen proporcionada por Web Essentials para generar los botones HTML que representan las opciones de la pregunta.

1. Cambie a Visual Studio.
2. Abra el **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.
3. Reemplace el **&lt;!--TODO: agregar opciones aquí--&gt;** comentario con el código siguiente y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. El código debe ampliarse a HTML.

    ![Expande HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")

    *HTML expandido*

    > [!NOTE]
    > Para obtener más información sobre sintaxis Zen de codificación, vea el siguiente [artículo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Haga clic en el **Actualizar exploradores** botón para actualizar los dos exploradores.

    ![Actualizar exploradores](visual-studio-2013-web-tools/_static/image7.png "Actualizar exploradores")

    *Actualizar exploradores*

    ![Internet Explorer - página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página se actualiza con cuatro botones")

    *Internet Explorer - página se actualiza con cuatro botones*

    ![Google Chrome: página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image9.png "Google Chrome: página se actualiza con cuatro botones")

    *Google Chrome: página se actualiza con cuatro botones*
6. Cambie a Visual Studio.
7. Los botones se han agregado a la página, pero aun así deberá agregar una pregunta de la plantilla. Para ello, usará una nueva característica en Web Essentials llamado **Lorem Ipsum generador**. Busque el **div** elemento con el **clase** atributo **front**.
8. Agregue el siguiente código como el primer elemento secundario de la **div**y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. El código debe ampliarse a HTML.

    ![Lorem Ipsum autogenerados](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerados")

    *Lorem Ipsum autogenerados*

    > [!NOTE]
    > Como parte de la programación Zen, ahora puede generar código de Lorem Ipsum directamente en el editor de HTML. Simplemente escriba **lorem** y posicionamiento **ficha** y un 30 Lorem Ipsum se insertará el texto de word. P. ej., *lorem10* inserta 10 palabras Lorem Ipsum.
10. Se agregará un logotipo en la parte superior de la pregunta mediante el uso de otra característica nueva en Web Essentials llamado **generador Lorem píxel**. Agregue el siguiente código como el primer elemento secundario de la **div** elemento con **contenedor** como **clase** valor y presione **ficha**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. El código debe expandirse en HTML.

    ![Lorem píxel autogenerados](visual-studio-2013-web-tools/_static/image11.png "Lorem píxel autogenerados")

    *Lorem píxel autogenerados*

    > [!NOTE]
    > Como parte de la programación Zen, también puede generar código de Lorem píxel directamente en el editor de HTML. Simplemente escriba **animales de 200 x 200 pix** y posicionamiento **ficha** y un **img** etiqueta con una imagen de 200 x 200 de un animal que se van a insertar. Para obtener más información, consulte [Lorem píxel](http://www.lorempixel.com).
12. Haga clic en el **Actualizar exploradores** botón para actualizar los dos exploradores.

    ![Internet Explorer - autogenerados imagen y texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - autogenerados imagen y texto")

    *Internet Explorer - autogenerados imagen y texto*

    ![Google Chrome: autogenerados imagen y texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome: texto e imágenes generadas automáticamente")

    *Google Chrome: texto e imágenes generadas automáticamente*

    > [!NOTE]
    > Dado que la imagen se selecciona aleatoriamente al agregar el fragmento de código, la imagen que se muestra en los exploradores puede diferir.
13. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarea 3: actualizar una propiedad de estilo

En esta tarea, utilizará el vínculo de explorador **inspeccionar modo** característica para detectar la ubicación exacta donde se genera el elemento DOM específico y, a continuación, actualice la propiedad de color de ese elemento mediante un selector de color proporcionado por Web Essentials.

1. En el Explorador de Internet Explorer, presione **CTRL** + **ALT** + **me** para habilitar el modo de inspeccionar.
2. Mueva el puntero sobre el borde azul claro y haga clic en.

    ![Mover el puntero sobre el borde azul claro](visual-studio-2013-web-tools/_static/image14.png "mover el puntero sobre el borde azul claro")

    *Mover el puntero sobre el borde azul claro*
3. Cambie a Visual Studio. Observe cómo el elemento HTML que seleccionó en el explorador también se selecciona en el editor HTML de Visual Studio.

    ![Elemento HTML seleccionado en el editor HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML seleccionado en el editor HTML de Visual Studio")

    *Elemento HTML seleccionado en el editor HTML de Visual Studio*
4. A continuación, actualizaremos la **front** clase CSS con el fin de cambiar el estilo del elemento seleccionado. Para ello, presione **CTRL** + **,** para abrir el **navegar a** cuadro de búsqueda. Tipo **site.css** y presione **ENTRAR** para abrir el archivo.

    ![Abrir el archivo Site.css](visual-studio-2013-web-tools/_static/image16.png "al abrir el archivo Site.css")

    *Al abrir el archivo Site.css*
5. Presione **CTRL** + **F** y tipo **.front .flip containter** para encontrar el selector de CSS.
6. Haga clic en el cuadrado azul claro en la propiedad de borde de la clase para abrir el selector de colores.

    ![Abrir el selector de colores](visual-studio-2013-web-tools/_static/image17.png "abrir el selector de colores")

    *Abrir el selector de colores*
7. Expanda el selector de colores, haga clic en el botón de contenido y seleccione un nuevo color.

    ![Expandir el selector de colores](visual-studio-2013-web-tools/_static/image18.png "expandiendo el selector de colores")

    *Expandir el selector de colores*
8. Presione **CTRL** + **ALT** + **ENTRAR** para actualizar los exploradores vinculados.
9. Cambie a Internet Explorer y observe cómo ha cambiado el color del borde.

    ![Internet Explorer: actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer: actualizarlo el color del borde")

    *Internet Explorer: actualizarlo el color del borde*
10. Cambie a Google Chrome y observe cómo ha cambiado el color del borde.

    ![Google Chrome: actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: actualizarlo el color del borde")

    *Google Chrome: actualizarlo el color del borde*
11. Cambie a Visual Studio.
12. Vaya al final de la **Site.css** archivo y presione **CTRL** + **F** para localizar el **.btn** selector.
13. Tenga en cuenta que el **- webkit-border-radius** propiedad está subrayada en verde.

    ![propiedad - webkit-border-radius del selector btn](visual-studio-2013-web-tools/_static/image21.png "propiedad - webkit-border-radius del selector btn")

    *propiedad - webkit-border-radius del selector btn*
14. Colocar el símbolo de intercalación en el **- webkit-border-radius** propiedad. Debe aparecer una línea azul debajo de la primera letra de la primera palabra de la propiedad. Se trata de la **de etiquetas inteligentes**.
15. Presione **CTRL** + **.** Abra el menú de sugerencias y haga clic en **agregar falta la propiedad estándar (border-radius)**.

    ![Falta la sugerencia de la propiedad estándar de agregar](visual-studio-2013-web-tools/_static/image22.png "agregar falta la sugerencia de la propiedad estándar")

    *Agregar falta la sugerencia de la propiedad estándar*
16. El **border-radius** propiedad se agrega automáticamente a la regla CSS.

    ![Falta la propiedad estándar agregado](visual-studio-2013-web-tools/_static/image23.png "falta la propiedad estándar agregado")

    *Falta la propiedad estándar agregado*
17. Mueva el puntero sobre el **border-radius** propiedad para mostrar el **información sobre herramientas de explorador matriz**. El **información sobre herramientas de explorador matriz** muestra la disponibilidad de la propiedad en cada explorador.

    ![Información sobre herramientas de explorador matriz](visual-studio-2013-web-tools/_static/image24.png "información sobre herramientas de explorador matriz")

    *Información sobre herramientas de explorador matriz*
18. Tenga en cuenta que el valor de la **border-radius** propiedad está subrayada todavía. Mueva el puntero sobre el valor para ver el mensaje de advertencia.

    ![Advertencia de valor de propiedad Border-radius](visual-studio-2013-web-tools/_static/image25.png "advertencia de valor de propiedad de radio de borde")

    *Advertencia de valor de propiedad de radio de borde*
19. Quite la unidad de la **border-radius** valor de propiedad como sugiere la información sobre herramientas.
20. Como **border-radius** es la propiedad estándar para definir el borde redondeado cómo son las esquinas, puede quitar el **- webkit-border-radius** propiedad y valor de la regla de CSS.
21. Colocar el símbolo de intercalación en el **ajuste** propiedad y observe que la etiqueta inteligente también aparece a continuación.
22. Abra el menú y haga clic en **agregar especificaciones de proveedor que falta**.

    ![Agregar la sugerencia de características específicas de proveedor que falta](visual-studio-2013-web-tools/_static/image26.png "agregar sugerencias de características específicas de proveedor que faltan")

    *Agregar la sugerencia de características específicas de proveedor que falta*
23. El **-ms-ajuste** propiedad se agrega automáticamente a la regla CSS.

    ![Propiedad de proveedor específica agregada](visual-studio-2013-web-tools/_static/image27.png "propiedad específico de proveedor agregada")

    *Propiedad de proveedor específica agregada*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarea 4: actualizar el código HTML desde el explorador

En esta tarea, utilizará el vínculo de explorador **el modo de diseño** característica para modificar el objeto DOM desde el explorador y transferir los cambios en el archivo de código fuente HTML en Visual Studio.

1. En Google Chrome, presione **CTRL** + **ALT** + **d.** para habilitar el modo de diseño.
2. Mueva el puntero sobre el **Lorem Ipsum dolor sit amet** etiqueta y haga clic en.

    ![Edición de la pregunta](visual-studio-2013-web-tools/_static/image28.png "la pregunta de edición")

    *Edición de la pregunta*
3. Debería aparecer un cursor. Reemplace el texto original con *¿cómo se ve como cuando escribe una pregunta más?* y, a continuación, presione **ESC** para salir del modo de diseño.

    ![Pregunta editar](visual-studio-2013-web-tools/_static/image29.png "pregunta editado")

    *Pregunta editado*
4. Cambiar a Visual Studio y abra **Index.cshtml**, si aún no está abierto. Tenga en cuenta que el texto interno de la **&lt;p&gt;** se ha actualizado el elemento.

    ![Pregunta de actualizado en la página HTML](visual-studio-2013-web-tools/_static/image30.png "pregunta actualizado en la página HTML")

    *Pregunta actualizada en la página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Advertencias relacionadas con la tarea 5: revisión de SEO

**Optimización de motor de búsqueda** (SEO) es el proceso de realizar un rango de sitio Web superior en la lista de un motor de búsqueda de resultados. Cuanto mayor sea el sitio clasifica y más coherente aparece, los visitantes más obtendrán el sitio de ese motor de búsqueda. Web Essentials incorpora una herramienta analítica que examina el HTML, los problemas de informes se encuentra y proporciona asistencia para corregirlas.

1. Vaya a la **vista** menú y haga clic en **lista de errores** para abrir el **lista de errores** ventana.

    ![Error en la vista Lista menú](visual-studio-2013-web-tools/_static/image31.png "lista de errores en el menú Ver")

    *Menú de vista de lista de errores*
2. Tenga en cuenta que hay una advertencia de SEO que le notifica que una **&lt;meta&gt;** para falta la descripción de la página de etiquetas. Haga doble clic en la entrada de advertencia de SEO para corregirlo.

    ![Ventana Lista de errores](visual-studio-2013-web-tools/_static/image32.png "ventana Lista de errores")

    *Ventana Lista de errores*
3. En el **Web Essentials** cuadro de diálogo, haga clic en **Sí** para insertar una descripción &lt;meta&gt; etiqueta.

    ![Cuadro de diálogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "cuadro de diálogo Web Essentials")

    *Cuadro de diálogo Web Essentials*
4. El editor para  **\_Layout.cshtml** se abre y la **&lt;meta&gt;** etiqueta a la que se agrega automáticamente a la **head** sección de la Archivo HTML.

    ![Etiqueta META que agrega automáticamente en la página de _diseño](visual-studio-2013-web-tools/_static/image34.png "etiqueta Meta que agrega automáticamente en la página de _diseño")

    *Etiqueta META que agrega automáticamente a \_página de diseño*
5. Cambie el valor de la **contenido** atributo *GeekQuiz* y guarde el archivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ejercicio 2: Sacar partido de IntelliSense y fragmentos de código

Web Essentials, el editor HTML se ha ampliado con funcionalidad adicional. En este ejercicio, verá algunas características nuevas que resultan útiles al desarrollar aplicaciones web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarea 1: uso de IntelliSense en documentos HTML

Se llama a la primera característica nueva que se ve en esta tarea **IntelliSense dinámico**. IntelliSense dinámica lee otras etiquetas y atributos para deducir los posibles identificadores que va a usar.

En esta tarea, creará un nuevo elemento de formulario HTML que contiene una etiqueta y un campo de entrada. A continuación, agregará un **para** atributo a la etiqueta para enlazarla a la entrada, para ver sugerencias de IntelliSense en función de los identificadores de las entradas en el ámbito.

1. Abrir **Visual Studio Express 2013 para Web** y **Begin.sln** solución se encuentra en el **Begin/origen/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **el Explorador de soluciones**, abra el **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.
3. Agregue el siguiente formulario dentro de la **&lt;sección&gt;** elemento.

    (Código de fragmento de código - *VisualStudio2013WebTooling* - *Ex2* - *formulario*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. La etiqueta input debe ir precedida de una etiqueta con una descripción del campo. Agregue la siguiente etiqueta antes de la etiqueta de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. El **para** atributo de un **&lt;etiqueta&gt;** especifica qué elemento de formulario de una etiqueta que está enlazado. El valor del atributo debe ser igual al identificador del elemento relacionado. Agregar el **para** atributo a la **&lt;etiqueta&gt;** elemento. Como se muestra en la siguiente ilustración, el &quot;nombre&quot; valor aparece en el cuadro de IntelliSense, según el identificador de los elementos dentro del mismo ámbito (incluido  **&lt;formulario&gt;**).

    ![Muestra el identificador en IntelliSense](visual-studio-2013-web-tools/_static/image35.png "que muestra el Id. de IntelliSense")

    *Que muestra el Id. de IntelliSense*
6. Eliminar el agregado recientemente **&lt;formulario&gt;** elemento y su contenido.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarea 2: usar fragmentos de código HTML

HTML5 introdujo más de 25 etiquetas semánticas nuevas. Visual Studio ya tenía la compatibilidad con IntelliSense para estas etiquetas, pero Visual Studio 2013 facilita más rápidas y fáciles de escribir el marcado mediante la adición de nuevos fragmentos de código. Aunque estas etiquetas no son complicadas, conlleva algunas sutilezas, tales como agregar el códec de reserva adecuado para el *audio* etiqueta. En esta tarea, verá los fragmentos de código HTML para la etiqueta de audio.

1. En el **Index.cshtml** de archivo, escriba  **&lt;aud** dentro de la **&lt;sección&gt;** elemento tal como se muestra en la ilustración siguiente.

    ![Inserción de un elemento audio](visual-studio-2013-web-tools/_static/image36.png "insertar un elemento de audio")

    *Inserción de un elemento de audio*
2. Presione **ficha** dos veces y observe cómo se agrega el código siguiente en la página y el cursor se coloca en el **src** atributo del primer origen.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Al presionar el **ficha** clave dos veces, el fragmento de código se inserta. El fragmento de audio muestra el uso estándar de la *audio* etiqueta, con dos archivos de origen para la compatibilidad mejorada.
3. Elimine la segunda línea y actualizar el origen de la primera línea con el siguiente vínculo en el estudio WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). El código resultante se muestra a continuación.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > El archivo de origen *KatanaProject.mp3* sirve como ejemplo. Puede usar otro origen, si lo prefiere.
4. Presione **CTRL** + **S** para guardar el archivo.
5. Presione **CTRL** + **F5** para iniciar la aplicación.
6. Tenga en cuenta que un Reproductor de audio se ha agregado a la aplicación.

    ![Reproductor de audio en Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Reproductor de Audio en Internet Explorer")

    *Reproductor de audio en Internet Explorer*

    ![Reproductor de audio en Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Reproductor de Audio en Google Chrome")

    *Reproductor de audio en Google Chrome*
7. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tarea 3: uso de IntelliSense en documentos de JavaScript

Con Web Essentials 2013, hojas de estilo y páginas HTML generan una lista de identificadores y nombres de clase. En esta tarea, obtendrá información sobre cómo mejoran la compatibilidad con IntelliSense para JavaScript en Web Essentials 2013 esas listas.

1. En el **Index.cshtml** , agregue el código siguiente para definir un **script** etiqueta para el código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Agregue el código siguiente dentro de la **script** etiqueta para definir la función de devolución de llamada listo.

    (Código de fragmento de código - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Colocar el símbolo de intercalación en el **script** etiqueta y presione **CTRL** + **.** Para abrir el menú de la sugerencia.
4. Haga clic en **extraer al archivo**.

    ![Extraer JavaScript a la sugerencia de archivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extraer a la sugerencia de archivo")

    *Extraer JavaScript a la sugerencia de archivo*
5. En el **Guardar como** ventana, seleccione el **Scripts** carpeta, el nombre del archivo **init.js** y haga clic en **guardar**.

    ![Guardar como ventana](visual-studio-2013-web-tools/_static/image40.png "ventana Guardar como")

    *Guardar como ventana*

    > [!NOTE]
    > El **init.js** se crea el archivo y el contenido de la secuencia de comandos se mueve al archivo.
    > 
    > ![Archivo init.js creado con el contenido incluido](visual-studio-2013-web-tools/_static/image41.png "archivo Init.js creado con el contenido incluido")
    > 
    > *Archivo init.js creado con el contenido incluido*
6. Abra el **Index.cshtml** de archivo y compruebe que la etiqueta de script se ha reemplazado por una referencia a la **init.js** archivo.

    ![Referencia de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referencia de html")

    *Referencia de html init.js*
7. Vaya a la **el Explorador de soluciones** y tenga en cuenta que el **init.js** archivo se incluye automáticamente en la solución.

    ![Archivo init.js incluido en la solución](visual-studio-2013-web-tools/_static/image43.png "archivo Init.js incluido en la solución")

    *Archivo init.js incluido en la solución*
8. Vuelva a la **init.js** archivo para actualizar el **listo** función de devolución de llamada.
9. Dentro de la definición de devolución de llamada de función que se pasa a *listo*, agregue el código siguiente para obtener todos los elementos mediante un atributo de clase específica.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Presione **CTRL** + **espacio** entre comillas dentro de la **getElementsByClassName** llamada de función.

    ![Que muestra IntelliSense para la función getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "que muestra IntelliSense para la función getElementsByClassName")

    *IntelliSense que muestra para la función getElementsByClassName*

    > [!NOTE]
    > Observe que IntelliSense muestra las clases definidas en las hojas de estilo del proyecto.
11. Reemplace la línea que ha creado con el código siguiente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Coloque el cursor después de **au** dentro de las comillas en el **getElementsByTagName** función y presione **CTRL** + **espacio**.

    ![Que muestra IntelliSense para el método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "que muestra IntelliSense para el método getElementByTagName")

    *Que muestra IntelliSense para el método getElementsByTagName*
13. Seleccione **&quot;audio&quot;** en la lista y presione **ENTRAR**. El resultado se muestra en la ilustración siguiente.

    ![Recuperar elementos de Audio](visual-studio-2013-web-tools/_static/image46.png "recuperar elementos de Audio")

    *Recuperar elementos de Audio*
14. En **el Explorador de soluciones**, haga clic en el **init.js** de archivos en el **Scripts** carpeta y seleccione **archivos JavaScript minimizar** desde el **Web Essentials** menú.

    ![Minimizar los archivos JavaScript](visual-studio-2013-web-tools/_static/image47.png "archivos JavaScript minimizar")

    *Minimizar los archivos de JavaScript*
15. Cuando se le solicite para habilitar la reducción automática cuando los cambios del archivo de origen, haga clic en **Sí**.

    ![Habilitar la advertencia de reducción automática](visual-studio-2013-web-tools/_static/image48.png "habilitar la advertencia de reducción automática")

    *Habilitar la advertencia de reducción automática*

    > [!NOTE]
    > El **init.min.js** se crea y se agrega como una dependencia de la **init.js** archivo.
    > 
    > ![Archivo init.min.js creado](visual-studio-2013-web-tools/_static/image49.png "archivo Init.min.js creado")
    > 
    > *Archivo init.min.js creado*
16. Abra el **init.min.js** de archivo y tenga en cuenta que el archivo se han minimizado.

    ![Contenido del archivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenido del archivo")

    *Contenido del archivo init.min.js*
17. En el **init.js** , agregue el código siguiente el **getElementsByTagName** llamada para reproducir todos los elementos de audio de función.

    (Código de fragmento de código - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Haga clic en **CTRL** + **S** para guardar el archivo. Puesto que ya está abierto el archivo minimizado, verá un cuadro de diálogo que dice que el archivo se modificó fuera del editor de origen. Haga clic en **Sí**.

    ![Advertencia de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "advertencia de Microsoft Visual Studio")

    *Advertencia de Microsoft Visual Studio*
19. Vuelva a la **init.min.js** archivo para comprobar que el archivo se ha actualizado con el nuevo código.

    ![Actualizado el archivo init.min.js](visual-studio-2013-web-tools/_static/image52.png "archivo Init.min.js actualizado")

    *Archivo init.min.js actualizado*
20. Haga clic en el **actualizar el vínculo explorador** botón.
21. Una vez que los dos exploradores se actualizan los reproductores de audio que se vio en la tarea anterior empezará a reproducirse automáticamente.

    ![Reproductor de audio incluido en la vista](visual-studio-2013-web-tools/_static/image53.png "incluido en la vista de Reproductor de Audio")

    *Reproductor de audio incluido en la vista*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Usar las nuevas características del editor HTML incluidas en Web Essentials como fragmentos de código enriquecidos de HTML5 y el Zen de codificación
- Usar las nuevas características del editor CSS incluidas en Web Essentials como el selector de colores y la información sobre herramientas de explorador matriz
- Usar las nuevas características del editor de JavaScript incluidas en Web Essentials, como extraer al archivo e IntelliSense para todos los elementos HTML
- Intercambiar datos entre su explorador y Visual Studio mediante el vínculo de explorador
