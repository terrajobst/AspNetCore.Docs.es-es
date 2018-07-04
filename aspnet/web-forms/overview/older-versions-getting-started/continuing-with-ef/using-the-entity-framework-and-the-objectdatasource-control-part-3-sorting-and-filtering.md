---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Con Entity Framework 4.0 y el Control ObjectDataSource, parte 3: ordenación y filtrado | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de Contoso University establecen que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. PUEDO...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378915"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="aca48-104">Con Entity Framework 4.0 y el Control ObjectDataSource, parte 3: ordenar y filtrar</span><span class="sxs-lookup"><span data-stu-id="aca48-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="aca48-105">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="aca48-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="aca48-106">Esta serie de tutoriales se basa en la aplicación web de Contoso University creado por el [Introducción a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="aca48-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="aca48-107">Si no has completado los tutoriales anteriores, como un punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado.</span><span class="sxs-lookup"><span data-stu-id="aca48-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="aca48-108">También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa.</span><span class="sxs-lookup"><span data-stu-id="aca48-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="aca48-109">Si tiene preguntas acerca de los tutoriales, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="aca48-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="aca48-110">En el tutorial anterior, implementa el patrón de repositorio en una aplicación web de n niveles que utiliza Entity Framework y el `ObjectDataSource` control.</span><span class="sxs-lookup"><span data-stu-id="aca48-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="aca48-111">Este tutorial muestra cómo ordenar y filtrar y controlar los escenarios de maestro y detalles.</span><span class="sxs-lookup"><span data-stu-id="aca48-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="aca48-112">Va a agregar las siguientes mejoras a la *Departments.aspx* página:</span><span class="sxs-lookup"><span data-stu-id="aca48-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="aca48-113">Un cuadro de texto para permitir a los usuarios seleccionar los departamentos de TI por su nombre.</span><span class="sxs-lookup"><span data-stu-id="aca48-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="aca48-114">Una lista de cursos para cada departamento que se muestra en la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="aca48-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="aca48-115">La capacidad de ordenar haciendo clic en los encabezados de columna.</span><span class="sxs-lookup"><span data-stu-id="aca48-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="aca48-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aca48-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="aca48-117">Agregar la capacidad para ordenar columnas de GridView</span><span class="sxs-lookup"><span data-stu-id="aca48-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="aca48-118">Abra el *Departments.aspx* página y agregue un `SortParameterName="sortExpression"` atributo a la `ObjectDataSource` control denominado `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="aca48-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="aca48-119">(Más adelante creará un `GetDepartments` método que toma un parámetro denominado `sortExpression`.) El marcado para la etiqueta de apertura del control ahora es similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="aca48-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="aca48-120">Agregar el `AllowSorting="true"` atributo a la etiqueta de apertura de la `GridView` control.</span><span class="sxs-lookup"><span data-stu-id="aca48-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="aca48-121">El marcado para la etiqueta de apertura del control ahora es similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="aca48-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="aca48-122">En *Departments.aspx.cs*, establecer el criterio de ordenación predeterminado mediante una llamada a la `GridView` del control `Sort` método desde el `Page_Load` método:</span><span class="sxs-lookup"><span data-stu-id="aca48-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="aca48-123">Puede agregar código que ordena o filtra en la clase de lógica de negocios o la clase del repositorio.</span><span class="sxs-lookup"><span data-stu-id="aca48-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="aca48-124">Si lo hace en la clase de lógica de negocios, la ordenación o filtrado de trabajo se realizará después de recuperan los datos de la base de datos, puesto que la clase de la lógica empresarial está trabajando con un `IEnumerable` objeto devuelto por el repositorio.</span><span class="sxs-lookup"><span data-stu-id="aca48-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="aca48-125">Si Agregar ordenación y filtrado de código de la clase de repositorio y hacerlo antes de una expresión LINQ o consulta de objeto se ha convertido en un `IEnumerable` de objeto, los comandos se pasarán a través de la base de datos para su procesamiento, que normalmente es más eficaz.</span><span class="sxs-lookup"><span data-stu-id="aca48-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="aca48-126">En este tutorial implementaremos ordenar y filtrar de forma que hace que el procesamiento se realice la base de datos, es decir, en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="aca48-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="aca48-127">Para agregar funcionalidad de ordenación, debe agregar un nuevo método a la interfaz de repositorio y clases de repositorio, así como para la clase de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="aca48-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="aca48-128">En el *ISchoolRepository.cs* , agregue un nuevo `GetDepartments` método que toma un `sortExpression` parámetro que se usará para ordenar la lista de los departamentos de TI que se devuelve:</span><span class="sxs-lookup"><span data-stu-id="aca48-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="aca48-129">El `sortExpression` parámetro especificará la columna para la ordenación y la dirección de ordenación.</span><span class="sxs-lookup"><span data-stu-id="aca48-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="aca48-130">Agregue código para el método nuevo a la *SchoolRepository.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="aca48-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="aca48-131">Cambiar las existentes sin parámetros `GetDepartments` método para llamar al método nuevo:</span><span class="sxs-lookup"><span data-stu-id="aca48-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="aca48-132">En el proyecto de prueba, agregue el siguiente método nuevo a *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="aca48-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="aca48-133">Si se va a crear cualquier prueba unitaria que dependía de este método devuelve una lista ordenada, necesitaría ordenar la lista antes de devolverlo.</span><span class="sxs-lookup"><span data-stu-id="aca48-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="aca48-134">Que no se creando pruebas similares en este tutorial, por lo que el método puede devolver simplemente la lista no ordenada de los departamentos de TI.</span><span class="sxs-lookup"><span data-stu-id="aca48-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="aca48-135">En el *SchoolBL.cs* , agregue el siguiente método nuevo a la clase de lógica de negocios:</span><span class="sxs-lookup"><span data-stu-id="aca48-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="aca48-136">Este código pasa el parámetro de ordenación al método del repositorio.</span><span class="sxs-lookup"><span data-stu-id="aca48-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="aca48-137">Ejecute el *Departments.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="aca48-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="aca48-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="aca48-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="aca48-139">Ahora puede hacer clic en cualquier encabezado de columna para ordenar por esa columna.</span><span class="sxs-lookup"><span data-stu-id="aca48-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="aca48-140">Si la columna ya está ordenada, al hacer clic en el encabezado se invierte la dirección de ordenación.</span><span class="sxs-lookup"><span data-stu-id="aca48-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="aca48-141">Agregar un cuadro de búsqueda</span><span class="sxs-lookup"><span data-stu-id="aca48-141">Adding a Search Box</span></span>

<span data-ttu-id="aca48-142">En esta sección podrá agregar un cuadro de texto de búsqueda, vincúlelo a la `ObjectDataSource` controlar mediante un parámetro de control y agregue un método a la clase de lógica de negocios para admitir el filtrado.</span><span class="sxs-lookup"><span data-stu-id="aca48-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="aca48-143">Abra el *Departments.aspx* página y agregue el marcado siguiente entre el encabezado y el primer `ObjectDataSource` control:</span><span class="sxs-lookup"><span data-stu-id="aca48-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="aca48-144">En el `ObjectDataSource` control denominado `DepartmentsObjectDataSource`, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="aca48-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="aca48-145">Agregar un `SelectParameters` (elemento) para un parámetro denominado `nameSearchString` que obtiene el valor especificado en el `SearchTextBox` control.</span><span class="sxs-lookup"><span data-stu-id="aca48-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="aca48-146">Cambiar el `SelectMethod` valor al atributo `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="aca48-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="aca48-147">(Creará este método más adelante.)</span><span class="sxs-lookup"><span data-stu-id="aca48-147">(You'll create this method later.)</span></span>

<span data-ttu-id="aca48-148">El marcado para el `ObjectDataSource` control ahora es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="aca48-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="aca48-149">En *ISchoolRepository.cs*, agregue un `GetDepartmentsByName` método que toma dos `sortExpression` y `nameSearchString` parámetros:</span><span class="sxs-lookup"><span data-stu-id="aca48-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="aca48-150">En *SchoolRepository.cs*, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="aca48-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="aca48-151">Este código usa un `Where` método para seleccionar los elementos que contienen la cadena de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="aca48-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="aca48-152">Si la cadena de búsqueda está vacía, se seleccionará todos los registros.</span><span class="sxs-lookup"><span data-stu-id="aca48-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="aca48-153">Tenga en cuenta que cuando se especifica el método llama a juntos en una instrucción similar al siguiente (`Include`, a continuación, `OrderBy`, a continuación, `Where`), el `Where` método siempre debe ser el último.</span><span class="sxs-lookup"><span data-stu-id="aca48-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="aca48-154">Cambiar las existentes `GetDepartments` método que toma un `sortExpression` parámetro al llamar al método nuevo:</span><span class="sxs-lookup"><span data-stu-id="aca48-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="aca48-155">En *MockSchoolRepository.cs* en el proyecto de prueba, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="aca48-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="aca48-156">En *SchoolBL.cs*, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="aca48-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="aca48-157">Ejecute el *Departments.aspx* página y escriba una cadena de búsqueda para asegurarse de que funciona la lógica de selección.</span><span class="sxs-lookup"><span data-stu-id="aca48-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="aca48-158">Deje vacío el cuadro de texto y se intenta realizar una búsqueda para asegurarse de que se devuelven todos los registros.</span><span class="sxs-lookup"><span data-stu-id="aca48-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="aca48-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="aca48-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="aca48-160">Agregar una columna de detalles para cada fila de cuadrícula</span><span class="sxs-lookup"><span data-stu-id="aca48-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="aca48-161">A continuación, desea ver todos los cursos para cada departamento que se muestra en la celda derecha de la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="aca48-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="aca48-162">Para ello, deberá usar un anidada `GridView` control y enlazar datos a los datos de la `Courses` propiedad de navegación de la `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="aca48-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="aca48-163">Abra *Departments.aspx* y en el marcado para la `GridView` controlar, especificar un controlador para el `RowDataBound` eventos.</span><span class="sxs-lookup"><span data-stu-id="aca48-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="aca48-164">El marcado para la etiqueta de apertura del control ahora es similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="aca48-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="aca48-165">Agregue un nuevo `TemplateField` elemento tras el `Administrator` campo de plantilla:</span><span class="sxs-lookup"><span data-stu-id="aca48-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="aca48-166">Este marcado crea un anidada `GridView` control que muestra el número de curso y el título de una lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="aca48-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="aca48-167">No especifica un origen de datos, porque deberá enlazar datos en el código en el `RowDataBound` controlador.</span><span class="sxs-lookup"><span data-stu-id="aca48-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="aca48-168">Abra *Departments.aspx.cs* y agregue el siguiente controlador para el `RowDataBound` eventos:</span><span class="sxs-lookup"><span data-stu-id="aca48-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="aca48-169">Este código obtiene el `Department` convierte la entidad de los argumentos del evento, el `Courses` propiedad de navegación a un `List` colección y enlaza los anidados `GridView` a la colección.</span><span class="sxs-lookup"><span data-stu-id="aca48-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="aca48-170">Abrir el *SchoolRepository.cs* archivo y especifique la carga diligente para la `Courses` propiedad de navegación mediante una llamada a la `Include` método en la consulta de objeto que se crea en el `GetDepartmentsByName` método.</span><span class="sxs-lookup"><span data-stu-id="aca48-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="aca48-171">El `return` instrucción en el `GetDepartmentsByName` método ahora es similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="aca48-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="aca48-172">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="aca48-172">Run the page.</span></span> <span data-ttu-id="aca48-173">Además de la ordenación y filtrado de funcionalidad que agregó anteriormente, el control GridView ahora muestra los detalles de curso anidada para cada departamento.</span><span class="sxs-lookup"><span data-stu-id="aca48-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="aca48-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="aca48-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="aca48-175">Con esto finaliza la introducción a los escenarios de ordenación, filtrados y pantalla principal-detallada.</span><span class="sxs-lookup"><span data-stu-id="aca48-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="aca48-176">En el siguiente tutorial, aprenderá a administrar la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="aca48-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aca48-177">[Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="aca48-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
