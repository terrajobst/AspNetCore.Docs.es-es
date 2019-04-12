---
title: Validación y formularios de componentes de razor
author: guardrex
description: Aprenda a usar formularios y escenarios de validación de campo en los componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515668"
---
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="397dd-103">Validación y formularios de componentes de razor</span><span class="sxs-lookup"><span data-stu-id="397dd-103">Razor Components forms and validation</span></span>

<span data-ttu-id="397dd-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="397dd-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="397dd-105">Formularios y la validación se admiten en los componentes de Razor mediante [anotaciones de datos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="397dd-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="397dd-106">Formularios y escenarios de validación están probables que cambien con cada versión de vista previa.</span><span class="sxs-lookup"><span data-stu-id="397dd-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="397dd-107">La siguiente `ExampleModel` tipo define la lógica de validación usando anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="397dd-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="397dd-108">Un formulario se define utilizando el `<EditForm>` componente.</span><span class="sxs-lookup"><span data-stu-id="397dd-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="397dd-109">Elementos típicos, componentes y código de Razor, muestra la forma siguiente:</span><span class="sxs-lookup"><span data-stu-id="397dd-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="397dd-110">El formulario valida la entrada del usuario en el `name` campo mediante la validación definida en el `ExampleModel` tipo.</span><span class="sxs-lookup"><span data-stu-id="397dd-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="397dd-111">El modelo se crea en el componente `@functions` bloquear y se mantiene en un campo privado (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="397dd-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="397dd-112">El campo se asigna a la `Model` atributo de la `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="397dd-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="397dd-113">El componente de validador de anotaciones de datos (`<DataAnnotationsValidator>`) se asocia con las anotaciones de datos de compatibilidad de validación.</span><span class="sxs-lookup"><span data-stu-id="397dd-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="397dd-114">El componente de resumen de validación (`<ValidationSummary>`) se resumen los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="397dd-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="397dd-115">se desencadena cuando el formulario envía correctamente (ha pasado la validación).</span><span class="sxs-lookup"><span data-stu-id="397dd-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="397dd-116">Un conjunto de componentes integrados de entrada están disponibles para recibir y validar la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="397dd-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="397dd-117">Las entradas se validan cuando se va a cambiar y cuando se envíe un formulario.</span><span class="sxs-lookup"><span data-stu-id="397dd-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="397dd-118">Componentes de entrada disponibles se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="397dd-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="397dd-119">Componente de entrada</span><span class="sxs-lookup"><span data-stu-id="397dd-119">Input component</span></span>   | <span data-ttu-id="397dd-120">Se representa como&hellip;</span><span class="sxs-lookup"><span data-stu-id="397dd-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="397dd-121">Los componentes de entrada proporcionan el comportamiento predeterminado para la validación de la edición y cambie su clase CSS para reflejar el estado de campo.</span><span class="sxs-lookup"><span data-stu-id="397dd-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="397dd-122">Algunos componentes incluyen la lógica de análisis útil.</span><span class="sxs-lookup"><span data-stu-id="397dd-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="397dd-123">Por ejemplo, `<InputDate>` y `<InputNumber>` controlar valores no se puede analizar correctamente al registrarlos como errores de validación.</span><span class="sxs-lookup"><span data-stu-id="397dd-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="397dd-124">Tipos que aceptan valores null también admiten la nulabilidad del campo de destino (por ejemplo, `int?`).</span><span class="sxs-lookup"><span data-stu-id="397dd-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="397dd-125">La siguiente `Starship` tipo define la lógica de validación con un conjunto mayor de propiedades y [anotaciones de datos](xref:mvc/models/validation) que el anterior `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="397dd-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="397dd-126">El formato siguiente valida la entrada de usuario mediante la validación definida en el `Starship` modelo:</span><span class="sxs-lookup"><span data-stu-id="397dd-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="397dd-127">El `<EditForm>` crea un `EditContext` como un [valor en cascada](xref:razor-components/components#cascading-values-and-parameters) que realiza el seguimiento de los metadatos acerca del proceso de edición, incluidos qué campos se han modificado y los mensajes de validación actual.</span><span class="sxs-lookup"><span data-stu-id="397dd-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="397dd-128">El `<EditForm>` también proporciona eventos cómodos para envía válidos y no válidos (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="397dd-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="397dd-129">También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de campo con el código de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="397dd-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="397dd-130">El componente de validador de anotaciones de datos (`<DataAnnotationsValidator>`) asocia el soporte técnico de validación usando anotaciones de datos a la cascada `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="397dd-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="397dd-131">Habilitar la compatibilidad para la validación mediante las anotaciones de datos actualmente requiere este gesto explícito, pero estamos considerando por lo que es el comportamiento predeterminado que, a continuación, puede invalidar.</span><span class="sxs-lookup"><span data-stu-id="397dd-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="397dd-132">Para usar un sistema de validación diferente que las anotaciones de datos, reemplace el validador de anotaciones de datos con una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="397dd-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="397dd-133">La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="397dd-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="397dd-134">La implementación de ASP.NET Core está sujeto a actualizaciones rápidas durante el período de versión de vista previa.</span><span class="sxs-lookup"><span data-stu-id="397dd-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="397dd-135">El componente de resumen de validación (`<ValidationSummary>`) se resumen todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="397dd-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="397dd-136">El componente de mensajes de validación (`<ValidationMessage>`) muestra mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="397dd-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="397dd-137">Especifique el campo para la validación con el `For` atributo y una expresión lambda, la propiedad del modelo de nomenclatura:</span><span class="sxs-lookup"><span data-stu-id="397dd-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="397dd-138">Los componentes integrados de entrada tienen limitaciones que esperamos solucionar en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="397dd-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="397dd-139">Por ejemplo, no se puede especificar atributos arbitrarios en generado `<input>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="397dd-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="397dd-140">Cree sus propias subclases de componente para controlar los escenarios no está disponible.</span><span class="sxs-lookup"><span data-stu-id="397dd-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
