---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828672"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="cb19d-103">Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 3</span><span class="sxs-lookup"><span data-stu-id="cb19d-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="cb19d-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="cb19d-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="cb19d-105">Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cb19d-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="cb19d-106">Trabajar con tipos complejos</span><span class="sxs-lookup"><span data-stu-id="cb19d-106">Working with Complex Types</span></span>

<span data-ttu-id="cb19d-107">En esta sección podrá crear una clase de dirección y obtenga información sobre cómo crear una plantilla para que se muestre.</span><span class="sxs-lookup"><span data-stu-id="cb19d-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="cb19d-108">En el *modelos* carpeta, cree un nuevo archivo de clase denominado *Person.cs* que colocará dos tipos: un `Person` clase y un `Address` clase.</span><span class="sxs-lookup"><span data-stu-id="cb19d-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="cb19d-109">El `Person` clase contendrá una propiedad que se escribe como `Address`.</span><span class="sxs-lookup"><span data-stu-id="cb19d-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="cb19d-110">El `Address` tipo es un tipo complejo, lo que significa que no es uno de los tipos integrados como `int`, `string`, o `double`.</span><span class="sxs-lookup"><span data-stu-id="cb19d-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="cb19d-111">En su lugar, tiene varias propiedades.</span><span class="sxs-lookup"><span data-stu-id="cb19d-111">Instead, it has several properties.</span></span> <span data-ttu-id="cb19d-112">El código para las nuevas clases tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="cb19d-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="cb19d-113">En el `Movie` controlador, agregue las siguientes `PersonDetail` acción para mostrar una instancia de person:</span><span class="sxs-lookup"><span data-stu-id="cb19d-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="cb19d-114">A continuación, agregue el código siguiente a la `Movie` controlador para rellenar el `Person` modelar con algunos datos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cb19d-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="cb19d-115">Abra el *Views\Movies\PersonDetail.cshtml* archivo y agregue el siguiente marcado para el `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="cb19d-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="cb19d-116">Presione Ctrl + F5 para ejecutar la aplicación y vaya a *películas/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="cb19d-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="cb19d-117">El `PersonDetail` vista no contiene el `Address` tipo complejo, como puede ver en esta captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="cb19d-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="cb19d-118">(No se muestra ninguna dirección).</span><span class="sxs-lookup"><span data-stu-id="cb19d-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="cb19d-119">El `Address` no se muestra el modelo de datos porque es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="cb19d-120">Para mostrar la información de dirección, abra el *Views\Movies\PersonDetail.cshtml* archivo nuevo y agregue el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="cb19d-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="cb19d-121">El marcado completo para el `PersonDetail` ahora la vista tiene un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb19d-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="cb19d-122">Ejecute de nuevo la aplicación y mostrar la `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="cb19d-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="cb19d-123">Ahora se muestra la información de dirección:</span><span class="sxs-lookup"><span data-stu-id="cb19d-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="cb19d-124">Creación de una plantilla para un tipo complejo</span><span class="sxs-lookup"><span data-stu-id="cb19d-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="cb19d-125">En esta sección creará una plantilla que se usará para representar el `Address` tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="cb19d-126">Cuando se crea una plantilla para el `Address` tipo, ASP.NET MVC automáticamente sirve para dar formato a un modelo de dirección en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cb19d-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="cb19d-127">Esto le ofrece una manera de controlar la representación de la `Address` tipo desde un solo lugar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cb19d-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="cb19d-128">En el *Views\Shared\DisplayTemplates* carpeta, cree una vista parcial fuertemente tipada denominada **dirección**:</span><span class="sxs-lookup"><span data-stu-id="cb19d-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="cb19d-129">Haga clic en **agregar**y, a continuación, abra el nuevo *Views\Shared\DisplayTemplates\Address.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="cb19d-130">La nueva vista contiene el marcado generado siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb19d-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="cb19d-131">Ejecute la aplicación y mostrar la `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="cb19d-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="cb19d-132">Esta vez, el `Address` plantilla que acaba de crear se utiliza para mostrar el `Address` tipo complejo, por lo que la pantalla es similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb19d-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="cb19d-133">Resumen: Formas de especificar el formato de presentación del modelo y la plantilla</span><span class="sxs-lookup"><span data-stu-id="cb19d-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="cb19d-134">Hemos visto que puede especificar el formato o la plantilla para una propiedad de modelo mediante los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="cb19d-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="cb19d-135">Aplicar el `DisplayFormat` atributo a una propiedad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="cb19d-136">Por ejemplo, el código siguiente hace que la fecha que se mostrará sin la hora:</span><span class="sxs-lookup"><span data-stu-id="cb19d-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="cb19d-137">Aplicar un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a una propiedad en el modelo y especificar el tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="cb19d-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="cb19d-138">Por ejemplo, el código siguiente hace que la fecha que se mostrará sin incluir el tiempo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="cb19d-139">Si la aplicación contiene un *date.cshtml* plantilla en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta, esa plantilla se usará para representar el `DateTime` propiedad.</span><span class="sxs-lookup"><span data-stu-id="cb19d-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="cb19d-140">En caso contrario, el sistema de creación de plantillas ASP.NET integrado muestra la propiedad como una fecha.</span><span class="sxs-lookup"><span data-stu-id="cb19d-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="cb19d-141">Creación de una plantilla de visualización en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta cuyo nombre coincide con el tipo de datos que desea dar formato.</span><span class="sxs-lookup"><span data-stu-id="cb19d-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="cb19d-142">Por ejemplo, hemos visto que la *Views\Shared\DisplayTemplates\DateTime.cshtml* se usa para representar `DateTime` propiedades en un modelo, sin agregar un atributo para el modelo y sin agregar ningún marcado para las vistas.</span><span class="sxs-lookup"><span data-stu-id="cb19d-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="cb19d-143">Mediante el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo en el modelo para especificar la plantilla para mostrar la propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="cb19d-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="cb19d-144">Agregar explícitamente el nombre de plantilla para mostrar el [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) llama en una vista.</span><span class="sxs-lookup"><span data-stu-id="cb19d-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="cb19d-145">El método que se utilice depende de lo que necesita hacer en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cb19d-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="cb19d-146">No es raro combinar ambos métodos para obtener exactamente el tipo de formato que necesita.</span><span class="sxs-lookup"><span data-stu-id="cb19d-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="cb19d-147">En la sección siguiente, tendrá que cambiar un poco de artes y mover de personalizar cómo se muestran los datos para personalizar la forma de especificar.</span><span class="sxs-lookup"><span data-stu-id="cb19d-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="cb19d-148">Podrá enlazar datepicker de jQuery a las vistas de edición de la aplicación con el fin de proporcionar una manera sorprendente para especificar las fechas.</span><span class="sxs-lookup"><span data-stu-id="cb19d-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cb19d-149">[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="cb19d-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
