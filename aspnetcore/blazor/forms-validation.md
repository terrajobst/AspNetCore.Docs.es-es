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
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor formularios y validación

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Los formularios y la validación se admiten en Blazor con [anotaciones de datos](xref:mvc/models/validation).

El siguiente tipo de `ExampleModel` define la lógica de validación mediante anotaciones de datos:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un formulario se define mediante el componente de `EditForm`. En el siguiente formulario se muestran los elementos, componentes y código Razor típicos:

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

* El formulario valida los datos proporcionados por el usuario en el campo `name` mediante la validación definida en el tipo de `ExampleModel`. El modelo se crea en el bloque `@code` del componente y se mantiene en un campo privado (`exampleModel`). El campo se asigna al atributo `Model` del elemento `<EditForm>`.
* El componente `DataAnnotationsValidator` adjunta la compatibilidad con la validación mediante anotaciones de datos.
* El componente `ValidationSummary` resume los mensajes de validación.
* `HandleValidSubmit` se desencadena cuando el formulario se envía correctamente (pasa la validación).

Hay disponible un conjunto de componentes de entrada integrados para recibir y validar los datos proporcionados por el usuario. Las entradas se validan cuando se cambian y cuando se envía un formulario. Los componentes de entrada disponibles se muestran en la tabla siguiente.

| Componente de entrada | Se representa como&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Todos los componentes de entrada, incluidos los `EditForm`, admiten atributos arbitrarios. Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento HTML representado.

Los componentes de entrada proporcionan el comportamiento predeterminado para validar en la edición y cambiar su clase CSS para reflejar el estado del campo. Algunos componentes incluyen lógica de análisis útil. Por ejemplo, `InputDate` y `InputNumber` controlar correctamente los valores no analizables al registrarlos como errores de validación. Los tipos que pueden aceptar valores null también admiten la nulabilidad del campo de destino (por ejemplo, `int?`).

El tipo de `Starship` siguiente define la lógica de validación mediante un conjunto mayor de propiedades y anotaciones de datos que los `ExampleModel`anteriores:

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

En el ejemplo anterior, `Description` es opcional porque no hay ninguna anotación de datos presente.

El siguiente formulario valida los datos proporcionados por el usuario mediante la validación definida en el modelo de `Starship`:

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

En el `EditForm` se crea un `EditContext` como un [valor en cascada](xref:blazor/components#cascading-values-and-parameters) que realiza un seguimiento de los metadatos sobre el proceso de edición, incluidos los campos modificados y los mensajes de validación actuales. El `EditForm` también proporciona eventos útiles para envíos válidos y no válidos (`OnValidSubmit`, `OnInvalidSubmit`). También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de los campos con código de validación personalizado.

## <a name="inputtext-based-on-the-input-event"></a>InputText basado en el evento de entrada

Utilice el componente de `InputText` para crear un componente personalizado que use el evento `input` en lugar del evento `change`.

Cree un componente con el marcado siguiente y use el componente tal como se usa `InputText`:

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Compatibilidad con la validación

El componente `DataAnnotationsValidator` asocia la compatibilidad con la validación mediante anotaciones de datos en el `EditContext`en cascada. Habilitar la compatibilidad con la validación mediante anotaciones de datos requiere este gesto explícito. Para usar un sistema de validación distinto al de las anotaciones de datos, reemplace el `DataAnnotationsValidator` por una implementación personalizada. La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor realiza dos tipos de validación:

* La *validación de campos* se realiza cuando el usuario sale de las pestañas de un campo. Durante la validación de campos, el componente de `DataAnnotationsValidator` asocia todos los resultados de validación registrados con el campo.
* La *validación del modelo* se realiza cuando el usuario envía el formulario. Durante la validación del modelo, el componente `DataAnnotationsValidator` intenta determinar el campo en función del nombre del miembro que notifica el resultado de la validación. Los resultados de validación que no están asociados a un miembro individual están asociados al modelo en lugar de a un campo.

### <a name="validation-summary-and-validation-message-components"></a>Resumen de validación y componentes de mensajes de validación

El componente `ValidationSummary` resume todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de Resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

Los mensajes de validación de salida para un modelo específico con el parámetro `Model`:
  
```razor
<ValidationSummary Model="@starship" />
```

En el componente de `ValidationMessage` se muestran los mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Especifique el campo para la validación con el atributo `For` y una expresión lambda que nombra la propiedad del modelo:

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Los componentes `ValidationMessage` y `ValidationSummary` admiten atributos arbitrarios. Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento `<div>` o `<ul>` generado.

### <a name="custom-validation-attributes"></a>Atributos de validación personalizados

Para asegurarse de que un resultado de validación está asociado correctamente a un campo cuando se usa un [atributo de validación personalizado](xref:mvc/models/validation#custom-attributes), pase el <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> del contexto de validación al crear el <xref:System.ComponentModel.DataAnnotations.ValidationResult>:

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor paquete de validación de anotaciones de datos

[Microsoft. AspNetCore.Blazor. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) es un paquete que llena los huecos de experiencia de validación mediante el componente de `DataAnnotationsValidator`. El paquete es *experimental*actualmente.

### <a name="compareproperty-attribute"></a>Atributo [CompareProperty]

El <xref:System.ComponentModel.DataAnnotations.CompareAttribute> no funciona bien con el componente `DataAnnotationsValidator`. [Microsoft. AspNetCore.Blazor. DataAnnotations:](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) el paquete *experimental* de validación introduce un atributo de validación adicional, `ComparePropertyAttribute`, que soluciona estas limitaciones. En una aplicación Blazor, `[CompareProperty]` es un reemplazo directo del atributo `[Compare]`. Para obtener más información, vea [CompareAttribute omitido con OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).

### <a name="nested-models-collection-types-and-complex-types"></a>Modelos anidados, tipos de colección y tipos complejos

Blazor proporciona compatibilidad para validar la entrada del formulario mediante anotaciones de datos con la `DataAnnotationsValidator`integrada. Sin embargo, el `DataAnnotationsValidator` solo valida las propiedades de nivel superior del modelo enlazadas al formulario que no son propiedades de tipo complejo o colección.

Para validar el gráfico de objetos completo del modelo enlazado, incluidas las propiedades de tipo complejo y de colección, use el `ObjectGraphDataAnnotationsValidator` proporcionado por [Microsoft. AspNetCore.Blazorexperimental. DataAnnotations.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) paquete de validación:

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Anotar las propiedades del modelo con `[ValidateComplexType]`. En las clases de modelo siguientes, la clase `ShipDescription` contiene anotaciones de datos adicionales para validar cuando el modelo está enlazado al formulario:

*Starship.CS*:

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

*ShipDescription.CS*:

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

### <a name="validation-of-complex-or-collection-type-properties"></a>Validación de propiedades de tipo complejo o de colección

Los atributos de validación que se aplican a las propiedades de un modelo se validan cuando se envía el formulario. Sin embargo, las propiedades de las colecciones o los tipos de datos complejos de un modelo no se validan en el envío del formulario por parte del `DataAnnotationsValidator` componente. Para respetar los atributos de validación anidados en este escenario, utilice un componente de validación personalizado. Para obtener un ejemplo, vea el [ejemplo de validación deBlazor (ASPNET/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
