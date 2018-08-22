---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordenar, paginar y filtrar datos con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839224"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c4adc-104">Ordenar, paginar y filtrar datos con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="c4adc-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="c4adc-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c4adc-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c4adc-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4adc-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c4adc-107">Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="c4adc-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c4adc-108">Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="c4adc-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c4adc-109">Este tutorial muestra cómo agregar ordenación, paginación y filtrado de los datos a través del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="c4adc-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="c4adc-110">Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="c4adc-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="c4adc-111">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="c4adc-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c4adc-112">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c4adc-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c4adc-113">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4adc-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="c4adc-114">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="c4adc-114">What you'll build</span></span>

<span data-ttu-id="c4adc-115">En este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="c4adc-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c4adc-116">Habilitar la ordenación y paginación de los datos</span><span class="sxs-lookup"><span data-stu-id="c4adc-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="c4adc-117">Habilitar el filtrado de los datos basados en una selección por el usuario</span><span class="sxs-lookup"><span data-stu-id="c4adc-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="c4adc-118">Agregar ordenación</span><span class="sxs-lookup"><span data-stu-id="c4adc-118">Add sorting</span></span>

<span data-ttu-id="c4adc-119">Es muy fácil habilitar la ordenación en el control GridView.</span><span class="sxs-lookup"><span data-stu-id="c4adc-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="c4adc-120">En el archivo Student.aspx, basta con establecer **AllowSorting** a **true** en GridView.</span><span class="sxs-lookup"><span data-stu-id="c4adc-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="c4adc-121">No es necesario establecer una **SortExpression** valor para cada columna, tal como se usa automáticamente la propiedad DataField.</span><span class="sxs-lookup"><span data-stu-id="c4adc-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="c4adc-122">El control GridView modifica la consulta para incluir la clasificación de los datos por el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c4adc-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="c4adc-123">El código resaltado siguiente se muestra la incorporación que tiene que realizar para habilitar la ordenación.</span><span class="sxs-lookup"><span data-stu-id="c4adc-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="c4adc-124">Ejecutar la aplicación web y probar la ordenación de los registros de alumnos por los valores en columnas diferentes.</span><span class="sxs-lookup"><span data-stu-id="c4adc-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![estudiantes de ordenación](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="c4adc-126">Agregar paginación</span><span class="sxs-lookup"><span data-stu-id="c4adc-126">Add paging</span></span>

<span data-ttu-id="c4adc-127">También es muy fácil habilitar la paginación.</span><span class="sxs-lookup"><span data-stu-id="c4adc-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="c4adc-128">En el control GridView, establezca el **AllowPaging** propiedad **true** y establezca el **PageSize** propiedad para el número de registros que desea mostrar en cada página.</span><span class="sxs-lookup"><span data-stu-id="c4adc-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="c4adc-129">En este tutorial, puede establecer a 4.</span><span class="sxs-lookup"><span data-stu-id="c4adc-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="c4adc-130">Ejecutar la aplicación web y observe que ahora se dividen los registros a través de varias páginas con no más de 4 registros mostrados en una sola página.</span><span class="sxs-lookup"><span data-stu-id="c4adc-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Agregar paginación](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="c4adc-132">Ejecución de consultas en diferido mejora la eficacia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c4adc-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="c4adc-133">En lugar de recuperar todo el conjunto de datos, el control GridView modifica la consulta para recuperar sólo los registros de la página actual.</span><span class="sxs-lookup"><span data-stu-id="c4adc-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="c4adc-134">Filtrar registros mediante la selección del usuario</span><span class="sxs-lookup"><span data-stu-id="c4adc-134">Filter records by user selection</span></span>

<span data-ttu-id="c4adc-135">Enlace de modelos agrega varios atributos que permiten designar cómo establecer el valor de un parámetro en un método de enlace de modelo.</span><span class="sxs-lookup"><span data-stu-id="c4adc-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="c4adc-136">Estos atributos se encuentran en el **System.Web.ModelBinding** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c4adc-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="c4adc-137">Son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4adc-137">They include:</span></span>

- <span data-ttu-id="c4adc-138">Control</span><span class="sxs-lookup"><span data-stu-id="c4adc-138">Control</span></span>
- <span data-ttu-id="c4adc-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="c4adc-139">Cookie</span></span>
- <span data-ttu-id="c4adc-140">Form</span><span class="sxs-lookup"><span data-stu-id="c4adc-140">Form</span></span>
- <span data-ttu-id="c4adc-141">Perfil</span><span class="sxs-lookup"><span data-stu-id="c4adc-141">Profile</span></span>
- <span data-ttu-id="c4adc-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="c4adc-142">QueryString</span></span>
- <span data-ttu-id="c4adc-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="c4adc-143">RouteData</span></span>
- <span data-ttu-id="c4adc-144">Sesión</span><span class="sxs-lookup"><span data-stu-id="c4adc-144">Session</span></span>
- <span data-ttu-id="c4adc-145">Perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="c4adc-145">UserProfile</span></span>
- <span data-ttu-id="c4adc-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="c4adc-146">ViewState</span></span>

<span data-ttu-id="c4adc-147">En este tutorial, usará el valor de un control para filtrar los registros que se muestran en el control GridView.</span><span class="sxs-lookup"><span data-stu-id="c4adc-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="c4adc-148">Agregará el **Control** atributo al método de consulta que había creado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c4adc-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="c4adc-149">En un [más adelante](using-query-string-values-to-retrieve-data.md) tutorial, se aplicará el **QueryString** atributo a un parámetro para especificar que el valor del parámetro procede de un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c4adc-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="c4adc-150">En primer lugar, encima de la ValidationSummary, agregue una lista desplegable de filtrado que los alumnos se muestran.</span><span class="sxs-lookup"><span data-stu-id="c4adc-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="c4adc-151">En el archivo de código subyacente, modifique el método select para recibir un valor del control y establezca el nombre del parámetro en el nombre del control que proporciona el valor.</span><span class="sxs-lookup"><span data-stu-id="c4adc-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="c4adc-152">Debe agregar un **mediante** instrucción para el **System.Web.ModelBinding** espacio de nombres para resolver el atributo de Control.</span><span class="sxs-lookup"><span data-stu-id="c4adc-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="c4adc-153">El código siguiente muestra el método select volver a trabajado para filtrar los datos devueltos según el valor de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="c4adc-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="c4adc-154">Agregar un atributo de control antes de que un parámetro especifica que el valor para este parámetro procede de un control con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="c4adc-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="c4adc-155">Ejecute la aplicación web y seleccione valores diferentes en la lista desplegable para filtrar la lista de alumnos.</span><span class="sxs-lookup"><span data-stu-id="c4adc-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![estudiantes de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="c4adc-157">Conclusión</span><span class="sxs-lookup"><span data-stu-id="c4adc-157">Conclusion</span></span>

<span data-ttu-id="c4adc-158">En este tutorial, ha habilitado la ordenación y paginación de los datos.</span><span class="sxs-lookup"><span data-stu-id="c4adc-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="c4adc-159">También ha habilitado el filtrado de los datos por el valor de un control.</span><span class="sxs-lookup"><span data-stu-id="c4adc-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="c4adc-160">En los próximos [tutorial](integrating-jquery-ui.md) mejorará la interfaz de usuario mediante la integración de un widget de JQuery UI en la plantilla de datos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="c4adc-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c4adc-161">[Anterior](updating-deleting-and-creating-data.md)
> [Siguiente](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="c4adc-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
