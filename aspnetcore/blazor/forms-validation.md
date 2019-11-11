---
title: Formularios y validación de ASP.NET Core increíbles
author: guardrex
description: Obtenga información sobre cómo usar los escenarios de validación de campos y formularios en extraordinarias.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6dcc36c5133367493b476655dbdf73b75db9d168
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905731"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>Formularios y validación de ASP.NET Core increíbles

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Los formularios y la validación se admiten en el uso de [anotaciones de datos](xref:mvc/models/validation).

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

En el `EditForm` se crea un `EditContext` como un [valor en cascada](xref:blazor/components#cascading-values-and-parameters) que realiza un seguimiento de los metadatos sobre el proceso de edición, incluidos los campos modificados y los mensajes de validación actuales. El `EditForm` también proporciona eventos útiles para envíos válidos y no válidos (`OnValidSubmit`, `OnInvalidSubmit`). También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de los campos con código de validación personalizado.

## <a name="inputtext-based-on-the-input-event"></a>InputText basado en el evento de entrada

Utilice el componente de `InputText` para crear un componente personalizado que use el evento `input` en lugar del evento `change`.

Cree un componente con el marcado siguiente y use el componente tal como se usa `InputText`:

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Compatibilidad con la validación

El componente `DataAnnotationsValidator` asocia la compatibilidad con la validación mediante anotaciones de datos en el `EditContext`en cascada. Habilitar la compatibilidad con la validación mediante anotaciones de datos requiere este gesto explícito. Para usar un sistema de validación distinto al de las anotaciones de datos, reemplace el `DataAnnotationsValidator` por una implementación personalizada. La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

El componente `ValidationSummary` resume todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de Resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

En el componente de `ValidationMessage` se muestran los mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Especifique el campo para la validación con el atributo `For` y una expresión lambda que nombra la propiedad del modelo:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Los componentes `ValidationMessage` y `ValidationSummary` admiten atributos arbitrarios. Cualquier atributo que no coincida con un parámetro de componente se agrega al elemento `<div>` o `<ul>` generado.

::: moniker range=">= aspnetcore-3.1"

**Paquete Microsoft. AspNetCore. increíbles. DataAnnotations. Validation**

[Microsoft. AspNetCore. increíbles. DataAnnotations. Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) es un paquete que llena los huecos de experiencia de validación mediante el componente de `DataAnnotationsValidator`. El paquete es *experimental*actualmente y tenemos previsto agregar estos escenarios en el marco de ASP.net Core en una versión futura.

El componente `DataAnnotationsValidator` no valida las subpropiedades de propiedades complejas en un modelo de validación. No se validan los elementos de las propiedades de tipo de colección. Para validar estos tipos, el paquete de `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` introduce el atributo de validación `ValidateComplexType` que funciona junto con el componente `ObjectGraphDataAnnotationsValidator`. Para obtener un ejemplo de estos tipos en uso, vea el ejemplo de validación de increíble información [en el repositorio de github de ASPNET/samples ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

El <xref:System.ComponentModel.DataAnnotations.CompareAttribute> no funciona bien con el componente `DataAnnotationsValidator`. El paquete de `Microsoft.AspNetCore.Blazor.DataAnnotations.Validation` introduce un atributo de validación adicional, `ComparePropertyAttribute`, que soluciona estas limitaciones. En una aplicación increíblemente ligera, `ComparePropertyAttribute` es un reemplazo directo del `CompareAttribute`. Para obtener más información, vea [CompareAttribute omitido con OnValidSubmit EditForm (ASPNET/AspNetCore \#10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Validación de propiedades de tipo complejo o de colección

Los atributos de validación que se aplican a las propiedades de un modelo se validan cuando se envía el formulario. Sin embargo, las propiedades de las colecciones o los tipos de datos complejos de un modelo no se validan en el envío del formulario por parte del `DataAnnotationsValidator` componente. Para respetar los atributos de validación anidados en este escenario, utilice un componente de validación personalizado. Para obtener un ejemplo, consulte el [ejemplo de validación de increíble información en el repositorio de github de ASPNET/samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
