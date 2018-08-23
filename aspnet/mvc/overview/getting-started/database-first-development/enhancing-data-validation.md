---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First con MVC de ASP.NET: mejora de la validación de datos | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: dbff33a1c4f47474adda82e796d9c292392142f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835960"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="3b798-104">EF Database First con MVC de ASP.NET: mejora de la validación de datos</span><span class="sxs-lookup"><span data-stu-id="3b798-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="3b798-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3b798-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3b798-106">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3b798-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="3b798-107">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b798-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="3b798-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b798-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="3b798-109">Esta parte de la serie se centra en Agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y formato para mostrar.</span><span class="sxs-lookup"><span data-stu-id="3b798-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="3b798-110">Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.</span><span class="sxs-lookup"><span data-stu-id="3b798-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="3b798-111">Agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="3b798-111">Add data annotations</span></span>

<span data-ttu-id="3b798-112">Como se vio en un tema anterior, algunas reglas de validación de datos se aplican automáticamente a la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="3b798-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="3b798-113">Por ejemplo, solo puede proporcionar un número para la propiedad de nivel.</span><span class="sxs-lookup"><span data-stu-id="3b798-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="3b798-114">Para especificar más de las reglas de validación de datos, puede agregar anotaciones de datos a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="3b798-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="3b798-115">Estas anotaciones se aplican en toda la aplicación web para la propiedad especificada.</span><span class="sxs-lookup"><span data-stu-id="3b798-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="3b798-116">También puede aplicar atributos de formato que cambiar cómo se muestran las propiedades; el valor utilizado para las etiquetas de texto, como el cambio.</span><span class="sxs-lookup"><span data-stu-id="3b798-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="3b798-117">En este tutorial, agregará las anotaciones de datos para restringir la longitud de los valores proporcionados para las propiedades FirstName, LastName y MiddleName.</span><span class="sxs-lookup"><span data-stu-id="3b798-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="3b798-118">En la base de datos, estos valores se limitan a 50 caracteres. Sin embargo, en la aplicación web ese límite de caracteres actualmente no se aplica.</span><span class="sxs-lookup"><span data-stu-id="3b798-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="3b798-119">Si un usuario proporciona más de 50 caracteres para uno de esos valores, la página se bloqueará al intentar guardar el valor en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b798-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="3b798-120">También limitará el grado de valores entre 0 y 4.</span><span class="sxs-lookup"><span data-stu-id="3b798-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="3b798-121">Abra el **Student.cs** de archivos en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="3b798-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="3b798-122">Agregue el código resaltado siguiente a la clase.</span><span class="sxs-lookup"><span data-stu-id="3b798-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="3b798-123">En Enrollment.cs, agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="3b798-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="3b798-124">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="3b798-124">Build the solution.</span></span>

<span data-ttu-id="3b798-125">Vaya a la página de edición o creación de un alumno.</span><span class="sxs-lookup"><span data-stu-id="3b798-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="3b798-126">Si se intenta escribir más de 50 caracteres, se muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="3b798-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Mostrar mensaje de error](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="3b798-128">Vaya a la página para editar las inscripciones e intenta proporcionar un nivel por encima de 4.</span><span class="sxs-lookup"><span data-stu-id="3b798-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![error de intervalo de nivel](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="3b798-130">Para obtener una lista completa de las anotaciones de validación de datos se puede aplicar a las clases y propiedades, vea [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b798-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="3b798-131">Agregar clases de metadatos</span><span class="sxs-lookup"><span data-stu-id="3b798-131">Add metadata classes</span></span>

<span data-ttu-id="3b798-132">Agregar los atributos de validación directamente a la clase del modelo funciona cuando no tiene previsto cambiar; la base de datos Sin embargo, si los cambios de la base de datos y necesita volver a generar la clase del modelo, perderá todos los atributos que se había aplicado a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="3b798-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="3b798-133">Este enfoque puede ser muy ineficaz y propenso a la pérdida de las reglas de validación importante.</span><span class="sxs-lookup"><span data-stu-id="3b798-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="3b798-134">Para evitar este problema, puede agregar una clase de metadatos que contiene los atributos.</span><span class="sxs-lookup"><span data-stu-id="3b798-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="3b798-135">Al asociar la clase del modelo a la clase de metadatos, esos atributos se aplican al modelo.</span><span class="sxs-lookup"><span data-stu-id="3b798-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="3b798-136">En este enfoque, puede volver a generar la clase del modelo sin perder todos los atributos que se han aplicado a la clase de metadatos.</span><span class="sxs-lookup"><span data-stu-id="3b798-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="3b798-137">En el **modelos** carpeta, agregue una clase denominada **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="3b798-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Agregar clase de metadatos](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="3b798-139">Reemplace el código en Metadata.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3b798-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="3b798-140">Estas clases de metadatos contienen todos los atributos de validación que había aplicado anteriormente a las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="3b798-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="3b798-141">El **mostrar** atributo se utiliza para cambiar el valor utilizado para las etiquetas de texto.</span><span class="sxs-lookup"><span data-stu-id="3b798-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="3b798-142">Ahora, debe asociar las clases del modelo con las clases de metadatos.</span><span class="sxs-lookup"><span data-stu-id="3b798-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="3b798-143">En el **modelos** carpeta, agregue una clase denominada **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="3b798-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="3b798-144">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3b798-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="3b798-145">Tenga en cuenta que cada clase está marcada como un `partial` clase y cada uno de ellos coincide con el nombre y espacio de nombres como la clase que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3b798-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="3b798-146">Al aplicar el atributo de metadatos a la clase parcial, se asegura que se aplicará a la clase genera automáticamente los atributos de validación de datos.</span><span class="sxs-lookup"><span data-stu-id="3b798-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="3b798-147">Estos atributos no se perderán al volver a generar las clases del modelo porque se aplica el atributo de metadatos en clases parciales que no se vuelven a generar.</span><span class="sxs-lookup"><span data-stu-id="3b798-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="3b798-148">Para volver a generar las clases generadas automáticamente, abra el archivo ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="3b798-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="3b798-149">Una vez más, haga doble clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="3b798-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="3b798-150">Aunque no ha cambiado la base de datos, este proceso volverá a generar las clases.</span><span class="sxs-lookup"><span data-stu-id="3b798-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="3b798-151">En el **actualizar** ficha, seleccione **tablas** y **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="3b798-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![actualizar tablas](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="3b798-153">Guarde el archivo ContosoModel.edmx para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="3b798-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="3b798-154">Abra el archivo Student.cs o Enrollment.cs y tenga en cuenta que los atributos de validación de datos que aplicó anteriormente ya no están en el archivo.</span><span class="sxs-lookup"><span data-stu-id="3b798-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="3b798-155">Sin embargo, ejecute la aplicación y observe que todavía se aplican las reglas de validación al escribir datos.</span><span class="sxs-lookup"><span data-stu-id="3b798-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3b798-156">[Anterior](customizing-a-view.md)
> [Siguiente](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="3b798-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
