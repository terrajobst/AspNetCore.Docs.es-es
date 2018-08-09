---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Descripción de las capacidades de depuración de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La capacidad para depurar el código es una habilidad que todos los desarrolladores deben tener en su arsenal independientemente de la tecnología que usa. Aunque muchos desarrolladores son...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 95c2487f26109cbdd8c76dc6f269f37264f5e34b
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655451"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Descripción de las capacidades de depuración de ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La capacidad para depurar el código es una habilidad que todos los desarrolladores deben tener en su arsenal independientemente de la tecnología que usa. Mientras que muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones de ASP.NET que usan código C# o VB.NET, algunas no están conscientes de que también es muy útil para depurar código de cliente, como JavaScript. También puede aplicarse el mismo tipo de las técnicas utilizadas para depurar aplicaciones de .NET para aplicaciones habilitadas para AJAX y más específicamente a las aplicaciones de ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Depuración de aplicaciones de ASP.NET AJAX

Dan Wahlin

La capacidad para depurar el código es una habilidad que todos los desarrolladores deben tener en su arsenal independientemente de la tecnología que usa. Hace falta decir que entender las diferentes opciones de depuración que están disponibles puede guardar una gran cantidad de tiempo en un proyecto y quizás incluso algunos dolores de cabeza. Mientras que muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones de ASP.NET que usan código C# o VB.NET, algunas no están conscientes de que también es muy útil para depurar código de cliente, como JavaScript. También puede aplicarse el mismo tipo de las técnicas utilizadas para depurar aplicaciones de .NET para aplicaciones habilitadas para AJAX y más específicamente a las aplicaciones de ASP.NET AJAX.

En este artículo verá cómo se pueden usar Visual Studio 2008 y otras herramientas para depurar aplicaciones de ASP.NET AJAX para localizar rápidamente los errores y otros problemas. Esta discusión incluirá información sobre cómo habilitar Internet Explorer 6 o superior para la depuración, uso de Visual Studio 2008 y el Explorador de scripts para recorrer el código, así como otras herramientas gratuitas como Web Development Helper. También aprenderá a depurar aplicaciones de ASP.NET AJAX en Firefox utilizando que una extensión denominada a Firebug que permite recorrer el código de JavaScript directamente en el explorador sin cualquier otra herramienta. Por último, se muestra una introducción a las clases de la biblioteca de AJAX de ASP.NET que puede ayudarle con diversas tareas de depuración, como el seguimiento y las instrucciones de aserción de código.

Antes de intentar depurar páginas visitadas en Internet Explorer, hay algunos pasos básicos que deberá llevar a cabo para habilitarla para la depuración. Echemos un vistazo a algunos requisitos de configuración básica que deben realizarse para empezar a trabajar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configuración de Internet Explorer para la depuración

Mayoría de las personas no esté interesada en Ver problemas de JavaScript que se encuentran en un sitio Web con Internet Explorer ver. De hecho, el usuario medio incluso no sabría qué hacer si ha visto un mensaje de error. Como resultado, las opciones de depuración están desactivadas de forma predeterminada en el explorador. Sin embargo, es muy sencillo activar la depuración y empezar a usar al desarrollar nuevas aplicaciones AJAX.

Para habilitar la funcionalidad de depuración, vaya a opciones de Internet de las herramientas en el menú de Internet Explorer y seleccione la ficha Opciones avanzadas. En la sección de exploración Asegúrese de que se desactivan los siguientes elementos:

- Deshabilitar la depuración de scripts (Internet Explorer)
- Deshabilitar la depuración de scripts (otros)

Aunque no es necesario, si intenta depurar una aplicación que probablemente deseará los errores de JavaScript en la página esté visible y obvio de inmediato. Puede forzar que todos los errores que se mostrará un cuadro de mensaje activando la casilla "Mostrar una notificación sobre cada error de script". Aunque esto es una buena opción para activar al desarrollar una aplicación, rápidamente puede resultar molesto si simplemente está examinar otros sitios Web ya que las posibilidades de que se produzcan errores de JavaScript son bastante buenas.

Una vez que se ha configurado correctamente para la depuración, debe ser que Internet Explorer avanzada de cuadro de diálogo se muestra en la figura 1.


[![Configuración de Internet Explorer para la depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configuración de Internet Explorer para la depuración.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Una vez que la depuración se ha activado, verá un nuevo elemento de menú aparece en el menú de vista denominado a depurador de Script. Tiene dos opciones disponibles incluidas abierto y salto en la próxima instrucción. Cuando se selecciona abierto se le pedirá para depurar la página en Visual Studio 2008 (tenga en cuenta que Visual Web Developer Express también puede utilizarse para la depuración). Si ya se está ejecutando Visual Studio .NET puede usar esa instancia o para crear una nueva instancia. Cuando se selecciona la interrupción en la siguiente instrucción que se le pedirá para depurar la página cuando se ejecuta el código de JavaScript. Si el código JavaScript que se ejecuta en el evento onLoad de la página puede actualizar la página para desencadenar una sesión de depuración. Si el código de JavaScript se ejecuta después de que se hace clic en un botón, a continuación, el depurador se ejecutará inmediatamente después de que se hace clic en el botón.

> [!NOTE]
> Si está ejecutando en Windows Vista con Control de acceso de usuario (UAC) habilitado, y tiene Visual Studio 2008 configurado para ejecutarse como administrador, se producirá un error en Visual Studio adjuntar al proceso cuando se le pida para adjuntar. Para solucionar este problema, inicie Visual Studio en primer lugar y utilice esa instancia para depurar.

Aunque la siguiente sección demuestra cómo depurar una página de ASP.NET AJAX directamente desde dentro de Visual Studio 2008, utilizando la opción del depurador de Script de Internet Explorer es útil cuando una página ya está abierta y le gustaría inspeccionarla más completa.

## <a name="debugging-with-visual-studio-2008"></a>Depuración con Visual Studio 2008

Visual Studio 2008 ofrece funcionalidad de depuración que los desarrolladores en todo el mundo confían en cada día a las aplicaciones de .NET de depuración. El depurador integrado permite recorrer el código, vista de datos de objetos, inspección de variables específicas, supervisión la pila de llamadas y mucho más. Además de depurar el código de C# o VB.NET, el depurador también es útil para depurar aplicaciones de ASP.NET AJAX y le permitirá recorrer el código línea por línea de JavaScript. Los detalles que siguen el foco en las técnicas que pueden usarse para depurar archivos de script de cliente en lugar de proporcionar un discurso en todo el proceso de depuración de aplicaciones con Visual Studio 2008.

El proceso de depuración de una página en Visual Studio 2008 se puede iniciar de varias maneras diferentes. En primer lugar, puede usar la opción de Internet Explorer Script Debugger se ha mencionado en la sección anterior. Esto funciona bien cuando una página ya está cargada en el explorador y desea comenzar a depurarlo. Como alternativa, puede hacer doble clic en una página .aspx en el Explorador de soluciones y seleccione Establecer como página de inicio en el menú. Si está acostumbrado a la depuración de páginas ASP.NET, a continuación, probablemente hacer esto antes. Una vez que se presiona F5 se puede depurar la página. Sin embargo, mientras que por lo general puede establecer un punto de interrupción en cualquier lugar que desee en código C# o VB.NET, que no siempre es el caso con JavaScript como verá a continuación.

*Embedded frente a Scripts externos*

El depurador de Visual Studio 2008 trata JavaScript incrustado en una página diferente a archivos JavaScript externos. Archivos de script externo, puede abrir el archivo y establecer un punto de interrupción en cualquier línea que elija. Los puntos de interrupción se pueden establecer haciendo clic en el área de la Bandeja de gris a la izquierda de la ventana del editor de código. Cuando se inserta JavaScript directamente en una página con el `<script>` etiqueta, establecer un punto de interrupción, haga clic en el área gris bandeja no es una opción. Intenta establecer un punto de interrupción en una línea de script incrustado dará como resultado una advertencia que indica "No es una ubicación válida para un punto de interrupción".

Puede obtener en torno a este problema, mueva el código en un archivo externo .js y que hacen referencia a ella mediante el atributo src de la &lt;script&gt; etiqueta:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

¿Qué ocurre si mueve el código a un archivo externo no es una opción o requiere más trabajo vale la pena? Mientras no se puede establecer un punto de interrupción mediante el editor, puede agregar la instrucción de depurador directamente en el código donde desea iniciar la depuración. También puede usar la clase Sys.Debug disponible en la biblioteca AJAX de ASP.NET para forzar que iniciar la depuración. Aprenderá más acerca de la clase Sys.Debug más adelante en este artículo.

Un ejemplo del uso de la `debugger` palabra clave se muestra en el listado 1. En este ejemplo se fuerza el depurador se interrumpa adecuado antes de que se realiza una llamada a una función de actualización.

**Listado 1. Con la palabra clave depurador para forzar que el depurador de Visual Studio .NET se interrumpa.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Una vez que se alcanza la instrucción de depurador que se pedirá a depurar la página utilizando Visual Studio .NET y puede comenzar a recorrer el código. Mientras realiza esta puede encontrar un problema con el acceso a los archivos de script de biblioteca AJAX de ASP.NET utilizados en la página, así que vamos a tomar un vistazo al uso de Visual Studio. Explorador de scripts de la red.

## <a name="using-visual-studio-net-windows-to-debug"></a>Uso de Windows de .NET de Visual Studio para depurar

Una vez que se inicia una sesión de depuración y empezar a recorrer el código mediante la tecla F11 de forma predeterminada, puede encontrar el cuadro de diálogo de error que se muestra en consulte la figura 2, a menos que todos los archivos de script que se utiliza en la página están abiertos y disponibles para la depuración.


[![Cuadro de diálogo de error mostrado cuando no hay código fuente está disponible para la depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: cuadro de diálogo de Error mostrado cuando no hay código fuente está disponible para la depuración.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Este cuadro de diálogo se muestra porque Visual Studio .NET no está seguro de cómo abrir el código fuente de algunas de las secuencias de comandos que se hace referencia a la página. Aunque esto puede resultar muy frustrante en primer lugar, hay una revisión sencilla. Una vez que haya iniciado una sesión de depuración y alcanza un punto de interrupción, vaya a la ventana Explorador de scripts de Windows de depuración en el menú de Visual Studio 2008 o use la combinación de teclas Ctrl + Alt + N.

> [!NOTE]
> Si no ve el menú del explorador de scripts que aparece, vaya a **herramientas** > **personalizar** > **comandos** en el menú de Visual Studio. NET. Busque el **depurar** entrada en las categorías de sección y haga clic para mostrar todas las entradas de menú disponibles. En la lista de comandos, desplácese hacia abajo hasta el Explorador de scripts y, a continuación, arrastre hacia arriba en el menú se ha mencionado anteriormente de Windows Debug. Esto hará que la entrada de menú del explorador de scripts disponible cada vez que ejecute Visual Studio. NET.

El Explorador de scripts puede usarse para ver todos los scripts que se usa en una página y abrirlos en el editor de código. Una vez abierto el Explorador de scripts, haga doble clic en la página .aspx que se está depura actualmente para abrirlo en la ventana del editor de código. Realizar la misma acción para todos los demás scripts que se muestra en el Explorador de scripts. Una vez que todos los scripts están abiertos en la ventana de código puede presione F11 (y use las otras teclas de aceleración de depuración) para recorrer el código. Figura 3 muestra un ejemplo del explorador de scripts. Muestra el archivo actual que se está depurando (Demo.aspx), así como dos scripts personalizados y dos scripts insertados en la página de forma dinámica mediante el control de ScriptManager de ASP.NET AJAX.


[![El Explorador de scripts proporciona fácil acceso a los scripts usados en una página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. El Explorador de scripts proporciona fácil acceso a los scripts usados en una página.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Algunos otros windows también pueden utilizarse para proporcionar información útil al ir avanzando por el código en una página. Por ejemplo, puede usar la ventana variables locales para ver los valores de diferentes variables utilizadas en la página, la ventana Inmediato para evaluar variables específicas o las condiciones y ver la salida. También puede usar la ventana de salida para ver las instrucciones de seguimiento escritas utilizando la función Sys.Debug.trace (lo que se explicará más adelante en este artículo) o Debug.writeln (función) de Internet Explorer.

Paso a paso a través del código con el depurador puede pasa el mouse sobre las variables en el código para ver el valor que están asignados. Sin embargo, el depurador de script en ocasiones, no mostrará nada cuando mueve el mouse sobre una variable de JavaScript determinada. Para ver el valor, resalte la instrucción o la variable que está intentando ver en la ventana del editor de código y, a continuación, mueva el mouse sobre él. Aunque esta técnica no funciona en todas las situaciones, muchas veces podrá ver el valor sin tener que buscar en una ventana de depuración diferente como la ventana variables locales.

Se puede ver un tutorial de vídeo que muestra algunas de las características que se tratan aquí en [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depuración con Web Development Helper

Aunque Visual Studio 2008 (y Visual Web Developer Express 2008) son muy compatibles con las herramientas de depuración, hay opciones adicionales que también se pueden usar lo que son más ligeros. Una de las herramientas más recientes que se libere es la Web Development Helper. Microsoft Nikhil Kothari (uno de los principales arquitectos de AJAX de ASP.NET en Microsoft) escribió este excelente herramienta que puede realizar diversas tareas de depuración simples para ver los mensajes de solicitud y respuesta HTTP. Web Development Helper puede descargarse en [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Aplicación auxiliar de desarrollo de Web se puede usar directamente dentro de Internet Explorer, lo que resulta fácil de usar. Se inicia seleccionando herramientas Web Development Helper en el menú de Internet Explorer. Se abrirá la herramienta en la parte inferior del explorador que es bueno, ya que no tenga que salir del explorador para llevar a cabo varias tareas como el registro de mensajes de solicitud y respuesta HTTP. Figura 4 muestra el aspecto de Web Development Helper en acción.


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: Web Development Helper ([haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Aplicación auxiliar de desarrollo de Web no es una herramienta que se va a usar para recorrer el código línea por línea como con Visual Studio 2008. Sin embargo, se puede usar para ver el resultado del seguimiento, fácilmente evaluar las variables en una secuencia de comandos o explorar datos que se están dentro de un objeto JSON. También es muy útil para ver los datos que se pasan a y desde una página AJAX de ASP.NET y un servidor.

Una vez abierto en Internet Explorer Web Development Helper, depuración de scripts debe habilitarse mediante la selección habilitar secuencia de comandos de depuración de scripts en el menú de la aplicación auxiliar de desarrollo Web tal como se muestra en la figura 4. Esto permite que la herramienta para interceptar los errores que se producen cuando se ejecuta una página. También permite un acceso sencillo a los mensajes de seguimiento que se generan en la página. Para ver información de seguimiento o ejecutar secuencias de comandos para probar diferentes funciones dentro de una página, seleccione Mostrar secuencias de comandos de consola de secuencia de comandos en el menú de Web Development Helper. Esto proporciona acceso a una ventana de comandos y una simple ventana Inmediato.

*Visualización de los mensajes de seguimiento y datos de objetos JSON*

La ventana Inmediato puede usarse para ejecutar comandos de script o incluso cargar o guardar los scripts que se usan para probar las distintas funciones en una página. La ventana de comandos muestra mensajes de seguimiento o de depuración escritos por la página que se está viendo. Listado 2 muestra cómo escribir un mensaje de seguimiento mediante Debug.writeln (función) de Internet Explorer.

**Listado 2. Escribe un mensaje de seguimiento del lado cliente mediante la clase Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Si la propiedad LastName contiene un valor de Doe, Web Development Helper mostrará el mensaje "el nombre de persona: Doe" en la ventana de comandos de la consola de secuencia de comandos (suponiendo que se ha habilitado la depuración). Web Development Helper también agrega un objeto de nivel superior debugService en las páginas que pueden usarse para escribir información de seguimiento o ver el contenido de objetos JSON. Listado 3 muestra un ejemplo del uso de la función de seguimiento de la clase debugService.

**Listado 3. Mediante la clase de debugService de Web Development Helper escribir un mensaje de seguimiento.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una característica interesante de la clase debugService es que funciona incluso si no está habilitada la depuración en Internet Explorer, lo que facilita siempre cuando se está ejecutando Web Development Helper, tener acceso a datos de seguimiento. Cuando la herramienta no se emplean para depurar una página, se omitirán las instrucciones de seguimiento, ya que la llamada a window.debugService devolverá false.

La clase debugService también permite que los datos de objeto JSON para verse mediante la ventana del inspector de Web Development Helper. Listado 4 crea un objeto JSON simple que contiene los datos de la persona. Una vez creado el objeto, se realiza una llamada a la debugService la clase inspeccionar la función para permitir que el objeto JSON debe inspeccionar visualmente.

**Listado 4. Mediante la función debugService.inspect para ver los datos del objeto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Una llamada a la función GetPerson() en la página o a través de la ventana Inmediato dará como resultado en la ventana de cuadro de diálogo del Inspector de objetos que aparecen como se muestra en la figura 5. Se pueden cambiar dinámicamente las propiedades dentro del objeto resaltándolos, cambiar el valor mostrado en el cuadro de texto de valor y, a continuación, haga clic en el vínculo de actualización. Con el Inspector de objeto resulta sencilla ver datos de objetos JSON y experimentar con la aplicación de valores diferentes a las propiedades.

*Errores de depuración*

Además de permitir que los datos de seguimiento y que se muestren los objetos JSON, Web Development helper también puede ayudar a depurar errores en una página. Si se produce un error, se le indicará que continuar con la siguiente línea de código o depurar el script (consulte la figura 6). La ventana de cuadro de diálogo de Error de secuencia de comandos muestra la llamada completa de la pila, así como los números de línea para que pueda identificar fácilmente donde están los problemas dentro de una secuencia de comandos.


[![Uso de la ventana del Inspector de objetos para ver un objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: uso de la ventana del Inspector de objeto para ver un objeto JSON.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Seleccionar la opción de depuración permite ejecutar instrucciones de script directamente en la ventana Inmediato del Web Development Helper para ver el valor de variables, escribir objetos JSON y más. Si se vuelve a ejecutar la misma acción que desencadenó el error y está disponible en el equipo de Visual Studio 2008, se le pedirá que inicie una sesión de depuración para que puede recorrer el código línea por línea como se describe en la sección anterior.


[![Cuadro de diálogo de Error de Script de la aplicación auxiliar de desarrollo Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: cuadro de diálogo de Error del Web Development Helper Script ([haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Inspección de mensajes de solicitud y respuesta*

Durante la depuración de páginas de ASP.NET AJAX a menudo resulta útil ver los mensajes de solicitud y respuesta enviados entre una página y el servidor. Ver el contenido dentro de mensajes permite ver si se pasan los datos adecuados, así como el tamaño de los mensajes. Web Development Helper proporciona una excelente característica de registrador de mensajes HTTP que facilita la tarea ver los datos como texto sin formato o en un formato más legible.

Para ver los mensajes de solicitud y respuesta de AJAX de ASP.NET, el registrador HTTP debe habilitarse seleccionando HTTP habilitar el registro HTTP en el menú Web Development Helper. Una vez habilitada, se pueden ver todos los mensajes enviados desde la página actual en el Visor de registros HTTP que se puede acceder mediante la selección de los registros de HTTP muestran HTTP.

Aunque ver el texto sin formato enviado en cada mensaje de solicitud/respuesta es ciertamente útil (y una opción en la Web Development Helper), a menudo es más fácil ver los datos del mensaje en un formato más gráfico. Una vez que se ha habilitado el registro de HTTP y se han registrado mensajes, los datos del mensaje pueden verse haciendo doble clic en el mensaje en el Visor de registros HTTP. Esto le permite ver todos los encabezados asociados con un mensaje, así como el mensaje real contenido. Figura 7 muestra un ejemplo de un mensaje de solicitud y el mensaje de respuesta que se ve en la ventana del Visor de registro de HTTP.


[![Mediante el Visor de registros de HTTP para ver los datos de mensaje de solicitud y respuesta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: mediante el Visor de registros de HTTP para ver los datos de mensaje de solicitud y respuesta.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


El Visor de registros de HTTP automáticamente analiza los objetos JSON y los muestra con una vista de árbol, facilitando la rápida y fácil de ver datos de la propiedad del objeto. Cuando se utiliza un UpdatePanel en una página AJAX de ASP.NET, el Visor se desborda cada parte del mensaje en partes individuales como se muestra en la figura 8. Esta es una característica excelente que facilita mucho más fácil ver y comprender lo que está en el mensaje en comparación con la visualización de los datos sin procesar del mensaje.


[![Un mensaje de respuesta de UpdatePanel que ver con el Visor de registro de HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: mensaje de respuesta de un UpdatePanel ve utilizando el Visor de registro de HTTP.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Hay otras herramientas que pueden usarse para ver los mensajes de solicitud y respuesta además Web Development Helper. Otra buena opción es Fiddler que está disponible gratis en [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Aunque no se tratarán Fiddler aquí, también es una buena opción cuando necesite inspeccionar minuciosamente datos y los encabezados del mensaje.

## <a name="debugging-with-firefox-and-firebug"></a>Depuración con Firebug y Firefox

Aunque Internet Explorer sigue siendo el explorador más usado, otros exploradores como Firefox han vuelto bastante populares y se usan más y más. Como resultado, deseará ver y depurar las páginas de AJAX de ASP.NET en Firefox, así como Internet Explorer para asegurarse de que sus aplicaciones funcionen correctamente. Aunque Firefox no se enlazan directamente en Visual Studio 2008 para la depuración, tiene una extensión denominada a Firebug que se puede usar para depurar páginas. Firebug puede descargarse de forma gratuita, vaya a [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug ofrece un entorno de depuración completa que puede usarse para recorrer el código línea por línea, tener acceso a todos los scripts usados dentro de una página, ver estructuras de DOM, mostrar los estilos CSS e incluso seguimiento de eventos que se producen en una página. Una vez instalado, se puede tener acceso a Firebug seleccionando herramientas Firebug abierto Firebug en el menú de Firefox. Similar a Web Development Helper, Firebug se usa directamente en el explorador, aunque también se pueden usar como una aplicación independiente.

Una vez que se está ejecutando Firebug, pueden establecer puntos de interrupción en cualquier línea de un archivo JavaScript si el script está incrustado en una página o no. Para establecer un punto de interrupción, cargar la página adecuada que desea depurar en Firefox. Una vez que se carga la página, seleccione la secuencia de comandos para la depuración desde la lista desplegable de Firebug secuencias de comandos. Se mostrarán todos los scripts usados por la página. Se establece un punto de interrupción haciendo clic en gris bandeja área del Firebug en la línea donde el punto de interrupción debe ir debe tal como se haría en Visual Studio 2008.

Una vez que se ha establecido un punto de interrupción en Firebug puede realizar la acción necesaria para ejecutar el script que necesita que se desea depurar como hacer clic en un botón o actualizar el explorador para desencadenar el evento onLoad. La ejecución se detendrá automáticamente en la línea que contiene el punto de interrupción. Figura 9 muestra un ejemplo de un punto de interrupción que se ha desencadenado en Firebug.


[![Control de puntos de interrupción en Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: control de puntos de interrupción en Firebug.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Una vez que se alcanza un punto de interrupción paso a paso, paso a paso o salir de código mediante los botones de flecha. Paso a paso a través del código, las variables de secuencia de comandos se muestran en la parte derecha del depurador que permite ver los valores y profundizar en los objetos. Firebug también incluye una lista desplegable de pila de llamadas para ver los pasos de ejecución del script que condujeron a la línea actual que se está depura.

Firebug también incluye una ventana de consola que puede usarse para probar las instrucciones de la secuencia de comandos diferentes, evaluar variables y ver el resultado del seguimiento. Se tiene acceso haciendo clic en la pestaña de la consola en la parte superior de la ventana Firebug. La página que se está depura también puede "inspeccionar" para ver su estructura DOM y contenido, haga clic en la pestaña de inspeccionar. Como se resaltará el mouse sobre los diferentes elementos de DOM que se muestra en la ventana del inspector de la parte correspondiente de la página lo que facilita ver dónde se utiliza el elemento en la página. "Live" para experimentar con la aplicación de diferentes anchos, estilos, etc. a un elemento, se pueden cambiar los valores de atributo asociados con un elemento determinado. Esta es una característica agradable que le evita tener que cambiar constantemente entre el editor de código fuente y el explorador Firefox para ver el efecto de los cambios una página simple.

Figura 10 muestra un ejemplo del uso del inspector de DOM para buscar un cuadro de texto denominado txtCountry en la página. El inspector Firebug también puede utilizarse para ver los estilos CSS que se usan en una página, así como eventos que se producen como el seguimiento de movimientos del mouse, clics de botón, además de más.


[![Usar el inspector de DOM del Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: inspector de DOM del uso de Firebug.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug ofrece una manera ligera de depurar rápidamente una página directamente en Firefox, así como una herramienta excelente para inspeccionar los distintos elementos dentro de la página.

## <a name="debugging-support-in-aspnet-ajax"></a>Compatibilidad con la depuración en ASP.NET AJAX

La biblioteca de AJAX de ASP.NET incluye muchas clases diferentes que se pueden usar para simplificar el proceso de agregar capacidades de AJAX en una página Web. Puede utilizar estas clases para buscar elementos dentro de una página y manipularlos, agregar nuevos controles, llamar a servicios Web y controlar los eventos. La biblioteca ASP.NET AJAX también contiene clases que se pueden usar para mejorar el proceso de depuración de páginas. En esta sección muestra una introducción a la clase Sys.Debug y vea cómo puede utilizarse en aplicaciones.

*Uso de la clase Sys.Debug*

La clase Sys.Debug (una clase JavaScript que se encuentra en el espacio de nombres Sys) puede usarse para realizar varias funciones diferentes, como escribir la salida de seguimiento, realizar aserciones de código y forzar el código de error para que se puede depurar. Se utiliza ampliamente en los archivos de depuración de la biblioteca AJAX de ASP.NET (se instala de forma predeterminada en C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) para realizar pruebas condicionales) llama a las aserciones) asegúrese de que los parámetros se pasan correctamente para las funciones, que los objetos contienen los datos esperados y escribir instrucciones de seguimiento.

La clase Sys.Debug expone varias funciones diferentes que pueden usarse para controlar el seguimiento, las aserciones de código o errores, como se muestra en la tabla 1.

**Tabla 1. Funciones de clase Sys.Debug.**

| **Nombre de la función** | **Descripción** |
| --- | --- |
| Assert (condición, mensaje, mostrarLlamador) | Declara que el parámetro de la condición es true. Si la condición que se está probando es false, se utilizará un cuadro de mensaje para mostrar el valor del parámetro de mensaje. Si el parámetro mostrarLlamador es true, el método también muestra información sobre el llamador. |
| clearTrace() | Borra los resultados de las instrucciones de las operaciones de seguimiento. |
| Fail(Message) | Hace que el programa detener la ejecución e interrumpir el depurador. El parámetro de mensaje puede utilizarse para proporcionar un motivo del error. |
| Trace(Message) | Escribe el parámetro de mensaje en la salida del seguimiento. |
| traceDump (object, nombre) | Devuelve los datos de un objeto en un formato legible. El parámetro name puede utilizarse para proporcionar una etiqueta para el volcado de seguimiento. De forma predeterminada, se escribirá cualquier subobjeto dentro del objeto que se va a volcar. |

Seguimiento del lado cliente se puede usar en la misma manera que la funcionalidad de seguimiento disponible en ASP.NET. Permite que los mensajes distintos a verse fácilmente sin interrumpir el flujo de la aplicación. Listado 5 muestra un ejemplo del uso de la función Sys.Debug.trace para escribir en el registro de seguimiento. Esta función simplemente toma el mensaje que se debe escribir como un parámetro.

**Listado 5. Mediante la función Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Si ejecuta el código que se muestra en el listado 5 no verá ningún resultado de seguimiento en la página. La única forma de verlo es usar una ventana de consola disponible en Visual Studio. NET, Web Development Helper o Firebug. Si desea ver el resultado de seguimiento en la página, a continuación, deberá agregar una etiqueta de área de texto y asígnele un identificador de elemento TraceConsole tal como se muestra a continuación:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Cualquier instrucción Sys.Debug.trace en la página se escribirán en TraceConsole TextArea.

En los casos donde desee ver los datos contenidos en un objeto JSON puede usar la función de la clase Sys.Debug traceDump. Esta función toma dos parámetros, incluidos los objetos que se deben volcar en la consola de seguimiento y un nombre que se puede usar para identificar el objeto en la salida del seguimiento. Listado 6 muestra un ejemplo del uso de la función traceDump.

**Listado 6. Mediante la función Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figura 11 muestra la salida de la llamada a la función Sys.Debug.traceDump. Tenga en cuenta que además de escribir los datos del objeto Person, también escribe la dirección datos del objeto-sub.

Además de seguimiento, la clase Sys.Debug también puede utilizarse para realizar aserciones de código. Las aserciones se usan para probar que se cumplen determinadas condiciones mientras se está ejecutando una aplicación. La versión de depuración de las secuencias de comandos de la biblioteca AJAX de ASP.NET contiene varias instrucciones para probar una variedad de condiciones de assert.

Listado 7 muestra un ejemplo del uso de la función Sys.Debug.assert para probar una condición. El código comprueba si el objeto de dirección es null antes de actualizar un objeto Person.


[![Salida de la función Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: salida de la función Sys.Debug.traceDump.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Listado 7. Mediante la función debug.assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tres parámetros se pasan como la condición para evaluar el mensaje para mostrar si la aserción devuelve false y si se debe mostrar información sobre el llamador. En casos donde se produce un error en una aserción, se mostrará el mensaje, así como información del llamador si el tercer parámetro era true. Figura 12 muestra un ejemplo del cuadro de diálogo de error que aparece si se produce un error en la aserción que se muestra en la lista 7.

La función final para cubrir es Sys.Debug.fail. Cuando desea forzar un error en una línea determinada en una secuencia de comandos de código puede agregar una llamada Sys.Debug.fail en lugar de la instrucción de depurador que se suelen usar en aplicaciones de JavaScript. La función Sys.Debug.fail acepta un parámetro de cadena único que representa el motivo del error, tal como se muestra a continuación:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Un mensaje de error Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert un mensaje de error.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Cuando se encuentra una instrucción Sys.Debug.fail mientras se está ejecutando una secuencia de comandos, se mostrará el valor del parámetro del mensaje en la consola de una aplicación de depuración, como Visual Studio 2008 y se le pedirá para depurar la aplicación. Un caso donde puede ser bastante útil es cuando no se puede establecer un punto de interrupción con Visual Studio 2008 en un script en línea, pero desea que el código para detenerse en línea determinada, por lo que puede inspeccionar el valor de variables.

*Descripción ScriptMode propiedad del Control ScriptManager*

La biblioteca ASP.NET AJAX incluye depuración y de lanzamiento de las versiones de script que se instalan en C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 de forma predeterminada. Los scripts de depuración son fáciles de leer, bien formateada y disponen de varias llamadas Sys.Debug.assert repartidos por todo ellos mientras que los scripts de lanzamiento tienen espacios en blanco secciona y use la clase Sys.Debug con moderación para minimizar su tamaño total.

El control ScriptManager que se agregan a las páginas de AJAX de ASP.NET lee el atributo de depuración del elemento de compilación en el archivo web.config para determinar qué versiones de scripts de biblioteca para cargar. Sin embargo, puede controlar si debug o release scripts están cargados (scripts de biblioteca y sus propios scripts personalizados) cambiando la propiedad ScriptMode. ScriptMode acepta una enumeración ScriptMode cuyos miembros incluyen automáticamente, Debug, Release y heredar.

ScriptMode valor predeterminado es Auto, lo que significa que el control ScriptManager comprobará el atributo de depuración en el archivo web.config. Cuando es false depuración ScriptManager cargará la versión de lanzamiento de scripts de la biblioteca de AJAX de ASP.NET. Cuando se cumple la depuración se cargará la versión de depuración de las secuencias de comandos. Cambiar la propiedad ScriptMode para liberar o depurar forzará el ScriptManager para cargar las secuencias de comandos adecuados, independientemente de qué valor tiene el atributo de depuración en el archivo web.config. Listado 8 muestra un ejemplo del uso del control ScriptManager para cargar los scripts de depuración de la biblioteca AJAX de ASP.NET.

**Listado 8. Cargando scripts de depuración mediante el control ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

También puede cargar diferentes versiones (debug o release) de sus propios scripts personalizados mediante el uso de propiedad de las secuencias de comandos de ScriptManager junto con el componente ScriptReference tal como se muestra en el listado 9.

**Listado 9. Cargando scripts personalizados mediante el control ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Si está cargando scripts personalizados mediante el componente ScriptReference debe notificar el ScriptManager cuando el script ha terminado de cargarse agregando el código siguiente en la parte inferior de la secuencia de comandos:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

El código que se muestra en el listado 9 indica el ScriptManager para buscar una versión de depuración de la secuencia de comandos de la persona que su aspecto automáticamente para Person.debug.js en lugar de Person.js. Si no se encuentra el archivo de Person.debug.js, se generará un error.

En casos donde desea que una depuración o versión de lanzamiento de un script personalizado que se cargue en función del valor de la propiedad ScriptMode establecidas en el control ScriptManager, puede establecer la propiedad ScriptMode del control ScriptReference a heredar. Esto hará que la versión correcta del script personalizado que se cargue en función de propiedad de ScriptMode de ScriptManager tal como se muestra en la lista de 10. Porque la propiedad ScriptMode del control ScriptManager está establecida en la depuración, la secuencia de comandos Person.debug.js se cargarán y se utiliza en la página.

**Listado 10. Heredar el ScriptMode de ScriptManager para scripts personalizados.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Mediante el uso de la propiedad ScriptMode adecuadamente puede depurar aplicaciones más fácilmente y simplificar el proceso general. Los scripts de lanzamiento de la biblioteca AJAX de ASP.NET son bastante difíciles de paso a paso y se han leído desde el formato de código se ha quitado mientras se da formato a los scripts de depuración específicamente para fines de depuración.

## <a name="conclusion"></a>Conclusión

La tecnología de Microsoft ASP.NET AJAX proporciona una base sólida para la creación de aplicaciones habilitadas para AJAX que pueden mejorar la experiencia global del usuario final. Sin embargo, como con cualquier tecnología de programación, errores y otros problemas de la aplicación sin duda surgirán. Saber sobre las distintas opciones de depuración disponibles ahorrará gran cantidad de tiempo y el resultado en un producto más estable.

En este artículo aprendió un poco sobre varias técnicas diferentes para la depuración de páginas de ASP.NET AJAX, incluido Internet Explorer con Visual Studio 2008, Web Development Helper y Firebug. Estas herramientas pueden simplificar el proceso de depuración general, ya puede tener acceso a datos de la variable, recorrer el código línea por línea y ver las instrucciones de seguimiento. Además de las distintas herramientas de depuración descritas, también ha visto cómo Sys.Debug (clase) de la biblioteca AJAX de ASP.NET puede utilizarse en una aplicación y cómo se puede usar la clase ScriptManager para cargar depuración o lanzamiento de versiones de las secuencias de comandos.

## <a name="bio"></a>Biografía

Dan Wahlin (Microsoft Most Valuable Professional para ASP.NET y servicios Web XML) es .NET development instructor y arquitectura consultor en formación técnica de interfaz ([www.interfacett.com)](http://www.interfacett.com). Dan fundó el XML para el sitio Web de desarrolladores de ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se encuentra en la oficina del orador de INETA y es orador en varios congresos. Dan coautor DNA de Windows (Wrox), ASP.NET Professional: sugerencias, tutoriales y código (SAM), especialista en soluciones de ASP.NET 1.1, Professional ASP.NET 2.0 AJAX (Wrox), Hacks de MVP de ASP.NET 2.0 y XML creado para desarrolladores de ASP.NET (SAM). Cuando no está escribiendo código, artículos o libros, Dan disfruta de escribir y grabar música y jugar golf y baloncesto con su esposa e hijos.

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-web-services.md)
