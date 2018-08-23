---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: 'Introducción a ASP.NET Web Pages: conceptos básicos de formularios HTML | Microsoft Docs'
author: tfitzmac
description: Este tutorial muestra los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora que ya...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 5cf628b63e868e3f76aa19879d4af554241979c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829127"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introducción a las páginas Web ASP.NET - conceptos básicos de formularios HTML
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora que tiene una base de datos, deberá usar sus habilidades de formulario para que los usuarios puedan buscar películas específicas en la base de datos. Supone que ha completado la serie a través de [Introducción para mostrar datos utilizando ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Lo que aprenderá:
> 
> - Cómo crear un formulario mediante el uso de los elementos HTML estándar.
> - Cómo leer el usuario de la entrada en un formulario.
> - Cómo crear una consulta SQL que selectivamente obtiene los datos mediante una búsqueda de términos el usuario proporciona.
> - Cómo hacer que los campos en la página "recuerda" el usuario especificado.
>   
> 
> Características y tecnologías tratadas:
> 
> - Objeto `Request`.
> - El código SQL `Where` cláusula.


## <a name="what-youll-build"></a>¿Qué va a crear

En el tutorial anterior, creó una base de datos, agrega datos a ella y, a continuación, usa el `WebGrid` auxiliar para mostrar los datos. En este tutorial, agregará un cuadro de búsqueda que permite buscar películas de un género concreto o cuyo título contiene cualquier palabra que escriba. (Por ejemplo, podrá encontrar todas las películas que pertenezcan al género es "Acción" o cuyo título contiene "Harry" o "Adventure".)

Cuando haya terminado con este tutorial, tendrá una página como esta:

![Página de películas con búsqueda de género y título](form-basics/_static/image1.png)

La parte de la lista de la página es igual que en el último tutorial &mdash; una cuadrícula. La diferencia será que la cuadrícula mostrará solo las películas que va a buscar.

## <a name="about-html-forms"></a>Acerca de los formularios HTML

(Si tiene experiencia con la creación de formularios HTML y con la diferencia entre `GET` y `POST`, puede omitir esta sección.)

Un formulario tiene elementos de entrada de usuario &mdash; cuadros de texto, botones, botones de opción, casillas, listas desplegables y así sucesivamente. Los usuarios rellenar estos controles o realice las selecciones y, a continuación, envíen el formulario, haga clic en un botón.

La sintaxis básica de HTML de un formulario se muestra en este ejemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Cuando este marcado se ejecuta en una página, crea un formulario simple que es similar a esta ilustración:

![Formulario HTML básico como representado en el explorador](form-basics/_static/image2.png)

El `<form>` elemento abarca los elementos HTML que se envíen. (Una fácil cometer consiste en Agregar elementos a la página pero se olvida, a continuación, colocarlos dentro un `<form>` elemento. En ese caso, no hay nada que se envía.) El `method` atributo indica al explorador cómo enviar la entrada del usuario. Se establece en `post` si va a realizar una actualización en el servidor o a `get` si simplemente está obteniendo datos del servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST y seguridad de verbo HTTP**
> 
> HTTP, el protocolo que usan los exploradores y servidores para intercambiar información, es sorprendentemente sencilla en sus operaciones básicas. Los exploradores utilizan sólo unos cuantos verbos para realizar solicitudes a los servidores. Al escribir código para la web, es útil para comprender estos verbos y usan de cómo el explorador y el servidor. De lejos los verbos más usados son los siguientes:
> 
> - `GET`. El explorador utiliza este verbo para capturar algo desde el servidor. Por ejemplo, cuando escriba una dirección URL en el explorador, el explorador realiza una `GET` operación para solicitar la página que desee. Si la página incluye gráficos, el explorador realiza adicionales `GET` operaciones para obtener las imágenes. Si el `GET` operación tiene que pasar información al servidor, la información se pasa como parte de la dirección URL en la cadena de consulta.
> - `POST`. El explorador envía una `POST` solicitud con el fin de enviar los datos que se agregarán o se cambian en el servidor. Por ejemplo, el `POST` verbo se utiliza para crear registros en una base de datos o cambiar las existentes. La mayoría de los casos, al rellenar un formulario y haga clic en el botón de envío, el explorador realiza una `POST` operación. En un `POST` operación, los datos que se pasan al servidor están en el cuerpo de la página.
> 
> Es una diferencia importante entre estos verbos que un `GET` operación no debe hacer ningún cambio en el servidor, o ponerlo en una forma un poco más abstracta, un `GET` operación no produce un cambio de estado en el servidor. Puede realizar un `GET` operación en los mismos recursos tantas veces como desee, y no cambian esos recursos. (Un `GET` operación a menudo se dice "seguras", o para usar un término técnico, es *idempotente*.) En cambio, por supuesto, un `POST` solicitud cambia algo en el servidor cada vez que realice la operación.
> 
> Dos ejemplos que le ayudarán a ilustrar esta distinción. Al realizar una búsqueda mediante un motor como Bing o Google, rellene un formulario que consta de un cuadro de texto y, a continuación, haga clic en el botón de búsqueda. El explorador realiza una `GET` operación, con el valor especificado en el cuadro que se pasa como parte de la dirección URL. Mediante un `GET` operación para este tipo de formulario está bien, ya que una operación de búsqueda no cambia los recursos en el servidor, solo captura información.
> 
> Ahora, considere el proceso de pedidos en línea. Rellene los detalles del pedido y, a continuación, haga clic en el botón Enviar. Esta operación será un `POST` solicitar, porque la operación dará como resultado cambios en el servidor, como un nuevo registro de pedido, un cambio en la información de su cuenta y quizás muchos otros cambios. A diferencia de la `GET` operación, no se puede repetir el `POST` solicitud, si lo hiciera, cada vez que se vuelven a enviar la solicitud, se generaría un pedido nuevo en el servidor. (En casos como éste, los sitios Web a menudo le advertirá que no haga clic en un botón de envío de más de una vez o deshabilitará el botón Enviar para que no vuelva a enviar el formulario por accidente.)
> 
> En el transcurso de este tutorial, deberá usar un `GET` operación y un `POST` operación para trabajar con formularios HTML. Explicaremos en cada caso porqué el verbo que use es el adecuado.
> 
> (Para más información acerca de los verbos HTTP, consulte el [definiciones de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artículo en el sitio de W3C.)


La mayoría los elementos de entrada de usuario son HTML `<input>` elementos. Parecen `<input type="type" name="name">,` donde *tipo* indica el tipo de control de entrada de usuario que desee. Estos elementos son los más comunes:

- Cuadro de texto: `<input type="text">`
- Casilla de verificación: `<input type="check">`
- Botón de radio: `<input type="radio">`
- Botón: `<input type="button">`
- Botón de envío: `<input type="submit">`

También puede usar el `<textarea>` elemento para crear un cuadro de texto multilínea y `<select>` elemento para crear una lista desplegable o lista desplazable. (Para más información sobre la HTML los elementos del formulario, consulte [formularios HTML y entrada](http://www.w3schools.com/html/html_forms.asp) en el sitio W3Schools.)

El `name` atributo es muy importante, porque el nombre es cómo se obtendrá el valor del elemento más adelante, como verá en breve.

La parte más interesante es, el desarrollador de páginas, ¿qué con la entrada del usuario. No hay ningún comportamiento integrado asociado con estos elementos. En su lugar, tendrá que obtener los valores que el usuario ha entrado o seleccionado y hacer algo con ellos. Eso es lo que aprenderá en este tutorial.

> [!TIP] 
> 
> **HTML5 y formularios de entrada**
> 
> Como ya sabe, HTML está en transición y la versión más reciente (HTML5) incluye compatibilidad con las formas más intuitivas para los usuarios escriban información. Por ejemplo, en HTML5, (el desarrollador de páginas) puede indicar la página que desea que el usuario que escriba una fecha. A continuación, el explorador puede mostrar automáticamente un calendario, en lugar de requerir al usuario que escriba una fecha manualmente. Sin embargo, HTML5 es nuevo y aún no se admite en todos los exploradores.
> 
> ASP.NET Web Pages es compatible con HTML5 en la medida en que examinador del usuario hace de entrada. Para obtener una idea de los nuevos atributos para el `<input>` el elemento en HTML5, vea [HTML &lt;entrada&gt; atributo type](http://www.w3schools.com/html/html_form_input_types.asp) en el sitio W3Schools.


## <a name="creating-the-form"></a>Crear el formulario

En WebMatrix, en el **archivos** área de trabajo, abra el *Movies.cshtml* página.

Después del cierre `</h1>` etiqueta y antes de la apertura `<div>` etiqueta de la `grid.GetHtml` llamada, agregue el siguiente marcado:

[!code-html[Main](form-basics/samples/sample2.html)]

Este marcado crea un formulario que tiene un cuadro de texto denominado `searchGenre` y un botón de envío. El texto del cuadro y enviar el botón se incluyen en un `<form>` elemento cuyo `method` atributo está establecido en `get`. (Recuerde que si no coloca el cuadro de texto y botón dentro de envío un `<form>` elemento, nada se enviarán al hacer clic en el botón.) Usa el `GET` verbo aquí porque se está creando un formulario que no realice cambios en el servidor, solo resulta en una búsqueda. (En el tutorial anterior, ha utilizado un `post` método, que es cómo enviar los cambios realizados en el servidor. Verá en el siguiente tutorial nuevo.)

Ejecute la página. Aunque no ha definido ningún comportamiento del formulario, puede ver su aspecto:

![Página de películas con el cuadro de búsqueda de género](form-basics/_static/image3.png)

Escriba un valor en el cuadro de texto, como "Comedia". A continuación, haga clic en **búsqueda género**.

Tome nota de la dirección URL de la página. Porque se configuró el `<form>` del elemento `method` atribuir a `get`, el valor especificado es ahora parte de la cadena de consulta en la dirección URL, similar al siguiente:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Leer valores de formulario

La página ya contiene código que obtiene de la base de datos y muestra los resultados en una cuadrícula. Ahora debe agregar código que lee el valor del cuadro de texto para que pueda ejecutar una consulta SQL que incluye el término de búsqueda.

Dado que el método del formulario se establece en `get`, puede leer el valor que se especificó en el cuadro de texto utilizando código similar al siguiente:

`var searchTerm = Request.QueryString["searchGenre"];`

El `Request.QueryString` objeto (el `QueryString` propiedad de la `Request` objeto) incluye los valores de los elementos que se enviaron como parte de la `GET` operación. El `Request.QueryString` propiedad contiene un *colección* (lista) de los valores que se envían en el formulario. Para obtener cualquier valor individual, especifique el nombre del elemento que desee. Por eso debe tener un `name` atributo el `<input>` elemento (`searchTerm`) que crea el cuadro de texto. (Para más información sobre la `Request` de objetos, consulte el [sidebar](#BKMK_TheRequestObject) más adelante.)

Es lo suficientemente sencilla para leer el valor del cuadro de texto. Sin embargo, si el usuario no escribe nada en absoluto en el cuadro de texto, pero hace clic en **búsqueda** de todos modos, puede omitir ese clic, ya que no hay nada que buscar.

El código siguiente es un ejemplo que muestra cómo implementar estas condiciones. (No tiene que agregar este código aún; lo hará en un momento).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

La prueba se divide de esta manera:

- Obtener el valor de `Request.QueryString["searchGenre"]`, concretamente, el valor que se especificó en el `<input>` elemento denominado `searchGenre`.
- Averiguar si está vacío utilizando el `IsEmpty` método. Este método es el método estándar para determinar si algo (por ejemplo, un elemento de formulario) contiene un valor. Pero en realidad, ocupa sólo si se ha *no* vacía, por lo tanto...
- Agregar el `!` operador delante de la `IsEmpty` probar. (El `!` operador significa NOT lógico).

En términos sencillos, toda la `if` condición se traduce en lo siguiente: *si el elemento de searchGenre del formulario no está vacío, a continuación...*

Este bloque prepara el terreno para crear una consulta que utiliza el término de búsqueda. Hágalo en la sección siguiente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **El objeto de solicitud**
> 
> La `Request` objeto contiene toda la información que el explorador envía a la aplicación cuando se solicita una página o se envían. Este objeto incluye toda la información que proporciona el usuario, como los valores del cuadro de texto o un archivo para cargarlo. También incluye todo tipo de información adicional, como las cookies, los valores de la cadena de consulta de dirección URL (si existe), la ruta de acceso de la página que se está ejecutando, el tipo de explorador que está usando el usuario, la lista de idiomas que se establecen en el explorador y mucho más.
> 
> El `Request` objeto es un *colección* (lista) de valores. Obtiene un valor individual fuera de la colección especificando su nombre:
> 
> `var someValue = Request["name"];`
> 
> La `Request` objeto realmente expone varios subconjuntos. Por ejemplo:
> 
> - `Request.Form` Proporciona valores de elementos dentro de la enviada `<form>` elemento si la solicitud es un `POST` solicitud.
> - `Request.QueryString` proporciona solo los valores en la cadena de consulta de URL. (En una dirección URL como `http://mysite/myapp/page?searchGenre=action&page=2`, el `?searchGenre=action&page=2` sección de la dirección URL es la cadena de consulta.)
> - `Request.Cookies` colección proporciona acceso a las cookies que se ha enviado el explorador.
> 
> Para obtener un valor que sabe que está en el formulario enviado, puede usar `Request["name"]`. Como alternativa, puede usar las versiones más específicas `Request.Form["name"]` (para `POST` solicitudes) o `Request.QueryString["name"]` (para `GET` solicitudes). Por supuesto, *nombre* es el nombre del elemento que se va a obtener.
> 
> El nombre del elemento que desea obtener debe ser único dentro de la colección que está usando. Por eso la `Request` objeto proporciona los subconjuntos como `Request.Form` y `Request.QueryString`. Supongamos que la página contiene un elemento de formulario denominado `userName` y *también* contiene una cookie denominada `userName`. Si obtiene `Request["userName"]`, si desea que el valor del formulario o la cookie es ambiguo. Sin embargo, si se produce `Request.Form["userName"]` o `Request.Cookie["userName"]`, le sean explícitos sobre qué valor va a obtener.
> 
> Es una buena práctica para ser específico y usar el subconjunto de `Request` que le interesa, como `Request.Form` o `Request.QueryString`. Para las páginas simples que se va a crear en este tutorial, probablemente no realiza realmente cualquier diferencia. Sin embargo, como crear páginas más complejas, mediante la versión explícita `Request.Form` o `Request.QueryString` puede ayudarle a evitar problemas que pueden surgir cuando la página contiene un formulario (o varios formularios), las cookies, los valores de cadena de consulta y así sucesivamente.


## <a name="creating-a-query-by-using-a-search-term"></a>Creación de una consulta con un término de búsqueda

Ahora que sabe cómo obtener el término de búsqueda que el usuario especificado, puede crear una consulta que lo usa. Recuerde que para obtener todos los elementos de la película fuera de la base de datos, está usando una consulta SQL que es similar a esta instrucción:

`SELECT * FROM Movies`

Para obtener solo determinadas películas, tiene que usar una consulta que incluye un `Where` cláusula. Esta cláusula permite establecer una condición en la que se devuelven las filas por la consulta. Por ejemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

El formato básico es `WHERE column = value`. Puede usar distintos operadores además de simplemente `=`, como `>` (mayor que), `<` (menor que), `<>` (no igual a), `<=` (menor o igual que), etc., según lo está buscando.

Si se lo está preguntando, las instrucciones SQL no distinguen mayúsculas de minúsculas &mdash; `SELECT` es el mismo que `Select` (o incluso `select`). Sin embargo, las personas a menudo aprovechar como palabras clave en una instrucción SQL, `SELECT` y `WHERE`, para que sea más fácil de leer.

### <a name="passing-the-search-term-as-a-parameter"></a>Pasando el término de búsqueda como un parámetro

Busca un género concreto es bastante fácil (`WHERE Genre = 'Action'`), pero desea poder buscar cualquier género que el usuario escribe. Para ello, cree como consulta SQL que incluye un marcador de posición para el valor para buscar. Será similar a este comando:

`SELECT * FROM Movies WHERE Genre = @0`

El marcador de posición es el `@` caracteres seguidos de cero. Como puede imaginar, una consulta puede contener varios marcadores de posición, y se denominará `@0`, `@1`, `@2`, etcetera.

Para configurar la consulta y pásele realmente el valor, utilice el código similar al siguiente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Este código es similar a lo que ha hecho ya para mostrar datos en la cuadrícula. Las únicas diferencias son:

- La consulta contiene un marcador de posición (`WHERE Genre = @0"`).
- La consulta se coloca en una variable (`selectCommand`); antes, se pasa directamente a la consulta el `db.Query` método.
- Cuando se llama a la `db.Query` método, pase la consulta y el valor que se usará para el marcador de posición. (Si la consulta tiene varios marcadores de posición, podría pasar todos como valores independientes para el método.)

Si coloca todos estos elementos juntos, obtendrá el siguiente código:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **¡Importante!** Uso de marcadores de posición (como `@0`) pasar valores a un comando SQL es *extremadamente importante* para la seguridad. La manera en que ve aquí, con marcadores de posición para los datos de la variable, es la única manera que se debe construir los comandos SQL.
> 
> Nunca cree una instrucción SQL mediante la colocación de los valores que obtener del usuario y texto literal (concatenar). Concatenación de entrada del usuario en una instrucción SQL abre el sitio para un *ataques de inyección SQL* donde un usuario malintencionado envía valores a la página que hack en la base de datos. (Encontrará información adicional en el artículo [inyección de código SQL](https://msdn.microsoft.com/library/ms161953.aspx) el sitio Web de MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Actualizar la página de películas con código de búsqueda

Ahora puede actualizar el código en el *Movies.cshtml* archivo. Para empezar, reemplace el código en el bloque de código en la parte superior de la página con este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La diferencia es que ha colocado la consulta en el `selectCommand` variable, que pasará a `db.Query` más adelante. Colocar la instrucción SQL en una variable le permite cambiar la instrucción, que es lo haré para llevar a cabo la búsqueda.

También se han quitado estas dos líneas, que le vuelve a colocar en versiones posteriores:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

No desea ejecutar la consulta aún (es decir, llamar a `db.Query`) y no desea inicializar el `WebGrid` auxiliar todavía cualquiera. Llevará a cabo esas cosas después de que haya descubierto qué instrucción SQL debe ejecutarse.

Después de este bloque ha vuelto a escribir, puede agregar la nueva lógica para controlar la búsqueda. El código completo tendrá un aspecto similar al siguiente. Actualice el código en la página para que coincida con este ejemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La página ahora funciona así. Cada vez que se ejecuta la página, el código abre la base de datos y el `selectCommand` variable se establece en la instrucción SQL que obtiene todos los registros desde el `Movies` tabla. El código también inicializa la `searchTerm` variable.

Sin embargo, si la solicitud actual incluye un valor para el `searchGenre` elemento, el código establece `selectCommand` a una consulta diferente, es decir, para que incluya el `Where` cláusula para buscar un género. También establece `searchTerm` a lo que se pasó para el cuadro de búsqueda (que puede ser nothing).

Independientemente de qué SQL instrucción está en `selectCommand`, el código, a continuación, llama a `db.Query` para ejecutar la consulta, pasándole el se encuentra en `searchTerm`. Si no hay nada `searchTerm`, no importa, porque en ese caso, no hay ningún parámetro para pasar el valor a `selectCommand` de todos modos.

Por último, el código inicializa el `WebGrid` auxiliar mediante el uso de los resultados de consulta, como antes.

Puede ver que al colocar la instrucción SQL y el término de búsqueda en variables, se ha agregado flexibilidad al código. Como verá más adelante en este tutorial, puede usar este marco de trabajo básico y seguir agregando la lógica para los distintos tipos de búsquedas.

## <a name="testing-the-search-by-genre-feature"></a>Probar la característica de búsqueda por género

En WebMatrix, ejecute el *Movies.cshtml* página. Consulte la página con el cuadro de texto para el género.

Escriba un género que ha especificado para uno de los registros de prueba y luego haga clic en **búsqueda**. Esta vez verá una lista de solo las películas que coincidan con ese género:

![Página de películas lista después de buscar género 'Comedies'](form-basics/_static/image4.png)

Escriba un género diferentes y buscar de nuevo. Pruebe a escribir el género mediante el uso de todas las letras mayúsculas o minúsculas para que puedan ver que la búsqueda no distingue mayúsculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Recordar" el usuario especificado

Puede que haya observado que después de que ha especificado un género y hace clic en **búsqueda género**, vio una lista de ese género. Sin embargo, el cuadro de texto de búsqueda estaba vacío &mdash; en otras palabras, no recordar lo que había escrito.

Es importante comprender por qué se produce este comportamiento. Cuando se envía una página, el explorador envía una solicitud al servidor web. Cuando ASP.NET recibe la solicitud, crea una nueva instancia de la página, se ejecuta el código en él y, a continuación, vuelve a presentar la página al explorador. En la práctica, sin embargo, la página no sabe que acaba de trabajar con una versión anterior de sí mismo. Todo lo que sabe es que TI recibió una solicitud que tenía algunos datos del formulario en ella.

Cada vez que se solicita una página &mdash; ya sea por primera vez o enviándolo &mdash; le esté sacando una nueva página. El servidor web no tiene memoria de la última solicitud. Pero tampoco ASP.NET, y tampoco el explorador. La única conexión entre estas dos instancias independientes de la página es cualquier dato que transmite entre ellos. Por ejemplo, si envía una página, la nueva instancia de la página puede obtener los datos del formulario enviado por la instancia anterior. (Otra forma de pasar datos entre páginas es usar cookies).

Es decir que las páginas web son una manera formal para describir esta situación *sin estado*. Los servidores Web y las propias páginas y los elementos de la página no mantienen toda la información sobre el estado de una página anterior. La web se diseñó así porque el mantenimiento del estado para las solicitudes individuales podría rápidamente agotar los recursos de servidores web, que a menudo miles, quizás incluso cientos de miles de solicitudes por segundo.

Por lo que es el motivo por el cuadro de texto está vacío. Una vez enviada la página, ASP.NET crea una nueva instancia de la página y se ejecutó a través del código y marcado. No hubo nada en el código que le indica a ASP.NET para colocar un valor en el cuadro de texto. Por lo que ASP.NET no hace nada y el cuadro de texto se representa sin un valor en él.

Hay realmente una manera sencilla de solucionar este problema. El género que ha introducido en el cuadro de texto *es* disponibles en el código &mdash; resulta `Request.QueryString["searchGenre"]`.

Actualice el marcado del cuadro de texto para que el `value` atributo obtiene su valor de `searchTerm`, al igual que en este ejemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

En esta página, podría haber establecido también el `value` atributo a la `searchTerm` variable, ya que esa variable también contiene el género que escribió. Pero al usar el `Request` objeto para establecer el `value` atributo tal como se muestra aquí es la manera estándar para realizar esta tarea. (Suponiendo que incluso desee hacerlo &mdash; en algunas situaciones, es posible que desee presentar la página *sin* valores en los campos. Todo depende de lo que está ocurriendo con su aplicación.)

> [!NOTE]
> No se "recuerda" el valor de un cuadro de texto que se usa para las contraseñas. Sería una vulnerabilidad de seguridad para permitir que los usuarios rellenar un campo de contraseña mediante el uso de código.


Vuelva a ejecutar la página, escriba un género y haga clic en **búsqueda género**. Esta vez no sólo verá los resultados de la búsqueda, pero el cuadro de texto le recuerda lo que especificó la última vez:

![Página que muestra el cuadro de texto se "recuerda" la entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Buscar cualquier palabra en el título

Ahora puede buscar cualquier género, pero también puede buscar un título. Es difícil obtener un título elaborarlo correctamente al realizar una búsqueda, por lo tanto, que puede buscar una palabra que aparece en cualquier lugar dentro de un título. Para ello en SQL, usa el `LIKE` operador y la sintaxis como la siguiente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Este comando obtiene todas las películas cuyos títulos contienen "adventure". Cuando se usa el `LIKE` operador, incluir el carácter comodín `%` como parte del término de búsqueda. La búsqueda `LIKE 'adventure%'` significa "comenzar con 'adventure'". (Técnicamente, significa "La cadena"adventure"seguido de nada.") De forma similar, el término de búsqueda `LIKE '%adventure'` "cualquier cosa seguido por la cadena"adventure"", significa que es otra manera de decir "finaliza con"adventure"".

El término de búsqueda `LIKE '%adventure%'` significa, por tanto, "con"adventure"en cualquier lugar en el título." (Técnicamente, "cualquier cosa en el título, seguido de"adventure", seguido de nada".)

Dentro de la `<form>` elemento, agregue el marcado siguiente justo debajo del cierre `</div>` etiqueta para la búsqueda de género (justo antes de cerrar `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

El código para controlar esta búsqueda es similar al código de la búsqueda de género, salvo que lo debe ensamblar el `LIKE` búsqueda. Dentro del bloque de código en la parte superior de la página, agregue esto `if` justo después de bloquear la `if` bloque para la búsqueda de género:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Este código usa la misma lógica que vio anteriormente, salvo que la búsqueda utiliza un `LIKE` operador y la coloca código "`%`" antes y después el término de búsqueda.

Tenga en cuenta cómo resultó muy fácil agregar otra búsqueda a la página. Sólo tenía que hacer era:

- Crear un `if` bloque que probar para ver si el cuadro de búsqueda pertinentes tenía un valor.
- Establecer el `selectCommand` variable a una nueva instrucción SQL.
- Establecer el `searchTerm` variable con el valor para pasar a la consulta.

Este es el bloque de código completo, que contiene la lógica para la búsqueda de un título nuevo:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Este es un resumen de lo que hace este código:

- Las variables `searchTerm` y `selectCommand` se inicializan en la parte superior. Va a establecer estas variables para el término de búsqueda adecuado (si existe) y comando SQL adecuado en función de lo que hace el usuario en la página. La búsqueda predeterminada es el caso sencillo de obtener todas las películas de la base de datos.
- En las pruebas para `searchGenre` y `searchTitle`, el código establece `searchTerm` en el valor que desea buscar. Los bloques de código también establece `selectCommand` a un comando SQL adecuado para la búsqueda.
- El `db.Query` método se invoca una sola vez, mediante cualquier comando SQL está en `selectedCommand` y cualquier valor que se encuentra en `searchTerm`. Si no hay ningún término de búsqueda (sin género y no hay ninguna palabra título), el valor de `searchTerm` es una cadena vacía. Sin embargo, que no importa, porque en ese caso, la consulta no requiere un parámetro.

## <a name="testing-the-title-search-feature"></a>Probar la característica de búsqueda de título

Ahora puede probar la página de búsqueda completada. Ejecute *Movies.cshtml*.

Escriba un género y haga clic en **búsqueda género**. La cuadrícula muestra películas de ese género, al igual que antes.

Escriba una palabra de título y haga clic en **título de la búsqueda**. La cuadrícula muestra películas con esa palabra en el título.

![Enumerar después de buscar 'El' en el título de página de películas](form-basics/_static/image6.png)

Deje en blanco ambos cuadros de texto y haga clic en cualquiera de los botones. La cuadrícula muestra todas las películas.

## <a name="combining-the-queries"></a>Combinar las consultas

Es posible que tenga en cuenta que las búsquedas que puede realizar son exclusivas. No se puede buscar el título y el género al mismo tiempo, incluso si los dos cuadros de búsqueda tienen valores en ellos. Por ejemplo, no se puede buscar todas las películas de acción cuyo título contiene "Adventure". (Como la página está codificada por ahora, si escribe valores para el género y el título, la búsqueda de título obtiene precedencia). Para crear una búsqueda que combina las condiciones, deberá crear una consulta SQL que tiene una sintaxis similar al siguiente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Y tendría que ejecutar la consulta mediante una instrucción similar al siguiente (en términos generales):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Crear la lógica para permitir las permutaciones de muchos de los criterios de búsqueda puede obtener un poco implicado, como puede ver. Por lo tanto, dejaremos de aquí.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, creará una página que usa un formulario para permitir que los usuarios agregar películas a la base de datos.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Lista completa de la página de película (actualizada con la búsqueda)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE de SQL](http://www.w3schools.com/sql/sql_where.asp) en el sitio W3Schools
- [Las definiciones de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artículo en el sitio de W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md)
> [Siguiente](entering-data.md)
