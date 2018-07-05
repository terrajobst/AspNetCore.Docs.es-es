---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Actualizar, eliminar y crear datos con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: b6cafe29d1cb46061a8743cbee62a7ffec6be990
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393881"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="91a2a-104">Actualizar, eliminar y crear datos con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="91a2a-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="91a2a-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="91a2a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="91a2a-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="91a2a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="91a2a-107">Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="91a2a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="91a2a-108">Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="91a2a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="91a2a-109">Este tutorial muestra cómo crear, actualizar y eliminar datos con enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="91a2a-110">Establecerá las propiedades siguientes:</span><span class="sxs-lookup"><span data-stu-id="91a2a-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="91a2a-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="91a2a-111">DeleteMethod</span></span>
> - <span data-ttu-id="91a2a-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="91a2a-112">InsertMethod</span></span>
> - <span data-ttu-id="91a2a-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="91a2a-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="91a2a-114">Estas propiedades reciben el nombre del método que controla la operación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="91a2a-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="91a2a-115">Dentro de ese método, debe proporcionar la lógica para interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="91a2a-116">Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="91a2a-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="91a2a-117">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="91a2a-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="91a2a-118">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="91a2a-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="91a2a-119">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="91a2a-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="91a2a-120">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="91a2a-120">What you'll build</span></span>

<span data-ttu-id="91a2a-121">En este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="91a2a-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="91a2a-122">Agregar plantillas de datos dinámicos</span><span class="sxs-lookup"><span data-stu-id="91a2a-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="91a2a-123">Habilitar la actualización y eliminación de datos a través de los métodos de enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="91a2a-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="91a2a-124">Aplicar reglas de validación de datos: habilitación de la creación de un nuevo registro en la base de datos</span><span class="sxs-lookup"><span data-stu-id="91a2a-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="91a2a-125">Agregar plantillas de datos dinámicos</span><span class="sxs-lookup"><span data-stu-id="91a2a-125">Add dynamic data templates</span></span>

<span data-ttu-id="91a2a-126">Para ofrecer la mejor experiencia de usuario y reducir la repetición del código, deberá usar plantillas de datos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="91a2a-127">Puede integrar fácilmente las plantillas precompiladas datos dinámicos en el sitio existente al instalar un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="91a2a-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="91a2a-128">Desde el **administrar paquetes de NuGet**, instale el **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="91a2a-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![plantillas de datos dinámicos](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="91a2a-130">Tenga en cuenta que ahora el proyecto incluye una carpeta denominada **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="91a2a-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="91a2a-131">En esa carpeta, encontrará las plantillas que se aplican automáticamente a los controles dinámicos en los formularios web forms.</span><span class="sxs-lookup"><span data-stu-id="91a2a-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![carpeta de datos dinámicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="91a2a-133">Habilitar actualización y eliminación</span><span class="sxs-lookup"><span data-stu-id="91a2a-133">Enable updating and deleting</span></span>

<span data-ttu-id="91a2a-134">Permitir a los usuarios actualizar y eliminar registros en la base de datos es muy similar al proceso de recuperación de datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="91a2a-135">En el **UpdateMethod** y **DeleteMethod** propiedades, especifique los nombres de los métodos que realizan esas operaciones.</span><span class="sxs-lookup"><span data-stu-id="91a2a-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="91a2a-136">Con un control GridView, también puede especificar la generación automática de editar y eliminar botones.</span><span class="sxs-lookup"><span data-stu-id="91a2a-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="91a2a-137">El código resaltado siguiente muestra las adiciones al código de GridView.</span><span class="sxs-lookup"><span data-stu-id="91a2a-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="91a2a-138">En el archivo de código subyacente, agregue una mediante declaración para **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="91a2a-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="91a2a-139">A continuación, agregue la siguiente actualización y elimina métodos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="91a2a-140">El **TryUpdateModel** método aplica los valores enlazados a datos coincidentes del formulario web Forms al elemento de datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="91a2a-141">El elemento de datos se recupera en función del valor del parámetro del identificador.</span><span class="sxs-lookup"><span data-stu-id="91a2a-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="91a2a-142">Exigir los requisitos de validación</span><span class="sxs-lookup"><span data-stu-id="91a2a-142">Enforce validation requirements</span></span>

<span data-ttu-id="91a2a-143">Los atributos de validación que aplican a las propiedades FirstName, LastName y año en la clase Student se aplican automáticamente al actualizar los datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="91a2a-144">Los controles de DynamicField agregan validadores de cliente y servidor según los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="91a2a-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="91a2a-145">Las propiedades FirstName y LastName son necesarios.</span><span class="sxs-lookup"><span data-stu-id="91a2a-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="91a2a-146">FirstName no puede superar los 20 caracteres de longitud y LastName no puede superar los 40 caracteres.</span><span class="sxs-lookup"><span data-stu-id="91a2a-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="91a2a-147">Año debe ser un valor válido para la enumeración AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="91a2a-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="91a2a-148">Si el usuario infringe uno de los requisitos de validación, no continúe la actualización.</span><span class="sxs-lookup"><span data-stu-id="91a2a-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="91a2a-149">Para ver el mensaje de error, agregue un control ValidationSummary por encima del control GridView.</span><span class="sxs-lookup"><span data-stu-id="91a2a-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="91a2a-150">Para mostrar los errores de validación del enlace de modelos, establezca el **ShowModelStateErrors** propiedad establecida en **true**.</span><span class="sxs-lookup"><span data-stu-id="91a2a-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="91a2a-151">Ejecute la aplicación web y actualizar y eliminar cualquiera de los registros.</span><span class="sxs-lookup"><span data-stu-id="91a2a-151">Run the web application, and update and delete any of the records.</span></span>

![actualizar datos](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="91a2a-153">Tenga en cuenta que, cuando en el modo de edición, el valor de la propiedad año automáticamente se representa como una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="91a2a-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="91a2a-154">La propiedad de año es un valor de enumeración y la plantilla de datos dinámicos para un valor de enumeración especifica una lista desplegable de edición.</span><span class="sxs-lookup"><span data-stu-id="91a2a-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="91a2a-155">Puede encontrar esa plantilla abriendo el **enumeración\_Edit.ascx** de archivos en el **DynamicData**/**FieldTemplates** carpeta.</span><span class="sxs-lookup"><span data-stu-id="91a2a-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="91a2a-156">Si proporciona los valores válidos, la actualización se complete correctamente.</span><span class="sxs-lookup"><span data-stu-id="91a2a-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="91a2a-157">Si se infringe uno de los requisitos de validación, la actualización no continúa y se muestra un mensaje de error encima de la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="91a2a-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![mensaje de error](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="91a2a-159">Agregar nuevos registros</span><span class="sxs-lookup"><span data-stu-id="91a2a-159">Add new records</span></span>

<span data-ttu-id="91a2a-160">El control GridView no incluye el **InsertMethod** propiedad y, por tanto, no se puede usar para agregar un nuevo registro con el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="91a2a-161">Puede encontrar la propiedad InsertMethod en el **FormView**, **DetailsView**, o **ListView** controles.</span><span class="sxs-lookup"><span data-stu-id="91a2a-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="91a2a-162">En este tutorial, usará un control FormView para agregar un nuevo registro.</span><span class="sxs-lookup"><span data-stu-id="91a2a-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="91a2a-163">En primer lugar, agregue un vínculo a la nueva página que se va a crear para agregar un nuevo registro.</span><span class="sxs-lookup"><span data-stu-id="91a2a-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="91a2a-164">Sobre el control ValidationSummary, agregue:</span><span class="sxs-lookup"><span data-stu-id="91a2a-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="91a2a-165">El nuevo vínculo aparecerá en la parte superior del contenido de la página Students.</span><span class="sxs-lookup"><span data-stu-id="91a2a-165">The new link will appear at the top of the content for the Students page.</span></span>

![nuevo vínculo](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="91a2a-167">A continuación, agregue un nuevo formulario web con una página maestra y asígnele el nombre **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="91a2a-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="91a2a-168">Seleccione Site.Master como la página maestra.</span><span class="sxs-lookup"><span data-stu-id="91a2a-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="91a2a-169">Se representarán los campos para agregar un nuevo alumno mediante un **DynamicEntity** control.</span><span class="sxs-lookup"><span data-stu-id="91a2a-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="91a2a-170">El control de DynamicEntity representa esa propiedades editables de la clase especificada en la propiedad de tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="91a2a-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="91a2a-171">La propiedad StudentID se marcó con el **[scaffoldcolumn (false)]** atributo para que no se represente.</span><span class="sxs-lookup"><span data-stu-id="91a2a-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="91a2a-172">En el marcador de posición MainContent de la página AddStudent, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="91a2a-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="91a2a-173">En el archivo de código subyacente (AddStudent.aspx.cs), agregue un **mediante** instrucción para el **ContosoUniversityModelBinding.Models** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="91a2a-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="91a2a-174">A continuación, agregue los métodos siguientes para especificar cómo insertar un nuevo registro y un controlador de eventos para el botón Cancelar.</span><span class="sxs-lookup"><span data-stu-id="91a2a-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="91a2a-175">Guarde todos los cambios.</span><span class="sxs-lookup"><span data-stu-id="91a2a-175">Save all of the changes.</span></span>

<span data-ttu-id="91a2a-176">Ejecutar la aplicación web y crear un nuevo alumno.</span><span class="sxs-lookup"><span data-stu-id="91a2a-176">Run the web application and create a new student.</span></span>

![Agregar nuevo alumno](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="91a2a-178">Haga clic en **insertar** y observe el alumno nuevo se ha creado.</span><span class="sxs-lookup"><span data-stu-id="91a2a-178">Click **Insert** and notice the new student has been created.</span></span>

![pantalla nuevo alumno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="91a2a-180">Conclusión</span><span class="sxs-lookup"><span data-stu-id="91a2a-180">Conclusion</span></span>

<span data-ttu-id="91a2a-181">En este tutorial, ha habilitado actualizar, eliminar y crear datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="91a2a-182">Se garantiza que se aplican las reglas de validación al interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="91a2a-183">En los próximos [tutorial](sorting-paging-and-filtering-data.md) en esta serie, permitirá ordenar, paginar y filtrar datos.</span><span class="sxs-lookup"><span data-stu-id="91a2a-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="91a2a-184">[Anterior](retrieving-data.md)
> [Siguiente](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="91a2a-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
