---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Con Entity Framework 4.0 y el Control ObjectDataSource, parte 3: ordenar y filtrar | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de la Universidad de Contoso que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887660"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Con Entity Framework 4.0 y el Control ObjectDataSource, parte 3: ordenar y filtrar
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales que se basa en la aplicación web de la Universidad de Contoso que se crea mediante la [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales. Si no has completado los tutoriales anteriores, como punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene alguna pregunta acerca de los tutoriales, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior implementa el modelo de repositorio en una aplicación web de n niveles que utiliza Entity Framework y el `ObjectDataSource` control. Este tutorial muestra cómo realizar la ordenación y filtrado y administrar escenarios de principal-detalle. Podrá agregar las siguientes mejoras para la *Departments.aspx* página:

- Un cuadro de texto para permitir a los usuarios seleccionar departamentos por su nombre.
- Una lista de cursos para cada departamento que se muestra en la cuadrícula.
- La capacidad de ordenar haciendo clic en los encabezados de columna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Agregar la capacidad para ordenar columnas de GridView

Abra la *Departments.aspx* página y agregue un `SortParameterName="sortExpression"` atribuir a la `ObjectDataSource` control denominado `DepartmentsObjectDataSource`. (Más adelante podrá crear un `GetDepartments` método que toma un parámetro denominado `sortExpression`.) El marcado para la etiqueta de apertura del control ahora similar al siguiente ejemplo.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Agregar el `AllowSorting="true"` atributo a la etiqueta de apertura de la `GridView` control. El marcado para la etiqueta de apertura del control ahora similar al siguiente ejemplo.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

En *Departments.aspx.cs*, establecer el criterio de ordenación predeterminado mediante una llamada a la `GridView` del control `Sort` método desde el `Page_Load` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Puede agregar código que ordena o filtra en la clase de la lógica de negocios o la clase de repositorio. Si lo hace en la clase de la lógica de negocios, la ordenación o filtrado de trabajo se hará una vez recuperados los datos de la base de datos, porque la clase de la lógica de negocios está trabajando con un `IEnumerable` objeto devuelto por el repositorio. Si Agregar ordenación y filtrado de código de la clase de repositorio y reiniciarlo antes de una expresión LINQ o consulta de objeto se ha convertido en un `IEnumerable` de objetos, los comandos se pasarán a través de la base de datos para su procesamiento, que normalmente es más eficaz. En este tutorial implementaremos ordenar y filtrar de forma que se produzca el procesamiento se realice la base de datos, es decir, en el repositorio.

Para agregar capacidad de ordenación, debe agregar un nuevo método a la interfaz de repositorio y clases de repositorio, así como para la clase de la lógica de negocios. En el *ISchoolRepository.cs* de archivos, agregue un nuevo `GetDepartments` método que toma un `sortExpression` parámetro que se usarán para ordenar la lista de departamentos que se devuelve:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

El `sortExpression` parámetro especificará la columna para la ordenación y la dirección de ordenación.

Agregue código para el nuevo método a la *SchoolRepository.cs* archivo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Cambiar los existentes sin parámetros `GetDepartments` método para llamar al método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

En el proyecto de prueba, agregue el siguiente método nuevo para *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Si se va a crear cualquier prueba unitaria que dependía de este método devuelve una lista ordenada, necesitaría ordenar la lista antes de devolverlo. Es no crear pruebas probable que en este tutorial, por lo que el método puede devolver simplemente la lista sin ordenar de departamentos.

En el *SchoolBL.cs* , agregue el siguiente método nuevo a la clase de la lógica de negocios:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Este código pasa el parámetro de ordenación para el método de repositorio.

Ejecute el *Departments.aspx* página.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Ahora puede hacer clic en cualquier encabezado de columna para ordenar por dicha columna. Si la columna ya está ordenada, al hacer clic en el encabezado se invierte la dirección de ordenación.

## <a name="adding-a-search-box"></a>Agregar un cuadro de búsqueda

En esta sección podrá agregar un cuadro de texto de búsqueda, vincúlelo a la `ObjectDataSource` controlar mediante un parámetro de control y agregar un método a la clase de lógica de negocios para admitir el filtrado.

Abra la *Departments.aspx* página y agregue el siguiente marcado entre el encabezado y el primer `ObjectDataSource` control:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

En el `ObjectDataSource` control denominado `DepartmentsObjectDataSource`, realice lo siguiente:

- Agregar un `SelectParameters` (elemento) para un parámetro denominado `nameSearchString` que obtiene el valor especificado en el `SearchTextBox` control.
- Cambiar el `SelectMethod` valor al atributo `GetDepartmentsByName`. (Creará este método más adelante.)

El marcado para el `ObjectDataSource` control ahora es similar al siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

En *ISchoolRepository.cs*, agregue un `GetDepartmentsByName` método que toma dos `sortExpression` y `nameSearchString` parámetros:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

En *SchoolRepository.cs*, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Este código usa un `Where` método para seleccionar los elementos que contengan la cadena de búsqueda. Si la cadena de búsqueda está vacía, se seleccionarán todos los registros. Tenga en cuenta que, cuando se especifica el método se llama juntos en una instrucción similar al siguiente (`Include`, a continuación, `OrderBy`, a continuación, `Where`), el `Where` método siempre debe ser el último.

Cambiar existente `GetDepartments` método que toma un `sortExpression` parámetro al llamar al método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

En *MockSchoolRepository.cs* en el proyecto de prueba, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

En *SchoolBL.cs*, agregue el siguiente método nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Ejecute el *Departments.aspx* página e introduzca una cadena de búsqueda para asegurarse de que funciona la lógica de selección. Deje vacío el cuadro de texto y vuelva a intentarlo una búsqueda para asegurarse de que se devuelven todos los registros.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Agregar una columna de detalles para cada fila de cuadrícula

A continuación, desea ver todos los cursos para cada departamento que se muestra en la celda derecha de la cuadrícula. Para ello, deberá usar una anidada `GridView` control y enlazar a datos de la `Courses` propiedad de navegación de la `Department` entidad.

Abra *Departments.aspx* y en el marcado para la `GridView` controlar, especificar un controlador para el `RowDataBound` eventos. El marcado para la etiqueta de apertura del control ahora similar al siguiente ejemplo.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Agregue un nuevo `TemplateField` elemento tras la `Administrator` campos de plantilla:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Este marcado crea un anidada `GridView` control que muestra el número y el título de una lista de cursos. No se especifica un origen de datos porque deberá enlazar datos en el código en el `RowDataBound` controlador.

Abra *Departments.aspx.cs* y agregue el siguiente controlador para el `RowDataBound` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Este código obtiene la `Department` convierte de entidad de los argumentos del evento, el `Courses` propiedad de navegación para una `List` colección y enlaza las anidadas `GridView` a la colección.

Abra el *SchoolRepository.cs* de archivos y especificar una carga diligente de la `Courses` propiedad de navegación mediante una llamada a la `Include` método en la consulta de objeto que se crea en el `GetDepartmentsByName` (método). El `return` instrucción en el `GetDepartmentsByName` método ahora similar al siguiente ejemplo.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Ejecute la página. Además de la ordenación y filtrado de capacidad que agregó anteriormente, el control GridView ahora muestra los detalles de curso anidada para cada departamento.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Con esto finaliza la introducción a los escenarios de ordenación, filtrados y principal-detalle. En el siguiente tutorial, verá cómo controlar la simultaneidad.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
