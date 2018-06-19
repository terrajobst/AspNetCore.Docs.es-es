---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Descripción de funciones de depuración de AJAX de ASP.NET | Documentos de Microsoft
author: scottcate
description: La capacidad para depurar el código es una especialización que todo desarrollador debe tener en su arsenal independientemente de la tecnología que usa. Mientras que muchos desarrolladores son...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: f082e2206f5e691579670e42634f30b57e3b3593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890936"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Descripción de funciones de depuración de AJAX de ASP.NET
====================
por [Scott ndicar](https://github.com/scottcate)

[Descarga de PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La capacidad para depurar el código es una especialización que todo desarrollador debe tener en su arsenal independientemente de la tecnología que usa. Mientras que muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones ASP.NET que utilizan código de C# o VB.NET, algunas no están consciente de que también es muy útil para depurar el código de cliente, como JavaScript. También puede aplicarse el mismo tipo de técnicas que se utilizan para depurar aplicaciones de .NET para aplicaciones habilitadas para AJAX y más concretamente las aplicaciones ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Depurar aplicaciones de AJAX de ASP.NET

Dan Wahlin

La capacidad para depurar el código es una especialización que todo desarrollador debe tener en su arsenal independientemente de la tecnología que usa. Hace falta decir que descripción de las distintas opciones de depuración que están disponibles puede ahorrar una gran cantidad de tiempo en un proyecto y dolores de cabeza incluso unos. Mientras que muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones ASP.NET que utilizan código de C# o VB.NET, algunas no están consciente de que también es muy útil para depurar el código de cliente, como JavaScript. También puede aplicarse el mismo tipo de técnicas que se utilizan para depurar aplicaciones de .NET para aplicaciones habilitadas para AJAX y más concretamente las aplicaciones ASP.NET AJAX.

En este artículo verá cómo se pueden usar Visual Studio 2008 y otras herramientas para depurar aplicaciones de ASP.NET AJAX a encontrar rápidamente los errores y otros problemas. Esta discusión incluirá información sobre la habilitación de Internet Explorer 6 o posterior para depurar, mediante Visual Studio 2008 y el Explorador de la secuencia de comandos para recorrer el código, así como con otras herramientas disponibles como aplicación auxiliar de desarrollo Web. También aprenderá a depurar aplicaciones de ASP.NET AJAX en Firefox con que una extensión de nombre a Firebug que permite recorrer el código de JavaScript directamente en el explorador sin cualquier otra herramienta. Por último, se muestra una introducción a las clases en la biblioteca de AJAX de ASP.NET que puede ayudar a varias tareas de depuración como el seguimiento y las instrucciones de aserción de código.

Antes de intentar depurar páginas que ve en Internet Explorer, hay algunos pasos básicos que debe realizar para habilitar para la depuración. ¡Eche un vistazo a algunos requisitos de configuración básica que deben realizarse para empezar a trabajar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurar Internet Explorer para la depuración

Mayoría de los usuarios no está interesada en Ver problemas de JavaScript que se producen en un sitio Web con Internet Explorer ver. De hecho, el usuario promedio incluso no sabría qué hacer si vio un mensaje de error. Como resultado, las opciones de depuración están desactivadas de forma predeterminada en el explorador. Sin embargo, es muy sencillo activar la depuración y empezar a usar como desarrollar nuevas aplicaciones AJAX.

Para habilitar la funcionalidad de depuración, vaya a opciones de herramientas de Internet en el menú de Internet Explorer y seleccione la ficha Opciones avanzadas. En la sección de exploración de asegurarse de que los elementos siguientes están desactivados:

- Deshabilitar la depuración de scripts (Internet Explorer)
- Deshabilitar la depuración de scripts (otros)

Aunque no es obligatorio, si se intenta depurar una aplicación que probablemente deseará los errores de JavaScript en la página inmediatamente sea visible y obvio. Puede forzar que todos los errores que se muestre con un cuadro de mensaje activando la casilla de verificación "Mostrar una notificación sobre cada error de script". Aunque esto es una buena opción para activar al desarrollar una aplicación, rápidamente puede resultar molesto si simplemente está examinar otros sitios Web debido a que las posibilidades de que se produzcan errores de JavaScript son bastante buenas.

Figura 1 muestra qué Internet Explorer cuadro de diálogo avanzado debe tener un aspecto una vez se ha configurado correctamente para la depuración.


[![Configurar Internet Explorer para depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configurar Internet Explorer para la depuración.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Una vez que la depuración se ha activado, verá un nuevo elemento de menú aparece en el menú de vista denominado al depurador de scripts. Tiene dos opciones disponibles incluidas abierto e interrupción en la siguiente instrucción. Cuando se selecciona la apertura se le pedirá para depurar la página en Visual Studio 2008 (tenga en cuenta que Visual Web Developer Express también puede utilizarse para la depuración). Si ya se está ejecutando Visual Studio .NET puede usar esa instancia o para crear una nueva instancia. Cuando se selecciona la interrupción en la siguiente instrucción que se le pedirá para depurar la página cuando se ejecuta el código de JavaScript. Si el código de JavaScript se ejecuta en el evento onLoad de la página puede actualizar la página para desencadenar una sesión de depuración. Si el código de JavaScript se ejecuta después de que se hace clic en un botón, a continuación, el depurador se ejecutará inmediatamente después de que se hace clic en el botón.

> *> [!NOTE] Si está ejecutando en Windows Vista con Control de acceso de usuario (UAC) habilitado, y tiene Visual Studio 2008 configurado para ejecutarse como administrador, se producirá un error de Visual Studio adjuntar al proceso cuando se le pida para adjuntar. Para solucionar este problema, inicie Visual Studio en primer lugar y utilice esa instancia para depurar.*


Aunque la sección siguiente muestra cómo depurar una página AJAX de ASP.NET directamente desde dentro de Visual Studio 2008, con la opción del depurador de Script de Internet Explorer es útil cuando una página ya está abierta y que le gustaría inspeccionar más completa.

## <a name="debugging-with-visual-studio-2008"></a>Depuración con Visual Studio 2008

Visual Studio 2008 proporciona funcionalidad de depuración que los desarrolladores de todo el mundo se basan en todos los días a las aplicaciones de .NET de depuración. El depurador integrado permite recorrer el código, vista de datos de objetos, inspección para unas variables específicas, supervisar la pila de llamadas y muchas más. Además de la depuración de código de C# o VB.NET, el depurador también es útil para depurar aplicaciones de AJAX de ASP.NET y le permitirá recorrer el código línea por línea de JavaScript. Los detalles que siguen el foco de técnicas que pueden utilizar para depurar los archivos de script de cliente en lugar de proporcionar un discurso en todo el proceso de depuración de aplicaciones mediante Visual Studio 2008.

El proceso de depuración de una página en Visual Studio 2008 se puede iniciar de varias maneras diferentes. En primer lugar, puede usar la opción del depurador de Script de Internet Explorer se ha mencionado en la sección anterior. Esto funciona bien cuando una página ya está cargada en el explorador y que desea iniciar la depuración. Como alternativa, puede haga doble clic en una página .aspx en el Explorador de soluciones y seleccione Establecer como página de inicio en el menú. Si está acostumbrado a depurar páginas ASP.NET, a continuación, probablemente hecho esto antes. Una vez que se presiona F5 se puede depurar la página. Sin embargo, aunque generalmente puede establecer un punto de interrupción en cualquier lugar le gustaría en código de C# o VB.NET, que no siempre es el caso de JavaScript como verá a continuación.

*Embedded frente a Scripts externos*

El depurador de Visual Studio 2008 trata JavaScript incrustado en una página diferente a los archivos JavaScript externos. Con los archivos de script externo, puede abrir el archivo y establecer un punto de interrupción en cualquiera de las líneas que elija. Pueden establecer puntos de interrupción, haga clic en el área de bandeja de gris a la izquierda de la ventana del editor de código. Cuando JavaScript se incrusta directamente en una página mediante la `<script>` etiqueta, establecer un punto de interrupción, haga clic en el área gris bandeja no es una opción. Intenta establecer un punto de interrupción en una línea de script incrustado se producirá una advertencia que indica "No es una ubicación válida para un punto de interrupción".

Puede sortear este problema si mueve el código a un archivo .js externo y hacer referencia a él mediante el atributo src de la &lt;script&gt; etiqueta:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

¿Qué ocurre si mueve el código a un archivo externo no es una opción o requiere más trabajo merece la pena? Aunque no se puede establecer un punto de interrupción mediante el editor, puede agregar la instrucción de depurador directamente en el código donde desea iniciar la depuración. También puede usar la clase Sys.Debug disponible en la biblioteca de AJAX de ASP.NET para forzar para iniciar la depuración. Aprenderá más acerca de la clase Sys.Debug más adelante en este artículo.

Un ejemplo del uso del `debugger` palabra clave se muestra en la lista 1. Este ejemplo fuerza el depurador se interrumpa derecho antes de que se realiza una llamada a una función de actualización.

**Lista 1. Usa la palabra clave debugger para aplicar el depurador de Visual Studio .NET se interrumpa.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Una vez que se alcanza la instrucción de depurador que se pedirá a depurar la página mediante Visual Studio .NET y puede empezar a recorrer el código. Aunque esto, puede encontrar un problema con el acceso a archivos de script de biblioteca de AJAX de ASP.NET utilizados en la página así que vamos a echar un vistazo a la hora de usar Visual Studio. Explorador de scripts de NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usar ventanas de Visual Studio .NET para depurar

Una vez que se inicia una sesión de depuración y empezar a recorrer el código mediante la tecla F11 de manera predeterminada, puede encontrar el cuadro de diálogo de error que se muestra en consulte la figura 2, a menos que todos los archivos de script que se usan en la página están abierta y disponible para la depuración.


[![Cuadro de diálogo de error que se muestra cuando no hay código fuente está disponible para la depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: cuadro de diálogo de Error se muestra cuando no hay código fuente está disponible para la depuración.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Este cuadro de diálogo se muestra porque Visual Studio .NET no está seguro de cómo obtener el código de origen de algunas de las secuencias de comandos al que hace referencia la página. Aunque esto puede ser muy frustrante en primer lugar, hay una solución sencilla. Una vez que haya iniciado una sesión de depuración y alcanza un punto de interrupción, vaya a la ventana Explorador de scripts de Windows de depurar en el menú de Visual Studio 2008 o usar una combinación de teclas Ctrl + Alt + N.

> *> [!NOTE] Si no ve el menú del explorador de la secuencia de comandos enumerado, vaya a herramientas* *personalizar* *comandos del menú de Visual Studio. NET. Busque la entrada de depuración en la sección de categorías y haga clic en él para mostrar todas las entradas de menú disponibles. En la lista de comandos, desplácese hasta el Explorador de scripts y, a continuación, arrastre hacia arriba en la depuración* *menú de Windows se ha mencionado anteriormente. Esto hará que la entrada de menú del explorador de scripts disponibles cada vez que ejecute Visual Studio. NET.*


El Explorador de scripts puede usarse para ver todos los scripts que se utiliza en una página y abrirlos en el editor de código. Una vez abierto el Explorador de scripts, haga doble clic en la página .aspx que se está depurada para abrirlo en la ventana del editor de código. Realizar la misma acción para todos los demás scripts que se muestra en el Explorador de scripts. Una vez que todos los scripts están abiertos en la ventana de código, puede presionar F11 (y usar las otras teclas de aceleración de depuración) para recorrer el código. Figura 3 muestra un ejemplo del explorador de secuencia de comandos. Muestra el archivo actual que se está depurando (Demo.aspx), así como dos scripts personalizados y dos scripts insertados en la página de forma dinámica por el ScriptManager de ASP.NET AJAX.


[![El Explorador de Script proporciona un acceso sencillo a las secuencias de comandos que se utiliza en una página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. El Explorador de Script proporciona un acceso sencillo a las secuencias de comandos que se utiliza en una página.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Algunas de las windows también pueden usarse para proporcionar información útil ejecutar paso a paso a través del código en una página. Por ejemplo, puede utilizar la ventana variables locales para ver los valores de diferentes variables utilizadas en la página, la ventana Inmediato para evaluar variables específicas o condiciones y ver el resultado. También puede usar la ventana de salida para ver las instrucciones de seguimiento que se escribe utilizando la función Sys.Debug.trace (que se explicará más adelante en este artículo) o Debug.writeln (función) de Internet Explorer.

Paso a paso a través del código con el depurador puede pasar el mouse sobre las variables en el código para ver el valor que están asignados. Sin embargo, el depurador de script en ocasiones, no mostrará nada tal y como mueva el mouse sobre una variable de JavaScript determinada. Para ver el valor, resalte la instrucción o la variable que está intentando ver en la ventana del editor de código y, a continuación, mueva el mouse sobre él. Aunque esta técnica no funciona en todas las situaciones, muchas veces podrá ver el valor sin tener que buscar en una ventana diferente de depuración como la ventana variables locales.

Un tutorial de vídeo que muestra algunas de las características que se tratan aquí se puede ver en [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depuración con la aplicación auxiliar de desarrollo de Web

Aunque Visual Studio 2008 (y Visual Web Developer Express 2008) son muy compatibles con herramientas de depuración, hay opciones adicionales que pueden usarse también que son más ligera. Una de las herramientas más recientes que se liberen es la aplicación auxiliar de desarrollo de Web. Se ha escrito Nikhil Kothari de Microsoft (uno de los arquitectos de AJAX de ASP.NET claves de Microsoft) esta herramienta excelente que puede realizar diversas tareas de depuración simple para ver los mensajes de solicitud y respuesta HTTP. Aplicación auxiliar de desarrollo de Web se puede descargar en [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Aplicación auxiliar de desarrollo de Web se puede usar directamente en Internet Explorer que resulta cómodo para utilizarlo. Se inicia mediante la selección de herramientas Web Development Helper en el menú de Internet Explorer. Se abrirá la herramienta en la parte inferior del explorador que es bueno, ya que no es necesario dejar el explorador para realizar varias tareas, como el registro de mensajes de solicitud y respuesta HTTP. Figura 4 muestra el aspecto de Web Development Helper en acción.


[![Aplicación auxiliar de desarrollo de Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: Web Development Helper ([haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Aplicación auxiliar de desarrollo de Web no es una herramienta que se va a utilizar para recorrer el código línea por línea como con Visual Studio 2008. Sin embargo, se puede utilizar para ver el resultado del seguimiento, evaluar las variables en una secuencia de comandos o fácilmente explorar datos se están dentro de un objeto JSON. También es muy útil para ver los datos que se pasan a y desde una página AJAX de ASP.NET y un servidor.

Una vez Web Development Helper está abierto en Internet Explorer, la depuración de scripts debe habilitarse seleccionando Script habilitar la depuración Script en el menú de la aplicación auxiliar de desarrollo Web tal como se muestra en la figura 4. Esto permite que la herramienta para interceptar errores que se producen cuando se ejecuta una página. También permite un acceso sencillo a los mensajes de seguimiento que se generan en la página. Para ver información de seguimiento o ejecutar comandos de script para probar diferentes funciones dentro de una página, seleccione Mostrar secuencias de comandos de consola de secuencia de comandos en el menú de aplicación auxiliar de desarrollo Web. Esto proporciona acceso a una ventana de comandos y una ventana inmediata simple.

*Ver mensajes de seguimiento y datos de objetos JSON*

La ventana Inmediato puede utilizarse para ejecutar comandos de script o incluso cargar o guardar las secuencias de comandos que se usan para probar las distintas funciones en una página. La ventana de comandos muestra mensajes de seguimiento o depuración escritos en la página que se está visualiza. La lista 2 muestra cómo escribir un mensaje de seguimiento mediante Debug.writeln (función) de Internet Explorer.

**La lista 2. Escriba un mensaje de seguimiento del lado cliente mediante la clase Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Si la propiedad LastName contiene un valor de Doe, Web Development Helper mostrará el mensaje "nombre de persona: Doe" en la ventana de comandos de la consola de la secuencia de comandos (suponiendo que se ha habilitado la depuración). Web Development Helper también agrega un objeto de nivel superior debugService en las páginas que pueden usarse para escribir información de seguimiento o ver el contenido de objetos JSON. El listado 3 muestra un ejemplo del uso de la función de seguimiento de la clase debugService.

**El listado 3. Utilizando la clase de debugService del Ayudante de Web desarrollo para escribir un mensaje de seguimiento.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una característica interesante de la clase debugService es que funciona incluso si no está habilitada la depuración en Internet Explorer, lo que facilita siempre obtener acceso a los datos de seguimiento cuando se ejecuta la aplicación auxiliar de desarrollo Web. Cuando la herramienta no se usa para depurar una página, se omitirán las instrucciones de seguimiento ya que la llamada a window.debugService devolverá false.

La clase debugService también permite que los datos de objeto JSON para verse mediante la ventana del inspector del Ayudante de desarrollo Web. Listado 4, crea un objeto JSON simple que contiene datos de la persona. Una vez creado el objeto, se realiza una llamada a la debugService la clase inspeccionar la función para permitir que el objeto JSON que se va a inspeccionar visualmente.

**Listado 4. Usar la función de debugService.inspect para ver los datos de objeto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Llamar a la función GetPerson() en la página o a través de la ventana Inmediato dará como resultado en la ventana de cuadro de diálogo del Inspector de objetos que aparecen como se muestra en la figura 5. Propiedades dentro del objeto se pueden cambiar dinámicamente resaltándolos, cambiar el valor mostrado en el cuadro de texto del valor y, a continuación, haga clic en el vínculo de actualización. Con el Inspector de objeto facilita ver datos de objeto JSON y experimentar con la aplicación de valores diferentes a las propiedades.

*Errores de depuración*

Además de permitir que los datos de seguimiento y que se muestren los objetos JSON, aplicación auxiliar de desarrollo Web también ayuda a depurar errores en una página. Si se produce un error, se le pedirá para continuar con la siguiente línea de código o la secuencia de comandos de depuración (consulte la figura 6). La ventana de cuadro de diálogo de Error de secuencia de comandos muestra la llamada completa de la pila, así como los números de línea para que pueda identificar fácilmente donde están los problemas dentro de una secuencia de comandos.


[![Usar la ventana de Inspector de objetos para ver un objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: usar la ventana de Inspector de objetos para ver un objeto JSON.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Al seleccionar la opción de depuración le permite ejecutar las instrucciones de la secuencia de comandos directamente en la ventana Inmediato del Ayudante de desarrollo de Web para ver el valor de variables, escribir objetos JSON o más. Si se vuelve a ejecutar la misma acción que desencadenó el error y Visual Studio 2008 está disponible en el equipo, se le pedirá que inicie una sesión de depuración para que puede recorrer el código línea por línea como se describe en la sección anterior.


[![Cuadro de diálogo de Error de secuencia de comandos del Ayudante de desarrollo Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: cuadro de diálogo de Error de secuencia de comandos del Ayudante de Web Development ([haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Inspeccionar mensajes de solicitud y respuesta*

Durante la depuración de páginas de ASP.NET AJAX a menudo resulta útil ver los mensajes de solicitud y respuesta enviados entre una página y el servidor. Ver el contenido de mensajes permite ver si se pasan los datos apropiados, así como el tamaño de los mensajes. Aplicación auxiliar de desarrollo Web proporciona una característica excelente de registrador de mensajes HTTP que hace más fácil ver los datos como texto sin formato o en un formato más legible.

Para ver los mensajes de solicitud y respuesta de AJAX de ASP.NET, debe habilitarse el registrador HTTP, seleccione HTTP habilitar el registro HTTP en el menú de aplicación auxiliar de desarrollo Web. Una vez habilitada, todos los mensajes enviados desde la página actual pueden verse en el Visor de registros HTTP que se puede acceder mediante la selección HTTP mostrar registros de HTTP.

Aunque ver el texto sin formato que se envía en cada mensaje de solicitud/respuesta es ciertamente útil (y una opción en el Asistente para desarrollo Web), a menudo resulta más fácil ver los datos del mensaje en un formato gráfico más. Una vez que se ha habilitado el registro de HTTP y el mensaje se ha registrado, los datos de mensaje pueden verse haciendo doble clic en el mensaje en el Visor de registros HTTP. Esto le permite ver todos los encabezados asociados con un mensaje, así como el mensaje real contenido. Figura 7 muestra un ejemplo de un mensaje de solicitud y el mensaje de respuesta que se ve en la ventana Visor de registro de HTTP.


[![Uso del Visor de registro de HTTP para ver los datos de mensaje de solicitud y respuesta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: uso del Visor de registro de HTTP para ver los datos de mensaje de solicitud y respuesta.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


El Visor de registro HTTP automáticamente analiza los objetos JSON y lo muestra con una vista de árbol para convertirla rápida y sencilla ver los datos de propiedad del objeto. Cuando se usa un UpdatePanel en una página AJAX de ASP.NET, el Visor se interrumpe cada parte del mensaje en partes individuales tal como se muestra en la figura 8. Se trata de una característica excelente que resulta más fácil ver y comprender lo que aparece en el mensaje en comparación con la visualización de los datos sin procesar del mensaje.


[![Un mensaje de respuesta de UpdatePanel ver utilizando el Visor de registro de HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: mensaje de respuesta de un UpdatePanel ve utilizando el Visor de registro de HTTP.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Hay varias otras herramientas que pueden utilizar para ver los mensajes de solicitud y respuesta además de la aplicación auxiliar de desarrollo Web. Otra buena opción es Fiddler que está disponible de forma gratuita en [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Aunque aquí no se analizará Fiddler, también es una buena opción cuando necesite inspeccionar exhaustivamente los datos y encabezados del mensaje.

## <a name="debugging-with-firefox-and-firebug"></a>Depuración con Firefox y Firebug

Mientras que Internet Explorer sigue siendo el explorador utilizado más comúnmente, otros exploradores como Firefox han vuelto bastante populares y se usan cada vez más. Como resultado, deseará ver y depurar las páginas de ASP.NET AJAX en Firefox, así como Internet Explorer para asegurarse de que las aplicaciones funcionan correctamente. Aunque Firefox no se enlazan directamente en Visual Studio 2008 para la depuración, tiene una extensión denominada a Firebug que puede utilizarse para depurar páginas. Firebug puede descargarse de forma gratuita, vaya a [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug proporciona un entorno de depuración completa que puede usarse para recorrer el código línea por línea, tener acceso a todos los scripts que se usa dentro de una página, ver estructuras de DOM, mostrar estilos CSS e incluso eventos de seguimiento que se producen en una página. Una vez instalado, se puede tener acceso a Firebug seleccionando herramientas Firebug abierto Firebug en el menú de Firefox. Como aplicación auxiliar de desarrollo Web, Firebug se utiliza directamente en el explorador, aunque también se puede usar como una aplicación independiente.

Una vez que se está ejecutando Firebug, pueden establecer puntos de interrupción en cualquier línea de un archivo JavaScript si el script está incrustado en una página o no. Para establecer un punto de interrupción, cargar la página apropiada que le gustaría depurar en Firefox. Una vez que se carga la página, seleccione la secuencia de comandos para depurar desde la lista desplegable de Firebug secuencias de comandos. Se mostrarán todos los scripts usados por la página. Un punto de interrupción se establece haciendo clic en el área de bandeja gris del Firebug en la línea donde el punto de interrupción debe ir debe como lo haría en Visual Studio 2008.

Una vez que se ha establecido un punto de interrupción en Firebug puede realizar la acción necesaria para ejecutar el script que tiene que ser depurada como hacer clic en un botón o actualizar el explorador para desencadenar el evento onLoad. La ejecución se detendrá automáticamente en la línea que contiene el punto de interrupción. Ilustración 9 se muestra un ejemplo de un punto de interrupción que se ha desencadenado en Firebug.


[![Control de puntos de interrupción en Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: control de puntos de interrupción en Firebug.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Una vez que se visita un punto de interrupción puede depurar paso a paso, paso por procedimientos o paso a paso de código mediante los botones de flecha. Paso a paso a través del código, las variables de secuencia de comandos se muestran en la parte derecha del depurador que permite ver los valores y exploración en profundidad en objetos. Firebug también incluye una lista de desplegable de la pila de llamadas para ver los pasos de ejecución de la secuencia de comandos que condujeron a la línea actual que se está depura.

Firebug también incluye una ventana de consola que puede usarse para probar las instrucciones de script diferente, evaluar las variables y ver resultados de seguimiento. Se tiene acceso haciendo clic en la pestaña de la consola en la parte superior de la ventana de Firebug. La página que se está depura también puede "inspeccionar" para ver su estructura de DOM y el contenido, haga clic en la pestaña inspeccionar. Como se resaltarán mouse (ratón) sobre los distintos elementos DOM que se muestra en la ventana del inspector de la parte correspondiente de la página lo que facilita ver dónde se utiliza el elemento en la página. Los valores de atributo asociados a un elemento determinado se pueden cambiar "live" para experimentar con la aplicación de diferentes anchos, estilos, etc. a un elemento. Se trata de una característica interesante que le evita tener que cambiar constantemente entre el editor de código fuente y el explorador Firefox para ver cómo simple cambios efecto una página.

Figura 10 muestra un ejemplo del uso del inspector de DOM para buscar un cuadro de texto denominado txtCountry en la página. El inspector Firebug también puede utilizarse para ver los estilos CSS que se usa en una página, así como los eventos que se producen como el seguimiento de los movimientos del mouse, clics de botón o más.


[![Con el inspector de DOM del Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: inspector de DOM del uso de Firebug.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug proporciona una manera ligera para depurar rápidamente una página directamente en Firefox, así como una herramienta excelente para inspeccionar los distintos elementos dentro de la página.

## <a name="debugging-support-in-aspnet-ajax"></a>Compatibilidad con la depuración en ASP.NET AJAX

La biblioteca de AJAX de ASP.NET incluye muchas clases diferentes que pueden usarse para simplificar el proceso de agregar capacidades de AJAX en una página Web. Puede utilizar estas clases para buscar elementos dentro de una página y manipularlos, agregar nuevos controles, llamar a servicios Web y controlar los eventos. La biblioteca ASP.NET AJAX también contiene clases que pueden usarse para mejorar el proceso de depuración de páginas. En esta sección se muestra una introducción a la clase Sys.Debug y vea cómo se puede usar en aplicaciones.

*Uso de la clase Sys.Debug*

La clase Sys.Debug (una clase de JavaScript ubicada en el espacio de nombres Sys) puede usarse para realizar varias acciones diferentes, como escribir los resultados de seguimiento, realizar aserciones de código y forzar el código de error para que se puede depurar. Se utiliza mucho en archivos de depuración de la biblioteca de AJAX de ASP.NET (que se instala en C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 de forma predeterminada) para realizar pruebas condicionales ( llama a las aserciones) asegúrese de que se pasan los parámetros correctamente para las funciones, que los objetos contienen los datos esperados y escribir instrucciones de seguimiento.

La clase Sys.Debug expone varias funciones diferentes que pueden usarse para controlar el seguimiento, las aserciones de código o errores, como se muestra en la tabla 1.

**Tabla 1. Funciones de clase Sys.Debug.**

| **Nombre de la función** | **Descripción** |
| --- | --- |
| Assert (condición, mensaje, mostrarLlamador) | Afirma que el parámetro de condición es true. Si la condición que se está probando es false, se utilizará un cuadro de mensaje para mostrar el valor de parámetro de mensaje. Si el parámetro mostrarLlamador es true, el método también muestra información sobre el llamador. |
| clearTrace() | Borra los resultados de las instrucciones de operaciones de seguimiento. |
| Fail(Message) | Hace que el programa detener la ejecución y el depurador. El parámetro de mensaje puede utilizarse para proporcionar un motivo del error. |
| Trace(Message) | Escribe el parámetro de mensaje en el resultado del seguimiento. |
| traceDump (objeto, nombre) | Genera datos de un objeto en un formato legible. El parámetro name puede utilizarse para proporcionar una etiqueta para el volcado de seguimiento. Los objetos secundarios dentro del objeto que se va a volcar se escribirá de forma predeterminada. |

Seguimiento del lado cliente puede usarse de la misma manera como la funcionalidad de seguimiento disponible en ASP.NET. Permite que los distintos mensajes para identificar fácilmente sin interrumpir el flujo de la aplicación. Listado 5 muestra un ejemplo del uso de la función Sys.Debug.trace para escribir en el registro de seguimiento. Esta función simplemente toma el mensaje que se debe escribir como un parámetro.

**Listado 5. Con la función Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Si ejecuta el código que se muestra en el listado 5 no verá ningún resultado de seguimiento en la página. La única manera de verlo es usar una ventana de consola disponible en Visual Studio. NET, Web Development Helper o Firebug. Si desea ver los resultados de seguimiento en la página, a continuación, se deberá agregar una etiqueta TextArea y asígnele un identificador de elemento TraceConsole tal y como se muestra a continuación:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Las instrucciones de Sys.Debug.trace en la página se escribirán en TraceConsole TextArea.

En casos donde desea ver los datos contenidos dentro de un objeto JSON puede usar la función de la clase Sys.Debug traceDump. Esta función toma dos parámetros, incluidos el objeto que debe realizar el volcado en la consola de seguimiento y un nombre que puede utilizarse para identificar el objeto en el resultado del seguimiento. Listado 6 muestra un ejemplo del uso de la función traceDump.

**Listado 6. Con la función Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figura 11 muestra el resultado de la llamada a la función Sys.Debug.traceDump. Tenga en cuenta que además de escribir los datos del objeto de la persona, también escribe la dirección datos del objeto-sub.

Además de seguimiento, la clase Sys.Debug también sirve para realizar aserciones de código. Las aserciones se utilizan para comprobar que se cumplen determinadas condiciones mientras se ejecuta una aplicación. La versión de depuración de los scripts de la biblioteca de AJAX de ASP.NET contiene varias assert (instrucciones) para probar una variedad de condiciones.

Listado 7 muestra un ejemplo del uso de la función Sys.Debug.assert para probar una condición. El código comprueba si el objeto de dirección es null antes de actualizar un objeto Person o no.


[![Resultado de la función Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: resultado de la función Sys.Debug.traceDump.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Listado 7. Con la función de debug.assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tres parámetros se pasan como la condición para evaluar, el mensaje que se mostrará si la aserción devuelve false y si se debe mostrar información sobre el llamador. En casos donde se produce un error en una aserción, se mostrará el mensaje, así como información del autor de la llamada si el tercer parámetro es true. Figura 12 muestra un ejemplo del cuadro de diálogo de error que aparece si se produce un error en la aserción que se muestra en la lista 7.

La función final para cubrir es Sys.Debug.fail. Cuando desea forzar código produce un error en una línea determinada en una secuencia de comandos puede agregar una llamada Sys.Debug.fail en lugar de la instrucción de depurador que se suelen usar en aplicaciones de JavaScript. La función Sys.Debug.fail acepta un parámetro de cadena único que representa el motivo del error, tal y como se muestra a continuación:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Un mensaje de error Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert un mensaje de error.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Cuando se encuentra una instrucción Sys.Debug.fail mientras se está ejecutando una secuencia de comandos, se mostrará el valor del parámetro del mensaje en la consola de una aplicación de depuración como Visual Studio 2008 y se le pedirá para depurar la aplicación. Un caso donde puede resultar bastante útil es cuando no se puede establecer un punto de interrupción con Visual Studio 2008 en un script en línea, pero desea que el código para detener en línea determinada, por lo que puede inspeccionar el valor de variables.

*Descripción ScriptMode propiedad del Control ScriptManager*

La biblioteca de AJAX de ASP.NET incluye depuración y de lanzamiento de las versiones de secuencia de comandos que se instalan en C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 de forma predeterminada. Las secuencias de comandos de depuración están bien formateada, fácil de leer y hayan varias llamadas de Sys.Debug.assert dispersos a lo largo de ellas mientras que las secuencias de comandos de versión tienen elimina el espacio en blanco y usar la clase Sys.Debug con moderación para reducir su tamaño total.

El control ScriptManager que se agregan a las páginas de AJAX de ASP.NET lee el atributo de depuración del elemento de compilación en el archivo web.config para determinar qué versiones de scripts de la biblioteca para cargar. Sin embargo, puede controlar si depuración o lanzamiento de las secuencias de comandos están cargados (scripts de la biblioteca y sus propios scripts personalizados) cambiando la propiedad ScriptMode. ScriptMode acepta una enumeración ScriptMode cuyos miembros incluyen automáticamente, Debug, Release y heredar.

ScriptMode el valor predeterminado es un valor de "Auto" lo que significa que el objeto ScriptManager comprobará el atributo de depuración en el archivo web.config. Cuando es false depuración ScriptManager cargará la versión de lanzamiento de scripts de la biblioteca de AJAX de ASP.NET. Cuando se cumple la depuración se cargará la versión de depuración de las secuencias de comandos. Cambiar la propiedad ScriptMode para liberar o depurar, se forzará el ScriptManager para cargar las secuencias de comandos adecuadas, independientemente de qué valor tiene el atributo de depuración en el archivo web.config. Listado 8 muestra un ejemplo del uso del control ScriptManager para cargar los scripts de depuración de la biblioteca de AJAX de ASP.NET.

**Enumerar 8. Cargar depurar scripts mediante el objeto ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

También puede cargar versiones diferentes (debug o release) de sus propios scripts personalizados mediante el uso de propiedad, las secuencias de comandos de ScriptManager junto con el componente ScriptReference tal y como se muestra en el listado 9.

**Listado 9. Cargar los scripts personalizados mediante el control ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Si se carga scripts personalizados mediante el componente ScriptReference debe notificar el ScriptManager cuando la secuencia de comandos ha terminado de cargarse agregando el código siguiente en la parte inferior de la secuencia de comandos:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

El código que se muestra en el listado 9 indica el objeto ScriptManager para buscar una versión de depuración de la secuencia de comandos de la persona que por lo que se buscará automáticamente Person.debug.js en lugar de Person.js. Si no se encuentra el archivo de Person.debug.js, que se producirá un error.

En casos donde desea una depuración o versión de lanzamiento de un script personalizado que se va a cargar en función del valor de la propiedad ScriptMode establecida en el control ScriptManager, puede establecer la ScriptReference propiedad del control ScriptMode a heredar. Esto hará que la versión correcta de la secuencia de comandos personalizada para cargarse en función de la propiedad de ScriptMode de ScriptManager tal como se muestra en la lista de 10. Dado que la propiedad ScriptMode del control ScriptManager se establece en la depuración, la secuencia de comandos de Person.debug.js se cargarán y utilizado en la página.

**Lista de 10. Heredar el ScriptMode de ScriptManager para scripts personalizados.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Utilizando la propiedad ScriptMode adecuadamente puede depurar aplicaciones más fácilmente y simplificar el proceso general. Scripts de lanzamiento de la biblioteca de AJAX de ASP.NET son difíciles de en su lugar recorrer paso a paso y se han leído desde el formato de código se ha quitado mientras se da formato a las secuencias de comandos de depuración específicamente para fines de depuración.

## <a name="conclusion"></a>Conclusión

Tecnología de Microsoft, AJAX de ASP.NET proporciona una base sólida para la creación de aplicaciones habilitadas para AJAX que pueden mejorar la experiencia global del usuario final. Sin embargo, como con cualquier tecnología de programación, errores y otros problemas de la aplicación sin duda surja. Saber sobre las distintas opciones de depuración disponibles ahorrará mucho tiempo y el resultado en un producto más estable.

En este artículo se ha se ha introducido para varias técnicas diferentes para la depuración de páginas de ASP.NET AJAX, incluido Internet Explorer con Visual Studio 2008, Web Development Helper y Firebug. Estas herramientas pueden simplificar el proceso de depuración general puesto que puede tener acceso a datos de la variable, recorrer el código línea por línea y ver instrucciones de seguimiento. Además de las distintas herramientas de depuración descritas, también ha visto cómo se puede usar Sys.Debug (clase) de la biblioteca de AJAX de ASP.NET en una aplicación y cómo se puede utilizar la clase ScriptManager para cargar depuración o lanzamiento de las versiones de las secuencias de comandos.

## <a name="bio"></a>Bio

Dan Wahlin (Microsoft los profesionales más valiosos de ASP.NET y servicios Web XML) es un consultor de instructor y arquitectura de desarrollo de .NET en formación técnica de interfaz ([www.interfacett.com)](http://www.interfacett.com). Dan fundada el XML para el sitio Web de desarrolladores de ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se encuentra en la oficina del orador INETA y ofrece varias conferencias. Dan coautor Professional Windows ADN (Wrox), ASP.NET: sugerencias, tutoriales y código (SAM), ASP.NET 1.1 Insider soluciones, Professional ASP.NET 2.0 AJAX (Wrox), ataques de MVP de ASP.NET 2.0 y XML creado para los programadores de ASP.NET (SAM). Si no, que está escribiendo código, artículos o libros, Dan disfruta de escribir y grabar música y reproducir golf y baloncesto con su esposa e hijos.

Scott categoría ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el director general de myKB.com ([www.myKB.com](http://www.myKB.com)) donde está especializado en la escritura de ASP.NET en función de las aplicaciones que se centra en las soluciones de Software de Base de conocimiento. Scott se puede contactar a través de correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-web-services.md)
