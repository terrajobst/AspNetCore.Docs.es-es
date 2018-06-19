---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Base de datos EF primero con ASP.NET MVC: generar vistas | Documentos de Microsoft'
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879756"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Base de datos EF primero con ASP.NET MVC: generación de vistas
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en mediante Scaffolding de ASP.NET para generar los controladores y vistas.


## <a name="add-scaffold"></a>Agregar scaffolding

Está listo para generar el código que le proporcionará las operaciones de datos estándar para las clases del modelo. Agregue el código agregando un elemento de scaffolding. Existen muchas opciones para el tipo de scaffolding que puede agregar; en este tutorial, el scaffolding incluirá un controlador y vistas que corresponden a los modelos de Student y Enrollment que creó en la sección anterior.

Para mantener la coherencia en el proyecto, agregará el nuevo controlador a la existente **controladores** carpeta. Haga clic en el **controladores** carpeta y seleccione **agregar** : **nuevo elemento de scaffolding**.

![Agregar scaffolding](generating-views/_static/image1.png)

Seleccione el **controlador de MVC 5 con vistas, mediante Entity Framework** opción. Esta opción generará el controlador y vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.

![Agregar controlador de mvc](generating-views/_static/image2.png)

Seleccione **Student** para la clase de modelo y seleccione la **ContosoUniversityEntities** para la clase de contexto. Mantener el nombre del controlador como **StudentsController**,

![especificar controlador](generating-views/_static/image3.png)

Haga clic en **Agregar**.

Si recibe un error, puede deberse a que no se ha generado el proyecto en la sección anterior. Si es así, pruebe a compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.

Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en el proyecto.

![Mostrar vistas](generating-views/_static/image4.png)

Vuelva a realizar los mismos pasos, pero agregar scaffolding para la clase de inscripción. Cuando termine, debe tener un **EnrollmentsController.cs** archivo y una carpeta bajo **vistas** denominado **las inscripciones** con Create, Delete, detalles, editar e índice Vistas.

![Mostrar vistas](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Agregar vínculos a las nuevas vistas

Para facilitar la vaya a sus vistas nuevo, puede agregar un par de hipervínculos a las vistas de índice para estudiantes y las inscripciones. Abra el archivo en **Views/Home/Index.cshtml**, que es la página principal de su sitio. Agregue el código siguiente debajo del jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

El método ActionLink, el primer parámetro es el texto que se mostrará en el vínculo. El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador. Por ejemplo, el primer vínculo apunta a la acción del índice en StudentsController. El hipervínculo real se construye a partir de estos valores. El primer vínculo lleva en última instancia a los usuarios para el **Index.cshtml** archivo dentro de la **vistas/estudiantes** carpeta.

## <a name="display-student-views"></a>Mostrar las vistas de estudiante

Comprobará que el código que se agregó correctamente a su proyecto de muestra una lista de los alumnos y permite a los usuarios editar, crear o eliminar los registros de estudiante en la base de datos.

Haga clic en el **Views/Home/Index.cshtml** de archivos y seleccione **ver en el explorador**. En esta página, haga clic en el vínculo de la lista de estudiantes.

![](generating-views/_static/image6.png)

En esta página, observe la lista de los estudiantes y vínculos para modificar estos datos.

![lista de estudiantes](generating-views/_static/image7.png)

Haga clic en el **crear nuevo** vincular y proporcionar algunos valores para un nuevo alumno.

![Crear nuevo alumno](generating-views/_static/image8.png)

Haga clic en **crear**y observe los estudiantes nueva se agregan a la lista.

![lista con nuevo alumno](generating-views/_static/image9.png)

Seleccione el **editar** vincular y cambiar algunos de los valores de un estudiante.

![Editar student](generating-views/_static/image10.png)

Haga clic en **guardar**y observe el registro de estudiante ha cambiado.

Por último, seleccione la **eliminar** vincular y confirmar que desea eliminar el registro, haga clic en el **eliminar** botón.

![Eliminar alumno](generating-views/_static/image11.png)

Sin escribir ningún código, ha agregado vistas que llevan a cabo operaciones comunes en los datos en la tabla de estudiantes.

Es podrán que haya observado que la etiqueta de texto de un campo se basa en la propiedad de base de datos (como **LastName**) que no es necesariamente lo que desea que aparezca en la página web. Por ejemplo, quizás prefiera la etiqueta sea **Last Name**. Corregirá este problema de presentación más adelante en el tutorial.

## <a name="display-enrollment-views"></a>Mostrar las vistas de inscripción

La base de datos incluye una relación de uno a varios entre las tablas Student y Enrollment y una relación de uno a varios entre las tablas de curso e inscripción. Las vistas para la inscripción de administrar correctamente estas relaciones. Vaya a la página principal de su sitio y seleccione el **lista de las inscripciones** vínculo y, a continuación, el **crear nuevo** vínculo. La vista muestra un formulario para crear un nuevo registro de inscripción. En concreto, tenga en cuenta que el formulario contiene dos listas desplegables que se rellenan con los valores de las tablas relacionadas.

![crear la inscripción](generating-views/_static/image12.png)

Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo. Grado de requiere un número, por lo que se muestra un mensaje de error si intenta proporcionar un valor incompatible.

![mensaje de validación](generating-views/_static/image13.png)

Ha comprobado que las vistas generado automáticamente que los usuarios puedan trabajar con los datos en la base de datos. En el siguiente tutorial de esta serie, se actualizará la base de datos y realice los cambios correspondientes en la aplicación web.

> [!div class="step-by-step"]
> [Anterior](creating-the-web-application.md)
> [Siguiente](changing-the-database.md)
