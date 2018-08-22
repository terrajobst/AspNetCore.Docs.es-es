---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Uso de los valores de cadena de consulta para filtrar los datos con enlace de modelos y formularios web Forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828483"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Uso de valores de cadena de consulta para filtrar los datos con enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> Este tutorial muestra cómo pasar un valor de la cadena de consulta y usar ese valor para recuperar los datos a través del enlace de modelos.
> 
> Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

1. Agregue una nueva página para mostrar los cursos inscritos de un alumno
2. Recuperar los inscritos cursos para el alumno seleccionado según un valor en la cadena de consulta
3. Agregar un hipervínculo con un valor de cadena de consulta de la vista de cuadrícula a la nueva página

Los pasos descritos en este tutorial son bastante similares a lo que hizo anteriormente [tutorial](sorting-paging-and-filtering-data.md) para filtrar los alumnos mostrados basándose en la selección del usuario en una lista desplegable. En este tutorial, ha utilizado el **Control** atributo en el método select para especificar que el valor del parámetro procede de un control. En este tutorial, usará el **QueryString** atributo en el método select para especificar que el valor del parámetro procede de la cadena de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Agregar nueva página para mostrar cursos de un alumno

Agregue un nuevo formulario web que usa la página maestra Site.master y el nombre de la página **cursos**.

En el **Courses.aspx** , agregue una vista de cuadrícula para mostrar los cursos para el alumno seleccionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir el método select

En **Courses.aspx.cs**, se agregará el método select con el nombre especificado en la vista de cuadrícula **SelectMethod** propiedad. En ese método, deberá definir la consulta para recuperar los cursos de un alumno y especificar que el parámetro procede de un valor de cadena de consulta con el mismo nombre que el parámetro.

En primer lugar, debe agregar lo siguiente **mediante** instrucciones.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

A continuación, agregue el código siguiente a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

El atributo de cadena de consulta significa que un valor de cadena de consulta denominado StudentID automáticamente se asigna al parámetro de este método.

## <a name="add-hyperlink-with-query-string-value"></a>Agregar hipervínculo con el valor de cadena de consulta

En la vista de cuadrícula en Students.aspx, agregará un campo de hipervínculo que se vincula a la nueva página de cursos. El hipervínculo incluirá un valor de cadena de consulta con el identificador del estudiante.

En Students.aspx, agregue el siguiente campo a las columnas de la vista de cuadrícula justo debajo del campo de créditos totales.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Ejecute la aplicación y observe que la vista de cuadrícula incluye ahora el vínculo de cursos.

![Agregar hipervínculo](using-query-string-values-to-retrieve-data/_static/image1.png)

Al hacer clic en uno de los vínculos, podrá ver cursos inscritos de ese estudiante.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha agregado un vínculo con un valor de cadena de consulta. Utiliza ese valor de cadena de consulta para el valor del parámetro en el método select.

En los próximos [tutorial](adding-business-logic-layer.md), se moverá el código de los archivos de código subyacente en una capa de lógica empresarial y una capa de acceso a datos.

> [!div class="step-by-step"]
> [Anterior](integrating-jquery-ui.md)
> [Siguiente](adding-business-logic-layer.md)
