---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Novedades de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 es un marco para crear aplicaciones web escalable y basada en estándares con los patrones de diseño bien establecido y la eficacia de ASP.NET y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8862c4da0d881a6f1084317e08697354c0ae6d48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374109"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Novedades de ASP.NET MVC 4

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 es un marco para crear aplicaciones web escalable y basada en estándares con los patrones de diseño bien establecido y la eficacia de ASP.NET y .NET framework. Esta nueva, cuarta versión de framework se centra en facilitar el desarrollo de aplicaciones web móviles.

Para empezar, cuando se crea un nuevo proyecto de ASP.NET MVC 4 ahora hay una plantilla de proyecto de aplicación móvil que puede usar para crear una aplicación independiente y específicamente para dispositivos móviles. Además, ASP.NET MVC 4 se integra con jQuery Mobile a través de un paquete de NuGet jQuery.Mobile.MVC. jQuery Mobile es un marco basado en HTML5 para desarrollar aplicaciones web que son compatibles con todas las plataformas de dispositivos móviles más populares, incluidos Windows Phone, iPhone, Android y así sucesivamente. Sin embargo, si necesita especialización, ASP.NET MVC 4 también permite atender vistas diferentes para distintos dispositivos y proporcionar las optimizaciones específicas del dispositivo.

En este laboratorio práctico, se iniciará con ASP.NET MVC 4 &quot;aplicación de Internet&quot; plantilla de proyecto para crear una aplicación de la Galería fotográfica. Progresivamente mejorará la aplicación mediante jQuery Mobile y las nuevas características de ASP.NET MVC 4 para que sea compatible con diferentes dispositivos móviles y exploradores web de escritorio. También aprenderá acerca de las recetas de código nuevo para la generación de código y cómo ASP.NET MVC 4 hace más fácil de escribir métodos de acción asincrónicos al admitir la tarea&lt;ActionResult&gt; tipos de valor devuelto.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [Novedades de formularios Web Forms en ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Aprovechar las ventajas de las mejoras en la plantillas de proyecto incluidos ASP.NET MVC la nueva plantilla de proyecto de aplicación móvil
- Use el atributo de ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejoras progresivas y para generar la interfaz de usuario web táctil optimizada
- Crear vistas específicas de dispositivos móviles
- Usar el componente de vista modificador para alternar entre las vistas en la aplicación de escritorio y móviles
- Cree controladores asincrónicos mediante la compatibilidad con la tarea

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluido en la instalación de Microsoft Visual Studio 2012)
- Emulador de Windows Phone (incluido en el [7.1.1 de Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcional: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulador** extensión (solo para el ejercicio 3 utiliza para examinar la aplicación web con un simulador de iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

En todo el documento de laboratorio, se le pedirá que inserte los bloques de código. Para su comodidad, la mayoría de ese código se proporciona como fragmentos de código de Visual Studio, puede utilizar desde dentro de Visual Studio para evitar tener que agregarla manualmente.

Para instalar los fragmentos de código:

1. Abra una ventana del explorador de Windows y vaya a la práctica **Source\Setup** carpeta.
2. Haga doble clic en el **Setup.cmd** en esta carpeta para instalar los fragmentos de código de Visual Studio.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los ejercicios siguientes:

1. [Nuevas plantillas de proyecto de ASP.NET MVC 4](#Exercise1)
2. [Creación de la aplicación Web Photo Gallery](#Exercise2)
3. [Agregar compatibilidad para dispositivos móviles](#Exercise3)
4. [Usar controladores asincrónicos](#Exercise4)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ejercicio 1: Nuevas plantillas de proyecto de ASP.NET MVC 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4. Además de la plantilla de aplicación de Internet, ya está presente en MVC 3, esta versión ahora incluye una plantilla independiente para las aplicaciones móviles. En primer lugar, se examinarán algunas características relevantes de cada una de las plantillas. A continuación, trabajará en el procesamiento de la página correctamente en las diferentes plataformas utilizando el enfoque correcto.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tarea 1: exploración de la plantilla de aplicación de Internet

1. Abra **Visual Studio**.
2. Seleccione el **archivo | Nuevo | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.

    > [!NOTE]
    > Más adelante, personalizará la solución PhotoGallery ASP.NET MVC 4 que ahora va a crear.

    ![Crear un nuevo proyecto](whats-new-in-aspnet-mvc-4/_static/image1.png "crear un nuevo proyecto")

    *Crear un nuevo proyecto*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como el motor de vista.

    ![Crear una nueva aplicación de Internet de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "creando una nueva aplicación de Internet de ASP.NET MVC 4")

    *Crear una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Se ha introducido la sintaxis de Razor en ASP.NET MVC 3. Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite un rápido y fluido de flujo de trabajo de codificación. Razor aprovecha existentes de C# / VB (u otros) habilidades lingüísticas y ofrece una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.
4. Presione **F5** para ejecutar la solución y ver las plantillas renovadas. Puede comprobar las características siguientes:

    **Plantillas de estilo moderno**

    Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.

    ![Las plantillas de ASP.NET MVC 4 se ha rediseñado](whats-new-in-aspnet-mvc-4/_static/image3.png "vuelto a diseñar plantillas de MVC 4")

    *Plantillas de ASP.NET MVC 4 se ha rediseñado*

    ![Nueva página de contacto](whats-new-in-aspnet-mvc-4/_static/image4.png "página nuevo contacto")

    *Nueva página de contacto*

    **Procesamiento adaptable**

    Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente para el nuevo tamaño de ventana. Estas plantillas usan la técnica de procesamiento adaptable se presenten correctamente en las plataformas móviles y de escritorio sin ninguna personalización.

    ![Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente](whats-new-in-aspnet-mvc-4/_static/image5.png "plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente")

    *Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente*

    **Interfaz de usuario enriquecida con JavaScript**

    Otra mejora para las plantillas de proyecto predeterminada es el uso de JavaScript para proporcionar un JavaScript más interactiva. Los vínculos de inicio de sesión y registro usados en la plantilla ejemplifican cómo usar la validaciones de jQuery para validar los campos de entrada del lado cliente.

    ![Validación de jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Validación de jQuery*

    > [!NOTE]
    > Tenga en cuenta que los dos inicie sesión en secciones, en la primera sección se puede iniciar sesión con una cuenta ha registrado desde el sitio y en la segunda sección que puede altenativelly inicie sesión con otro servicio de autenticación, como google (deshabilitado de forma predeterminada).
5. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
6. Abra el archivo **AuthConfig.cs** ubicado en el **aplicación\_iniciar** carpeta.
7. Quitar el comentario de la última línea para registrar el cliente de Google para *OAuth* autenticación.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Tenga en cuenta que puede habilitar fácilmente la autenticación mediante cualquier servicio OpenID o OAuth, como Facebook, Twitter, Microsoft, etcetera.
8. Presione **F5** para ejecutar la solución y navegar a la página de inicio de sesión.
9. Seleccione **Google** servicio para iniciar sesión.

    ![Seleccionar el registro de servicio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Seleccionar el registro de servicio*
10. Inicie sesión con su cuenta de Google.
11. Permitir al sitio (localhost) recuperar información de cuenta de Google.
12. Por último, tendrá que registrar en el sitio para asociar la cuenta de Google.

   ![Asociar su cuenta de Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Asociar su cuenta de Google*
13. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
14. Explore la solución para desproteger algunas otras características nuevas introducidas por ASP.NET MVC 4 en la plantilla de proyecto.

   ![La plantilla de proyecto de aplicación de ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

   *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

   - **HTML 5 marcado**

       Examinar vistas de la plantilla para averiguar el nuevo marcado de tema.

       ![Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml.")

       *Nueva plantilla, usando un marcado de Razor y HTML5 (About.cshtml).*
   - **Bibliotecas actualizadas de JavaScript**

       La plantilla predeterminada de ASP.NET MVC 4 ahora incluye KnockoutJS, un marco MVVM JavaScript que le permite crear enriquecidos y aplicaciones de alta capacidad de respuesta web con JavaScript y HTML. Al igual que en MVC3, jQuery y bibliotecas de interfaz de usuario de jQuery también se incluyen en ASP.NET MVC 4.

     > [!NOTE]
     > Puede obtener más información acerca de la biblioteca KnockOutJS en este vínculo: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Además, puede obtener información acerca de jQuery y jQuery UI en [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tarea 2: exploración de la plantilla de aplicación móvil

ASP.NET MVC 4 facilita el desarrollo de sitios Web para dispositivos móviles y exploradores de Tablet PC. Esta plantilla tiene la misma estructura de aplicación que la plantilla de aplicación de Internet (Observe que el código del controlador es prácticamente idéntico), pero se modificó su estilo para representar correctamente en los dispositivos móviles basados en funcionalidad táctil.

1. Seleccione el **archivo | Nuevo | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija el **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery.Mobile**, seleccione una ubicación (o deje el valor predeterminado), seleccione &quot;agregar a solución&quot; y haga clic en **Aceptar**.
2. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación Mobile** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como el motor de vista.

    ![Crear una nueva aplicación móvil de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "creando una nueva aplicación móvil de ASP.NET MVC 4")

    *Crear una nueva aplicación móvil de ASP.NET MVC 4*
3. Ahora es posible explorar la solución y consulte algunas de las nuevas características introducidas por la plantilla de solución de ASP.NET MVC 4 para dispositivos móviles:

    - **jQuery Mobile biblioteca**

        La plantilla de proyecto de aplicación de Mobile incluye la biblioteca de jQuery Mobile, que es una biblioteca de código abierto para la compatibilidad de explorador móvil. jQuery Mobile aplica la mejora progresiva a los exploradores móviles que admiten CSS y JavaScript. Mejora progresiva permite todos los exploradores mostrar el contenido básico de una página web, mientras que solo permite que los exploradores más eficaces mostrar el contenido enriquecido. Los archivos JavaScript y CSS, incluidos en el estilo de móviles, jQuery ayudan a los exploradores móviles para que quepa el contenido en la pantalla sin realizar ningún cambio en el marcado de página.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *biblioteca de jQuery móvil incluido en la plantilla*
    - **Marcado basados en HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Plantilla de aplicación móvil mediante marcado de HTML5, (Login.cshtml y index.cshtml)*
4. Presione **F5** para ejecutar la solución.
5. Abra el **Windows Phone 7 emulador**.
6. En la pantalla de inicio del teléfono, abra Internet Explorer. Compruebe la dirección URL donde se inició la aplicación de escritorio y vaya a esa dirección URL desde el teléfono (p. ej. `http://localhost:[PortNumber]/`).
7. Ahora puede realizar entran en la página de inicio de sesión o desproteger el acerca de la página. Tenga en cuenta que el estilo del sitio Web se basa en la nueva aplicación de Metro para dispositivos móviles. La plantilla de proyecto de ASP.NET MVC 4 se muestra correctamente en los dispositivos móviles, asegurándose de que todos los elementos de la página son visibles y están habilitados. Tenga en cuenta que los vínculos en el encabezado son lo suficientemente grandes para que se hace clic o pulsa.

    ![Páginas de la plantilla en un dispositivo móvil del proyecto](whats-new-in-aspnet-mvc-4/_static/image14.png "proyecto páginas de plantilla en un dispositivo móvil")

    *Páginas de la plantilla de proyecto en un dispositivo móvil*
8. La nueva plantilla también utiliza el **etiqueta meta de ventanilla**. Los exploradores móviles más definen un ancho de una ventana del explorador virtual o &quot;ventanilla&quot;, que es mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles mostrar toda la página web dentro de la pantalla virtual. El **etiqueta meta de ventanilla** permite a los desarrolladores de web establecer el ancho, alto y escalado del área del explorador en dispositivos móviles **.** La plantilla de ASP.NET MVC 4 para las aplicaciones móviles establece la ventanilla en el ancho del dispositivo (&quot;ancho = ancho del dispositivo&quot;) en la plantilla de diseño (*Views\Shared\_Layout.cshtml*), de modo que todas las páginas tendrán su ventanilla establece el ancho de pantalla del dispositivo. Tenga en cuenta que la etiqueta meta de ventanilla no cambiará la vista de explorador predeterminado.
9. Abra  **\_Layout.cshtml**, que se encuentra en la **vistas | Compartido** carpeta y un comentario de la etiqueta meta de ventanilla. Ejecute la aplicación, si no ya abierto y revise las diferencias.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![El sitio después de marcar como etiqueta meta de ventanilla](whats-new-in-aspnet-mvc-4/_static/image15.png "el sitio después de marcar como etiqueta meta de ventanilla")

*El sitio después de marcar como etiqueta meta de ventanilla*
10. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
11. Quite el comentario de la etiqueta meta de ventanilla.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tarea 3: usar procesamiento adaptable

En esta tarea, obtendrá información sobre otro método para representar una página Web correctamente en exploradores Web y dispositivos móviles al mismo tiempo sin ninguna personalización. Ya ha utilizado la etiqueta meta de ventanilla con un propósito similar. Ahora que adapta a otro método eficaces: *procesamiento adaptable*.

Procesamiento adaptable es una técnica que usa **consultas de medios CSS3** para personalizar el estilo aplicado a una página. Las consultas de medios definen condiciones dentro de una hoja de estilos, agrupar los estilos CSS en una determinada condición. Solo cuando la condición es true, el estilo se aplica a los objetos declarados.

La flexibilidad proporcionada por la técnica de procesamiento adaptable permite a cualquier personalización para mostrar el sitio en diferentes dispositivos. Puede definir tantos estilos como desee en una hoja de estilo único sin necesidad de escribir código de lógica para elegir el estilo. Por lo tanto, es una forma muy clara de la adaptación de estilos de página, ya que reduce la cantidad de código duplicado y lógica de efectos de representación. Por otro lado, podría aumentar consumo de ancho de banda, como el tamaño de los archivos CSS podría aumentar ligeramente.

Mediante la técnica de procesamiento adaptable, su sitio será **muestre correctamente, independientemente del explorador.** Sin embargo, debe considerar si el ancho de banda adicional se carga es un problema.

> [!NOTE]
> El formato básico de una consulta de medios es: @media \[ámbito: todos los | mano | imprimir | proyección | pantalla\] ([: valor de propiedad] y... [propiedad: valor])


Ejemplos de consultas de medios: &gt;  <strong>@media todas y (ancho máximo: 1000px) y (min-width: 700px) {}:</strong> para todas las resoluciones entre 700px y 1000px.

> <strong>@media pantalla y (min-width: 400px) y (ancho máximo: 700px) {...}:</strong> solo para las pantallas. La resolución debe estar entre 400 y 700px.
> 
> <strong>@media portátiles y (min-width: 20em), pantalla y (min-width: 20em) {...}:</strong> para dispositivos de mano (mobile y dispositivos) y pantallas. El ancho mínimo debe ser mayor que 20em.
> 
> Puede encontrar más información al respecto en el [sitio W3C](http://www.w3.org/TR/css3-mediaqueries/).


Ahora explorará cómo funciona el procesamiento adaptable, mejorar la legibilidad de ASP.NET MVC 4 predeterminada plantilla de sitio Web.

1. Abra el **PhotoGallery.sln** solución que ha creado en la tarea 1 y seleccione el **PhotoGallery** proyecto. Presione **F5** para ejecutar la solución.
2. Cambio de tamaño el ancho del explorador, configuración de las ventanas a la mitad o menos de un trimestre de su tamaño original. Tenga en cuenta lo que sucede con los elementos en el encabezado: algunos elementos no aparecerán en el área visible del encabezado.
3. Abra <strong>Site.css</strong> archivo desde el Explorador de soluciones de Visual Studio, ubicado en <strong>contenido</strong> carpeta del proyecto. Presione <strong>CTRL + F</strong> para abrir la búsqueda integrada en Visual Studio y escribir <strong>@media</strong> para localizar el <strong>consulta de medios CSS</strong>.

    La condición de consulta de medios definida en esta plantilla funciona de esta manera: cuando tamaño de la ventana del explorador está por debajo **850 px**, aplica las reglas de CSS son los que se definen dentro de este bloque de medios.

    ![Buscando la consulta de medios](whats-new-in-aspnet-mvc-4/_static/image16.png "buscando la consulta de medios")

    *Buscando la consulta de medios*
4. Reemplace el valor de atributo de ancho máximo establecido en 850 píxeles con **10px**, con el fin de deshabilitar el procesamiento adaptable y presione **CTRL + S** para guardar los cambios. Vuelva al explorador y presione **CTRL + F5** para actualizar la página con los cambios realizados. Tenga en cuenta las diferencias en las dos páginas al ajustar el ancho de la ventana.

    ![En el lado izquierdo, la página se aplica el @media estilo, en la derecha, el estilo se omite](whats-new-in-aspnet-mvc-4/_static/image17.png "en la izquierda, está aplicando la página el @media estilo, en la derecha, el estilo se omite")

    <em>En el lado izquierdo, la página está aplicando la @media estilo, en la derecha, el estilo se omite</em>

    Ahora, vamos a comprobar lo que sucede en los dispositivos móviles:

    ![En el lado izquierdo, la página se aplica el @media estilo, en la derecha, el estilo se omite](whats-new-in-aspnet-mvc-4/_static/image18.png "en la izquierda, está aplicando la página el @media estilo, en la derecha, el estilo se omite")

    <em>En el lado izquierdo, la página está aplicando la @media estilo, en la derecha, el estilo se omite</em>

    Aunque observará que los cambios cuando la página se representa en un explorador Web no son muy importantes, cuando se usa un dispositivo móvil las diferencias se hacen más evidentes. En el lado izquierdo de la imagen, podemos ver que el estilo personalizado mejora la legibilidad.

    Procesamiento adaptable puede usarse en muchos escenarios adicionales, facilitando la tarea aplicar estilos a un sitio Web condicional y solucionar problemas comunes de estilo de un buen enfoque.

    La etiqueta meta de ventanilla y las consultas de medios CSS no son específicas de ASP.NET MVC 4, por lo que puede aprovechar estas características en cualquier aplicación web.
5. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ejercicio 2: Creación de la aplicación Web Photo Gallery

En este ejercicio, funcionará en una aplicación de la Galería fotográfica para mostrar fotos. Se iniciará con la plantilla de proyecto de ASP.NET MVC 4 y, a continuación, agregará una función para recuperar las fotos de un servicio y mostrarlos en la página principal.

En el siguiente ejercicio, se actualizará esta solución para mejorar la forma en que se muestra en los dispositivos móviles.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tarea 1: creación de un servicio ficticio foto

En esta tarea, creará un simulacro del servicio para recuperar el contenido que se mostrará en la Galería fotográfica. Para ello, agregará un nuevo controlador que simplemente devolverá un archivo JSON con los datos de cada fotografía.

1. Abra **Visual Studio** si aún no está abierto.
2. Seleccione el **archivo | Nuevo | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.** Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**. Como alternativa, puede seguir trabajando desde su versión existente de ASP.NET MVC 4 **aplicación de Internet** solución desde **ejercicio 1** y omitir el paso siguiente.
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Asegúrese de que tiene seleccionado como el motor de vistas de Razor.
4. En el **el Explorador de soluciones**, haga clic en el **aplicación\_datos** carpeta del proyecto y seleccione **Add | Elemento existente**. Vaya a la **Source\Assets\App\_datos** carpeta de este laboratorio y agregue el **Photos.json** archivo.
5. Cree un nuevo controlador con el nombre **PhotoController**. Para ello, haga doble clic en el **controladores** carpeta, vaya a **agregar** y seleccione **controlador.** Completar el nombre del controlador, deje el **controlador de MVC vacío** plantilla y haga clic en **agregar**.

    ![Agregar el PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "agregando el PhotoController")

    *Agregar el PhotoController*
6. Reemplace el **índice** método con el siguiente **galería** acción y devolver el contenido desde el archivo JSON que se ha agregado recientemente al proyecto.

    (Código de fragmento de código - *acción de MVC de ASP.NET 4 - Ex02 - Lab galería*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Presione **F5** para ejecutar la solución y, a continuación, vaya a la siguiente dirección URL con el fin de probar el servicio de fotografías ficticios: `http://localhost:[port]/photo/gallery` (el valor de [puerto] depende en el puerto actual que inició la aplicación). La solicitud a esta dirección URL debe recuperar el contenido de la **Photos.json** archivo.

    ![Probar el servicio de fotografías ficticios](whats-new-in-aspnet-mvc-4/_static/image20.png "probar el servicio de fotografías simulada")

    *Probar el servicio de fotografías simulada*

En una implementación real podría utilizar [ASP.NET Web API](../../../../web-api/index.md) para implementar el servicio de la Galería fotográfica. ASP.NET Web API es un marco que facilita la creación de servicios HTTP que lleguen a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tarea 2: mostrar la Galería fotográfica de

En esta tarea, actualizará la página principal para mostrar la Galería fotográfica mediante el servicio ficticio que creó en la primera tarea de este ejercicio. Agregará los archivos de modelo y actualizar las vistas de galería.

1. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
2. Crear el **Photo** clase en el **modelos** carpeta. Para ello, haga doble clic en el **modelos** carpeta, seleccione **agregar** y haga clic en **clase**. A continuación, establezca el nombre en **Photo.cs** y haga clic en **agregar**.
3. Agregue los siguientes miembros para el **Photo** clase.

    (Código de fragmento de código - *modelo foto de ASP.NET MVC 4 laboratorio - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Abra el archivo **HomeController.cs** desde la carpeta **Controladores**.
5. Agregue las siguientes instrucciones de uso.

    (Código de fragmento de código - *instrucciones using de ASP.NET MVC 4 - Ex02 - Lab HomeController*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Actualización de la **índice** acción para usar **HttpClient** para recuperar los datos de la galería y, a continuación, utilice el **JavaScriptSerializer** a deserializarlo para el modelo de vista.

    (Código de fragmento de código - *acción del índice de ASP.NET MVC 4 laboratorio - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Abra el **Index.cshtml** archivo ubicado en el **Views\Home** carpeta y reemplace todo el contenido con el código siguiente.

    Este código establece bucles en todas las fotografías recuperadas del servicio y los muestra en una lista sin ordenar.

    (Código de fragmento de código - *lista de fotos de ASP.NET MVC 4 laboratorio - Ex02 -*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. En el **el Explorador de soluciones**, haga clic en el **contenido** carpeta del proyecto y seleccione **Add | Elemento existente**. Vaya a la **Source\Assets\Content** carpeta de este laboratorio y agregue el **Site.css** archivo. Tendrá que confirmar su reemplazo. Si tiene la **Site.css** el archivo abierto, tendrá que confirmar para volver a cargar el archivo también.
9. Abra el Explorador de archivos y copie todo el **fotos** carpeta situada en la **Source\Assets** carpeta de este laboratorio a la carpeta raíz del proyecto en el Explorador de soluciones.
10. Ejecute la aplicación. Ahora debería ver la página principal de mostrar las fotos en la galería.

    ![La Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image21.png "Galería fotográfica")

    *La Galería fotográfica*
11. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ejercicio 3: Agregar compatibilidad con dispositivos móviles

Una de las actualizaciones claves en ASP.NET MVC 4 es la compatibilidad para el desarrollo móvil. En este ejercicio, explorará las nuevas características de ASP.NET MVC 4 para las aplicaciones móviles mediante la extensión de la solución PhotoGallery que ha creado en el ejercicio anterior.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tarea 1: instalar jQuery Mobile en una aplicación ASP.NET MVC 4

1. Abra el **comenzar** solución ubicado en **origen/Ex3-MobileSupport/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **Package Manager Console** haciendo clic en el **herramientas** &gt; **Administrador de paquetes de biblioteca** &gt; **Administrador de paquetes Consola** opción de menú.

    ![Abra la consola de administrador de paquetes de NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "abriendo la consola de administrador de paquetes de NuGet")

    *Abra la consola del Administrador de paquetes de NuGet*
3. En la consola de administrador de paquetes, ejecute el siguiente comando para instalar el **jQuery.Mobile.MVC** paquete.

    jQuery Mobile es una biblioteca de código abierto para la creación de la interfaz de usuario web optimizadas para toque. El paquete de NuGet jQuery.Mobile.MVC incluye aplicaciones auxiliares para usar jQuery Mobile con una aplicación ASP.NET MVC 4.

    > [!NOTE]
    > Al ejecutar el comando siguiente, se descarga la biblioteca jQuery.Mobile.MVC desde Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Este comando instala jQuery Mobile y algunos archivos auxiliares, incluidos los siguientes:

    - **Views/Shared/\_Layout.Mobile.cshtml**: es un diseño de jQuery Mobile-based optimizado para una pantalla más pequeña. Cuando el sitio Web recibe una solicitud de un explorador móvil, reemplazará el diseño original (\_Layout.cshtml) por este otro.
    - Un componente de modificador de vista: consta de los **Views/Shared/\_ViewSwitcher.cshtml** vista parcial y el **ViewSwitcherController.cs** controlador. Este componente mostrará un vínculo en los exploradores móviles para permitir que los usuarios cambiar a la versión de escritorio de la página.  
        ![Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil](whats-new-in-aspnet-mvc-4/_static/image23.png "proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil")

        *Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil*
4. Registrar las agrupaciones de dispositivos móviles. Para ello, abra el **Global.asax.cs** archivo y agregue la siguiente línea.

    (Código de fragmento de código - *agrupaciones móviles de ASP.NET MVC 4 laboratorio - Ex03 - Register*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Ejecute la aplicación mediante un explorador web de escritorio.
6. Abra el **emulador de Windows Phone 7,** ubicado en **menú Inicio | Todos los programas | Windows Phone SDK 7.1 | Emulador de Windows Phone.**
7. En la pantalla de inicio del teléfono, abra Internet Explorer. Compruebe la dirección URL donde se inicia la aplicación y vaya a esa dirección URL con el Explorador de teléfono (p. ej. `http://localhost:[PortNumber]/`).

    Observará que la aplicación tendrá un aspecto diferente en el emulador de Windows Phone, como el jQuery.Mobile.MVC ha creado nuevos recursos en el proyecto que se muestran vistas optimizadas para dispositivos móviles.

    Observe el mensaje en la parte superior del teléfono, que muestra el vínculo que se activa en la vista de escritorio. Además, el  **\_Layout.Mobile.cshtml** diseño que se creó el paquete ha instalado es incluir un diseño diferente en la aplicación.

    > [!NOTE]
    > Hasta ahora, no hay ningún vínculo para volver a la vista móvil. Se incluirá en versiones posteriores.

    ![Vista móvil de la página principal de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image24.png "vista móvil de la página principal de la Galería fotográfica")

    *Vista móvil de la página principal de la Galería fotográfica*
8. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tarea 2: creación de vistas móviles

En esta tarea, creará una versión móvil de la vista de índice con el contenido que se ha adaptado a appareance mejor en dispositivos móviles.

1. Copia el **Views\Home\Index.cshtml** ver y péguelo en la creación de una copia, cambie el nombre del nuevo archivo a **Index.Mobile.cshtml**.
2. Abra el nuevo creó **Index.Mobile.cshtml** ver y reemplazar el existente &lt;ul&gt; etiqueta con este código. Al hacerlo, se va a actualizar el &lt;ul&gt; etiqueta con las anotaciones de datos móvil de jQuery para usar los temas de jQuery mobile.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Tenga en cuenta que:
    > 
    > - El **datos rol** atributo establecido en **listview** representará la lista con los estilos de listview.
    > 
    > - El **datos bajorrelieve** atributo establecido en true mostrará la lista con las esquinas del borde y margen.
    > 
    > - El **datos filtro** atributo establecido en **true** generará un cuadro de búsqueda.
    > 
    > Puede aprender más sobre las convenciones de jQuery Mobile en la documentación del proyecto: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Presione **CTRL + S** para guardar los cambios.
4. Cambie a la **emulador de Windows Phone** y actualizar el sitio. Tenga en cuenta la nueva apariencia de la lista de la galería, así como el nuevo cuadro de búsqueda que se encuentra en la parte superior. A continuación, escriba una palabra en el cuadro de búsqueda (por ejemplo, **Tulips**) para iniciar una búsqueda en la Galería fotográfica.

    ![Galería con estilo de listview con filtrado](whats-new-in-aspnet-mvc-4/_static/image25.png "utilizando el estilo de listview con el filtrado de la Galería")

    *Galería con estilo de listview con filtrado*

    En resumen, ha usado la receta Mobilizer de vista para crear una copia de la vista de índice con el &quot;móvil&quot; sufijo. Este sufijo se indica a ASP.NET MVC 4 que cada solicitud que se genera a partir de un dispositivo móvil utilizará esta copia del índice. Además, ha actualizado la versión móvil de la vista de índice que usa jQuery Mobile para mejorar la apariencia y comportamiento de sitio en los dispositivos móviles.
5. Vuelva a Visual Studio y abra **Site.Mobile.css** ubicado en el **contenido** carpeta.
6. Corregir la posición del título para que aparezca en el lado derecho de la imagen de foto. Para ello, agregue el código siguiente a la **Site.Mobile.css** archivo.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Presione **CTRL + S** para guardar los cambios.
8. Vuelva a la **emulador de Windows Phone** y actualizar el sitio. Tenga en cuenta que el título de la foto está posicionado apropiadamente ahora.

    ![Título que se coloca en el lado derecho de la imagen](whats-new-in-aspnet-mvc-4/_static/image26.png "título se coloca en el lado derecho de la imagen")

    *Título que se coloca en el lado derecho de la imagen*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tarea 3: temas de jQuery Mobile

Cada diseño y widgets en jQuery Mobile está diseñada en torno a un nuevo marco CSS orientada a objetos que permite aplicar un tema de completar el diseño visual unificada a los sitios y aplicaciones.

Temas predeterminados de jQuery Mobile incluyen 5 muestras que se asignan letras (a, b, c y d, e) como referencia rápida.

En esta tarea, actualizará el diseño móvil para utilizar un tema diferente que el valor predeterminado.

1. Cambie a Visual Studio.
2. Abra el  **\_Layout.Mobile.cshtml** archivo se encuentra en **Views\Shared**.
3. Busque el elemento div con el rol de datos establecido en &quot;página&quot; y actualizar la **data-theme** a &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Presione **CTRL + S** para guardar los cambios.
5. Actualizar el sitio en el **emulador de Windows Phone** y observe la nueva combinación de colores.

    ![Diseño para dispositivos móviles con una combinación de colores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "diseño móvil con una combinación de colores diferentes")

    *Diseño para dispositivos móviles con una combinación de colores diferentes*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tarea 4: mediante el componente de modificador de vista y el Explorador de invalidación de características

Es una convención para las páginas web optimizada para móviles agregar un vínculo cuyo texto es algo similar a la vista de escritorio o en modo de sitio completa que permite a los usuarios cambiar a una versión de escritorio de la página. Incluye un ejemplo en el paquete jQuery.Mobile.MVC **modificador de vista** componente para este propósito que se usan en el  **\_Layout.Mobile.cshtml** vista.

![Vínculo para cambiar a vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image28.png "vínculo para cambiar a vista de escritorio")

*Use el vínculo para cambiar a vista de escritorio*

El modificador de vista utiliza una nueva característica denominada **explorador reemplazar**. Esta característica permite que la aplicación trate las solicitudes como si provinieran de un explorador distinto (agente de usuario) a la que proceden de.

En esta tarea, explorará la implementación de ejemplo de un modificador de vista agregado por jQuery.Mobile.MVC y el nuevo explorador reemplazar las características de ASP.NET MVC 4.

1. Cambie a Visual Studio.
2. Abra el  **\_Layout.Mobile.cshtml** vista ubicado en el **Views\Shared** carpeta y observe el componente de modificador de vista que se hace referencia como una vista parcial.

    ![Diseño para dispositivos móviles mediante el componente de modificador de vista](whats-new-in-aspnet-mvc-4/_static/image29.png "diseño para dispositivos móviles mediante el componente de modificador de vista")

    *Diseño para dispositivos móviles mediante el componente de modificador de vista*
3. Abra el  **\_ViewSwitcher.cshtml** vista parcial.

    La vista parcial utiliza el nuevo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar el origen de la solicitud web y mostrar el vínculo correspondiente para pasar a las vistas de escritorio o móviles.

    El **GetOverridenBrowser** método devuelve un **HttpBrowserCapabilitiesBase** instancia que se corresponde con el agente de usuario establecido actualmente para la solicitud (real o reemplazada). Puede usar este valor para obtener las propiedades como **IsMobileDevice**.

    ![Vista parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher de vista parcial")

    *Vista parcial ViewSwitcher*
4. Abra el **ViewSwitcherController.cs** clase ubicado en el **controladores** carpeta. Comprobación de esa acción SwitchView se llama mediante el vínculo en el componente ViewSwitcher y observe los nuevos métodos de HttpContext.

    - El **HttpContext.ClearOverridenBrowser()** método quita cualquier agente de usuario invalidado para la solicitud actual.
    - El **HttpContext.SetOverridenBrowser()** método invalida el valor de agente de usuario real de la solicitud mediante el agente de usuario especificado.  
        ![Controlador ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")  
*Controlador ViewSwitcher*

        Invalidación de explorador es una característica principal de ASP.NET MVC 4, que también está disponible incluso si no instala el paquete jQuery.Mobile.MVC. Sin embargo, esta característica afecta solo la vista Diseño y vista parcial, y no afecta a ninguna de las características que dependen del objeto Request.Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tarea 5: agregar al modificador de vista en la vista de escritorio

En esta tarea, actualizará el diseño de escritorio para incluir al modificador de vista. Esto permitirá que los usuarios móviles volver a la vista móvil al examinar la vista de escritorio.

1. Actualizar el sitio en el **emulador de Windows Phone**.
2. Haga clic en el **vista de escritorio** vínculo en la parte superior de la galería. Tenga en cuenta que no hay ningún modificador de vista en la vista de escritorio para permitir que vuelva a la vista móvil.
3. Vuelva a Visual Studio y abra el  **\_Layout.cshtml** vista.
4. Busque la sección de inicio de sesión e inserte una llamada para representar el  **\_ViewSwitcher** vista parcial siguiente el  **\_LogOnPartial** vista parcial. A continuación, presione **CTRL + S** para guardar los cambios.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Presione **CTRL + S** para guardar los cambios.
6. Actualice la página en el emulador de Windows Phone y haga doble clic en la pantalla para acercar. Tenga en cuenta que ahora muestra la página principal de la **vista móvil** vínculo que se activa desde el móvil en vista de escritorio.

    ![Ver modificador representado en la vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image32.png "modificador de vista que se representan en la vista de escritorio")

    *Modificador de vista que se representan en la vista de escritorio*
7. Cambie a la vista móvil nuevo y vaya a <strong>sobre</strong> página (http://localhost[puerto] / Home/acerca de). Tenga en cuenta que, incluso si no ha creado una vista About.Mobile.cshtml, la página About se muestra con el diseño móvil (\_Layout.Mobile.cshtml).

    ![Acerca de la página](whats-new-in-aspnet-mvc-4/_static/image33.png "acerca de la página")

    *Acerca de la página*
8. Por último, abra el sitio en un explorador Web de escritorio. Tenga en cuenta que ninguna de las actualizaciones anteriores ha afectado a la vista de escritorio.

    ![Vista de escritorio PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery vista del escritorio")

    *Vista de escritorio PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tarea 6: creación de nuevos modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que se va a generar la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página principal, la aplicación devolverá el **Views\Home\Index.cshtml** plantilla. A continuación, si un explorador móvil solicita la página principal, la aplicación devolverá el **Views\Home\Index.mobile.cshtml** plantilla.

En esta tarea, creará un diseño personalizado para los dispositivos iPhone y tendrá que simulan las solicitudes de los dispositivos iPhone. Para ello, puede usar ya sea un emulador o simulador de iPhone (como [Electric simulador Mobile](http://www.electricplum.com/)) o un explorador con los complementos que modifican el agente de usuario. Para que obtener instrucciones sobre cómo establecer la cadena de agente de usuario en un explorador Safari emular un iPhone, consulte [cómo permitir que Safari simule que es el IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) en el blog de David Alison.

**Tenga en cuenta que esta tarea es opcional y puede continuar en todo el laboratorio sin ejecutarla.**

1. En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.
2. Abra **Global.asax.cs** y agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Agregue el siguiente código resaltado en la aplicación\_Start (método).

    (Código de fragmento de código - *ASP.NET MVC 4 laboratorio - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Se ha registrado un nuevo **DefaultDisplayMode denominado &quot;iPhone&quot;**, dentro de estático **DisplayModeProvider.Instance.Modes** lista estática, que se comparará con cada solicitud entrante. Si la solicitud entrante contiene la cadena &quot;iPhone&quot;, ASP.NET MVC buscará las vistas cuyo nombre contenga la &quot;iPhone&quot; sufijo. El parámetro 0 indica cómo específico es el nuevo modo; Por ejemplo, esta vista es más específica que general &quot;.mobile&quot; regla que coincida con las solicitudes de dispositivos móviles.

Después de ejecutar este código, cuando un explorador de iPhone genera una solicitud, la aplicación usará el **Views\Shared\\_Layout.iPhone.cshtml** diseño que se va a crear en los pasos siguientes.

> [!NOTE]
> Esta forma de probar la solicitud para el iPhone se ha simplificado con fines de demostración y podría no funcionar según lo previsto para cada cadena de agente de usuario de iPhone (para pruebas de ejemplo distingue mayúsculas de minúsculas).

4. Crear una copia de la  **\_Layout.Mobile.cshtml** de archivos en el **Views\Shared** carpeta y cambie el nombre de la copia a &quot; **\_Layout.iPhone.csthml**&quot;.
5. Abra  **\_Layout.iPhone.csthml** que creó en el paso anterior.
6. Busque el elemento div con el atributo de rol de datos establecido en **página** y cambie el **data-theme** atributo &quot; **un**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Ahora tiene 3 diseños en la aplicación de ASP.NET MVC 4:

1. **\_Layout.cshtml**: diseño predeterminado utilizado para los exploradores de escritorio.
2. **\_Layout.Mobile.cshtml**: diseño predeterminado utilizado para dispositivos móviles.
3. **\_Layout.iPhone.cshtml**: diseño específico para los dispositivos iPhone, mediante una combinación de colores diferentes para diferenciar de \_Layout.mobile.cshtml.
7. Presione **F5** para ejecutar la aplicación y examinar el sitio en el **emulador de Windows Phone**.
8. Abra un **simulador de iPhone** (consulte [Apéndice C](#AppendixC) para obtener instrucciones sobre cómo instalar y configurar un simulador de iPhone) y vaya al sitio demasiado. Tenga en cuenta que cada teléfono está utilizando la plantilla específica.

    ![Using-Different-Views-for-Each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Uso de vistas diferentes para cada dispositivo móvil*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ejercicio 4: Usar controladores asincrónicos

Microsoft .NET Framework 4.5 presenta nuevas características del lenguaje en C# y Visual Basic para proporcionar una nueva base para asincronía en programación. NET. Esta nueva base hace que la programación asincrónica similar a - y unos tan sencillo como - programación sincrónica. Ahora es posible escribir métodos de acción asincrónicos en ASP.NET MVC 4 mediante la **AsyncController** clase. Puede utilizar métodos de acción asincrónicos de ejecución prolongada, las solicitudes relacionadas con la CPU no. Esto evita que el servidor Web de realizar el trabajo mientras se procesa la solicitud de bloqueo. La clase AsyncController normalmente se usa para llamadas de servicio Web de ejecución prolongada.

Este ejercicio explica los conceptos básicos de la operación asincrónica en ASP.NET MVC 4. Si desea un análisis más profundo, puede consultar el artículo siguiente: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tarea 1: implementar un controlador asincrónico

1. Abra el **comenzar** solución ubicado en **origen/Ex4-Async/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **HomeController.cs** clase desde el **controladores** carpeta.
3. Agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Actualización de la **HomeController** herede de **AsyncController**. Controladores que derivan de AsyncController permiten a ASP.NET procesar solicitudes asincrónicas y pueden los métodos de acción sincrónicos de servicio.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Agregar el **async** palabra clave para el **índice** método y conviértalo en el tipo de valor devuelto **tarea&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > El **async** palabra clave es una de las nuevas palabras clave proporciona .NET Framework 4.5; indica al compilador que este método contiene el código asincrónico. Un **tarea** objeto representa una operación asincrónica que puede completar en algún momento en el futuro.
6. Reemplace el **cliente. GetAsync()** llamada con la versión completa async mediante palabra clave await, como se muestra a continuación.

    (Código de fragmento de código - *GetAsync de ASP.NET MVC 4 laboratorio - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > En la versión anterior, usaba la **resultado** propiedad desde la **tarea** objeto para bloquear el subproceso hasta que se devuelve el resultado (versión de sincronización).
    > 
    > Agregar el **await** palabra clave indica al compilador que esperar la tarea devuelta desde la llamada al método de manera asincrónica. Esto significa que el resto del código se ejecutará como una devolución de llamada después de completarse el método esperado. Otra cosa a tener en cuenta es que no es necesario cambiar el bloque try-catch para llevarlo a cabo: sin ningún trabajo adicional con un controlador proporcionado por el marco de trabajo todavía se detectarán las excepciones que se producen en segundo plano o en primer plano.
7. Cambiar el código para continuar con la implementación asincrónica reemplazando las líneas con el nuevo código, tal como se muestra a continuación

    (Código de fragmento de código - *ASP.NET MVC 4 laboratorio - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Ejecute la aplicación. Observará que no hay cambios importantes, pero el código no bloquea un subproceso del grupo de subprocesos que hace un mejor uso de los recursos del servidor y mejorar el rendimiento.

    > [!NOTE]
    > Puede aprender más sobre las nuevas características de programación asincrónicas en el laboratorio &quot; **programación asincrónica en .NET Framework 4.5 con C# y Visual Basic** &quot; incluido en el Kit de aprendizaje de Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tarea 2: control de los tiempos de espera con Tokens de cancelación

Métodos de acción asincrónicos que devuelven instancias de tarea también pueden admitir los tiempos de espera. En esta tarea, actualizará el código del método de índice para controlar un escenario de tiempo de espera utilizando un token de cancelación.

1. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.
2. Agregue la siguiente instrucción using a la **HomeController.cs** archivo.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Actualizar la acción del índice para recibir un **CancellationToken** argumento.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Actualización de la **GetAsync** llamada para pasar el token de cancelación.

    (Código de fragmento de código - *SendAsync de laboratorio - Ex04 - ASP.NET MVC 4 con CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decorar el *índice* método con un **AsyncTimeout** atributo establecido en 500 milisegundos y un **HandleError** configurado para controlar el atributo  **TaskCanceledException** redirigiendo a una **TimedOut** vista.

    (Código de fragmento de código - *atributos de ASP.NET MVC 4 laboratorio - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Abra el **PhotoController** clase y actualización de la **galería** método para retrasar la milisegundos ejecución 1000 (1 segundo) para simular una tarea de ejecución prolongada.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Abra el **Web.config** de archivos y habilitar los errores personalizados agregando el siguiente elemento.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Crear una nueva vista en **Views\Shared** denominado **TimedOut** y usar el diseño predeterminado. En el Explorador de soluciones, haga clic en el **Views\Shared** carpeta y seleccione **Add | Vista**.

    ![Uso de vistas diferentes para cada dispositivo móvil](whats-new-in-aspnet-mvc-4/_static/image36.png "con vistas diferentes para cada dispositivo móvil")

    *Uso de vistas diferentes para cada dispositivo móvil*
9. Actualización de la **TimedOut** ver contenido tal como se muestra a continuación.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Ejecute la aplicación y vaya a la dirección URL raíz. Como ha agregado un **Thread.Sleep** de 1000 milisegundos, obtendrá un error de tiempo de espera, generado por el **AsyncTimeout** de atributo y detecte mediante el **HandleError** atributo.

    ![Excepción de tiempo de espera controlado](whats-new-in-aspnet-mvc-4/_static/image37.png "excepción de tiempo de espera controlado")

    *Excepción de tiempo de espera controlado*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [apéndice D: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio de prácticas, ha observado algunas de las nuevas características residente en ASP.NET MVC 4. Se han tratado los conceptos siguientes:

- Aprovechar las ventajas de las mejoras en la plantillas de proyecto incluidos ASP.NET MVC la nueva plantilla de proyecto de aplicación móvil
- Use el atributo de ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejoras progresivas y para generar la interfaz de usuario web táctil optimizada
- Crear vistas específicas de dispositivos móviles
- Usar el componente de vista modificador para alternar entre las vistas en la aplicación de escritorio y móviles
- Cree controladores asincrónicos mediante la compatibilidad con la tarea

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-aspnet-mvc-4/_static/image38.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](whats-new-in-aspnet-mvc-4/_static/image40.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](whats-new-in-aspnet-mvc-4/_static/image41.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)***

1. Haga doble clic en el desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
3. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](whats-new-in-aspnet-mvc-4/_static/image42.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](whats-new-in-aspnet-mvc-4/_static/image43.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalar Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express para el icono de Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Apéndice C: instalar WebMatrix 2 y simulador de iPhone

Para ejecutar el sitio en un dispositivo simulado iPhone puede usar la extensión de WebMatrix &quot;Electric simulador Mobile para iPhone&quot;. Además, puede configurar la misma extensión para ejecutar el simulador de Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tarea 1: instalar WebMatrix 2

1. Vaya a [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>WebMatrix 2</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instalar WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar WebMatrix 2")

    *Instalar WebMatrix 2*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image50.png "aceptando los términos de licencia")

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image51.png "progreso de la instalación")

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![La instalación se completó](whats-new-in-aspnet-mvc-4/_static/image52.png "completada la instalación")

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tarea 2: instalar la extensión del simulador de iPhone

1. Ejecute **WebMatrix** y abrir cualquier sitio Web existente o cree uno nuevo.
2. Haga clic en el **ejecutar** botón desde la **inicio** la cinta de opciones y seleccione **Agregar nuevo**.

    ![Agregar nueva extensión de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "agregar nueva extensión de WebMatrix")

    *Agregar nueva extensión de WebMatrix*
3. Seleccione **iPhone simulador** y haga clic en **instalar**.

    ![Exploración de las extensiones de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "WebMatrix examinar extensiones")

    *Exploración de las extensiones de WebMatrix*
4. En los detalles del paquete, haga clic en **instalar** para continuar con la instalación de la extensión.

    ![extensión del simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensión del simulador de iPhone")

    *extensión del simulador de iPhone*
5. Lea y acepte la términos de licencia de extensión.

    ![Extensión de WebMatrix CLUF](whats-new-in-aspnet-mvc-4/_static/image56.png "términos de licencia de extensión de WebMatrix")

    *Términos de licencia de extensión de WebMatrix*
6. Ahora, puede ejecutar su sitio Web desde WebMatrix mediante la opción de simulador de iPhone.

    ![Ejecutar con iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "ejecutar con iPhone")

    *Ejecutar con iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tarea 3: configurar Visual Studio 2012 para ejecutar el simulador de iPhone

1. Abra **Visual Studio 2012** y abrir cualquier sitio Web o crear un nuevo proyecto.
2. Haga clic en la flecha hacia abajo en el botón de ejecución y seleccione **examinar con**.

    ![Examinar con](whats-new-in-aspnet-mvc-4/_static/image58.png "explorar con")

    *Explorar con*
3. En el &quot;explorar con&quot; cuadro de diálogo, haga clic en **agregar**.
4. En el &quot;Agregar programa&quot; cuadro de diálogo, use los siguientes valores:

   - <strong>Programa</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (actualización de la ruta de acceso según corresponda)</em>
   - **Argumentos**: &quot;1&quot;
   - **Nombre descriptivo**: simulador de iPhone

     ![Agregar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "Agregar programa")

     *Agregar programa para buscar*
5. Haga clic en **Aceptar** y cierre los cuadros de diálogo.
6. Ahora es posible ejecutar las aplicaciones Web en el simulador de iPhone desde Visual Studio 2012.

    ![Examinar con el simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "examinar con el simulador de iPhone")

    *Examinar con el simulador de iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice D: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy

En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web de Windows Azure Portal

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure de Windows*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](whats-new-in-aspnet-mvc-4/_static/image62.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en el sitio Web Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![Exploración al sitio web nuevo](whats-new-in-aspnet-mvc-4/_static/image64.png "exploración al sitio web nuevo")

    *Exploración al sitio web nuevo*

    ![Sitio Web que se ejecuta](whats-new-in-aspnet-mvc-4/_static/image65.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web para sitios Web de Windows Azure.

    ![Descargando el sitio web de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image67.png "descargando el sitio web de perfil de publicación")

    *Descargando el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image68.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas. No cree la base de datos, como se crearán en una etapa posterior.

    ![Panel de base de datos SQL Server](whats-new-in-aspnet-mvc-4/_static/image69.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botón.

    ![Agregar dirección IP del cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmar cambios*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-aspnet-mvc-4/_static/image73.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image74.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](whats-new-in-aspnet-mvc-4/_static/image75.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de implementación Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuración de implementación Web")

    *Configuración de implementación Web*
5. Configure la conexión de base de datos como sigue:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.

    ![Creación de la base de datos](whats-new-in-aspnet-mvc-4/_static/image78.png "creación de la cadena de la base de datos")

    *Creación de la base de datos*
7. La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **Preview** página, haga clic en **publicar**.

    ![Publicar la aplicación web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "publica la aplicación en Windows Azure")

    *Publicada aplicación en Windows Azure*
