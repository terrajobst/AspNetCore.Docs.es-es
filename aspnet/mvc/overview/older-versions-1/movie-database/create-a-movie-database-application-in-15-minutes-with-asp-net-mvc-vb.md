---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: "Crear una aplicación de base de datos de la película en 15 minutos con MVC de ASP.NET (VB) | Documentos de Microsoft"
author: StephenWalther
description: "Stephen Walther compila una todo controlada por la base de datos ASP.NET MVC la aplicación desde el principio para finalizar. Este tutorial se muestra una introducción excelente para las personas que son t nuevo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 4dbb3804bbb0ccb80506a592f1efb585c5748c2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Crear una aplicación de base de datos de la película en 15 minutos con MVC de ASP.NET (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descargar código](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther compila una todo controlada por la base de datos ASP.NET MVC la aplicación desde el principio para finalizar. Este tutorial se muestra una introducción excelente para las personas que son nuevas en el marco de MVC de ASP.NET y que desean para hacerse una idea del proceso de creación de una aplicación de ASP.NET MVC.


El propósito de este tutorial es para proporcionarle una idea de "¿Qué es como" para crear una aplicación ASP.NET MVC. En este tutorial, poner en funcionamiento mediante la creación de una aplicación de ASP.NET MVC toda desde el principio para finalizar. Mostrarle cómo compilar una aplicación sencilla de bases de datos que se muestra cómo enumerar, crear y editar registros de base de datos.

Para simplificar el proceso de creación de nuestra aplicación, se podrá sacar partido de las características de scaffolding de Visual Studio 2008. Dejaremos que Visual Studio genera el código inicial y el contenido de nuestro controladores, modelos y vistas.

Si ha trabajado con páginas ASP o ASP.NET, a continuación, encontrará ASP.NET MVC muy familiar. Las vistas de MVC de ASP.NET son muy similares a las páginas en una aplicación de páginas Active Server. Y, al igual que una aplicación de formularios Web Forms de ASP.NET tradicional, ASP.NET MVC proporciona acceso completo al conjunto enriquecido de idiomas y las clases proporcionadas por .NET framework.

Mi esperanza es que este tutorial le proporcionará una idea de cómo la experiencia de creación de una aplicación ASP.NET MVC es diferente de la experiencia de creación de una aplicación de páginas Active Server o de formularios Web Forms de ASP.NET y similares.

## <a name="overview-of-the-movie-database-application"></a>Información general de la aplicación de base de datos de la película

Dado que nuestro objetivo es no complicar las cosas, vamos a crear una aplicación de base de datos de película muy sencilla. Nuestra aplicación de base de datos de película simple nos permitirá hacer tres cosas:

1. Enumerar un conjunto de registros de base de datos de la película
2. Crear un nuevo registro de base de datos de película
3. Editar un registro de base de datos existente de película

Nuevo, puesto que deseamos no complicar las cosas, se podrá aprovechar el número mínimo de características del marco de ASP.NET MVC necesarios para compilar la aplicación. Por ejemplo, se no pueden sacar partido de desarrollo controlado por pruebas.

Para crear nuestra aplicación, es necesario completar cada uno de los siguientes pasos:

1. Crear el proyecto de aplicación Web de ASP.NET MVC
2. Crear la base de datos
3. Crear el modelo de base de datos
4. Crear el controlador de MVC de ASP.NET
5. Crear vistas de ASP.NET MVC

## <a name="preliminaries"></a>Pasos preliminares

Necesitará Visual Studio 2008 o Visual Web Developer 2008 Express para compilar una aplicación de ASP.NET MVC. También debe descargar el marco de MVC de ASP.NET.

Si no tiene Visual Studio 2008, puede descargar una versión de prueba de 90 días de Visual Studio 2008 desde este sitio Web:

[https://msdn.Microsoft.com/en-us/VS2008/Products/cc268305.aspx](https://msdn.microsoft.com/en-us/vs2008/products/cc268305.aspx)

Como alternativa, puede crear ASP.NET MVC aplicaciones con Visual Web Developer Express 2008. Si opta por utilizar Visual Web Developer Express, a continuación, debe tener instalado Service Pack 1. Puede descargar Visual Web Developer 2008 Express con Service Pack 1 desde este sitio Web:

[https://www.Microsoft.com/downloads/details.aspx?FamilyID=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang = es](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Después de instalar Visual Studio 2008 o Visual Web Developer 2008, debe instalar el marco de MVC de ASP.NET. Puede descargar el marco de MVC de ASP.NET desde el siguiente sitio Web:

[https://www.ASP.NET/MVC/](../../../index.md)

> [!NOTE] 
> 
> En lugar de descargar el marco ASP.NET y el marco de MVC de ASP.NET de forma individual, puede aprovechar las ventajas del instalador de plataforma Web. El instalador de plataforma Web es una aplicación que le permite administrar fácilmente las aplicaciones instaladas son el equipo:
> 
> [https://www.Microsoft.com/Web/Gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Crear un proyecto de aplicación Web de ASP.NET MVC

Empecemos creando un nuevo proyecto de aplicación Web ASP.NET MVC en Visual Studio 2008. Seleccione la opción de menú **archivo, nuevo proyecto** y verá el cuadro de diálogo nuevo proyecto en la figura 1. Seleccione Visual Basic como el lenguaje de programación y seleccione la plantilla de proyecto de aplicación Web ASP.NET MVC. Asigne al proyecto el nombre MovieApp y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Figura 01**: cuadro de diálogo de nuevo el proyecto ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


Asegúrese de que selecciona .NET Framework 3.5 en la lista desplegable en la parte superior del cuadro de diálogo nuevo proyecto o la plantilla de proyecto de aplicación Web ASP.NET MVC no aparecerá.


Siempre que cree un nuevo proyecto de aplicación Web de MVC, Visual Studio le pedirá que cree un proyecto de prueba de unidad independiente. Aparece el cuadro de diálogo en la figura 2. Dado que se no puede crear pruebas en este tutorial debido a las restricciones de tiempo (y Sí, debemos sentirnos culpables un poco acerca de este) seleccione la **No** opción y haga clic en el **Aceptar** botón.

> [!NOTE] 
> 
> Visual Web Developer no admite proyectos de prueba.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Figura 02**: cuadro de diálogo el proyecto de prueba unitaria crear ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Una aplicación de MVC de ASP.NET tiene un conjunto estándar de carpetas: una carpeta de modelos, vistas y controladores. Puede ver este conjunto estándar de carpetas en la ventana Explorador de soluciones. Necesitamos agregar archivos a cada una de las carpetas de modelos, vistas y controladores para crear nuestra aplicación de base de datos de película.

Cuando se crea una nueva aplicación MVC con Visual Studio, obtendrá una aplicación de ejemplo. Puesto que deseamos empezar desde cero, es necesario eliminar el contenido de esta aplicación de ejemplo. Debe eliminar el siguiente archivo y la carpeta siguiente:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Crear la base de datos

Es necesario crear una base de datos para albergar nuestros registros de base de datos de la película. Afortunadamente, Visual Studio incluye una base de datos gratuita denominado SQL Server Express. Siga estos pasos para crear la base de datos:

1. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione la **base de datos de SQL Server** plantilla (consulte la figura 3).
3. Nombre de la base de datos nueva *MoviesDB.mdf* y haga clic en el **agregar** botón.

Después de crear la base de datos, puede conectarse a la base de datos haciendo doble clic en el archivo MoviesDB.mdf ubicado en la aplicación\_carpeta de datos. Haga doble clic en el archivo MoviesDB.mdf, se abre la ventana Explorador de servidores.

> [!NOTE] 
> 
> La ventana Explorador de servidores con el nombre de la ventana del explorador de base de datos en el caso de Visual Web Developer.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Figura 03**: crear una base de datos de Microsoft SQL Server ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


A continuación, necesitamos crear una nueva tabla de base de datos. Desde dentro de la ventana Explorador de servidores, haga clic en la carpeta de tablas y seleccione la opción de menú **agregar nueva tabla**. Al seleccionar esta opción de menú, abre el Diseñador de tablas de base de datos. Cree las columnas de base de datos siguientes:

<a id="0.2_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | nvarchar (100) | False |
| Director de | nvarchar (100) | False |
| DateReleased | DateTime | False |


La primera columna, la columna Id., tiene dos propiedades especiales. En primer lugar, debe marcar la columna de identificador como columna de clave principal. Después de seleccionar la columna Id., haga clic en el **establecer clave principal** botón (será el icono que es similar a una clave). En segundo lugar, debe marcar la columna de identificador como una columna de identidad. En la ventana Propiedades de columna, desplácese hasta la sección de la especificación de identidad y expándalo. Cambiar el **identidad** valor para la propiedad **Sí**. Cuando haya terminado, la tabla debería parecerse a la figura 4.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Figura 04**: tabla de base de datos de las películas ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Es el último paso guardar la nueva tabla. Haga clic en el botón Guardar (el icono del disquete) y a las películas de nombre de la tabla nueva.

Cuando termine de crear la tabla, agregar algunos registros de la película a la tabla. Haga clic en la tabla de películas en la ventana Explorador de servidores y seleccione la opción de menú **mostrar datos de tabla**. Escriba una lista de las películas favoritos (Véase la figura 5).


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Figura 05**: escribir registros de películas ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Crear el modelo

A continuación, necesitamos crear un conjunto de clases para representar nuestra base de datos. Es necesario crear un modelo de base de datos. Se podrá aprovechar las ventajas de Microsoft Entity Framework para generar las clases de nuestro modelo de base de datos automáticamente.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está vinculado a Microsoft Entity Framework. Puede crear la base de datos modelo clases aprovechando las ventajas de una serie de asignación relacional de objeto (o / M) además de LINQ to SQL, Subsonic y NHibernate de herramientas.


Siga estos pasos para iniciar al Asistente para Entity Data Model:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione la **ADO.NET Entity Data Model** plantilla.
3. Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.

Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 6). Siga estos pasos para completar al asistente:

1. En el **Elegir contenido del modelo** paso, seleccione la **generar desde la base de datos** opción.
2. En el **elegir la conexión de datos** paso a paso, use la *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el **siguiente** botón.
3. En el **elija los objetos de base de datos** paso a paso, expanda el nodo tablas, seleccione la tabla de películas. Escriba el espacio de nombres *MovieApp.Models* y haga clic en el **finalizar** botón.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Figura 06**: generar un modelo de base de datos con el Asistente para Entity Data Model ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer. El diseñador debe mostrar la tabla de base de datos de películas (consulte la figura 7).


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Figura 07**: el Entity Data Model Designer ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


Es preciso realizar un cambio antes de continuar. El Asistente para Entity Data genera una clase de modelo denominada películas que representa la tabla de base de datos de películas. Dado que vamos a usar la clase de películas para representar una película determinada, es necesario modificar el nombre de la clase es *película* en lugar de *películas* (singular en lugar de plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambiar el nombre de la clase de películas a película. Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.

## <a name="creating-the-aspnet-mvc-controller"></a>Crear el controlador de MVC de ASP.NET

El siguiente paso es crear el controlador de MVC de ASP.NET. Un controlador es responsable de controlar la forma en que un usuario interactúa con una aplicación ASP.NET MVC.

Siga estos pasos:

1. En la ventana Explorador de soluciones, haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, controlador**.
2. En el cuadro de diálogo Agregar controlador, escriba el nombre *HomeController* y Active la casilla etiquetada **agregar métodos de acción para los escenarios de creación, actualización y detalles** (consulte la figura 8).
3. Haga clic en el **agregar** botón para agregar el nuevo controlador a su proyecto.

Después de completar estos pasos, se crea el controlador en la lista 1. Tenga en cuenta que contiene métodos con el nombre de índice, obtener información detallada, crear y editar. En las secciones siguientes, se agregará el código necesario para obtener estos métodos funcionen.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Figura 08**: agregar un nuevo controlador de MVC de ASP.NET ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Registros de base de datos de lista

El método Index() del controlador Home es el método predeterminado para una aplicación ASP.NET MVC. Cuando se ejecuta una aplicación de ASP.NET MVC, el método Index() es el primer método de controlador que se llama.

Vamos a usar el método Index() para mostrar la lista de registros de la tabla de base de datos de películas. Se podrá sacar partido de la base de datos las clases de modelo que hemos creado con anterioridad para recuperar los registros de base de datos de la película con el método Index().

Modifiqué la clase HomeController en el listado 2 para que contenga un nuevo campo privado denominado \_base de datos. La clase MoviesDBEntities representa nuestro modelo de base de datos y vamos a usar esta clase para comunicarse con nuestra base de datos.

También modifiqué el método Index() en el listado 2. El método Index() utiliza la clase MoviesDBEntities para recuperar todos los registros de la película de la tabla de base de datos de películas. La expresión  *\_base de datos. MovieSet.ToList()* devuelve una lista de todos los registros de la película de la tabla de base de datos de películas.

La lista de películas se pasa a la vista. Todo lo que se pasa al método View() se pasa a la vista como datos de la vista.

**La lista 2 – Controllers/HomeController.vb (método de índice modificado)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

El método Index() devuelve una vista con el nombre de índice. Es necesario crear esta vista para ver una lista de entradas de base de datos de la película. Siga estos pasos:


Debe compilar el proyecto (seleccione la opción de menú **compilar, compilar solución**) antes de abrir el **agregar vista** cuadro de diálogo o ninguna clase aparecerá en el **Ver clase de datos** lista desplegable.


1. Haga clic en el método Index() en el editor de código y seleccione la opción de menú **agregar vista** (consulte la figura 9).
2. En el cuadro de diálogo Agregar vista, compruebe que la casilla de verificación con la etiqueta **crear una vista fuertemente tipada** está activada.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *lista*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Movie*.
5. Haga clic en el botón Agregar para crear una nueva vista (consulte la figura 10).

Después de completar estos pasos, se agrega una nueva vista denominada Index.aspx a la carpeta Views\Home. El contenido de la vista de índice se incluye en la lista 3.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Figura 09**: agregar una vista de una acción de controlador ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Figura 10**: crear una nueva vista con el cuadro de diálogo Agregar vista ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

La vista de índice muestra todos los registros de la película de la tabla de base de datos de películas dentro de una tabla HTML. La vista contiene una para cada bucle que recorre en iteración cada película representada por la propiedad ViewData.Model. Si ejecuta la aplicación pulsando la tecla F5, verá la página web en la figura 11.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Figura 11**: vista del índice ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Creación de nuevos registros de base de datos

La vista de índice que hemos creado en la sección anterior incluye un vínculo para la creación de nuevos registros de base de datos. Sigamos adelante e implementan la lógica y crear la vista necesaria para la creación de nuevos registros de base de datos de la película.

El controlador Home contiene dos métodos denominados Create(). El primer método Create() no tiene parámetros. Esta sobrecarga del método Create() se utiliza para mostrar el formulario HTML para crear un nuevo registro de base de datos de la película.

El segundo método Create() tiene un parámetro de ColecciónFormulario. Esta sobrecarga del método Create() se llama cuando se envía el formulario HTML para crear una película nuevo al servidor. Observe que este segundo método Create() tiene un atributo AcceptVerbs que impide que el método que se llama a menos que se realiza una operación HTTP Post.

Este segundo método Create() se ha modificado en la clase HomeController actualizada en el listado 4. La nueva versión del método Create() acepta un parámetro de película y contiene la lógica para insertar una película nueva en la tabla de base de datos de películas.

> [!NOTE] 
> 
> Observe que el atributo de enlace. Dado que no desea actualizar la propiedad de Id. de la película de formulario HTML, es necesario excluir explícitamente esta propiedad.


**Listado 4 – Controllers\HomeController.vb (método de crear modificado)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio resulta muy sencillo crear el formulario para crear una nueva base de datos de la película grabar (consulte la figura 12). Siga estos pasos:

1. Haga clic en el método Create() en el editor de código y seleccione la opción de menú **agregar vista**.
2. Compruebe que la casilla de verificación con la etiqueta **crear una vista fuertemente tipada** está activada.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *crear*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Movie*.
5. Haga clic en el **agregar** botón para crear la nueva vista.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Figura 12**: agregar la vista de creación ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio genera la vista en el listado 5 automáticamente. Esta vista contiene un formulario HTML que incluye los campos que corresponden a cada una de las propiedades de la clase de la película.

**Listado 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> El formulario HTML generado por el cuadro de diálogo Agregar vista genera un campo de formulario de Id. Dado que la columna Id. es una columna de identidad, no necesitamos este campo de formulario y puede quitar de forma segura.


Después de agregar la vista de creación, puede agregar nuevos registros de película a la base de datos. Ejecute la aplicación presionando la tecla F5 y haga clic en el vínculo Crear nuevo para ver el formulario en la figura 13. Si completa y enviar el formulario, se crea un nuevo registro de base de datos de la película.

Tenga en cuenta que se obtiene automáticamente validación del formulario. Si no especifica una fecha de lanzamiento para una película, o escriba una fecha de lanzamiento no válido, se vuelve a mostrar el formulario y se resalta el campo de fecha de lanzamiento.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Figura 13**: crear un nuevo registro de base de datos de la película ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Editar registros de base de datos existente

En las secciones anteriores, se explicó cómo puede enumerar y crear nuevos registros de base de datos. En esta sección final, se describe cómo puede modificar los registros de base de datos existentes.

En primer lugar, es necesario generar el formulario de edición. Este paso es fácil, ya que Visual Studio generará el formulario de edición para que podamos automáticamente. Abra la clase HomeController.vb en el editor de código de Visual Studio y siga estos pasos:

1. Haga clic en el método Edit() en el editor de código y seleccione la opción de menú **agregar vista** (Véase la figura 14).
2. Active la casilla etiquetada **crear una vista fuertemente tipada**.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *editar*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Movie*.
5. Haga clic en el **agregar** botón para crear la nueva vista.

Completar estos pasos, agrega una nueva vista denominada Edit.aspx a la carpeta Views\Home. Esta vista contiene un formulario HTML para editar un registro de la película.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Figura 14**: agregar la vista de edición ([haga clic aquí para ver la imagen a tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> La vista de edición contiene un campo de formulario HTML que se corresponde con la propiedad de Id. de la película. Dado que no desea editar el valor de la propiedad Id de personas, debe quitar este campo de formulario.


Por último, es necesario modificar el controlador Home para que admita la edición de un registro de base de datos. La clase HomeController actualizada se encuentra en el listado 6.

**Enumerar 6: Controllers\HomeController.vb (métodos de edición)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

En el listado 6, he agregado lógica adicional para ambas sobrecargas del método Edit(). El primer método Edit() devuelve el registro de base de datos de la película que corresponde al parámetro de identificador pasado al método. La segunda sobrecarga realiza las actualizaciones a un registro de la película en la base de datos.

Tenga en cuenta que debe recuperar la película original y, a continuación, llame a ApplyPropertyChanges() para actualizar la película existente en la base de datos.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era para proporcionarle una idea de la experiencia de creación de una aplicación ASP.NET MVC. Espero que detecta que generar un ASP.NET MVC de aplicación web es muy similar a la experiencia de creación de una aplicación ASP.NET o de páginas Active Server.

En este tutorial, se examinan solo las características más básicas del marco de MVC de ASP.NET. En tutoriales futuros, hemos profundizar más en temas tales como controladores, las acciones de controlador, vistas, ver datos y aplicaciones auxiliares HTML.

>[!div class="step-by-step"]
[Anterior](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
