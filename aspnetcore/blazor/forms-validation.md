---
title: ASP.NET Core Blazor formularios y validación
author: guardrex
description: Obtenga información sobre cómo usar los escenarios de validación de campos y formularios en Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: a94a433f26e451bbadc73615e502e46d273f05c2
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828144"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="b6eae-103">ASP.NET Core Blazor formularios y validación</span><span class="sxs-lookup"><span data-stu-id="b6eae-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="b6eae-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b6eae-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b6eae-105">Los formularios y la validación se admiten en Blazor con [anotaciones de datos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="b6eae-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="b6eae-106">El siguiente tipo de `ExampleModel` define la lógica de validación mediante anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="b6eae-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="b6eae-107">Un formulario se define mediante el componente de `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="b6eae-108">En el siguiente formulario se muestran los elementos, componentes y código Razor típicos:</span><span class="sxs-lookup"><span data-stu-id="b6eae-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

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

* <span data-ttu-id="b6eae-109">El formulario valida los datos proporcionados por el usuario en el campo `name` mediante la validación definida en el tipo de `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="b6eae-110">El modelo se crea en el bloque `@code` del componente y se mantiene en un campo privado (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="b6eae-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="b6eae-111">El campo se asigna al atributo `Model` del elemento `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="b6eae-112">El componente `DataAnnotationsValidator` adjunta la compatibilidad con la validación mediante anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="b6eae-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="b6eae-113">El componente `ValidationSummary` resume los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="b6eae-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="b6eae-114">`HandleValidSubmit` se desencadena cuando el formulario se envía correctamente (pasa la validación).</span><span class="sxs-lookup"><span data-stu-id="b6eae-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="b6eae-115">Hay disponible un conjunto de componentes de entrada integrados para recibir y validar los datos proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="b6eae-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="b6eae-116">Las entradas se validan cuando se cambian y cuando se envía un formulario.</span><span class="sxs-lookup"><span data-stu-id="b6eae-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="b6eae-117">Los componentes de entrada disponibles se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="b6eae-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="b6eae-118">Componente de entrada</span><span class="sxs-lookup"><span data-stu-id="b6eae-118">Input component</span></span> | <span data-ttu-id="b6eae-119">Se representa como&hellip;</span><span class="sxs-lookup"><span data-stu-id="b6eae-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="b6eae-120">Todos los componentes de entrada, incluidos los `EditForm`, admiten atributos arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="b6eae-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="b6eae-121">Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento HTML representado.</span><span class="sxs-lookup"><span data-stu-id="b6eae-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="b6eae-122">Los componentes de entrada proporcionan el comportamiento predeterminado para validar en la edición y cambiar su clase CSS para reflejar el estado del campo.</span><span class="sxs-lookup"><span data-stu-id="b6eae-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="b6eae-123">Algunos componentes incluyen lógica de análisis útil.</span><span class="sxs-lookup"><span data-stu-id="b6eae-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="b6eae-124">Por ejemplo, `InputDate` y `InputNumber` controlar correctamente los valores no analizables al registrarlos como errores de validación.</span><span class="sxs-lookup"><span data-stu-id="b6eae-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="b6eae-125">Los tipos que pueden aceptar valores null también admiten la nulabilidad del campo de destino (por ejemplo, `int?`).</span><span class="sxs-lookup"><span data-stu-id="b6eae-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="b6eae-126">El tipo de `Starship` siguiente define la lógica de validación mediante un conjunto mayor de propiedades y anotaciones de datos que los `ExampleModel`anteriores:</span><span class="sxs-lookup"><span data-stu-id="b6eae-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="b6eae-127">En el ejemplo anterior, `Description` es opcional porque no hay ninguna anotación de datos presente.</span><span class="sxs-lookup"><span data-stu-id="b6eae-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="b6eae-128">El siguiente formulario valida los datos proporcionados por el usuario mediante la validación definida en el modelo de `Starship`:</span><span class="sxs-lookup"><span data-stu-id="b6eae-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```razor
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

<span data-ttu-id="b6eae-129">En el `EditForm` se crea un `EditContext` como un [valor en cascada](xref:blazor/components#cascading-values-and-parameters) que realiza un seguimiento de los metadatos sobre el proceso de edición, incluidos los campos modificados y los mensajes de validación actuales.</span><span class="sxs-lookup"><span data-stu-id="b6eae-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="b6eae-130">El `EditForm` también proporciona eventos útiles para envíos válidos y no válidos (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="b6eae-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="b6eae-131">También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de los campos con código de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="b6eae-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="b6eae-132">InputText basado en el evento de entrada</span><span class="sxs-lookup"><span data-stu-id="b6eae-132">InputText based on the input event</span></span>

<span data-ttu-id="b6eae-133">Utilice el componente de `InputText` para crear un componente personalizado que use el evento `input` en lugar del evento `change`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="b6eae-134">Cree un componente con el marcado siguiente y use el componente tal como se usa `InputText`:</span><span class="sxs-lookup"><span data-stu-id="b6eae-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="b6eae-135">Compatibilidad con la validación</span><span class="sxs-lookup"><span data-stu-id="b6eae-135">Validation support</span></span>

<span data-ttu-id="b6eae-136">El componente `DataAnnotationsValidator` asocia la compatibilidad con la validación mediante anotaciones de datos en el `EditContext`en cascada.</span><span class="sxs-lookup"><span data-stu-id="b6eae-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="b6eae-137">Habilitar la compatibilidad con la validación mediante anotaciones de datos requiere este gesto explícito.</span><span class="sxs-lookup"><span data-stu-id="b6eae-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="b6eae-138">Para usar un sistema de validación distinto al de las anotaciones de datos, reemplace el `DataAnnotationsValidator` por una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="b6eae-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="b6eae-139">La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="b6eae-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="b6eae-140"> realiza dos tipos de validación:</span><span class="sxs-lookup"><span data-stu-id="b6eae-140"> performs two types of validation:</span></span>

* <span data-ttu-id="b6eae-141">La *validación de campos* se realiza cuando el usuario sale de las pestañas de un campo.</span><span class="sxs-lookup"><span data-stu-id="b6eae-141">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="b6eae-142">Durante la validación de campos, el componente de `DataAnnotationsValidator` asocia todos los resultados de validación registrados con el campo.</span><span class="sxs-lookup"><span data-stu-id="b6eae-142">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="b6eae-143">La *validación del modelo* se realiza cuando el usuario envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="b6eae-143">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="b6eae-144">Durante la validación del modelo, el componente `DataAnnotationsValidator` intenta determinar el campo en función del nombre del miembro que notifica el resultado de la validación.</span><span class="sxs-lookup"><span data-stu-id="b6eae-144">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="b6eae-145">Los resultados de validación que no están asociados a un miembro individual están asociados al modelo en lugar de a un campo.</span><span class="sxs-lookup"><span data-stu-id="b6eae-145">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="b6eae-146">Resumen de validación y componentes de mensajes de validación</span><span class="sxs-lookup"><span data-stu-id="b6eae-146">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="b6eae-147">El componente `ValidationSummary` resume todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de Resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="b6eae-147">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="b6eae-148">Los mensajes de validación de salida para un modelo específico con el parámetro `Model`:</span><span class="sxs-lookup"><span data-stu-id="b6eae-148">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="b6eae-149">En el componente de `ValidationMessage` se muestran los mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b6eae-149">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="b6eae-150">Especifique el campo para la validación con el atributo `For` y una expresión lambda que nombra la propiedad del modelo:</span><span class="sxs-lookup"><span data-stu-id="b6eae-150">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="b6eae-151">Los componentes `ValidationMessage` y `ValidationSummary` admiten atributos arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="b6eae-151">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="b6eae-152">Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento `<div>` o `<ul>` generado.</span><span class="sxs-lookup"><span data-stu-id="b6eae-152">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="b6eae-153">Atributos de validación personalizados</span><span class="sxs-lookup"><span data-stu-id="b6eae-153">Custom validation attributes</span></span>

<span data-ttu-id="b6eae-154">Para asegurarse de que un resultado de validación está asociado correctamente a un campo cuando se usa un [atributo de validación personalizado](xref:mvc/models/validation#custom-attributes), pase el <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> del contexto de validación al crear el <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span><span class="sxs-lookup"><span data-stu-id="b6eae-154">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="b6eae-155"> paquete de validación de anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="b6eae-155"> data annotations validation package</span></span>

<span data-ttu-id="b6eae-156">[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) es un paquete que llena los huecos de experiencia de validación mediante el componente de `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-156">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="b6eae-157">El paquete es *experimental*actualmente.</span><span class="sxs-lookup"><span data-stu-id="b6eae-157">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="b6eae-158">Atributo [CompareProperty]</span><span class="sxs-lookup"><span data-stu-id="b6eae-158">[CompareProperty] attribute</span></span>

<span data-ttu-id="b6eae-159">El <xref:System.ComponentModel.DataAnnotations.CompareAttribute> no funciona bien con el componente `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-159">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="b6eae-160">[Microsoft. AspNetCore.Blazor. DataAnnotations:](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) el paquete *experimental* de validación introduce un atributo de validación adicional, `ComparePropertyAttribute`, que soluciona estas limitaciones.</span><span class="sxs-lookup"><span data-stu-id="b6eae-160">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="b6eae-161">En una aplicación Blazor, `[CompareProperty]` es un reemplazo directo del atributo `[Compare]`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-161">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span> <span data-ttu-id="b6eae-162">Para obtener más información, vea [CompareAttribute omitido con OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span><span class="sxs-lookup"><span data-stu-id="b6eae-162">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="b6eae-163">Modelos anidados, tipos de colección y tipos complejos</span><span class="sxs-lookup"><span data-stu-id="b6eae-163">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="b6eae-164"> proporciona compatibilidad para validar la entrada del formulario mediante anotaciones de datos con la `DataAnnotationsValidator`integrada.</span><span class="sxs-lookup"><span data-stu-id="b6eae-164"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="b6eae-165">Sin embargo, el `DataAnnotationsValidator` solo valida las propiedades de nivel superior del modelo enlazadas al formulario que no son propiedades de tipo complejo o colección.</span><span class="sxs-lookup"><span data-stu-id="b6eae-165">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="b6eae-166">Para validar el gráfico de objetos completo del modelo enlazado, incluidas las propiedades de tipo complejo y de colección, use el `ObjectGraphDataAnnotationsValidator` proporcionado por [Microsoft. AspNetCore.Blazorexperimental. DataAnnotations.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) paquete de validación:</span><span class="sxs-lookup"><span data-stu-id="b6eae-166">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="b6eae-167">Anotar las propiedades del modelo con `[ValidateComplexType]`.</span><span class="sxs-lookup"><span data-stu-id="b6eae-167">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="b6eae-168">En las clases de modelo siguientes, la clase `ShipDescription` contiene anotaciones de datos adicionales para validar cuando el modelo está enlazado al formulario:</span><span class="sxs-lookup"><span data-stu-id="b6eae-168">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="b6eae-169">*Starship.CS*:</span><span class="sxs-lookup"><span data-stu-id="b6eae-169">*Starship.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

<span data-ttu-id="b6eae-170">*ShipDescription.CS*:</span><span class="sxs-lookup"><span data-stu-id="b6eae-170">*ShipDescription.cs*:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="b6eae-171">Validación de propiedades de tipo complejo o de colección</span><span class="sxs-lookup"><span data-stu-id="b6eae-171">Validation of complex or collection type properties</span></span>

<span data-ttu-id="b6eae-172">Los atributos de validación que se aplican a las propiedades de un modelo se validan cuando se envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="b6eae-172">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="b6eae-173">Sin embargo, las propiedades de las colecciones o los tipos de datos complejos de un modelo no se validan en el envío del formulario por parte del `DataAnnotationsValidator` componente.</span><span class="sxs-lookup"><span data-stu-id="b6eae-173">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="b6eae-174">Para respetar los atributos de validación anidados en este escenario, utilice un componente de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="b6eae-174">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="b6eae-175">Para obtener un ejemplo, vea el [ejemplo de validación deBlazor (ASPNET/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="b6eae-175">For an example, see the [Blazor Validation sample (aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
