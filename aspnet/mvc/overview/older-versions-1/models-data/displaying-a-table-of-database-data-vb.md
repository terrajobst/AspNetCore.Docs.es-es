---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Mostrar una tabla de base de datos (VB) | Microsoft Docs
author: microsoft
description: En este tutorial, muestran dos métodos para mostrar un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f5d9062d901f28b1d64f400e13ed5eb0ca788ab8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362497"
---
<a name="displaying-a-table-of-database-data-vb"></a>Mostrar una tabla de base de datos (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> En este tutorial, muestran dos métodos para mostrar un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML. En primer lugar, voy a mostrar cómo dar formato a los registros de base de datos directamente dentro de una vista. A continuación, demuestro cómo puede sacar partido de parciales al dar formato a los registros de la base de datos.


El objetivo de este tutorial es explicar cómo se puede mostrar una tabla HTML de la base de datos en una aplicación ASP.NET MVC. En primer lugar, obtenga información sobre cómo usar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestra automáticamente un conjunto de registros. A continuación, obtenga información sobre cómo usar una parcial como una plantilla al dar formato a los registros de la base de datos.

## <a name="create-the-model-classes"></a>Crear las clases del modelo

Vamos a mostrar el conjunto de registros de la tabla de base de datos de películas. La tabla de base de datos de películas contiene las columnas siguientes:

<a id="0.4_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Title | Nvarchar (200) | False |
| Director | Nvarchar (50) | False |
| DateReleased | DateTime | False |


Para representar la tabla de películas en nuestra aplicación de ASP.NET MVC, necesitamos crear una clase de modelo. En este tutorial, usamos Microsoft Entity Framework para crear nuestras clases de modelo.

> [!NOTE] 
> 
> En este tutorial, usamos Microsoft Entity Framework. Sin embargo, es importante entender que puede usar una variedad de tecnologías diferentes para interactuar con una base de datos desde una aplicación ASP.NET MVC, además de LINQ to SQL, NHibernate o ADO.NET.


Siga estos pasos para iniciar al Asistente para Entity Data Model:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. Seleccione el **datos** categoría y seleccione el **ADO.NET Entity Data Model** plantilla.
3. Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.

Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 1). Siga estos pasos para completar al asistente:

1. En el **Elegir contenido de Model** paso, seleccione la **generar desde la base de datos** opción.
2. En el **elegir la conexión de datos** paso, utilice el *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el **siguiente** botón.
3. En el **elija los objetos de base de datos** paso, expanda el nodo tablas, seleccione la tabla de películas. Escriba el espacio de nombres *modelos* y haga clic en el **finalizar** botón.


[![Creación de LINQ a las clases SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figura 01**: crear clases de LINQ to SQL ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image2.png))


Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer. El diseñador debe mostrar la entidad de películas (consulte la figura 2).


[![El Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figura 02**: The Entity Data Model Designer ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image4.png))


Es necesario realizar un cambio antes de continuar. El Asistente para Entity Data genera una clase de modelo denominada *películas* que representa la tabla de base de datos de películas. Dado que vamos a usar la clase de películas para representar una película concreta, es necesario modificar el nombre de la clase se *película* en lugar de *películas* (singular lugar plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de películas para película. Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.

## <a name="create-the-movies-controller"></a>Crear el controlador de películas

Ahora que tenemos una forma de representar nuestros registros de base de datos, podemos crear un controlador que devuelve la colección de películas. En la ventana Explorador de soluciones de Visual Studio, haga clic en la carpeta Controllers y seleccione la opción de menú **Add, controlador** (consulte la figura 3).


[![El controlador menú Agregar](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figura 03**: The Agregar controlador de menú ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image6.png))


Cuando el **Agregar controlador** aparece el cuadro de diálogo, escriba el nombre del controlador MovieController (consulte la figura 4). Haga clic en el **agregar** para agregar el nuevo controlador.


[![El cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figura 04**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image8.png))


Es necesario modificar la acción de Index() expuesta por el controlador de película para que devuelva el conjunto de registros de base de datos. Modificar el controlador de modo que quede como el controlador en el listado 1.

**Listado 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

En el listado 1, la clase MoviesDBEntities se usa para representar la base de datos MoviesDB. La expresión *entidades. MovieSet.ToList()* devuelve el conjunto de todas las películas de la tabla de base de datos de películas.

## <a name="create-the-view"></a>Crear la vista

Es la manera más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML aprovechar el scaffolding proporcionado por Visual Studio.

Compile la aplicación seleccionando la opción de menú **crear, compilar solución**. Debe compilar la aplicación antes de abrir el **agregar vista** no aparecerán el cuadro de diálogo o las clases de datos en el cuadro de diálogo.

Haga clic en la acción de Index() y seleccione la opción de menú **agregar vista** (consulte la figura 5).


[![Agregar una vista](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figura 05**: adición de una vista ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image10.png))


En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**. Seleccione la clase Movie como el **Ver clase de datos**. Seleccione *lista* como el **ver contenido** (consulte la figura 6). Al seleccionar estas opciones, se generará una vista fuertemente tipada que muestra una lista de películas.


[![El cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figura 06**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image12.png))


Tras hacer clic en el **agregar** botón, la vista en el listado 2 se genera automáticamente. Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrar las propiedades de una película.

**Listado 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Puede ejecutar la aplicación seleccionando la opción de menú **depurar, Iniciar depuración** (o presionar la tecla F5). Se ejecuta la aplicación, inicia Internet Explorer. Si navega a la dirección URL de /Movie verá la página en la figura 7.


[![Una tabla de películas](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figura 07**: una tabla de películas ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-vb/_static/image14.png))


Si no le gusta nada acerca de la apariencia de la cuadrícula de registros de base de datos en la figura 7, a continuación, simplemente puede modificar la vista de índice. Por ejemplo, puede cambiar el *DateReleased* encabezado a *fecha de lanzamiento* mediante la modificación de la vista de índice.

## <a name="create-a-template-with-a-partial"></a>Crear una plantilla con una confianza parcial

Cuando una vista se complica demasiado, es una buena idea comenzar dividir la vista en parciales. Uso parciales facilita las vistas de entender y mantener. Vamos a crear una parcial que podemos usar como plantilla para dar formato a cada uno de los registros de base de datos de la película.

Siga estos pasos para crear la parcial:

1. Haga clic en la carpeta Views\Movie y seleccione la opción de menú **agregar vista**.
2. Active la casilla etiquetada *crear una vista parcial (.ascx)*.
3. Nombre de la parte *MovieTemplate*.
4. Active la casilla etiquetada **crear una vista fuertemente tipada**.
5. Seleccione la película como la *Ver clase de datos*.
6. Seleccione vacía como el *ver contenido*.
7. Haga clic en el **agregar** para agregar la parcial al proyecto.

Después de completar estos pasos, modifique el MovieTemplate parcial sea similar a la lista 3.

**Listado 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

La parcial en el listado 3 contiene una plantilla para una sola fila de registros.

La vista de índice modificada en el listado 4 usa el MovieTemplate parcial.

**Listado 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

La vista en el listado 4 contiene For cada bucle que recorre en iteración todas las películas. Para cada película, el MovieTemplate parcial se utiliza para dar formato a la película. Se representa el MovieTemplate llamando al método auxiliar RenderPartial().

La vista de índice modificada representa la tabla HTML muy misma de los registros de base de datos. Sin embargo, la vista se ha simplificado en gran medida.


El método RenderPartial() es diferente que la mayoría de los otros métodos auxiliares, porque no devuelve una cadena. Por lo tanto, debe llamar el método RenderPartial() mediante &lt;Html.RenderPartial() %&gt; en lugar de &lt;% = % Html.RenderPartial()&gt;.


## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo se puede mostrar un conjunto de registros de base de datos en una tabla HTML. En primer lugar, ha aprendido cómo devolver un conjunto de registros de base de datos de una acción de controlador aprovechando las ventajas de Microsoft Entity Framework. A continuación, ha aprendido a usar scaffolding de Visual Studio para generar una vista que muestra automáticamente una colección de elementos. Por último, ha aprendido cómo simplificar la vista aprovechando las ventajas de una parcial. Ha aprendido a usar una parcial como una plantilla de modo que puede dar formato a cada registro de base de datos.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-vb.md)
> [Siguiente](performing-simple-validation-vb.md)
