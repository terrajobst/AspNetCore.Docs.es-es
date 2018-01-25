---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: "Introducción a ASP.NET Web Pages: conceptos básicos de formularios HTML | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial muestra los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora que..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 68056759b2e80230e5fd2c0f9b2d2a89b549cf37
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introducción a ASP.NET Web Pages: conceptos básicos de formularios HTML
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra los conceptos básicos de cómo crear un formulario de entrada y cómo controlar la entrada del usuario cuando se usa ASP.NET Web Pages (Razor). Y ahora que tiene una base de datos, se usará sus habilidades de formulario para que los usuarios puedan buscar películas específicos en la base de datos. Supone que ha completado la serie a través de [Introducción para mostrar datos utilizando ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Lo que aprenderá:
> 
> - Cómo crear un formulario mediante el uso de los elementos HTML estándar.
> - Cómo leer el usuario de la entrada en un formulario.
> - Cómo crear una consulta SQL que selectivamente obtiene los datos mediante una búsqueda de términos el usuario proporciona.
> - Cómo hacer que los campos en la página "recordar" el usuario que especificó.
>   
> 
> Características y tecnologías que se tratan:
> 
> - Objeto `Request`.
> - La instrucción SQL `Where` cláusula.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En el tutorial anterior, creó una base de datos, agrega datos a ella y, a continuación, usa el `WebGrid` auxiliar para mostrar los datos. En este tutorial, agregará un cuadro de búsqueda que permite buscar películas de un género específico o cuyo título contiene cualquier palabra que escriba. (Por ejemplo, podrá encontrar todas las películas que pertenezcan al género es "Acción" o cuyo título contiene "Harry" o "Adventure".)

Cuando haya terminado con este tutorial, tendrá una página como esta:

![Página de películas con búsqueda de género y título](form-basics/_static/image1.png)

La parte de lista de la página es el mismo que en el último tutorial &mdash; una cuadrícula. La diferencia será que la cuadrícula mostrará únicamente las películas que va a buscar.

## <a name="about-html-forms"></a>Acerca de los formularios HTML

(Si tiene experiencia con la creación de formularios HTML y con la diferencia entre `GET` y `POST`, puede omitir esta sección.)

Un formulario tiene los elementos de entrada de usuario &mdash; cuadros de texto, botones, botones de opción, casillas de verificación, listas desplegables y así sucesivamente. Los usuarios rellenar estos controles o realice las selecciones y, a continuación, envíen el formulario, haga clic en un botón.

La sintaxis básica de HTML de un formulario se muestra en este ejemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Cuando se ejecuta este marcado en una página, crea un formulario simple que es similar a esta ilustración:

![Formulario HTML básico como representado en el explorador](form-basics/_static/image2.png)

El `<form>` elemento incluye elementos HTML que se va a enviar. (Un error habitual para hacer es agregar elementos a la página, pero se olvida, a continuación, colocarlos dentro de un `<form>` elemento. En ese caso, no se envía.) El `method` atributo indica al explorador cómo enviar la entrada del usuario. Se establece en `post` si va a realizar una actualización en el servidor o a `get` si simplemente está obteniendo datos del servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST y seguridad de verbo HTTP**
> 
> HTTP, el protocolo que los exploradores y servidores que se usan para intercambiar información, es muy sencillo en sus operaciones básicas. Los exploradores utilizan sólo unos cuantos verbos para realizar solicitudes a los servidores. Cuando se escribe código para la web, resulta útil comprender estos verbos y usan de cómo el explorador y el servidor. Lejos los verbos más usados son estas:
> 
> - `GET`. El explorador utiliza este verbo para capturar algo desde el servidor. Por ejemplo, cuando se escribe una dirección URL en el explorador, el explorador realiza una `GET` operación para solicitar la página que desee. Si la página incluye gráficos, el explorador realiza adicionales `GET` operaciones para obtener las imágenes. Si el `GET` operación tenga que pasar información al servidor, la información se pasa como parte de la dirección URL en la cadena de consulta.
> - `POST`. El explorador envía una `POST` solicitud con el fin de enviar datos para ser agregados o cambiados en el servidor. Por ejemplo, el `POST` verbo se utiliza para crear registros en una base de datos o cambiar los existentes. La mayoría de los casos, al rellenar un formulario y haga clic en el botón Enviar, el explorador realiza una `POST` operación. En un `POST` operación, los datos que se pasan al servidor están en el cuerpo de la página.
> 
> Una diferencia importante entre estos verbos es que un `GET` operación no debería para cambiar nada en el servidor, o colocar de forma ligeramente más abstracta, un `GET` operación no da como resultado un cambio de estado en el servidor. Puede realizar un `GET` operación en los mismos recursos tantas veces como le gusta y no cambian esos recursos. (Un `GET` operación suele decirse que son "seguros", o para usar un término técnico, es *idempotente*.) En cambio, por supuesto, un `POST` solicitud cambia algo en el servidor cada vez que realice la operación.
> 
> Dos ejemplos ayudarán a ilustrar esta distinción. Al realizar una búsqueda con un motor como Bing o Google, rellene un formulario que está formada por un cuadro de texto y, a continuación, haga clic en el botón de búsqueda. El explorador realiza una `GET` operación, con el valor especificado en el cuadro que se pasa como parte de la dirección URL. Mediante un `GET` operación para este tipo de formulario es un problema, dado que una operación de búsqueda no cambia los recursos en el servidor, simplemente captura información.
> 
> Ahora, imagina el proceso de pedidos en línea. Rellene los detalles del pedido y, a continuación, haga clic en el botón Enviar. Esta operación se volverá a un `POST` solicitar, porque la operación produce cambios en el servidor, como un nuevo registro de pedido, un cambio en la información de cuenta y quizás muchos otros cambios. A diferencia de la `GET` operación, no se puede repetir el `POST` solicitud, si lo hizo, cada vez que se vuelven a enviar la solicitud, se generaría un nuevo pedido en el servidor. (En casos como este, sitios Web a menudo le advertirá que no haga clic en un botón de envío de más de una vez, o deshabilitará el botón Enviar para que no vuelva a enviar el formulario por accidente.)
> 
> En el transcurso de este tutorial, deberá usar un `GET` operación y un `POST` operación para trabajar con formularios HTML. Explicaremos en cada caso porqué el verbo que use es el adecuado.
> 
> (Para más información acerca de los verbos HTTP, consulte el [definiciones de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artículo en el sitio de W3C.)


Elementos de entrada de usuario mayoría son HTML `<input>` elementos. Quedarán `<input type="type" name="name">,` donde *tipo* indica el tipo de control de entrada de usuario que desee. Estos elementos son las más comunes:

- Cuadro de texto:`<input type="text">`
- Casilla de verificación:`<input type="check">`
- Botón de opción:`<input type="radio">`
- Botón:`<input type="button">`
- Botón de envío:`<input type="submit">`

También puede usar el `<textarea>` elemento para crear un cuadro de texto multilínea y `<select>` elemento para crear una lista desplegable o una lista desplazable. (Para más información sobre HTML los elementos del formulario, consulte [formularios HTML y entrada](http://www.w3schools.com/html/html_forms.asp) en el sitio W3Schools.)

El `name` atributo es muy importante, porque el nombre es cómo obtendrá el valor del elemento de una versión posterior, como verá en breve.

La parte interesante es lo que usted como desarrollador de la página, hace con la entrada del usuario. Hay un comportamiento integrado asociado a estos elementos. En su lugar, tendrá que obtener los valores que el usuario ha especificado o seleccionado y hacer algo con ellos. Eso es lo que aprenderá en este tutorial.

> [!TIP] 
> 
> **HTML5 y formularios de entrada**
> 
> Como ya sabe, HTML está en transición y la versión más reciente (HTML5) incluye compatibilidad para ver las formas más intuitivo para los usuarios especifiquen información. Por ejemplo, en HTML5, usted (el programador de la página) puede indicar la página que desea que el usuario escriba una fecha. A continuación, el explorador puede mostrar automáticamente un calendario en lugar de requerir al usuario que escriba una fecha manualmente. Sin embargo, HTML5 es nuevo y aún no se admite en todos los exploradores.
> 
> ASP.NET Web Pages es compatible con HTML5 en la medida en que examinador del usuario hace de entrada. Para obtener una idea de los nuevos atributos para el `<input>` el elemento en HTML5, vea [HTML &lt;entrada&gt; atributo type](http://www.w3schools.com/html/html_form_input_types.asp) en el sitio W3Schools.


## <a name="creating-the-form"></a>Crear el formulario

En WebMatrix, en la **archivos** área de trabajo, abra el *Movies.cshtml* página.

Después del cierre `</h1>` etiqueta y antes de la apertura `<div>` etiqueta de la `grid.GetHtml` llamada, agregue el siguiente marcado:

[!code-html[Main](form-basics/samples/sample2.html)]

Este marcado crea un formulario que tiene un cuadro de texto denominado `searchGenre` y un botón de envío. El texto del cuadro y enviar botón se incluyen en un `<form>` elemento cuyo `method` atributo está establecido en `get`. (Recuerde que si no coloca el cuadro de texto y enviar botón dentro de un `<form>` elemento, no se enviarán al hacer clic en el botón.) Usa el `GET` verbo aquí porque va a crear un formulario que no realice cambios en el servidor: se produce solo en una búsqueda. (En el tutorial anterior, ha utilizado un `post` método, que es cómo enviar los cambios en el servidor. Verá en el siguiente tutorial nuevo.)

Ejecute la página. Aunque no ha definido ningún comportamiento para el formulario, puede ver su aspecto:

![Página de películas con el cuadro de búsqueda de género](form-basics/_static/image3.png)

Escriba un valor en el cuadro de texto, como "Comedia". A continuación, haga clic en **búsqueda género**.

Tome nota de la dirección URL de la página. Porque estableció el `<form>` del elemento `method` atribuir a `get`, el valor introducido es ahora parte de la cadena de consulta en la dirección URL, similar al siguiente:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Leer valores de formulario

La página ya contiene un código que recibe los datos de la base de datos y muestra los resultados en una cuadrícula. Ahora tiene que agregar código que lee el valor del cuadro de texto para que pueda ejecutar una consulta SQL que incluye el término de búsqueda.

Dado que el método del formulario se establece en `get`, puede leer el valor que se escribió en el cuadro de texto utilizando código similar al siguiente:

`var searchTerm = Request.QueryString["searchGenre"];`

El `Request.QueryString` objeto (el `QueryString` propiedad de la `Request` objeto) incluye los valores de elementos que se envían como parte de la `GET` operación. El `Request.QueryString` propiedad contiene un *colección* (una lista) de los valores que se envían en el formulario. Para obtener cualquier valor individual, especifique el nombre del elemento que desee. Por eso debe tener un `name` del atributo en el `<input>` elemento (`searchTerm`) que crea el cuadro de texto. (Para obtener más información sobre la `Request` de objetos, consulte la [sidebar](#BKMK_TheRequestObject) más adelante.)

Es lo suficientemente sencilla para leer el valor del cuadro de texto. Sin embargo, si el usuario no ha escrito nada en absoluto en el cuadro de texto, pero hace clic en **búsqueda** de todos modos, puede omitir ese clic, ya que hay que buscar.

El código siguiente es un ejemplo que muestra cómo implementar estas condiciones. (No tiene que agregar este código aún; lo hará en un momento).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

La prueba se desglosa en este modo:

- Obtener el valor de `Request.QueryString["searchGenre"]`, es decir, el valor que se escribió en el `<input>` elemento denominado `searchGenre`.
- Averiguar si está vacío utilizando el `IsEmpty` método. Este método es el método estándar para determinar si algo (por ejemplo, un elemento form) contiene un valor. Pero en realidad, ocupa sólo si se ha *no* vacía, por lo tanto...
- Agregar el `!` operador delante de la `IsEmpty` de prueba. (El `!` operador significa NOT lógico).

En términos sencillos, toda la `if` condición se traduce en lo siguiente: *si el elemento de searchGenre del formulario no está vacío...*

Este bloque establece la etapa para crear una consulta que utiliza el término de búsqueda. Llevará a cabo en la sección siguiente.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **El objeto de solicitud**
> 
> La `Request` objeto contiene toda la información que el explorador envía a la aplicación cuando se solicita una página o se envían. Este objeto incluye toda la información que proporciona el usuario, como los valores del cuadro de texto o un archivo para cargarlo. También incluye todo tipo de información adicional, como las cookies, los valores de la cadena de consulta de dirección URL (si existe), la ruta de acceso de archivo de la página que se está ejecutando, el tipo de explorador que está usando el usuario, la lista de idiomas que se establecen en el explorador y mucho más.
> 
> El `Request` objeto es un *colección* (lista) de valores. Obtiene un valor individual de la colección especificando su nombre:
> 
> `var someValue = Request["name"];`
> 
> La `Request` objeto realmente expone varios subconjuntos. Por ejemplo:
> 
> - `Request.Form`Proporciona valores de elementos dentro de la enviada `<form>` elemento si la solicitud es un `POST` solicitud.
> - `Request.QueryString`permite solo los valores de cadena de consulta de las direcciones URL. (En una dirección URL como `http://mysite/myapp/page?searchGenre=action&page=2`, la `?searchGenre=action&page=2` sección de la dirección URL es la cadena de consulta.)
> - `Request.Cookies`colección le da acceso a las cookies que ha enviado el explorador.
> 
> Para obtener un valor que sepa que está en el formulario enviado, puede usar `Request["name"]`. Como alternativa, puede usar las versiones más específicas `Request.Form["name"]` (para `POST` solicitudes) o `Request.QueryString["name"]` (para `GET` solicitudes). Por supuesto, *nombre* es el nombre del elemento que se va a obtener.
> 
> El nombre del elemento que desea obtener tiene que ser único dentro de la colección que está usando. Por eso la `Request` objeto proporciona los subconjuntos como `Request.Form` y `Request.QueryString`. Supongamos que la página contiene un elemento de formulario denominado `userName` y *también* contiene una cookie denominada `userName`. Si obtiene `Request["userName"]`, si desea que el valor de formulario o la cookie es ambiguo. Sin embargo, si obtiene `Request.Form["userName"]` o `Request.Cookie["userName"]`, le sean explícitos sobre qué valor se debe obtener.
> 
> Es una buena práctica para ser específico y usar el subconjunto de `Request` que le interesa, como `Request.Form` o `Request.QueryString`. Para las páginas simples que va a crear en este tutorial, probablemente no realmente tiene cualquier diferencia. Sin embargo, a medida que cree páginas más complejas, con la versión explícita `Request.Form` o `Request.QueryString` puede ayudar a evitar problemas que pueden surgir cuando la página contiene un formulario (o varios formularios), las cookies, los valores de cadena de consulta y así sucesivamente.


## <a name="creating-a-query-by-using-a-search-term"></a>Crear una consulta mediante el uso de un término de búsqueda

Ahora que sabe cómo obtener el término de búsqueda que el usuario ha escrito, puede crear una consulta que lo utilice. Recuerde que para obtener todos los elementos de la película fuera de la base de datos, que está utilizando una consulta SQL que es similar a esta instrucción:

`SELECT * FROM Movies`

Para obtener solo determinadas películas, tendrá que utilizar una consulta que incluye un `Where` cláusula. Esta cláusula permite establecer una condición en la que se devuelven filas por la consulta. Por ejemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

El formato básico es `WHERE column = value`. Puede usar operadores diferentes además just `=`, como `>` (mayor que), `<` (menor que), `<>` (no es igual a), `<=` (menor o igual que), etc., según lo está buscando.

En caso de que se esté preguntando, instrucciones SQL no distinguen mayúsculas de minúsculas &mdash; `SELECT` es el mismo que `Select` (o incluso `select`). Sin embargo, personas suelen aparecer como palabras clave en una instrucción SQL, `SELECT` y `WHERE`, para que resulten más fáciles de leer.

### <a name="passing-the-search-term-as-a-parameter"></a>Pase el término de búsqueda como un parámetro

Busca un género específico es bastante fácil (`WHERE Genre = 'Action'`), pero desea poder buscar cualquier género que el usuario especifica. Para ello, se crea como consulta SQL que incluya un marcador de posición para el valor para buscar. Será similar a este comando:

`SELECT * FROM Movies WHERE Genre = @0`

El marcador de posición es el `@` caracteres seguidos de cero. Como puede imaginar, una consulta puede contener varios marcadores de posición, y se denominará `@0`, `@1`, `@2`, etcetera.

Para configurar la consulta y pásele realmente el valor, utilice el código similar al siguiente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Este código es similar a lo que ya ha hecho para mostrar datos en la cuadrícula. Las únicas diferencias son:

- La consulta contiene un marcador de posición (`WHERE Genre = @0"`).
- La consulta se coloca en una variable (`selectCommand`); antes, pasa la consulta directamente a la `db.Query` método.
- Cuando se llama a la `db.Query` método, pase la consulta y el valor que se usará para el marcador de posición. (Si la consulta ha varios marcadores de posición, deberá pasar a todos como valores independientes para el método.)

Si reunir todos estos elementos, obtendrá el siguiente código:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante:** Con los marcadores de posición (como `@0`) pasar valores a un comando SQL es *importantísimo* para la seguridad. La manera que ve aquí, con marcadores de posición para los datos de la variable, es la única manera debe construir comandos SQL.
> 
> Nunca construir una instrucción SQL que reúnan los valores que obtener del usuario y texto literal (concatenar). Concatenar proporcionados por el usuario en una instrucción SQL abre el sitio a un *ataque de inyección de SQL* donde un usuario malintencionado envía los valores a la página que hack la base de datos. (Puede obtener en el artículo [inyección de código SQL](https://msdn.microsoft.com/library/ms161953.aspx) el sitio Web de MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Actualizar la página de películas con código de búsqueda

Ahora puede actualizar el código en el *Movies.cshtml* archivo. Para empezar, reemplace el código en el bloque de código en la parte superior de la página con este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La diferencia es que se ha establecido la consulta en el `selectCommand` variable, que se pasa a `db.Query` más tarde. Colocar la instrucción SQL en una variable le permite cambiar la instrucción, que es lo que deberá llevar a cabo para llevar a cabo la búsqueda.

También se han quitado estas dos líneas, lo que le vuelve a colocar en versiones posteriores:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

No desea ejecutar la consulta aún (es decir, llamar a `db.Query`) y no desea inicializar el `WebGrid` auxiliar todavía cualquiera. Una vez que haya descubierto qué instrucción SQL debe ejecutarse llevará a cabo esas operaciones.

Después de este bloque ha vuelto a escribir, puede agregar la nueva lógica para controlar la búsqueda. El código completo tendrá un aspecto similar al siguiente. Actualice el código en la página para que coincida con este ejemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La página ahora funciona del siguiente modo. Cada vez que se ejecuta la página, el código abre la base de datos y la `selectCommand` variable se establece en la instrucción SQL que obtiene todos los registros desde el `Movies` tabla. El código también inicializa el `searchTerm` variable.

Sin embargo, si la solicitud actual incluye un valor para el `searchGenre` elemento, el código establece `selectCommand` a una consulta diferente:; es decir, a uno que incluya el `Where` cláusula para buscar un género. También establece `searchTerm` a lo que se pasó para el cuadro de búsqueda (que podría ser nada).

Independientemente de qué SQL instrucción está en `selectCommand`, el código, a continuación, llama `db.Query` para ejecutar la consulta, pasándole el se encuentra en `searchTerm`. Si no hay nada `searchTerm`, no importa, porque en ese caso, no hay ningún parámetro para pasar el valor a `selectCommand` todos modos.

Por último, el código inicializa el `WebGrid` auxiliar mediante el uso de los resultados de la consulta, al igual que antes.

Puede ver que colocando la instrucción SQL y el término de búsqueda en variables, se ha agregado flexibilidad en el código. Como verá más adelante en este tutorial, puede utilizar este marco de trabajo básico y mantener agregar lógica de diferentes tipos de búsquedas.

## <a name="testing-the-search-by-genre-feature"></a>Probar la característica de búsqueda por género

En WebMatrix, ejecute el *Movies.cshtml* página. Consulte la página con el cuadro de texto para el género.

Escriba un género que ha escrito para uno de los registros de prueba y después haga clic en **búsqueda**. Esta vez verá una lista de solo las películas que coinciden con ese género:

![Página de películas enumerar después de buscar género 'Comedies'](form-basics/_static/image4.png)

Escriba un género diferentes y buscar de nuevo. Pruebe a escribir el género mediante el uso de todas las letras en minúsculas o mayúsculas para que puedan ver que la búsqueda no distingue entre mayúsculas y minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Recordar" el usuario especificado

Puede que haya observado que después de que ha especificado un género y hace clic en **búsqueda género**, vio una lista de ese género. Sin embargo, el cuadro de texto de búsqueda estaba vacío &mdash; en otras palabras, no recuerde que haya introducido.

Es importante comprender por qué se produce este comportamiento. Cuando se envía una página, el explorador envía una solicitud al servidor web. Cuando ASP.NET recibe la solicitud, crea una instancia nueva de la página, se ejecuta el código en él y, a continuación, representa la página en el Explorador de nuevo. De hecho, sin embargo, la página no sabe que acaba de trabajar con una versión anterior de sí mismo. Todos los que sepa es que TI recibió una solicitud que tenía algunos datos del formulario en ella.

Cada vez que se solicita una página &mdash; ya sea por primera vez o enviándolo &mdash; recibe una nueva página. El servidor web no tiene memoria de la última solicitud. Tampoco ASP.NET, ni tampoco el explorador. La única conexión entre estas dos instancias independientes de la página es cualquier dato que transmite entre ellos. Por ejemplo, si envía una página, la nueva instancia de la página puede obtener los datos del formulario que se envían la instancia anterior. (Otra forma de pasar datos entre páginas es usar cookies.)

Una forma formal para describir esta situación consiste en indicar que las páginas web son *sin estado*. Servidores Web y las propias páginas y los elementos de la página lo mantiene toda la información sobre el estado anterior de una página. La web se diseñó así porque mantener el estado para las solicitudes individuales se rápidamente agotar los recursos de servidores web, que a menudo miles, quizás incluso cientos de miles de solicitudes por segundo.

Por eso estaba vacío el cuadro de texto. Una vez enviada la página, ASP.NET crea una nueva instancia de la página y se ejecutó a través del código y marcado. No había nada en el código que informa de ASP.NET para establecer un valor en el cuadro de texto. Por lo que ASP.NET no hacer nada y el cuadro de texto se representa sin un valor en él.

No hay realmente es una manera sencilla de solucionar este problema. El género que ha introducido en el cuadro de texto *es* disponibles en código &mdash; resulta `Request.QueryString["searchGenre"]`.

Actualizar el marcado del cuadro de texto para que la `value` atributo obtiene su valor de `searchTerm`, al igual que en este ejemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

En esta página, podría haber establecido también el `value` atribuir a la `searchTerm` variable, ya que esa variable contiene también el género que especificó. Pero mediante la `Request` objeto para establecer el `value` atributo tal como se muestra aquí es la manera estándar para realizar esta tarea. (Suponiendo que desea hacer esto incluso &mdash; en algunas situaciones, puede representar la página *sin* valores en los campos. Todo depende de lo que ocurre con la aplicación.)

> [!NOTE]
> No se puede "recordar" el valor de un cuadro de texto que se usa para las contraseñas. Sería una vulnerabilidad de seguridad para permitir a los usuarios rellenar un campo de contraseña mediante código.


Vuelva a ejecutar la página, escriba un género y haga clic en **búsqueda género**. Esta vez no solo verá los resultados de la búsqueda, pero el cuadro de texto le recuerda lo que especificó última vez:

![Página que muestra que el cuadro de texto se "recuerdan" la entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Buscar cualquier palabra en el título

Ahora puede buscar cualquier género, pero puede que le interese buscar un título. Es difícil obtener un título elaborarlo correctamente al realizar una búsqueda, por lo que en su lugar, que puede buscar una palabra que aparece en cualquier lugar dentro de un título. Para ello en SQL, utilice la `LIKE` operador y la sintaxis como la siguiente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Este comando obtiene todas las películas cuyos títulos contienen "adventure". Cuando se usa el `LIKE` (operador), incluye el carácter comodín `%` como parte del término buscado. La búsqueda `LIKE 'adventure%'` significa "a partir de 'adventure'". (Técnicamente, significa "La cadena 'adventure' seguido de nada") De forma similar, el término de búsqueda `LIKE '%adventure'` "nada seguida por la cadena 'adventure'", significa que es otra manera de decir "final con 'adventure'".

El término de búsqueda `LIKE '%adventure%'` significa, por tanto, "con"adventure' en cualquier lugar en el título." (Técnicamente, "cualquier cosa en el título, seguido de 'adventure', seguido de nada".)

Dentro de la `<form>` elemento, agregue el siguiente marcado inmediatamente debajo de la de cierre `</div>` etiqueta para la búsqueda de género (justo antes del cierre `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

El código para controlar esta búsqueda es similar al código de la búsqueda de género, salvo que es necesario que ensamblar el `LIKE` búsqueda. Dentro del bloque de código en la parte superior de la página, agregue esta `if` justo después de bloquear el `if` bloque para la búsqueda de género:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Este código usa la misma lógica que vio anteriormente, salvo que la búsqueda usa un `LIKE` operador y la coloca código "`%`" antes y después el término de búsqueda.

Tenga en cuenta cómo era muy fácil agregar otra búsqueda a la página. Todo lo que tuve que hacer es:

- Crear un `if` bloque que probar para ver si el cuadro de búsqueda pertinente tenía un valor.
- Establecer el `selectCommand` variable a una nueva instrucción SQL.
- Establecer el `searchTerm` variable con el valor para pasar a la consulta.

Este es el bloque de código completo, que contiene la nueva lógica de búsqueda de un título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Este es un resumen de lo que hace este código:

- Las variables `searchTerm` y `selectCommand` se inicializan en la parte superior. Va a establecer estas variables para el término de búsqueda adecuado (si existe) y comando SQL adecuado en función de lo que hace el usuario en la página. La búsqueda predeterminada es el caso más sencillo para obtener todas las películas de la base de datos.
- En las pruebas para `searchGenre` y `searchTitle`, el código establece `searchTerm` en el valor que desea buscar. Los bloques de código también establece `selectCommand` a un comando SQL adecuado para la búsqueda.
- El `db.Query` método se invoca una sola vez, con el comando SQL es de `selectedCommand` y cualquier valor que se encuentra en `searchTerm`. Si no hay ningún término de búsqueda (ningún género y no hay ninguna palabra título), el valor de `searchTerm` es una cadena vacía. Sin embargo, que no importa, porque en ese caso, la consulta no requiere un parámetro.

## <a name="testing-the-title-search-feature"></a>Probar la característica de búsqueda de título

Ahora puede probar la página de búsqueda completada. Ejecutar *Movies.cshtml*.

Escriba un género y haga clic en **búsqueda género**. La cuadrícula muestra películas de ese género, al igual que antes.

Escriba una palabra de título y haga clic en **buscar en el título**. La cuadrícula muestra películas con esa palabra en el título.

![Enumerar después de buscar 'El' en el título de página de películas](form-basics/_static/image6.png)

Deje en blanco ambos cuadros de texto y haga clic en cualquiera de los botones. La cuadrícula muestra todas las películas.

## <a name="combining-the-queries"></a>Combinar las consultas

Puede observar que las búsquedas que se pueden realizar son excluyentes. No se puede buscar el título y el género al mismo tiempo, aunque ambos cuadros de búsqueda tienen valores en ellos. Por ejemplo, no se puede buscar todas las películas de acción cuyo título contiene "Adventure". (Como la página se codifica ahora, si escribe valores para el género y el título, la búsqueda de título obtiene prioridad). Para crear una búsqueda que combina las condiciones, tendría que crear una consulta SQL que tiene una sintaxis similar al siguiente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Es necesario ejecutar la consulta mediante una instrucción similar al siguiente (a grandes rasgos):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Crear una lógica para permitir que muchos permutaciones de criterios de búsqueda puede obtener un poco implicada, como puede ver. Por lo tanto, podrá detener aquí.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, creará una página que usa un formulario para permitir a los usuarios agregar películas a la base de datos.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Muestra la lista completa de página de la película (actualizada con la búsqueda)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE de SQL](http://www.w3schools.com/sql/sql_where.asp) en el sitio W3Schools
- [Definiciones de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artículo en el sitio de W3C

>[!div class="step-by-step"]
[Anterior](displaying-data.md)
[Siguiente](entering-data.md)
