---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrar Datepicker de JQuery UI con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: fc278575615e63af2fc7284e51814cf318165110
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807582"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="3b2c9-104">Integrar Datepicker de JQuery UI con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="3b2c9-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="3b2c9-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3b2c9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3b2c9-106">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="3b2c9-107">Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="3b2c9-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="3b2c9-108">Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="3b2c9-109">Este tutorial muestra cómo agregar el JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) a un formulario Web Forms y el modelo de uso de enlace para actualizar la base de datos con el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="3b2c9-110">Este tutorial se basa en el proyecto creado en el [primera](retrieving-data.md) y [segundo](updating-deleting-and-creating-data.md) partes de la serie.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="3b2c9-111">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="3b2c9-112">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="3b2c9-113">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="3b2c9-114">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="3b2c9-114">What you'll build</span></span>

<span data-ttu-id="3b2c9-115">En este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="3b2c9-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="3b2c9-116">Agregar una propiedad al modelo para registrar la fecha de inscripción de student</span><span class="sxs-lookup"><span data-stu-id="3b2c9-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="3b2c9-117">Permitir que el usuario seleccionar la fecha de inscripción con el widget Datepicker de JQuery UI</span><span class="sxs-lookup"><span data-stu-id="3b2c9-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="3b2c9-118">Exigir reglas de validación para la fecha de inscripción</span><span class="sxs-lookup"><span data-stu-id="3b2c9-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="3b2c9-119">El widget Datepicker de JQuery UI permite a los usuarios seleccionar una fecha fácilmente de un calendario emergente cuando el usuario interactúa con el campo.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="3b2c9-120">Uso de este widget puede ser más conveniente para los usuarios que escribir manualmente una fecha.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="3b2c9-121">Integrar el widget Datepicker en una página que usa el enlace de modelos para las operaciones de datos requiere sólo una pequeña cantidad de trabajo adicional.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="3b2c9-122">Agregar una nueva propiedad al modelo</span><span class="sxs-lookup"><span data-stu-id="3b2c9-122">Add a new property to the model</span></span>

<span data-ttu-id="3b2c9-123">En primer lugar, agregará un **Datetime** propiedad a los estudiantes de modelo y migrar ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="3b2c9-124">Abra **UniversityModels.cs**y agregue el código resaltado para el modelo Student.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="3b2c9-125">El **RangeAttribute** se incluye para exigir reglas de validación para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="3b2c9-126">Para este tutorial, asumiremos que Contoso University se fundó en 1 de enero de 2013 y, por tanto, las fechas de inscripción anteriores no son válidas.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="3b2c9-127">En la ventana de administración de paquetes, agregue una migración, ejecute el comando **AddEnrollmentDate migración agregar**.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="3b2c9-128">Tenga en cuenta que el código de la migración agrega la nueva columna de fecha y hora a la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="3b2c9-129">Para que coincida con el valor especificado en el RangeAttribute, agregue un valor predeterminado para la nueva columna, tal como se muestra en el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="3b2c9-130">Guarde los cambios en el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-130">Save your change to the migration file.</span></span>

<span data-ttu-id="3b2c9-131">No es necesario inicializar nuevamente los datos.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-131">You do not need to seed the data again.</span></span> <span data-ttu-id="3b2c9-132">Por lo tanto, abra **Configuration.cs** en la carpeta de migraciones y quitar o marque como comentario el código en el **inicialización** método.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="3b2c9-133">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-133">Save and close the file.</span></span>

<span data-ttu-id="3b2c9-134">Ahora, ejecute el comando **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="3b2c9-135">Tenga en cuenta que la columna ya existe en la base de datos y todos los registros existentes tienen el valor predeterminado para EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="3b2c9-136">Agregar controles dinámicos para la fecha de inscripción</span><span class="sxs-lookup"><span data-stu-id="3b2c9-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="3b2c9-137">Ahora, agregará controles para mostrar y editar la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="3b2c9-138">En este momento, se edita el valor a través de un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="3b2c9-139">Más adelante en el tutorial, cambiará el cuadro de texto para el widget Datepicker de JQuery.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="3b2c9-140">En primer lugar, es importante tener en cuenta que no es necesario realizar ningún cambio a la **AddStudent.aspx** archivo.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="3b2c9-141">El control DynamicEntity representará de forma automática la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="3b2c9-142">Abra **Students.aspx**y agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="3b2c9-143">Ejecute la aplicación y tenga en cuenta que puede establecer el valor de la fecha de inscripción, escriba una fecha.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="3b2c9-144">Al agregar un nuevo alumno:</span><span class="sxs-lookup"><span data-stu-id="3b2c9-144">When adding a new student:</span></span>

![Configurar fecha](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="3b2c9-146">O bien, editar un valor existente:</span><span class="sxs-lookup"><span data-stu-id="3b2c9-146">Or, editing an existing value:</span></span>

![Editar fecha](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="3b2c9-148">Escribir las obras de fecha, pero podría no ser que desee proporcionar la experiencia del cliente.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="3b2c9-149">En la siguiente sección, permitirá seleccionar una fecha mediante un calendario.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="3b2c9-150">Instalar el paquete de NuGet para trabajar con JQuery UI</span><span class="sxs-lookup"><span data-stu-id="3b2c9-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="3b2c9-151">El **la interfaz de usuario del zumo** paquete NuGet permite una integración fácil de los widgets de JQuery UI en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="3b2c9-152">Para usar este paquete, instalarlo a través de NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-152">To use this package, install it through NuGet.</span></span>

![agregar la interfaz de usuario del zumo](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="3b2c9-154">La versión de interfaz de usuario del zumo que instale podría entrar en conflicto con la versión de JQuery en su aplicación.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="3b2c9-155">Antes de continuar con este tutorial, pruebe a ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="3b2c9-156">Si se produce un error de JavaScript, debe reconciliar la versión de JQuery.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="3b2c9-157">Puede agregar la versión esperada de JQuery en su carpeta de Scripts (versión 1.8.2 en momento de redactar este tutorial), o en Site.master, especifique la ruta de acceso al archivo de JQuery.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="3b2c9-158">Personalizar la plantilla de fecha y hora para incluir el widget Datepicker</span><span class="sxs-lookup"><span data-stu-id="3b2c9-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="3b2c9-159">El widget Datepicker se agregará a la plantilla de datos dinámicos para editar un valor de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="3b2c9-160">Al agregar el widget a la plantilla, automáticamente se representa en forma de agregar un nuevo alumno y en la vista de cuadrícula para estudiantes de edición.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="3b2c9-161">Abra **DateTime\_Edit.ascx**y agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="3b2c9-162">En el archivo de código subyacente, establecerá las fechas mínimas y máximos para el objeto DatePicker.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="3b2c9-163">Al establecer estos valores, se impide que los usuarios navegar a las fechas no válidas.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="3b2c9-164">Se recuperarán los valores mínimos y máximo de la **RangeAttribute** en la propiedad de fecha y hora, si se proporciona uno.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="3b2c9-165">Abra **DateTime\_Edit.ascx.cs**y agregue el código resaltado siguiente a la página\_Load (método).</span><span class="sxs-lookup"><span data-stu-id="3b2c9-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="3b2c9-166">Ejecute la aplicación web y vaya a la página AddStudent.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="3b2c9-167">Proporcione valores para los campos y tenga en cuenta que al hacer clic en el cuadro de texto para la fecha de inscripción, se muestra el calendario.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Selector de fecha](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="3b2c9-169">Seleccionar una fecha y haga clic en **insertar**.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="3b2c9-170">El RangeAttribute exige la validación en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="3b2c9-171">Al establecer la propiedad minDate en Datepicker, también se aplican validación en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="3b2c9-172">El calendario no permite que el usuario navegue a una fecha anterior al valor de minDate.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="3b2c9-173">Cuando se edita un registro en la vista de cuadrícula, también se muestra el calendario.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker en GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="3b2c9-175">Conclusión</span><span class="sxs-lookup"><span data-stu-id="3b2c9-175">Conclusion</span></span>

<span data-ttu-id="3b2c9-176">En este tutorial, ha aprendido cómo incorporar un widget de JQuery en un formulario web Forms que usa el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="3b2c9-177">En los próximos [tutorial](using-query-string-values-to-retrieve-data.md), usará un valor de cadena de consulta al seleccionar datos.</span><span class="sxs-lookup"><span data-stu-id="3b2c9-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3b2c9-178">[Anterior](sorting-paging-and-filtering-data.md)
> [Siguiente](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="3b2c9-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
