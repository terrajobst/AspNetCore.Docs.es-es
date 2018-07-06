---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 'Introducción a ASP.NET Web Pages: conceptos básicos de programación | Microsoft Docs'
author: tfitzmac
description: "Este tutorial proporciona información general sobre cómo programar en ASP.NET Web Pages con sintaxis Razor. Lo que aprenderá: la sintaxis 'Razor' básica que usa para la incorporación de cambios..."
ms.author: aspnetcontent
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 56268943b09d366b15d3a11e641d6c6b6c95aa16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839754"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introducción a las páginas Web ASP.NET - conceptos básicos de programación
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial proporciona información general sobre cómo programar en ASP.NET Web Pages con sintaxis Razor.
> 
> Lo que aprenderá:
> 
> - La sintaxis "Razor" básica que usan para la programación en ASP.NET Web Pages.
> - Algunos básica C#, que es el lenguaje de programación que se va a usar.
> - Algunos conceptos básicos de programación de páginas Web.
> - Cómo instalar paquetes (componentes que contienen código creado previamente) para usar con su sitio.
> - Cómo usar *aplicaciones auxiliares* para realizar tareas comunes de programación.
>   
> 
> Características y tecnologías tratadas:
> 
> - NuGet y el Administrador de paquetes.
> - El `Gravatar` auxiliar.


Este tutorial es principalmente un ejercicio de presentación de la sintaxis de programación que va a usar para ASP.NET Web Pages. Obtendrá información sobre *sintaxis Razor* y lenguaje de programación de código que está escrito en C#. Obtuvo un vistazo a esta sintaxis en el tutorial anterior; en este tutorial explicaremos la sintaxis más.

Prometemos que este tutorial trata la mayoría de programación que verá en un tutorial de inicio único y que es el tutorial único que es *sólo* acerca de la programación. En el resto de los tutoriales en este conjunto, en realidad creará las páginas que hacer cosas interesantes.

También obtendrá información sobre *aplicaciones auxiliares de*. Una aplicación auxiliar es un componente, un fragmento de empaquetado de seguridad de código, que se pueden agregar a una página. La aplicación auxiliar realiza el trabajo que en caso contrario, podría ser una tarea tediosa o complejas hacer a mano.

## <a name="creating-a-page-to-play-with-razor"></a>Creación de una página para jugar con Razor

En esta sección reproducirá un poco con Razor para que pueda hacerse una idea de la sintaxis básica.

Inicie WebMatrix si ya no se está ejecutando. Deberá usar el sitio Web que creó en el tutorial anterior ([Introducción a trabajar con páginas Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para volver a abrirlo, haga clic en **Mis sitios** y elija **WebPageMovies**:

![Pantalla que muestra las opciones de abrir sitio y el resaltado de Mis sitios de inicio de WebMatrix](intro-to-web-pages-programming/_static/image1.png)

Seleccione el **archivos** área de trabajo.

En la cinta de opciones, haga clic en **New** para crear una página. Seleccione **CSHTML** y el nombre de la nueva página *TestRazor.cshtml*.

Haga clic en **Aceptar**.

Copie lo siguiente en el archivo y reemplazar completamente lo que no existe ya está.

> [!NOTE]
> Cuando copie código o marcado de los ejemplos en una página, la sangría y la alineación quizás no sea el mismo que en el tutorial. Sangría y alineación no afectan a cómo se ejecuta el código, sin embargo.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examen de la página de ejemplo

La mayoría de lo que ve es HTML normal. Sin embargo, en la parte superior hay este bloque de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Tenga en cuenta lo siguiente acerca de este bloque de código:

- El carácter @ indica a ASP.NET que lo que sigue es el código de Razor, no HTML. ASP.NET tratará todo tras el @ caracteres como código hasta que se ejecuta de nuevo en HTML. (En este caso, es el &lt;! DOCTYPE&gt; elemento.
- Las llaves ({y}) contenga un bloque de código Razor si el código tiene más de una línea. Las llaves indicar a ASP.NET, donde el código para ese bloque se inicia y finaliza.
- La / / caracteres marcan un comentario, es decir, una parte del código que no se ejecutará.
- Cada instrucción tiene que finalizar con un punto y coma (;). (Sin comentarios, aunque.)
- Puede almacenar valores en *variables*, que se crea (*declarar*) con la palabra clave var. Cuando se crea una variable, darle un nombre, que puede incluir letras, números y caracteres de subrayado (\_). Los nombres de variable no pueden empezar con un número y no pueden usar el nombre de una palabra clave de programación (por ejemplo, var).
- Escriba las cadenas de caracteres (por ejemplo, "ASP.NET" y "Páginas Web") entre comillas. (Deben ser comillas dobles). No son números entre comillas.
- No importa el espacio en blanco fuera de las comillas. No importan principalmente saltos de línea. la excepción es que no se puede dividir una cadena entre comillas entre líneas. No importan la sangría y alineación.

Algo que no sea obvio a partir de este ejemplo es que todo el código distingue mayúsculas de minúsculas. Esto significa que la suma de la variable es una variable diferente que las variables que se podría denominar la suma o la suma. De forma similar, var es una palabra clave, pero Var no es.

### <a name="objects-and-properties-and-methods"></a>Objetos y propiedades y métodos

Es la expresión DateTime.Now. En términos sencillos, fecha y hora es un *objeto*. Un objeto es algo que se puede programar con, una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente, etcetera. Los objetos tienen uno o más *propiedades* que describen sus características. Un objeto de cuadro de texto tiene una propiedad de texto (entre otros), un objeto de solicitud tiene una propiedad de dirección Url (y otros), un mensaje de correo electrónico tiene una propiedad From y una propiedad To, y así sucesivamente. Los objetos también tienen *métodos* que son los "verbos" pueden realizar. Trabajará con objetos mucho.

Como puede ver en el ejemplo, fecha y hora es un objeto que le permita programa fechas y horas. Tiene una propiedad denominada ahora devuelve la fecha y hora actuales.

### <a name="using-code-to-render-markup-in-the-page"></a>Uso de código para representar el marcado en la página

En el cuerpo de la página, tenga en cuenta lo siguiente:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Nuevamente, el carácter @ indica a ASP.NET que lo que sigue es el código, no HTML. En el marcado se puede agregar seguido de una expresión de código y ASP.NET representará el valor de ese derecho de la expresión en ese momento. En el ejemplo, @a representará a todo lo que es el valor de la variable denominada una, @product representa cualquier cosa que esté en el producto con nombre de variable y así sucesivamente.

No está limitado a las variables, sin embargo. En algunos casos, el carácter @ precede a una expresión:

- @(una\*b) el producto de cualquier cosa que esté en las variables se representa un y b. (El \* significa de operador de multiplicación.)
- @(tecnología + "" + producto) representa los valores de la tecnología de las variables y el producto después de concatenarlas todas y agregar un espacio entre ellos. El operador (+) para concatenar cadenas es igual que el operador para sumar números. ASP.NET normalmente pueden indicar si está trabajando con números o con cadenas y hace lo correcto con el operador +.
- @Request.Url representa la propiedad de dirección Url del objeto de solicitud. El objeto de solicitud contiene información sobre la solicitud actual desde el explorador y, por supuesto, la propiedad de dirección Url contiene la dirección URL de la solicitud actual.

El ejemplo también está diseñado para mostrar que puede trabajar de maneras diferentes. Puede realizar cálculos en el bloque de código en la parte superior, colocar los resultados en una variable y, a continuación, representar la variable en el marcado. O bien, puede hacer cálculos en un derecho de la expresión en el marcado. El método que se utilice depende de lo que está haciendo y, en cierta medida, en sus propias preferencias.

### <a name="seeing-the-code-in-action"></a>Ver el código en acción

Haga clic en el nombre del archivo y, a continuación, elija **iniciar en el explorador**. Vea la página en el explorador con todos los valores y las expresiones que se resuelve en la página.

![Página de 'TestRazor' que se ejecuta en el explorador](intro-to-web-pages-programming/_static/image2.png)

Examine el código fuente en el explorador.

![Origen de la página 'Test Razor' en el explorador](intro-to-web-pages-programming/_static/image3.png)

Tal y como se espera que su experiencia en el tutorial anterior, ninguno de los códigos de Razor están en la página. Todo lo que ve son los valores de presentación real. Cuando se ejecuta una página, realmente está realizando una solicitud al servidor web que está integrado en WebMatrix. Cuando se recibe la solicitud, ASP.NET resuelve todos los valores y las expresiones y procesa sus valores en la página. A continuación, envía la página al explorador.

> [!TIP] 
> 
> **Razor y C#**
> 
> Hasta ahora hemos dicho que está trabajando con sintaxis Razor. Esto es cierto, pero no es la historia completa. El lenguaje de programación real utiliza se denomina *C#*. C# creado por Microsoft hace una década y ha convertido en uno de los lenguajes de programación principales para crear aplicaciones de Windows. Todas las reglas que ha visto acerca de cómo asignar un nombre de una variable y cómo crear instrucciones y así sucesivamente son realmente todas las reglas del lenguaje C#.
> 
> Razor se refiere concretamente en el pequeño conjunto de convenciones para cómo insertar este código en una página. Por ejemplo, la convención de usar para marcar el código de la página y usar @ {} para insertar un bloque de código es el aspecto de Razor de una página. Las aplicaciones auxiliares también se consideran parte de Razor. Sintaxis de Razor se usa en más lugares que acaba en ASP.NET Web Pages. (Por ejemplo, se utiliza en las vistas de MVC de ASP.NET también.)
> 
> Mencionamos esto porque si busca información acerca de la programación ASP.NET Web Pages, encontrará una gran cantidad de referencias a Razor. Sin embargo, una gran cantidad de esas referencias no se aplican a lo que va a realizar y, por tanto, puede resultar confuso. Y, de hecho, muchas de sus preguntas sobre programación realmente se va a ser sobre cómo trabajar con C# o trabajar con ASP.NET. Por lo que si observa específicamente para obtener información acerca de Razor, es posible que no encontrará las respuestas que necesita.


## <a name="adding-some-conditional-logic"></a>Agregar lógica condicional

Una de las características importantes sobre el uso de código en una página es que puede cambiar lo que se produce según varias condiciones. En esta parte del tutorial, podrá experimentar con algunos métodos para cambiar lo que se muestra en la página.

El ejemplo será simple y un poco retocado, por lo que podemos concentrar en la lógica condicional. La página que se va a crear hará lo siguiente:

- Mostrar texto diferente en la página en función de si es la primera vez que se muestra la página o si ha elegido un botón para enviar la página. Que será la primera prueba condicional.
- Mostrar el mensaje solo si se pasa un valor determinado en la cadena de consulta de la dirección URL (http://...?show=true). Que será la segunda prueba condicional.

En WebMatrix, cree una página y asígnele el nombre *TestRazorPart2.cshtml*. (En la cinta de opciones, haga clic en **New**, elija **CSHTML**, asigne el nombre y, a continuación, haga clic en **Aceptar**.)

Reemplace el contenido de la página con lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

El bloque de código en la parte superior, inicializa una variable denominada message con algo de texto. En el cuerpo de la página, se muestra el contenido de la variable de mensaje dentro de un &lt;p&gt; elemento. También contiene el marcado un &lt;entrada&gt; elemento para crear un **enviar** botón.

Ejecute la página para ver cómo funciona ahora. Por ahora, es básicamente una página estática, incluso si hace clic en el **enviar** botón.

Vuelva a WebMatrix. Dentro del bloque de código, agregue el siguiente código resaltado *después* la línea que inicializa el mensaje:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If bloque {}

Lo que acaba de agregar era if condición. En el código, la condición if debe tener una estructura similar al siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condición de prueba está entre paréntesis. Debe ser un valor o una expresión que devuelve true o false. Si la condición es true, ASP.NET se ejecuta la instrucción o instrucciones que están dentro de las llaves. (Estos son la *, a continuación,* forma parte de la *if-then* lógica.) Si la condición es false, se omite el bloque de código.

Estos son algunos ejemplos de condiciones que puede probar en if instrucción:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Puede probar variables con valores o de expresiones mediante un <em>operador lógico</em> o <em>operador de comparación</em>: igual a (==), mayor que (&gt;), menor que (&lt;), mayor o igual a (&gt;=) y menor o igual a (&lt;=). El! = significa operador no es igual a, por ejemplo, si (un! = 0) significa <em>si</em> <em>un</em><em>no es igual a 0</em>.

> [!NOTE]
> Asegúrese de que se tenga en cuenta que el operador de comparación de igual a (==) no es el mismo =. El = operador se usa solo para asignar valores (var un = 2). Si mezcla estos operadores de, obtendrá un error u obtendrá resultados extraños.


Para probar si algo es true, la sintaxis completa es if(IsDone == true). Pero también puede usar el método abreviado if(IsDone). Si no hay ningún operador de comparación, ASP.NET se da por supuesto que está probando para true.

¡El operador! operador por sí mismo: un operador lógico NOT. Por ejemplo, la condición if (! IsPost) significa *si no se cumple IsPost*.

Puede combinar condiciones mediante el uso de un operador lógico AND (&amp; &amp; operador) o el operador lógico OR (|| (operador)). Por ejemplo, la última de if condiciones en los medios de los ejemplos anteriores *si FileProcessingIsDone no está establecida en true displayMessage y se establece en false*.

### <a name="the-else-block"></a>El bloque else

Una última cosa sobre if bloques: una si el bloque puede ir seguido de un bloque else. Un bloque else es útil es que tiene que ejecutar código diferente cuando la condición es false. Este es un ejemplo sencillo:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Podrá ver algunos ejemplos en los tutoriales posteriores de esta serie donde resulta útil usar un bloque else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Comprobar si la solicitud es un envío (post)

Hay más, pero volvamos al ejemplo, que tiene el if(IsPost) condición {...}. IsPost es realmente una propiedad de la página actual. La primera vez que se solicita la página, IsPost devuelve false. Sin embargo, si haga clic en un botón o enviar la página de lo contrario, es decir, publicarlo, IsPost devuelve true. Por lo tanto IsPost le permite determinar si está trabajando con un envío de formulario. (En términos de verbos HTTP, si la solicitud es una operación GET, IsPost devuelve false. Si la solicitud es una operación POST, IsPost devuelve true.) En un tutorial posterior a trabajar con formularios de entrada, donde esta prueba es muy útil.

Ejecute la página. Dado que esta es la primera vez se solicita la página, vea "Esto es la primera vez que se ha solicitado la página". Esa cadena es el valor que inicializa la variable de mensaje. Hay una prueba if(IsPost), pero que devuelve false en este momento, para que bloquee el código dentro de if no se ejecuta.

Haga clic en el **enviar** botón. La página se vuelve a solicitar. Como antes, se establece la variable de mensaje "Este es la primera vez...". Pero esta vez, la prueba if(IsPost) devuelve true, por lo que el código dentro de bloque se ejecuta. El código cambia el valor de la variable de mensaje en un valor diferente, que es lo que se presenta en el marcado.

Ahora agregue if condición en el marcado. A continuación el &lt;p&gt; elemento que contiene el **enviar** botón, agregue el siguiente marcado:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Va a agregar código dentro del marcado, por lo que tienes para empezar @. A continuación, hay una instrucción if prueba similar al que agregó anteriormente en el bloque de código. Entre las llaves, no obstante, va a agregar HTML ordinario, como mínimo, es normal hasta que se llega a @DateTime.Now. Se trata de otro poco de código Razor, nuevamente tiene que agregar delante de él.

El punto aquí es que puede agregar si condiciones tanto en el bloque de código en la parte superior y en el marcado. Si usas if condición en el cuerpo de la página, las líneas dentro del bloque puede estar marcado o código. En ese caso, y como ocurre en cualquier momento mezclar código y marcado, tendrá que usar para dejar claro a ASP.NET donde es el código.

Ejecute la página y haga clic en **enviar**. Esta vez no solo ve un mensaje diferente al enviar ("ahora que ha enviado..."), pero verá un mensaje nuevo que muestra la fecha y hora.

![Página 'Test Razor 2' que se ejecuta en el explorador con la marca de tiempo que se muestra después de enviar](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Comprobación del valor de una cadena de consulta

Una prueba más. En esta ocasión, añadirá if bloque que prueba un valor denominado show que se puede pasar la cadena de consulta. (Como en este ejemplo: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) va a cambiar la página para que el mensaje se ha sido mostrar ("Esta es la primera vez...", etc.) solo se muestra si el valor de presentación es true.

En la parte inferior (pero interior) el bloque de código en la parte superior de la página, agregue lo siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

El aspecto de ahora del bloque de código completo similar al ejemplo siguiente. (Recuerde que al copiar el código en la página, la sangría podría ser diferente. Pero no afecta a cómo se ejecuta el código).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

El nuevo código en el bloque Inicializa una variable denominada showMessage en false. Después, hace if prueba para buscar un valor en la cadena de consulta. Cuando se solicita en primer lugar la página, tiene una dirección URL como esta:

`http://localhost:43097/TestRazorPart2.cshtml`

El código determina si la dirección URL contiene una variable denominada que se muestran en la cadena de consulta similar a esta versión de la dirección URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostrar = true

La propia prueba examina la propiedad de cadena de consulta del objeto de solicitud. Si la cadena de consulta contiene un elemento denominado show, y si ese elemento se establece en true, if bloque se ejecuta y establece la variable showMessage en true.

Como puede ver, hay un truco. Como su nombre indica, la cadena de consulta es una cadena. Sin embargo, solo puede probar para true y false si el valor que se está probando es el valor booleano (true/false). Antes de poder probar el valor de la variable de mostrar la cadena de consulta, tendrá que convertirlo en un valor booleano. Eso es lo que hace el método AsBool: toma una cadena como entrada y lo convierte en un valor booleano. Claramente, si la cadena es "true", el método AsBool convierte ese valor en true. Si el valor de la cadena es cualquier otra cosa, AsBool devuelve false.

> [!TIP] 
> 
> **Tipos de datos y métodos As()**
> 
> Hemos sólo dijimos hasta el momento que cuando se crea una variable, use la palabra clave var. Esto no es la historia completa, sin embargo. Con el fin de manipular los valores, para agregar números, o concatenar cadenas, o comparar las fechas o de pruebas para true o false, C# tiene que trabajar con una representación interna correspondiente del valor. C# puede *normalmente* averiguar cuál debe ser esa representación (es decir, ¿qué *tipo* los datos) basándose en lo que hace con los valores. Sin embargo, no puede hacer. De lo contrario, tendrá que ayuda al indicar explícitamente cómo C# debe representar los datos. El método AsBool hace eso, indica a C# que un valor de cadena "true" o "false" debe tratarse como un valor booleano. Existen métodos similares para representar cadenas como otros tipos, como AsInt (tratado como un número entero), AsDateTime (tratado como una fecha y hora), AsFloat (tratado como un número de punto flotante) y así sucesivamente. Cuando se usa como métodos (), si C# no puede representar el valor de cadena cuando se le solicite, verá un error.


En el marcado de la página, quite o comente este elemento (aquí se muestra marcado como comentario):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Derecha donde quitado o comentadas ese texto, agregue lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

El si prueba indica que si es true, la variable showMessage representará un &lt;p&gt; elemento con el valor de la variable de mensaje.

### <a name="summary-of-your-conditional-logic"></a>Resumen de la lógica condicional

En caso de que no está completamente seguro de lo que acaba de hacer, presentamos un resumen.

- Se inicializa la variable de mensaje en una cadena predeterminada ("Esta es la primera vez...").
- Si la solicitud de página es el resultado de un envío (post), el valor del mensaje se cambia a "ahora que ha enviado..."
- Se inicializa la variable showMessage en false.
- Si contiene la cadena de consulta? mostrar = true, la variable showMessage se establece en true.
- En el marcado, si es true, showMessage un &lt;p&gt; se representa el elemento que se muestra el valor del mensaje. (Si es false showMessage, nada se representa en ese momento en el marcado.)
- En el marcado, si la solicitud es una publicación de un &lt;p&gt; se representa el elemento que muestra la fecha y hora.

Ejecute la página. No hay ningún mensaje, porque showMessage es false, por lo que en el marcado de la prueba if(showMessage) devuelve false.

Haga clic en **Enviar**. Ver la fecha y hora, pero aún no contiene ningún mensaje.

En el explorador, vaya al cuadro de dirección URL y agregue lo siguiente al final de la dirección URL:? mostrar = true y, a continuación, presione ENTRAR.

![Página 'Test Razor 2' en el explorador que muestra la cadena de consulta](intro-to-web-pages-programming/_static/image5.png)

Aparecerá la página de nuevo. (Debido a que cambió la dirección URL, esto es una nueva solicitud, no un envío). Haga clic en **enviar** nuevo. El mensaje aparecerá de nuevo, como es la fecha y hora.

![Página 'Test 2 Razor' después de enviar cuando hay una cadena de consulta](intro-to-web-pages-programming/_static/image6.png)

En la dirección URL, cambie? mostrar = true para? mostrar = false y presione ENTRAR. Enviar la página de nuevo. La página es volver a cómo inicia, ningún mensaje.

Como se indicó anteriormente, la lógica de este ejemplo es un poco retocado. Sin embargo, si se va a aparecer en muchas de las páginas, y tardará uno o varios de los formularios que ha visto aquí.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalar una aplicación auxiliar (mostrar una imagen de Gravatar)

Algunas tareas que las personas desean hacer en las páginas web a menudo requieren una gran cantidad de código o requieren conocimientos adicionales. Ejemplos: mostrar un gráfico para los datos; colocar un Facebook "Como" botón en una página; envío de correo electrónico desde su sitio Web; recortar o cambiar el tamaño de imágenes. uso de PayPal para su sitio. Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares de*. Las aplicaciones auxiliares son componentes que se instala un sitio y que le permiten realizar tareas habituales con solo unas pocas líneas de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet. NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se encarga de todos los detalles de la instalación.

En esta parte del tutorial, instalará una aplicación auxiliar que le permite mostrar una imagen de Gravatar ("avatar globalmente reconocido"). Obtendrá información sobre estas dos cosas. Uno es cómo buscar e instalar una aplicación auxiliar. También aprenderá cómo una aplicación auxiliar facilita hacer algo en caso contrario, deberá hacer con una gran cantidad de código que se tendría que escribir manualmente.

Puede registrar su propio Gravatar en el sitio Web de Gravatar en [ http://www.gravatar.com/ ](http://www.gravatar.com/), pero no es esencial para crear una cuenta de Gravatar para llevar a cabo esta parte del tutorial.

En WebMatrix, haga clic en el **NuGet** botón.

![Cuadro de diálogo de la Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles. (No todos los paquetes son aplicaciones auxiliares; algunos agregan funcionalidad a WebMatrix mismo, algunas son plantillas adicionales y así sucesivamente.) Es posible que obtenga un mensaje de error acerca de la incompatibilidad de versiones. Puede ignorar este mensaje de error, haga clic en **Aceptar** y continuar con este tutorial.

![Cuadro de diálogo de la Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image8.png)

En el cuadro de búsqueda, escriba "aplicaciones auxiliares de asp.net". NuGet muestra los paquetes que coinciden con los términos de búsqueda.

![Galería de NuGet en WebMatrix, que muestra los paquetes](intro-to-web-pages-programming/_static/image9.png)

La biblioteca de aplicaciones auxiliares de ASP.NET Web contiene código para simplificar muchas tareas comunes, incluido el uso de imágenes Gravatar. Seleccione el **ASP.NET Web Helpers Library** del paquete y, a continuación, haga clic en **instalar** para iniciar el programa de instalación. Seleccione **Sí** cuando se le pregunte si desea instalar el paquete y acepte los términos para completar la instalación.

Ya está. NuGet se descarga e instala todo, incluidos los componentes adicionales que podrían ser necesarios (*dependencias*).

Si por algún motivo tiene que desinstalar una aplicación auxiliar, el proceso es muy similar. Haga clic en el **NuGet** botón, haga clic en el **instalado** pestaña y seleccione el paquete que desea desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usar una aplicación auxiliar en una página

Ahora usará la aplicación auxiliar que acaba de instalar. El proceso para agregar una aplicación auxiliar a una página es similar para la mayoría de las aplicaciones auxiliares.

En WebMatrix, cree una página y asígnele el nombre *GravatarTest.cshml*. (Va a crear una página especial para probar la aplicación auxiliar, pero puede usar aplicaciones auxiliares en cualquier página del sitio).

Dentro de la &lt;cuerpo&gt; elemento, agregue un &lt;div&gt; elemento. Dentro de la &lt;div&gt; elemento, escriba lo siguiente:

@Gravatar.

El carácter @ es el mismo carácter que se ha utilizado para marcar el código Razor. **Gravatar** es el objeto auxiliar que está trabajando.

Tan pronto como escriba el punto (.), WebMatrix muestra una lista de *métodos* (funciones) que la aplicación auxiliar de Gravatar hace que estén disponible:

![Lista desplegable de IntelliSense de aplicación auxiliar de Gravatar](intro-to-web-pages-programming/_static/image10.png)

Esta característica se conoce como *IntelliSense*. Le ayuda a código, ya que proporciona opciones de contexto adecuada. IntelliSense funciona con HTML, CSS, el código ASP.NET, JavaScript y otros lenguajes que se admiten en WebMatrix. Es otra característica que facilita el desarrollo de web pages en WebMatrix.

Presionar G en el teclado y vea que IntelliSense busca el método GetHtml. Presione Tab. IntelliSense inserta el método seleccionado (GetHtml) por usted. Escriba un paréntesis de apertura y tenga en cuenta que el paréntesis de cierre se agrega automáticamente. Escriba su dirección de correo electrónico comillas entre los paréntesis de dos. Si tiene una cuenta de Gravatar, se devolverá la imagen del perfil. Si no tiene una cuenta de Gravatar, se devuelve una imagen predeterminada. Cuando haya terminado, la línea tiene este aspecto:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Ahora puede ver la página en un explorador. Dependiendo de si tiene una cuenta de Gravatar, se muestra la imagen o la imagen predeterminada.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagen predeterminada](intro-to-web-pages-programming/_static/image12.png)

Para hacerse una idea de lo que está haciendo la aplicación auxiliar para usted, vea el origen de la página en el explorador. Junto con el código HTML que tenía en la página, verá un elemento de imagen que incluye un identificador. Se trata de código que representa la aplicación auxiliar en la página en el lugar donde había @Gravatar.GetHtml. La aplicación auxiliar tardó la información proporcionada y generó el código que se comunica directamente con Gravatar con el fin de recibir la imagen correcta para la cuenta proporcionada.

El método GetHtml también le permite personalizar la imagen al proporcionar otros parámetros. El código siguiente muestra cómo solicitar una imagen con un ancho y alto de 40 píxeles y usa una imagen predeterminada especificada denominada **wavatar** si la cuenta especificada no existe.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Este código genera algo parecido a lo siguiente (la imagen predeterminada de forma aleatoria puede variar).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Próxima

Para mantener este breve tutorial, tuvimos que centrarse en solo algunos conceptos básicos. Naturalmente, hay un *mucho* más Razor y C#. Aprenderá más que vaya a través de estos tutoriales. Si está interesado en aprender más sobre los aspectos de programación de C# y Razor ahora mismo, puede leer una introducción más completa aquí: [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

El siguiente tutorial presenta a trabajar con una base de datos. En ese tutorial, empezará a crear la aplicación de ejemplo que le permite enumerar sus películas favoritas.

## <a name="complete-listing-for-testrazor-page"></a>Lista completa de página TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Lista completa de página TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Lista completa de página GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Aplicación auxiliar de Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Siguiente](displaying-data.md)
