---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Crear un diseño de todo el sitio mediante páginas maestras (VB) | Documentos de Microsoft
author: rick-anderson
description: Este tutorial le mostrará los conceptos básicos de la página maestra. Es decir, ¿cuáles son las páginas maestras, ¿cómo uno crear una página maestra, ¿cuáles son los marcadores de posición de contenido, ¿cómo un cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d18993af7159de552db0c622fbef58e814e36ebb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Crear un diseño de todo el sitio mediante páginas maestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Este tutorial le mostrará los conceptos básicos de la página maestra. Es decir, ¿cuáles son las páginas maestras, cómo se crea una página maestra, ¿cuáles son los marcadores de posición de contenido, cómo se crea una página ASP.NET que utiliza una página maestra, cómo modificar la página maestra se refleja automáticamente en sus páginas de contenido asociadas y así sucesivamente.


## <a name="introduction"></a>Introducción

Un atributo de un sitio Web bien diseñado es un diseño de página coherente de todo el sitio. Tomemos como ejemplo el sitio Web www.asp.net. En el momento de redactar este artículo, cada página tiene el mismo contenido en la parte superior e inferior de la página. Como se muestra en la figura 1, la parte superior de cada página muestra una barra gris con una lista de Microsoft Communities. Debajo de ese es el logotipo del sitio, la lista de idiomas en la que se ha traducido el sitio y en las secciones principales: inicio, introducción, aprendizaje, descargas y así sucesivamente. Del mismo modo, la parte inferior de la página incluye información acerca de la publicidad en www.asp.net, una declaración de copyright y un vínculo a la declaración de privacidad.


[![El sitio Web www.asp.net emplea una apariencia coherente en todas las páginas](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Figura 01</strong>: el sitio Web www.asp.net emplea un aspecto coherente y sentirse a través de todas las páginas ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Otro atributo de un sitio bien diseñado es la facilidad con la que se puede cambiar la apariencia del sitio. La figura 1 muestra la página principal de www.asp.net a partir de marzo de 2008, pero entre ahora y publicación de este tutorial, puede haber cambiado la apariencia y comportamiento. Quizás los elementos de menú en la parte superior se expandirán para incluir una sección nueva para el marco de MVC. O quizás será dio un diseño radicalmente nuevo con distintos colores, fuentes y diseño. Aplicar dichos cambios a todo el sitio debe ser un proceso rápido y simple que no se requiere la modificación de las miles de páginas web que componen el sitio.

Crear una plantilla de página de todo el sitio en ASP.NET es posible mediante el uso de *páginas maestras*. En pocas palabras, una página maestra es un tipo especial de página ASP.NET que define el marcado que es común para todos los *páginas de contenido* , así como áreas que son personalizables según una página de contenido a página contenido. (Una página de contenido es una página ASP.NET que se enlaza a la página maestra). Cuando se cambia el diseño o el formato de una página maestra, todos los resultados de sus páginas contenido es igualmente actualiza de forma inmediata, que facilita la aplicación de cambios de apariencia en todo el sitio tan fáciles como actualización e implementación de un solo archivo (es decir, la página maestra).

Este es el primer tutorial de una serie de tutoriales que explican cómo utilizar las páginas maestras. En el transcurso de esta serie de tutoriales es:

- Examine la creación de páginas maestras y sus páginas de contenido asociadas
- Describe una variedad de sugerencias, trucos y capturas,
- Identificar los errores de página maestra comunes y explorar las soluciones alternativas,
- Vea cómo obtener acceso a la página maestra de una página de contenido y un viceversa,
- Obtenga información acerca de cómo especificar la página maestra de una página de contenido en tiempo de ejecución, y
- Otros temas de página maestra avanzados.

Estos tutoriales están pensados para ser conciso y se proporcionan instrucciones paso a paso con una gran cantidad de capturas de pantalla para guiarle a través del proceso visualmente. Cada tutorial está disponible en versiones de Visual Basic y C# e incluye la descarga de código completo usado.

Este tutorial inaugural comienza con un vistazo a los conceptos básicos de la página maestra. Describe cómo funcionan las páginas maestras, cómo crear una página maestra y las páginas de contenido asociadas con Visual Web Developer y vea cómo los cambios realizados en una página maestra se reflejan inmediatamente en las páginas de contenido. Comencemos.

## <a name="understanding-how-master-pages-work"></a>Descripción de cómo funcionan las páginas maestras

Creación de un sitio Web con un diseño de página de todo el sitio coherente requiere que cada página web emitir marcado formato común además de su contenido personalizado. Por ejemplo, mientras que cada entrada de tutorial o foro en www.asp.net tiene su propio contenido único, cada una de estas páginas también representar una serie de común `<div>` elementos que se muestran los vínculos de la sección de nivel superior: Home, introducción, obtenga más información y así sucesivamente.

Hay una variedad de técnicas para crear páginas web con una apariencia coherente. Un enfoque naïve es simplemente copie y pegue el marcado de diseño comunes en todas las páginas web, pero este enfoque tiene varias desventajas. Para empezar, cada vez que se crea una nueva página, debe olvidar copie y pegue el contenido compartido en la página. Como copiar y pegar las operaciones son errores de madurez accidentalmente, puede copiar solo un subconjunto de las marcas compartidas en una nueva página. Y para completarlo, este enfoque permite reemplazar la apariencia de todo el sitio existente por uno nuevo, un número real dolor porque se debe editar cada página única en el sitio para poder usar la nueva apariencia y funcionamiento.

Antes de la versión 2.0 de ASP.NET, página a los desarrolladores a menudo se colocan marcado comunes en [controles de usuario](https://msdn.microsoft.com/library/y6wb1a0e.aspx) y, a continuación, agregar estos controles de usuario para cada página. Este enfoque requiere que el desarrollador de páginas no olvide agregar manualmente los controles de usuario a cada nueva página, pero permite las modificaciones de todo el sitio sea más fácil porque al actualizar el marcado comunes solo los controles de usuario deba modificarse. Por desgracia, Visual Studio .NET 2002 y 2003: las versiones de Visual Studio usa para crear aplicaciones de ASP.NET 1.x - representadas controles de usuario en la vista Diseño como cuadros grises. Por lo tanto, los desarrolladores de páginas con este enfoque no disfrute de un entorno de tiempo de diseño WYSIWYG.

Los inconvenientes del uso de controles de usuario se han trabajado en la versión 2.0 de ASP.NET y Visual Studio 2005 con la introducción de *páginas maestras*. Una página maestra es un tipo especial de página ASP.NET que define tanto el marcado de todo el sitio y la *regiones* donde asociados *páginas de contenido* definir su marcado personalizada. Como se verá en el paso 1, estas regiones se definen mediante controles ContentPlaceHolder. El control ContentPlaceHolder simplemente indica una posición en la jerarquía de control de la página maestra donde se puede insertar el contenido personalizado mediante una página de contenido.

> [!NOTE]
> Los conceptos básicos y la funcionalidad de las páginas maestras no ha cambiado desde la versión 2.0 de ASP.NET. Sin embargo, Visual Studio 2008 ofrece compatibilidad en tiempo de diseño para las páginas maestras anidadas, una característica que faltaba en Visual Studio 2005. Veremos usar páginas maestras anidadas en un tutorial posterior.


La figura 2 muestra la página maestra para www.asp.net quedará. Tenga en cuenta que la página maestra define el diseño de todo el sitio comunes - el marcado en la parte superior, inferior y derecho de cada página -, así como un control ContentPlaceHolder en la parte central izquierda, donde se ubica el contenido único para cada página web.


![Una página maestra define el diseño de todo el sitio y las regiones modificables en forma de página de contenido a página contenido](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Figura 02**: una página maestra define el diseño de todo el sitio y las regiones modificables en forma de página de contenido a página contenido


Una vez que se ha definido una página maestra pueden estar limitado a las nuevas páginas ASP.NET a través de la marca de verificación de un control checkbox. Estas páginas ASP.NET - denominadas páginas de contenido: incluyen un control de contenido para cada uno de los controles de la página maestra ContentPlaceHolder. Cuando se visita la página de contenido a través de un explorador el motor ASP.NET crea la jerarquía de control de la página maestra e inserta la jerarquía de control de la página de contenido en los lugares adecuados. Se representa esta jerarquía de control combinado y el HTML resultante se devuelve al explorador del usuario final. Por lo tanto, en la página de contenido se emite el marcado comunes definidos en la página maestra fuera de los controles ContentPlaceHolder y el marcado específico de la página definido dentro de sus propios controles de contenido. Figura 3 ilustra este concepto.


[![Marcado de la página solicitada está fusionado en la página principal](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Figura 03**: marcado de la página solicitada está fusionado en la página maestra ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Ahora que ya hemos analizado cómo funcionan las páginas maestras, echemos un vistazo a la creación de una página maestra y las páginas de contenido asociadas con Visual Web Developer.

> [!NOTE]
> Para llegar a la audiencia más amplia posible, el sitio Web ASP.NET compilamos a lo largo de esta serie de tutoriales se creará con ASP.NET 3.5 versión gratuita de Microsoft de Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Si aún no ha actualizado a ASP.NET 3.5, no se preocupe: los conceptos tratados en estos tutoriales de trabajo igualmente bien con ASP.NET 2.0 y Visual Studio 2005. Sin embargo, algunas aplicaciones de demostración pueden utilizar las características nuevas de .NET Framework versión 3.5; Cuando se utilizan características específicas de 3.5, incluir una nota que explica cómo implementar una funcionalidad similar en la versión 2.0. Tenga en cuenta que las aplicaciones de demostración disponibles para descargar desde cada tutorial destino .NET Framework versión 3.5, que da como resultado un `Web.config` archivo que incluye elementos de configuración específicos de 3.5. En resumen, si todavía tiene que instalar .NET 3.5 en el equipo, a continuación, la aplicación web que se pueden descargar no funcionará sin quitar primero el marcado específico del 3.5 desde `Web.config`. Vea [del Diseccionando ASP.NET versión 3.5 `Web.config` archivo](http://www.4guysfromrolla.com/articles/121207-1.aspx) para obtener más información acerca de este tema.


## <a name="step-1-creating-a-master-page"></a>Paso 1: Crear una página maestra

Antes de que podemos explorar crear y usar páginas maestras y de contenido, primero es necesario un sitio Web ASP.NET. Comience creando un nuevo archivo basado en sistema sitio Web de ASP.NET. Para ello, inicie Visual Web Developer y, a continuación, vaya al menú archivo y elija Nuevo sitio Web, mostrar el cuadro de diálogo nuevo sitio Web cuadro (consulte la figura 4). Elija la plantilla de sitio Web ASP.NET, o establecer la lista de la lista desplegable de ubicación al sistema de archivos, elija una carpeta para colocar el sitio web y establecer el idioma de Visual Basic. Esto creará un nuevo sitio web con un `Default.aspx` página ASP.NET, un `App_Data` carpeta y un `Web.config` archivo.

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios Web y proyectos de aplicación Web. Proyectos de sitios Web no tienen un archivo de proyecto, mientras que los proyectos de aplicación Web imitar la arquitectura de proyecto en Visual Studio .NET 2002/2003, que incluyen un archivo de proyecto y compilan código fuente del proyecto en un único ensamblado, que se coloca en el `/bin` carpeta. Visual Studio 2005 inicialmente solo compatibles proyectos de sitio Web, aunque el modelo de proyecto de aplicación Web se vuelve a producirse con Service Pack 1; Visual Studio 2008 ofrece dos modelos de proyecto. Visual Web Developer 2005 y 2008 ediciones, sin embargo, solo admiten proyectos de sitios Web. Usar el modelo de proyecto de sitio Web para mi demostraciones en esta serie de tutoriales. Si está usando una edición no sean Express y desea utilizar el [modelo de proyecto de aplicación Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) en su lugar, no dude en hacerlo, pero tenga en cuenta que puede haber algunas discrepancias entre lo que ve en la pantalla y los pasos que debe seguir en comparación con el capturas de pantalla se muestra y las instrucciones proporcionadas en estos tutoriales.


[![Crear un nuevo archivo basado en sistema sitio Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Figura 04**: crear un sitio Web de New File System-Based ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


A continuación, agregue una página maestra al sitio en el directorio raíz con el botón secundario en el nombre del proyecto, elija Agregar nuevo elemento y seleccionando la plantilla de página maestra. Tenga en cuenta que las páginas maestras terminan con la extensión `.master`. Nombre de esta nueva página maestra `Site.master` y haga clic en Agregar.


[![Agregar una página maestra denominada Site.master para el sitio Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Figura 05**: agregar un nombre de página maestra `Site.master` al sitio Web ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Agregar un nuevo archivo de página maestra mediante Visual Web Developer crea una página maestra con el siguiente marcado declarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

La primera línea en el marcado declarativo es el [ `@Master` directiva](https://msdn.microsoft.com/library/ms228176.aspx). El `@Master` directiva es similar a la [ `@Page` directiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) que aparece en las páginas ASP.NET. Define el idioma de servidor (VB) y la información sobre la ubicación y la herencia de clase de código subyacente de la página maestra.

El `DOCTYPE` y marcado declarativo de la página aparece debajo de la `@Master` directiva. La página incluye HTML estático junto con cuatro controles de servidor:

- **Un formulario Web Forms (el `<form runat="server">`)** : porque todas las páginas ASP.NET normalmente tienen un formulario Web - y porque la página maestra puede incluir controles Web que deben aparecer dentro de un formulario Web, asegúrese de agregar el formulario Web Forms a la página principal (en lugar de agregar un formulario Web Forms e página de contenido esté).
- **Un control ContentPlaceHolder denominado `ContentPlaceHolder1`**  -este control ContentPlaceHolder aparece en el formulario Web Forms y actúa como la región de la interfaz de usuario de la página de contenido.
- **Un servidor `<head>` elemento** - la `<head>` elemento tiene el `runat="server"` atributo, lo que puede tener acceso mediante código del lado servidor. El `<head>` elemento se implementa de esta manera para que el título de la página y otros `<head>`-relacionados con marcado se pueden agregar o ajustar mediante programación. Por ejemplo, si se establece una página ASP.NET `Title` cambios de propiedad el `<title>` elemento representado por el `<head>` control de servidor.
- **Un control ContentPlaceHolder denominado `head`**  -este control ContentPlaceHolder aparece dentro de la `<head>` server controlar y puede utilizarse mediante declaración agregar contenido a la `<head>` elemento.

Este marcado declarativo de la página maestra predeterminada actúa como punto de partida para diseñar sus propias páginas maestras. Puede modificar el código HTML o para agregar controles Web adicionales o ContentPlaceHolders a la página maestra.

> [!NOTE]
> Al diseñar una página maestra Asegúrese de que la página maestra contiene un formulario Web Forms y que al menos un control ContentPlaceHolder aparece dentro de este formulario Web Forms.


### <a name="creating-a-simple-site-layout"></a>Crear un diseño de sitio Simple

Vamos a expandir `Site.master`del marcado declarativo predeterminado para crear un diseño de sitio que comparten todas las páginas: un encabezado común; una columna de la izquierda con la navegación, noticias y otro tipo de contenido en todo el sitio; y un pie de página que muestra el icono "Con la tecnología por Microsoft ASP.NET". Figura 6 se muestra el resultado final de la página maestra cuando uno de sus páginas de contenido se ve mediante un explorador. La región en un círculo roja en la figura 6 es específica de la página que se está visitada (`Default.aspx`); el resto del contenido está definido en la página maestra y, por tanto, coherentes en todas las páginas de contenido.


[![La página maestra define el marcado para la parte superior, izquierda y las partes de la parte inferior](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Figura 06**: la página maestra define el marcado para la parte superior, izquierda y las partes de la parte inferior ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Para lograr el diseño del sitio se muestra en la figura 6, inicie actualizando la `Site.master` página principal para que contenga el siguiente marcado declarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Diseño de la página maestra se define mediante una serie de `<div>` elementos HTML. El `topContent` `<div>` contiene el marcado que aparece en la parte superior de cada página, mientras que la `mainContent`, `leftContent`, y `footerContent` `<div>` se utilizan para mostrar contenido de la página, la columna izquierda y el "con la tecnología de Microsoft Icono ASP.NET", respectivamente. Además de agregar estos `<div>` elementos, cambiar también el `ID` propiedad del control ContentPlaceHolder principal de `ContentPlaceHolder1` a `MainContent`.

Las reglas de formato y diseño para estos ordenadas `<div>` elementos se establece en el [hoja de estilos en cascada (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) archivo `Styles.css`, que se especifica a través de un `<link>` elemento en de la página principal `<head>`elemento. Estas reglas diversos definen la apariencia de cada `<div>` elemento indicados anteriormente. Por ejemplo, el `topContent` `<div>` elemento, que muestra el texto "Tutoriales de páginas maestras" y el vínculo, tiene sus reglas de formato especificadas en `Styles.css` como se indica a continuación:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Si está siguiendo a lo largo de su equipo, debe descargar el código que lo acompañan de este tutorial y agregar el `Styles.css` archivo al proyecto. De forma similar, también debe crear una carpeta denominada `Images` y copie el icono "Con la tecnología por Microsoft ASP.NET" desde el sitio Web de demostración descargado a su proyecto.

> [!NOTE]
> Una discusión de CSS y el formato de página web queda fuera del ámbito de este artículo. Para obtener más información sobre CSS, visite la [tutoriales de CSS](http://www.w3schools.com/css/default.asp) en [W3Schools.com](http://www.w3schools.com/). También le animo a descargar código adjunto de este tutorial y jugar con los valores de CSS en `Styles.css` para ver los efectos de las reglas de formato diferentes.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Crear una página maestra usando una plantilla de diseño existente

En los años he creado un número de aplicaciones web ASP.NET para pequeñas y medianas empresas. Algunos de Mis clientes tenían un diseño de sitio existente que deseaban utilizar; otros contratado a un diseñador gráfico competente. Algunos otorgados me para diseñar el sitio Web. Como puede ver en la figura 6, tareas del programador para definir el diseño de un sitio Web es normalmente tan aconsejable como si tuviera un contable realizar open-heart surgery mientras su médico hará los impuestos.

Afortunadamente, hay innumerous sitios Web que ofrecen las plantillas de diseño HTML libres: Google devuelve resultados de más de seis millones para el término de búsqueda "plantillas de sitio Web gratuita." Uno de Mis favoritos es [OpenDesigns.org](http://opendesigns.org/). Una vez que encuentre una plantilla de sitio Web que desee, agregar los archivos CSS y las imágenes al proyecto del sitio Web e integrar HTML de la plantilla en la página maestra.

> [!NOTE]
> Microsoft también ofrece una serie de [plantillas de Kit de inicio de diseño de ASP.NET gratuitas](https://msdn.microsoft.com/asp.net/aa336613.aspx) que se integran en el cuadro de diálogo nuevo sitio Web en Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Paso 2: Crear páginas de contenido asociadas

Con la página maestra que se creó, estamos preparados para empezar a crear páginas ASP.NET que están enlazadas a la página maestra. Estas páginas se conocen como *páginas de contenido*.

Vamos a agregar una nueva página ASP.NET para el proyecto y enlazarla a la `Site.master` página maestra. Haga doble clic en el nombre del proyecto en el Explorador de soluciones y elija la opción de agregar nuevo elemento. Seleccione la plantilla de formulario Web Forms, escriba el nombre `About.aspx`y, a continuación, active la casilla de verificación "Seleccionar la página principal" tal como se muestra en la figura 7. Si lo hace, se mostrará el, seleccione un cuadro de diálogo de página maestra cuadro (consulte la figura 8) desde donde puede elegir la página maestra que se usa.

> [!NOTE]
> Si ha creado el sitio Web ASP.NET utilizando el modelo de proyecto de aplicación Web en lugar del modelo de proyecto de sitio Web no verá la casilla de verificación "Seleccionar la página principal" en el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 7. Para crear un contenido de página cuando usando el proyecto de aplicación Web de modelo debe elegir la plantilla de formulario de contenido Web en lugar de la plantilla de formulario Web Forms. Después de seleccionar la plantilla de formulario de contenido Web y haga clic en Agregar, el mismo seleccionar una página maestra se abre el cuadro de diálogo que se muestra en la figura 8.


[![Agregue una nueva página de contenido](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Figura 07**: agregar una nueva página de contenido ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Seleccione la página maestra Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Figura 08**: seleccione la `Site.master` página maestra ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Como se muestra en el siguiente marcado declarativo, una nueva página de contenido contiene un `@Page` directiva que señala de vuelta a su maestro de página y un control de contenido para cada uno de los controles de la página maestra ContentPlaceHolder.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> En la sección "Crear un diseño de sitio Simple" en el paso 1 cambia el nombre `ContentPlaceHolder1` a `MainContent`. Si no cambió el nombre del control ContentPlaceHolder `ID` de la misma manera, marcado declarativo de la página de contenido serán ligeramente diferentes en el marcado mostrado anteriormente. Del es decir, el segundo control de contenido `ContentPlaceHolderID` reflejará la `ID` del ContentPlaceHolder correspondiente de control en la página maestra.


Al representar una página de contenido, el motor ASP.NET debe fusible la página controles con su maestro ContentPlaceHolder controles de la página de contenido. El motor ASP.NET determina el contenido de página principal de la `@Page` la directiva `MasterPageFile` atributo. Como se muestra en el marcado anterior, esta página de contenido está enlazada a `~/Site.master`.

Dado que la página maestra tiene dos controles ContentPlaceHolder - `head` y `MainContent` -Visual Web Developer genera dos controles de contenido. Cada control de contenido hace referencia a un determinado ContentPlaceHolder a través de su `ContentPlaceHolderID` propiedad.

Donde se destacan páginas maestras respecto a las técnicas de plantilla de todo el sitio anterior es con su compatibilidad en tiempo de diseño. Ilustración 9 se muestra la `About.aspx` página de contenido cuando se ven a través de la vista de diseño del Visual Web Developer. Tenga en cuenta que mientras el contenido de la página maestra no está visible, está atenuado y no se puede modificar. Los controles de contenido correspondiente a ContentPlaceHolders la página maestra son, sin embargo, puede editables. Y, al igual que con cualquier otra página ASP.NET, puede crear la interfaz de la página de contenido mediante la adición de controles Web a través de las vistas de origen o de diseño.


[![La vista de diseño de la página de contenido muestra tanto el contenido de la página maestra y específica de la página](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Figura 09**: el contenido de la página diseño vista muestra tanto la específica de la página y contenido de la página maestra ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Agregar marcado y los controles Web a la página de contenido

Tómese un momento para crear parte del contenido de la `About.aspx` página. Como puede ver en la figura 10, que se ha especificado un encabezado "Sobre el autor" y un par de párrafos de texto, pero no dude en Agregar controles Web, también. Después de crear esta interfaz, visite la `About.aspx` página a través de un explorador.


[![Visite la página About.aspx a través de un explorador](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Figura 10**: visite el `About.aspx` página a través de un explorador ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


Es importante comprender que la página de contenido solicitada y su página principal asociada se fusionada y se presentan como un todo completamente en el servidor web. Explorador del usuario final, a continuación, se envía el HTML resultante, combinado. Para comprobar esto, vea el código HTML recibido por el explorador, vaya al menú Ver y elija el origen. Tenga en cuenta que no hay ningún marcos o las otras técnicas especializadas para mostrar dos páginas web diferentes en una sola ventana.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Enlazar una página principal a una página ASP.NET existente

Como hemos visto en este paso, agregar una nueva página de contenido a una aplicación web ASP.NET es tan fácil como activando la casilla de verificación "Seleccionar la página principal" y seleccionar la página maestra. Por desgracia, no es tan fácil convertir una página ASP.NET existente a una página maestra.

Para enlazar una página maestra a una página ASP.NET existente debe realizar los pasos siguientes:

1. Agregar el `MasterPageFile` atributo a la página ASP.NET `@Page` directiva, haga que apunte a la página principal adecuada.
2. Agregar controles de contenido para cada uno de los ContentPlaceHolders en la página maestra.
3. Selectivamente corte y pegue el contenido de la página ASP.NET existente en los controles de contenido adecuados. Quiero "selectivamente" aquí porque ASP.NET página probablemente contiene marcado que ya se expresa mediante la página maestra, como el `DOCTYPE`, el `<html>` elemento y el formulario Web Forms.

Para obtener instrucciones paso a paso acerca de este proceso junto con capturas de pantalla, visite [Scott Guthrie](https://weblogs.asp.net/scottgu/)del [utilizando páginas principales y la navegación de sitio](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) tutorial. La "actualización `Default.aspx` y `DataSample.aspx` para usar la página maestra" sección detallan estos pasos.

Dado que es mucho más fácil crear nuevas páginas de contenido que es convertir páginas ASP.NET existentes en páginas de contenido, recomienda que siempre que cree un nuevo sitio Web ASP.NET agregar una página maestra al sitio. Enlazar todas las páginas ASP.NET nuevo a esta página maestra. No se preocupe si la página maestra inicial es muy simple o sin formato; puede actualizar la página maestra más adelante.

> [!NOTE]
> Al crear una nueva aplicación de ASP.NET, Visual Web Developer agrega un `Default.aspx` página que no está enlazada a una página maestra. Si desea convertir una página ASP.NET existente en una página de contenido de práctica, siga adelante y hágalo con `Default.aspx`. Como alternativa, puede eliminar `Default.aspx` y, a continuación, vuelva a agregar, pero esta vez activando la casilla de verificación "Seleccionar la página principal".


## <a name="step-3-updating-the-master-pages-markup"></a>Paso 3: Actualizar el marcado de la página maestra

Una de las principales ventajas de las páginas maestras es que una sola página maestra puede utilizarse para definir el diseño general de varias páginas en el sitio. Por lo tanto, es necesario actualizar un único archivo: la página maestra actualizando la apariencia del sitio.

Para ilustrar este comportamiento, vamos a actualizar la página maestra para incluir la fecha actual en la parte superior de la columna izquierda. Agregar una etiqueta denominada `DateDisplay` a la `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

A continuación, cree un `Page_Load` controlador de eventos para el maestro de página y agregue el código siguiente:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

El código anterior establece la etiqueta `Text` formato de la propiedad a la fecha y hora actual como el día de la semana, el nombre del mes y el día de dos dígitos (consulte la figura 11). Con este cambio, vuelva a visitar una de las páginas de contenido. Como se muestra en la figura 11, el marcado resultante se actualiza inmediatamente para que incluya el cambio a la página maestra.


[![Los cambios realizados en la página principal están reflejados al ver la página de contenido](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Figura 11**: los cambios en la página principal están reflejados al ver la página de contenido ([haga clic aquí para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Como se muestra en este ejemplo, páginas maestras pueden contener controles de servidor Web, código y controladores de eventos.


## <a name="summary"></a>Resumen

Las páginas maestras permiten a los desarrolladores ASP.NET crear un diseño coherente en todo el sitio que es actualizable fácilmente. Crear páginas maestras y sus páginas de contenido asociadas es tan simple como crear páginas ASP.NET estándar, como Visual Web Developer ofrece buena compatibilidad en tiempo de diseño.

El ejemplo de página maestra que se creó en este tutorial tenía dos controles ContentPlaceHolder, head y MainContent. Solo especificamos marcado para el control MainContent ContentPlaceHolder en nuestra página de contenido, sin embargo. En el siguiente tutorial veremos con contenido de varios controles en la página de contenido. También se muestra cómo definir predeterminado controla el marcado para el contenido dentro de la página maestra, así como el modo para alternar entre el uso de manera predeterminada marcado definido en el maestro de página y proporcionar marcado personalizada de la página de contenido.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ASP.NET para diseñadores: libre para las plantillas de diseño y orientación sobre la creación de sitios Web ASP.NET utilizando estándares Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Información general de páginas maestras de ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutoriales de (CSS) de hojas de estilos en cascada](http://www.w3schools.com/css/default.asp)
- [Establecer el título de la página de forma dinámica](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Páginas maestras en ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Tutoriales de inicio rápido de páginas maestras](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](nested-master-pages-cs.md)
> [Siguiente](multiple-contentplaceholders-and-default-content-vb.md)
