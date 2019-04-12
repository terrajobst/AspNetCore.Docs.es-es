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
# <a name="razor-components-forms-and-validation"></a>Validación y formularios de componentes de razor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Formularios y la validación se admiten en los componentes de Razor mediante [anotaciones de datos](xref:mvc/models/validation).

> [!NOTE]
> Formularios y escenarios de validación están probables que cambien con cada versión de vista previa.

La siguiente `ExampleModel` tipo define la lógica de validación usando anotaciones de datos:

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un formulario se define utilizando el `<EditForm>` componente. Elementos típicos, componentes y código de Razor, muestra la forma siguiente:

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

* El formulario valida la entrada del usuario en el `name` campo mediante la validación definida en el `ExampleModel` tipo. El modelo se crea en el componente `@functions` bloquear y se mantiene en un campo privado (`exampleModel`). El campo se asigna a la `Model` atributo de la `<EditForm>`.
* El componente de validador de anotaciones de datos (`<DataAnnotationsValidator>`) se asocia con las anotaciones de datos de compatibilidad de validación.
* El componente de resumen de validación (`<ValidationSummary>`) se resumen los mensajes de validación.
* `HandleValidSubmit` se desencadena cuando el formulario envía correctamente (ha pasado la validación).

Un conjunto de componentes integrados de entrada están disponibles para recibir y validar la entrada del usuario. Las entradas se validan cuando se va a cambiar y cuando se envíe un formulario. Componentes de entrada disponibles se muestran en la tabla siguiente.

| Componente de entrada   | Se representa como&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

Los componentes de entrada proporcionan el comportamiento predeterminado para la validación de la edición y cambie su clase CSS para reflejar el estado de campo. Algunos componentes incluyen la lógica de análisis útil. Por ejemplo, `<InputDate>` y `<InputNumber>` controlar valores no se puede analizar correctamente al registrarlos como errores de validación. Tipos que aceptan valores null también admiten la nulabilidad del campo de destino (por ejemplo, `int?`).

La siguiente `Starship` tipo define la lógica de validación con un conjunto mayor de propiedades y [anotaciones de datos](xref:mvc/models/validation) que el anterior `ExampleModel`:

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

El formato siguiente valida la entrada de usuario mediante la validación definida en el `Starship` modelo:

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

El `<EditForm>` crea un `EditContext` como un [valor en cascada](xref:razor-components/components#cascading-values-and-parameters) que realiza el seguimiento de los metadatos acerca del proceso de edición, incluidos qué campos se han modificado y los mensajes de validación actual. El `<EditForm>` también proporciona eventos cómodos para envía válidos y no válidos (`OnValidSubmit`, `OnInvalidSubmit`). También puede usar `OnSubmit` para desencadenar la validación y comprobar los valores de campo con el código de validación personalizado.

El componente de validador de anotaciones de datos (`<DataAnnotationsValidator>`) asocia el soporte técnico de validación usando anotaciones de datos a la cascada `EditContext`. Habilitar la compatibilidad para la validación mediante las anotaciones de datos actualmente requiere este gesto explícito, pero estamos considerando por lo que es el comportamiento predeterminado que, a continuación, puede invalidar. Para usar un sistema de validación diferente que las anotaciones de datos, reemplace el validador de anotaciones de datos con una implementación personalizada. La implementación de ASP.NET Core está disponible para su inspección en el origen de referencia: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *La implementación de ASP.NET Core está sujeto a actualizaciones rápidas durante el período de versión de vista previa.*

El componente de resumen de validación (`<ValidationSummary>`) se resumen todos los mensajes de validación, que es similar a la [aplicación auxiliar de etiquetas de resumen de validación](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

El componente de mensajes de validación (`<ValidationMessage>`) muestra mensajes de validación de un campo específico, que es similar a la [aplicación auxiliar de etiquetas de mensaje de validación](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Especifique el campo para la validación con el `For` atributo y una expresión lambda, la propiedad del modelo de nomenclatura:

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> Los componentes integrados de entrada tienen limitaciones que esperamos solucionar en futuras versiones. Por ejemplo, no se puede especificar atributos arbitrarios en generado `<input>` etiquetas. Cree sus propias subclases de componente para controlar los escenarios no está disponible.
