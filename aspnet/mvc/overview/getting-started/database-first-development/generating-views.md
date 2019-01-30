---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: Generar vistas para EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en usar el Scaffolding de ASP.NET para generar los controladores y vistas.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236424"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Generar vistas para EF Database First con la aplicación de ASP.NET MVC

Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

En este tutorial se centra en usar el Scaffolding de ASP.NET para generar los controladores y vistas.

En este tutorial ha:

> [!div class="checklist"]
> * Agregar scaffold
> * Agregar vínculos a las nuevas vistas
> * Mostrar las vistas de alumno
> * Mostrar las vistas de inscripción

## <a name="prerequisite"></a>Requisito previo

* [Crear modelos de datos y aplicaciones de web](creating-the-web-application.md)

## <a name="add-scaffold"></a>Agregar scaffold

Está listo para generar el código que le proporcionará las operaciones de datos estándar para las clases del modelo. Agregue el código mediante la adición de un elemento de scaffolding. Hay muchas opciones para el tipo de scaffolding que puede agregar; en este tutorial, las plantillas scaffold incluirá un controlador y vistas que corresponden a los modelos de inscripción y estudiantes que creó en la sección anterior.

Para mantener la coherencia en el proyecto, agregará el nuevo controlador existente **controladores** carpeta. Haga clic en el **controladores** carpeta y seleccione **agregar** > **nuevo elemento de scaffolding**.

Seleccione el **controlador MVC 5 con vistas, usando Entity Framework** opción. Esta opción generará el controlador y vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.

![Agregar controlador de mvc](generating-views/_static/image2.png)

Seleccione **Student (ContosoSite.Models)** para la clase de modelo y seleccione el **ContosoUniversityDataEntities (ContosoSite.Models)** para la clase de contexto. Mantenga el nombre del controlador como **StudentsController**.

Haga clic en **Agregar**.

Si recibe un error, es posible que no se compiló el proyecto en la sección anterior. Si es así, intente compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.

Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en el proyecto **controladores** y **vistas** > **estudiantes** carpetas .


Vuelva a realizar los mismos pasos, pero agregar scaffolding para el **inscripción** clase. Cuando termine, tendrá un **EnrollmentsController.cs** archivo y una carpeta bajo **vistas** denominado **inscripciones** con las vistas de crear, eliminar, detalles, edición e índice.

## <a name="add-links-to-new-views"></a>Agregar vínculos a las nuevas vistas

Para facilitar la vaya a las nuevas vistas, puede agregar un par de hipervínculos a las vistas de índice para estudiantes y las inscripciones. Abra el archivo en **vistas** > **principal** > *Index.cshtml*, que es la página principal de su sitio. Agregue el código siguiente debajo del jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

El método ActionLink, el primer parámetro es el texto que se muestra en el vínculo. El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador. Por ejemplo, el primer vínculo apunta a la acción del índice en StudentsController. El hipervínculo real se construye a partir de estos valores. En última instancia, el primer vínculo conduce al usuario a la **Index.cshtml** archivo dentro de la **vistas/estudiantes** carpeta.

## <a name="display-student-views"></a>Mostrar las vistas de alumno

Comprobará que el código que se agregó correctamente a su proyecto muestra una lista de los estudiantes y permite a los usuarios editar, crear o eliminar los registros de alumnos en la base de datos.

Haga clic en el **vistas** > **inicio** > *Index.cshtml* de archivo y seleccione **ver en el explorador**. En la página de inicio de la aplicación, seleccione **lista de alumnos**.

![](generating-views/_static/image6.png)

En el **índice** página, observe la lista de los estudiantes y vínculos para modificar estos datos. Seleccione el **crear nuevo** vincular y proporcione valores para un alumno nuevo. Haga clic en **crear**y observe el alumno nuevo se agrega a la lista.

En el **índice** página, seleccione el **editar** vincular y cambiar algunos de los valores de un alumno. Haga clic en **guardar**y observe el registro de estudiante ha cambiado.

Por último, seleccione el **eliminar** vincular y confirmar que desea eliminar el registro, haga clic en el **eliminar** botón.

Sin escribir ningún código, ha agregado vistas que realizan operaciones comunes en los datos en la tabla Student.

Es podrán que haya observado que la etiqueta de texto para un campo se basa en la propiedad de base de datos (como **LastName**) que no es necesariamente lo que desea mostrar en la página web. Por ejemplo, quizás prefiera la etiqueta sea **apellido**. Corregirá este problema de presentación más adelante en el tutorial.

## <a name="display-enrollment-views"></a>Mostrar las vistas de inscripción

La base de datos incluye una relación uno a varios entre las tablas Student e inscripción y una relación uno a varios entre las tablas Course y Enrollment. Las vistas para la inscripción de controlan correctamente estas relaciones. Vaya a la página principal de su sitio y seleccione el **lista de inscripciones de** vínculo y, a continuación, el **crear nuevo** vínculo.

La vista muestra un formulario para crear un nuevo registro de inscripción. En concreto, tenga en cuenta que el formulario contiene un **CourseID** lista desplegable y un **StudentID** lista desplegable. Ambos se rellenan con valores de las tablas relacionadas.

Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo. **Grado** requiere un número, por lo que se muestra un mensaje de error si intenta proporcionar un valor incompatible: *El nivel de campo debe ser un número.*

Ha comprobado que las vistas genera automáticamente los usuarios pueden trabajar con los datos en la base de datos. En el siguiente tutorial de esta serie, va a actualizar la base de datos y haga los cambios correspondientes en la aplicación web.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Se ha agregado scaffold
> * Se han agregado vínculos a las nuevas vistas
> * Vistas de alumno mostrados
> * Vistas de inscripción mostrados

En el siguiente tutorial para aprender a cambiar la base de datos.
> [!div class="nextstepaction"]
> [Cambiar la base de datos](changing-the-database.md)