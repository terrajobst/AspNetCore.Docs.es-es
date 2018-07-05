---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Crear una aplicación de base de datos de la película en 15 minutos con ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther crea una completa controlado por base de datos ASP.NET MVC aplicación desde el principio para finalizar. Este tutorial es una excelente introducción para quienes t nuevo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf41eb0622d032578a77dd2d3f7427b244b2e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395872"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Crear una aplicación de base de datos de la película en 15 minutos con ASP.NET MVC (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descargue el código](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther crea una completa controlado por base de datos ASP.NET MVC aplicación desde el principio para finalizar. Este tutorial es una introducción excelente para las personas que está familiarizado con el marco de MVC de ASP.NET y que quieren tener una idea del proceso de creación de una aplicación ASP.NET MVC.


El propósito de este tutorial es proporcionarle una idea de "lo que es similar a" para crear una aplicación ASP.NET MVC. En este tutorial, insultar a través de la creación de una aplicación ASP.NET MVC toda de principio a fin. Mostrarle cómo crear una sencilla aplicación controlada por base de datos que se muestra cómo enumerar, crear y editar registros de base de datos.

Para simplificar el proceso de creación de nuestra aplicación, se podrán aprovechar las ventajas de las características de scaffolding de Visual Studio 2008. Vamos a Visual Studio genere el código inicial y el contenido para nuestro controladores, modelos y vistas.

Si ha trabajado con ASP.NET o de páginas Active Server, a continuación, encontrará ASP.NET MVC muy familiar. Las vistas de MVC de ASP.NET son muy similares a las páginas en una aplicación de páginas Active Server. Además, al igual que una aplicación de formularios Web Forms de ASP.NET tradicional, ASP.NET MVC proporciona con acceso completo al conjunto enriquecido de lenguajes y las clases proporcionadas por .NET framework.

Mi esperanza es que este tutorial le dará una idea de cómo la experiencia de crear una aplicación ASP.NET MVC es diferente de la experiencia de crear una aplicación de formularios Web Forms de ASP.NET o de páginas Active Server y similares.

## <a name="overview-of-the-movie-database-application"></a>Información general de la aplicación de base de datos de película

Dado que nuestro objetivo es simplificar las cosas, vamos a crear una aplicación de base de datos de película muy sencilla. Nuestra aplicación de base de datos de película simple nos permitirá hacer tres cosas:

1. Enumera un conjunto de registros de base de datos de película
2. Cree un nuevo registro de base de datos de película
3. Editar un registro de base de datos existente de película

De nuevo, porque queremos simplificar las cosas, se podrá aprovechar el número mínimo de características de ASP.NET MVC framework necesarios para compilar la aplicación. Por ejemplo, se no se aprovecha la ventaja de desarrollo controlado por pruebas.

Para crear nuestra aplicación, es necesario completar cada uno de los pasos siguientes:

1. Crear el proyecto de aplicación Web de ASP.NET MVC
2. Creación de la base de datos
3. Crear el modelo de base de datos
4. Crear el controlador de MVC de ASP.NET
5. Crear vistas de ASP.NET MVC

## <a name="preliminaries"></a>Pasos preliminares

Necesitará Visual Studio 2008 o Visual Web Developer 2008 Express para compilar una aplicación ASP.NET MVC. También deberá descargar el marco de MVC de ASP.NET.

Si no dispone de Visual Studio 2008, puede descargar una versión de prueba de 90 días de Visual Studio 2008 desde este sitio Web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Como alternativa, puede crear ASP.NET MVC aplicaciones con Visual Web Developer Express 2008. Si opta por usar Visual Web Developer Express, a continuación, debe tener instalado el Service Pack 1. Puede descargar Visual Web Developer 2008 Express con Service Pack 1 de este sitio Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Después de instalar Visual Studio 2008 o Visual Web Developer 2008, deberá instalar el marco de MVC de ASP.NET. Puede descargar el marco de MVC de ASP.NET desde el sitio Web siguiente:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> En lugar de descargar el marco de ASP.NET y ASP.NET MVC framework individualmente, puede aprovechar el instalador de plataforma Web. El instalador de plataforma Web es una aplicación que le permite administrar fácilmente las aplicaciones instaladas son el equipo:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Crear un proyecto de aplicación Web de ASP.NET MVC

Comencemos creando un nuevo proyecto de aplicación Web ASP.NET MVC en Visual Studio 2008. Seleccione la opción de menú **archivo, nuevo proyecto** y verá el cuadro de diálogo nuevo proyecto en la figura 1. Seleccione C# como lenguaje de programación y seleccione la plantilla de proyecto de aplicación Web ASP.NET MVC. Asigne al proyecto el nombre MovieApp y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: cuadro de diálogo nuevo proyecto ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Asegúrese de que seleccione .NET Framework 3.5 en la lista desplegable en la parte superior del cuadro de diálogo nuevo proyecto o la plantilla de proyecto de aplicación Web ASP.NET MVC no aparecerá.


Siempre que cree un nuevo proyecto de aplicación Web de MVC, Visual Studio le pedirá que cree un proyecto de prueba de unidad independiente. Aparece el cuadro de diálogo en la figura 2. Dado que no se puede crear pruebas en este tutorial debido a restricciones de tiempo (y Sí, debemos sentirnos culpables un poco acerca de esto) seleccione la **No** opción y haga clic en el **Aceptar** botón.

> [!NOTE] 
> 
> Visual Web Developer no admite proyectos de prueba.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo de la creación de proyecto de prueba de unidad ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Una aplicación de MVC de ASP.NET tiene un conjunto de carpetas estándar: una carpeta de modelos, vistas y controladores. Puede ver este conjunto estándar de carpetas en la ventana Explorador de soluciones. Necesitamos agregar archivos a cada una de las carpetas de modelos, vistas y controladores para compilar la aplicación de base de datos de película.

Cuando se crea una nueva aplicación MVC con Visual Studio, obtendrá una aplicación de ejemplo. Porque queremos empezar desde cero, se debe eliminar el contenido de esta aplicación de ejemplo. Es preciso eliminar el archivo siguiente y la siguiente carpeta:

- Controllers\homecontroller. cs
- Views\Home

## <a name="creating-the-database"></a>Creación de la base de datos

Es necesario crear una base de datos para almacenar nuestras entradas de base de datos de la película. Afortunadamente, Visual Studio incluye una base de datos gratuita denominada SQL Server Express. Siga estos pasos para crear la base de datos:

1. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione el **base de datos de SQL Server** plantilla (consulte la figura 3).
3. Nombre de la base de datos *MoviesDB.mdf* y haga clic en el **agregar** botón.

Después de crear la base de datos, puede conectarse a la base de datos haciendo doble clic en el archivo MoviesDB.mdf ubicado en la aplicación\_carpeta de datos. Haga doble clic en el archivo MoviesDB.mdf se abre la ventana del explorador de servidores.

> [!NOTE] 
> 
> La ventana del explorador de servidores se denomina la ventana del explorador de base de datos en el caso de Visual Web Developer.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: creación de una base de datos de Microsoft SQL Server ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


A continuación, necesitamos crear una nueva tabla de base de datos. Desde dentro de la ventana Explorador de servidores, haga clic en la carpeta Tables y seleccione la opción de menú **agregar nueva tabla**. Al seleccionar esta opción de menú, abre el Diseñador de tablas de base de datos. Cree las siguientes columnas de la base de datos:

<a id="0.1_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Title | Nvarchar (100) | False |
| Director | Nvarchar (100) | False |
| DateReleased | DateTime | False |


La primera columna, la columna Id, tiene dos propiedades especiales. En primer lugar, deberá marcar la columna de identificador como columna de clave principal. Después de seleccionar la columna de identificador, haga clic en el **establecer clave principal** botón (es el icono que parece una clave). En segundo lugar, deberá marcar la columna de identificador como una columna de identidad. En la ventana Propiedades de columna, desplácese hacia abajo hasta la sección de la especificación de identidad y expándalo. Cambiar el **identidad** valor para la propiedad **Sí**. Cuando haya terminado, la tabla debería parecerse a la figura 4.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: tabla de base de datos de las películas ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Es el último paso guardar la nueva tabla. Haga clic en el botón Guardar (el icono del disquete) y proporcionar las películas de nombre de la nueva tabla.

Cuando termine de crear la tabla, agregue algunos registros de la película a la tabla. Haga clic en la tabla de películas en la ventana del explorador de servidores y seleccione la opción de menú **mostrar datos de tabla**. Escriba una lista de las películas favoritas (consulte la figura 5).


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: inserción de registros de película ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Creación del modelo

A continuación, se debe crear un conjunto de clases para representar la base de datos. Es necesario crear un modelo de base de datos. Se podrán aprovechar las ventajas de Microsoft Entity Framework para generar las clases para nuestro modelo de base de datos automáticamente.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está asociado a Microsoft Entity Framework. Puede crear la base de datos de las clases de modelo aprovechando las ventajas de una variedad de asignación relacional de objetos (o / M) herramientas de LINQ to SQL, Subsonic y NHibernate.


Siga estos pasos para iniciar al Asistente para Entity Data Model:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione el **ADO.NET Entity Data Model** plantilla.
3. Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.

Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 6). Siga estos pasos para completar al asistente:

1. En el **Elegir contenido de Model** paso, seleccione la **generar desde la base de datos** opción.
2. En el **elegir la conexión de datos** paso, utilice el *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el **siguiente** botón.
3. En el **elija los objetos de base de datos** paso, expanda el nodo tablas, seleccione la tabla de películas. Escriba el espacio de nombres *MovieApp.Models* y haga clic en el **finalizar** botón.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: generar un modelo de base de datos con el Asistente para Entity Data Model ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer. El diseñador debe mostrar la tabla de base de datos de películas (consulte la figura 7).


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: The Entity Data Model Designer ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Es necesario realizar un cambio antes de continuar. El Asistente para Entity Data genera una clase de modelo denominada *películas* que representa la tabla de base de datos de películas. Dado que vamos a usar la clase de películas para representar una película concreta, es necesario modificar el nombre de la clase se *película* en lugar de *películas* (singular lugar plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de películas para película. Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.

## <a name="creating-the-aspnet-mvc-controller"></a>Crear el controlador de MVC de ASP.NET

El siguiente paso es crear el controlador de MVC de ASP.NET. Un controlador es responsable de controlar cómo un usuario interactúa con una aplicación ASP.NET MVC.

Siga estos pasos:

1. En la ventana Explorador de soluciones, haga clic en la carpeta Controllers y seleccione la opción de menú **Add, controlador**.
2. En el cuadro de diálogo Agregar controlador, escriba el nombre *HomeController* y Active la casilla etiquetada **agregar métodos de acción para los escenarios Create, Update y detalles** (consulte la figura 8).
3. Haga clic en el **agregar** para agregar el nuevo controlador al proyecto.

Después de completar estos pasos, se crea el controlador en el listado 1. Tenga en cuenta que contiene métodos con el nombre de índice, detalles, crear y editar. En las secciones siguientes, agregaremos el código necesario para obtener estos métodos para trabajar.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: agregar un nuevo controlador de MVC de ASP.NET ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Listado 1 – controllers\homecontroller. cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Registros de base de datos de lista

El método Index() del controlador principal es el método predeterminado para una aplicación ASP.NET MVC. Al ejecutar una aplicación ASP.NET MVC, el método Index() es el primer método de controlador que se llama.

Vamos a usar el método Index() para mostrar la lista de registros de la tabla de base de datos de películas. Echaremos las ventajas de la base de datos de las clases de modelo que hemos creados anteriormente para recuperar los registros de base de datos de la película con el método Index().

He modificado la clase HomeController en el listado 2 para que contenga un nuevo campo privado denominado \_db. La clase MoviesDBEntities representa nuestro modelo de base de datos y esta clase se usará para comunicarse con nuestra base de datos.

También modifiqué el método Index() en el listado 2. El método Index() utiliza la clase MoviesDBEntities para recuperar todos los registros de la película de la tabla de base de datos de películas. La expresión  *\_db. MovieSet.ToList()* devuelve una lista de todos los registros de la película de la tabla de base de datos de películas.

La lista de películas se pasa a la vista. Todo lo que se pasa al método View() se pasa a la vista como datos de la vista.

**Listado 2 – Controllers/HomeController.cs (método de índice modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

El método Index() devuelve una vista llamada Index. Es necesario crear esta vista para mostrar la lista de registros de base de datos de películas. Siga estos pasos:


Debe compilar el proyecto (seleccione la opción de menú **crear, compilar solución**) antes de abrir el **agregar vista** cuadro de diálogo o clases no aparecerá en el **Ver clase de datos** lista desplegable.


1. Haga clic en el método Index() en el editor de código y seleccione la opción de menú **agregar vista** (consulte la figura 9).
2. En el cuadro de diálogo Agregar vista, compruebe que la casilla de verificación con la etiqueta **crear una vista fuertemente tipada** está activada.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *lista*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Models.Movie*.
5. Haga clic en el botón Agregar para crear la nueva vista (consulte la figura 10).

Después de completar estos pasos, se agrega una nueva vista denominada Index.aspx a la carpeta Views\Home. El contenido de la vista de índice se incluye en el listado 3.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: adición de una vista desde una acción de controlador ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: crear una nueva vista con el cuadro de diálogo Agregar vista ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**Listado 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

La vista de índice muestra todos los registros de la película de la tabla de base de datos de películas en una tabla HTML. La vista contiene un bucle foreach que itera por cada película representado por la propiedad ViewData.Model. Si ejecuta la aplicación presionando la tecla F5, verá la página web en la figura 11.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: vista de índice de la ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Creación de nuevos registros de base de datos

La vista de índice que creamos en la sección anterior incluye un vínculo para la creación de nuevos registros de base de datos. Sigamos adelante e implementan la lógica y crear la vista necesarios para crear nuevos registros de base de datos de la película.

El controlador Home contiene dos métodos denominados Create(). El primer método Create() no tiene parámetros. Esta sobrecarga del método Create() se utiliza para mostrar el formulario HTML para crear un nuevo registro de base de datos de la película.

El segundo método Create() tiene un parámetro de ColecciónFormulario. Esta sobrecarga del método Create() se llama cuando se publica el formulario HTML para crear una película nueva en el servidor. Tenga en cuenta que este segundo método Create() tiene un atributo AcceptVerbs que impide que el método que se llama a menos que se realiza una operación HTTP POST.

Este segundo método Create() se ha modificado en la clase HomeController actualizada en el listado 4. La nueva versión del método Create() acepta un parámetro de la película y contiene la lógica para insertar una película nueva en la tabla de base de datos de películas.

> [!NOTE] 
> 
> Tenga en cuenta el atributo Bind. Dado que no desea actualizar la propiedad de Id. de película de formulario HTML, es necesario excluir explícitamente de esta propiedad.


**Listado 4 – controllers\homecontroller. cs (método Create modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio resulta muy sencillo crear el formulario para crear una nueva base de datos de película grabar (consulte la figura 12). Siga estos pasos:

1. Haga clic en el método Create() en el editor de código y seleccione la opción de menú **agregar vista**.
2. Compruebe que la casilla de verificación con la etiqueta **crear una vista fuertemente tipada** está activada.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *crear*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Models.Movie*.
5. Haga clic en el **agregar** botón para crear la nueva vista.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: adición de la vista de creación ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio genera automáticamente la vista en el listado 5. Esta vista contiene un formulario HTML que incluye los campos que corresponden a cada una de las propiedades de la clase de la película.

**Listado 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> El formulario HTML generado por el cuadro de diálogo Agregar vista genera un campo de formulario de Id. Dado que la columna de identificador es una columna de identidad, no necesitamos este campo de formulario y se puede eliminar sin riesgos.


Después de agregar la vista de creación, puede agregar nuevos registros de película a la base de datos. Ejecute la aplicación presionando la tecla F5 y haga clic en el vínculo Crear nuevo para ver el formulario en la figura 13. Si se complete y envíe el formulario, se crea un nuevo registro de base de datos de la película.

Tenga en cuenta que se obtiene automáticamente una validación de formulario. Si no especifica una fecha de lanzamiento para una película, o escriba una fecha de lanzamiento no válido, se vuelve a mostrar el formulario y se resalta el campo de fecha de lanzamiento.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: crear un nuevo registro de base de datos de películas ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Editar los registros de base de datos existentes

En las secciones anteriores, analizamos cómo puede enumerar y crear nuevos registros de base de datos. En esta última sección, trataremos cómo puede editar los registros de base de datos existentes.

En primer lugar, tenemos que generar el formulario de edición. Este paso es fácil, ya que Visual Studio generará el formulario de edición para que podamos automáticamente. Abra la clase HomeController.cs en el editor de código de Visual Studio y siga estos pasos:

1. Haga clic en el método Edit() en el editor de código y seleccione la opción de menú **agregar vista** (vea la figura 14).
2. Active la casilla etiquetada **crear una vista fuertemente tipada**.
3. Desde el **ver contenido** lista desplegable, seleccione el valor *editar*.
4. Desde el **Ver clase de datos** lista desplegable, seleccione el valor *MovieApp.Models.Movie*.
5. Haga clic en el **agregar** botón para crear la nueva vista.

Completar estos pasos, agrega una nueva vista denominada Edit.aspx a la carpeta Views\Home. Esta vista contiene un formulario HTML para editar un registro de la película.


[![El cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: adición de la vista de edición ([haga clic aquí para ver imagen en tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> La vista de edición contiene un campo de formulario HTML que se corresponde con la propiedad de Id. de película. Dado que no desea que las personas que editar el valor de la propiedad Id, debe quitar este campo de formulario.


Por último, se debe modificar el controlador Home para que admita la edición de un registro de base de datos. La clase HomeController actualizada se encuentra en el listado 6.

**Listado 6 – controllers\homecontroller. cs (métodos)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

En el listado 6, he agregado lógica adicional a las dos sobrecargas del método Edit(). El primer método Edit() devuelve el registro de base de datos de película que corresponde al parámetro de identificador pasado al método. La segunda sobrecarga realiza las actualizaciones a un registro de la película en la base de datos.

Tenga en cuenta que debe recuperar la película original y, a continuación, llame a ApplyPropertyChanges() para actualizar la película en la base de datos existente.

## <a name="summary"></a>Resumen

El propósito de este tutorial era para hacerse una idea de la experiencia de creación de una aplicación ASP.NET MVC. Espero que detecta que la creación de aplicación web ASP.NET MVC es muy similar a la experiencia de crear una aplicación ASP.NET o de páginas Active Server.

En este tutorial, hemos visto solo las características más básicas de ASP.NET MVC framework. En próximos tutoriales, profundice en temas tales como controladores, las acciones de controlador, vistas, ver los datos y aplicaciones auxiliares HTML.

> [!div class="step-by-step"]
> [Siguiente](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
