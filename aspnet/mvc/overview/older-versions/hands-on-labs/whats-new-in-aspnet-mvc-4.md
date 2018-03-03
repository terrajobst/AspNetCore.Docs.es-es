---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "' S New en ASP.NET MVC 4 | Documentos de Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de ASP.NET y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 35f9402ad6090c0441425a23b2b8063350892210
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>' S New en ASP.NET MVC 4

Por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de ASP.NET y .NET framework. Esta nueva, cuarta versión de framework se centra en facilitar el desarrollo de aplicaciones web móvil.

Para comenzar con, cuando se crea un nuevo proyecto de ASP.NET MVC 4 ahora hay una plantilla de proyecto de aplicación para dispositivos móviles que puede usar para compilar una aplicación independiente específicamente para dispositivos móviles. Además, ASP.NET MVC 4 se integra con jQuery Mobile a través de un paquete de NuGet jQuery.Mobile.MVC. jQuery Mobile es un marco basado en HTML5 para desarrollar aplicaciones web que son compatibles con todas las plataformas de dispositivos móviles populares, incluidos Windows Phone, iPhone, Android y así sucesivamente. Sin embargo, si necesita especialización, ASP.NET MVC 4 también permite actuar distintas vistas para diferentes dispositivos y proporcionar las optimizaciones específicas del dispositivo.

En este laboratorio práctico, se iniciará con ASP.NET MVC 4 &quot;aplicación de Internet&quot; plantilla de proyecto para crear una aplicación de la Galería fotográfica. Progresivamente mejorará la aplicación con jQuery Mobile y nuevas características de ASP.NET MVC 4 para que sea compatible con dispositivos móviles diferentes y exploradores web de escritorio. También obtendrá información sobre las recetas de código nuevo para la generación de código y lo ASP.NET MVC 4 que resulta más fácil de escribir métodos de acción asincrónicos admitiendo la tarea&lt;ActionResult&gt; tipos de valor devuelto.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [What's New en formularios Web Forms de ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Aprovechar las ventajas de las mejoras en las plantillas de proyecto incluidas ASP.NET MVC la nueva plantilla de proyecto de aplicación para dispositivos móviles
- Utilice el atributo de la ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejorar el progresiva y para la creación de la interfaz de usuario web táctiles optimizadas
- Crear vistas específicas de mobile
- Utilice el componente de modificador de vista para alternar entre las vistas de escritorio y móviles en la aplicación
- Crear controladores asincrónicos utilizando el soporte de tarea

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluido en la instalación de Microsoft Visual Studio 2012)
- Emulador de Windows Phone (incluido en el [7.1.1 de Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcional: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulador** extensión (solo para el ejercicio 3 usan para navegar por la aplicación web con un emulador de iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

En este documento de laboratorio, le indicará que insertar bloques de código. Para su comodidad, la mayor parte de ese código se proporciona como fragmentos de código de Visual Studio, puede utilizar desde dentro de Visual Studio para evitar tener que agregarla manualmente.

Para instalar los fragmentos de código:

1. Abra una ventana del explorador de Windows y vaya a la práctica **Source\Setup** carpeta.
2. Haga doble clic en el **Setup.cmd** archivo en esta carpeta para instalar los fragmentos de código de Visual Studio.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Nuevas plantillas de proyecto de MVC de ASP.NET 4](#Exercise1)
2. [Crear la aplicación Web de la Galería fotográfica](#Exercise2)
3. [Agregar compatibilidad para dispositivos móviles](#Exercise3)
4. [Usar controladores asincrónicos](#Exercise4)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ejercicio 1: Nuevas plantillas de proyecto de MVC de ASP.NET 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4. Además de la plantilla de aplicación de Internet, ya está presente en MVC 3, esta versión ahora incluye una plantilla independiente para las aplicaciones móviles. En primer lugar, veremos algunas características relevantes de cada una de las plantillas. A continuación, va a trabajar en la página correctamente en las diferentes plataformas de representación utilizando el enfoque correcto.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tarea 1: explorar la plantilla de aplicación de Internet

1. Abra **Visual Studio**.
2. Seleccione el **archivo | Nuevos | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.

    > [!NOTE]
    > Más adelante, personalizará la solución de la Galería fotográfica ASP.NET MVC 4 que ahora va a crear.

    ![Crear un nuevo proyecto](whats-new-in-aspnet-mvc-4/_static/image1.png "crear un nuevo proyecto")

    *Crear un nuevo proyecto*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como el motor de vista.

    ![Crear una nueva aplicación de Internet de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "crear una nueva aplicación de Internet de ASP.NET MVC 4")

    *Crear una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Se ha introducido la sintaxis Razor en ASP.NET MVC 3. Su objetivo es reducir el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite una rápida y fluido de flujo de trabajo de codificación. Aprovecha Razor existente C# / VB (u otros) dominio del idioma y ofrece una sintaxis de marcado de plantilla que permite a un flujo de trabajo de construcción de HTML Maravilla.
4. Presione **F5** para ejecutar la solución y ver las plantillas renovadas. Puede consultar las siguientes características:

    **Plantillas de estilo moderno**

    Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.

    ![Plantillas de MVC de ASP.NET 4 estilo](whats-new-in-aspnet-mvc-4/_static/image3.png "estilo plantillas de MVC 4")

    *Plantillas de estilo de ASP.NET MVC 4*

    ![Nuevo contacto, página](whats-new-in-aspnet-mvc-4/_static/image4.png "página nuevo contacto")

    *Nuevo contacto, página*

    **Representación adaptable**

    Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al tamaño de la ventana nueva. Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.

    ![Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador](whats-new-in-aspnet-mvc-4/_static/image5.png "plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador")

    *Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador*

    **Interfaz de usuario más enriquecida con JavaScript**

    Otra mejora para plantillas de proyecto predeterminadas es el uso de JavaScript para proporcionar un JavaScript más interactivo. Los vínculos de inicio de sesión y de registro usados en la plantilla son elementos que describen cómo usar la validaciones de jQuery para validar los campos de entrada del lado cliente.

    ![Validación de jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    Validación de jQuery

    > [!NOTE]
    > Aviso de que registro de los dos en secciones, en la primera sección se puede iniciar sesión con una cuenta registrado desde el sitio y en la segunda sección que puede altenativelly sesión con otro servicio de autenticación como google (deshabilitada de forma predeterminada).
5. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
6. Abra el archivo **AuthConfig.cs** situado bajo el **aplicación\_iniciar** carpeta.
7. Quite el comentario de la última línea para registrar el cliente de Google para *OAuth* autenticación.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Tenga en cuenta que puede habilitar fácilmente la autenticación mediante cualquier servicio OpenID o OAuth como Facebook, Twitter, Microsoft, etcetera.
8. Presione **F5** para ejecutar la solución y navegar hasta la página de inicio de sesión.
9. Seleccione **Google** servicio para iniciar sesión.

    ![Seleccionar el registro de servicio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    Seleccionar el registro de servicio
10. Inicie sesión con su cuenta de Google.
11. Permitir que el sitio (localhost) recuperar información de cuenta de Google.
12. Por último, tendrá que registrar en el sitio para asociar la cuenta de Google.

    ![Asociar su cuenta de Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    *Asociar su cuenta de Google*
13. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
14. Ahora, explore la solución para desproteger algunas otras nuevas características introducidas en ASP.NET MVC 4 en la plantilla de proyecto.

    ![La plantilla de proyecto de aplicación de ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

    *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

    - **HTML 5 marcado**

        Examinar vistas de plantilla para averiguar la nueva marca de tema.

        ![Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml.")

        *Nueva plantilla, uso de marcado de Razor y HTML5 (About.cshtml).*
    - **Bibliotecas actualizadas de JavaScript**

        La plantilla predeterminada de ASP.NET MVC 4 incluye ahora KnockoutJS, un marco de JavaScript MVVM que le permite crear enriquecidos y aplicaciones de alta capacidad de respuesta web con JavaScript y HTML. Al igual que en MVC3, jQuery y jQuery bibliotecas de interfaz de usuario también se incluyen en ASP.NET MVC 4.

        > [!NOTE]
        > Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Además, puede obtener información acerca de jQuery y jQuery UI en [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tarea 2: explorar la plantilla de aplicación para dispositivos móviles

ASP.NET MVC 4 facilita el desarrollo de sitios Web para dispositivos móviles y exploradores de Tablet PC. Esta plantilla tiene la misma estructura de aplicación que la plantilla de aplicación de Internet (Observe que el código del controlador es prácticamente idéntico), pero su estilo se modificó para que se representen correctamente en dispositivos móviles basados en la entrada táctil.

1. Seleccione el **archivo | Nuevos | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija la **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery.Mobile**, seleccione una ubicación (o deje el valor predeterminado), seleccione &quot;agregar a solución&quot; y haga clic en **Aceptar**.
2. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación Mobile** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como el motor de vista.

    ![Crear una nueva aplicación móvil de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "crear una nueva aplicación móvil de ASP.NET MVC 4")

    *Crear una nueva aplicación móvil de ASP.NET MVC 4*
3. Ahora es posible explorar la solución y extraer del repositorio algunas de las nuevas características introducidas por la plantilla de solución de ASP.NET MVC 4 para dispositivos móviles:

    - **jQuery Mobile biblioteca**

        La plantilla de proyecto de aplicación Mobile incluye la biblioteca de jQuery móvil, que es una biblioteca de código abierto para la compatibilidad de explorador móvil. jQuery Mobile aplica mejora progresiva a los exploradores móviles que admiten CSS y JavaScript. Mejora progresiva permite todos los exploradores mostrar el contenido básico de una página web, mientras que sólo permite que los exploradores más eficaces mostrar el contenido enriquecido. Los archivos JavaScript y CSS, incluidos en el estilo de dispositivos móvil, jQuery ayudan a los exploradores móviles para adaptarlos al contenido en la pantalla sin realizar ningún cambio en el marcado de la página.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *biblioteca de jQuery móvil incluido en la plantilla*
    - **Marcado en función de HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Plantilla de aplicaciones móviles con HTML5 marcado, (Login.cshtml y index.cshtml)*
4. Presione **F5** para ejecutar la solución.
5. Abra la **Windows Phone 7 emulador**.
6. En la pantalla de inicio de teléfono, abra Internet Explorer. Visite la dirección URL que se inicia la aplicación de escritorio y vaya a esa dirección URL desde el teléfono (p. ej. `http://localhost:[PortNumber]/`).
7. Ahora puede especificar la página de inicio de sesión o desproteger el acerca de la página. Tenga en cuenta que el estilo del sitio Web se basa en la nueva aplicación de Metro para dispositivos móviles. La plantilla de proyecto de ASP.NET MVC 4 se muestra correctamente en los dispositivos móviles, asegurándose de que todos los elementos de la página son visibles y están habilitados. Tenga en cuenta que los vínculos en el encabezado son lo suficientemente grandes como se hizo clic o puntea.

    ![Páginas de la plantilla en un dispositivo móvil del proyecto](whats-new-in-aspnet-mvc-4/_static/image14.png "proyecto páginas de plantilla en un dispositivo móvil")

    *Páginas de la plantilla de proyecto en un dispositivo móvil*
8. La nueva plantilla también usa el **ventanilla meta etiqueta**. Exploradores móviles más definen un ancho de una ventana del explorador virtual o &quot;ventanilla&quot;, que es mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles mostrar toda la página web dentro de la pantalla virtual. El **ventanilla meta etiqueta** permite a los desarrolladores de web establecer el ancho, el alto y la escala del área del explorador en dispositivos móviles **.** La plantilla de ASP.NET MVC 4 para las aplicaciones móviles establece la ventanilla para el ancho del dispositivo (&quot;ancho = dispositivo ancho&quot;) en la plantilla de diseño (*Views\Shared\_Layout.cshtml*), de modo que todos los el páginas tendrá su ventanilla establecida para el ancho de pantalla del dispositivo. Tenga en cuenta que la etiqueta meta de ventanilla no cambiará la vista de explorador predeterminado.
9. Abra  **\_Layout.cshtml**, que se encuentra en la **vistas | Compartido** carpeta, y marque como comentario la etiqueta meta de la ventanilla. Ejecute la aplicación, si no ya abierto y desproteger las diferencias.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    ![El sitio después de la creación de comentarios de la ventanilla meta etiqueta](whats-new-in-aspnet-mvc-4/_static/image15.png "el sitio después de la creación de comentarios de la ventanilla meta etiqueta")

    *El sitio después de la creación de comentarios de la ventanilla meta etiqueta*
10. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
11. Quite el comentario de la etiqueta de metadatos de la ventanilla.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tarea 3: usar la representación adaptable

En esta tarea, obtendrá información sobre otro método para presentar una página Web correctamente en los dispositivos móviles y exploradores Web al mismo tiempo sin ninguna personalización. Ya ha usado ventanilla meta etiqueta con un propósito similar. Ahora que adapta a otro método eficaz: *representación adaptable*.

Representación adaptable es una técnica que usa **consultas de medios de CSS3** para personalizar el estilo aplicado a una página. Consultas de medios definen las condiciones dentro de una hoja de estilos, agrupar los estilos CSS en una determinada condición. Solo cuando la condición es true, el estilo se aplica a los objetos declarados.

La flexibilidad proporcionada por la técnica de representación adaptable permite cualquier personalización para mostrar el sitio en diferentes dispositivos. Puede definir tantos estilos como desee en una sola hoja de estilos sin tener que escribir código con la lógica para elegir el estilo. Por lo tanto, es una manera muy ordenada de adaptar los estilos de la página, ya que reduce la cantidad de código duplicado y la lógica para fines de representación. Por otro lado, el consumo de ancho de banda aumentaría, como el tamaño de los archivos CSS podría aumentar ligeramente.

Mediante la técnica de representación adaptable, su sitio será **muestre correctamente, independientemente del explorador.** Sin embargo, debe considerar si se carga el ancho de banda adicional es un problema.

> [!NOTE]
> El formato básico de una consulta de medios es: @media \[ámbito: todos | mano | imprimir | proyección | pantalla\] ([: valor de propiedad] y... [propiedad: valor])


Ejemplos de consultas de medios: &gt;  **@media todos y (ancho máximo: 1000px) y (min ancho: 700px) {}:** para todas las resoluciones entre 700px y 1000px.

> **@media pantalla y (min ancho: 400px) y (ancho máximo: 700px) {...}:** solo para las pantallas. La resolución debe estar entre 400 y 700px.
> 
> **@media portátiles y (min ancho: 20em), pantalla y (min ancho: 20em) {...}:** para dispositivos de mano (mobile y dispositivos) y las pantallas. El ancho mínimo debe ser mayor que 20em.
> 
> Puede encontrar más información sobre esto en la [sitio W3C](http://www.w3.org/TR/css3-mediaqueries/).


Ahora explorará el funcionamiento de la representación adaptable, mejorar la legibilidad de ASP.NET MVC 4 predeterminados plantilla de sitio Web.

1. Abra la **PhotoGallery.sln** solución se ha creado en la tarea 1 y se selecciona el **PhotoGallery** proyecto. Presione **F5** para ejecutar la solución.
2. Cambiar el tamaño de ancho del explorador, establecer las ventanas a la mitad o menos de un cuarto de su tamaño original. Tenga en cuenta lo que ocurre con los elementos en el encabezado: algunos elementos no aparecerán en el área visible del encabezado.
3. Abra **Site.css** archivo desde el Explorador de soluciones de Visual Studio, ubicado en **contenido** carpeta del proyecto. Presione **CTRL + F** para abrir la búsqueda integrada en Visual Studio y escribir  **@media**  para buscar la **consulta de medios CSS**.

    La condición de consulta de medios definida en esta plantilla funciona de esta manera: cuando el tamaño de la ventana del explorador está por debajo **850 px**, las reglas CSS que se aplican son los definidos dentro de este bloque de medios.

    ![Buscando la consulta de medios](whats-new-in-aspnet-mvc-4/_static/image16.png "buscando la consulta de medios")

    *Buscando la consulta de medios*
4. Reemplace el valor de atributo de ancho máximo establecido en 850 px con **10px**, con el fin de deshabilitar la representación adaptable y presione **CTRL + S** para guardar los cambios. Volver al explorador y presione **CTRL + F5** para actualizar la página con los cambios realizados. Tenga en cuenta las diferencias en ambas páginas al ajustar el ancho de la ventana.

    ![En la izquierda, la página está aplicando el @media se omite style, en la parte derecha, el estilo de](whats-new-in-aspnet-mvc-4/_static/image17.png "en la izquierda, está aplicando la página el @media se omite style, en la parte derecha, el estilo de")

    *En la izquierda, la página está aplicando la @media se omite style, en la parte derecha, el estilo*

    Ahora, vamos a desproteger lo que sucede en los dispositivos móviles:

    ![En la izquierda, la página está aplicando el @media se omite style, en la parte derecha, el estilo de](whats-new-in-aspnet-mvc-4/_static/image18.png "en la izquierda, está aplicando la página el @media se omite style, en la parte derecha, el estilo de")

    *En la izquierda, la página está aplicando la @media se omite style, en la parte derecha, el estilo*

    Aunque, observará que los cambios cuando se representa la página en un explorador Web no son muy importantes, cuando se usa un dispositivo móvil resulta más evidentes en las diferencias. En el lado izquierdo de la imagen, podemos ver que el estilo personalizado mejora la legibilidad.

    Representación adaptable puede usarse en muchos escenarios más, lo que sea más fácil aplicar estilos a un sitio Web y solucionar los problemas comunes de estilo con un enfoque ordenado condicional.

    La etiqueta meta de la ventanilla y las consultas de medios CSS no son específicas de ASP.NET MVC 4, por lo que puede aprovechar las ventajas de estas características en cualquier aplicación web.
5. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ejercicio 2: Crear la aplicación Web de la Galería fotográfica

En este ejercicio, trabajará en una aplicación de la Galería fotográfica para mostrar fotografías. Se iniciará con la plantilla de proyecto de ASP.NET MVC 4 y, a continuación, agregará una función para recuperar las fotos de un servicio y mostrarlos en la página principal.

En el siguiente ejercicio, se actualizará esta solución para mejorar la forma en que se muestra en los dispositivos móviles.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tarea 1: crear un servicio de fotografía ficticio

En esta tarea, creará un simulacro del servicio para recuperar el contenido que se mostrará en la Galería fotográfica. Para ello, agregará un controlador nuevo que va a devolver simplemente un archivo JSON con los datos de cada fotografía.

1. Abra **Visual Studio** si aún no está abierto.
2. Seleccione el **archivo | Nuevos | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**. Como alternativa, puede seguir trabajando desde la versión existente de ASP.NET MVC 4 **aplicación de Internet** solución de **ejercicio 1** y omitir el paso siguiente.
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que tiene seleccionado como el motor de vistas de Razor.
4. En el **el Explorador de soluciones**, haga clic en el **aplicación\_datos** carpeta del proyecto y seleccione **agregar | Elemento existente**. Vaya a la **Source\Assets\App\_datos** carpeta de este laboratorio y agregue el **Photos.json** archivo.
5. Crear un nuevo controlador con el nombre **PhotoController**. Para ello, haga doble clic en el **controladores** carpeta, vaya a **agregar** y seleccione **controlador.** Escriba el nombre de controlador, deje el **controlador MVC vacío** plantilla y haga clic en **agregar**.

    ![Agregar el PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "agregando el PhotoController")

    *Agregar el PhotoController*
6. Reemplace el **índice** método con el siguiente **galería** acción y devuelve el contenido del archivo JSON que haya agregado recientemente al proyecto.

    (Código de fragmento de código: *acción de la Galería de ASP.NET MVC 4 laboratorio - Ex02 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Presione **F5** para ejecutar la solución y, a continuación, vaya a la dirección URL siguiente para probar el servicio de fotografías simuladas: `http://localhost:[port]/photo/gallery` (el valor de [puerto] depende de puerto actual donde se inicia la aplicación). La solicitud a esta dirección URL debe recuperar el contenido de la **Photos.json** archivo.

    ![Probar el servicio de fotos simuladas](whats-new-in-aspnet-mvc-4/_static/image20.png "probar el servicio de fotos simuladas")

    *Probar el servicio de fotos simuladas*

En una implementación real podría utilizar [ASP.NET Web API](../../../../web-api/index.md) para implementar el servicio de la Galería fotográfica. ASP.NET Web API es un marco que facilita la creación de servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tarea 2: mostrar la Galería fotográfica

En esta tarea, actualizará la página de inicio para mostrar la Galería fotográfica mediante el servicio ficticios que creó en la primera tarea de este ejercicio. Agregará los archivos de modelo y actualizar las vistas de la galería.

1. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
2. Crear el **fotográfica** clase en el **modelos** carpeta. Para ello, haga doble clic en el **modelos** carpeta, seleccione **agregar** y haga clic en **clase**. A continuación, establezca el nombre **Photo.cs** y haga clic en **agregar**.
3. Agregue los siguientes miembros a la **fotográfica** clase.

    (Código de fragmento de código: *modelo foto de ASP.NET MVC 4 laboratorio - Ex02 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Abra el archivo **HomeController.cs** desde la carpeta **Controladores**.
5. Agregue las siguientes instrucciones de uso.

    (Código de fragmento de código: *HomeController usos de ASP.NET MVC 4 laboratorio - Ex02 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Actualización del **índice** acción que se utilizará **HttpClient** para recuperar los datos de la galería y, a continuación, usar el **JavaScriptSerializer** para deserializar en el modelo de vista.

    (Código de fragmento de código: *acción del índice de ASP.NET MVC 4 laboratorio - Ex02 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Abra la **Index.cshtml** archivo situado bajo el **Views\Home** carpeta y reemplace todo el contenido con el código siguiente.

    Este código recorre en bucle todas las fotos que se recupera del servicio y las muestra en una lista sin ordenar.

    (Código de fragmento de código: *lista de fotos de ASP.NET MVC 4 laboratorio - Ex02 -*)


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. En el **el Explorador de soluciones**, haga clic en el **contenido** carpeta del proyecto y seleccione **agregar | Elemento existente**. Vaya a la **Source\Assets\Content** carpeta de este laboratorio y agregue el **Site.css** archivo. Tendrá que confirmar su reemplazo. Si tiene la **Site.css** abierto el archivo, tendrá que confirmar para volver a cargar el archivo también.
9. Abra el Explorador de archivos y copiar todo el **fotos** carpeta se encuentra en la **Source\Assets** carpeta de este laboratorio a la carpeta raíz del proyecto en el Explorador de soluciones.
10. Ejecute la aplicación. Ahora debería ver la página principal muestra las fotos en la galería.

    ![La Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image21.png "la Galería fotográfica")

    *La Galería fotográfica*
11. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ejercicio 3: Agregar compatibilidad con dispositivos móviles

Una de las actualizaciones de claves en ASP.NET MVC 4 es la compatibilidad para el desarrollo móvil. En este ejercicio, explorará las nuevas características de ASP.NET MVC 4 para aplicaciones móviles mediante la extensión de la solución de la Galería fotográfica que ha creado en el ejercicio anterior.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tarea 1: instalar jQuery Mobile en una aplicación de ASP.NET MVC 4

1. Abra la **comenzar** solución ubicado en **origen/Ex3-MobileSupport/Begin/** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la **Package Manager Console** haciendo clic en el **herramientas** &gt; **Administrador de paquetes de biblioteca** &gt; **el Administrador de paquetes Consola** opción de menú.

    ![Abra la consola de administrador de paquetes de NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "abrir la consola de administrador de paquetes de NuGet")

    *Abra la consola de administrador de paquetes de NuGet*
3. En la consola de administrador de paquetes, ejecute el siguiente comando para instalar el **jQuery.Mobile.MVC** paquete.

    jQuery Mobile es una biblioteca de código abierto para la creación de la interfaz de usuario web táctil optimizada. El paquete de NuGet jQuery.Mobile.MVC incluye aplicaciones auxiliares para usar jQuery Mobile con una aplicación de ASP.NET MVC 4.

    > [!NOTE]
    > Al ejecutar el comando siguiente, se descarga la biblioteca jQuery.Mobile.MVC de Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Este comando instala jQuery Mobile y algunos archivos de aplicación auxiliar, incluido lo siguiente:

    - **Vistas/compartida/\_Layout.Mobile.cshtml**: es un diseño basado en Mobile de jQuery optimizado para una pantalla más pequeña. Cuando el sitio Web recibe una solicitud de un explorador móvil, reemplazará el diseño original (\_Layout.cshtml) con éste.
    - Un componente de modificador de vista: consta de los **vistas/compartida/\_ViewSwitcher.cshtml** vista parcial y la **ViewSwitcherController.cs** controlador. Este componente mostrará un vínculo en exploradores móviles para permitir a los usuarios cambiar a la versión de escritorio de la página.  
        ![Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil](whats-new-in-aspnet-mvc-4/_static/image23.png "proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil")

        *Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil*
4. Registrar las agrupaciones móviles. Para ello, abra el **Global.asax.cs** de archivos y agregue la siguiente línea.

    (Código de fragmento de código: *agrupaciones móviles de ASP.NET MVC 4 laboratorio - Ex03 - Register*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Ejecute la aplicación mediante un explorador web de escritorio.
6. Abra la **emulador de Windows Phone 7,** ubicado en **menú Inicio | Todos los programas | Windows Phone SDK 7.1 | Emulador de Windows Phone.**
7. En la pantalla de inicio de teléfono, abra Internet Explorer. Visite la dirección URL donde se inicia la aplicación y vaya a esa dirección URL con el Explorador de teléfono (p. ej. `http://localhost:[PortNumber]/`).

    Observará que la aplicación tendrá un aspecto distinta en el emulador de Windows Phone, como el jQuery.Mobile.MVC ha creado nuevos activos del proyecto que muestren vistas optimizadas para dispositivos móviles.

    Observe el mensaje en la parte superior del teléfono, que muestra el vínculo que se activa en la vista de escritorio. Además, el  **\_Layout.Mobile.cshtml** diseño que se creó el paquete ha instalado incluyen un diseño diferente en la aplicación.

    > [!NOTE]
    > Hasta ahora, no hay ningún vínculo para volver a la vista móvil. Se incluirá en versiones posteriores.

    ![Vista móvil de la página principal de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image24.png "vista móvil de la página principal de la Galería fotográfica")

    *Vista móvil de la página principal de la Galería fotográfica*
8. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tarea 2: creación de vistas móviles

En esta tarea, creará una versión móvil de la vista de índice con contenido adaptado para appareance mejor en dispositivos móviles.

1. Copia la **Views\Home\Index.cshtml** ver y péguelo para crear una copia, cambie el nombre del nuevo archivo a **Index.Mobile.cshtml**.
2. Abrir la nueva creada **Index.Mobile.cshtml** ver y reemplazar a las &lt;ul&gt; etiqueta con este código. Al hacerlo, actualizará el &lt;ul&gt; etiqueta con las anotaciones de datos móviles de jQuery que se va a utilizar los temas de jQuery mobile.


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Tenga en cuenta que:
    > 
    > - El **datos rol** atributo establecido en **listview** se representarán la lista con los estilos de listview.
    > 
    > - El **datos bajorrelieve** atributo establecido en true mostrará la lista con borde redondeado y margen.
    > 
    > - El **filtro de datos** atributo establecido en **true** generará un cuadro de búsqueda.
    > 
    > Puede aprender más acerca de las convenciones de móviles jQuery en la documentación del proyecto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Presione **CTRL + S** para guardar los cambios.
4. Cambie a la **emulador de Windows Phone** y actualizar el sitio. Tenga en cuenta la nueva apariencia y funcionamiento de la lista de la galería, así como el nuevo cuadro de búsqueda que se encuentra en la parte superior. A continuación, escriba una palabra en el cuadro de búsqueda (por ejemplo, **Tulips**) para iniciar una búsqueda en la Galería fotográfica.

    ![Galería con estilo de listview con filtrado](whats-new-in-aspnet-mvc-4/_static/image25.png "utilizando el estilo de listview con el filtrado de la Galería")

    *Utilizando el estilo de listview con el filtrado de la Galería*

    En resumen, ha utilizado la receta Mobilizer de vista para crear una copia de la vista de índice con la &quot;móvil&quot; sufijo. Este sufijo indica a ASP.NET MVC 4 que cada solicitud que se genera a partir de un dispositivo móvil utilizará esta copia del índice. Además, se actualizó la versión móvil de la vista de índice para usar jQuery Mobile para mejorar la apariencia y funcionamiento de sitio en los dispositivos móviles.
5. Vuelva a Visual Studio y abra **Site.Mobile.css** situado bajo el **contenido** carpeta.
6. Corrija la posición del título fotográfica para que aparezca en el lado derecho de la imagen. Para ello, agregue el código siguiente a la **Site.Mobile.css** archivo.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Presione **CTRL + S** para guardar los cambios.
8. Vuelva a la **emulador de Windows Phone** y actualizar el sitio. Tenga en cuenta que el título de la fotografía está colocado correctamente ahora.

    ![Título que se coloca en el lado derecho de la imagen](whats-new-in-aspnet-mvc-4/_static/image26.png "título situado en el lado derecho de la imagen")

    *Título que se coloca en el lado derecho de la imagen*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tarea 3: temas de jQuery Mobile

Cada diseño y el widget de jQuery Mobile está diseñado alrededor de un nuevo marco CSS orientada a objetos que permite aplicar un tema de completar el diseño visual unificado a sitios y aplicaciones.

predeterminado de jQuery Mobile tema incluye 5 muestras que se asignan letras (a, b, c y d, e) para una referencia rápida.

En esta tarea, actualizará el diseño móvil para que utilice un tema diferente que el valor predeterminado.

1. Cambie a Visual Studio.
2. Abra la  **\_Layout.Mobile.cshtml** archivo se encuentra en **Views\Shared**.
3. Busque el elemento div con el rol de datos establecido en &quot;página&quot; y actualizar la **datos tema** a &quot; **e**&quot;.


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Presione **CTRL + S** para guardar los cambios.
5. Actualizar el sitio en el **emulador de Windows Phone** y observe la nueva combinación de colores.

    ![Diseño móvil con una combinación de colores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "diseño móvil con una combinación de colores diferentes")

    *Diseño móvil con una combinación de colores diferentes*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tarea 4: mediante el componente de modificador de vista y el explorador reemplazar características

Es una convención para las páginas de web móvil optimizada agregar un vínculo cuyo texto está algo como vista de escritorio o en modo de sitio completo que permite a los usuarios cambiar a una versión de escritorio de la página. El paquete jQuery.Mobile.MVC incluye un ejemplo de **modificador de vista** componente para este propósito que se utiliza en el  **\_Layout.Mobile.cshtml** vista.

![Vínculo para cambiar a vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image28.png "vínculo para cambiar a vista de escritorio")

*Use el vínculo para cambiar a la vista de escritorio*

El modificador de vista utiliza una nueva característica denominada **explorador reemplazar**. Esta característica permite que la aplicación trate las solicitudes como si procediesen de un explorador distinto (agente de usuario) a la que realmente proceden del.

En esta tarea, explorará la implementación del ejemplo de un modificador de vista agregada por jQuery.Mobile.MVC y el nuevo explorador reemplazar las características de ASP.NET MVC 4.

1. Cambie a Visual Studio.
2. Abra la  **\_Layout.Mobile.cshtml** vista se encuentra en la **Views\Shared** carpeta y observe el componente de modificador de vista que se hace referencia como una vista parcial.

    ![Diseño móvil mediante el componente de modificador de vista](whats-new-in-aspnet-mvc-4/_static/image29.png "diseño móvil mediante el componente de modificador de vista")

    *Diseño móvil mediante el componente de modificador de vista*
3. Abra la  **\_ViewSwitcher.cshtml** vista parcial.

    La vista parcial utiliza el nuevo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar el origen de la solicitud web y mostrar el vínculo correspondiente para pasar a las vistas de escritorio o móvil.

    El **GetOverridenBrowser** método devuelve un **HttpBrowserCapabilitiesBase** instancia que se corresponde con el agente de usuario establecido actualmente para la solicitud (real o reemplazada). Puede usar este valor para obtener propiedades como **IsMobileDevice**.

    ![Vista parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher de vista parcial")

    *Vista parcial ViewSwitcher*
4. Abra la **ViewSwitcherController.cs** clase ubicado en el **controladores** carpeta. Desprotección esa acción SwitchView es llamado por el vínculo en el componente ViewSwitcher y observe los nuevos métodos de HttpContext.

    - El **HttpContext.ClearOverridenBrowser()** método quita cualquier agente de usuario invalidado para la solicitud actual.
    - El **HttpContext.SetOverridenBrowser()** método invalida el valor de agente de usuario real de la solicitud mediante el agente de usuario especificado.  
        ![Controlador de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")  
*Controlador ViewSwitcher*

        Reemplazando explorador es una característica fundamental de ASP.NET MVC 4, que también está disponible incluso si no instala el paquete jQuery.Mobile.MVC. Sin embargo, esta característica puede afectar a solo vista, diseño y vista parcial y no afecta a ninguna de las características que dependen del objeto Request.Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tarea 5: agregar al modificador de vista en la vista de escritorio

En esta tarea, actualizará el diseño del escritorio para que incluya al modificador de vista. Esto permitirá a los usuarios móviles volver a la vista móvil al examinar la vista de escritorio.

1. Actualizar el sitio en el **emulador de Windows Phone**.
2. Haga clic en el **vista de escritorio** vínculo en la parte superior de la galería. Tenga en cuenta que no hay ningún modificador de vista en la vista de escritorio para permitir que volver a la vista móvil.
3. Vuelva a Visual Studio y abra el  **\_Layout.cshtml** vista.
4. Busque la sección de inicio de sesión e inserte una llamada para representar la  **\_ViewSwitcher** vista parcial siguiente la  **\_LogOnPartial** vista parcial. A continuación, presione **CTRL + S** para guardar los cambios.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Presione **CTRL + S** para guardar los cambios.
6. Actualice la página en el emulador de Windows Phone y haga doble clic en la pantalla para acercar. Observe que ahora se muestra en la página principal de la **vista móvil** vínculo que se activa desde dispositivos móviles en vista de escritorio.

    ![Ver modificador que se representan en la vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image32.png "cambiador de vistas que se representan en la vista de escritorio")

    *Modificador de vista que se representan en la vista de escritorio*
7. Cambie a la vista móvil nuevo y vaya a **sobre** página (http://localhost[puerto]/principal/acercade). Tenga en cuenta que, incluso si no ha creado una vista About.Mobile.cshtml, acerca de se muestra la página con el diseño móvil (\_Layout.Mobile.cshtml).

    ![Acerca de la página](whats-new-in-aspnet-mvc-4/_static/image33.png "acerca de la página")

    *Acerca de la página*
8. Por último, abra el sitio en un explorador Web de escritorio. Tenga en cuenta que ninguna de las actualizaciones anteriores ha afectado a la vista de escritorio.

    ![Vista de escritorio de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image34.png "vista de escritorio de la Galería fotográfica")

    *Vista de escritorio de la Galería fotográfica*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tarea 6: crear nuevos modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que se va a generar la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página de inicio, la aplicación devolverá la **Views\Home\Index.cshtml** plantilla. A continuación, si un explorador móvil solicita la página de inicio, la aplicación devolverá la **Views\Home\Index.mobile.cshtml** plantilla.

En esta tarea, creará un diseño personalizado para los dispositivos iPhone y tendrá que simular las solicitudes de dispositivos iPhone. Para ello, puede usar cualquier un iPhone emulador o simulador (como [Electric simulador Mobile](http://www.electricplum.com/)) o un explorador con los complementos que modifican el agente de usuario. Para que obtener instrucciones sobre cómo establecer la cadena de agente de usuario en un explorador Safari emular un iPhone, consulte [cómo permiten Safari Finjan es IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) en el blog de David Alison.

**Tenga en cuenta que esta tarea es opcional y puede continuar a lo largo del laboratorio sin ejecutarla.**

1. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
2. Abra **Global.asax.cs** y agregue la siguiente instrucción using.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Agregue el código que aparece resaltado en la aplicación\_Start (método).

    (Código de fragmento de código: *iPhone DisplayMode de ASP.NET MVC 4 laboratorio - Ex03 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    Se ha registrado un nuevo **DefaultDisplayMode denominado &quot;iPhone&quot;**, en el método estático **DisplayModeProvider.Instance.Modes** lista estática, que se comparará con cada solicitud entrante. Si la solicitud entrante contiene la cadena &quot;iPhone&quot;, ASP.NET MVC se detectarán las vistas cuyo nombre contenga la &quot;iPhone&quot; sufijo. El parámetro 0 indica cómo específico es el nuevo modo; Por ejemplo, esta vista es más específica que la ficha general &quot;.mobile&quot; regla que coincide con las solicitudes de dispositivos móviles.

    Después de ejecuta este código, cuando un explorador de iPhone genera una solicitud, la aplicación utilizará la **Views\Shared\\_Layout.iPhone.cshtml** diseño que se va a crear en los pasos siguientes.

    > [!NOTE]
    > Esta manera de probar la solicitud para iPhone se ha simplificado para fines de demostración y podría no funcionar según lo previsto para cada cadena de agente de usuario de iPhone (para la prueba de ejemplo distingue mayúsculas de minúsculas).
4. Crear una copia de la  **\_Layout.Mobile.cshtml** un archivo en el **Views\Shared** carpeta y cambiar el nombre de la copia a &quot;  **\_Layout.iPhone.csthml** &quot;.
5. Abra  **\_Layout.iPhone.csthml** que creó en el paso anterior.
6. Busque el elemento div con el atributo de rol de datos establecido en **página** y cambie el **datos tema** atribuir a &quot; **una**&quot;.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    Ahora tiene 3 diseños en la aplicación de ASP.NET MVC 4:

    1. **\_Layout.cshtml**: diseño predeterminado utilizado para los exploradores de escritorio.
    2. **\_Layout.Mobile.cshtml**: diseño predeterminado utilizado para dispositivos móviles.
    3. **\_Layout.iPhone.cshtml**: diseño específico de dispositivos de iPhone, mediante una combinación de colores diferentes para diferenciar de \_Layout.mobile.cshtml.
7. Presione **F5** para ejecutar la aplicación y explora el sitio en el **emulador de Windows Phone**.
8. Abrir un **iPhone simulador** (consulte [Apéndice C](#AppendixC) para obtener instrucciones sobre cómo instalar y configurar un emulador de iPhone) y busque el sitio demasiado. Tenga en cuenta que cada teléfono está usando la plantilla específica.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Usar vistas diferentes para cada dispositivo móvil*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ejercicio 4: Usar controladores asincrónicos

Microsoft .NET Framework 4.5 presenta nuevas características del lenguaje en C# y Visual Basic para proporcionar una nueva base de asincronía en programación de. NET. Esta nueva base hace que la programación asincrónica similar a - y unos tan sencillo como - programación sincrónica. Ahora es posible escribir métodos de acción asincrónicos en ASP.NET MVC 4 mediante la **AsyncController** clase. Puede utilizar métodos de acción asincrónicos de ejecución prolongada, no con la CPU solicitudes. Esto evita que el servidor Web realizar trabajo mientras se procesa la solicitud de bloqueo. La clase AsyncController normalmente se usa para llamadas de servicio Web de ejecución prolongada.

Este ejercicio explica los conceptos básicos de la operación asincrónica en ASP.NET MVC 4. Si desea un análisis más profundo, puede consultar el siguiente artículo: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tarea 1: implementar un controlador asincrónico

1. Abra la **comenzar** solución ubicado en **origen/Ex4-Async/Begin/** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la **HomeController.cs** clase desde el **controladores** carpeta.
3. Agregue la siguiente instrucción using.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Actualización de la **HomeController** clase herede de **AsyncController**. Controladores que se derivan de AsyncController permiten a ASP.NET procesar solicitudes asincrónicas y pueden métodos de acción sincrónicos de servicio.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Agregar el **async** palabra clave para la **índice** método y hacer que el tipo de valor devuelto **tarea&lt;ActionResult&gt;**.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > El **async** palabra clave es una de las nuevas palabras clave proporciona .NET Framework 4.5; indica al compilador que este método contiene código asincrónico. A **tarea** objeto representa una operación asincrónica que puede completar en algún momento en el futuro.
6. Reemplace el **cliente. GetAsync()** llamada con la versión de async completa mediante palabra clave await, tal y como se muestra a continuación.

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > En la versión anterior, usaba la **resultado** propiedad desde el **tarea** objeto para bloquear el subproceso hasta que el resultado se devuelve (versión de sincronización).
    > 
    > Agregar el **await** palabra clave indica al compilador que esperar para la tarea devuelta desde la llamada al método de forma asincrónica. Esto significa que el resto del código se ejecutará como una devolución de llamada únicamente cuando haya finalizado el método esperado. Otra cosa que debe tener en cuenta es que no es necesario cambiar el bloque try-catch para solucionar este problema: todavía se detectarán las excepciones que se producen en segundo plano o en primer plano sin ningún trabajo adicional con un controlador proporcionado por el marco de trabajo.
7. Cambiar el código para continuar con la implementación asincrónica reemplazando las líneas con el nuevo código, tal y como se muestra a continuación

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Ejecute la aplicación. Observará que no hay cambios importantes, pero el código, no se bloqueará un subproceso del grupo de subprocesos que hace un mejor uso de los recursos del servidor y se mejora el rendimiento.

    > [!NOTE]
    > Puede aprender más sobre las nuevas características de programación asincrónicas en el laboratorio &quot; **programación asincrónica en .NET 4.5 con C# y Visual Basic** &quot; incluidos en el Kit de aprendizaje Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tarea 2: los tiempos de espera de control con Tokens de cancelación

Métodos de acción asincrónicos que devuelven instancias de la tarea también admiten los tiempos de espera. En esta tarea, actualizará el código del método de índice para controlar una situación de tiempo de espera con un token de cancelación.

1. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.
2. Agregue la siguiente instrucción using en la **HomeController.cs** archivo.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Actualizar la acción del índice para recibir un **CancellationToken** argumento.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Actualización de la **GetAsync** llamada para pasar el token de cancelación.

    (Código de fragmento de código: *ASP.NET MVC 4 laboratorio - Ex04 - SendAsync con CancellationToken*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorar la *índice* método con un **AsyncTimeout** atributo establecido en 500 milisegundos y un **HandleError** configurado para controlar el atributo  **TaskCanceledException** redirigiendo a un **TimedOut** vista.

    (Código de fragmento de código: *atributos de ASP.NET MVC 4 laboratorio - Ex04 -*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Abra el **PhotoController** clase y actualizar el **galería** método retrasar los milisegundos de ejecución 1000 (1 segundo) para simular una tarea de ejecución prolongada.


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Abra la **Web.config** de archivos y habilitar errores personalizados agregando el siguiente elemento.


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Crear una nueva vista en **Views\Shared** denominado **TimedOut** y usar el diseño predeterminado. En el Explorador de soluciones, haga clic en el **Views\Shared** carpeta y seleccione **agregar | Vista**.

    ![Usar vistas diferentes para cada dispositivo móvil](whats-new-in-aspnet-mvc-4/_static/image36.png "con vistas diferentes para cada dispositivo móvil")

    *Usar vistas diferentes para cada dispositivo móvil*
9. Actualización de la **TimedOut** ver el contenido tal y como se muestra a continuación.


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Ejecute la aplicación y vaya a la dirección URL raíz. Tal y como se ha agregado un **Thread.Sleep** de 1000 milisegundos, obtendrá un error de tiempo de espera, generado por el **AsyncTimeout** de atributo y captura el **HandleError** atributo.

    ![Excepción de tiempo de espera controlan](whats-new-in-aspnet-mvc-4/_static/image37.png "excepción de tiempo de espera controlan")

    *Excepción de tiempo de espera controlan*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [apéndice D: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio de prácticas, se han observado que algunas de las nuevas características residente en ASP.NET MVC 4. Se han tratado los conceptos siguientes:

- Aprovechar las ventajas de las mejoras en las plantillas de proyecto incluidas ASP.NET MVC la nueva plantilla de proyecto de aplicación para dispositivos móviles
- Utilice el atributo de la ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejorar el progresiva y para la creación de la interfaz de usuario web táctiles optimizadas
- Crear vistas específicas de mobile
- Utilice el componente de modificador de vista para alternar entre las vistas de escritorio y móviles en la aplicación
- Crear controladores asincrónicos utilizando el soporte de tarea

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-aspnet-mvc-4/_static/image38.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](whats-new-in-aspnet-mvc-4/_static/image40.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](whats-new-in-aspnet-mvc-4/_static/image41.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)***

1. Haga clic en donde desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
3. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](whats-new-in-aspnet-mvc-4/_static/image42.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](whats-new-in-aspnet-mvc-4/_static/image43.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Express de VS para icono Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Apéndice C: instalar WebMatrix 2 y iPhone simulador

Para ejecutar el sitio en un dispositivo simulado iPhone puede utilizar la extensión de WebMatrix &quot;Electric simulador Mobile para iPhone&quot;. Además, puede configurar la misma extensión para ejecutar el simulador de Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tarea 1: instalar WebMatrix 2

1. Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *WebMatrix 2*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instalar WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar WebMatrix 2")

    *Instalar WebMatrix 2*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image50.png "aceptando los términos de licencia")

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image51.png "progreso de la instalación")

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](whats-new-in-aspnet-mvc-4/_static/image52.png "se completó la instalación")

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tarea 2: instalar la extensión de simulador de iPhone

1. Ejecutar **WebMatrix** y abrir cualquier sitio Web existente o cree uno nuevo.
2. Haga clic en el **ejecutar** botón desde la **inicio** la cinta de opciones y seleccione **agregar nueva**.

    ![Agregar nueva extensión de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "agregar nueva extensión de WebMatrix")

    *Agregar nueva extensión de WebMatrix*
3. Seleccione **iPhone simulador** y haga clic en **instalar**.

    ![Examen de las extensiones de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensiones de examen de WebMatrix")

    *Exploración de las extensiones de WebMatrix*
4. En los detalles del paquete, haga clic en **instalar** para continuar con la instalación de la extensión.

    ![iPhone extensión simulador](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone extensión simulador")

    *extensión de simulador de iPhone*
5. Lea y acepte la términos de licencia de extensión.

    ![Extensión de WebMatrix CLUF](whats-new-in-aspnet-mvc-4/_static/image56.png "extensión de WebMatrix términos de licencia")

    *Extensión de WebMatrix términos de licencia*
6. Ahora, puede ejecutar el sitio Web desde WebMatrix mediante la opción de simulador de iPhone.

    ![Ejecutar con iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "ejecutar con iPhone")

    *Ejecutar con iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tarea 3: configuración de Visual Studio 2012 para ejecutar el simulador de iPhone

1. Abra **Visual Studio 2012** y abrir cualquier sitio Web o crear un nuevo proyecto.
2. Haga clic en la flecha hacia abajo en el botón de ejecución y seleccione **examinar con**.

    ![Examinar con](whats-new-in-aspnet-mvc-4/_static/image58.png "examinar con")

    *Examinar con*
3. En el &quot;explorar con&quot; cuadro de diálogo, haga clic en **agregar**.
4. En el &quot;Agregar programa&quot; cuadro de diálogo, utilice los siguientes valores:

    - **Programa**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(actualización de la ruta de acceso según corresponda)*
    - **Argumentos**: &quot;1&quot;
    - **Nombre descriptivo**: iPhone simulador

    ![Agregar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "Agregar programa")

    *Agregar programa para examinar con*
5. Haga clic en **Aceptar** y cierre los cuadros de diálogo.
6. Ahora es posible ejecutar las aplicaciones Web en el simulador iPhone desde Visual Studio 2012.

    ![Examinar con iPhone simulador](whats-new-in-aspnet-mvc-4/_static/image60.png "examinar con iPhone simulador")

    *Examinar con iPhone simulador*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice D: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](whats-new-in-aspnet-mvc-4/_static/image62.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](whats-new-in-aspnet-mvc-4/_static/image64.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](whats-new-in-aspnet-mvc-4/_static/image65.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.

    ![Descargar el sitio web de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image67.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image68.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botón.

    ![Agregar dirección IP del cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmar cambios*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-aspnet-mvc-4/_static/image73.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image74.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](whats-new-in-aspnet-mvc-4/_static/image75.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

    - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
    - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
    - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
    - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

    ![Configurar la cadena de conexión de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "configurar la cadena de conexión de destino")

    *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](whats-new-in-aspnet-mvc-4/_static/image78.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplicación se publicó en Windows Azure")

    *Aplicación que se publican en Windows Azure*
