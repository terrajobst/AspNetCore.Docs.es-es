---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "Ordenación, paginación y filtrado de datos con el enlace de modelos y formularios web forms | Documentos de Microsoft"
author: tfitzmac
description: "Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Ordenación, paginación y filtrado de datos con el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.
> 
> Este tutorial muestra cómo agregar ordenación, paginación y filtrado de los datos a través del enlace del modelo.
> 
> Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013. Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En este tutorial, deberá:

1. Habilitar la ordenación y la paginación de los datos
2. Habilitar el filtrado de los datos basados en una selección por el usuario

## <a name="add-sorting"></a>Agregar ordenación

Habilitar la ordenación en el control GridView es muy fácil. En el archivo Student.aspx, basta con establecer **AllowSorting** a **true** en GridView. No es necesario establecer un **SortExpression** valor para cada columna, tal como se utiliza automáticamente la propiedad DataField. GridView modifica la consulta para incluir ordenar los datos por el valor seleccionado. El código que aparece resaltado a continuación se muestra la incorporación que debe realizar para habilitar la ordenación.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Ejecutar la aplicación web y probar los registros de los estudiantes ordenación por los valores en columnas diferentes.

![estudiantes de ordenación](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Agregar paginación

Habilitar paginación también es muy fácil. En el control GridView, establezca el **AllowPaging** propiedad **true** y establezca el **PageSize** propiedad para el número de registros que se va a mostrar en cada página. En este tutorial, puede establecerlo en 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Ejecutar la aplicación web y observe que ahora los registros se dividen en varias páginas con no más de 4 registros que se muestran en una sola página.

![Agregar paginación](sorting-paging-and-filtering-data/_static/image4.png)

Ejecución de consultas en diferido mejora la eficacia de la aplicación. En lugar de recuperar todo el conjunto de datos, el control GridView modifica la consulta para recuperar únicamente los registros de la página actual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros mediante la selección del usuario

Enlace del modelo agrega varios atributos que permiten designar cómo establecer el valor de un parámetro en un método de enlace de modelo. Estos atributos no están en el **System.Web.ModelBinding** espacio de nombres. Son los siguientes:

- Control
- Cookie
- Form
- Perfil
- QueryString
- RouteData
- Sesión
- Perfil de usuario
- Estado de vista

En este tutorial, usará el valor de un control para filtrar qué registros se muestran en el control GridView. Agregará el **Control** atributo al método de consulta que había creado anteriormente. En un [más adelante](using-query-string-values-to-retrieve-data.md) tutorial, aprenderá a aplicar la **QueryString** atributo a un parámetro para especificar que el valor del parámetro procede de un valor de cadena de consulta.

En primer lugar, encima de la ValidationSummary, agregue una lista desplegable de filtrado se muestran los alumnos.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

En el archivo de código subyacente, modifique el método de selección para recibir un valor desde el control y establezca el nombre del parámetro en el nombre del control que proporciona el valor.

Debe agregar una **con** instrucción para el **System.Web.ModelBinding** espacio de nombres para resolver el atributo de Control.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

El código siguiente muestra el método de selección vuelve a funcionó bien para filtrar los datos devueltos en función del valor de la lista desplegable. Agregar un atributo de control antes de que un parámetro especifica que el valor de este parámetro procede de un control con el mismo nombre.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Ejecutar la aplicación web y seleccione valores diferentes en la lista desplegable para filtrar la lista de estudiantes.

![estudiantes de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, se habilitó la ordenación y la paginación de los datos. También ha habilitado el filtrado de los datos por el valor de un control.

En la siguiente [tutorial](integrating-jquery-ui.md) mejorará la interfaz de usuario mediante la integración de un widget de JQuery UI en la plantilla de datos dinámicos.

>[!div class="step-by-step"]
[Anterior](updating-deleting-and-creating-data.md)
[Siguiente](integrating-jquery-ui.md)
