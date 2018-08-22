---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 'Introducción a ASP.NET Web Pages: mostrar datos | Microsoft Docs'
author: tfitzmac
description: Este tutorial muestra cómo crear una base de datos en WebMatrix y cómo mostrar la base de datos en una página al usar ASP.NET Web Pages (Razor). Se supone y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829463"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introducción a las páginas Web ASP.NET - mostrar datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una base de datos en WebMatrix y cómo mostrar la base de datos en una página al usar ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [Introducción a la programación de ASP.NET Web Pages](../introducing-razor-syntax-c.md).
> 
> Lo que aprenderá:
> 
> - Cómo usar las herramientas de WebMatrix para crear una base de datos y tablas de base de datos.
> - Cómo usar las herramientas de WebMatrix para agregar datos a una base de datos.
> - Cómo mostrar los datos de la base de datos en una página.
> - Cómo ejecutar comandos SQL en ASP.NET Web Pages.
> - Cómo personalizar el `WebGrid` auxiliar para cambiar la presentación de datos y agregar paginación y ordenación.
>   
> 
> Características y tecnologías tratadas:
> 
> - Herramientas de base de datos de WebMatrix.
> - `WebGrid` aplicación auxiliar.


## <a name="what-youll-build"></a>¿Qué va a crear

En el tutorial anterior, se introdujeron en ASP.NET Web Pages (*.cshtml* archivos), los conceptos básicos de la sintaxis de Razor y a las aplicaciones auxiliares. En este tutorial, empezará a crear la aplicación web real que va a usar para el resto de la serie. La aplicación es una aplicación de película simple que le permite ver, agregar, cambiar y eliminar información sobre películas.

Cuando haya terminado con este tutorial, podrá ver una lista de películas que es similar a esta página:

![Pantalla de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image1.png)

Pero para empezar, debe crear una base de datos.

## <a name="a-very-brief-introduction-to-databases"></a>Una introducción breve a las bases de datos

Este tutorial le ofrece solo la briefest Introducción a las bases de datos. Si tiene experiencia en bases de datos, puede omitir esta sección corto.

Una base de datos contiene una o varias tablas que contienen información &mdash; por ejemplo, las tablas para los clientes, pedidos y proveedores, o para los estudiantes, profesores, clases y calificaciones. Estructuralmente, una tabla de base de datos es similar a una hoja de cálculo. Imagine una libreta de direcciones típico. Para cada entrada de la libreta de direcciones (es decir, para cada persona) tiene varias piezas de información como el nombre, apellido, dirección, dirección de correo electrónico y número de teléfono.

![Tabla de base de datos de ejemplo como una cuadrícula simple](displaying-data/_static/image2.png)

(Las filas se conocen a veces como *registros*, y las columnas se conocen a veces como *campos*.)

Para la mayoría de las tablas de base de datos, la tabla debe tener una columna que contiene un valor único, como un número de cliente, número de cuenta y así sucesivamente. Este valor se conoce como la tabla *clave principal*, y se usa para identificar cada fila de la tabla. En el ejemplo, la columna de identificador es la clave principal de la libreta de direcciones que se muestra en el ejemplo anterior.

Gran parte del trabajo que hacer en las aplicaciones web consta de al leer información de la base de datos y mostrarlos en una página. Con frecuencia, también podrá recopilar información de usuarios y agregarlo a una base de datos, o podrá modificar los registros que ya están en la base de datos. (Hablaremos sobre todas estas operaciones en el transcurso de este conjunto del tutorial.)

Trabajo de base de datos puede ser extremadamente complejo y puede requerir conocimientos especializados. Sin embargo, para esta serie de tutoriales, tiene que conocer solo de los conceptos básicos, que todo se explicará a medida que avance.

## <a name="creating-a-database"></a>Creación de una base de datos

WebMatrix incluye herramientas que facilitan la creación de una base de datos y crear tablas en la base de datos. (La estructura de una base de datos se conoce como la base de datos *esquema*.) Para este conjunto del tutorial, creará una base de datos que contiene solo una tabla de &mdash; películas.

Abra WebMatrix si aún no lo ha hecho y abra el sitio WebPagesMovies que creó en el tutorial anterior.

En el panel izquierdo, haga clic en el **base de datos** área de trabajo.

![Pestaña de área de trabajo de la base de datos de WebMatrix](displaying-data/_static/image3.png)

Los cambios de la cinta de opciones para mostrar las tareas relacionadas con la base de datos. En la cinta de opciones, haga clic en **nueva base de datos**.

![Botón 'Nueva base de datos' en la cinta de opciones de WebMatrix](displaying-data/_static/image4.png)

WebMatrix crea una base de datos de SQL Server CE (un *.sdf* archivo) que tiene el mismo nombre que su sitio &mdash; *WebPagesMovies.sdf*. (No hacer esto aquí, pero puede cambiar el nombre del archivo al que desee, siempre y cuando tenga un *.sdf* extensión.)

## <a name="creating-a-table"></a>Creación de una tabla

En la cinta de opciones, haga clic en **nueva tabla**. WebMatrix abre el Diseñador de tablas en una nueva pestaña. (Si la opción de nueva tabla no está disponible, asegúrese de que la nueva base de datos está seleccionado en la vista de árbol de la izquierda.)

![Botón "Nueva tabla" en la cinta de opciones de WebMatrix](displaying-data/_static/image5.png)

En el cuadro de texto en la parte superior (donde la marca de agua dice "Escriba el nombre de la tabla"), escriba "Movies".

![Escribir un nombre de tabla en el Diseñador de la base de datos de WebMatrix](displaying-data/_static/image6.png)

El panel debajo del nombre de tabla es donde se definen las columnas individuales. Para el *películas* tabla en este tutorial, creará solo unas pocas columnas: *ID*, *título*, *género*, y *año*.

En el **nombre** , escriba "ID". Si introduce un valor aquí activa todos los controles para la nueva columna.

Tabulador para ir a la **tipo de datos** lista y elija **int**. Este valor especifica que la columna de identificador contendrá datos enteros (número).

> [!NOTE]
> No lo llamamos cualquier más aquí (mucho), pero puede usar gestos de teclado estándares de Windows para navegar en esta cuadrícula. Por ejemplo, puede cambiar entre los campos, simplemente puede empezar a escribir con el fin de seleccionar un elemento en una lista y así sucesivamente.


Tabulador, cambie el **Default Value** cuadro (es decir, déjelo en blanco). Tabulador para ir a la **es la clave principal** casilla y selecciónelo. Esta opción indica a la base de datos que el *ID* columna contendrá los datos que identifica las filas individuales. (Es decir, cada fila tendrá un valor único en la columna de identificador que puede usar para encontrar esa fila.)

Elija la **identidad** opción. Esta opción indica a la base de datos que debe generar automáticamente el siguiente número secuencial para cada nueva fila. (El **identidad** opción solo si también seleccionó la **es la clave principal** opción.)

Haga clic en la siguiente fila de cuadrícula, o presione la tecla Tab dos veces para finalizar la fila actual. Cualquier movimiento guarda la fila actual e inicia la siguiente. Tenga en cuenta que el **Default Value** columna ahora dice **Null**. (Null es el valor predeterminado para el valor predeterminado, por así decirlo.)

Cuando haya terminado de definir el nuevo **ID** columna, el diseñador tendrá un aspecto similar de esta ilustración:

![Diseñador de la base de datos de WebMatrix después de definir la columna de Id. de la tabla de películas](displaying-data/_static/image7.png)

Para crear la columna siguiente, haga clic en el cuadro en el **nombre** columna. Escriba "Title" para la columna y, a continuación, seleccione **nvarchar** para el **tipo de datos** valor. La parte "var" de **nvarchar** indica a la base de datos que los datos de esta columna será una cadena cuyo tamaño puede variar entre los registros. (El prefijo "n" representa "national", lo que indica que el campo puede contener datos de caracteres para cualquier alfabeto o el sistema de escritura, es decir, el campo contiene datos Unicode.)

Cuando se elige **nvarchar**, aparece otro cuadro donde puede especificar la longitud máxima para el campo. Escriba 50, en la suposición de que ningún título de la película que se va a trabajar en este tutorial será más de 50 caracteres.

Omitir **Default Value** y desactive el **permitir valores nulos** opción. No desea permitir alguna película debe especificarse en la base de datos que no tiene un título de la base de datos.

Cuando haya terminado y desplazarse a la fila siguiente, el diseñador se parecerá a esta ilustración:

![Diseñador de la base de datos de WebMatrix después de definir la columna de título de la tabla de películas](displaying-data/_static/image8.png)

Repita estos pasos para crear una columna denominada "Género", excepto para la longitud, establézcalo en sólo 30.

Crear otra columna denominada "Año". Para el tipo de datos, elija **nchar** (no **nvarchar**) y establecer la longitud en 4. Para el año, va a usar un número de 4 dígitos, como "1995" o "2010", por lo que no requieren una columna de tamaño variable.

Este es el aspecto del diseño finalizado:

![Diseñador de base de datos de WebMatrix después de que todos los campos definidos para la tabla de películas](displaying-data/_static/image9.png)

Presione Ctrl + S o haga clic en el **guardar** botón en la barra de herramientas de acceso rápido. Cierre el Diseñador de la base de datos al cerrar la pestaña.

## <a name="adding-some-example-data"></a>Agregando algunos datos de ejemplo

Más adelante en esta serie de tutoriales, creará una página donde puede especificar nuevas películas en un formulario. Por ahora, sin embargo, puede agregar algunos datos de ejemplo que, a continuación, se pueden mostrar en una página.

En el **base de datos** área de trabajo de WebMatrix, observe que hay un árbol que muestra la *.sdf* archivo que creó anteriormente. Abra el nodo para el nuevo *.sdf* archivo y, a continuación, abra el **tablas** nodo.

![Área de trabajo de base de datos de WebMatrix con árbol abierto a la tabla de películas](displaying-data/_static/image10.png)

Haga clic en el **películas** nodo y, a continuación, elija **datos**. WebMatrix abre una cuadrícula donde puede escribir los datos para el *películas* tabla:

![Cuadrícula de entrada de la base de datos en WebMatrix (vacío)](displaying-data/_static/image11.png)

Haga clic en el **título** columna y escriba "Cuando Harry cumplen Sally". Mover a la **género** columna (que puede utilizar la tecla Tab) y escriba "Románticos Comedia". Mover a la **año** columna y escriba "1989":

![Cuadrícula de entrada de base de datos con un registro en WebMatrix](displaying-data/_static/image12.png)

Presione ENTRAR y WebMatrix guarda la nueva película. Tenga en cuenta que el **ID** columna ha sido rellenada.

![Cuadrícula de entrada de base de datos con un único registro y el identificador generado automáticamente en WebMatrix](displaying-data/_static/image13.png)

Escriba otra película (por ejemplo, "pasado con el viento", "Drama", "1939"). Se rellena la columna Id. nuevo:

![Cuadrícula de entrada de base de datos en WebMatrix con dos registros y los identificadores generados automáticamente](displaying-data/_static/image14.png)

Escriba una película terceros (por ejemplo, "Ghostbusters", "Comedia"). Como experimento, deje el **año** columna en blanco y, a continuación, presione ENTRAR. Dado que anula la selección de la **permitir valores nulos** opción, la base de datos muestra un error:

![Error de 'Datos no válidos' muestra si se deja en blanco el valor de una columna necesaria](displaying-data/_static/image15.png)

Haga clic en **Aceptar** para volver atrás y corregir la entrada (el año para "Ghostbusters" es 1984) y, a continuación, presione ENTRAR.

Rellene varias películas hasta que haya 8 o al menos. (Escriba 8 resulta más fácil trabajar con paginación más adelante. Pero si es demasiado elevado, escriba algunas por ahora). No importan los datos reales.

![Cuadrícula de entrada de base de datos en WebMatrix con ambos registros](displaying-data/_static/image16.png)

Si ha especificado todas las películas sin errores, los valores de identificador son secuenciales. Si intenta guardar un registro de película incompletos, los números de identificador no sea secuenciales. En caso afirmativo, eso está bien. Los números no tienen ningún significado inherente y lo único que es importante es que son únicos en el *películas* tabla.

Cierre la pestaña que contiene el Diseñador de la base de datos.

Ahora puede activar para mostrar los datos en una página web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Mostrar datos en una página mediante el uso de la aplicación auxiliar WebGrid

Para mostrar datos en una página, que va a usar el `WebGrid` auxiliar. Esta aplicación auxiliar genera una pantalla en una cuadrícula o una tabla (las filas y columnas). Como verá, será capaz de restringir la cuadrícula con formato y otras características.

Para ejecutar la cuadrícula, tendrá que escribir unas cuantas líneas de código. Estas pocas líneas actuará como un tipo de modelo para casi todo el acceso a los datos que en este tutorial.

> [!NOTE]
> Tiene muchas opciones para mostrar los datos en una página; en realidad el `WebGrid` auxiliar es solo uno. Se eligió para este tutorial porque es la manera más fácil para mostrar los datos y porque es bastante flexible. En el conjunto del tutorial siguiente, verá cómo usar la forma "manual" más para trabajar con datos en la página, que ofrece un control más directo sobre cómo mostrar los datos.


En el panel izquierdo en WebMatrix, haga clic en el **archivos** área de trabajo.

Es la nueva base de datos que creó en el *aplicación\_datos* carpeta. Si aún no existe la carpeta, WebMatrix creó para la nueva base de datos. (Podría haber existido la carpeta si había instalado previamente las aplicaciones auxiliares).

En la vista de árbol, seleccione la raíz del sitio Web. Debe seleccionar la raíz del sitio Web; en caso contrario, el nuevo archivo podría agregarse a la aplicación\_carpeta de datos.

En la cinta de opciones, haga clic en **New**. En el **elegir un tipo de archivo** , seleccione **CSHTML**.

En el **nombre** cuadro, asigne el nombre de la nueva página "Movies.cshtml":

![Cuadro de diálogo "Elegir un tipo de archivo" que muestra la página de 'películas;'](displaying-data/_static/image17.png)

Haga clic en el **Aceptar** botón. WebMatrix abre un nuevo archivo con algunos elementos esqueletos en ella. En primer lugar deberá escribir algo de código para obtener los datos de la base de datos. A continuación, agregará el marcado en la página para mostrar los datos.

### <a name="writing-the-data-query-code"></a>Escribir el código de la consulta de datos

En la parte superior de la página, entre el `@{` y `}` caracteres, escriba el código siguiente. (Asegúrese de que escriba este código entre las llaves de cierre y apertura).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La primera línea abre la base de datos que creó anteriormente, que siempre es el primer paso antes de hacer algo con la base de datos. Indica el `Database.Open` nombre del método de la base de datos para abrir. Tenga en cuenta que no incluye *.sdf* en el nombre. El `Open` método supone que está buscando un *.sdf* archivo (es decir, *WebPagesMovies.sdf*) y que la *.sdf* archivo se encuentra en la *aplicación\_ Datos* carpeta. (Anteriormente mencionamos que el *aplicación\_datos* carpeta está reservada; este escenario es uno de los lugares donde ASP.NET hace suposiciones sobre ese nombre.)

Cuando se abre la base de datos, se coloca una referencia a él en la variable denominada `db`. (Lo que podría tener cualquier nombre.) El `db` variable es cómo terminará interactuar con la base de datos.

La segunda línea realmente captura los datos de la base de datos mediante el uso de la `Query` método. Tenga en cuenta cómo funciona este código: el `db` variable tiene una referencia a la base de datos abierto y se invoca el `Query` método utilizando la `db` variable (`db.Query`).

La propia consulta es una instancia de SQL `Select` instrucción. (Para cierta información básica acerca de SQL, vea la explicación más adelante). En la instrucción, `Movies` identifica la tabla a la consulta. El `*` carácter especifica que la consulta debe devolver todas las columnas de la tabla. (Podría también mostrar columnas individualmente, separados por comas.)

Los resultados de la consulta, si existe, se devuelven y se ponen a disposición de los `selectedData` variable. De nuevo, la variable puede tener cualquier nombre.

Por último, la tercera línea indica a ASP.NET que desea usar una instancia de la `WebGrid` auxiliar. Crear (*crear instancias de*) el objeto auxiliar mediante el uso de la `new` palabra clave y le pasa los resultados de consulta a través de la `selectedData` variable. El nuevo `WebGrid` objeto, junto con los resultados de la consulta de base de datos, se ponen a disposición en el `grid` variable. Necesitará ese resultado en un momento para ver los datos en la página.

En esta fase, se ha abierto la base de datos, ha obtenido los datos que desea y que ha preparado el `WebGrid` auxiliar con esos datos. A continuación es crear el marcado en la página.

> [!TIP] 
> 
> **Lenguaje de consulta estructurado (SQL)**
> 
> SQL es un lenguaje que se usa en muchas bases de datos relacionales para administrar los datos en una base de datos. Incluye los comandos que le permiten recuperar datos y actualizarlo y que le permiten crear, modificar y administrar datos en tablas de base de datos. SQL no es un lenguaje de programación (por ejemplo, C#). Con SQL, indicar la base de datos, lo que desea y trabajo de la base de datos consiste en averiguar cómo obtener los datos o realizar la tarea. Estos son algunos ejemplos de algunos comandos SQL y qué hacer:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La primera `Select` instrucción obtiene todas las columnas (especificado por `*`) desde el *películas* tabla.
> 
> El segundo `Select` instrucción recupera las columnas ID, Name y precio de los registros en el *producto* tabla cuyo valor de la columna precio es superior a 10. El comando devuelve los resultados en orden alfabético según los valores de la columna nombre. Si no hay registros que coincidan con los criterios de precio, el comando devuelve un conjunto vacío.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Este comando inserta un nuevo registro en el *producto* tabla, al establecer la columna nombre para el precio, la columna Description para "Los usuarios no confiables A" y "Croissant" a 1.99.
> 
> Tenga en cuenta que, al especificar un valor no numérico, el valor esté entre comillas simples (no comillas dobles, como en C#). Utilice estas comillas alrededor de los valores de texto o fecha, pero no en torno a los números.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Este comando elimina los registros de la *producto* tabla cuya columna de fecha de expiración es anterior al 1 de enero de 2008. (El comando supone que el *producto* tabla tiene una columna de este tipo, por supuesto.) Se especifica la fecha en formato MM/DD/AAAA, pero se debe escribir en el formato que se usa para la configuración regional.
> 
> El `Insert` y `Delete` comandos no devuelven conjuntos de resultados. En su lugar, devuelven un número que indica el número de registros afectado por el comando.
> 
> Para algunas de estas operaciones (por ejemplo, insertar y eliminar registros), el proceso que solicita la operación debe tener los permisos adecuados en la base de datos. Es por eso para bases de datos de producción a menudo tiene que proporcionar un nombre de usuario y una contraseña cuando se conecta a la base de datos.
> 
> Existen docenas de comandos SQL, pero siguen un patrón similar a los comandos que aparecen aquí. Puede usar los comandos SQL para crear tablas de base de datos, contar el número de registros en una tabla, calcular precios y realizar muchas más operaciones.


### <a name="adding-markup-to-display-the-data"></a>Adición de marcado para mostrar los datos

Dentro de la `<head>` elemento, el conjunto de contenido de la `<title>` elemento a "Movies":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Dentro de la `<body>` elemento de la página, agregue lo siguiente:

[!code-html[Main](displaying-data/samples/sample3.html)]

Ya está. El `grid` variable es el valor que creó cuando creó la `WebGrid` objeto en el código anterior.

En la vista de árbol de WebMatrix, haga clic en la página y seleccione **iniciar en el explorador**. Verá algo parecido a esta página:

![Salida de aplicación auxiliar WebGrid predeterminada de la tabla de películas](displaying-data/_static/image18.png)

Haga clic en un vínculo de encabezado de columna para ordenar por esa columna. Poder ordenar haciendo clic en un encabezado es una característica que está integrada en el **WebGrid** auxiliar.

El `GetHtml` método, como su nombre sugiere, genera marcado que muestra los datos. De forma predeterminada, el `GetHtml` método genera un elemento HTML `<table>` elemento. (Si lo desea, puede comprobar la representación examinando el origen de la página en el explorador).

## <a name="modifying-the-look-of-the-grid"></a>Modificar la apariencia de la cuadrícula

Mediante el `WebGrid` auxiliar como acaba de hacer es fácil, pero la presentación resultante es simple. El `WebGrid` auxiliar tiene todo tipo de opciones que permiten controlar cómo se muestran los datos. Hay demasiadas para explorar en este tutorial, pero en esta sección le dará una idea de algunas de esas opciones. Algunas opciones adicionales se tratarán en otros tutoriales de esta serie.

### <a name="specifying-individual-columns-to-display"></a>Especificar columnas individuales para mostrar

Para empezar, puede especificar que desea mostrar solo algunas columnas. De forma predeterminada, como ha visto, la cuadrícula muestra cuatro de las columnas de la *películas* tabla.

En el *Movies.cshtml* de archivo, reemplace el `@grid.GetHtml()` marcado que acaba de agregar por lo siguiente:

[!code-css[Main](displaying-data/samples/sample4.css)]

Para indicar a qué columnas desea mostrar de la aplicación auxiliar, incluye un `columns` parámetro para el `GetHtml` método y pasar una colección de columnas. En la colección, debe especificar cada columna que se va a incluir. Especifique una columna individual para mostrar mediante la inclusión de un `grid.Column` de objetos y pase el nombre de la columna de datos que desee. (Estas columnas deben incluirse en los resultados de consulta SQL: el `WebGrid` auxiliar no puede mostrar las columnas que no se han devuelto por la consulta.)

Iniciar el *Movies.cshtml* página en el explorador y esta vez obtendrá una pantalla como la siguiente (tenga en cuenta que no se muestra ninguna columna de identificador):

![WebGrid que muestre solo las columnas seleccionadas](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Cambiar la apariencia de la cuadrícula

Hay bastantes más opciones para mostrar columnas, algunas de las cuales se analizarán en los tutoriales posteriores de este conjunto. Por ahora, esta sección le presentará maneras en que puede aplicar el estilo de la cuadrícula como un todo.

Dentro de la `<head>` sección de la página, justo antes de cerrar `</head>` etiqueta, agregue las siguientes `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Este marcado CSS define clases denominadas `grid`, `head`, y así sucesivamente. También puede poner estas definiciones de estilo en otro *.css* de archivos y vincularla a la página. (De hecho, se hará más adelante en este conjunto del tutorial.) Pero para facilitar las cosas para este tutorial, dentro de la misma página que muestra los datos.

Ahora puede obtener el `WebGrid` auxiliar para utilizar estas clases de estilo. La aplicación auxiliar tiene un número de propiedades (por ejemplo, `tableStyle`) para este fin, asigne un nombre de clase de estilo CSS a ellos, y ese nombre de clase se representa como parte del marcado que representa la aplicación auxiliar.

Cambiar el `grid.GetHtml` marcado para que ahora quede similar a este código:

[!code-css[Main](displaying-data/samples/sample6.css)]

Lo nuevo aquí es que se ha agregado `tableStyle`, `headerStyle`, y `alternatingRowStyle` parámetros para el `GetHtml` método. Estos parámetros se han establecido los nombres de los estilos CSS que agregó hace un momento.

Ejecute la página, y esta vez verá una cuadrícula que se parece mucho menos simple que antes:

![Pantalla de WebGrid que incluye los parámetros establecidos en los nombres de clase CSS](displaying-data/_static/image20.png)

Para ver qué la `GetHtml` método generado, puede buscar en el origen de la página en el explorador. No entraremos en detalles aquí, pero lo importante es que mediante la especificación de parámetros como `tableStyle`, provocó la cuadrícula generar etiquetas HTML similar al siguiente:

`<table class="grid">`

El `<table>` etiqueta a la que ha tenido un `class` atributo agregado a lo que hace referencia a una de las reglas CSS que agregó anteriormente. Este código muestra el patrón básico &mdash; parámetros diferentes para el `GetHtml` let, referencia de los métodos CSS clases que, a continuación, genera el método junto con el marcado. Qué hacer con las clases CSS depende de usted.

## <a name="adding-paging"></a>Agregar paginación

Como la última tarea para este tutorial, agregará paginación a la cuadrícula. En estos momentos no supone ningún problema para mostrar todas las películas a la vez. Pero si ha agregado cientos de películas, se obtendría mucho la presentación de página.

En el código de página, cambie la línea que crea la `WebGrid` objeto en el código siguiente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

La única diferencia respecto antes es que se ha agregado un `rowsPerPage` parámetro que se establece en 3.

Ejecute la página. La cuadrícula muestra 3 filas en un tiempo, además de vínculos de navegación que permiten la página a través de las películas en la base de datos:

![Visualización de WebGrid con paginación](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, obtendrá información sobre cómo usar código de Razor y C# para obtener la entrada de usuario en un formulario. Por lo que puede buscar películas por título o género, agregará un cuadro de búsqueda a la página de películas.

## <a name="complete-listing-for-movies-page"></a>Lista completa de la página de películas

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Anterior](intro-to-web-pages-programming.md)
> [Siguiente](form-basics.md)
