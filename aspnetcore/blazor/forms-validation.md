---
title: Formularios y validación de ASP.NET Core increíbles
author: guardrex
description: Obtenga información sobre cómo usar los escenarios de validación de campos y formularios en extraordinarias.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2019
uid: blazor/forms-validation
ms.openlocfilehash: c68ebf7f7bf07b6c243ab16307716cea13870446
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176345"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a><span data-ttu-id="5b7fe-103">Formularios y validación de ASP.NET Core increíbles</span><span class="sxs-lookup"><span data-stu-id="5b7fe-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="5b7fe-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b7fe-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b7fe-105">Los formularios y la validación se admiten en el uso de [anotaciones de datos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="5b7fe-106">El tipo `ExampleModel` siguiente define la lógica de validación mediante anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="5b7fe-107">Un formulario se define mediante el `EditForm` componente.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="5b7fe-108">En el siguiente formulario se muestran los elementos, componentes y código Razor típicos:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="5b7fe-109">El formulario valida los datos proporcionados por `name` el usuario en el campo utilizando la `ExampleModel` validación definida en el tipo.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="5b7fe-110">El modelo se crea en el bloque del `@code` componente y se mantiene en un campo privado`exampleModel`().</span><span class="sxs-lookup"><span data-stu-id="5b7fe-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="5b7fe-111">El campo se asigna al `Model` atributo `<EditForm>` del elemento.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="5b7fe-112">El `DataAnnotationsValidator` componente adjunta la compatibilidad con la validación mediante anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="5b7fe-113">El `ValidationSummary` componente resume los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="5b7fe-114">`HandleValidSubmit`se desencadena cuando el formulario se envía correctamente (pasa la validación).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="5b7fe-115">Hay disponible un conjunto de componentes de entrada integrados para recibir y validar los datos proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="5b7fe-116">Las entradas se validan cuando se cambian y cuando se envía un formulario.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="5b7fe-117">Los componentes de entrada disponibles se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="5b7fe-118">Componente de entrada</span><span class="sxs-lookup"><span data-stu-id="5b7fe-118">Input component</span></span> | <span data-ttu-id="5b7fe-119">Se representa como&hellip;</span><span class="sxs-lookup"><span data-stu-id="5b7fe-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="5b7fe-120">Todos los componentes de entrada, incluido `EditForm`, admiten atributos arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="5b7fe-121">Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento HTML representado.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="5b7fe-122">Los componentes de entrada proporcionan el comportamiento predeterminado para validar en la edición y cambiar su clase CSS para reflejar el estado del campo.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="5b7fe-123">Algunos componentes incluyen lógica de análisis útil.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="5b7fe-124">Por ejemplo, `InputDate` y `InputNumber` controlan los valores que no se puedan analizar correctamente si se registran como errores de validación.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="5b7fe-125">Los tipos que pueden aceptar valores null también admiten la nulabilidad del campo de destino ( `int?`por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="5b7fe-126">El tipo `Starship` siguiente define la lógica de validación mediante un conjunto mayor de propiedades y anotaciones de datos que `ExampleModel`los anteriores:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="5b7fe-127">En el ejemplo anterior, `Description` es opcional porque no hay ninguna anotación de datos presente.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="5b7fe-128">En el siguiente formulario se validan los datos proporcionados por el `Starship` usuario mediante la validación definida en el modelo:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="5b7fe-129">Crea como un [valor en cascada](xref:blazor/components#cascading-values-and-parameters) que realiza un seguimiento de los metadatos sobre el proceso de edición, incluidos los campos modificados y los mensajes de validación actuales. `EditContext` `EditForm`</span><span class="sxs-lookup"><span data-stu-id="5b7fe-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="5b7fe-130">También proporciona eventos útiles para envíos válidos y no válidos `OnInvalidSubmit`(`OnValidSubmit`,). `EditForm`</span><span class="sxs-lookup"><span data-stu-id="5b7fe-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="5b7fe-131">También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de los campos con código de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="5b7fe-132">InputText basado en el evento de entrada</span><span class="sxs-lookup"><span data-stu-id="5b7fe-132">InputText based on the input event</span></span>

<span data-ttu-id="5b7fe-133">Utilice el `InputText` componente para crear un componente personalizado que use el `input` `change` evento en lugar del evento.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="5b7fe-134">Cree un componente con el marcado siguiente y use el componente tal y como `InputText` se usa:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="5b7fe-135">Compatibilidad con la validación</span><span class="sxs-lookup"><span data-stu-id="5b7fe-135">Validation support</span></span>

<span data-ttu-id="5b7fe-136">El `DataAnnotationsValidator` componente adjunta la compatibilidad con la validación mediante anotaciones de datos en `EditContext`cascada.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="5b7fe-137">Habilitar la compatibilidad con la validación mediante anotaciones de datos requiere este gesto explícito.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="5b7fe-138">Para usar un sistema de validación distinto al de las `DataAnnotationsValidator` anotaciones de datos, reemplace con una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="5b7fe-139">La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

<span data-ttu-id="5b7fe-140">El `ValidationSummary` componente resume todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de Resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-140">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="5b7fe-141">El `ValidationMessage` componente muestra los mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-141">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="5b7fe-142">Especifique el campo para la validación con `For` el atributo y una expresión lambda con el nombre de la propiedad del modelo:</span><span class="sxs-lookup"><span data-stu-id="5b7fe-142">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="5b7fe-143">Los `ValidationMessage` componentes `ValidationSummary` y admiten atributos arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-143">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="5b7fe-144">Cualquier atributo que no coincida con un parámetro de componente se agrega al `<div>` elemento `<ul>` o generado.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-144">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="5b7fe-145">Validación de propiedades de tipo complejo o de colección</span><span class="sxs-lookup"><span data-stu-id="5b7fe-145">Validation of complex or collection type properties</span></span>

<span data-ttu-id="5b7fe-146">Los atributos de validación que se aplican a las propiedades de un modelo se validan cuando se envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-146">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="5b7fe-147">Sin embargo, las propiedades de las colecciones o los tipos de datos complejos de un modelo no se validan en el envío del formulario.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-147">However, the properties of collections or complex data types of a model aren't validated on form submission.</span></span> <span data-ttu-id="5b7fe-148">Para respetar los atributos de validación anidados en este escenario, utilice un componente de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="5b7fe-148">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="5b7fe-149">Para obtener un ejemplo, consulte el [ejemplo de validación de increíble información en el repositorio de github de ASPNET/samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="5b7fe-149">For an example, see the [Blazor Validation sample in the aspnet/samples GitHub repository](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>
