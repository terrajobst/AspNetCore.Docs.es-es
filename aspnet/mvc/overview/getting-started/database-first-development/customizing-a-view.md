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
ms.locfileid: "30867663"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="48bf9-104">Base de datos EF primero con ASP.NET MVC: personalizar una vista</span><span class="sxs-lookup"><span data-stu-id="48bf9-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="48bf9-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="48bf9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="48bf9-106">Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="48bf9-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="48bf9-107">Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="48bf9-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="48bf9-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="48bf9-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="48bf9-109">Esta parte de la serie se centra acerca de cómo cambiar las vistas generados automáticamente para mejorar la presentación.</span><span class="sxs-lookup"><span data-stu-id="48bf9-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="48bf9-110">Agregar cursos inscritos a los detalles de estudiante</span><span class="sxs-lookup"><span data-stu-id="48bf9-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="48bf9-111">El código generado proporciona un buen punto de partida para la aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="48bf9-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="48bf9-112">Puede personalizar el código para satisfacer los requisitos específicos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="48bf9-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="48bf9-113">Actualmente, la aplicación no muestra los cursos inscritos de los estudiantes seleccionados.</span><span class="sxs-lookup"><span data-stu-id="48bf9-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="48bf9-114">En esta sección, agregará los cursos inscritos para cada estudiante a la **detalles** vista de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="48bf9-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="48bf9-115">Abra **Students/Details.cshtml**y por debajo de la última &lt;/dl&gt; ficha, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="48bf9-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="48bf9-116">Este código crea una tabla que muestra una fila para cada registro de la tabla de inscripción para los estudiantes seleccionados.</span><span class="sxs-lookup"><span data-stu-id="48bf9-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="48bf9-117">El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión.</span><span class="sxs-lookup"><span data-stu-id="48bf9-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="48bf9-118">Utilice el método Display (en lugar de incrustar simplemente el valor de propiedad en el código) para asegurarse de que se da formato al valor correctamente según su tipo y la plantilla para ese tipo.</span><span class="sxs-lookup"><span data-stu-id="48bf9-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="48bf9-119">En este ejemplo, cada expresión devuelve una sola propiedad desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.</span><span class="sxs-lookup"><span data-stu-id="48bf9-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="48bf9-120">Vaya a la vista de los alumnos/índice nuevo y seleccione **detalles** para uno de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="48bf9-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="48bf9-121">Verá que los cursos inscritos se han incluido en la vista.</span><span class="sxs-lookup"><span data-stu-id="48bf9-121">You will see the enrolled courses have been included in the view.</span></span>

![alumno con la inscripción](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="48bf9-123">[Anterior](changing-the-database.md)
> [Siguiente](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="48bf9-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
