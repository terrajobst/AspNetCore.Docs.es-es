---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: "Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic) | Documentos de Microsoft"
author: tfitzmac
description: "Este apéndice ofrece una visión general de la programación con las páginas Web ASP.NET en Visual Basic, mediante la sintaxis de Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 8f5d223a5944d8adb9fe65c89e87829d18d1c7ee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo ofrece una visión general de la programación con ASP.NET Web Pages mediante la sintaxis Razor y Visual Basic. ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores web.
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


Mayoría de los ejemplos del uso de ASP.NET Web Pages con sintaxis Razor usa C#. Pero la sintaxis de Razor también es compatible con Visual Basic. Para programar una página web ASP.NET en Visual Basic, se crea una página web con un *.vbhtml* extensión de nombre de archivo y, a continuación, agregue el código de Visual Basic. Este artículo proporciona información general sobre cómo trabajar con el lenguaje Visual Basic y la sintaxis para crear las páginas Web ASP.NET.

> [!NOTE]
> Las plantillas de sitio Web predeterminado para Microsoft WebMatrix (**pastelería**, **Galería fotográfica**, y **Starter Site**, etc.) están disponibles en versiones de C# y Visual Basic. Puede instalar las plantillas de Visual Basic, como paquetes de NuGet. Plantillas de sitios Web se instalan en la carpeta raíz de su sitio en una carpeta denominada *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Las sugerencias de programación 8 superior

Esta sección enumeran algunas sugerencias que sea absolutamente necesario conocer al comenzar a escribir código de servidor ASP.NET mediante la sintaxis Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Agregar código a una página con el carácter @

El `@` carácter asociado empiece a expresiones en línea, bloques de instrucción única y bloques de múltiples instrucciones:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

El resultado que se muestra en un explorador:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codificación HTML**
> 
> Al mostrar el contenido de una página con el `@` de caracteres, como en los ejemplos anteriores, ASP.NET codifica en HTML el resultado. Esto reemplaza los caracteres reservados de HTML (como `<` y `>` y `&`) con códigos que permiten a los caracteres que pueden mostrarse como caracteres en una página web no se interprete como etiquetas HTML o entidades. Sin codificación HTML, la salida desde el código de servidor podría no mostrarse correctamente y podría exponer una página a riesgos de seguridad.
> 
> Si su objetivo es generar el marcado HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` a hacer hincapié en texto), vea la sección [combinación de texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.
> 
> Puede leer más sobre la codificación de HTML en [trabajar con formularios HTML en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Incluir bloques de código con código... Código de fin

Un bloque de código incluye una o varias instrucciones de código y se incluye con las palabras clave `Code` y `End Code`. Coloque la apertura `Code` palabra clave inmediatamente después de la `@` carácter &#8212; no puede haber espacios en blanco entre ellos.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

El resultado que se muestra en un explorador:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Dentro de un bloque, finalizar cada instrucción de código con un salto de línea

En un bloque de código de Visual Basic, cada instrucción finaliza con un salto de línea. (Más adelante en el artículo verá una manera de encapsular una instrucción de código larga en varias líneas si es necesario.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilizar variables para almacenar valores

Puede almacenar valores en una *variable*, incluidas las cadenas, números y fechas, etcetera. Crear una nueva variable mediante la `Dim` palabra clave. Puede insertar valores de las variables directamente en una página con `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

El resultado que se muestra en un explorador:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Incluir valores de cadena literal de comillas dobles

A *cadena* es una secuencia de caracteres que se tratan como texto. Para especificar una cadena, escríbalo entre comillas dobles:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Para incrustar comillas dobles dentro de un valor de cadena, inserte dos caracteres de comillas dobles. Si desea que aparezca una vez en el resultado de la página el carácter de comillas dobles, escríbala como `""` en el citado de cadena y si desea que aparezca dos veces, escríbala como `""""` dentro de la cadena entre comillas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

El resultado que se muestra en un explorador:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Código de Visual Basic no distingue mayúsculas de minúsculas

El lenguaje Visual Basic no distingue mayúsculas de minúsculas. Palabras clave de programación (como `Dim`, `If`, y `True`) y los nombres de variable (como `myString`, o `subTotal`) puede escribirse en cualquier caso.

Las siguientes líneas de código asigna un valor a la variable `lastname` con una minúscula nombre y, a continuación, generar el valor de la variable a la página con un nombre en mayúsculas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

El resultado que se muestra en un explorador:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Gran parte de la codificación implica trabajar con objetos

Un objeto representa una cosa que se puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente (filas de la base de datos), etcetera. Los objetos tienen propiedades que describen sus características &#8212; un objeto de cuadro de texto tiene un `Text` propiedad, un objeto de solicitud tiene un `Url` propiedad, un mensaje de correo electrónico tiene un `From` propiedad y un objeto customer tiene una `FirstName` propiedad. Métodos de los objetos también tienen la &quot;verbos&quot; pueden realizar. Algunos ejemplos son un objeto de archivo `Save` /método siguiente, un objeto de imagen `Rotate` método y un objeto de correo electrónico `Send` método.

A menudo trabajará con la `Request` campos de objeto, que proporciona información como los valores de formulario en la página (cuadros de texto, etc.), el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera. Este ejemplo muestra cómo tener acceso a propiedades de la `Request` objeto y cómo llamar a la `MapPath` método de la `Request` objeto, que proporciona la ruta de acceso absoluta de la página en el servidor:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

El resultado que se muestra en un explorador:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Puede escribir código que toma decisiones

Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de condiciones. La manera más común de hacerlo es con la `If` instrucción (y opcionalmente `Else` instrucción).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

La instrucción `If IsPost` es una forma abreviada de escritura `If IsPost = True`. Junto con `If` instrucciones, hay una variedad de maneras de probar condiciones, repita bloques de código, y así sucesivamente, que se describen más adelante en este artículo.

El resultado mostrado en un explorador (después de hacer clic **enviar**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET y POST métodos y la propiedad IsPost**
> 
> El protocolo utilizado para las páginas web (HTTP) es compatible con un número muy limitado de métodos (&quot;verbos&quot;) que se usan para realizar solicitudes al servidor. Los dos más comunes son GET, que se utiliza para leer una página, y POST, que se usa para enviar una página. En general, la primera vez que un usuario solicita una página, la página se solicita mediante GET. Si el usuario rellena un formulario y, a continuación, haga clic en **enviar**, el explorador realiza una solicitud POST al servidor.
> 
> En la programación web, a menudo resulta útil saber si se solicita una página que se va a como una operación GET o como una entrada de blog para que sepa cómo procesar la página. En ASP.NET Web Pages, puede utilizar el `IsPost` propiedad para ver si una solicitud es una operación GET o POST. Si la solicitud es una entrada, el `IsPost` propiedad devolverá true y puede hacer cosas como la lectura de los valores de los cuadros de texto en un formulario. Muchos ejemplos verá mostrarle cómo procesar la página de forma diferente dependiendo del valor de `IsPost`.


## <a name="a-simple-code-example"></a>Un ejemplo de código sencillo

Este procedimiento muestra cómo crear una página que muestra las técnicas de programación básicas. En el ejemplo, cree una página que permite a los usuarios escribir dos números, a continuación, agrega y muestra el resultado.

1. En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers.vbhtml*.
2. Copie el siguiente código y marcado en la página, reemplazando cualquier elemento ya está en la página.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Hay algunas cuestiones que tener en cuenta:

    - El `@` carácter asociado empiece el primer bloque de código en la página y precede a la `totalMessage` variable incrustados en la parte inferior.
    - El bloque en la parte superior de la página se incluye en `Code...End Code`.
    - Las variables `total`, `num1`, `num2`, y `totalMessage` almacenar varios números y una cadena.
    - El valor de cadena literal asignado a la `totalMessage` variable está entre comillas dobles.
    - Dado que el código de Visual Basic no distingue mayúsculas de minúsculas, cuando la `totalMessage` variable se usa en la parte inferior de la página, el nombre solo debe coincidir con la ortografía de la declaración de variable en la parte superior de la página. No importan las mayúsculas y minúsculas.
    - La expresión `num1.AsInt()`  +  `num2.AsInt()` muestra cómo trabajar con objetos y métodos. El `AsInt` método en cada variable convierte la cadena especificada por un usuario en un número entero (un entero) que se pueden agregar.
    - El `<form>` etiqueta incluye un `method="post"` atributo. Esto especifica que cuando el usuario hace clic en **agregar**, la página se enviará al servidor mediante el método HTTP POST. Cuando se envía la página, el código `If IsPost` se evalúa como true y la directiva de ejecución, mostrar el resultado de sumar los números del código.
3. Guarde la página y ejecútelo en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Escriba dos números enteros y, a continuación, haga clic en el **agregar** botón.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Sintaxis y el lenguaje Visual Basic

Anteriormente se mostró un ejemplo básico de cómo crear una página web ASP.NET y cómo puede agregar código de servidor en formato HTML. Aquí aprenderá los conceptos básicos del uso de Visual Basic para escribir código de servidor ASP.NET mediante la sintaxis de Razor &#8212; es decir, las reglas de lenguaje de programación.

Si tiene experiencia con la programación (sobre todo si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que leer aquí le resultará familiar. Probablemente necesitará para familiarizarse con cómo se agrega código de WebMatrix a marcado en *.vbhtml* archivos.

### <a id="BM_CombiningTextMarkupAndCode"></a>Combinación de texto, marcado y código en bloques de código

En bloques de código de servidor, es a menudo conveniente para la salida de texto y marcado en la página. Si un bloque de código de servidor contiene texto que no es código y que en su lugar, se debe representar tal cual, debe ser capaz de distinguir entre el texto de ASP.NET. Existen varias formas de hacerlo.

- El texto se incluya en un elemento de bloque HTML como `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    El elemento HTML puede incluir texto, los elementos adicionales de HTML y expresiones de código del servidor. Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), se representa todo el contenido del elemento y su contenido como está en el explorador (y resuelve las expresiones de código del servidor).

- Use la `@:` operador o `<text>` elemento. El `@:` genera una sola línea de contenido que contiene texto sin formato o las etiquetas HTML no coincidentes; el `<text>` elemento abarca varias líneas a la salida. Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    En el ejemplo siguiente se repite el ejemplo anterior, pero utiliza un único par de `<text>` etiquetas para delimitar el texto que se va a representar.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    En el ejemplo siguiente, la `<text>` y `</text>` etiquetas incluir tres líneas, todos los cuales tienen algún texto dependiente y etiquetas HTML no coincidentes (`<br />`), junto con el código de servidor y las etiquetas HTML coincidentes. Una vez más, también puede preceder en cada línea individualmente con la `@:` operador; cualquiera de ellas funciona de manera.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Al generar texto tal y como se muestra en esta sección &#8212; utilizando un elemento HTML, el `@:` (operador), o la `<text>` elemento &#8212; ASP.NET no codificar en HTML el resultado. (Como se indicó anteriormente, ASP.NET codificar la salida de expresiones de código de servidor y de bloques de código de servidor que estén precedidos por `@`, excepto en los casos especiales que se indican en esta sección.)

### <a name="whitespace"></a>Whitespace

Espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dividir instrucciones largas en varias líneas

Puede interrumpir una instrucción de código larga en varias líneas mediante el carácter de subrayado `_` (que en Visual Basic se llama a la *carácter de continuación*) después de cada línea de código. Para interrumpir una instrucción en la línea siguiente, al final de la línea, agregue un espacio y, a continuación, el carácter de continuación. Continuar con la instrucción en la línea siguiente. Puede ajustar las instrucciones en líneas tantos como necesite para mejorar la legibilidad. Las instrucciones siguientes son los mismos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Sin embargo, no se puede ajustar una línea en medio de un literal de cadena. No funciona en el ejemplo siguiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Para combinar una cadena larga que se ajusta a varias líneas al igual que el código anterior, tendría que usar la *operador de concatenación* (`&`), que verá más adelante en este artículo.

### <a name="code-comments"></a>Comentarios en código

Los comentarios permiten dejar notas para usted mismo u otros. Comentarios de la sintaxis de Razor tienen el prefijo `@*` una letra y terminar con `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dentro de los bloques de código puede usar los comentarios de la sintaxis de Razor, o puede usar normal carácter de comentario de Visual Basic, que es una comilla simple (`'`) como prefijo a cada línea.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variables

Una variable es un objeto con nombre que utiliza para almacenar los datos. Puede asignar variables, pero el nombre debe comenzar con un carácter alfabético y no puede contener espacios en blanco o caracteres reservados. En Visual Basic, tal y como vio anteriormente, no importa el caso de las letras de un nombre de variable.

### <a name="variables-and-data-types"></a>Las variables y tipos de datos

Una variable puede tener un tipo de datos específico, lo que indica qué tipo de datos se almacena en la variable. Puede hacer que las variables de cadena que almacenan valores de cadena (como &quot;Hola a todos&quot;), las variables de entero que almacenan valores de número entero (por ejemplo, 3 o 79) y las variables de fecha que almacenan valores de fecha en una variedad de formatos (por ejemplo, 12/4/2012 o de marzo de 2009 ). Y hay muchos otros tipos de datos que se puede usar.

Sin embargo, no tendrá que especificar un tipo de una variable. En la mayoría de los casos, ASP.NET puede averiguar el tipo en función de cómo se usa los datos de la variable. (En ocasiones, debe especificar un tipo; verá ejemplos donde esto es true).

Para declarar una variable sin especificar un tipo, use `Dim` junto con el nombre de variable (por ejemplo, `Dim myVar`). Para declarar una variable con un tipo, use `Dim` junto con el nombre de variable, seguido de `As` y, a continuación, el nombre de tipo (por ejemplo, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

El ejemplo siguiente muestra algunas expresiones en línea que utilizan las variables en una página web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

El resultado que se muestra en un explorador:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertir y tipos de datos de pruebas

Aunque por lo general, ASP.NET puede determinar un tipo de datos automáticamente, a veces no es posible. Por lo tanto, deberá ayudan ASP.NET mediante la realización de una conversión explícita. Incluso si no tiene que convertir los tipos, a veces resulta útil probar para ver qué tipo de datos podría estar trabajando con.

El caso más común es que tiene que convertir una cadena a otro tipo, como en un entero o una fecha. En el ejemplo siguiente se muestra un caso típico donde debe convertir una cadena en un número.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Como norma, proporcionados por el usuario aparecerán como cadenas. Incluso si se ha pide al usuario que escriba un número e incluso si ha escrito un dígito, cuando se envían proporcionados por el usuario y leerlo en el código, los datos están en formato de cadena. Por lo tanto, debe convertir la cadena en un número. En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, el siguiente error se produce, porque ASP.NET no se puede agregar dos cadenas:

`Cannot implicitly convert type 'string' to 'int'.`

Para convertir los valores enteros, se llama a la `AsInt` método. Si la conversión se realiza correctamente, a continuación, puede agregar los números.

En la tabla siguiente se enumera algunos métodos de conversión y prueba comunes para las variables.

| **Método** | **Descripción** | **Ejemplo** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Convierte una cadena que representa un número entero (como &quot;593&quot;) en un entero. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
| `AsBool(), IsBool()` | Convierte una cadena como &quot;true&quot; o &quot;false&quot; a un tipo booleano. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
| `AsFloat(), IsFloat()` | Convierte una cadena que tiene un valor decimal como &quot;1.3&quot; o &quot;7.439&quot; un número de punto flotante. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
| `AsDecimal(), IsDecimal()` | Convierte una cadena que tiene un valor decimal como &quot;1.3&quot; o &quot;7.439&quot; en un número decimal. (En ASP.NET, un número decimal es más preciso que un número de punto flotante.) | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` | Convierte una cadena que representa un valor de fecha y hora para ASP.NET `DateTime` tipo. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
| `ToString()` | Cualquier otro tipo de datos se convierte en una cadena. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>Operadores

Un operador es una palabra clave o un carácter que indica qué tipo de comando que se ejecuta en una expresión de ASP.NET. Visual Basic admite muchos de los operadores, pero solo debe reconocer algunos a empezar a desarrollar páginas web de ASP.NET. En la tabla siguiente se resume los operadores más comunes.

| **Operator** | **Descripción** | **Ejemplos** |
| --- | --- | --- |
| `+ - * /` | Operadores matemáticos usados en expresiones numéricas. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)] |
| `=` | Asignación y comparaciones de igualdad. Según el contexto, asigna el valor en el lado derecho de una instrucción para el objeto en el lado izquierdo o comprueba los valores para la igualdad. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)] |
| `<>` | Desigualdad. Devuelve `True` si los valores no son iguales. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)] |
| `< > <= >=` | Menor que, mayor que, menor que o igual a y mayor que o igual. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)] |
| `&` | Concatenación, que se usa para combinar cadenas. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
| `+= -=` | Los operadores de incremento y decremento, que la suma y resta 1 (respectivamente) de una variable. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)] |
| `.` | Punto. Se utiliza para distinguir objetos y sus propiedades y métodos. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)] |
| `()` | Paréntesis. Se usa en expresiones de grupo, para pasar parámetros a métodos y acceder a los miembros de matrices y colecciones. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
| `Not` | No. Invierte un valor true a false y viceversa. Se utiliza normalmente como una manera abreviada para comprobar la `False` (es decir, para no `True`). | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)] |
| `AndAlso OrElse` | Operador lógico AND y OR, que se usan para vincular condiciones. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)] |

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
| Ruta de acceso virtual | */humanresources/CompanyPolicy.htm* |
| Ruta de acceso física | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Es la raíz virtual /, al igual que la raíz de la unidad C: unidad es \. (Las rutas de acceso de la carpeta virtual siempre usan barras diagonales). La ruta de acceso virtual de una carpeta no tiene que tener el mismo nombre que la carpeta física; puede ser un alias. (En los servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta.)

A veces cuando se trabaja con archivos y carpetas en el código, es necesario hacer referencia a la ruta de acceso física y, a veces, una ruta de acceso virtual, dependiendo de qué objetos se trabaja con. ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el `Server.MapPath` método y el `~` operador y `Href` método.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversión de rutas de acceso virtuales a física: el método Server.MapPath

El `Server.MapPath` método convierte una ruta de acceso virtual (como */default.cshtml*) a una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilice este método cada vez que tenga una ruta de acceso física completa. Un ejemplo típico es cuando esté leyendo o escribiendo un archivo de texto o un archivo de imagen en el servidor web.

Normalmente no conoce la ruta de acceso física absoluta del sitio en el servidor de un sitio de hospedaje, por lo que este método puede convertir la ruta de acceso sabe: la ruta de acceso virtual, a la ruta de acceso correspondiente en el servidor automáticamente. Pasar la ruta de acceso virtual a un archivo o carpeta para el método y devuelve la ruta de acceso física:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Hacer referencia a la raíz virtual: el ~ operador y Href (método)

En un *.cshtml* o *vbhtml* archivo, puede hacer referencia a la ruta de acceso de la raíz virtual utilizando la `~` operador. Esto es muy útil porque puede moverse páginas en un sitio y los vínculos que contienen a otras páginas no se interrumpe. También es útil en caso de que alguna vez mover su sitio Web a una ubicación diferente. A continuación se muestran algunos ejemplos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Si el sitio Web es `http://myserver/myapp`, mostramos cómo ASP.NET tratan estas rutas de acceso cuando se ejecuta la página:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(En realidad no verán estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso es lo que eran anteriormente).

Puede usar el `~` operador en el código de servidor (como anteriormente) y en el marcado, similar al siguiente:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

En el marcado, utilice la `~` operador para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y los archivos CSS. Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todos los `~` las referencias a la ruta de acceso adecuada.

## <a name="conditional-logic-and-loops"></a>Bucles y la lógica condicional

Código de servidor ASP.NET le permite realizar tareas basadas en condiciones y la escritura de código que se repite las instrucciones de código, es decir, un número específico de veces que se ejecuta un bucle).

### <a name="testing-conditions"></a>Condiciones de pruebas

Para probar una condición sencilla que utiliza el `If...Then` instrucción, que devuelve `True` o `False` en función de una prueba que especifique:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

El `If` palabra clave inicia un bloque. La prueba real (condición) sigue el `If` (palabra clave) y devuelve true o false. El `If` instrucción termina con `Then`. Se incluyen las instrucciones que se ejecutarán si se cumple la prueba por `If` y `End If`. Un `If` instrucción puede incluir un `Else` bloque que especifica las instrucciones que se ejecutarán si la condición es falsa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Si un `If` instrucción inicia un bloque de código, no tendrá que usar la normal `Code...End Code` instrucciones para incluir los bloques. Puedes agregar `@` al bloque, y que funcione. Este enfoque funciona con `If` , así como otra palabras clave que van seguidos de bloques de código, incluidos los de programación de Visual Basic `For`, `For Each`, `Do While`, etcetera.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Puede agregar varias condiciones mediante uno o más `ElseIf` bloques:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

En este ejemplo, si la primera condición en la `If` bloque no es true, el `ElseIf` se comprueba la condición. Si se cumple esta condición, las instrucciones de la `ElseIf` bloque se ejecutan. Si no se cumple ninguna de las condiciones, las instrucciones de la `Else` bloque se ejecutan. Puede agregar cualquier número de `ElseIf` bloquea y, a continuación, cierre con un `Else` bloquear como el &quot;todo lo demás&quot; condición.

Para probar un gran número de condiciones, use un `Select Case` bloque:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Es el valor que desea probar entre paréntesis (en el ejemplo, la variable de día de la semana). Cada prueba individual usa un `Case` instrucción que se muestra un valor. Si el valor de un `Case` instrucción coincide con el valor de prueba, el código que `Case` bloque se ejecuta.

El resultado de los dos últimos bloques condicionales mostrado en un explorador:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Bucle de código

A menudo necesitará ejecutar repetidamente las mismas instrucciones. Para ello, ejecutando un bucle. Por ejemplo, se ejecuta con frecuencia las mismas instrucciones para cada elemento de una colección de datos. Si sabe exactamente el número de veces que desea crear un bucle, puede utilizar un `For` bucle. Este tipo de bucle es especialmente útil para contar o contando hacia abajo:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

El bucle comienza con la `For` (palabra clave), seguido de tres elementos:

- Inmediatamente después de la `For` la instrucción, se declara una variable de contador (no tiene que usar `Dim`) y, a continuación, indique el intervalo, como en `i = 10 to 20`. Esto significa que la variable `i` se empezará a contar en 10 y puede continuar hasta que llega a 20 (ambos inclusive).
- Entre los `For` y `Next` instrucciones es el contenido del bloque. Esto puede contener una o varias instrucciones de código que se ejecutan con cada bucle.
- El `Next i` instrucción finaliza el bucle. Incrementa el contador y comienza la siguiente iteración del bucle.

La línea de código entre la `For` y `Next` líneas contiene el código que se ejecuta para cada iteración del bucle. El marcado crea un nuevo párrafo (`<p>` elemento) cada vez y se agrega una línea a la salida, mostrar el valor de i (el contador). Cuando se ejecuta esta página, en el ejemplo se crea 11 líneas mostrar el resultado, con el texto de cada línea que indica el número del elemento.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Si está trabajando con una colección o matriz, utiliza a menudo un `For Each` bucle. Una colección es un grupo de objetos similares y el `For Each` bucle permite efectuar una tarea en cada elemento de la colección. Este tipo de bucle es conveniente para las colecciones, ya que a diferencia de un `For` bucles, no tendrá que incrementar el contador o establecer un límite. En su lugar, la `For Each` código del bucle simplemente pasa a través de la colección hasta que haya terminado.

Este ejemplo devuelve los elementos de la `Request.ServerVariables` colección (que contiene información sobre el servidor web). Usa un `For Each` bucle para mostrar el nombre de cada elemento mediante la creación de un nuevo `<li>` elemento en una lista con viñetas de HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

El `For Each` palabra clave va seguida de una variable que representa un elemento único en la colección (en el ejemplo, `myItem`), seguido por el `In` (palabra clave), seguido de la colección que desee para recorrer en bucle. En el cuerpo de la `For Each` bucle, puede obtener acceso al elemento actual usando la variable que declaró anteriormente.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Para crear un bucle más general, utilice el `Do While` instrucción:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Este bucle comienza con la `Do While` (palabra clave), seguido de una condición, seguido por el bloque se repita. Bucles normalmente incrementar (agregar a) o disminuir (restar) una variable o un objeto utilizado para el recuento. En el ejemplo, el `+=` operador de suma 1 al valor de una variable cada vez que se ejecuta el bucle. (Para disminuir una variable en un bucle que cuenta atrás, usaría el operador de decremento `-=`.)

## <a name="objects-and-collections"></a>Objetos y colecciones

Casi todo el contenido de un sitio Web ASP.NET es un objeto, incluida la propia página web. Esta sección describen algunos objetos importantes que trabajará con frecuencia en el código.

### <a name="page-objects"></a>Objetos de la página

El objeto más básico de ASP.NET es la página. Puede tener acceso a propiedades del objeto de página directamente sin ningún objeto de calificación. El código siguiente obtiene la ruta de acceso del archivo de la página, con el `Request` objeto de la página:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Puede utilizar propiedades de la `Page` objeto que se obtendrá una gran cantidad de información, como:

- `Request`. Como ya ha visto, esto es una colección de información sobre la solicitud actual, incluidos el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera.
- `Response`. Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor ha terminado de ejecutarse. Por ejemplo, puede utilizar esta propiedad para escribir información en la respuesta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de colección (matrices y diccionarios)

Una colección es un grupo de objetos del mismo tipo, como una colección de `Customer` objetos desde una base de datos. ASP.NET contiene muchas colecciones integradas, como el `Request.Files` colección.

A menudo podrá trabajar con datos en colecciones. Dos tipos de colección comunes son el *matriz* y *diccionario*. Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente para almacenar cada elemento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con las matrices, se declara un tipo de datos específico, como `String`, `Integer`, o `DateTime`. Para indicar que la variable puede contener una matriz, agregar paréntesis para el nombre de la declaración de variable (como `Dim myVar() As String`). Se pueden tener acceso a los elementos en una matriz mediante su posición (índice) o mediante el `For Each` instrucción. Matriz de índices de base cero de se &#8212; es decir, el primer elemento está en posición 0, el segundo elemento está en la posición 1 y así sucesivamente.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Puede determinar el número de elementos de una matriz obteniendo sus `Length` propiedad. Para obtener la posición de un elemento específico de la matriz (es decir, buscar en la matriz), use el `Array.IndexOf` método. También puede hacer cosas como inversa el contenido de una matriz (la `Array.Reverse` método) u ordenar el contenido (la `Array.Sort` método).

La salida del código de matriz de cadena mostrado en un explorador:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un diccionario es una colección de pares clave/valor, donde proporcionar la clave (o el nombre) para establecer o recuperar el valor correspondiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Para crear un diccionario, utilice el `New` palabra clave para indicar que se va a crear un nuevo `Dictionary` objeto. Un diccionario puede asignar a una variable mediante el `Dim` palabra clave. Indicar los tipos de datos de los elementos en el diccionario de uso de paréntesis ( `( )` ). Al final de la declaración, debe agregar otro par de paréntesis, porque esto es realmente un método que crea un nuevo diccionario.

Para agregar elementos al diccionario, puede llamar a la `Add` método de la variable de diccionario (`myScores` en este caso) y, a continuación, especifique una clave y un valor. Como alternativa, puede usar paréntesis para indicar la clave y realice una asignación simple, como en el ejemplo siguiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Para obtener un valor del diccionario, especifique la clave entre paréntesis:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Llamar a métodos con parámetros

Como vimos anteriormente en este artículo, los objetos que se han programado con tienen métodos. Por ejemplo, un `Database` objeto podría tener un `Database.Connect` método. Muchos métodos también tienen uno o más parámetros. A *parámetro* es un valor que se pasa a un método para habilitar el método completar su tarea. Por ejemplo, un vistazo a una declaración para el `Request.MapPath` método, que toma tres parámetros:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada. Los tres parámetros para el método son `virtualPath`, `baseVirtualDir`, y `allowCrossAppMapping`. (Tenga en cuenta que en la declaración, se enumeran los parámetros con los tipos de datos de los datos que aceptará.) Cuando se llama a este método, debe suministrar valores para los tres parámetros.

Cuando se está utilizando Visual Basic con la sintaxis de Razor, tienes dos opciones para pasar parámetros a un método: *parámetros posicionales* o *parámetros con nombre*. Para llamar a un método con parámetros posicionales, pasar los parámetros en un orden estricto que se especifica en la declaración del método. (Normalmente sabría este orden, lea la documentación del método.) Debe seguir el orden y no se puede omitir cualquiera de los parámetros &#8212; Si es necesario, se pasa una cadena vacía (`""`) o null para un parámetro posicional que no tienen un valor para.

En el siguiente ejemplo se da por supuesto que tiene una carpeta denominada *secuencias de comandos* en el sitio Web. El código llama el `Request.MapPath` método y pasa los valores para los tres parámetros en el orden correcto. A continuación, muestra la ruta de acceso asignado resultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Cuando hay demasiados parámetros para un método, puede mantener el código sea más legible y más limpia mediante el uso de parámetros con nombre. Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de `:=` y, a continuación, proporcione el valor. Una ventaja de parámetros con nombre es que puede agregarlos en cualquier orden que desee. (Una desventaja es que la llamada al método no es lo más compacta).

En el ejemplo siguiente se llama al mismo método que la mostrada anteriormente, pero usa parámetros para proporcionar los valores con nombre:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Como puede ver, los parámetros se pasan en un orden diferente. Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, le devuelven el mismo valor.

## <a name="handling-errors"></a>Control de errores

### <a name="try-catch-statements"></a>Instrucciones Try-Catch

A menudo, tendrá las instrucciones en el código que puede producir un error por motivos de fuera de su control. Por ejemplo:

- Si el código intenta abrir, crear, leer o escribir un archivo, que podría producirse todo tipo de errores. No exista el archivo que desea, puede que esté bloqueado, el código podría no tener permisos y así sucesivamente.
- De forma similar, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, es posible que se pierdan la conexión a la base de datos, los datos que se va a guardar podrían ser no válido y así sucesivamente.

En términos de programación, se llaman a estas situaciones *excepciones*. Si el código encuentra una excepción, genera (produce) un mensaje de error es decir, en el mejor, molestar a los usuarios.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

En situaciones donde el código puede encontrar las excepciones y para evitar mensajes de error de este tipo, puede usar `Try/Catch` instrucciones. En el `Try` instrucción, se ejecuta el código que está protegiendo. En uno o varios `Catch` instrucciones, puede buscar de determinados errores (tipos específicos de excepciones) que pudieran haberse producido. Puede incluir tantos `Catch` instrucciones que necesitan buscar errores que está anticipación.

> [!NOTE]
> Se recomienda evitar el uso de la `Response.Redirect` método `Try/Catch` instrucciones, ya que puede provocar una excepción en la página.


En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo. En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que producirá una excepción. El código incluye `Catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, lo que ocurre si el nombre de archivo es incorrecto, y `DirectoryNotFoundException`, que se produce si ASP.NET aún no se encuentra la carpeta. (Puede quita el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente.)

Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior. Sin embargo, la `Try/Catch` sección le ayuda a impedir que el usuario se vean estos tipos de errores.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Recursos adicionales

### <a name="reference-documentation"></a>Documentación de referencia

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic (lenguaje)](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
