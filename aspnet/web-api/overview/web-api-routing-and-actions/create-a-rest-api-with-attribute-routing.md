---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Crear una API de REST con el atributo enrutamiento en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 9ecc233e595716a167ad800a0a21a6162b051648
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Crear una API de REST con atributo enrutar en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2 es compatible con un nuevo tipo de enrutamiento, denominado *atributo enrutamiento*. Para obtener una descripción general de la ruta de atributo, vea [atributo enrutamiento en API Web 2](attribute-routing-in-web-api-2.md). En este tutorial, utilizará el enrutamiento de atributo para crear una API de REST para una colección de libros. La API será compatible con las siguientes acciones:

| Acción | URI de ejemplo |
| --- | --- |
| Obtener una lista de todos los libros. | / api/libros |
| Obtener un libro por identificador. | /API/Books/1 |
| Obtener los detalles de un libro. | /API/Books/1/Details |
| Obtener una lista de libros por género. | /API/Books/fantasy |
| Obtener una lista de libros por fecha de publicación. | /API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (forma alternativa) |
| Obtener una lista de libros por un autor concreto. | /API/authors/1/Books |

Todos los métodos son de solo lectura (las solicitudes HTTP GET).

Para la capa de datos, vamos a usar Entity Framework. Registros de libros tendrá los siguientes campos:

- Id.
- Título
- Género
- Fecha de publicación
- Precio
- Descripción
- AuthorID (clave externa a una tabla Authors)

Sin embargo, para la mayoría de las solicitudes, la API devolverá un subconjunto de estos datos (título, autor y género). Para obtener el registro completo, el cliente solicitudes `/api/books/{id}/details`.

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Comience ejecutando Visual Studio. Desde el **archivo** menú, seleccione **New** y, a continuación, seleccione **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **vacía** plantilla. En "Agregar carpetas y principales referencias de", seleccione la **API Web** casilla de verificación. Haga clic en **crear proyecto**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Esto crea un proyecto de esqueleto que está configurado para la funcionalidad de la API Web.

### <a name="domain-models"></a>Modelos de dominio

A continuación, agregue las clases para los modelos de dominio. En el Explorador de soluciones, haga clic en la carpeta Models. Seleccione **agregar**, a continuación, seleccione **clase**. Asigne a la clase el nombre `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Reemplace el código en Author.cs con lo siguiente:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Ahora agregue otra clase denominada `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Agregar un controlador de API Web

En este paso, agregaremos un controlador de API Web que utiliza Entity Framework como la capa de datos.

Presione Ctrl+Mayús+B para compilar el proyecto. Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado crear el esquema de base de datos.

En el Explorador de soluciones, haga clic en la carpeta de controladores. Seleccione **agregar**, a continuación, seleccione **controlador**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

En el **agregar scaffolding** cuadro de diálogo, seleccione "Web API 2 controlador con acciones de lectura/escritura, que usan Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

En el **Agregar controlador** cuadro de diálogo, para **nombre de controlador**, escriba &quot;BooksController&quot;. Seleccione el &quot;utilizar las acciones de controlador asincrónico&quot; casilla de verificación. Para **clase modelo**, seleccione &quot;libreta&quot;. (Si no ve la `Book` clase aparece en la lista desplegable, asegúrese de que ha generado el proyecto.) A continuación, haga clic en el botón "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Haga clic en **agregar** en el **nuevo contexto de datos** cuadro de diálogo.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Haga clic en **agregar** en el **Agregar controlador** cuadro de diálogo. La técnica scaffolding agrega una clase denominada `BooksController` que define el controlador de API. También agrega una clase denominada `BooksAPIContext` en la carpeta Models, que define el contexto de datos de Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Valor de inicialización de la base de datos

En el menú Herramientas, seleccione **Administrador de paquetes de biblioteca**y, a continuación, seleccione **Package Manager Console**.

En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Este comando crea una carpeta de las migraciones y agrega un nuevo archivo de código con el nombre de archivo Configuration.cs que. Abra este archivo y agregue el código siguiente a la `Configuration.Seed` método.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Estos comandos creación una base de datos local e invocación el método de inicialización para rellenar la base de datos.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Agregar clases DTO

Si ejecuta la aplicación y envía una solicitud GET a /api/books/1, la respuesta es similar al siguiente. (Agregado sangría para facilitar la lectura).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

En su lugar, deseo que esta solicitud para devolver un subconjunto de los campos. Además, deseo para devolver el nombre del autor, en lugar de por el identificador del autor. Para lograr esto, se modificará los métodos de controlador para devolver un *objeto de transferencia de datos* (DTO) en lugar del modelo EF. Un DTO es un objeto que está diseñado solo para transportar datos.

En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** | **nueva carpeta**. Nombre de la carpeta &quot;dto&quot;. Agregue una clase denominada `BookDto` hasta la carpeta dto, con la siguiente definición:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Agregue otra clase denominada `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

A continuación, actualice el `BooksController` clase para devolver `BookDto` instancias. Vamos a usar la [Queryable.Select](https://msdn.microsoft.com/en-us/library/system.linq.queryable.select.aspx) método al proyecto `Book` instancias de `BookDto` instancias. Este es el código actualizado para la clase de controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Se ha eliminado el `PutBook`, `PostBook`, y `DeleteBook` métodos, porque no son necesarios para este tutorial.


Ahora, ejecute la aplicación y solicitar /api/books/1, el cuerpo de respuesta debe ser similar al siguiente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Agregar atributos de ruta

A continuación, se podrá convertir el controlador para usar el enrutamiento de atributo. En primer lugar, agregue un **RoutePrefix** atributo al controlador. Este atributo define los segmentos URI iniciales para todos los métodos en este controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

A continuación, agregue **[ruta]** atributos a las acciones de controlador, como se indica a continuación:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

La plantilla de ruta para cada método de controlador es el prefijo además de la cadena especificada en el **ruta** atributo. Para el `GetBook` método, la plantilla de ruta incluye la cadena con parámetros &quot;{Id.: int}&quot;, que hace coincidir si el segmento URI contiene un valor entero.

| Método | Plantilla de ruta | URI de ejemplo |
| --- | --- | --- |
| `GetBooks` | "api/libros" | `http://localhost/api/books` |
| `GetBook` | "api/libros / {Id.: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Obtener detalles del libro

Para obtener detalles del libro, el cliente enviará una solicitud GET en `/api/books/{id}/details`, donde *{id}* es el identificador del libro.

Agregue el método siguiente a la clase `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Si se solicitan `/api/books/1/details`, la respuesta tiene el siguiente aspecto:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Obtener libros por género

Para obtener una lista de libros de un género específico, el cliente enviará una solicitud GET en `/api/books/genre`, donde *género* es el nombre del género. (Por ejemplo: `/get/books/fantasy`.)

Agregue el método siguiente a `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Aquí estamos vamos a definir una ruta que contiene un parámetro de {género} en la plantilla URI. Observe que la API Web es capaz de distinguir a estos dos URI y enrutarlas a diferentes métodos:

`/api/books/1`

`/api/books/fantasy`

Esto es así porque el `GetBook` método incluye una restricción de que el segmento "id" debe ser un valor entero:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Si se solicitan /api/books/fantasy, la respuesta es similar al siguiente:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Obtener libros por autor

Para obtener una lista de un libros de un autor concreto, el cliente enviará una solicitud GET en `/api/authors/id/books`, donde *identificador* es el identificador del autor.

Agregue el método siguiente a `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

En este ejemplo es interesante porque &quot;libros&quot; es trata un recurso secundario de &quot;autores&quot;. Este patrón es bastante común en las API de REST.

La tilde (~) en la plantilla de ruta invalida el prefijo de ruta en el **RoutePrefix** atributo.

## <a name="get-books-by-publication-date"></a>Obtener libros por fecha de publicación

Para obtener una lista de libros por fecha de publicación, el cliente enviará una solicitud GET en `/api/books/date/yyyy-mm-dd`, donde *aaaa-mm-dd* es la fecha.

Aquí es una manera de hacerlo:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

El `{pubdate:datetime}` parámetro está restringido para que coincida con un **DateTime** valor. Esto funciona, pero es realmente más permisivo que nos gustaría. Por ejemplo, estos URI también encontrarán coincidencias de la ruta:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

No hay ningún problema con lo que a estos URI. Sin embargo, puede restringir la ruta a un formato determinado mediante la adición de una restricción de expresión regular para la plantilla de ruta:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Ahora sólo las fechas en el formulario &quot;aaaa-mm-dd&quot; coincidirá. Tenga en cuenta que no usamos la expresión regular para validar que obtuvimos una fecha real. Que se controla al API Web intenta convertir el segmento URI en una **DateTime** instancia. Una fecha no válida, como ' 2012-47-99' no se puede convertir, y el cliente reciba un error 404.

También puede disponer de un separador de barra diagonal (`/api/books/date/yyyy/mm/dd`) mediante la adición de otra **[ruta]** atributo con una regex diferentes.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Hay un detalle sutil pero importante. La segunda plantilla de ruta tenga un carácter comodín (\*) al principio del parámetro {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Esto indica que el motor de enrutamiento que {pubdate} debe coincidir con el resto de la dirección URI. De forma predeterminada, un parámetro de plantilla coincide con un único segmento URI. En este caso, queremos {pubdate} que abarquen varios segmentos URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Código del controlador

Este es el código completo para la clase BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Resumen

Atributo enrutamiento ofrece más control y una mayor flexibilidad al diseñar a los URI de la API.
