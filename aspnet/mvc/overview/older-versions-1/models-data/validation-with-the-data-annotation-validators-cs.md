---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Validación con los validadores de anotación de datos (C#) | Microsoft Docs
author: microsoft
description: Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC. Obtenga información sobre cómo usar los diferentes tipos de control de validación...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 203e49e67d8a9c6eb9dbf605a8d7d860737de073
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835916"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="d4324-104">Validación con los validadores de anotación de datos (C#)</span><span class="sxs-lookup"><span data-stu-id="d4324-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="d4324-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d4324-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d4324-106">Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d4324-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="d4324-107">Obtenga información sobre cómo usar los diferentes tipos de atributos de validación y trabajar con ellos en Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4324-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="d4324-108">En este tutorial, obtendrá información sobre cómo usar los validadores de anotación de datos para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d4324-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d4324-109">La ventaja de usar los validadores de anotación de datos es que permiten llevar a cabo la validación agregando uno o varios atributos, como los necesarios o atributo StringLength: para una propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="d4324-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="d4324-110">Para poder usar los validadores de anotación de datos, debe descargar el enlazador de modelos de anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d4324-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="d4324-111">Puede descargar el ejemplo de enlazador de modelo de anotaciones de datos desde el sitio Web de CodePlex haciendo [aquí](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="d4324-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="d4324-112">Es importante comprender que el enlazador de modelos de anotaciones de datos no es una parte oficial de Microsoft ASP.NET MVC framework.</span><span class="sxs-lookup"><span data-stu-id="d4324-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="d4324-113">Aunque el enlazador de modelos de anotaciones de datos creado por el equipo de Microsoft ASP.NET MVC, Microsoft no ofrece soporte técnico oficial para el enlazador de modelos de anotaciones de datos se describe y usado en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d4324-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="d4324-114">Usar el enlazador de modelos de anotación de datos</span><span class="sxs-lookup"><span data-stu-id="d4324-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="d4324-115">Para poder usar el enlazador de modelos de anotaciones de datos en una aplicación ASP.NET MVC, primero deberá agregar una referencia al ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y el ensamblado System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="d4324-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="d4324-116">Seleccione la opción de menú **proyecto, agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="d4324-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="d4324-117">A continuación, haga clic en el **examinar** pestaña y vaya a la ubicación donde descargó (y descomprime) en el ejemplo de enlazador de modelos de anotaciones de datos (consulte **figura 1**).</span><span class="sxs-lookup"><span data-stu-id="d4324-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="d4324-118">**Figura 1**: agregar una referencia al enlazador de modelos de anotaciones de datos ([haga clic aquí para ver imagen en tamaño completo](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d4324-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="d4324-119">Seleccione el ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y el ensamblado System.ComponentModel.DataAnnotations.dll y haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="d4324-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="d4324-120">No se puede usar el ensamblado System.ComponentModel.DataAnnotations.dll incluido con .NET Framework Service Pack 1 con el enlazador de modelos de anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d4324-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="d4324-121">Debe usar la versión del ensamblado System.ComponentModel.DataAnnotations.dll incluido con la descarga de ejemplo de enlazador de modelo de anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d4324-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="d4324-122">Por último, deberá registrar el enlazador de modelos DataAnnotations en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="d4324-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="d4324-123">Agregue la siguiente línea de código a la aplicación\_Start() controlador de eventos para que la aplicación\_método Start() tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="d4324-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="d4324-124">Esta línea de código registra la ataAnnotationsModelBinder como el enlazador de modelos predeterminado para toda la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d4324-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="d4324-125">Uso de los atributos de validadores de anotación de datos</span><span class="sxs-lookup"><span data-stu-id="d4324-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="d4324-126">Cuando se usa el enlazador de modelos de anotaciones de datos, utilice los atributos de validación para realizar la validación.</span><span class="sxs-lookup"><span data-stu-id="d4324-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="d4324-127">El espacio de nombres System.ComponentModel.DataAnnotations incluye los siguientes atributos de validación:</span><span class="sxs-lookup"><span data-stu-id="d4324-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="d4324-128">Intervalo: le permite validar si el valor de una propiedad se encuentra entre un intervalo de valores especificado.</span><span class="sxs-lookup"><span data-stu-id="d4324-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="d4324-129">RegularExpression – le permite validar si el valor de una propiedad coincide con un patrón de expresión regular especificada.</span><span class="sxs-lookup"><span data-stu-id="d4324-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="d4324-130">Requerido: le permite marcar una propiedad según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="d4324-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="d4324-131">StringLength: le permite especificar una longitud máxima para una propiedad de cadena.</span><span class="sxs-lookup"><span data-stu-id="d4324-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="d4324-132">Validación: la clase base para todos los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="d4324-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d4324-133">Si no se cumplen sus necesidades de validación mediante cualquiera de los validadores estándares siempre tiene la opción de crear un atributo del validador personalizado heredando un nuevo atributo de validador de atributo de validación base.</span><span class="sxs-lookup"><span data-stu-id="d4324-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="d4324-134">La clase de producto en **listado 1** muestra cómo utilizar estos atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="d4324-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="d4324-135">Las propiedades de nombre, descripción y UnitPrice se marcan según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="d4324-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="d4324-136">La propiedad Name debe tener una longitud de cadena que tenga menos de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="d4324-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="d4324-137">Por último, la propiedad UnitPrice debe coincidir con un patrón de expresión regular que representa un importe de divisa.</span><span class="sxs-lookup"><span data-stu-id="d4324-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="d4324-138">**Listado 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="d4324-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="d4324-139">La clase de producto muestra cómo utilizar un atributo adicional: el atributo DisplayName.</span><span class="sxs-lookup"><span data-stu-id="d4324-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="d4324-140">El atributo DisplayName le permite modificar el nombre de la propiedad cuando la propiedad se muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="d4324-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="d4324-141">En lugar de mostrar el mensaje de error "el campo UnitPrice is required" puede mostrar el mensaje de error "el campo de precio is required".</span><span class="sxs-lookup"><span data-stu-id="d4324-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d4324-142">Si desea personalizar completamente el mensaje de error mostrado por un validador puede asignar un mensaje de error personalizado a propiedad de mensaje de error del control de validación similar al siguiente: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="d4324-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="d4324-143">Puede usar la clase de producto en **listado 1** con la acción del controlador Create() en **listado 2**.</span><span class="sxs-lookup"><span data-stu-id="d4324-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="d4324-144">Esta acción de controlador vuelve a mostrar la vista de creación cuando el estado del modelo contiene los errores.</span><span class="sxs-lookup"><span data-stu-id="d4324-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="d4324-145">**Listado 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="d4324-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="d4324-146">Por último, puede crear la vista en **listado 3** haciendo clic en la acción Create() y seleccionar la opción de menú **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="d4324-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="d4324-147">Crear una vista fuertemente tipado con la clase Product como la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="d4324-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="d4324-148">Seleccione **crear** en la lista de contenido de lista desplegable de vista (consulte **figura 2**).</span><span class="sxs-lookup"><span data-stu-id="d4324-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="d4324-149">**Figura 2**: adición de la vista de creación</span><span class="sxs-lookup"><span data-stu-id="d4324-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="d4324-150">**Listado 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="d4324-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d4324-151">Quite el campo de Id. del formulario de creación generado por el **agregar vista** opción de menú.</span><span class="sxs-lookup"><span data-stu-id="d4324-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="d4324-152">Dado que el campo Id. corresponde a una columna de identidad, no desea permitir que los usuarios escriban un valor para este campo.</span><span class="sxs-lookup"><span data-stu-id="d4324-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="d4324-153">Si se envía el formulario de creación de un producto y no escribir valores para los campos obligatorios, entonces el error de validación de mensajes en **figura 3** se muestran.</span><span class="sxs-lookup"><span data-stu-id="d4324-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="d4324-154">**Figura 3**: faltan campos obligatorios</span><span class="sxs-lookup"><span data-stu-id="d4324-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="d4324-155">Si escribe una cantidad de moneda no válido y, a continuación, el mensaje de error en **figura 4** se muestra.</span><span class="sxs-lookup"><span data-stu-id="d4324-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="d4324-156">**Figura 4**: cantidad de moneda no válido</span><span class="sxs-lookup"><span data-stu-id="d4324-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="d4324-157">Uso de validadores de anotación de datos con Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d4324-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="d4324-158">Si está utilizando Microsoft Entity Framework para generar las clases de modelo de datos no se puede aplicar los atributos del validador directamente a las clases.</span><span class="sxs-lookup"><span data-stu-id="d4324-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="d4324-159">Dado que Entity Framework Designer genera las clases del modelo, cualquier cambio que realice en las clases del modelo se sobrescribirán la próxima vez que realice los cambios en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="d4324-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="d4324-160">Si desea usar los controles de validación con las clases generadas por Entity Framework, a continuación, deberá crear clases de datos de metadatos.</span><span class="sxs-lookup"><span data-stu-id="d4324-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="d4324-161">Los controles de validación se aplican a la clase de datos de metadatos en lugar de aplicar los validadores a la clase real.</span><span class="sxs-lookup"><span data-stu-id="d4324-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="d4324-162">Por ejemplo, imagine que ha creado una clase llamada Movie mediante Entity Framework (consulte **figura 5**).</span><span class="sxs-lookup"><span data-stu-id="d4324-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="d4324-163">Además, imagínese que desea realizar el título de la película y el Director de las propiedades de las propiedades necesarias.</span><span class="sxs-lookup"><span data-stu-id="d4324-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="d4324-164">En ese caso, puede crear la clase parcial y la clase de datos de metadatos en **listado 4**.</span><span class="sxs-lookup"><span data-stu-id="d4324-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="d4324-165">**Figura 5**: clase Movie generada por Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d4324-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="d4324-166">**Listado 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="d4324-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="d4324-167">El archivo en **listado 4** contiene dos clases denominadas película y MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="d4324-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="d4324-168">La clase Movie es una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="d4324-168">The Movie class is a partial class.</span></span> <span data-ttu-id="d4324-169">Se corresponde con la clase parcial generada por Entity Framework que se encuentra en el archivo DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="d4324-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="d4324-170">Actualmente, .NET framework no admite propiedades parciales.</span><span class="sxs-lookup"><span data-stu-id="d4324-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="d4324-171">Por lo tanto, no hay ninguna manera de aplicar los atributos de validación a las propiedades de la clase Movie definido en el archivo DataModel.Designer.vb aplicando los atributos de validación a las propiedades de la clase Movie definido en el archivo en **listado 4**.</span><span class="sxs-lookup"><span data-stu-id="d4324-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="d4324-172">Tenga en cuenta que la clase parcial de la película se decora con un atributo de MetadataType que señala a la clase MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="d4324-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="d4324-173">La clase MovieMetaData contiene las propiedades de proxy para las propiedades de la clase de la película.</span><span class="sxs-lookup"><span data-stu-id="d4324-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="d4324-174">Los atributos de validación se aplican a las propiedades de la clase MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="d4324-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="d4324-175">Las propiedades de título, el Director y DateReleased se marcan como las propiedades necesarias.</span><span class="sxs-lookup"><span data-stu-id="d4324-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="d4324-176">La propiedad Director debe asignarse a una cadena que contiene menos de 5 caracteres.</span><span class="sxs-lookup"><span data-stu-id="d4324-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="d4324-177">Por último, se aplica el atributo DisplayName a la propiedad DateReleased para mostrar un mensaje de error como "el campo de fecha liberado is required."</span><span class="sxs-lookup"><span data-stu-id="d4324-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="d4324-178">en lugar el error "el campo DateReleased es obligatorio".</span><span class="sxs-lookup"><span data-stu-id="d4324-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d4324-179">Tenga en cuenta que las propiedades de proxy en la clase MovieMetaData no es necesario representar los mismos tipos que las propiedades correspondientes de la clase Movie.</span><span class="sxs-lookup"><span data-stu-id="d4324-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="d4324-180">Por ejemplo, la propiedad Director es una propiedad de cadena en la clase Movie y una propiedad de objeto en la clase MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="d4324-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="d4324-181">En la página de **figura 6** muestra los mensajes de error devueltos al especificar valores no válidos para las propiedades de la película.</span><span class="sxs-lookup"><span data-stu-id="d4324-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="d4324-182">**Figura 6**: uso de los validadores con Entity Framework ([haga clic aquí para ver imagen en tamaño completo](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="d4324-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="d4324-183">Resumen</span><span class="sxs-lookup"><span data-stu-id="d4324-183">Summary</span></span>

<span data-ttu-id="d4324-184">En este tutorial, aprendió a aprovechar el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d4324-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="d4324-185">Ha aprendido a usar los diferentes tipos de atributos de validación, como los necesarios y StringLength atributos.</span><span class="sxs-lookup"><span data-stu-id="d4324-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="d4324-186">También ha aprendido a usar estos atributos cuando se trabaja con Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4324-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d4324-187">[Anterior](validating-with-a-service-layer-cs.md)
> [Siguiente](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d4324-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
