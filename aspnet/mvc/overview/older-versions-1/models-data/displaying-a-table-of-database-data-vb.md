---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Muestra una tabla de base de datos (VB) | Documentos de Microsoft
author: microsoft
description: En este tutorial, muestran dos métodos de presentación de un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-vb"></a>Muestra una tabla de base de datos (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> En este tutorial, muestran dos métodos de presentación de un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML. En primer lugar, muestra cómo dar formato a los registros de base de datos directamente dentro de una vista. A continuación, muestra cómo puede aprovechar las ventajas de parciales al dar formato a los registros de base de datos.


El objetivo de este tutorial es explicar cómo se puede mostrar una tabla HTML de la base de datos en una aplicación ASP.NET MVC. En primer lugar, se muestra cómo utilizar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestra automáticamente un conjunto de registros. A continuación, se muestra cómo utilizar un parcial como una plantilla al dar formato a los registros de base de datos.

## <a name="create-the-model-classes"></a>Crear las clases del modelo

Vamos a mostrar el conjunto de registros de la tabla de base de datos de películas. La tabla de base de datos de películas contiene las columnas siguientes:

<a id="0.4_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Title | Nvarchar(200) | False |
| Director de | NVarchar(50) | False |
| DateReleased | DateTime | False |


Para representar la tabla de películas en nuestra aplicación de ASP.NET MVC, necesitamos crear una clase de modelo. En este tutorial, se utiliza el marco de la entidad de Microsoft para crear las clases de modelo.

> [!NOTE] 
> 
> En este tutorial, se utiliza el marco de la entidad de Microsoft. Sin embargo, es importante entender que puede usar varias tecnologías diferentes para interactuar con una base de datos desde una aplicación de ASP.NET MVC además de LINQ to SQL, NHibernate o ADO.NET.


Siga estos pasos para iniciar al Asistente para Entity Data Model:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione la **ADO.NET Entity Data Model** plantilla.
3. Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.

Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 1). Siga estos pasos para completar al asistente:

1. En el **Elegir contenido del modelo** paso, seleccione la **generar desde la base de datos** opción.
2. En el **elegir la conexión de datos** paso a paso, use la *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el **siguiente** botón.
3. En el **elija los objetos de base de datos** paso a paso, expanda el nodo tablas, seleccione la tabla de películas. Escriba el espacio de nombres *modelos* y haga clic en el **finalizar** botón.


[![Creación de LINQ a las clases SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figura 01**: crear clases de LINQ to SQL ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image2.png))


Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer. El diseñador debe mostrar la entidad de películas (consulte la figura 2).


[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figura 02**: el Entity Data Model Designer ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image4.png))


Es preciso realizar un cambio antes de continuar. El Asistente para Entity Data genera una clase de modelo denominada *películas* que representa la tabla de base de datos de películas. Dado que vamos a usar la clase de películas para representar una película determinada, es necesario modificar el nombre de la clase es *película* en lugar de *películas* (singular en lugar de plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambiar el nombre de la clase de películas a película. Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.

## <a name="create-the-movies-controller"></a>Crear el controlador de películas

Ahora que tenemos una manera de representar nuestros registros de base de datos, podemos crear un controlador que devuelve la colección de películas. Dentro de la ventana Explorador de soluciones de Visual Studio, haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, controlador** (consulte la figura 3).


[![El controlador menú Agregar](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figura 03**: controlador de agregar el menú ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image6.png))


Cuando el **Agregar controlador** aparece el cuadro de diálogo, escriba el nombre del controlador MovieController (consulte la figura 4). Haga clic en el **agregar** botón para agregar el nuevo controlador.


[![El cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figura 04**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image8.png))


Es necesario modificar la acción de Index() expuesta por el controlador de película para que devuelva el conjunto de registros de base de datos. Modifique el controlador de modo que parece que el controlador en la lista 1.

**Lista 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

En la lista 1, la clase MoviesDBEntities se usa para representar la base de datos de MoviesDB. La expresión *entidades. MovieSet.ToList()* devuelve el conjunto de todas las películas de la tabla de base de datos de películas.

## <a name="create-the-view"></a>Crear la vista

Es la manera más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML aprovechar el scaffolding proporcionado por Visual Studio.

Compilar la aplicación seleccionando la opción de menú **compilar, compilar solución**. Debe compilar la aplicación antes de abrir el **agregar vista** cuadro de diálogo o las clases de datos no aparecerán en el cuadro de diálogo.

Haga clic en la acción de Index() y seleccione la opción de menú **agregar vista** (Véase la figura 5).


[![Agregar una vista](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figura 05**: agregar una vista ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image10.png))


En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**. Seleccione la clase de película como el **Ver clase de datos**. Seleccione *lista* como el **ver contenido** (consulte la figura 6). Al seleccionar estas opciones, se generará una vista fuertemente tipada que muestra una lista de películas.


[![El cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figura 06**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image12.png))


Tras hacer clic en el **agregar** botón, la vista en el listado 2 se genera automáticamente. Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrará cada una de las propiedades de una película.

**La lista 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Puede ejecutar la aplicación seleccionando la opción de menú **depurar, Iniciar depuración** (o presionar la tecla F5). La aplicación se ejecuta, inicia Internet Explorer. Si navega a la dirección URL /Movie, a continuación, verá la página en la figura 7.


[![Una tabla de películas](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figura 07**: una tabla de películas ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-vb/_static/image14.png))


Si no le gusta a nada sobre el aspecto de la cuadrícula de registros de base de datos en la figura 7, a continuación, simplemente puede modificar la vista de índice. Por ejemplo, puede cambiar la *DateReleased* encabezado a *fecha de lanzamiento* mediante la modificación de la vista de índice.

## <a name="create-a-template-with-a-partial"></a>Crear una plantilla con una confianza parcial

Cuando una vista será demasiado complicada, resulta una buena idea empezar interrumpa la vista parciales. Usar parciales hace que las vistas fáciles de entender y mantener. Vamos a crear un parcial que podemos usar como plantilla para dar formato a cada uno de los registros de base de datos de la película.

Siga estos pasos para crear la parcial:

1. Haga clic en la carpeta Views\Movie y seleccione la opción de menú **agregar vista**.
2. Active la casilla etiquetada *crear una vista parcial (.ascx)*.
3. Nombre de parte *MovieTemplate*.
4. Active la casilla etiquetada **crear una vista fuertemente tipada**.
5. Seleccione la película como el *Ver clase de datos*.
6. Seleccione vacío como el *ver contenido*.
7. Haga clic en el **agregar** botón para agregar la parcial al proyecto.

Después de completar estos pasos, modifique la MovieTemplate parcial que se parezca a listado 3.

**El listado 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Parte en el listado 3 contiene una plantilla para una sola fila de registros.

La vista de índice modificada en el listado 4 utiliza la MovieTemplate parcial.

**Listado 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

La vista en el listado 4 contiene For cada bucle que recorre en iteración todas las películas. Para cada película, el MovieTemplate parcial se usa para dar formato a la película. El MovieTemplate se representa mediante una llamada al método de aplicación auxiliar RenderPartial().

La vista de índice modificada representa la tabla HTML muy misma de registros de base de datos. Sin embargo, la vista se ha simplificado en gran medida.


El método RenderPartial() es diferente de la mayoría de los otros métodos de aplicación auxiliar porque no devuelve una cadena. Por lo tanto, debe llamar el método RenderPartial() mediante &lt;Html.RenderPartial() %&gt; en lugar de &lt;% = Html.RenderPartial() %&gt;.


## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo se puede mostrar un conjunto de registros de base de datos en una tabla HTML. En primer lugar, aprendió cómo devolver un conjunto de registros de base de datos de una acción de controlador aprovechando las ventajas de Microsoft Entity Framework. A continuación, aprendió a utilizar el scaffolding de Visual Studio para generar una vista que muestra automáticamente una colección de elementos. Por último, aprendió a simplificar la vista aprovechando las ventajas de una parcial. Aprendió a utilizar un parcial como una plantilla, por lo que puede dar formato a cada registro de base de datos.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-vb.md)
> [Siguiente](performing-simple-validation-vb.md)
