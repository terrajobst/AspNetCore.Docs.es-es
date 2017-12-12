---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: "Introducción a ASP.NET Web Pages: conceptos básicos de programación | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial ofrece una introducción al programa en ASP.NET Web Pages con sintaxis Razor. Lo que aprenderá: la sintaxis 'Razor' básica que utiliza para pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: eed07f4f8a13ea9082ab3aad3e3db24febff8ef6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introducción a ASP.NET Web Pages: conceptos básicos de programación
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial ofrece una introducción al programa en ASP.NET Web Pages con sintaxis Razor.
> 
> Lo que aprenderá:
> 
> - La sintaxis básica de "Razor" que usan para la programación en ASP.NET Web Pages.
> - Algunos básico de C#, que es el lenguaje de programación que se va a utilizar.
> - Algunos conceptos básicos de programación de páginas Web.
> - Cómo instalar paquetes (componentes que contienen código creada previamente) que se va a utilizar con su sitio.
> - Cómo usar *aplicaciones auxiliares* para realizar tareas de programación habituales.
>   
> 
> Características y tecnologías que se tratan:
> 
> - NuGet y el Administrador de paquetes.
> - El `Gravatar` auxiliar.


Este tutorial es principalmente un ejercicio de ellas se presenta la sintaxis de programación que va a utilizar para ASP.NET Web Pages. Obtendrá información sobre *sintaxis Razor* y lenguaje de programación de código que se escribe en C#. Obtuvo un vistazo a esta sintaxis en el tutorial anterior; en este tutorial le explicaremos la sintaxis más.

Te prometemos que este tutorial afecta a la mayoría de programación que verá en un tutorial único y que es el tutorial único que es *sólo* acerca de la programación. En el resto de los tutoriales en este conjunto, podrá crear realmente páginas que hacer cosas interesantes.

También obtendrá información sobre *aplicaciones auxiliares*. Una aplicación auxiliar es un componente, un fragmento de empaquetado de seguridad de código, que se pueden agregar a una página. La aplicación auxiliar realiza un trabajo para que en caso contrario, podría ser una tarea tediosa o complejas hacer a mano.

## <a name="creating-a-page-to-play-with-razor"></a>Creación de una página para reproducir con Razor

En esta sección reproducirá un poco con Razor por lo que puede hacerse una idea de la sintaxis básica.

Si ya no se está ejecutando, inicie WebMatrix. Se usará el sitio Web que creó en el tutorial anterior ([obtener iniciado con páginas Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para volver a abrirlo, haga clic en **Mis sitios** y elija **WebPageMovies**:

![Pantalla de inicio de WebMatrix que muestra las opciones de abrir sitio Web y Mis sitios resaltado](intro-to-web-pages-programming/_static/image1.png)

Seleccione el **archivos** área de trabajo.

En la cinta de opciones, haga clic en **New** para crear una página. Seleccione **CSHTML** y el nombre de la nueva página *TestRazor.cshtml*.

Haga clic en **Aceptar**.

Copie lo siguiente en el archivo, reemplazar completamente novedades no existe ya.

> [!NOTE]
> Cuando copie código o marcado de los ejemplos en una página, la sangría y alineación quizás no sea el mismo que en el tutorial. Sangría y alineación no afectan a cómo se ejecuta el código, aunque.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examen de la página de ejemplo

La mayor parte de lo que ve es HTML normal. Sin embargo, en la parte superior hay este bloque de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Tenga en cuenta lo siguiente acerca de este bloque de código:

- El carácter @ indica a ASP.NET que lo que sigue es el código Razor, no HTML. ASP.NET tratará todo después el carácter como código hasta que se ejecuta en el código HTML nuevo @. (En este caso, que el &lt;! DOCTYPE&gt; elemento.
- Las llaves ({y}) contenga un bloque de código Razor si el código tiene más de una línea. Las llaves indican ASP.NET donde el código de dicho bloque inicia y finaliza.
- El / / caracteres marcan un comentario, es decir, una parte del código que no ejecute.
- Cada instrucción tiene que finalizar con un punto y coma (;). (No los comentarios, aunque.)
- Puede almacenar valores en *variables*, que se crea (*declarar*) con la palabra clave var Cuando se crea una variable, se proporciona un nombre, que puede incluir letras, números y caracteres de subrayado (\_). Los nombres de variable no pueden comenzar con un número y no pueden usar el nombre de una palabra clave de programación (por ejemplo, var).
- Delimitar cadenas de caracteres (por ejemplo, "ASP.NET" y "Páginas Web") entre comillas. (Deben ser comillas dobles). No son números entre comillas.
- No importa el espacio en blanco fuera de las comillas. No importan principalmente saltos de línea; la excepción es que no se puede dividir una cadena entre comillas entre las líneas. No importan la sangría y la alineación.

Algo que no sea obvio desde este ejemplo es que todo el código distingue mayúsculas de minúsculas. Esto significa que la suma de la variable es una variable distinta que las variables podría llamarse la suma o la suma. De forma similar, var es una palabra clave, pero Var no es.

### <a name="objects-and-properties-and-methods"></a>Objetos y propiedades y métodos

A continuación, hay la expresión DateTime.Now. En otras palabras, la fecha y hora es un *objeto*. Un objeto es algo que se puede programar con: una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente, etcetera. Los objetos tienen uno o más *propiedades* que describen sus características. Un objeto de cuadro de texto tiene una propiedad de texto (entre otras cosas), un objeto de solicitud tiene una propiedad de dirección Url (y otros), un mensaje de correo electrónico tiene una propiedad y una propiedad para, y así sucesivamente. Los objetos también tienen *métodos* que son los "verbos" pueden realizar. Trabajará con objetos mucho.

Como puede ver en el ejemplo, fecha y hora es un objeto que permite programa fechas y horas. Tiene una propiedad denominada ahora devuelve la fecha y hora actuales.

### <a name="using-code-to-render-markup-in-the-page"></a>Uso de código para representar el marcado en la página

En el cuerpo de la página, tenga en cuenta lo siguiente:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Una vez más, el carácter @ indica a ASP.NET que lo que sigue es el código, no HTML. En el marcado puede agregar seguido de una expresión de código y ASP.NET se representará el valor de ese derecho de la expresión en ese momento. En el ejemplo, @a presenta lo que es el valor de la variable denominada a, @product representa lo que está en el producto designado variable y así sucesivamente.

No está limitado a las variables, aunque. En algunos casos, el carácter @ precede a una expresión:

- @(una\*b) representa el producto de cualquier objeto almacenado en las variables de una y b. (El \* significa de operador de multiplicación.)
- @(tecnología + "" + producto) representa los valores de la tecnología de las variables y el producto después de concatenarlas todas y agregar un espacio entre ambas. El operador (+) para la concatenación de cadenas es el mismo que el operador para agregar números. ASP.NET normalmente puede saber si está trabajando con números o con cadenas y toma las medidas adecuadas con el operador +.
- @Request.Urlrepresenta la propiedad de dirección Url del objeto de solicitud. El objeto de solicitud contiene información sobre la solicitud actual desde el explorador y, por supuesto, la propiedad de dirección Url contiene la dirección URL de la solicitud actual.

El ejemplo también se ha diseñado para mostrar que puede trabajar de maneras diferentes. Puede realizar cálculos en el bloque de código en la parte superior, colocar los resultados en una variable y, a continuación, representar la variable en el marcado. O bien, puede realizar cálculos en un derecho de la expresión en el marcado. El método utilizado depende de lo que está haciendo y, hasta cierto punto, en sus propias preferencias.

### <a name="seeing-the-code-in-action"></a>Ver el código de acción

Haga clic en el nombre del archivo y, a continuación, elija **iniciar en un explorador**. Vea la página en el explorador con todos los valores y expresiones que se resuelve en la página.

![Página 'TestRazor' que se ejecuta en el explorador](intro-to-web-pages-programming/_static/image2.png)

Examine el código fuente en el explorador.

![Origen de la página 'Test Razor' en el explorador](intro-to-web-pages-programming/_static/image3.png)

Tal y como esperaba desde su experiencia en el tutorial anterior, ninguno de los códigos de Razor están en la página. Todo lo que ve son los valores de presentación real. Cuando se ejecuta una página, realmente está realizando una solicitud al servidor web que está integrado en WebMatrix. Cuando se recibe la solicitud, ASP.NET resuelve todos los valores y las expresiones y sus valores se representa en la página. A continuación, envía la página al explorador.

> [!TIP] 
> 
> **Razor y C#**
> 
> Hasta ahora que hemos dicho que está trabajando con sintaxis Razor. Esto es cierto, pero no es la historia completa. El lenguaje de programación real que está usando se denomina *C#*. C# se creó con Microsoft hace una década y ha convertido en uno de los lenguajes de programación principales para crear aplicaciones de Windows. Todas las reglas que ha visto sobre cómo denominar una variable y cómo crear instrucciones y así sucesivamente son realmente todas las reglas del lenguaje C#.
> 
> Razor hace referencia más específicamente para el conjunto reducido de convenciones para cómo incrustar este código en una página. Por ejemplo, la convención de usar para marcar el código de la página y uso de @ {} para incrustar un bloque de código es el aspecto de Razor de una página. Aplicaciones auxiliares también se consideran parte de Razor. Se utiliza la sintaxis Razor en más lugares que acaba en ASP.NET Web Pages. (Por ejemplo, se utiliza en las vistas de MVC de ASP.NET también.)
> 
> Hemos mencionado esto porque si busca información sobre programación ASP.NET Web Pages, encontrará una gran cantidad de referencias a Razor. Sin embargo, muchas de esas referencias no son aplicables a qué está hacerlo y, por tanto, puede resultar confuso. Y, de hecho, muchas de sus preguntas sobre programación realmente va a estar relacionados con trabajar con C# o para trabajar con ASP.NET. Por lo que si observa específicamente para obtener información acerca de Razor, no puede encontrar las respuestas que necesita.


## <a name="adding-some-conditional-logic"></a>Agregar lógica condicional

Una de las mejores características sobre el uso de código en una página es que puede cambiar lo que sucede basándose en varias condiciones. En esta parte del tutorial, podrá experimentar con algunas maneras de cambiar lo que se muestra en la página.

El ejemplo será simple y un poco retocado para que podamos concentrar en la lógica condicional. La página que se va a crear hace esto:

- Mostrar texto diferente en la página en función de si es la primera vez que se muestra la página o si ha hecho clic en un botón para enviar la página. Que será la primera prueba condicional.
- Mostrar el mensaje solo si se pasa un valor determinado en la cadena de consulta de la dirección URL (http://...?show=true). Que será la segunda prueba condicional.

En WebMatrix, cree una página y asígnele el nombre *TestRazorPart2.cshtml*. (En la cinta de opciones, haga clic en **New**, elija **CSHTML**, un nombre al archivo y, a continuación, haga clic en **Aceptar**.)

Reemplace el contenido de la página con lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

El bloque de código en la parte superior, inicializa una variable denominada message algún texto. En el cuerpo de la página, se muestra el contenido de la variable de mensaje dentro de un &lt;p&gt; elemento. También contiene el marcado una &lt;entrada&gt; elemento que se va a crear un **enviar** botón.

Ejecute la página para ver cómo funciona ahora. Por ahora, se trata básicamente de una página estática, incluso si hace clic en el **enviar** botón.

Vuelva a WebMatrix. Dentro del bloque de código, agregue el siguiente código resaltado *después* la línea que inicializa el mensaje:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If bloque {}

Lo que acaba de agregar era if condición. En el código, el si condición tiene una estructura similar al siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condición de prueba está entre paréntesis. Debe ser un valor o una expresión que devuelve true o false. Si la condición es true, ASP.NET se ejecuta la instrucción o instrucciones que están dentro de las llaves. (Que son el *, a continuación,* forma parte de la *if-then* lógica.) Si la condición es false, se omitirá el bloque de código.

Estos son algunos ejemplos de condiciones que puede probar en if instrucción:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Puede probar las variables con valores o en expresiones mediante un *operador lógico* o *operador de comparación*: igual a (==), mayor que (&gt;), menor que (&lt;), mayor o igual que (&gt;=) y menor o igual que (&lt;=). El! = (operador) significa que no es igual a, por ejemplo, si (un! = 0) significa *si* *un**no es igual a 0*.

> [!NOTE]
> Asegúrese de que se tenga en cuenta que el operador de comparación para igual a (==) no es el mismo valor que =. El = operador se usa solo para asignar valores (var un = 2). Si mezcla estos operadores, obtendrá un error u obtendrá resultados extraños.


Para probar si un elemento es true, la sintaxis completa es if(IsDone == true). Pero también puede utilizar el método abreviado if(IsDone). Si no hay ningún operador de comparación, ASP.NET se da por supuesto que está probando para true.

¡El operador! operador por sí solo significa que un operador lógico NOT. Por ejemplo, la condición if (¡! IsPost) significa *si no se cumple IsPost*.

Puede combinar condiciones mediante el uso de un operador lógico AND (&amp; &amp; operador) o el operador lógico OR (|| (operador)). Por ejemplo, el último del if condiciones en los medios de los ejemplos anteriores *si FileProcessingIsDone no está establecido en true displayMessage AND se establece en false*.

### <a name="the-else-block"></a>El bloque else

Una última cosa sobre if bloques: un si bloque puede ir seguido de un bloque else. Un bloque else es útil es tiene que ejecutar código diferente cuando la condición es false. Este es un ejemplo simple:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Verá algunos ejemplos en los tutoriales posteriores de esta serie en el uso de un bloque else es útil.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Comprobando la existencia de la solicitud es un envío (post)

Hay más, pero volvamos al ejemplo, que tiene el if(IsPost) condición {...}. IsPost es realmente una propiedad de la página actual. La primera vez que se solicita la página, IsPost devuelve false. Sin embargo, si haga clic en un botón o enviar la página de lo contrario, es decir, publíquela: IsPost devuelve true. Por lo tanto IsPost le permite determinar si está tratando con un envío de formulario. (En términos de verbos HTTP, si la solicitud es una operación GET, IsPost devuelve false. Si la solicitud es una operación POST, IsPost devuelve true.) En un tutorial posterior trabajará con los formularios de entrada, que esta prueba es muy útil.

Ejecute la página. Puesto que esta es la primera vez que le solicita la página, vea "Esto es la primera vez que se ha solicitado la página". Esa cadena es el valor que inicializa la variable de mensaje a. Hay una prueba de if(IsPost), pero que devuelve false en este momento, de modo que impiden que el código dentro de la a ataques si no se ejecuta.

Haga clic en el **enviar** botón. Nuevo que se solicita la página. Como antes, la variable de mensaje se establece en "Es la primera vez...". Pero esta vez, la if(IsPost) de prueba devuelve true, por lo que el código dentro de la if bloque se ejecuta. El código cambia el valor de la variable de mensaje en un valor diferente, que es lo que se presenta en el marcado.

Ahora, agregue un if condición en el marcado. A continuación el &lt;p&gt; elemento que contiene el **enviar** botón, agregue el siguiente marcado:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Va a agregar código en el marcado, por lo que tienen que empezar por @. A continuación, hay una instrucción if prueba similar a la que agregó anteriormente en el bloque de código. Entre las llaves, sin embargo, va a agregar HTML normal, como mínimo, es normal hasta que se llega a @DateTime.Now. Se trata de otro poco de código Razor, por lo que tienes que agregar delante de él.

El punto aquí es que puede agregar si condiciones tanto en el bloque de código en la parte superior y en el marcado. Si se utilice una estructura if condición en el cuerpo de la página, las líneas dentro del bloque puede ser o marcado del código. En ese caso, y como ocurre en cualquier momento mezclar código y marcado, tendrá que usar para dejar claro a ASP.NET donde es el código.

Ejecute la página y haga clic en **enviar**. Esta vez no solo verá un mensaje diferente al enviar ("ahora que ha enviado..."), pero verá un nuevo mensaje que muestra la fecha y hora.

![Página 'Test Razor 2' que se ejecuta en el explorador con la marca de tiempo que se muestra después de enviar](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Probar el valor de una cadena de consulta

Una prueba más. En este momento, podrá agregar un campo si el bloque que prueba un valor denominado show que podría pasarse en la cadena de consulta. (Similar a éste: "http://localhost:43097/TestRazorPart2.cshtml`?show=true`) va a cambiar la página para que el mensaje ha sido mostrar ("Esto es la primera vez...", etc.) aparece únicamente si el valor de presentación es true.

En la parte inferior (pero interior) el bloque de código en la parte superior de la página, agregue lo siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

La apariencia de ahora del bloque de código completo similar al ejemplo siguiente. (Recuerde que cuando copie el código en la página, la sangría podría ser diferente. Pero no afecta a cómo se ejecuta el código).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

El nuevo código en el bloque Inicializa una variable denominada showMessage en false. A continuación, realiza un if prueba para buscar un valor en la cadena de consulta. Cuando se solicita en primer lugar la página, tiene una dirección URL como esta:

`http://localhost:43097/TestRazorPart2.cshtml`

El código determina si la dirección URL contiene una variable denominada mostrar en la cadena de consulta, al igual que esta versión de la dirección URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostrar = true

La propia prueba examina la propiedad de cadena de consulta del objeto de solicitud. Si la cadena de consulta contiene un elemento denominado mostrar, y si ese elemento se establece en true, el if bloque se ejecuta y establece la variable showMessage en true.

Hay un truco aquí, como puede ver. Como se indica en el nombre, la cadena de consulta es una cadena. Sin embargo, solo puede probar para true y false si el valor que se está probando es un valor de booleano (verdadero/falso). Para poder probar el valor de la variable de programa en la cadena de consulta, tiene que convertir en un valor booleano. Que es lo que hace el método AsBool, toma una cadena como entrada y lo convierte en un valor booleano. Claramente, si la cadena es "true", el método AsBool convierte ese valor en true. Si el valor de la cadena es cualquier otra cosa, AsBool devuelve false.

> [!TIP] 
> 
> **Tipos de datos y los métodos de As()**
> 
> Hemos sólo dijimos hasta el momento que cuando se crea una variable, use la palabra clave var Esto no es todo el artículo, aunque. Para manipular valores, para agregar números, o concatenar cadenas, o comparar las fechas o de prueba para el valor true/false, C# tiene que trabajar con una representación interna apropiada del valor. C# puede *normalmente* averiguar cuál debería ser esa representación (es decir, ¿qué *tipo* los datos) en función de lo que hace con los valores. Ahora y, a continuación, sin embargo, no puede hacer. Si no es así, tendrá que ayudan al indicar explícitamente cómo C# deben representar los datos. El método AsBool hace eso, indica a C# que un valor de cadena "true" o "false" se debe tratar como un valor booleano. Existen métodos similares para representan cadenas como otros tipos, como AsInt (tratar como un entero), AsDateTime (tratar como una fecha y hora), AsFloat (tratar como un número de punto flotante) y así sucesivamente. Cuando se usa como métodos (), si C# no puede representar el valor de cadena cuando se le solicite, verá un error.


En el marcado de la página, quite o marque como comentario este elemento (aquí se muestra comentado):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Derecha donde quitado o comentada ese texto, agregue lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

El si prueba indica que si es true, la variable showMessage representará un &lt;p&gt; elemento con el valor de la variable de mensaje.

### <a name="summary-of-your-conditional-logic"></a>Resumen de la lógica condicional

En caso de que no está completamente seguro de lo que acaba de hecho, este es un resumen.

- La variable de mensaje se inicializa en una cadena predeterminada ("Esto es la primera vez...").
- Si la solicitud de página es el resultado de un envío (post), el valor del mensaje se cambia a "ahora que ha enviado..."
- La variable showMessage se inicializa en false.
- Si la cadena de consulta contiene? mostrar = true, la variable showMessage se establece en true.
- En el marcado, si es true, showMessage un &lt;p&gt; elemento se representa que muestra el valor del mensaje. (Si showMessage es false, no se representa en ese momento en el marcado.)
- En el marcado, si la solicitud es una entrada, una &lt;p&gt; se representa el elemento que muestra la fecha y hora.

Ejecute la página. No hay ningún mensaje, porque showMessage es false, por lo que en el marcado de la prueba de if(showMessage) devuelve false.

Haga clic en **enviar**. Verá la fecha y hora, pero aún no hay ningún mensaje.

En el explorador, vaya al cuadro de dirección URL y agregue lo siguiente al final de la dirección URL:? mostrar = true y, a continuación, presione ENTRAR.

![Página 'Test Razor 2' en el explorador y muestra la cadena de consulta](intro-to-web-pages-programming/_static/image5.png)

Se abrirá la página nuevo. (Debido a que cambió la dirección URL, esto es una nueva solicitud, no un envío). Haga clic en **enviar** nuevo. Se muestra el mensaje de nuevo, como es la fecha y hora.

![Página 'Prueba 2 de Razor' después de enviar cuando hay una cadena de consulta](intro-to-web-pages-programming/_static/image6.png)

En la dirección URL, cambie? mostrar = true para? mostrar = false y presione ENTRAR. Enviar la página de nuevo. La página es volver a cómo empezar a trabajar: ningún mensaje.

Como se indicó anteriormente, la lógica de este ejemplo es un poco retocado. Sin embargo, si se va a dar en muchas de las páginas, y le llevará una o varias de las formas que ha visto aquí.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalar una aplicación auxiliar (mostrar una imagen de Gravatar)

Algunas tareas que personas a menudo es conveniente en las páginas web requieren una gran cantidad de código o requieren conocimientos adicionales. Ejemplos: mostrar un gráfico de datos; colocar un Facebook "Como" botón en una página; al enviar correo electrónico de su sitio Web; recortar o cambiar el tamaño de las imágenes; uso de PayPal para su sitio. Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*. Aplicaciones auxiliares son componentes que se instala un sitio y que le permiten realizar tareas habituales con unas pocas líneas de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet. NuGet permite seleccionar un paquete que desea instalar y, a continuación, se encarga de todos los detalles de la instalación.

En esta parte del tutorial, instalará una aplicación auxiliar que le permite mostrar una imagen de Gravatar ("avatar globalmente reconocido"). Aprenderá dos cosas. Uno se muestra cómo buscar e instalar una aplicación auxiliar. También aprenderá cómo una aplicación auxiliar facilita la hacer algo que tendrían que hacer con una gran cantidad de código que tendría que escribir expresamente.

Puede registrar su propio Gravatar en el sitio Web de Gravatar en [http://www.gravatar.com/](http://www.gravatar.com/), pero no es esencial para crear una cuenta de Gravatar para llevar a cabo esta parte del tutorial.

En WebMatrix, haga clic en el **NuGet** botón.

![Cuadro de diálogo de la Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles. (No todos los paquetes de aplicaciones auxiliares; algunas agregan funcionalidad a WebMatrix propio, algunas son plantillas adicionales y así sucesivamente.) Puede obtener un mensaje de error acerca de la incompatibilidad de versiones. Puede ignorar este mensaje de error, haga clic en **Aceptar** y continuar con este tutorial.

![Cuadro de diálogo de la Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image8.png)

En el cuadro de búsqueda, escriba "aplicaciones auxiliares de asp.net". NuGet muestra los paquetes que coinciden con los términos de búsqueda.

![Galería de NuGet en WebMatrix que muestra los paquetes](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Helpers Library contiene código para simplificar muchas tareas comunes, incluido el uso de imágenes de Gravatar. Seleccione el **ASP.NET Web Helpers Library** del paquete y, a continuación, haga clic en **instalar** para iniciar el programa de instalación. Seleccione **Sí** cuando se le pregunte si desea instalar el paquete y acepte los términos para completar la instalación.

Ya está. NuGet se descarga e instala todo, incluidos los componentes adicionales que podrían ser necesarios (*dependencias*).

Si por algún motivo tiene que desinstalar una aplicación auxiliar, el proceso es muy similar. Haga clic en el **NuGet** botón, haga clic en el **instalado** pestaña y elegir el paquete que desea desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usar una aplicación auxiliar en una página

Ahora deberá usar la aplicación auxiliar que acaba de instalar. El proceso para agregar una aplicación auxiliar para una página es similar para la mayoría de aplicaciones auxiliares.

En WebMatrix, cree una página y asígnele el nombre *GravatarTest.cshml*. (Se va a crear una página especial para probar la aplicación auxiliar, pero puede usar aplicaciones auxiliares en cualquier página de su sitio).

Dentro de la &lt;cuerpo&gt; elemento, agregue un &lt;div&gt; elemento. Dentro de la &lt;div&gt; elemento, escriba lo siguiente:

@Gravatar.

El carácter @ es el mismo carácter que usó para marcar el código Razor. **Gravatar** es el objeto de aplicación auxiliar que se trabaja con.

En cuanto escriba el punto (.), WebMatrix muestra una lista de *métodos* (funciones) que la aplicación auxiliar Gravatar hace que estén disponible:

![Lista de desplegable IntelliSense auxiliar Gravatar](intro-to-web-pages-programming/_static/image10.png)

Esta característica se conoce como *IntelliSense*. Ayuda a que el código proporcionando las opciones de contexto que le corresponde. IntelliSense funciona con HTML, CSS, el código ASP.NET, JavaScript y otros lenguajes que se admiten en WebMatrix. Es otra característica que facilita el desarrollo de páginas web en WebMatrix.

Presione G en el teclado y vea que IntelliSense busca el método GetHtml. Presione Tab. IntelliSense inserta el método seleccionado (GetHtml) por usted. Escriba un paréntesis de apertura y observe que el paréntesis de cierre se agrega automáticamente. Escriba su dirección de correo electrónico de comillas entre los dos paréntesis. Si tiene una cuenta de Gravatar, se devolverá la imagen del perfil. Si no tiene una cuenta de Gravatar, se devuelve una imagen predeterminada. Cuando haya terminado, la línea tenga un aspecto similar al siguiente:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Ahora puede ver la página en un explorador. Dependiendo de si tiene una cuenta de Gravatar, se muestra la imagen o la imagen predeterminada.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagen predeterminada](intro-to-web-pages-programming/_static/image12.png)

Para hacerse una idea de lo que está haciendo la aplicación auxiliar para usted, ver el código fuente de la página en el explorador. Junto con el código HTML que tenía en la página, verá un elemento de imagen que incluye un identificador. Se trata de código que representa la aplicación auxiliar en la página en el lugar donde había @Gravatar.GetHtml. La aplicación auxiliar tardó la información proporcionada y genera el código que se comunica directamente con Gravatar para recuperar la imagen correcta para la cuenta proporcionada.

El método GetHtml también permite personalizar la imagen proporcionando otros parámetros. El código siguiente muestra cómo solicitar una imagen tiene un ancho y alto de 40 píxeles y usa una imagen predeterminada especificada con el nombre **wavatar** si la cuenta especificada no existe.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Este código genera algo parecido a lo siguiente (la imagen predeterminada aleatoriamente puede variar).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Próxima

Para mantener este breve tutorial, tuvimos que centrarse en solo algunos conceptos básicos. Naturalmente, hay un *muchos* más Razor y C#. Aprenderá más que vaya a través de estos tutoriales. Si está interesado en aprender más acerca de los aspectos de programación de C# y Razor ahora mismo, puede leer una introducción más exhaustiva aquí: [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

El siguiente tutorial muestra cómo trabajar con una base de datos. En este tutorial, podrá empezar a crear la aplicación de ejemplo que le permite enumerar sus películas favoritas.

## <a name="complete-listing-for-testrazor-page"></a>Muestra la lista completa de página TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Muestra la lista completa de página TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Muestra la lista completa de página GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Aplicación auxiliar de Twitter](../../ui-layouts-and-themes/twitter-helper.md)

>[!div class="step-by-step"]
[Anterior](getting-started.md)
[Siguiente](displaying-data.md)
