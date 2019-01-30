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
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="dc1de-103">Tutorial: Personalizar la vista de EF Database First con la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="dc1de-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="dc1de-104">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="dc1de-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="dc1de-105">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc1de-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="dc1de-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dc1de-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="dc1de-107">En este tutorial se centra en cambiar las vistas que se generan automáticamente para mejorar la presentación.</span><span class="sxs-lookup"><span data-stu-id="dc1de-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="dc1de-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="dc1de-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc1de-109">Agregar cursos a la página de detalle de alumno</span><span class="sxs-lookup"><span data-stu-id="dc1de-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="dc1de-110">Confirme que los cursos se agregan a la página</span><span class="sxs-lookup"><span data-stu-id="dc1de-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc1de-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="dc1de-111">Prerequisites</span></span>

* [<span data-ttu-id="dc1de-112">Cambiar la base de datos</span><span class="sxs-lookup"><span data-stu-id="dc1de-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="dc1de-113">Agregar cursos a los detalles del estudiante</span><span class="sxs-lookup"><span data-stu-id="dc1de-113">Add courses to student detail</span></span>

<span data-ttu-id="dc1de-114">El código generado proporciona un buen punto de partida para su aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dc1de-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="dc1de-115">Puede personalizar el código para satisfacer los requisitos específicos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dc1de-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="dc1de-116">Actualmente, la aplicación no muestra los inscritos cursos para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="dc1de-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="dc1de-117">En esta sección, agregará los cursos inscritos para cada alumno a la **detalles** vista para el alumno.</span><span class="sxs-lookup"><span data-stu-id="dc1de-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="dc1de-118">Abra **vistas** > **estudiantes** > *Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dc1de-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="dc1de-119">Debajo de la última &lt;/dl&gt; etiqueta, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="dc1de-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="dc1de-120">Este código crea una tabla que muestra una fila para cada registro en la tabla Enrollment para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="dc1de-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="dc1de-121">El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión.</span><span class="sxs-lookup"><span data-stu-id="dc1de-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="dc1de-122">Utilice el método de presentación (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el formato del valor correctamente según su tipo y la plantilla para ese tipo.</span><span class="sxs-lookup"><span data-stu-id="dc1de-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="dc1de-123">En este ejemplo, cada expresión devuelve una propiedad única desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.</span><span class="sxs-lookup"><span data-stu-id="dc1de-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="dc1de-124">Confirme que se agregan cursos</span><span class="sxs-lookup"><span data-stu-id="dc1de-124">Confirm courses are added</span></span>

<span data-ttu-id="dc1de-125">Ejecute la solución.</span><span class="sxs-lookup"><span data-stu-id="dc1de-125">Run the solution.</span></span> <span data-ttu-id="dc1de-126">Haga clic en **lista de alumnos** y seleccione **detalles** para uno de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="dc1de-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="dc1de-127">Verá que se han incluido los cursos inscritos en la vista.</span><span class="sxs-lookup"><span data-stu-id="dc1de-127">You will see the enrolled courses have been included in the view.</span></span>

![estudiante con la inscripción](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="dc1de-129">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="dc1de-129">Next steps</span></span>
<span data-ttu-id="dc1de-130">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="dc1de-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc1de-131">Cursos agregados a la página de detalle de alumno</span><span class="sxs-lookup"><span data-stu-id="dc1de-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="dc1de-132">Confirma que los cursos se agregan a la página</span><span class="sxs-lookup"><span data-stu-id="dc1de-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="dc1de-133">Avance al siguiente tutorial para obtener información sobre cómo agregar anotaciones de datos para especificar los requisitos de validación y formato para mostrar.</span><span class="sxs-lookup"><span data-stu-id="dc1de-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="dc1de-134">Mejorar la validación de datos</span><span class="sxs-lookup"><span data-stu-id="dc1de-134">Enhance data validation</span></span>](enhancing-data-validation.md)
