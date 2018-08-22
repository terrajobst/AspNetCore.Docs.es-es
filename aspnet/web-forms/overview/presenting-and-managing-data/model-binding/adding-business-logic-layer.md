---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 2c54c0a635c662c8740aa7f9b6bd02cd71913e24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837995"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="d2d3c-104">Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="d2d3c-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="d2d3c-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d2d3c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d2d3c-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d2d3c-107">Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d2d3c-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d2d3c-108">Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d2d3c-109">Este tutorial muestra cómo utilizar el enlace de modelos con una capa de lógica empresarial.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="d2d3c-110">Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="d2d3c-111">Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="d2d3c-112">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d2d3c-113">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d2d3c-114">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d2d3c-115">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="d2d3c-115">What you'll build</span></span>

<span data-ttu-id="d2d3c-116">Enlace de modelos le permite colocar el código de interacción de datos en el archivo de código subyacente para una página web o en una clase de lógica de negocio independientes.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="d2d3c-117">Los tutoriales anteriores se muestra cómo usar los archivos de código subyacente para el código de interacción de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="d2d3c-118">Este enfoque funciona para sitios pequeños, pero que puede dar lugar a código repetición y mayor dificultad al mantener un sitio grande.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="d2d3c-119">También puede ser muy difícil de probar código que reside en los archivos de código subyacente porque no hay ninguna capa de abstracción de mediante programación.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="d2d3c-120">Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contiene toda la lógica para interactuar con datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="d2d3c-121">A continuación, llame a la capa de lógica de negocios desde las páginas web.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="d2d3c-122">Este tutorial muestra cómo mover todo el código que ha escrito en los tutoriales anteriores en una capa de lógica empresarial y, a continuación, usar dicho código desde las páginas.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="d2d3c-123">En este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="d2d3c-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d2d3c-124">Mover el código de archivos de código subyacente a una capa de lógica empresarial</span><span class="sxs-lookup"><span data-stu-id="d2d3c-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="d2d3c-125">Cambiar los controles enlazados a datos para llamar a los métodos en la capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="d2d3c-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="d2d3c-126">Crear la capa de lógica empresarial</span><span class="sxs-lookup"><span data-stu-id="d2d3c-126">Create business logic layer</span></span>

<span data-ttu-id="d2d3c-127">Ahora, creará la clase que se llama desde las páginas web.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="d2d3c-128">Los métodos de esta clase tener un aspecto similares a los métodos que usó en los tutoriales anteriores e incluyen los atributos de proveedor de valor.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="d2d3c-129">En primer lugar, agregue una nueva carpeta denominada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-129">First, add a new folder called **BLL**.</span></span>

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="d2d3c-131">En la carpeta BLL, cree una nueva clase denominada **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="d2d3c-132">Contendrá todas las operaciones de datos que residían originalmente en los archivos de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="d2d3c-133">Los métodos son prácticamente los mismos que los métodos en el archivo de código subyacente, pero incluyen algunos cambios.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="d2d3c-134">El cambio más importante a tener en cuenta es que ya no se ejecuta el código desde una instancia de **página** clase.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="d2d3c-135">La clase de página contiene el **TryUpdateModel** método y el **ModelState** propiedad.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="d2d3c-136">Cuando este código se traslada a una capa de lógica empresarial, ya no tiene una instancia de la clase de página para llamar a estos miembros.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="d2d3c-137">Para solucionar este problema, debe agregar un **ModelMethodContext** parámetro a cualquier método que tiene acceso a TryUpdateModel o ModelState.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="d2d3c-138">Utilice este parámetro ModelMethodContext para llamar a TryUpdateModel o recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="d2d3c-139">No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="d2d3c-140">Reemplace el código en SchoolBL.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="d2d3c-141">Revise las páginas existentes para recuperar datos de la capa de lógica empresarial</span><span class="sxs-lookup"><span data-stu-id="d2d3c-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="d2d3c-142">Por último, convertirá las páginas Students.aspx, AddStudent.aspx y Courses.aspx del uso de las consultas en el archivo de código subyacente al uso de la capa de lógica empresarial.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="d2d3c-143">En los archivos de código subyacente para estudiantes, AddStudent y cursos, elimine o comente los siguientes métodos de consulta:</span><span class="sxs-lookup"><span data-stu-id="d2d3c-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="d2d3c-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="d2d3c-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="d2d3c-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="d2d3c-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="d2d3c-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="d2d3c-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="d2d3c-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="d2d3c-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="d2d3c-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="d2d3c-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="d2d3c-149">Ahora no debería tener ningún código en el archivo de código subyacente que pertenecen a las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="d2d3c-150">El **OnCallingDataMethods** controlador de eventos le permite especificar un objeto que se usará para los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="d2d3c-151">En Students.aspx, agregue un valor para ese controlador de eventos y cambiar los nombres de los métodos de datos a los nombres de los métodos de la clase de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="d2d3c-152">En el archivo de código subyacente para Students.aspx, definir el controlador de eventos para el evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="d2d3c-153">En este controlador de eventos, especifique la clase de lógica de negocios para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="d2d3c-154">En AddStudent.aspx, realizar modificaciones similares.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="d2d3c-155">En Courses.aspx, realizar modificaciones similares.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="d2d3c-156">Ejecute la aplicación y observe que todas las páginas funcionan que tenían anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="d2d3c-157">La lógica de validación también funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d2d3c-158">Conclusión</span><span class="sxs-lookup"><span data-stu-id="d2d3c-158">Conclusion</span></span>

<span data-ttu-id="d2d3c-159">En este tutorial, estructurado volver a la aplicación para usar una capa de acceso a datos y la capa de lógica empresarial.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="d2d3c-160">Especifica que los controles de datos utilizan un objeto que no es la página actual para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d2d3c-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d2d3c-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="d2d3c-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
