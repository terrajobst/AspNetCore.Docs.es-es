---
title: Aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core
author: rick-anderson
description: En este tema se describen las aplicaciones auxiliares de etiquetas que se usan en los formularios.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 805c2ba5b3a9669d5547e1c595883436eea0d11a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Introducción al uso de aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette) y [Jerrie Pelser](https://github.com/jerriep)

En este documento se explica cómo trabajar con formularios y se detallan los elementos HTML que se usan habitualmente en un formulario. El elemento HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) proporciona el mecanismo principal que las aplicaciones web usan a la hora de devolver datos al servidor. La mayor parte de este documento se centra en describir las [aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) y cómo pueden servir para crear formularios HTML eficaces de manera productiva. Se recomienda leer [Introduction to Tag Helpers](tag-helpers/intro.md) (Introducción a las aplicaciones auxiliares de etiquetas) antes de este documento.

En muchos casos, las aplicaciones auxiliares HTML proporcionan un método alternativo a una aplicación auxiliar de etiquetas específica, pero es importante tener en cuenta que las aplicaciones auxiliares de etiquetas no reemplazan a las aplicaciones auxiliares HTML y que, de igual modo, no hay una aplicación auxiliar de etiqueta para cada aplicación auxiliar HTML. Si existe una alternativa de aplicación auxiliar HTML, se mencionará aquí.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Aplicación auxiliar de etiquetas de formulario (Form)

La aplicación auxiliar de etiquetas [Form](https://www.w3.org/TR/html401/interact/forms.html) hace lo siguiente:

* Genera el valor de atributo `action` del elemento HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) de una acción de controlador MVC o ruta con nombre.

* Genera un [token comprobación de solicitudes](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) oculto que impide que se falsifiquen solicitudes entre sitios (cuando se usa con el atributo `[ValidateAntiForgeryToken]` en el método de acción HTTP Post).

* Proporciona el atributo `asp-route-<Parameter Name>`, donde `<Parameter Name>` se agrega a los valores de ruta. Los parámetros `routeValues` de `Html.BeginForm` y `Html.BeginRouteForm` proporcionan una funcionalidad similar.

* Tiene `Html.BeginForm` y `Html.BeginRouteForm` como alternativa de aplicación auxiliar HTML.

Ejemplo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

La aplicación auxiliar de etiquetas Form anterior genera el siguiente HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

El tiempo de ejecución MVC genera el valor de atributo `action` de los atributos de aplicación auxiliar de etiquetas Form `asp-controller` y `asp-action`. La aplicación auxiliar de etiquetas Form genera también un [token de comprobación de solicitudes](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) oculto que impide que se falsifiquen solicitudes entre sitios (cuando se usa con el atributo `[ValidateAntiForgeryToken]` en el método de acción HTTP Post). Proteger un elemento HTML Form puro de la falsificación de solicitudes entre sitios no es tarea fácil, y la aplicación auxiliar de etiquetas Form presta este servicio.

### <a name="using-a-named-route"></a>Uso de una ruta con nombre

El atributo de aplicación auxiliar de etiquetas `asp-route` puede generar también el marcado del atributo HTML `action`. Una aplicación con una [ruta](../../fundamentals/routing.md) denominada `register` podría usar el siguiente marcado para la página de registro:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Muchas de las vistas de la carpeta *Views/Account* (que se genera cuando se crea una aplicación web con *Cuentas de usuario individuales*) contienen el atributo [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms):

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Con las plantillas integradas, `returnUrl` se rellena automáticamente solo cuando alguien intenta obtener acceso a un recurso autorizado, pero no se ha autenticado o no tiene autorización. Si se intenta realizar un acceso no autorizado, el middleware de seguridad redirige a la página de inicio de sesión con `returnUrl` configurado.

## <a name="the-input-tag-helper"></a>Aplicación auxiliar de etiquetas de entrada (Input)

La aplicación auxiliar de etiquetas Input enlaza un elemento HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) a una expresión de modelo en la vista de Razor.

Sintaxis:

```HTML
<input asp-for="<Expression Name>" />
```

La aplicación auxiliar de etiquetas Input hace lo siguiente:

* Genera los atributos HTML `id` y `name` del nombre de expresión especificado en el atributo `asp-for`. `asp-for="Property1.Property2"` es equivalente a `m => m.Property1.Property2`. El nombre de una expresión es lo que se usa para el valor de atributo `asp-for`. Vea la sección [Nombres de expresión](#expression-names) para obtener más información.

* Establece el valor de atributo HTML `type` según los atributos de tipo de modelo y de [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) aplicados a la propiedad de modelo.

* No sobrescribirá el valor de atributo HTML `type` cuando se especifique uno.

* Genera atributos de validación [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) a partir de los atributos de [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) aplicados a las propiedades de modelo.

* Tiene características de aplicación auxiliar HTML que se superponen a `Html.TextBoxFor` y `Html.EditorFor`. Vea la sección **Alternativas de aplicación auxiliar HTML a la aplicación auxiliar de etiquetas Input** para obtener más información.

* Permite establecer tipado fuerte. Si el nombre de la propiedad cambia y no se actualiza la aplicación auxiliar de etiquetas, aparecerá un error similar al siguiente:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

La aplicación auxiliar de etiquetas `Input` establece el atributo HTML `type` en función del tipo .NET. En la siguiente tabla se enumeran algunos tipos .NET habituales y el tipo HTML generado correspondiente (no incluimos aquí todos los tipos .NET).

|Tipo de .NET|Tipo de entrada|
|---|---|
|Bool|type=”checkbox”|
|String|type=”text”|
|DateTime|type=”datetime”|
|Byte|type=”number”|
|Valor int.|type=”number”|
|Single, Double|type=”number”|


En la siguiente tabla se muestran algunos atributos de [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) comunes que la aplicación auxiliar de etiquetas Input asignará a tipos de entrada concretos (no incluimos aquí todos los atributo de validación):


|Atributo|Tipo de entrada|
|---|---|
|[EmailAddress]|type=”email”|
|[Url]|type=”url”|
|[HiddenInput]|type=”hidden”|
|[Phone]|type=”tel”|
|[DataType(DataType.Password)]| type=”password”|
|[DataType(DataType.Date)]| type=”date”|
|[DataType(DataType.Time)]| type=”time”|


Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

El código anterior genera el siguiente HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

Las anotaciones de datos que se aplican a las propiedades `Email` y `Password` generan metadatos en el modelo. La aplicación auxiliar de etiquetas Input usa esos metadatos del modelo y genera atributos [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`. Vea [Model Validation](../models/validation.md) (Validación del modelo). Estos atributos describen los validadores que se van a adjuntar a los campos de entrada, lo que proporciona HTML5 discreto y validación de [jQuery](https://jquery.com/). Los atributos discretos tienen el formato `data-val-rule="Error Message"`, donde "rule" es el nombre de la regla de validación (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.). Si aparece un mensaje de error en el atributo, se mostrará como el valor del atributo `data-val-rule`. También hay atributos con el formato `data-val-ruleName-argumentName="argumentValue"` que aportan más información sobre la regla, por ejemplo, `data-val-maxlength-max="1024"`.

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternativas de aplicación auxiliar HTML a la aplicación auxiliar de etiquetas Input

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` y `Html.EditorFor` tienen características que se superponen a la aplicación auxiliar de etiquetas Input. La aplicación auxiliar de etiquetas Input establecerá automáticamente el atributo `type`, cosa que no ocurrirá con `Html.TextBox` ni `Html.TextBoxFor`. `Html.Editor` y `Html.EditorFor` controlan colecciones, objetos complejos y plantillas, pero la aplicación auxiliar de etiquetas Input no. La aplicación auxiliar de etiquetas Input, `Html.EditorFor` y `Html.TextBoxFor` están fuertemente tipados (usan expresiones lambda), pero `Html.TextBox` y `Html.Editor` no (usan nombres de expresión).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()` y `@Html.EditorFor()` usan una entrada `ViewDataDictionary` especial denominada `htmlAttributes` al ejecutar sus plantillas predeterminadas. Si lo desea, este comportamiento se puede enriquecer con parámetros `additionalViewData`. En la clave "htmlAttributes" se distingue entre mayúsculas y minúsculas. La clave "htmlAttributes" se controla de forma similar al objeto `htmlAttributes` pasado a aplicaciones auxiliares de etiqueta Input como `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nombres de expresión

El valor del atributo `asp-for` es una `ModelExpression` y la parte de la derecha de una expresión lambda. Por tanto, `asp-for="Property1"` se convierte en `m => m.Property1` en el código generado, motivo por el que no es necesario incluir el prefijo `Model`. Puede usar el carácter "@" para iniciar una expresión insertada y moverla antes de `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Se genera el siguiente HTML:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Con las propiedades de colección, `asp-for="CollectionProperty[23].Member"` genera el mismo nombre que `asp-for="CollectionProperty[i].Member"` si `i` tiene el valor `23`.

### <a name="navigating-child-properties"></a>Navegar a las propiedades secundarias

También se puede navegar a las propiedades secundarias a través de la ruta de acceso de propiedades del modelo de vista. Pensemos en una clase de modelo más compleja que contiene una propiedad secundaria `Address`.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

En la vista, enlazamos a `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Se genera el siguiente código HTML para `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>Colecciones y nombres de expresión

Como ejemplo, un modelo que contiene una matriz de `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

El método de acción:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

El siguiente código de Razor muestra cómo se tiene acceso a un elemento `Color` concreto:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

La plantilla *Views/Shared/EditorTemplates/String.cshtml*:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Ejemplo en el que se usa `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

El siguiente código de Razor muestra cómo iterar por una colección:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

La plantilla *Views/Shared/EditorTemplates/ToDoItem.cshtml*:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Use siempre `for` (y *no* `foreach`) para iterar por una lista. Evaluar un indizador en una expresión de LINQ puede ser costoso, con lo cual esa posibilidad hay que reducirla al mínimo.

&nbsp;

>[!NOTE]
>El código de ejemplo comentado anterior muestra cómo reemplazaríamos la expresión lambda por el operador `@` para tener acceso a cada elemento `ToDoItem` de la lista.

## <a name="the-textarea-tag-helper"></a>Aplicación auxiliar de etiquetas de área de texto (Textarea)

La aplicación auxiliar de etiquetas `Textarea Tag Helper` es similar a la aplicación auxiliar de etiquetas Input.

* Genera los atributos `id` y `name`, y los atributos de validación de datos del modelo de un elemento [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea).

* Permite establecer tipado fuerte.

* Alternativa de aplicación auxiliar HTML: `Html.TextAreaFor`.

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Se genera el siguiente código HTML:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>Aplicación auxiliar de etiquetas Label

* Genera el título de la etiqueta y el atributo `for` en un elemento [<label>](https://www.w3.org/wiki/HTML/Elements/label) de un nombre de expresión.

* Alternativa de aplicación auxiliar HTML: `Html.LabelFor`.

`Label Tag Helper` proporciona las siguientes ventajas con respecto a un elemento HTML Label puro:

* Obtendrá automáticamente el valor de la etiqueta descriptiva del atributo `Display`. El nombre para mostrar que se busca puede cambiar con el tiempo y la combinación del atributo `Display`, y la aplicación auxiliar de etiquetas Label aplicará el elemento `Display` en cualquier lugar donde se use.

* El código fuente contiene menos marcado.

* Permite establecer tipado fuerte con la propiedad de modelo.

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Se genera el siguiente código HTML para el elemento `<label>`:

```HTML
<label for="Email">Email Address</label>
```

La aplicación auxiliar de etiquetas Label genera el valor de atributo `for` de "Email", que es el identificador asociado al elemento `<input>`. Las aplicaciones auxiliares de etiqueta generan elementos `id` y `for` coherentes para que se puedan asociar correctamente. El título de este ejemplo proviene del atributo `Display`. Si el modelo no contuviera un atributo `Display`, el título sería el nombre de propiedad de la expresión.

## <a name="the-validation-tag-helpers"></a>Aplicaciones auxiliares de etiquetas de validación

Hay dos aplicaciones auxiliares de etiquetas de validación: `Validation Message Tag Helper` (que muestra un mensaje de validación relativo a una única propiedad del modelo) y `Validation Summary Tag Helper` (que muestra un resumen de los errores de validación). `Input Tag Helper` agrega atributos de validación del lado cliente HTML5 a los elementos de entrada en función de los atributos de anotación de datos de las clases del modelo. La validación también se realiza en el lado servidor. La aplicación auxiliar de etiquetas de validación muestra estos mensajes de error cuando se produce un error de validación.

### <a name="the-validation-message-tag-helper"></a>Aplicación auxiliar de etiquetas de mensaje de validación

* Agrega el atributo [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` al elemento [span](https://developer.mozilla.org/docs/Web/HTML/Element/span), que adjunta los mensajes de error de validación en el campo de entrada de la propiedad de modelo especificada. Cuando se produce un error de validación en el lado cliente, [jQuery](https://jquery.com/) muestra el mensaje de error en el elemento `<span>`.

* La validación también tiene lugar en el lado servidor. Puede que JavaScript esté deshabilitado en los clientes, mientras que hay algunas validaciones que solo se pueden realizar en el lado servidor.

* Alternativa de aplicación auxiliar HTML: `Html.ValidationMessageFor`.

`Validation Message Tag Helper` se usa con el atributo `asp-validation-for` en un elemento HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span).

```HTML
<span asp-validation-for="Email"></span>
```

La aplicación auxiliar de etiquetas de mensaje de validación generará el siguiente código HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

`Validation Message Tag Helper` suele usarse por lo general después de una aplicación auxiliar de etiquetas `Input` en la misma propiedad. Gracias a esto, se mostrarán todos los mensajes de error de validación cerca de la entrada que produjo el error.

> [!NOTE]
> Debe tener una vista con las referencias de script de JavaScript y de [jQuery](https://jquery.com/) adecuadas para la validación del lado cliente. Para más información, vea [Introduction to model validation in ASP.NET Core MVC](../models/validation.md) (Introducción a la validación de modelos en ASP.NET Core MVC).

Cuando se produce un error de validación del lado servidor (por ejemplo, porque haya una validación del lado servidor personalizada o porque la validación del lado cliente esté deshabilitada), MVC pone ese mensaje de error como cuerpo del elemento `<span>`.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Aplicación auxiliar de etiquetas de resumen de validación

* Tiene como destino todos los elementos `<div>` con el atributo `asp-validation-summary`.

* Alternativa de aplicación auxiliar HTML: `@Html.ValidationSummary`.

`Validation Summary Tag Helper` se usa para mostrar un resumen de los mensajes de validación. El valor de atributo `asp-validation-summary` puede ser cualquiera de los siguientes:

|asp-validation-summary|Mensajes de validación que se muestran|
|--- |--- |
|ValidationSummary.All|Nivel de modelo y de propiedad|
|ValidationSummary.ModelOnly|Modelo|
|ValidationSummary.None|Ninguna|

### <a name="sample"></a>Ejemplo

En el siguiente ejemplo, el modelo de datos se complementa con atributos `DataAnnotation`, lo que genera mensajes de error de validación sobre el elemento `<input>`.  Cuando se produce un error de validación, la aplicación auxiliar de etiquetas de validación muestra el mensaje de error:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

El código HTML generado (cuando el modelo es válido) es este:

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>Aplicación auxiliar de etiquetas de selección (Select)

* Genera el elemento [select](https://www.w3.org/wiki/HTML/Elements/select) y el elemento asociado [option](https://www.w3.org/wiki/HTML/Elements/option) de las propiedades del modelo.

* Tiene `Html.DropDownListFor` y `Html.ListBoxFor` como alternativa de aplicación auxiliar HTML.

El elemento `asp-for` de `Select Tag Helper` especifica el nombre de la propiedad de modelo del elemento [select](https://www.w3.org/wiki/HTML/Elements/select), mientras que `asp-items` especifica los elementos [option](https://www.w3.org/wiki/HTML/Elements/option).  Por ejemplo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

El método `Index` inicializa `CountryViewModel`, establece el país seleccionado y lo pasa a la vista `Index`.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

El método HTTP POST `Index` muestra la selección:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

La vista `Index`:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Genera el siguiente código HTML (con la selección "CA"):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> No se recomienda usar `ViewBag` o `ViewData` con la aplicación auxiliar de etiquetas Select. Un modelo de vista es más eficaz a la hora de proporcionar metadatos MVC y suele ser menos problemático.

El valor de atributo `asp-for` es un caso especial y no necesita un prefijo `Model`, mientras que los otros atributos de aplicación auxiliar de etiquetas sí (como `asp-items`).

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Enlace con enum

A menudo, conviene usar `<select>` con una propiedad `enum` y generar los elementos `SelectListItem` a partir de valores `enum`.

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

El método `GetEnumSelectList` genera un objeto `SelectList` para una enumeración.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Puede complementar la lista de enumeradores con el atributo `Display` para obtener una interfaz de usuario más completa:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Se genera el siguiente código HTML:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>Agrupamiento de opciones

El elemento HTML [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) se genera cuando el modelo de vista contiene uno o varios objetos `SelectListGroup`.

`CountryViewModelGroup` agrupa los elementos `SelectListItem` en los grupos "North America" y "Europe":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Aquí mostramos los dos grupos:

![Ejemplo de agrupamiento de opciones](working-with-forms/_static/grp.png)

El código HTML generado:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Selección múltiple

La aplicación auxiliar de etiquetas Select generará automáticamente el atributo [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) si la propiedad especificada en el atributo `asp-for` es `IEnumerable`. Por ejemplo, si tenemos el siguiente modelo:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Con la siguiente vista:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Se genera el siguiente código HTML:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Sin selección

Si ve que usa la opción "sin especificar" en varias páginas, puede crear una plantilla para no tener que repetir el código HTML:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

La plantilla *Views/Shared/EditorTemplates/CountryViewModel.cshtml*:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

La posibilidad de agregar elementos HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) no se limita exclusivamente a los casos en los que no se seleccionada nada**. Por ejemplo, el método de acción y vista siguientes generarán un código HTML similar al código anterior:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Se seleccionará el elemento `<option>` correcto (que contenga el atributo `selected="selected"`) en función del valor real de `Country`.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Recursos adicionales

* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
* [HTML Form element](https://www.w3.org/TR/html401/interact/forms.html) (Elemento HTML Form)
* [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) (Token de comprobación de solicitudes)
* [Enlace de modelos](xref:mvc/models/model-binding)
* [Introduction to model validation in ASP.NET Core MVC](xref:mvc/models/validation) (Introducción a la validación de modelos en ASP.NET Core MVC)
* [IAttributeAdapter Interface](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter) (Interfaz IAttributeAdapter)
* [Fragmentos de código de este documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)
