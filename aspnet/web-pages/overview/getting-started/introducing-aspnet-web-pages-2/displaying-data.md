---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: "Introducción a ASP.NET Web Pages: mostrar datos | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial muestra cómo crear una base de datos en WebMatrix y cómo mostrar datos de la base de datos en una página cuando se utiliza ASP.NET Web Pages (Razor). Se supone y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: fdb9af0ba87c7802c63451ac7aa422e0020b5719
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introducción a ASP.NET Web Pages: mostrar datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una base de datos en WebMatrix y cómo mostrar datos de la base de datos en una página cuando se utiliza ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [Introducción a la programación de páginas Web de ASP.NET](../introducing-razor-syntax-c.md).
> 
> Lo que aprenderá:
> 
> - Cómo utilizar las herramientas de WebMatrix para crear una base de datos y tablas de base de datos.
> - Cómo utilizar las herramientas de WebMatrix para agregar datos a una base de datos.
> - Cómo mostrar los datos de la base de datos en una página.
> - Describe cómo ejecutar comandos SQL en ASP.NET Web Pages.
> - Cómo personalizar la `WebGrid` auxiliar para cambiar la presentación de datos y agregar paginación y la ordenación.
>   
> 
> Características y tecnologías que se tratan:
> 
> - Herramientas de base de datos de WebMatrix.
> - `WebGrid`aplicación auxiliar.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En el tutorial anterior, se han incorporado a ASP.NET Web Pages (*.cshtml* archivos), los conceptos básicos de la sintaxis Razor y aplicaciones auxiliares. En este tutorial, podrá empezar a crear la aplicación web real que va a utilizar para el resto de la serie. La aplicación es una aplicación de película simple que le permite ver, agregar, cambiar y eliminar información sobre las películas.

Cuando haya terminado con este tutorial, podrá ver una lista de película similar a esta página:

![Pantalla de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image1.png)

Pero, para empezar, deberá crear una base de datos.

## <a name="a-very-brief-introduction-to-databases"></a>Una breve introducción a las bases de datos

Este tutorial proporcionará sólo la briefest Introducción a las bases de datos. Si tiene experiencia en base de datos, puede omitir esta sección corta.

Una base de datos contiene una o más tablas que contienen información &mdash; por ejemplo, tablas de clientes, pedidos y los proveedores, o para estudiantes, profesores, clases y grados. Estructuralmente, una tabla de base de datos es similar a una hoja de cálculo. Imagine una libreta de direcciones típico. Para cada entrada de la libreta de direcciones (es decir, para cada persona) tienen varios elementos de información como el nombre, apellido, dirección, dirección de correo electrónico y número de teléfono.

![Tabla de base de datos de ejemplo como una cuadrícula simple](displaying-data/_static/image2.png)

(Filas se conocen a veces como *registros*, y las columnas se denominan a veces *campos*.)

Para la mayoría de las tablas de base de datos, la tabla debe tener una columna que contiene un valor único, como un número de cliente, número de cuenta y así sucesivamente. Este valor se conoce como la tabla *clave principal*, y usarlo para identificar cada fila de la tabla. En el ejemplo, la columna Id. es la clave principal de la libreta de direcciones que se muestra en el ejemplo anterior.

Gran parte del trabajo que realice en las aplicaciones web consiste en leer información de la base de datos y mostrar en una página. A menudo, también podrá recopilar información de usuarios y agregarlo a una base de datos, o podrá modificar los registros que ya están en la base de datos. (Se tratarán todas estas operaciones en el transcurso de este conjunto de tutorial.)

Trabajo de base de datos puede ser enormemente compleja y puede requerir un conocimiento especializado. Sin embargo, para el conjunto de este tutorial, deberá comprender solo de los conceptos básicos, que todos se explicarán marcha.

## <a name="creating-a-database"></a>Crear una base de datos

WebMatrix incluye herramientas que facilitan la tarea para crear una base de datos y para crear tablas en la base de datos. (La estructura de una base de datos se conoce como la base de datos *esquema*.) Para el conjunto de este tutorial, creará una base de datos que tiene solo una tabla en ella &mdash; películas.

Abra WebMatrix si aún no lo ha hecho y abra el sitio de WebPagesMovies que creó en el tutorial anterior.

En el panel izquierdo, haga clic en el **base de datos** área de trabajo.

![Ficha de área de trabajo de la base de datos de WebMatrix](displaying-data/_static/image3.png)

Los cambios de la cinta de opciones para mostrar las tareas relacionadas con la base de datos. En la cinta de opciones, haga clic en **nueva base de datos**.

![Botón 'Nueva base de datos' en la cinta de opciones de WebMatrix](displaying-data/_static/image4.png)

WebMatrix crea una base de datos de SQL Server CE (un *.sdf* archivo) que tiene el mismo nombre que su sitio &mdash; *WebPagesMovies.sdf*. (No hace esto aquí, pero puede cambiar el nombre del archivo a cualquier elemento que desee, siempre y cuando tenga un *.sdf* extensión.)

## <a name="creating-a-table"></a>Creación de una tabla

En la cinta de opciones, haga clic en **nueva tabla**. WebMatrix abre el Diseñador de tablas en una nueva pestaña. (Si la opción de nueva tabla no está disponible, asegúrese de que la nueva base de datos está seleccionada en la vista de árbol de la izquierda.)

![Botón 'Nueva tabla' en la cinta de opciones de WebMatrix](displaying-data/_static/image5.png)

En el cuadro de texto en la parte superior (donde la marca de agua dice "Escriba el nombre de la tabla"), escriba "Películas".

![Escribir un nombre de tabla en el Diseñador de la base de datos de WebMatrix](displaying-data/_static/image6.png)

El panel debajo del nombre de tabla es donde se definen las columnas individuales. Para el *películas* tabla en este tutorial, creará solo unas pocas columnas: *identificador*, *título*, *género*, y *año*.

En el **nombre** cuadro, escriba "ID". Si se introduce un valor aquí activa todos los controles para la nueva columna.

Tabulador para ir a la **tipo de datos** lista y elija **int**. Este valor especifica que la columna Id. contendrá datos enteros (número).

> [!NOTE]
> No lo llamaremos cualquier mas informacion (mucho), pero puede utilizar movimientos de teclado estándares de Windows para desplazarse en esta cuadrícula. Por ejemplo, puede cambiar mediante tabulación entre los campos, simplemente puede empezar a escribir con el fin de seleccionar un elemento de una lista y así sucesivamente.


Tabulador, cambie la **Default Value** cuadro (es decir, déjelo en blanco). Tabulador para ir a la **es la clave principal** casilla de verificación y selecciónelo. Esta opción indica a la base de datos que la *identificador* columna contendrá los datos que identifica las filas individuales. (Es decir, cada fila tendrá un valor único en la columna de identificador que puede usar para encontrar esa fila.)

Elija la **identidad** opción. Esta opción indica a la base de datos que debe generar automáticamente el siguiente número secuencial para cada nueva fila. (El **identidad** opción funciona sólo si también ha seleccionado el **es la clave principal** opción.)

Haga clic en la siguiente fila de cuadrícula, o presione la tecla Tab dos veces para finalizar la fila actual. Cualquier movimiento guarda la fila actual y siguiente inicia. Tenga en cuenta que la **Default Value** columna ahora dice **Null**. (Null es el valor predeterminado para el valor predeterminado, por lo que decirlo.)

Cuando haya terminado de definir el nuevo **identificador** columna, el diseñador tendrá un aspecto similar de esta ilustración:

![Diseñador de la base de datos de WebMatrix después de definir la columna de Id. de la tabla de películas](displaying-data/_static/image7.png)

Para crear la columna siguiente, haga clic en el cuadro en el **nombre** columna. Escriba "Título" para la columna y, a continuación, seleccione **nvarchar** para el **tipo de datos** valor. La parte de "var" de **nvarchar** indica que la base de datos que los datos de esta columna será una cadena cuyo tamaño puede variar entre los registros. (El prefijo "n" representa "national", lo que indica que el campo puede contener datos de caracteres para cualquier alfabeto o sistema de escritura, es decir, el campo contiene datos Unicode.)

Cuando eliges **nvarchar**, aparecerá otro cuadro donde puede especificar la longitud máxima para el campo. Escriba 50, en la suposición de que ningún título de la película que va a trabajar en este tutorial será mayor que 50 caracteres.

Omitir **Default Value** y desactive el **permitir valores NULL** opción. No desea permitir que las películas que deben especificarse en la base de datos que no tienen un título de la base de datos.

Cuando haya terminado y desplazarse a la fila siguiente, el diseñador es similar a esta ilustración:

![Diseñador de la base de datos de WebMatrix después de definir la columna de título para la tabla de películas](displaying-data/_static/image8.png)

Repita estos pasos para crear una columna denominada "Género", excepto para la longitud, establézcalo en simplemente 30.

Cree otra columna denominada "Año". Para el tipo de datos, elija **nchar** (no **nvarchar**) y establecer la longitud en 4. Para el año, va a utilizar un número de 4 dígitos como "1995" o "2010", por lo que no requieren una columna de tamaño variable.

Este es el aspecto del diseño finalizado:

![Diseñador de bases de datos de WebMatrix después de que todos los campos definidos para la tabla de películas](displaying-data/_static/image9.png)

Presione Ctrl + S o haga clic en el **guardar** botón en la barra de herramientas de acceso rápido. Cierre el Diseñador de la base de datos, el cierre de la ficha.

## <a name="adding-some-example-data"></a>Agregue algunos datos de ejemplo

Más adelante en esta serie de tutoriales creará una página donde puede escribir nuevas películas en un formulario. Por ahora, sin embargo, puede agregar algunos datos de ejemplo que, a continuación, puede mostrar en una página.

En el **base de datos** área de trabajo de WebMatrix, observe que hay un árbol que muestra la *.sdf* archivo creado anteriormente. Abra el nodo para el nuevo *.sdf* de archivos y, a continuación, abra el **tablas** nodo.

![Área de trabajo de base de datos de WebMatrix con árbol abierta a la tabla de películas](displaying-data/_static/image10.png)

Haga clic en el **películas** nodo y, a continuación, elija **datos**. WebMatrix abre una cuadrícula donde puede escribir los datos para la *películas* tabla:

![Cuadrícula de la entrada de base de datos en WebMatrix (vacío)](displaying-data/_static/image11.png)

Haga clic en el **título** columna y escriba "Cuando Harry cumplen Sally". Mover a la **género** columna (que puede utilizar la tecla Tab) y escriba "Románticos Comedia". Mover a la **año** columna y escriba "1989":

![Cuadrícula de entrada de base de datos con un registro en WebMatrix](displaying-data/_static/image12.png)

Presione ENTRAR y WebMatrix guarda la película nuevo. Tenga en cuenta que la **identificador** columna ha sido rellenada.

![Cuadrícula de entrada de base de datos de WebMatrix con un único registro y un identificador generado automáticamente](displaying-data/_static/image13.png)

Escriba otra película (por ejemplo, "ya no existe con el viento", "Series", "1939"). Se rellena la columna Id. nuevo:

![Cuadrícula de entrada de base de datos de WebMatrix con dos registros y los identificadores generados automáticamente](displaying-data/_static/image14.png)

Escriba una película terceros (por ejemplo, "Ghostbusters", "Comedia"). Como experimento, deje el **año** columna en blanco y, a continuación, presione ENTRAR. Dado que anula la selección de la **permitir valores NULL** opción, la base de datos muestra un error:

![Error de 'Datos no válidos' muestra si un valor de columna necesarios se deja en blanco](displaying-data/_static/image15.png)

Haga clic en **Aceptar** para volver atrás y corregir la entrada (el año correspondiente a "Ghostbusters" es 1984) y, a continuación, presione ENTRAR.

Rellene varias películas hasta que haya 8 o modo. (Escriba 8 resulta más fácil trabajar con paginación más adelante. Pero si es un número demasiado grande, escriba algunos por ahora). No importan los datos reales.

![Cuadrícula de entrada de base de datos de WebMatrix con cualquiera de los registros](displaying-data/_static/image16.png)

Si ha especificado todas las películas sin errores, los valores de identificador son secuenciales. Si intenta guardar un registro de película incompletos, los números de identificador no sea secuenciales. Si es así, que es correcto. Los números no tienen ningún significado inherente y lo único que es importante es que sean únicos en el *películas* tabla.

Cerrar la pestaña que contiene el Diseñador de la base de datos.

Ahora puede optar por mostrar estos datos en una página web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Mostrar datos en una página mediante la aplicación auxiliar WebGrid

Para mostrar datos en una página, va a utilizar el `WebGrid` auxiliar. Esta aplicación auxiliar produzca una presentación en una cuadrícula o una tabla (filas y columnas). Como verá, le puede refinar la cuadrícula con formato y otras características.

Para ejecutar la cuadrícula, tendrá que escribir unas cuantas líneas de código. Algunas de estas líneas le servirá como un tipo de modelo para casi todo el acceso a los datos que haga en este tutorial.

> [!NOTE]
> Realmente tiene muchas opciones para mostrar datos en una página; el `WebGrid` auxiliar es solo uno. Se eligió para este tutorial porque es la manera más fácil de mostrar los datos y porque es bastante flexible. En el siguiente conjunto de tutorial, verá cómo se utiliza una forma más "manual" para trabajar con datos en la página, que proporciona un control más directo sobre cómo mostrar los datos.


En el panel izquierdo en WebMatrix, haga clic en el **archivos** área de trabajo.

Es la nueva base de datos que creó en el *aplicación\_datos* carpeta. Si la carpeta no existe, WebMatrix creó para la nueva base de datos. (Podría haber existido la carpeta si anteriormente ha instalado aplicaciones auxiliares).

En la vista de árbol, seleccione la raíz del sitio Web. Debe seleccionar la raíz del sitio Web; en caso contrario, se podría agregar el nuevo archivo a la aplicación\_carpeta de datos.

En la cinta de opciones, haga clic en **nuevo**. En el **elegir un tipo de archivo** cuadro, elija **CSHTML**.

En el **nombre** cuadro, asigne el nombre de la nueva página "Movies.cshtml":

![Cuadro de diálogo 'Elegir un tipo de archivo' que muestra la página 'Películas'](displaying-data/_static/image17.png)

Haga clic en el **Aceptar** botón. WebMatrix abre un nuevo archivo con algunos elementos esqueletos en ella. En primer lugar, se escribirá código para ir a obtener los datos de la base de datos. A continuación, agregará el marcado a la página para mostrar los datos.

### <a name="writing-the-data-query-code"></a>Escribir el código de la consulta de datos

En la parte superior de la página, entre el `@{` y `}` caracteres, escriba el código siguiente. (Asegúrese de que se escribe este código entre las llaves de cierre y apertura).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La primera línea abre la base de datos que creó anteriormente, que siempre es el primer paso antes de realizar alguna acción con la base de datos. Puede indicar la `Database.Open` nombre del método de la base de datos para abrir. Tenga en cuenta que no incluye *.sdf* en el nombre. El `Open` método supone que está buscando un *.sdf* archivo (es decir, *WebPagesMovies.sdf*) y que la *.sdf* archivo se encuentra en la *aplicación\_ Datos* carpeta. (Anteriormente mencionamos que el *aplicación\_datos* carpeta está reservada; este escenario es uno de los lugares donde ASP.NET hace suposiciones sobre ese nombre.)

Cuando se abre la base de datos, una referencia a él se coloca en la variable denominada `db`. (Que puede tener cualquier nombre.) El `db` variable es cómo terminará interactuar con la base de datos.

La segunda línea realmente captura los datos de la base de datos mediante el uso de la `Query` método. Observe cómo funciona este código: el `db` variable tiene una referencia a la base de datos abierto, y se invoca el `Query` método utilizando el `db` variable (`db.Query`).

La propia consulta es una instancia de SQL `Select` instrucción. (Para algunos antecedentes acerca de SQL, vea la explicación más adelante). En la instrucción, `Movies` identifica la tabla a la consulta. El `*` carácter especifica que la consulta debe devolver todas las columnas de la tabla. (También puede hacer una lista las columnas individualmente, separados por comas.)

Los resultados de la consulta, si existe, se devuelve y están disponibles en la `selectedData` variable. Una vez más, la variable puede tener cualquier nombre.

Por último, la tercera línea indica a ASP.NET que desea usar una instancia de la `WebGrid` auxiliar. Crear (*crear instancias de*) el objeto auxiliar mediante el uso de la `new` palabra clave y pasar los resultados de consulta a través de la `selectedData` variable. El nuevo `WebGrid` objeto, junto con los resultados de la consulta de base de datos, se ponen a disposición de los `grid` variable. Necesitará que den como resultado en un momento para ver los datos en la página.

En esta fase, se ha abierto la base de datos, ha recibido los datos que desea y que ha preparado el `WebGrid` auxiliar con esos datos. A continuación consiste en crear el marcado en la página.

> [!TIP] 
> 
> **Lenguaje de consulta estructurado (SQL)**
> 
> SQL es un lenguaje que se utiliza en la mayoría de bases de datos relacionales para administrar los datos en una base de datos. Incluye comandos que permiten recuperar datos y actualizarlo y que le permiten crear, modificar y administrar datos en tablas de base de datos. SQL es diferente de un lenguaje de programación (como C#). Con SQL, puede indicar a la base de datos de la acción que realizará y es trabajo de la base de datos para averiguar cómo obtener los datos o realizar la tarea. Estos son algunos ejemplos de algunos comandos SQL y qué hacer:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La primera `Select` instrucción obtiene todas las columnas (especificado por `*`) desde el *películas* tabla.
> 
> El segundo `Select` instrucción captura las columnas Id., el nombre y el precio de registros en la *producto* tabla cuyo valor de columna precio es superior a 10. El comando devuelve los resultados en orden alfabético según los valores de la columna nombre. Si no hay registros que coincidan con los criterios de precio, el comando devuelve un conjunto vacío.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Este comando inserta un nuevo registro en el *producto* tabla, establecer la columna de nombre "Croissant", la columna Description para "Alegría de inestable A" y el precio en 1.99.
> 
> Tenga en cuenta que cuando está especificando un valor no numérico, el valor se encierra entre comillas simples (no comillas dobles, como en C#). Use estas comillas alrededor de los valores de texto o de fecha, pero no en torno a números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Este comando elimina los registros de la *producto* tabla cuya columna de fecha de expiración es anterior al 1 de enero de 2008. (El comando supone que la *producto* tabla tiene una columna de este tipo, por supuesto.) Se especifica la fecha en formato MM/DD/AAAA, pero debe especificarse en el formato que se usa para la configuración regional.
> 
> El `Insert` y `Delete` comandos no devuelven conjuntos de resultados. En su lugar, devuelven un número que indica el número de registros se vieron afectado por el comando.
> 
> Para algunas de estas operaciones (por ejemplo, insertar y eliminar registros), el proceso que está solicitando la operación debe tener los permisos adecuados en la base de datos. Que es la razón para bases de datos de producción a menudo tendrá que proporcionar un nombre de usuario y una contraseña cuando se conecta a la base de datos.
> 
> Existen docenas de comandos SQL, pero siguen un patrón como los comandos que ve aquí. Puede utilizar comandos SQL para crear tablas de base de datos, contar el número de registros de una tabla, calcular precios y realizar muchas más operaciones.


### <a name="adding-markup-to-display-the-data"></a>Agregar marcado para mostrar los datos

Dentro de la `<head>` elemento, conjunto de contenido de la `<title>` elemento a "Películas":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Dentro de la `<body>` elemento de la página, agregue lo siguiente:

[!code-html[Main](displaying-data/samples/sample3.html)]

Ya está. El `grid` variable es el valor que se creó al crear la `WebGrid` objeto en el código anterior.

En la vista de árbol de WebMatrix, haga clic en la página y seleccione **iniciar en un explorador**. Verá algo parecido a esta página:

![Salida de aplicación auxiliar WebGrid predeterminada de la tabla de películas](displaying-data/_static/image18.png)

Haga clic en un vínculo de encabezado de columna para ordenar por esa columna. Posibilidad de ordenar haciendo clic en un encabezado es una característica que está integrada en el **WebGrid** auxiliar.

El `GetHtml` método, tal y como sugiere su nombre, genera marcado que muestra los datos. De forma predeterminada, el `GetHtml` método genera HTML `<table>` elemento. (Si lo desea, puede comprobar la representación mediante la visualización en el origen de la página en el explorador.)

## <a name="modifying-the-look-of-the-grid"></a>Modificar la apariencia de la cuadrícula

Mediante el `WebGrid` auxiliar como hizo simplemente es fácil, pero la presentación resultante es sencillo. El `WebGrid` auxiliar tiene todo tipo de opciones que permiten controlar cómo se muestran los datos. Hay demasiadas para explorar en este tutorial, pero en esta sección le dará una idea de algunas de estas opciones. Algunas opciones adicionales se tratará en tutoriales posteriores de esta serie.

### <a name="specifying-individual-columns-to-display"></a>Especificar columnas individuales para mostrar

Para empezar, puede especificar que desea mostrar solo ciertas columnas. De forma predeterminada, tal y como se ha visto, la cuadrícula muestra los cuatro de las columnas de la *películas* tabla.

En el *Movies.cshtml* archivo, reemplace el `@grid.GetHtml()` marcado que acaba de agregar por lo siguiente:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para indicar qué columnas desea mostrar de la aplicación auxiliar, incluye un `columns` parámetro para el `GetHtml` método y pasar de una colección de columnas. En la colección, puede especificar cada columna que se va a incluir. Especificar una columna individual para mostrar mediante la inclusión de un `grid.Column` de objetos y pasa el nombre de la columna de datos que desea. (Estas columnas deben incluirse en los resultados de la consulta SQL: el `WebGrid` auxiliar no puede mostrar las columnas que no se han devuelto por la consulta.)

Iniciar el *Movies.cshtml* página en el explorador y esta vez llegue una pantalla como la siguiente (Observe que no se muestra ninguna columna de Id.):

![WebGrid que muestre sólo las columnas seleccionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Cambiar la apariencia de la cuadrícula

Hay bastantes más opciones para mostrar columnas, algunos de los cuales se explorarán en tutoriales más adelante en este conjunto. Por ahora, esta sección describe a formas en que puede aplicar el estilo de la cuadrícula como un todo.

Dentro de la `<head>` sección de la página, justo antes del cierre `</head>` etiqueta, agregue las siguientes `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Este marcado CSS define clases denominadas `grid`, `head`, y así sucesivamente. También puede colocar estas definiciones de estilo en otro *.css* de archivos y vincularla a la página. (De hecho, llevará a cabo más adelante en este tutorial conjunto.) Pero para facilitar la cosas para este tutorial, se usan dentro de la misma página que muestra los datos.

Ahora puede obtener el `WebGrid` auxiliar para utilizar estas clases de estilo. La aplicación auxiliar tiene un número de propiedades (por ejemplo, `tableStyle`) para este fin, se asigna un nombre de clase de estilo CSS a ellos, y ese nombre de clase se representa como parte del marcado que representa la aplicación auxiliar.

Cambiar el `grid.GetHtml` marcado para ese y ahora como se muestra en este código:

[!code-css[Main](displaying-data/samples/sample6.css)]

' S new aquí es que se ha agregado `tableStyle`, `headerStyle`, y `alternatingRowStyle` parámetros para el `GetHtml` método. Estos parámetros se encuentran en los nombres de los estilos CSS que agregó hace un momento.

Ejecute la página, y esta ocasión verá una cuadrícula que se parece mucho más sencillo que antes:

![Pantalla de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image20.png)

Para ver qué la `GetHtml` método generado, puede buscar en el origen de la página en el explorador. No entraremos en detalles aquí, pero lo importante es que al especificar parámetros como `tableStyle`, provocó la cuadrícula generar etiquetas HTML similar al siguiente:

`<table class="grid">`

El `<table>` etiqueta ha tenido un `class` atributo agregado a lo que hace referencia a una de las reglas CSS que agregó anteriormente. Este código muestra el patrón básico &mdash; parámetros diferentes para el `GetHtml` método permiten hacer referencia CSS clases que el método genera, a continuación, junto con el marcado. Qué hacer con las clases CSS depende de usted.

## <a name="adding-paging"></a>Agregar paginación

Como la última tarea para este tutorial, agregará la paginación a la cuadrícula. En estos momentos no es ningún problema para mostrar todas las películas a la vez. Pero si agrega cientos de películas, la presentación de página obtendría largo.

En el código de la página, cambie la línea que crea la `WebGrid` objeto en el código siguiente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

La única diferencia de antes es que se ha agregado un `rowsPerPage` parámetro que se establece en 3.

Ejecute la página. La cuadrícula muestra 3 filas en una hora más vínculos de navegación que permiten hojear las películas en la base de datos:

![Mostrar WebGrid con paginación](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, aprenderá cómo utilizar código Razor y C# para obtener proporcionados por el usuario en un formulario. Para que puedan encontrar películas por título o género, agregará un cuadro de búsqueda a la página de películas.

## <a name="complete-listing-for-movies-page"></a>Muestra la lista completa de la página de películas

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

>[!div class="step-by-step"]
[Anterior](intro-to-web-pages-programming.md)
[Siguiente](form-basics.md)
