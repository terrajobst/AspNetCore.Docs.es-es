---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First con MVC de ASP.NET: personalizar una vista | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: bfbcfd39dd1cf0abe89a00d2958ca010f0e5e109
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376503"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="8d81c-104">EF Database First con MVC de ASP.NET: personalizar una vista</span><span class="sxs-lookup"><span data-stu-id="8d81c-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="8d81c-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8d81c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8d81c-106">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="8d81c-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8d81c-107">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d81c-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8d81c-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d81c-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="8d81c-109">Esta parte de la serie se centra en cambiar las vistas que se generan automáticamente para mejorar la presentación.</span><span class="sxs-lookup"><span data-stu-id="8d81c-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="8d81c-110">Agregar inscritos cursos a los detalles del estudiante</span><span class="sxs-lookup"><span data-stu-id="8d81c-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="8d81c-111">El código generado proporciona un buen punto de partida para su aplicación, pero no necesariamente proporciona toda la funcionalidad que necesita en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d81c-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="8d81c-112">Puede personalizar el código para satisfacer los requisitos específicos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d81c-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="8d81c-113">Actualmente, la aplicación no muestra los inscritos cursos para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8d81c-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="8d81c-114">En esta sección, agregará los cursos inscritos para cada alumno a la **detalles** vista para el alumno.</span><span class="sxs-lookup"><span data-stu-id="8d81c-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="8d81c-115">Abra **Students/Details.cshtml**y por debajo de la última &lt;/dl&gt; ficha, pero antes del cierre &lt;/div&gt; etiqueta, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="8d81c-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="8d81c-116">Este código crea una tabla que muestra una fila para cada registro en la tabla Enrollment para el alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8d81c-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="8d81c-117">El **mostrar** método representa HTML para el objeto (modelItem) que representa la expresión.</span><span class="sxs-lookup"><span data-stu-id="8d81c-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="8d81c-118">Utilice el método de presentación (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el formato del valor correctamente según su tipo y la plantilla para ese tipo.</span><span class="sxs-lookup"><span data-stu-id="8d81c-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="8d81c-119">En este ejemplo, cada expresión devuelve una propiedad única desde el registro actual en el bucle y los valores son tipos primitivos que se representan como texto.</span><span class="sxs-lookup"><span data-stu-id="8d81c-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="8d81c-120">Vaya a la vista de índice de Students/de nuevo y seleccione **detalles** para uno de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="8d81c-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="8d81c-121">Verá que se han incluido los cursos inscritos en la vista.</span><span class="sxs-lookup"><span data-stu-id="8d81c-121">You will see the enrolled courses have been included in the view.</span></span>

![estudiante con la inscripción](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="8d81c-123">[Anterior](changing-the-database.md)
> [Siguiente](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="8d81c-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
