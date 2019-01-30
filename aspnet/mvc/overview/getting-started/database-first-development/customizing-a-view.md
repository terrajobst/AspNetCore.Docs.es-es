---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: Personalizar la vista de EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en cambiar las vistas que se generan automáticamente para mejorar la presentación.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236502"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Personalizar la vista de EF Database First con la aplicación de ASP.NET MVC

Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

En este tutorial se centra en cambiar las vistas que se generan automáticamente para mejorar la presentación.

En este tutorial ha:

> [!div class="checklist"]
> * Agregar cursos a la página de detalle de alumno
> * Confirme que los cursos se agregan a la página

## <a name="prerequisites"></a>Requisitos previos

* [Cambiar la base de datos](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Agregar cursos a los detalles del estudiante

El código generado proporciona un buen punto de partida para su aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación. Puede personalizar el código para satisfacer los requisitos específicos de la aplicación. Actualmente, la aplicación no muestra los inscritos cursos para el alumno seleccionado. En esta sección, agregará los cursos inscritos para cada alumno a la **detalles** vista para el alumno.

Abra **vistas** > **estudiantes** > *Details.cshtml*. Debajo de la última &lt;/dl&gt; etiqueta, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Este código crea una tabla que muestra una fila para cada registro en la tabla Enrollment para el alumno seleccionado. El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión. Utilice el método de presentación (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el formato del valor correctamente según su tipo y la plantilla para ese tipo. En este ejemplo, cada expresión devuelve una propiedad única desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.

## <a name="confirm-courses-are-added"></a>Confirme que se agregan cursos

Ejecute la solución. Haga clic en **lista de alumnos** y seleccione **detalles** para uno de los estudiantes. Verá que se han incluido los cursos inscritos en la vista.

![estudiante con la inscripción](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial ha:

> [!div class="checklist"]
> * Cursos agregados a la página de detalle de alumno
> * Confirma que los cursos se agregan a la página

Avance al siguiente tutorial para obtener información sobre cómo agregar anotaciones de datos para especificar los requisitos de validación y formato para mostrar.
> [!div class="nextstepaction"]
> [Mejorar la validación de datos](enhancing-data-validation.md)
