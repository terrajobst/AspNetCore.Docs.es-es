---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First con MVC de ASP.NET: generación de vistas | Microsoft Docs'
author: Rick-Anderson
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7d925573dd4cdf5c1a36e51f312e18093bd35043
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021097"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF Database First con MVC de ASP.NET: generación de vistas
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en usar el Scaffolding de ASP.NET para generar los controladores y vistas.


## <a name="add-scaffold"></a>Agregar scaffold

Está listo para generar el código que le proporcionará las operaciones de datos estándar para las clases del modelo. Agregue el código mediante la adición de un elemento de scaffolding. Hay muchas opciones para el tipo de scaffolding que puede agregar; en este tutorial, las plantillas scaffold incluirá un controlador y vistas que corresponden a los modelos de inscripción y estudiantes que creó en la sección anterior.

Para mantener la coherencia en el proyecto, agregará el nuevo controlador existente **controladores** carpeta. Haga clic en el **controladores** carpeta y seleccione **agregar** – **nuevo elemento de scaffolding**.

![Agregar scaffold](generating-views/_static/image1.png)

Seleccione el **controlador MVC 5 con vistas, usando Entity Framework** opción. Esta opción generará el controlador y vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.

![Agregar controlador de mvc](generating-views/_static/image2.png)

Seleccione **estudiante** para la clase de modelo y seleccione el **ContosoUniversityEntities** para la clase de contexto. Mantenga el nombre del controlador como **StudentsController**,

![Especifique el controlador](generating-views/_static/image3.png)

Haga clic en **Agregar**.

Si recibe un error, es posible que no se compiló el proyecto en la sección anterior. Si es así, intente compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.

Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en el proyecto.

![Mostrar vistas](generating-views/_static/image4.png)

Vuelva a realizar los mismos pasos, pero agregar una scaffold para la clase de inscripción. Cuando termine, debe tener un **EnrollmentsController.cs** archivo y una carpeta bajo **vistas** denominado **inscripciones** con crear, eliminar, detalles, edición e índice Vistas.

![Mostrar vistas](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Agregar vínculos a las nuevas vistas

Para facilitar la vaya a las nuevas vistas, puede agregar un par de hipervínculos a las vistas de índice para estudiantes y las inscripciones. Abra el archivo en **Views/Home/Index.cshtml**, que es la página principal de su sitio. Agregue el código siguiente debajo del jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

El método ActionLink, el primer parámetro es el texto que se muestra en el vínculo. El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador. Por ejemplo, el primer vínculo apunta a la acción del índice en StudentsController. El hipervínculo real se construye a partir de estos valores. En última instancia, el primer vínculo conduce al usuario a la **Index.cshtml** archivo dentro de la **vistas/estudiantes** carpeta.

## <a name="display-student-views"></a>Mostrar las vistas de alumno

Comprobará que el código que se agregó correctamente a su proyecto muestra una lista de los estudiantes y permite a los usuarios editar, crear o eliminar los registros de alumnos en la base de datos.

Haga clic en el **Views/Home/Index.cshtml** de archivo y seleccione **ver en el explorador**. En esta página, haga clic en el vínculo para obtener la lista de alumnos.

![](generating-views/_static/image6.png)

En esta página, observe la lista de los estudiantes y vínculos para modificar estos datos.

![lista de estudiantes](generating-views/_static/image7.png)

Haga clic en el **crear nuevo** vincular y proporcione valores para un alumno nuevo.

![Crear nuevo alumno](generating-views/_static/image8.png)

Haga clic en **crear**y observe el alumno nuevo se agrega a la lista.

![lista con nuevo alumno](generating-views/_static/image9.png)

Seleccione el **editar** vincular y cambiar algunos de los valores de un alumno.

![Editar alumno](generating-views/_static/image10.png)

Haga clic en **guardar**y observe el registro de estudiante ha cambiado.

Por último, seleccione el **eliminar** vincular y confirmar que desea eliminar el registro, haga clic en el **eliminar** botón.

![Eliminar alumno](generating-views/_static/image11.png)

Sin escribir ningún código, ha agregado vistas que realizan operaciones comunes en los datos en la tabla Student.

Es podrán que haya observado que la etiqueta de texto para un campo se basa en la propiedad de base de datos (como **LastName**) que no es necesariamente lo que desea mostrar en la página web. Por ejemplo, quizás prefiera la etiqueta sea **apellido**. Corregirá este problema de presentación más adelante en el tutorial.

## <a name="display-enrollment-views"></a>Mostrar las vistas de inscripción

La base de datos incluye una relación uno a varios entre las tablas Student e inscripción y una relación uno a varios entre las tablas Course y Enrollment. Las vistas para la inscripción de controlan correctamente estas relaciones. Vaya a la página principal de su sitio y seleccione el **lista de inscripciones de** vínculo y, a continuación, el **crear nuevo** vínculo. La vista muestra un formulario para crear un nuevo registro de inscripción. En concreto, tenga en cuenta que el formulario contiene dos listas desplegables que se rellenan con valores de las tablas relacionadas.

![crear la inscripción](generating-views/_static/image12.png)

Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo. Nivel requiere un número, por lo que se muestra un mensaje de error si intenta proporcionar un valor incompatible.

![mensaje de validación](generating-views/_static/image13.png)

Ha comprobado que las vistas genera automáticamente los usuarios pueden trabajar con los datos en la base de datos. En el siguiente tutorial de esta serie, va a actualizar la base de datos y haga los cambios correspondientes en la aplicación web.

> [!div class="step-by-step"]
> [Anterior](creating-the-web-application.md)
> [Siguiente](changing-the-database.md)
