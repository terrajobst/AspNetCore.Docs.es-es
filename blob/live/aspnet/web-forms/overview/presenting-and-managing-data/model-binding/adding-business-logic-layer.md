---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms | Documentos de Microsoft"
author: tfitzmac
description: "Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="f1626-104">Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="f1626-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="f1626-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f1626-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f1626-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1626-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="f1626-107">Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="f1626-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="f1626-108">Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="f1626-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="f1626-109">Este tutorial muestra cómo utilizar el enlace de modelos a una capa de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="f1626-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="f1626-110">Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="f1626-111">Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.</span><span class="sxs-lookup"><span data-stu-id="f1626-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="f1626-112">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="f1626-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="f1626-113">El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f1626-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="f1626-114">Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="f1626-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f1626-115">Lo que vamos a compilar</span><span class="sxs-lookup"><span data-stu-id="f1626-115">What you'll build</span></span>

<span data-ttu-id="f1626-116">Enlace de modelos le permite colocar el código de interacción de datos en el archivo de código subyacente para una página web o en una clase de lógica de negocios independientes.</span><span class="sxs-lookup"><span data-stu-id="f1626-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="f1626-117">Los tutoriales anteriores han demostrado cómo usar los archivos de código subyacente para el código de interacción de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="f1626-118">Este enfoque funciona para sitios pequeños, pero se pueden producir en código repetición y mayor dificultad al mantener un sitio grande.</span><span class="sxs-lookup"><span data-stu-id="f1626-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="f1626-119">También puede ser muy difícil probar mediante programación el código que se encuentra en archivos de código subyacente porque no hay ninguna capa de abstracción.</span><span class="sxs-lookup"><span data-stu-id="f1626-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="f1626-120">Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contiene toda la lógica para interactuar con datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="f1626-121">A continuación, llamar a la capa de lógica de negocios desde las páginas web.</span><span class="sxs-lookup"><span data-stu-id="f1626-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="f1626-122">Este tutorial muestra cómo mover todo el código que ha escrito en los tutoriales anteriores en una capa de lógica de negocios y, a continuación, usar ese código desde las páginas.</span><span class="sxs-lookup"><span data-stu-id="f1626-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="f1626-123">En este tutorial, deberá:</span><span class="sxs-lookup"><span data-stu-id="f1626-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="f1626-124">Mueva el código de archivos de código subyacente a una capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="f1626-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="f1626-125">Cambiar los controles enlazados a datos para llamar a los métodos en la capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="f1626-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="f1626-126">Crear la capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="f1626-126">Create business logic layer</span></span>

<span data-ttu-id="f1626-127">Ahora, se creará la clase que se llama desde las páginas web.</span><span class="sxs-lookup"><span data-stu-id="f1626-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="f1626-128">Los métodos de esta clase un aspecto similares a los métodos utilizados en los tutoriales anteriores e incluyen los atributos de proveedor de valor.</span><span class="sxs-lookup"><span data-stu-id="f1626-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="f1626-129">En primer lugar, agregue una nueva carpeta denominada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="f1626-129">First, add a new folder called **BLL**.</span></span>

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="f1626-131">En la carpeta BLL, cree una nueva clase denominada **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="f1626-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="f1626-132">Contendrá todas las operaciones de datos que residían originalmente en archivos de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="f1626-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="f1626-133">Los métodos son prácticamente los mismos que los métodos en el archivo de código subyacente, pero incluyen algunos cambios.</span><span class="sxs-lookup"><span data-stu-id="f1626-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="f1626-134">El cambio más importante tener en cuenta es que ya no se está ejecutando el código desde dentro de una instancia de **página** clase.</span><span class="sxs-lookup"><span data-stu-id="f1626-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="f1626-135">La clase de página contiene el **TryUpdateModel** método y **ModelState** propiedad.</span><span class="sxs-lookup"><span data-stu-id="f1626-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="f1626-136">Cuando este código se traslada a una capa de lógica de negocios, ya no tiene una instancia de la clase de página para llamar a estos miembros.</span><span class="sxs-lookup"><span data-stu-id="f1626-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="f1626-137">Para solucionar este problema, debe agregar una **ModelMethodContext** parámetro a cualquier método que tiene acceso a TryUpdateModel o ModelState.</span><span class="sxs-lookup"><span data-stu-id="f1626-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="f1626-138">Este parámetro ModelMethodContext se usa para llamar a TryUpdateModel o recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="f1626-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="f1626-139">No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.</span><span class="sxs-lookup"><span data-stu-id="f1626-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="f1626-140">Reemplace el código en SchoolBL.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f1626-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="f1626-141">Revise las páginas existentes para recuperar datos de capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="f1626-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="f1626-142">Por último, convertirá las páginas Students.aspx, AddStudent.aspx y Courses.aspx del uso de consultas en el archivo de código subyacente al uso de la capa de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="f1626-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="f1626-143">En los archivos de código subyacente para estudiantes, AddStudent y cursos, eliminar o marque como comentario los métodos de consulta siguiente:</span><span class="sxs-lookup"><span data-stu-id="f1626-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="f1626-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="f1626-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="f1626-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="f1626-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="f1626-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="f1626-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="f1626-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="f1626-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="f1626-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="f1626-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="f1626-149">Ahora no debería tener ningún código en el archivo de código subyacente que pertenece a las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="f1626-150">El **OnCallingDataMethods** controlador de eventos le permite especificar un objeto que se usará para los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="f1626-151">En Students.aspx, agregue un valor para ese controlador de eventos y cambiar los nombres de los métodos de datos a los nombres de los métodos de la clase de la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="f1626-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="f1626-152">En el archivo de código subyacente para Students.aspx, definir el controlador de eventos para el evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="f1626-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="f1626-153">En este controlador de eventos, se especifica la clase de la lógica de negocios para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="f1626-154">En AddStudent.aspx, realizar modificaciones similares.</span><span class="sxs-lookup"><span data-stu-id="f1626-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="f1626-155">En Courses.aspx, realizar modificaciones similares.</span><span class="sxs-lookup"><span data-stu-id="f1626-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="f1626-156">Ejecute la aplicación y observe que todas las páginas funcionen como que tenían anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f1626-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="f1626-157">La lógica de validación también funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="f1626-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f1626-158">Conclusión</span><span class="sxs-lookup"><span data-stu-id="f1626-158">Conclusion</span></span>

<span data-ttu-id="f1626-159">En este tutorial, volver a estructurados de su aplicación para que utilice una capa de acceso a datos y la capa de lógica empresarial.</span><span class="sxs-lookup"><span data-stu-id="f1626-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="f1626-160">Especifica que los controles de datos utilizan un objeto que no es la página actual para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="f1626-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f1626-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="f1626-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
