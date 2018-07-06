---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introducción a las páginas Web ASP.NET - Introducción | Microsoft Docs
author: tfitzmac
description: WebMatrix no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use Visual Studio o Visual Studio Code. Esta guía un...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: b4f554d2bf8bf564fd69239fcc7cc605158c83c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825042"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Introducción a las páginas Web ASP.NET - Introducción
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) o [código de Visual Studio](https://code.visualstudio.com/).
> 
> 
> Esta guía y la aplicación le ofrece una visión general de ASP.NET Web Pages (versión 2 o posterior) y la sintaxis de Razor, un marco ligero para la creación de sitios Web dinámicos. También introduce WebMatrix, una herramienta para crear páginas y sitios.
> 
> **Nivel**: nuevo a ASP.NET Web Pages.  
> **Habilidades supone**: HTML, hojas de estilos en cascada (CSS) básico.
> 
> Lo que aprenderá en el primer tutorial del conjunto:
> 
> - Es de qué tecnología ASP.NET Web Pages y para qué sirve.
> - ¿Qué es WebMatrix.
> - Cómo instalar todos los componentes.
> - Cómo crear un sitio Web mediante WebMatrix.
>   
> 
> Características y tecnologías tratadas:
> 
> - Instalador de plataforma Web de Microsoft.
> - WebMatrix.
> - *.cshtml* páginas
>   
> 
> Mike Pope originalmente escribió este tutorial. Tom FitzMacken había actualizado para Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>¿Qué debería saber?

Suponemos que está familiarizado con:

- **HTML**. No se requiere ninguna experiencia en profundidad. No explicaremos HTML, pero también no usamos ninguna acción demasiado compleja. Proporcionaremos vínculos a tutoriales HTML que creemos que son útiles.
- **Hojas de estilos en cascada (CSS)**. Igual que con HTML.
- **Ideas de la base de datos básica**. Si ha usa una hoja de cálculo para los datos y ordenar y filtrar los datos, que es el nivel de experiencia generalmente suponemos para esta serie de tutoriales.

Suponemos que está interesado en aprender programación básica. ASP.NET Web Pages usan un lenguaje de programación llamado C#. No tienes que tienen cualquier experiencia en programación, simplemente un interés en él. Si alguna vez escribió cualquier código JavaScript en una página web antes, tiene una gran cantidad de fondo.

Tenga en cuenta que si está familiarizado con la programación, es posible que este tutorial establece inicialmente se mueven lentamente mientras se poner al día de los programadores nuevos. Cuando obtenemos más allá de los tutoriales de pocos primeros, sin embargo, habrá menos básicos de programación para explicar y las cosas se moverán a lo largo de en un clip más rápido.

## <a name="what-do-you-need"></a>¿Lo que necesita?

Aquí está lo que necesita:

- Un equipo que ejecuta Windows 8, Windows 7, Windows Server 2008 o Windows Server 2012.
- Una conexión activa a internet.
- Privilegios de administrador (necesarios para el proceso de instalación).

## <a name="what-is-aspnet-web-pages"></a>¿Qué es ASP.NET Web Pages?

Las páginas Web ASP.NET es un marco que puede usar para crear páginas web dinámicas. Una página web HTML es estático; su contenido viene determinada por el marcado HTML fijo que se encuentra en la página. Las páginas dinámicas como los creados con ASP.NET Web Pages le permiten crear contenido de la página sobre la marcha, mediante código.

Las páginas dinámicas le permiten realizar a todo tipo de cosas. Puede pedir al usuario para la entrada mediante un formulario y, a continuación, cambiar lo que se muestra en la página o su aspecto. Puede toma la información de un usuario, guárdela en una base de datos y, a continuación, la lista más adelante. Puede enviar correo electrónico desde su sitio. Puede interactuar con otros servicios en la web (por ejemplo, un servicio de asignación) y generar páginas que integran la información de esos orígenes.

## <a name="what-is-webmatrix"></a>¿Qué es WebMatrix?

WebMatrix es una herramienta que integra un editor de páginas web, una utilidad de la base de datos, un servidor web para probar las páginas y características para publicar su sitio Web en Internet. WebMatrix es gratuito y es fácil de instalar y fácil de usar. (También funciona para las páginas HTML simples, así como para otras tecnologías como PHP.)

No lo hace realmente *tiene* a usar WebMatrix para trabajar con ASP.NET Web Pages. Por ejemplo, puede crear páginas mediante el uso de un texto editor y probar mediante el uso de un servidor web que tienen acceso a las páginas. Sin embargo, WebMatrix facilita todo, por lo que va a utilizar WebMatrix a lo largo de estos tutoriales.

## <a name="about-these-tutorials"></a>Acerca de estos tutoriales

Esta serie de tutoriales es una introducción al uso de ASP.NET Web Pages. Hay 9 tutoriales total en esta serie de tutoriales introductoria. Lo 's parte de una serie de conjuntos de tutoriales que le llevará desde páginas Web de ASP.NET principiante a la creación de sitios Web real y profesional.

Este primer tutorial establece concentrados en mostrarle los aspectos básicos de cómo trabajar con ASP.NET Web Pages. Cuando haya terminado, puede trabajar con conjuntos de tutoriales adicionales que recoja donde finaliza este y que explorar páginas Web con más detalle.

Nos deliberadamente funcionan con las explicaciones exhaustivas. Y cada vez que se muestra algo, para esta serie de tutoriales siempre elegimos la manera que creemos que es más fácil de entender. Conjuntos de tutorial posteriores explican con mayor profundidad y mostrarle los enfoques más eficaces o más flexibles (también más divertido). Pero estos tutoriales requieren que comprender los conceptos básicos en primer lugar.

El conjunto del tutorial que acaba de iniciar trata estas características de ASP.NET Web Pages:

- Introducción y comprobar que todo instalado. (Que está en el tutorial que está leyendo).
- Los conceptos básicos de la programación de ASP.NET Web Pages.
- Creación de una base de datos.
- Creación y procesamiento de un formulario de entrada de usuario.
- Agregar, actualizar y eliminar datos en la base de datos.

## <a name="what-will-you-create"></a>¿Qué va a crear?

Establezca este tutorial y posteriores que giran en torno a un sitio Web donde puede enumerar las películas que le guste. Podrá escribir películas, editarlos y mostrarlas. Estos son algunos de las páginas que creará en esta serie de tutoriales. La primera de ellas muestra la película al mostrar la página que se va a crear:

![Aplicación de película ha terminado que muestra una lista de películas](getting-started/_static/image1.png)

Y aquí es la página que le permite agregar nueva información de películas en su sitio:

![Aplicación de película finalizada que muestra la página Agregar película](getting-started/_static/image2.png)

Compilación de conjuntos de tutorial posteriores en este conjunto y agregar más funcionalidad, como cargar imágenes, lo que permite a las personas iniciar sesión, enviar correo electrónico y la integración con redes sociales.

## <a name="see-this-app-running-on-azure"></a>Consulte esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

## <a name="installing-everything"></a>Instalar todo

Puede instalar todo el contenido mediante el uso del instalador de plataforma Web de Microsoft. De hecho, instale al instalador y, a continuación, usarlo para instalar todo lo demás.

Para usar las páginas Web, debe tener al menos Windows XP con Service Pack 3 instalado, o Windows Server 2008 o posterior.

En el [página de Web Pages](../../../index.md) del sitio Web ASP.NET, haga clic en **instalar**.

![Mostrando del sitio Web de ASP.NET &quot;instalar WebMatrix&quot; botón](getting-started/_static/image3.png)

Deberá aceptar los términos de licencia y declaración de privacidad antes de instalar WebMatrix.

![Acepte los términos para comenzar la instalación](getting-started/_static/image4.png)

Haga clic en **ejecutar** para iniciar la instalación. (Si desea guardar el programa de instalación, haga clic en **guardar** y, a continuación, ejecute el instalador desde la carpeta donde ha guardado.)

![](getting-started/_static/image5.png)

Aparece el instalador de plataforma Web, está listo para instalar WebMatrix. Haga clic en **Instalar**.

![](getting-started/_static/image6.png)

El proceso de instalación averigua qué tiene que instalar en el equipo e inicia el proceso. Función exactamente lo que debe estar instalado, el proceso puede tardar desde unos minutos hasta varios minutos. Seleccione **acepto** para aceptar los términos de licencia.

## <a name="hello-webmatrix"></a>Hola, WebMatrix

Cuando termine, el proceso de instalación puede iniciar automáticamente WebMatrix. Si no es así, en Windows, desde el **iniciar** menú, inicie **Microsoft WebMatrix**.

Cuando inicie WebMatrix por primera vez, tendrá la oportunidad de iniciar sesión en Microsoft Azure con su cuenta de Microsoft. Al iniciar sesión, recibirán 10 aplicaciones web gratuito a través de Azure. Estas aplicaciones web gratuitas proporcionan una manera cómoda para probar sus aplicaciones. Si aún no tiene una cuenta de Azure, pero tiene una suscripción a MSDN, puede [activar sus beneficios de suscripción de MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). En caso contrario, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

No es necesario que iniciar sesión en este momento continuar con este tutorial. Si no inicia sesión ahora, aún tendrá la opción de iniciar sesión más tarde. La última [tema](publishing.md) en este tutorial serie cubre cómo implementar su sitio Web en Azure; por lo tanto, deberá iniciar sesión para completar ese tema.

En este momento, inicie sesión con su cuenta de Microsoft o seleccione **ahora no** en la esquina inferior derecha.

![Iniciar sesión](getting-started/_static/image7.png)

Para empezar, deberá crear un sitio Web en blanco y agregue una página. En un tutorial más adelante en este conjunto reproducirá con una de las plantillas de sitio Web integrados.

En la ventana de inicio, haga clic en **New**.

![Pantalla de inicio de WebMatrix](getting-started/_static/image8.png)

Las plantillas son archivos creados previamente y páginas para diferentes tipos de sitios Web. Para ver todas las plantillas que están disponibles de forma predeterminada, seleccione la opción de la Galería de plantillas.

![Seleccionar galería de plantillas](getting-started/_static/image9.png)

En el **inicio rápido** ventana, seleccione **sitio vacío** desde el **ASP.NET** de grupo y el nuevo sitio el nombre "WebPagesMovies".

![Ventana de inicio rápido de WebMatrix con la plantilla sitio vacío seleccionada](getting-started/_static/image10.png)

Haga clic en **Siguiente**.

Si ha iniciado sesión en su cuenta de Microsoft, se le ofrecerá la oportunidad de crear el sitio en Azure. Según el nombre del sitio, el nombre predeterminado de **WebPagesMovies.azurewebsites.net** se sugiere; sin embargo, el signo de exclamación indica que este nombre no está disponible en Windows Azure. Por motivos de simplicidad, seleccione **Skip** para omitir la creación del sitio web en Azure ahora mismo. Más adelante en esta serie, el sitio se publicará en Azure.

![Crear sitio de azure](getting-started/_static/image11.png)

WebMatrix crea y abre el sitio:

![Nuevo sitio WebPagesMovies abiertos en WebMatrix](getting-started/_static/image12.png)

En la parte superior, hay una barra de herramientas de acceso rápido y una cinta de opciones. En la parte inferior izquierda, verá el selector de área de trabajo donde cambia entre las tareas (**sitio**, **archivos**, **bases de datos**, **informes**). A la derecha es el panel de contenido para el editor y para los informes. Y en la parte inferior, ocasionalmente verá una barra de notificación de mensajes.

Aprenderá más acerca de WebMatrix y sus características conforme avance en estos tutoriales.

## <a name="creating-a-web-page"></a>Creación de una página Web

Para familiarizarse con WebMatrix y ASP.NET Web Pages, creará una página sencilla.

En el selector de área de trabajo, seleccione el **archivos** área de trabajo. Esta área de trabajo le permite trabajar con archivos y carpetas. El panel izquierdo muestra la estructura de archivos de su sitio. Los cambios de la cinta de opciones para mostrar las tareas relacionadas con el archivo.

![Archivo de área de trabajo de WebMatrix](getting-started/_static/image13.png)

En la cinta de opciones, haga clic en la flecha situada debajo **New** y, a continuación, haga clic en **nuevo archivo**.

![Mediante el &quot;New&quot; comando en la cinta de opciones para crear un nuevo archivo](getting-started/_static/image14.png)

WebMatrix muestra una lista de tipos de archivo. Seleccione **CSHTML**y en el **nombre** , escriba "HelloWorld". Una página CSHTML es una página de ASP.NET Web Pages.

![Crear una nueva página CSHTML denominado HelloWorld.cshtml](getting-started/_static/image15.png)

Haga clic en **Aceptar**.

WebMatrix creará la página y abre en el editor.

![La nueva página HelloWorld en el editor de WebMatrix](getting-started/_static/image16.png)

Como puede ver, la página contiene principalmente normal marcado HTML, excepto en un bloque en la parte superior que tiene este aspecto:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Esto para agregar el código, como verá en breve.

Tenga en cuenta que las distintas partes de la página &mdash; los nombres de elementos, atributos y texto, además del bloque en la parte superior, están en distintos colores. Esto se denomina *resaltado de sintaxis*, y es más fácil de mantener todo claro. Es una de las características que facilita trabajar con páginas web en WebMatrix.

Agregar contenido para el `<head>` y `<body>` elementos como en el ejemplo siguiente. (Si lo desea, puede simplemente copie el siguiente bloque y reemplace toda la página existente con este código.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

En la barra de herramientas de acceso rápido o en el **archivo** menú, haga clic en **guardar**.

![Botón Guardar en la barra de herramientas de acceso rápido WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Probar la página

En el **archivos** área de trabajo, haga clic en el *HelloWorld.cshtml* página y, a continuación, haga clic en **iniciar en el explorador**.

![Ejecución de una página con el botón ejecutar en la cinta de opciones de WebMatrix](getting-started/_static/image18.png)

WebMatrix inicia un servidor web integrado (IIS Express) que puede usar para probar las páginas en el equipo. (Sin IIS Express en WebMatrix, deberá publicar la página en un servidor web en algún lugar antes de que se podría probar.) La página se muestra en el explorador predeterminado.

![&quot;Hola mundo&quot; página que se ejecuta en el explorador](getting-started/_static/image19.png)

Tenga en cuenta que, al probar una página en WebMatrix, la dirección URL en el explorador es algo parecido a `http://localhost:33651/HelloWorld.cshtml.` el nombre *localhost* hace referencia a un servidor local, lo que significa que la página proporciona un servidor web que se encuentra en su propio equipo. Como se indicó, WebMatrix incluye un programa de servidor web denominado IIS Express que se ejecuta cuando se inicia una página.

El número después *localhost* (por ejemplo, *localhost:33651*) hace referencia a un *número de puerto* en el equipo. Este es el número de "canal" que IIS Express se utiliza para este sitio Web determinado. El número de puerto se selecciona de forma aleatoria desde el intervalo de 1024 a 65536 cuando cree su sitio, y es diferente para cada sitio que cree. (Cuando se prueba un sitio propio, el número de puerto casi seguro que será un número diferente de 33561.) Mediante el uso de un puerto diferente para cada sitio Web, IIS Express puede mantener recta cuál de los sitios que está hablando con.

Más adelante al publicar su sitio en un servidor web pública, ya no verá *localhost* en la dirección URL. En ese momento, verá una dirección URL más típica como `http://myhostingsite/mywebsite/HelloWorld.cshtml` o cualquier la página. Aprenderá más acerca de cómo publicar un sitio más adelante en esta serie de tutoriales.

## <a name="adding-some-server-side-code"></a>Agregar algo de código del lado servidor

Cierre el explorador y vuelva a la página en WebMatrix.

Agregue una línea al bloque de código para que quede como el código siguiente:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Esto es un poco de código Razor. Está probablemente claro que obtiene la fecha y hora actual y coloca ese valor en un *variable* denominado `currentDateTime`. Podrá leer más sobre la sintaxis de Razor en el siguiente tutorial.

En el cuerpo de la página, una vez el `<p>Hello World!</p>` elemento, agregue lo siguiente:

[!code-html[Main](getting-started/samples/sample4.html)]

Este código obtiene el valor que se pone en la `currentDateTime` variable en la parte superior y lo inserta en el marcado de la página. El `@` carácter marca el código de ASP.NET Web Pages en la página.

Ejecute la página nuevo (WebMatrix guarda los cambios automáticamente antes de ejecutar la página). Esta vez verá la fecha y hora en la página.

![&quot;Hola mundo&quot; página que se ejecuta en el explorador con una visualización del tiempo generada dinámicamente](getting-started/_static/image20.png)

Espere unos instantes y, a continuación, actualice la página en el explorador. Se actualiza la presentación de fecha y hora.

En el explorador, examine el código fuente de la página. Parece que el marcado siguiente:

[!code-html[Main](getting-started/samples/sample5.html)]

Tenga en cuenta que el `@{ }` bloque en la parte superior, no se encuentra allí. Observe también que la presentación de fecha y hora muestra una cadena real de caracteres (`1/18/2012 2:49:50 PM` o cualquier), no `@currentDateTime` que tenía en el *.cshtml* página. ¿Qué ha ocurrido aquí es que al ejecutar la página, ASP.NET procesa todo el código (muy poco en este caso) que se marcó con `@`. El código genera resultados, y que los resultados se insertan en la página.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Esto es lo que las páginas Web ASP.NET se acerca de

Cuando se leen que ASP.NET Web Pages genera contenido web dinámico, lo que ha visto aquí es la idea. La página que acaba de crear contiene el mismo marcado HTML que se ha visto antes. También puede contener código que puede realizar a todo tipo de tareas. En este ejemplo, realizaba la tarea trivial de obtener la fecha y hora actuales. Como vimos, puede intercalar código con el código HTML para generar la salida en la página. Cuando un usuario solicita un *.cshtml* página en el explorador, ASP.NET procesa la página mientras aún está en manos del servidor web. ASP.NET inserta el resultado del código (si existe) en la página como HTML. Cuando se realiza el procesamiento de código, ASP.NET envía la página resultante en el explorador. Todo el explorador nunca obtiene es HTML. Este es un diagrama:

![Flujo conceptual de cómo genera ASP.NET HTML dinámicamente](getting-started/_static/image21.png)

La idea es sencilla, pero hay muchas tareas interesantes que puede realizar el código y hay muchas maneras interesantes en el que puede agregar dinámicamente contenido HTML a la página. Y ASP.NET *.cshtml* páginas, como cualquier página HTML, también pueden incluir código que se ejecuta en el explorador mismo (código de JavaScript y jQuery). Explorará todas estas cosas en esta serie de tutoriales y en las posteriores.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial de esta serie, explorar un poco más de programación de ASP.NET Web Pages.

## <a name="additional-resources"></a>Recursos adicionales

[Crear un sitio Web de ASP.NET desde cero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Este es un tutorial que es específicamente sobre el uso de WebMatrix (no ASP.NET Web Pages). Pasa a un poco más detalles sobre algunas de las características adicionales de WebMatrix que se va a tratar en esta serie de tutoriales.

> [!div class="step-by-step"]
> [Siguiente](intro-to-web-pages-programming.md)
