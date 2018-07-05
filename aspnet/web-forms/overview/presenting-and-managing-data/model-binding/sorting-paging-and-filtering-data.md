---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordenar, paginar y filtrar datos con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 7529c811e6196327094f8f735de1bd65be76ee3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398312"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Ordenar, paginar y filtrar datos con enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> Este tutorial muestra cómo agregar ordenación, paginación y filtrado de los datos a través del enlace de modelos.
> 
> Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

1. Habilitar la ordenación y paginación de los datos
2. Habilitar el filtrado de los datos basados en una selección por el usuario

## <a name="add-sorting"></a>Agregar ordenación

Es muy fácil habilitar la ordenación en el control GridView. En el archivo Student.aspx, basta con establecer **AllowSorting** a **true** en GridView. No es necesario establecer una **SortExpression** valor para cada columna, tal como se usa automáticamente la propiedad DataField. El control GridView modifica la consulta para incluir la clasificación de los datos por el valor seleccionado. El código resaltado siguiente se muestra la incorporación que tiene que realizar para habilitar la ordenación.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Ejecutar la aplicación web y probar la ordenación de los registros de alumnos por los valores en columnas diferentes.

![estudiantes de ordenación](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Agregar paginación

También es muy fácil habilitar la paginación. En el control GridView, establezca el **AllowPaging** propiedad **true** y establezca el **PageSize** propiedad para el número de registros que desea mostrar en cada página. En este tutorial, puede establecer a 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Ejecutar la aplicación web y observe que ahora se dividen los registros a través de varias páginas con no más de 4 registros mostrados en una sola página.

![Agregar paginación](sorting-paging-and-filtering-data/_static/image4.png)

Ejecución de consultas en diferido mejora la eficacia de la aplicación. En lugar de recuperar todo el conjunto de datos, el control GridView modifica la consulta para recuperar sólo los registros de la página actual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros mediante la selección del usuario

Enlace de modelos agrega varios atributos que permiten designar cómo establecer el valor de un parámetro en un método de enlace de modelo. Estos atributos se encuentran en el **System.Web.ModelBinding** espacio de nombres. Son los siguientes:

- Control
- Cookie
- Form
- Perfil
- QueryString
- RouteData
- Sesión
- Perfil de usuario
- ViewState

En este tutorial, usará el valor de un control para filtrar los registros que se muestran en el control GridView. Agregará el **Control** atributo al método de consulta que había creado anteriormente. En un [más adelante](using-query-string-values-to-retrieve-data.md) tutorial, se aplicará el **QueryString** atributo a un parámetro para especificar que el valor del parámetro procede de un valor de cadena de consulta.

En primer lugar, encima de la ValidationSummary, agregue una lista desplegable de filtrado que los alumnos se muestran.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

En el archivo de código subyacente, modifique el método select para recibir un valor del control y establezca el nombre del parámetro en el nombre del control que proporciona el valor.

Debe agregar un **mediante** instrucción para el **System.Web.ModelBinding** espacio de nombres para resolver el atributo de Control.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

El código siguiente muestra el método select volver a trabajado para filtrar los datos devueltos según el valor de la lista desplegable. Agregar un atributo de control antes de que un parámetro especifica que el valor para este parámetro procede de un control con el mismo nombre.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Ejecute la aplicación web y seleccione valores diferentes en la lista desplegable para filtrar la lista de alumnos.

![estudiantes de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha habilitado la ordenación y paginación de los datos. También ha habilitado el filtrado de los datos por el valor de un control.

En los próximos [tutorial](integrating-jquery-ui.md) mejorará la interfaz de usuario mediante la integración de un widget de JQuery UI en la plantilla de datos dinámicos.

> [!div class="step-by-step"]
> [Anterior](updating-deleting-and-creating-data.md)
> [Siguiente](integrating-jquery-ui.md)
