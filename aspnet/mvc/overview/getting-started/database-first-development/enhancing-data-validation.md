---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutorial: Mejorar la validación de datos para EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en Agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y formato para mostrar.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236489"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="7aab9-103">Tutorial: Mejorar la validación de datos para EF Database First con la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7aab9-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="7aab9-104">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="7aab9-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7aab9-105">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7aab9-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="7aab9-107">En este tutorial se centra en Agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y formato para mostrar.</span><span class="sxs-lookup"><span data-stu-id="7aab9-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="7aab9-108">Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.</span><span class="sxs-lookup"><span data-stu-id="7aab9-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="7aab9-109">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="7aab9-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7aab9-110">Agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="7aab9-110">Add data annotations</span></span>
> * <span data-ttu-id="7aab9-111">Agregar clases de metadatos</span><span class="sxs-lookup"><span data-stu-id="7aab9-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7aab9-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7aab9-112">Prerequisites</span></span>

* [<span data-ttu-id="7aab9-113">Personalizar una vista</span><span class="sxs-lookup"><span data-stu-id="7aab9-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="7aab9-114">Agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="7aab9-114">Add data annotations</span></span>

<span data-ttu-id="7aab9-115">Como se vio en un tema anterior, algunas reglas de validación de datos se aplican automáticamente a la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7aab9-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="7aab9-116">Por ejemplo, solo puede proporcionar un número para la propiedad de nivel.</span><span class="sxs-lookup"><span data-stu-id="7aab9-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="7aab9-117">Para especificar más de las reglas de validación de datos, puede agregar anotaciones de datos a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="7aab9-118">Estas anotaciones se aplican en toda la aplicación web para la propiedad especificada.</span><span class="sxs-lookup"><span data-stu-id="7aab9-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="7aab9-119">También puede aplicar atributos de formato que cambiar cómo se muestran las propiedades; el valor utilizado para las etiquetas de texto, como el cambio.</span><span class="sxs-lookup"><span data-stu-id="7aab9-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="7aab9-120">En este tutorial, agregará las anotaciones de datos para restringir la longitud de los valores proporcionados para las propiedades FirstName, LastName y MiddleName.</span><span class="sxs-lookup"><span data-stu-id="7aab9-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="7aab9-121">En la base de datos, estos valores se limitan a 50 caracteres. Sin embargo, en la aplicación web ese límite de caracteres actualmente no se aplica.</span><span class="sxs-lookup"><span data-stu-id="7aab9-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="7aab9-122">Si un usuario proporciona más de 50 caracteres para uno de esos valores, la página se bloqueará al intentar guardar el valor en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="7aab9-123">También limitará el grado de valores entre 0 y 4.</span><span class="sxs-lookup"><span data-stu-id="7aab9-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="7aab9-124">Seleccione **modelos** > **ContosoModel.edmx** > **ContosoModel.tt** y abra el *Student.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="7aab9-125">Agregue el código resaltado siguiente a la clase.</span><span class="sxs-lookup"><span data-stu-id="7aab9-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="7aab9-126">Abra *Enrollment.cs* y agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="7aab9-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="7aab9-127">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="7aab9-127">Build the solution.</span></span>

<span data-ttu-id="7aab9-128">Haga clic en **lista de alumnos** y seleccione **editar**.</span><span class="sxs-lookup"><span data-stu-id="7aab9-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="7aab9-129">Si se intenta escribir más de 50 caracteres, se muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="7aab9-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Mostrar mensaje de error](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="7aab9-131">Vuelva a la página principal.</span><span class="sxs-lookup"><span data-stu-id="7aab9-131">Go back to the home page.</span></span> <span data-ttu-id="7aab9-132">Haga clic en **lista de inscripciones de** y seleccione **editar**.</span><span class="sxs-lookup"><span data-stu-id="7aab9-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="7aab9-133">Intenta proporcionar un nivel por encima de 4.</span><span class="sxs-lookup"><span data-stu-id="7aab9-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="7aab9-134">Recibirá este error: *El campo que categoría debe estar comprendido entre 0 y 4.*</span><span class="sxs-lookup"><span data-stu-id="7aab9-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="7aab9-135">Agregar clases de metadatos</span><span class="sxs-lookup"><span data-stu-id="7aab9-135">Add metadata classes</span></span>

<span data-ttu-id="7aab9-136">Agregar los atributos de validación directamente a la clase del modelo funciona cuando no tiene previsto cambiar; la base de datos Sin embargo, si los cambios de la base de datos y necesita volver a generar la clase del modelo, perderá todos los atributos que se había aplicado a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="7aab9-137">Este enfoque puede ser muy ineficaz y propenso a la pérdida de las reglas de validación importante.</span><span class="sxs-lookup"><span data-stu-id="7aab9-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="7aab9-138">Para evitar este problema, puede agregar una clase de metadatos que contiene los atributos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="7aab9-139">Al asociar la clase del modelo a la clase de metadatos, esos atributos se aplican al modelo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="7aab9-140">En este enfoque, puede volver a generar la clase del modelo sin perder todos los atributos que se han aplicado a la clase de metadatos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="7aab9-141">En el **modelos** carpeta, agregue una clase denominada *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="7aab9-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="7aab9-142">Reemplace el código de *Metadata.cs* con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7aab9-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="7aab9-143">Estas clases de metadatos contienen todos los atributos de validación que había aplicado anteriormente a las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="7aab9-144">El **mostrar** atributo se utiliza para cambiar el valor utilizado para las etiquetas de texto.</span><span class="sxs-lookup"><span data-stu-id="7aab9-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="7aab9-145">Ahora, debe asociar las clases del modelo con las clases de metadatos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="7aab9-146">En el **modelos** carpeta, agregue una clase denominada *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="7aab9-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="7aab9-147">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7aab9-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="7aab9-148">Tenga en cuenta que cada clase está marcada como un `partial` clase y cada uno de ellos coincide con el nombre y espacio de nombres como la clase que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7aab9-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="7aab9-149">Al aplicar el atributo de metadatos a la clase parcial, se asegura que se aplicará a la clase genera automáticamente los atributos de validación de datos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="7aab9-150">Estos atributos no se perderán al volver a generar las clases del modelo porque se aplica el atributo de metadatos en clases parciales que no se vuelven a generar.</span><span class="sxs-lookup"><span data-stu-id="7aab9-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="7aab9-151">Para volver a generar las clases generadas automáticamente, abra el *ContosoModel.edmx* archivo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="7aab9-152">Una vez más, haga doble clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="7aab9-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="7aab9-153">Aunque no ha cambiado la base de datos, este proceso volverá a generar las clases.</span><span class="sxs-lookup"><span data-stu-id="7aab9-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="7aab9-154">En el **actualizar** ficha, seleccione **tablas** y **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="7aab9-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="7aab9-155">Guardar el *ContosoModel.edmx* archivo para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="7aab9-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="7aab9-156">Abra el *Student.cs* archivo o la *Enrollment.cs* archivo y tenga en cuenta que los atributos de validación de datos que aplicó anteriormente ya no están en el archivo.</span><span class="sxs-lookup"><span data-stu-id="7aab9-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="7aab9-157">Sin embargo, ejecute la aplicación y observe que todavía se aplican las reglas de validación al escribir datos.</span><span class="sxs-lookup"><span data-stu-id="7aab9-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7aab9-158">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7aab9-158">Additional resources</span></span>

<span data-ttu-id="7aab9-159">Para obtener una lista completa de las anotaciones de validación de datos se puede aplicar a las clases y propiedades, vea [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="7aab9-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7aab9-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7aab9-160">Next steps</span></span>

<span data-ttu-id="7aab9-161">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="7aab9-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7aab9-162">Anotaciones de datos agregados</span><span class="sxs-lookup"><span data-stu-id="7aab9-162">Added data annotations</span></span>
> * <span data-ttu-id="7aab9-163">Clases de metadatos agregados</span><span class="sxs-lookup"><span data-stu-id="7aab9-163">Added metadata classes</span></span>

<span data-ttu-id="7aab9-164">En el siguiente tutorial para aprender a publicar la aplicación web y la base de datos en Azure.</span><span class="sxs-lookup"><span data-stu-id="7aab9-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7aab9-165">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="7aab9-165">Publish to Azure</span></span>](publish-to-azure.md)