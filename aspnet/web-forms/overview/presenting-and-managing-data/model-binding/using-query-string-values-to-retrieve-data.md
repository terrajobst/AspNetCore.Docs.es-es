---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usar valores de cadena de consulta para filtrar los datos con el enlace de modelos y formularios web Forms | Documentos de Microsoft
author: tfitzmac
description: "Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Uso de valores de cadena de consulta para filtrar los datos con el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.
> 
> Este tutorial muestra cómo pasar un valor en la cadena de consulta y use ese valor para recuperar los datos a través del enlace del modelo.
> 
> Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013. Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En este tutorial, deberá:

1. Agregue una nueva página para mostrar los cursos inscritos para un estudiante
2. Recuperar los cursos inscritos de los estudiantes seleccionado basándose en un valor de la cadena de consulta
3. Agregar un hipervínculo con un valor de cadena de consulta de la vista de cuadrícula a la nueva página

Los pasos de este tutorial son bastante similares a lo que hizo en el anterior [tutorial](sorting-paging-and-filtering-data.md) para filtrar los alumnos mostrados en función de la selección del usuario en una lista desplegable. En este tutorial, ha utilizado la **Control** atributo en el método select para especificar que el valor del parámetro procede de un control. En este tutorial, usará el **QueryString** atributo en el método select para especificar que el valor del parámetro procede de la cadena de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Agregar nueva página para mostrar cursos de un alumno

Agregue un nuevo formulario web que usa la página maestra Site.master y el nombre de la página **cursos**.

En el **Courses.aspx** de archivos, agregar una vista de cuadrícula para mostrar los cursos del alumno seleccionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir el método de selección

En **Courses.aspx.cs**, agregará el método de selección con el nombre especificado en la vista de cuadrícula **SelectMethod** propiedad. En ese método, deberá definir la consulta para recuperar los cursos de un alumno y especificar que el parámetro procede de un valor de cadena de consulta con el mismo nombre que el parámetro.

En primer lugar, debe agregar la siguiente **con** instrucciones.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

A continuación, agregue el código siguiente a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

El atributo de cadena de consulta significa que un valor de cadena de consulta denominado StudentID se asigna automáticamente al parámetro de este método.

## <a name="add-hyperlink-with-query-string-value"></a>Agregar hipervínculo con valor de cadena de consulta

En la vista de cuadrícula de Students.aspx, agregará un campo de hipervínculo que se vincula a la nueva página de cursos. El hipervínculo incluirá un valor de cadena de consulta con el identificador del estudiante.

En Students.aspx, agregue el siguiente campo a las columnas de la vista de cuadrícula justo debajo del campo para créditos Total.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Ejecute la aplicación y observe que la vista de cuadrícula incluye ahora el vínculo cursos.

![Agregar hipervínculo](using-query-string-values-to-retrieve-data/_static/image1.png)

Al hacer clic en uno de los vínculos, podrá ver cursos inscritos de ese estudiante.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha agregado un vínculo con un valor de cadena de consulta. Usa ese valor de cadena de consulta para el valor del parámetro en el método select.

En la siguiente [tutorial](adding-business-logic-layer.md), se moverá el código de los archivos de código subyacente en una capa de lógica de negocios y una capa de acceso a datos.

>[!div class="step-by-step"]
[Anterior](integrating-jquery-ui.md)
[Siguiente](adding-business-logic-layer.md)
