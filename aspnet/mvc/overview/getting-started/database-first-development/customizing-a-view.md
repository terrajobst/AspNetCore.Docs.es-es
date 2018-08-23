---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First con MVC de ASP.NET: personalizar una vista | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: ce450af93459f2a69557b3fe0d1ead813ae99986
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837000"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First con MVC de ASP.NET: personalizar una vista
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en cambiar las vistas que se generan automáticamente para mejorar la presentación.


## <a name="add-enrolled-courses-to-student-details"></a>Agregar inscritos cursos a los detalles del estudiante

El código generado proporciona un buen punto de partida para su aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación. Puede personalizar el código para satisfacer los requisitos específicos de la aplicación. Actualmente, la aplicación no muestra los inscritos cursos para el alumno seleccionado. En esta sección, agregará los cursos inscritos para cada alumno a la **detalles** vista para el alumno.

Abra **Students/Details.cshtml**y por debajo de la última &lt;/dl&gt; ficha, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Este código crea una tabla que muestra una fila para cada registro en la tabla Enrollment para el alumno seleccionado. El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión. Utilice el método de presentación (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el formato del valor correctamente según su tipo y la plantilla para ese tipo. En este ejemplo, cada expresión devuelve una propiedad única desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.

Vaya a la vista de índice de Students/de nuevo y seleccione **detalles** para uno de los estudiantes. Verá que se han incluido los cursos inscritos en la vista.

![estudiante con la inscripción](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](changing-the-database.md)
> [Siguiente](enhancing-data-validation.md)
