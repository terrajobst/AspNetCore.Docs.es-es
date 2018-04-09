---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Base de datos EF primero con ASP.NET MVC: personalizar una vista | Documentos de Microsoft'
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Base de datos EF primero con ASP.NET MVC: personalizar una vista
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra acerca de cómo cambiar las vistas generados automáticamente para mejorar la presentación.


## <a name="add-enrolled-courses-to-student-details"></a>Agregar cursos inscritos a los detalles de estudiante

El código generado proporciona un buen punto de partida para la aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación. Puede personalizar el código para satisfacer los requisitos específicos de la aplicación. Actualmente, la aplicación no muestra los cursos inscritos de los estudiantes seleccionados. En esta sección, agregará los cursos inscritos para cada estudiante a la **detalles** vista de los estudiantes.

Abra **Students/Details.cshtml**y por debajo de la última &lt;/dl&gt; ficha, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Este código crea una tabla que muestra una fila para cada registro de la tabla de inscripción para los estudiantes seleccionados. El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión. Utilice el método Display (en lugar de incrustar simplemente el valor de propiedad en el código) para asegurarse de que se da formato al valor correctamente según su tipo y la plantilla para ese tipo. En este ejemplo, cada expresión devuelve una sola propiedad desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.

Vaya a la vista de los alumnos/índice nuevo y seleccione **detalles** para uno de los alumnos. Verá que los cursos inscritos se han incluido en la vista.

![alumno con la inscripción](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](changing-the-database.md)
> [Siguiente](enhancing-data-validation.md)
