---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (C#) | Documentos de Microsoft
author: tfitzmac
description: Este capítulo proporciona información general de la programación con ASP.NET Web Pages usando la sintaxis de Razor. ASP.NET es la tecnología de Microsoft para ejecutar pa de web dinámico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 48f49f40a6fc0c6a0c664873879f9f61080132ea
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483690"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (C#)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo ofrece una visión general de la programación con ASP.NET Web Pages mediante la sintaxis de Razor. ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores web. Este se centra de artículos sobre cómo utilizar el lenguaje de programación de C#.
> 
> **Lo que aprenderá**:
> 
> - Los 8 superior consejos para empezar con ASP.NET Web Pages con sintaxis Razor de programación de programación.
> - Conceptos básicos de programación, deberá.
> - El código de servidor ASP.NET y la sintaxis Razor es todo sobre.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Las sugerencias de programación 8 superior

Esta sección enumeran algunas sugerencias que sea absolutamente necesario conocer al comenzar a escribir código de servidor ASP.NET mediante la sintaxis Razor.

> [!NOTE]
> La sintaxis de Razor se basa en el lenguaje de programación de C#, y que es el lenguaje que se suele usar con ASP.NET Web Pages. Sin embargo, la sintaxis Razor también admite el lenguaje Visual Basic pero todo lo que se ven que también puede hacer en Visual Basic. Para obtener más información, consulte el apéndice [lenguaje Visual Basic y la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908).


Puede encontrar más detalles sobre la mayoría de estas técnicas de programación más adelante en el artículo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Agregar código a una página con el carácter @

El `@` carácter asociado empiece a expresiones en línea, bloques de instrucción única y bloques de múltiples instrucciones:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Este es el aspecto de estas instrucciones cuando la página se ejecuta en un explorador:

![Img1 Razor](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codificación HTML**
> 
> Al mostrar el contenido de una página con el `@` de caracteres, como en los ejemplos anteriores, ASP.NET codifica en HTML el resultado. Esto reemplaza los caracteres reservados de HTML (como `<` y `>` y `&`) con códigos que permiten a los caracteres que pueden mostrarse como caracteres en una página web no se interprete como etiquetas HTML o entidades. Sin codificación HTML, la salida desde el código de servidor podría no mostrarse correctamente y podría exponer una página a riesgos de seguridad.
> 
> Si su objetivo es generar el marcado HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` a hacer hincapié en texto), vea la sección [combinación de texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.
> 
> Puede leer más sobre la codificación de HTML en [trabajar con formularios](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Incluya los bloques de código entre llaves

A *bloque de código* incluye una o varias instrucciones de código y aparece encerrado entre llaves.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

El resultado que se muestra en un explorador:

![Img2 de Razor](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Dentro de un bloque, finalizar cada instrucción de código con un punto y coma

Dentro de un bloque de código, cada instrucción de código completo debe terminar con un punto y coma. Las expresiones en línea no terminen con un punto y coma.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilizar variables para almacenar valores

Puede almacenar valores en una *variable*, incluidas las cadenas, números y fechas, etcetera. Crear una nueva variable mediante la `var` palabra clave. Puede insertar valores de las variables directamente en una página con `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

El resultado que se muestra en un explorador:

![Img3 de Razor](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Incluir valores de cadena literal de comillas dobles

A *cadena* es una secuencia de caracteres que se tratan como texto. Para especificar una cadena, escríbalo entre comillas dobles:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Si la cadena que desea mostrar contiene un carácter de barra diagonal inversa ( `\` ) o comillas dobles ( `"` ), use un *literal de cadena textual* que tiene como prefijo el `@` operador. (En C#, el \ carácter tiene un significado especial a menos que utilice un literal de cadena textual.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Para incrustar las comillas dobles, use un literal de cadena textual y repita las comillas:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Este es el resultado del uso de estos ejemplos de ambos en una página:

![Img4 de Razor](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Tenga en cuenta que el `@` carácter se usa para marcar los literales de cadenas literales en C# y que marca el código en las páginas ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Código distingue mayúsculas de minúsculas

En C#, palabras clave (como `var`, `true`, y `if`) y nombres de variables distinguen entre mayúsculas y minúsculas. Las siguientes líneas de código crean dos variables distintas, `lastName` y `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Si se declara una variable como `var lastName = "Smith";` y si se intenta hacer referencia a esa variable en la página como `@LastName`, se producirá un error porque `LastName` no se reconoce.

> [!NOTE]
> En Visual Basic, palabras clave y las variables son *no* entre mayúsculas y minúsculas.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Gran parte de la codificación implica objetos

Un *objeto* representa algo que se puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente (filas de la base de datos), etcetera. Los objetos tienen propiedades que describen sus características y que puede leer o cambiar &#8212; un objeto de cuadro de texto tiene un `Text` propiedad (entre otras cosas), un objeto de solicitud tiene un `Url` propiedad, un mensaje de correo electrónico tiene un `From` propiedad, y un objeto customer tiene una `FirstName` propiedad. Métodos de los objetos también tienen la &quot;verbos&quot; pueden realizar. Algunos ejemplos son un objeto de archivo `Save` /método siguiente, un objeto de imagen `Rotate` método y un objeto de correo electrónico `Send` método.

A menudo trabajará con la `Request` objeto, que proporciona información como los valores de los cuadros de texto (campos de formulario) en la página, el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera. En el ejemplo siguiente se muestra cómo obtener acceso a propiedades de la `Request` objeto y cómo llamar a la `MapPath` método de la `Request` objeto, que proporciona la ruta de acceso absoluta de la página en el servidor:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

El resultado que se muestra en un explorador:

![Img5 de Razor](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Puede escribir código que toma decisiones

Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de condiciones. La manera más común de hacerlo es con la `if` instrucción (y opcionalmente `else` instrucción).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

La instrucción `if(IsPost)` es una forma abreviada de escritura `if(IsPost == true)`. Junto con `if` instrucciones, hay una variedad de maneras de probar condiciones, repita bloques de código, y así sucesivamente, que se describen más adelante en este artículo.

El resultado mostrado en un explorador (después de hacer clic **enviar**):

![Img6 de Razor](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET y POST métodos y la propiedad IsPost
> 
> El protocolo utilizado para las páginas web (HTTP) es compatible con un número muy limitado de métodos (verbos) que se usan para realizar solicitudes al servidor. Los dos más comunes son GET, que se utiliza para leer una página, y POST, que se usa para enviar una página. En general, la primera vez que un usuario solicita una página, la página se solicita mediante GET. Si el usuario rellena un formulario y, a continuación, haga clic en un botón de envío, el explorador realiza una solicitud POST al servidor.
> 
> En la programación web, a menudo resulta útil saber si se solicita una página que se va a como una operación GET o como una entrada de blog para que sepa cómo procesar la página. En ASP.NET Web Pages, puede utilizar el `IsPost` propiedad para ver si una solicitud es una operación GET o POST. Si la solicitud es una entrada, el `IsPost` propiedad devolverá true y puede hacer cosas como la lectura de los valores de los cuadros de texto en un formulario. Muchos ejemplos verá mostrarle cómo procesar la página de forma diferente dependiendo del valor de `IsPost`.


## <a name="a-simple-code-example"></a>Un ejemplo de código sencillo

Este procedimiento muestra cómo crear una página que muestra las técnicas de programación básicas. En el ejemplo, cree una página que permite a los usuarios escribir dos números, a continuación, agrega y muestra el resultado.

1. En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers.cshtml*.
2. Copie el siguiente código y marcado en la página, reemplazando cualquier elemento ya está en la página.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Hay algunas cuestiones que tener en cuenta:

    - El `@` carácter asociado empiece el primer bloque de código en la página y precede a la `totalMessage` variable que se incrusta en la parte inferior de la página.
    - El bloque en la parte superior de la página aparece encerrado entre llaves.
    - En el bloque en la parte superior, todas las líneas terminan con un punto y coma.
    - Las variables `total`, `num1`, `num2`, y `totalMessage` almacenar varios números y una cadena.
    - El valor de cadena literal asignado a la `totalMessage` variable está entre comillas dobles.
    - Dado que el código distingue mayúsculas de minúsculas, cuando la `totalMessage` variable se usa en la parte inferior de la página, su nombre debe coincidir exactamente con la variable en la parte superior.
    - La expresión `num1.AsInt() + num2.AsInt()` muestra cómo trabajar con objetos y métodos. El `AsInt` método en cada variable convierte la cadena especificada por un usuario a un número (entero) para que pueda realizar operaciones aritméticas en él.
    - El `<form>` etiqueta incluye un `method="post"` atributo. Esto especifica que cuando el usuario hace clic en **agregar**, la página se enviará al servidor mediante el método HTTP POST. Cuando se envía la página, el `if(IsPost)` prueba se evalúa como true y la directiva de ejecución, mostrar el resultado de sumar los números del código.
3. Guarde la página y ejecútelo en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Escriba dos números enteros y, a continuación, haga clic en el **agregar** botón. 

    ![Img7 de Razor](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Conceptos básicos de programación

Este artículo proporciona una visión general de programación web ASP.NET. No es un examen exhaustivo, simplemente un paseo introductorio a través de los conceptos de programación que se usará con más frecuencia. Aun así, cubre casi todo que lo necesario para empezar a trabajar con ASP.NET Web Pages.

Pero primero, una pequeña Introducción técnica.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>La sintaxis de Razor, código del servidor y ASP.NET

La sintaxis Razor es una simple programación para incrustar código basado en el servidor en una página web. En una página web que usa la sintaxis de Razor, hay dos tipos de contenido: código de contenido y el servidor de cliente. Contenido de cliente es el material que está acostumbrado a en páginas web: el formato HTML (elementos), la información de estilo como CSS, tal vez algunos scripts de cliente, como JavaScript y texto sin formato.

Sintaxis de Razor le permite agregar código de servidor a este contenido de cliente. Si no hay código de servidor en la página, el servidor ejecuta dicho código en primer lugar, antes de enviar la página al explorador. Mediante la ejecución en el servidor, el código puede realizar las tareas que pueden ser mucho más complejas para hacer con solo, como el acceso a bases de datos basadas en servidor de contenido de cliente. Lo más importante, el código de servidor puede crear dinámicamente el contenido de cliente &#8212; puede generar el marcado HTML u otro contenido sobre la marcha y, a continuación, se enviará al explorador junto con cualquier HTML estático que la página puede contener. Desde la perspectiva del explorador, el contenido de cliente generado por el código del servidor es similar a cualquier otro contenido de cliente. Como ya ha visto, el código de servidor que se necesita es muy sencillo.

Páginas web ASP.NET que incluya la sintaxis Razor tienen una extensión de archivo especial (*.cshtml* o *.vbhtml*). El servidor reconoce estas extensiones, se ejecuta el código que se marca con sintaxis Razor y, a continuación, envía la página al explorador.

### <a name="where-does-aspnet-fit-in"></a>¿Dónde encaja ASP.NET?

Sintaxis de Razor se basa en una tecnología de Microsoft denominada ASP.NET, que a su vez se basa en Microsoft .NET Framework. .NET Framework es un marco de programación integral grande de Microsoft para desarrollar prácticamente cualquier tipo de aplicación del equipo. ASP.NET es la parte de .NET Framework que está diseñado específicamente para crear aplicaciones web. Los desarrolladores han usado ASP.NET para crear muchos de los sitios Web más grandes y más alto de tráfico en el mundo. (Cada vez que vea la extensión de nombre de archivo *.aspx* como parte de la dirección URL en un sitio, sabrá que el sitio se ha escrito utilizando ASP.NET.)

La sintaxis de Razor le ofrece toda la potencia de ASP.NET, pero con una sintaxis simplificada que es más fácil de obtener información sobre si es principiante y que mejora su productividad si es un experto. Aunque esta sintaxis es fácil de usar, su relación familia con ASP.NET y .NET Framework significa que sus sitios Web se vuelven más sofisticadas, tiene la capacidad de los marcos más grandes disponibles.

![Img8 de Razor](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Las clases e instancias**
> 
> Código de servidor ASP.NET usa objetos, que a su vez se basan en la idea de las clases. La clase es la definición o la plantilla de un objeto. Por ejemplo, una aplicación podría contener una `Customer` clase que define las propiedades y métodos que necesita de cualquier objeto de cliente.
> 
> Cuando la aplicación deba trabajar con la información real del cliente, crea una instancia de (o *crea una instancia*) un objeto de cliente. Cada cliente individual es una instancia independiente de la `Customer` clase. Cada instancia es compatible con las mismas propiedades y métodos, pero los valores de propiedad para cada instancia son normalmente diferentes, porque cada objeto customer es único. En el objeto de un cliente, el `LastName` propiedad podría ser "Smith"; en otro objeto de cliente, el `LastName` propiedad podría ser "Jones".
> 
> De igual forma, cualquier página web individuales en el sitio es un `Page` objeto que es una instancia de la `Page` clase. Un botón en la página es una `Button` objeto que es una instancia de la `Button` clase y así sucesivamente. Cada instancia tiene sus propias características, pero todos ellos se basan en lo que se especifique en la definición de clase del objeto.


## <a name="basic-syntax"></a>Sintaxis básica

Anteriormente se mostró un ejemplo básico de cómo crear una página de ASP.NET Web Pages y cómo puede agregar código de servidor en formato HTML. Aquí aprenderá los conceptos básicos de la escritura de código de servidor ASP.NET mediante la sintaxis Razor &#8212; es decir, las reglas de lenguaje de programación.

Si tiene experiencia con la programación (sobre todo si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que leer aquí le resultará familiar. Probablemente necesitará para familiarizarse con cómo se agrega el código del servidor para el marcado en *.cshtml* archivos.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinación de texto, marcado y código en bloques de código

En los bloques de código de servidor, a menudo desea salida texto o marcado (o ambos) a la página. Si un bloque de código de servidor contiene texto que no es código y que en su lugar, se debe representar tal cual, debe ser capaz de distinguir entre el texto de ASP.NET. Existen varias formas de hacerlo.

- El texto se incluya en un elemento HTML como `<p></p>` o `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    El elemento HTML puede incluir texto, los elementos adicionales de HTML y expresiones de código del servidor. Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), que se representa todo el contenido incluido el elemento y su contenido como está en el explorador, resolver expresiones de código del servidor a medida que pasa.
- Use la `@:` operador o `<text>` elemento. El `@:` genera una sola línea de contenido que contiene texto sin formato o las etiquetas HTML no coincidentes; el `<text>` elemento abarca varias líneas a la salida. Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Si desea enviar el resultado de varias líneas de texto o etiquetas HTML no coincidentes, puede preceder a cada línea con `@:`, ni puede incluir la línea en un `<text>` elemento. Al igual que el `@:` operador,`<text>` etiquetas se usa ASP.NET para identificar el contenido de texto y nunca se representan en el resultado de la página.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    El primer ejemplo se repite el ejemplo anterior, pero utiliza un único par de `<text>` etiquetas para delimitar el texto que se va a representar. En el segundo ejemplo, el `<text>` y `</text>` etiquetas incluir tres líneas, todos los cuales tienen algún texto dependiente y etiquetas HTML no coincidentes (`<br />`), junto con el código de servidor y las etiquetas HTML coincidentes. Una vez más, también puede preceder en cada línea individualmente con la `@:` operador; cualquiera de ellas funciona de manera.

    > [!NOTE]
    > Al generar texto tal y como se muestra en esta sección &#8212; utilizando un elemento HTML, el `@:` (operador), o la `<text>` elemento &#8212; ASP.NET no codificar en HTML el resultado. (Como se indicó anteriormente, ASP.NET codificar la salida de expresiones de código de servidor y de bloques de código de servidor que estén precedidos por `@`, excepto en los casos especiales que se indican en esta sección.)

### <a name="whitespace"></a>Whitespace

Espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un salto de línea en una instrucción no tiene ningún efecto en la instrucción, y puede incluir instrucciones para mejorar la legibilidad. Las instrucciones siguientes son los mismos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Sin embargo, no se puede ajustar una línea en medio de un literal de cadena. No funciona en el ejemplo siguiente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Para combinar una cadena larga que se ajusta a varias líneas al igual que el código anterior, hay dos opciones. Puede utilizar el operador de concatenación (`+`), que verá más adelante en este artículo. También puede usar el `@` carácter que se va a crear una cadena textual literal, tal y como se vio anteriormente en este artículo. Puede interrumpir literales de cadenas literales entre líneas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>(Marcado y código de) los comentarios

Los comentarios permiten dejar notas para usted mismo u otros. También permiten deshabilitar (*Comente*) una sección de código o marcado que no desea ejecutar pero desea conservar en la página por el momento.

Hay diferentes comentar la sintaxis de código Razor o si el marcado HTML. Al igual que con todo el código Razor, Razor comentarios son procesados (y, a continuación, quitar) en el servidor antes de que la página se envía al explorador. Por lo tanto, la sintaxis de comentario de Razor le permite colocar comentarios en el código del sistema (o incluso en el marcado) que puede ver cuando se edita el archivo, pero que los usuarios no vean, incluso en el origen de la página.

Para los comentarios de ASP.NET Razor, inicie el comentario con `@*` una letra y terminar con `*@`. El comentario puede estar en una sola línea o varias líneas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Este es un comentario dentro de un bloque de código:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Aquí está el mismo bloque de código, con la línea de código comentada para que no se ejecuta:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Dentro de un bloque de código, como una alternativa al uso de sintaxis de comentario de Razor, puede usar la sintaxis de comentario del lenguaje de programación que está utilizando, como C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

En C#, los comentarios de una línea están precedidos por la `//` caracteres y los comentarios de varias líneas que comienzan por `/*` una letra y terminar con `*/`. (Al igual que con los comentarios de Razor, C#, los comentarios no se representan en el explorador).

Para el marcado, ya que probablemente sabe, puede crear un comentario HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Comentarios HTML iniciar con `<!--` caracteres y terminan con `-->`. Puede utilizar comentarios HTML para rodear no sólo texto, sino también cualquier marcado HTML que desee conservar en la página pero no desea representar. Este comentario HTML ocultará todo el contenido de las etiquetas y el texto que contienen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

A diferencia de los comentarios de Razor, comentarios HTML *son* representa en la página y el usuario puede ver consultando el origen de la página.

Razor tiene limitaciones en los bloques anidados de C#. Para obtener más información consulte [denominado las Variables de C# y anidar bloquea generar divide el código](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variables

Una variable es un objeto con nombre que utiliza para almacenar los datos. Puede asignar variables, pero el nombre debe comenzar con un carácter alfabético y no puede contener espacios en blanco o caracteres reservados.

### <a name="variables-and-data-types"></a>Las variables y tipos de datos

Una variable puede tener un tipo de datos específico, lo que indica qué tipo de datos se almacena en la variable. Puede hacer que las variables de cadena que almacenan valores de cadena (como &quot;Hola a todos&quot;), las variables de entero que almacenan valores de número entero (por ejemplo, 3 o 79) y las variables de fecha que almacenan valores de fecha en una variedad de formatos (por ejemplo, 12/4/2012 o de marzo de 2009 ). Y hay muchos otros tipos de datos que se puede usar.

Sin embargo, generalmente no tendrá que especificar un tipo de una variable. La mayoría de los casos, ASP.NET puede averiguar el tipo en función de cómo se usa los datos de la variable. (En ocasiones, debe especificar un tipo; verá ejemplos donde esto es true).

Se pueden declarar una variable utilizando el `var` palabra clave (si no desea especificar un tipo) o mediante el nombre del tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

En el ejemplo siguiente se muestra los usos típicos de las variables en una página web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Si se combinan los ejemplos anteriores de una página, verá lo siguiente muestra en un explorador:

![Img9 de Razor](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertir y tipos de datos de pruebas

Aunque por lo general, ASP.NET puede determinar un tipo de datos automáticamente, a veces no es posible. Por lo tanto, deberá ayudan ASP.NET mediante la realización de una conversión explícita. Incluso si no tiene que convertir los tipos, a veces resulta útil probar para ver qué tipo de datos podría estar trabajando con.

El caso más común es que tiene que convertir una cadena a otro tipo, como en un entero o una fecha. En el ejemplo siguiente se muestra un caso típico donde debe convertir una cadena en un número.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Como norma, proporcionados por el usuario aparecerán como cadenas. Aunque le los usuarios escriban un número e incluso si ha especificado un dígito, cuando se envían proporcionados por el usuario y leerlo en el código, los datos están en formato de cadena. Por lo tanto, debe convertir la cadena en un número. En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, el siguiente error se produce, porque ASP.NET no se puede agregar dos cadenas:

*No se puede convertir implícitamente el tipo 'string' a 'int'.*

Para convertir los valores enteros, se llama a la `AsInt` método. Si la conversión se realiza correctamente, a continuación, puede agregar los números.

En la tabla siguiente se enumera algunos métodos de conversión y prueba comunes para las variables.

::: fila:::::: columna::: <strong>método</strong> ::: final de la columna:::::: columna::: <strong>descripción</strong> ::: final de la columna:::::: columna::: <strong>ejemplo</strong> ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `AsInt(), IsInt()` ::: final de la columna:::::: columna::: convierte una cadena que representa un número entero (por ejemplo, "593") en un entero.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `AsBool(), IsBool()` ::: final de la columna:::::: columna::: convierte una cadena como &quot;true&quot; o &quot;false&quot; a un tipo booleano.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `AsFloat(), IsFloat()` ::: final de la columna:::::: columna::: convierte una cadena que tiene un valor decimal como &quot;1.3&quot; o &quot;7.439&quot; un número de punto flotante.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `AsDecimal(), IsDecimal()` ::: final de la columna:::::: columna::: convierte una cadena que tiene un valor decimal como &quot;1.3&quot; o &quot;7.439&quot; en un número decimal. (En ASP.NET, un número decimal es más preciso que un número de punto flotante.) ::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `AsDateTime(), IsDateTime()` ::: final de la columna:::::: columna::: convierte una cadena que representa un valor de fecha y hora para ASP.NET `DateTime` tipo.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `ToString()` ::: final de la columna:::::: columna::: convierte cualquier otro tipo de datos en una cadena.
::: final de la columna:::::: columna: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    ::: final de la columna:::::: fin de la fila:

## <a name="operators"></a>Operadores

Un operador es una palabra clave o un carácter que indica qué tipo de comando que se ejecuta en una expresión de ASP.NET. El lenguaje C# (y la sintaxis de Razor que se basan en ellos) admiten muchos de los operadores, pero solo debe reconocer algunos para empezar a trabajar. En la tabla siguiente se resume los operadores más comunes.


::: fila:::::: columna::: <strong>operador</strong> ::: final de la columna:::::: columna::: <strong>descripción</strong> ::: final de la columna:::::: columna::: <strong>ejemplos</strong> ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `+` `-` `*` `/` ::: final de la columna:::::: columna::: operadores matemáticos usados en expresiones numéricas.
::: final de la columna:::::: columna: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `=` ::: final de la columna:::::: columna::: asignación. Asigna el valor en el lado derecho de una instrucción para el objeto en el lado izquierdo.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `==` ::: final de la columna:::::: columna::: igualdad. Devuelve `true` si los valores son iguales. (Tenga en cuenta la distinción entre el `=` operador y el `==` operador.)::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `!=` ::: final de la columna:::::: columna::: desigualdad. Devuelve `true` si los valores no son iguales.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `< > <= >=` ::: final de la columna:::::: columna::: menor-que, mayor-que, menor o igual que y mayor o igual.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `+` ::: final de la columna:::::: columna::: concatenación, que se usa para combinar cadenas. ASP.NET se conoce la diferencia entre este operador y el operador de suma según el tipo de datos de la expresión.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `+=` `-=` ::: final de la columna:::::: columna::: los operadores de incremento y decremento, que la suma y resta 1 (respectivamente) de una variable.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `.` ::: final de la columna:::::: columna::: punto. Se utiliza para distinguir objetos y sus propiedades y métodos.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `()` ::: final de la columna:::::: columna::: paréntesis. Se utiliza para las expresiones de grupo y para pasar parámetros a métodos.
::: final de la columna:::::: columna: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `[]` ::: final de la columna:::::: columna::: corchetes. Se utiliza para tener acceso a los valores de las matrices o colecciones.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `!` ::: final de la columna:::::: columna::: no. Invierte un `true` valor `false` y viceversa. Se utiliza normalmente como una manera abreviada para comprobar la `false` (es decir, para no `true`).
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    ::: final de la columna:::::: fin de la fila:
* * *
::: fila:::::: columna::: `&&` <code>&#124;&#124;</code> ::: final de la columna:::::: columna::: lógicos y y, que se usan para vincular condiciones o juntos.
::: final de la columna:::::: columna: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    ::: final de la columna:::::: fin de la fila:

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Trabajar con archivos y rutas de acceso de carpeta en el código

A menudo podrá trabajar con rutas de acceso de archivos y carpetas en el código. Este es un ejemplo de estructura de carpetas físico de un sitio Web como podría aparecer en el equipo de desarrollo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

A continuación encontrará algunos detalles esenciales acerca de las direcciones URL y las rutas de acceso:

- Una dirección URL comienza con ya sea un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).
- Una dirección URL que se corresponde con una ruta de acceso física en un equipo host. Por ejemplo, `http://myserver` pueden corresponder a la carpeta *C:\websites\mywebsite* en el servidor.
- Una ruta de acceso virtual es una abreviatura para representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa. Incluye la parte de una dirección URL que sigue al nombre de dominio o servidor. Cuando utilice rutas de acceso virtuales, puede mover el código a un dominio diferente o un servidor sin tener que actualizar las rutas de acceso.

Este es un ejemplo para ayudarle a entender las diferencias:

| Dirección URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nombre del servidor | *mycompanyserver* |
| Ruta de acceso virtual | */HumanResources/CompanyPolicy.htm* |
| Ruta de acceso física | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Es la raíz virtual /, al igual que la raíz de la unidad C: unidad es \. (Las rutas de acceso de la carpeta virtual siempre usan barras diagonales). La ruta de acceso virtual de una carpeta no tiene que tener el mismo nombre que la carpeta física; puede ser un alias. (En los servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta.)

A veces cuando se trabaja con archivos y carpetas en el código, es necesario hacer referencia a la ruta de acceso física y, a veces, una ruta de acceso virtual, dependiendo de qué objetos se trabaja con. ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el `Server.MapPath` método y el `~` operador y `Href` método.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversión de rutas de acceso virtuales a física: el método Server.MapPath

El `Server.MapPath` método convierte una ruta de acceso virtual (como */default.cshtml*) a una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilice este método cada vez que tenga una ruta de acceso física completa. Un ejemplo típico es cuando esté leyendo o escribiendo un archivo de texto o un archivo de imagen en el servidor web.

Normalmente no conoce la ruta de acceso física absoluta del sitio en el servidor de un sitio de hospedaje, por lo que este método puede convertir la ruta de acceso sabe: la ruta de acceso virtual, a la ruta de acceso correspondiente en el servidor automáticamente. Pasar la ruta de acceso virtual a un archivo o carpeta para el método y devuelve la ruta de acceso física:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Hacer referencia a la raíz virtual: el ~ operador y Href (método)

En un *.cshtml* o *vbhtml* archivo, puede hacer referencia a la ruta de acceso de la raíz virtual utilizando la `~` operador. Esto es muy útil porque puede moverse páginas en un sitio y los vínculos que contienen a otras páginas no se interrumpe. También es útil en caso de que alguna vez mover su sitio Web a una ubicación diferente. A continuación se muestran algunos ejemplos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Si el sitio Web es `http://myserver/myapp`, mostramos cómo ASP.NET tratan estas rutas de acceso cuando se ejecuta la página:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(En realidad no verán estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso es lo que eran anteriormente).

Puede usar el `~` operador en el código de servidor (como anteriormente) y en el marcado, similar al siguiente:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

En el marcado, utilice la `~` operador para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y los archivos CSS. Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todos los `~` las referencias a la ruta de acceso adecuada.

## <a name="conditional-logic-and-loops"></a>Bucles y la lógica condicional

Código de servidor ASP.NET le permite realizar tareas basadas en condiciones y escribir código que se repite instrucciones un número específico de veces (es decir, código que se ejecuta un bucle).

### <a name="testing-conditions"></a>Condiciones de pruebas

Para probar una condición sencilla que utiliza el `if` instrucción, que devuelve true o false en función de una prueba que especifique:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

El `if` palabra clave inicia un bloque. La prueba real (condición) está entre paréntesis y devuelve true o false. Las instrucciones que se ejecutan si se cumple la prueba están entre llaves. Un `if` instrucción puede incluir un `else` bloque que especifica las instrucciones que se ejecutarán si la condición es falsa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Puede agregar varias condiciones, mediante una `else if` bloque:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

En este ejemplo, si la primera condición de if bloque no es true, el `else if` se comprueba la condición. Si se cumple esta condición, las instrucciones de la `else if` bloque se ejecutan. Si no se cumple ninguna de las condiciones, las instrucciones de la `else` bloque se ejecutan. Puede agregar cualquier número de o si bloquea y, a continuación, cierre con un `else` bloquear como el &quot;todo lo demás&quot; condición.

Para probar un gran número de condiciones, use un `switch` bloque:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Es el valor que desea probar entre paréntesis (en el ejemplo, el `weekday` variable). Cada prueba individual usa un `case` instrucción que finaliza con un signo de dos puntos (:). Si el valor de un `case` instrucción coincide con el valor de prueba, se ejecuta el código dentro de dicho bloque case. Cerrar cada instrucción case con un `break` instrucción. (Si se olvida de incluir salto en cada uno de ellos `case` bloquear, el código de la siguiente `case` instrucción se ejecutará también.) A `switch` bloque tiene a menudo un `default` instrucción como el último caso para un &quot;todo lo demás&quot; opción que se ejecuta si ninguno de los demás casos son true.

El resultado de los dos últimos bloques condicionales mostrado en un explorador:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Bucle de código

A menudo necesitará ejecutar repetidamente las mismas instrucciones. Para ello, ejecutando un bucle. Por ejemplo, se ejecuta con frecuencia las mismas instrucciones para cada elemento de una colección de datos. Si sabe exactamente el número de veces que desea crear un bucle, puede utilizar un `for` bucle. Este tipo de bucle es especialmente útil para contar o contando hacia abajo:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

El bucle comienza con la `for` (palabra clave), seguido de tres instrucciones entre paréntesis, cada uno termina con un punto y coma.

- Dentro de los paréntesis, la primera instrucción (`var i=10;`) crea un contador y lo inicializa en 10. No es necesario el nombre del contador `i` &#8212; puede usar cualquier variable. Cuando el `for` bucle se ejecuta, el contador se incrementa automáticamente.
- La segunda instrucción (`i < 21;`) establece la condición de hasta qué punto desea contar. En este caso, desea que vaya a un máximo de 20 (es decir, para continuar mientras el contador es inferior a 21).
- La tercera instrucción (`i++` ) usa un operador de incremento, que simplemente especifica que el contador debe tener 1 le agrega cada vez que se ejecuta el bucle.

Dentro de las llaves es el código que se ejecutará en cada iteración del bucle. El marcado crea un nuevo párrafo (`<p>` elemento) cada vez y se agrega una línea a la salida, mostrar el valor de `i` (el contador). Cuando se ejecuta esta página, en el ejemplo se crea 11 líneas mostrar el resultado, con el texto de cada línea que indica el número del elemento.

![Img11 de Razor](introducing-razor-syntax-c/_static/image11.jpg)

Si está trabajando con una colección o matriz, utiliza a menudo un `foreach` bucle. Una colección es un grupo de objetos similares y el `foreach` bucle permite efectuar una tarea en cada elemento de la colección. Este tipo de bucle es conveniente para las colecciones, ya que a diferencia de un `for` bucles, no tendrá que incrementar el contador o establecer un límite. En su lugar, la `foreach` código del bucle simplemente pasa a través de la colección hasta que haya terminado.

Por ejemplo, el código siguiente devuelve los elementos de la `Request.ServerVariables` colección, que es un objeto que contiene información sobre el servidor web. Usa un `foreac` h bucle para mostrar el nombre de cada elemento mediante la creación de un nuevo `<li>` elemento en una lista con viñetas de HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

El `foreach` palabra clave está seguido de paréntesis donde se declara una variable que representa un elemento único en la colección (en el ejemplo, `var item`), seguido por el `in` (palabra clave), seguido de la colección que desee para recorrer en bucle. En el cuerpo de la `foreach` bucle, puede obtener acceso al elemento actual usando la variable que declaró anteriormente.

![Img12 de Razor](introducing-razor-syntax-c/_static/image12.jpg)

Para crear un bucle más general, utilice el `while` instrucción:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` bucle comienza con la `while` (palabra clave), seguido de paréntesis que especifica cuánto tiempo el bucle continúa (aquí, para siempre y cuando `countNum` es inferior a 50), a continuación, el bloque se repita. Bucles normalmente incrementar (agregar a) o disminuir (restar) una variable o un objeto utilizado para el recuento. En el ejemplo, el `+=` operador de suma 1 a `countNum` cada vez que se ejecuta el bucle. (Para disminuir una variable en un bucle que cuenta atrás, usaría el operador de decremento `-=`).

## <a name="objects-and-collections"></a>Objetos y colecciones

Casi todo el contenido de un sitio Web ASP.NET es un objeto, incluida la propia página web. Esta sección describen algunos objetos importantes que trabajará con frecuencia en el código.

### <a name="page-objects"></a>Objetos de la página

El objeto más básico de ASP.NET es la página. Puede tener acceso a propiedades del objeto de página directamente sin ningún objeto de calificación. El código siguiente obtiene la ruta de acceso del archivo de la página, con el `Request` objeto de la página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Para que sea claro que se están haciendo referencia a propiedades y métodos en el objeto de página actual, también puede usar la palabra clave `this` para representar el objeto de página en el código. Este es el ejemplo de código anterior, con `this` agrega para representar la página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Puede utilizar propiedades de la `Page` objeto que se obtendrá una gran cantidad de información, como:

- `Request`. Como ya ha visto, esto es una colección de información sobre la solicitud actual, incluidos el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera.
- `Response`. Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor ha terminado de ejecutarse. Por ejemplo, puede utilizar esta propiedad para escribir información en la respuesta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de colección (matrices y diccionarios)

A *colección* es un grupo de objetos del mismo tipo, como una colección de `Customer` objetos desde una base de datos. ASP.NET contiene muchas colecciones integradas, como el `Request.Files` colección.

A menudo podrá trabajar con datos en colecciones. Dos tipos de colección comunes son el *matriz* y *diccionario*. Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente para almacenar cada elemento:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Con las matrices, se declara un tipo de datos específico, como `string`, `int`, o `DateTime`. Para indicar que la variable puede contener una matriz, agregar corchetes a la declaración (como `string[]` o `int[]`). Se pueden tener acceso a los elementos en una matriz mediante su posición (índice) o mediante el `foreach` instrucción. Los índices de matriz son de base cero &#8212; es decir, el primer elemento está en posición 0, el segundo elemento en la posición 1 y así sucesivamente.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Puede determinar el número de elementos de una matriz obteniendo sus `Length` propiedad. Para obtener la posición de un elemento específico de la matriz (Buscar en la matriz), use el `Array.IndexOf` método. También puede hacer cosas como inversa el contenido de una matriz (la `Array.Reverse` método) u ordenar el contenido (la `Array.Sort` método).

La salida del código de matriz de cadena mostrado en un explorador:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un diccionario es una colección de pares clave/valor, donde proporcionar la clave (o el nombre) para establecer o recuperar el valor correspondiente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Para crear un diccionario, utilice el `new` palabra clave para indicar que se va a crear un nuevo objeto de diccionario. Un diccionario puede asignar a una variable mediante el `var` palabra clave. Indicar los tipos de datos de los elementos del diccionario utilizando corchetes angulares ( `< >` ). Al final de la declaración, debe agregar un par de paréntesis, porque esto es realmente un método que crea un nuevo diccionario.

Para agregar elementos al diccionario, puede llamar a la `Add` método de la variable de diccionario (`myScores` en este caso) y, a continuación, especifique una clave y un valor. Como alternativa, puede usar corchetes para indicar la clave y realice una asignación simple, como en el ejemplo siguiente:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Para obtener un valor del diccionario, especifique la clave entre corchetes:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Llamar a métodos con parámetros

Cuando lea anteriormente en este artículo, los objetos que se han programado con pueden tener métodos. Por ejemplo, un `Database` objeto podría tener un `Database.Connect` método. Muchos métodos también tienen uno o más parámetros. A *parámetro* es un valor que se pasa a un método para habilitar el método completar su tarea. Por ejemplo, un vistazo a una declaración para el `Request.MapPath` método, que toma tres parámetros:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La línea se ha ajustado para mejorar la legibilidad. Recuerde que puede colocar los saltos de línea casi cualquier lugar excepto interior cadenas entre comillas.)

Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada. Los tres parámetros para el método son `virtualPath`, `baseVirtualDir`, y `allowCrossAppMapping`. (Tenga en cuenta que en la declaración, se enumeran los parámetros con los tipos de datos de los datos que aceptará.) Cuando se llama a este método, debe suministrar valores para los tres parámetros.

La sintaxis de Razor le ofrece dos opciones para pasar parámetros a un método: *parámetros posicionales* y *parámetros con nombre*. Para llamar a un método con parámetros posicionales, pasar los parámetros en un orden estricto que se especifica en la declaración del método. (Normalmente sabría este orden, lea la documentación del método.) Debe seguir el orden y no se puede omitir cualquiera de los parámetros &#8212; si es necesario, se pasa una cadena vacía (`""`) o `null` para un parámetro posicional que no tienen un valor para.

En el siguiente ejemplo se da por supuesto que tiene una carpeta denominada *secuencias de comandos* en el sitio Web. El código llama el `Request.MapPath` método y pasa los valores para los tres parámetros en el orden correcto. A continuación, muestra la ruta de acceso asignado resultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Cuando un método tiene muchos parámetros, puede mantener el código sea más legible mediante el uso de parámetros con nombre. Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de dos puntos (:) y, a continuación, el valor. La ventaja de parámetros con nombre es que se puede pasar en cualquier orden que desee. (Una desventaja es que la llamada al método no es lo más compacta).

En el ejemplo siguiente se llama al mismo método que la mostrada anteriormente, pero usa parámetros para proporcionar los valores con nombre:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Como puede ver, los parámetros se pasan en un orden diferente. Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, le devuelven el mismo valor.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Control de errores

### <a name="try-catch-statements"></a>Instrucciones Try-Catch

A menudo, tendrá las instrucciones en el código que puede producir un error por motivos de fuera de su control. Por ejemplo:

- Si el código intenta crear o acceder a un archivo, puede producirse todo tipo de errores. No exista el archivo que desea, puede que esté bloqueado, el código podría no tener permisos y así sucesivamente.
- De forma similar, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, es posible que se pierdan la conexión a la base de datos, los datos que se va a guardar podrían ser no válido y así sucesivamente.

En términos de programación, se llaman a estas situaciones *excepciones*. Si el código encuentra una excepción, genera (produce) un mensaje de error del que, en el mejor, molestar a los usuarios:

![Img14 de Razor](introducing-razor-syntax-c/_static/image14.jpg)

En situaciones donde el código puede encontrar las excepciones y para evitar mensajes de error de este tipo, puede usar `try/catch` instrucciones. En el `try` instrucción, se ejecuta el código que está protegiendo. En uno o varios `catch` instrucciones, puede buscar de determinados errores (tipos específicos de excepciones) que pudieran haberse producido. Puede incluir tantos `catch` instrucciones que necesitan buscar los errores que se anticipación.

> [!NOTE]
> Se recomienda evitar el uso de la `Response.Redirect` método `try/catch` instrucciones, ya que puede provocar una excepción en la página.


En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo. En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que producirá una excepción. El código incluye `catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, lo que ocurre si el nombre de archivo es incorrecto, y `DirectoryNotFoundException`, que se produce si ASP.NET aún no se encuentra la carpeta. (Puede quita el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente.)

Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior. Sin embargo, la `try/catch` sección le ayuda a impedir que el usuario se vean estos tipos de errores.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

**Programación con Visual Basic**


[Apéndice: lenguaje Visual Basic y la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentación de referencia**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[Lenguaje C#](https://msdn.microsoft.com/library/kx37x362.aspx)
