---
title: Aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core
author: rick-anderson
description: Describe la integrada de aplicaciones auxiliares de etiquetas que se utilizan con formularios.
keywords: "Núcleo de ASP.NET, aplicación auxiliar de etiquetas, TagHelper, formulario de HTML, formularios"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3f7792d7458013f837a48ca2caa459f35658f02
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Introducción al uso de aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), y [Jerrie Pelser](https://github.com/jerriep)

Este documento muestra cómo trabajar con formularios y los elementos HTML que se utilizan habitualmente en un formulario. El código HTML [formulario](https://www.w3.org/TR/html401/interact/forms.html) elemento proporciona el uso de aplicaciones web mecanismo principal para enviar datos al servidor. La mayor parte de este documento describe [aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) y cómo puede ayudarle a crear productiva sólidos formularios HTML. Le recomendamos que lea [Introducción a las aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) antes de leer este documento.

En muchos casos, las aplicaciones auxiliares HTML proporcionan un enfoque alternativo para una aplicación auxiliar de etiqueta específico, pero es importante reconocer que aplicaciones auxiliares de etiquetas no reemplazan métodos auxiliares HTML y no es una aplicación auxiliar de etiquetas para cada aplicación auxiliar HTML. Cuando existe una alternativa de la aplicación auxiliar HTML, se menciona.

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a>La aplicación auxiliar de etiqueta de formulario

El [formulario](https://www.w3.org/TR/html401/interact/forms.html) auxiliar de etiquetas:

* Genera el código HTML [ \<formulario >](https://www.w3.org/TR/html401/interact/forms.html) `action` valor de atributo para una acción de controlador MVC o la ruta con nombre

* Genera oculto [comprobación de solicitud de Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para impedir la falsificación de solicitud entre sitios (cuando se usa con el `[ValidateAntiForgeryToken]` atributo en el método de acción HTTP Post)

* Proporciona el `asp-route-<Parameter Name>` atributo, donde `<Parameter Name>` se agrega a los valores de ruta. El `routeValues` parámetros `Html.BeginForm` y `Html.BeginRouteForm` proporcionan una funcionalidad similar.

* Una alternativa de la aplicación auxiliar HTML `Html.BeginForm` y`Html.BeginRouteForm`

Ejemplo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

El Ayudante de etiqueta de formulario anterior genera el siguiente código HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

El tiempo de ejecución MVC genera el `action` valor del atributo de los atributos de la aplicación auxiliar de etiqueta de formulario `asp-controller` y `asp-action`. La aplicación auxiliar de etiqueta de formulario también genera oculto [comprobación de solicitud de Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para impedir la falsificación de solicitud entre sitios (cuando se usa con el `[ValidateAntiForgeryToken]` atributo en el método de acción HTTP Post). Protección de un formulario HTML puro de falsificación de solicitudes entre sitios es difícil, la aplicación auxiliar de etiqueta de formulario proporciona este servicio.

### <a name="using-a-named-route"></a>Uso de una ruta con nombre

El `asp-route` atributo de aplicación auxiliar de etiquetas también puede generar el marcado para el código HTML `action` atributo. Una aplicación con un [ruta](../../fundamentals/routing.md) denominado `register` podría usar el siguiente marcado de la página de registro:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Muchas de las vistas en el *Views/Account* carpeta (generan cuando se crea una nueva aplicación web con *cuentas de usuario individuales*) contienen el [returnurl de ruta de asp](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atributo:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [2]}} -->

```none
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
   ```

>[!NOTE]
>Con las plantillas incorporadas, `returnUrl` solo se rellena automáticamente cuando intenta obtener acceso a un recurso autorizado, pero no son autentica o autoriza. Si trata de un acceso no autorizado, el middleware de seguridad le redirige a la página de inicio de sesión con el `returnUrl` establecido.

## <a name="the-input-tag-helper"></a>La aplicación auxiliar de la etiqueta de entrada

La aplicación auxiliar de etiqueta de entrada se enlaza a un elemento HTML [ \<entrada >](https://www.w3.org/wiki/HTML/Elements/input) elemento a una expresión de modelo en la vista de razor.

Sintaxis:

```HTML
<input asp-for="<Expression Name>" />
   ```

La aplicación auxiliar de etiqueta de entrada:

* Genera el `id` y `name` atributos HTML para el nombre de la expresión especificada en el `asp-for` atributo. `asp-for="Property1.Property2"` es equivalente a `m => m.Property1.Property2`. El nombre de la expresión es lo que se usa para la `asp-for` valor de atributo. Consulte la [nombres de la expresión](#expression-names) sección para obtener información adicional.

* Establece el código HTML `type` según el tipo de modelo de valor de atributo y [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados a la propiedad de modelo

* No se sobrescribirá el código HTML `type` valor de atributo cuando se especifica uno

* Genera [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributos de validación de [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados a las propiedades del modelo

* Tiene una característica de la aplicación auxiliar HTML que se superponen con `Html.TextBoxFor` y `Html.EditorFor`. Consulte la **alternativas de la aplicación auxiliar HTML para la aplicación auxiliar de etiqueta de entrada** sección para obtener más información.

* Proporciona el establecimiento inflexible de tipos. Si el nombre de los cambios de propiedad y no actualiza la aplicación auxiliar de etiqueta obtendrá un error similar al siguiente:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

El `Input` etiqueta auxiliar establece el código HTML `type` atributo basado en el tipo. NET. En la tabla siguiente se enumera algunos tipos comunes de .NET y el tipo HTML generado (no todos los tipos .NET aparece).

|Tipo de .NET|Tipo de entrada|
|---|---|
|Bool|tipo = "checkbox"|
|Cadena|tipo = "text"|
|DateTime|tipo = "datetime"|
|Byte|tipo = "number"|
|Valor int.|tipo = "number"|
|Single, Double|tipo = "number"|


La siguiente tabla muestra algunos común [las anotaciones de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos que la aplicación auxiliar de la etiqueta de entrada se asigne a tipos de entrada específicos (no cada atributo de validación se muestra):


|Atributo|Tipo de entrada|
|---|---|
|[EmailAddress]|tipo = "email"|
|[Url]|tipo = "url"|
|[HiddenInput]|tipo = "hidden"|
|[Phone]|tipo = "tel"|
|[DataType(DataType.Password)]| tipo = "contraseña"|
|[DataType(DataType.Date)]| tipo = "fecha"|
|[DataType(DataType.Time)]| tipo = "hora"|


Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

El código anterior genera el siguiente código HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
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

Las anotaciones de datos que se aplica a la `Email` y `Password` propiedades generan metadatos en el modelo. La aplicación auxiliar de etiqueta de entrada usa los metadatos del modelo y produce [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributos (vea [validación del modelo](../models/validation.md)). Estos atributos describen los validadores para adjuntar a los campos de entrada. Esto proporciona HTML5 discreto y [jQuery](https://jquery.com/) validación. Los atributos discretos tienen el formato `data-val-rule="Error Message"`, donde la regla es el nombre de la regla de validación (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etcetera.) Si un mensaje de error se proporciona en el atributo, se muestra como el valor de la `data-val-rule` atributo. También hay atributos del formulario `data-val-ruleName-argumentName="argumentValue"` que proporcionan detalles adicionales sobre la regla, por ejemplo, `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternativas de la aplicación auxiliar HTML para la aplicación auxiliar de etiqueta de entrada

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` y `Html.EditorFor` tienen características a la aplicación auxiliar de etiqueta de entrada que se superponen. La aplicación auxiliar de etiqueta de entrada se establecerá automáticamente el `type` atributo; `Html.TextBox` y `Html.TextBoxFor` no. `Html.Editor`y `Html.EditorFor` controlar colecciones, objetos complejos y plantillas; no lo hace la aplicación auxiliar de etiqueta de entrada. La aplicación auxiliar etiqueta de entrada, `Html.EditorFor` y `Html.TextBoxFor` están fuertemente tipados (usar expresiones lambda;) `Html.TextBox` y `Html.Editor` no son (usan nombres de la expresión).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`y `@Html.EditorFor()` usar una clase especial `ViewDataDictionary` entrada denominada `htmlAttributes` al ejecutar sus plantillas predeterminadas. Este comportamiento es aumentar opcionalmente utilizando `additionalViewData` parámetros. La clave "htmlAttributes" distingue mayúsculas de minúsculas. La clave "htmlAttributes" se controla de forma similar a la `htmlAttributes` objeto pasado como entrada aplicaciones auxiliares como `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nombres de la expresión

El `asp-for` valor del atributo es un `ModelExpression` y el lado derecho de una expresión lambda. Por lo tanto, `asp-for="Property1"` se convierte en `m => m.Property1` en el código generado, motivo por el que no es necesario con el prefijo `Model`. Puede usar el "@" caracteres para iniciar una expresión en línea y mover antes el `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Genera lo siguiente:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Con las propiedades de colección, `asp-for="CollectionProperty[23].Member"` genera el mismo nombre que `asp-for="CollectionProperty[i].Member"` cuando `i` tiene el valor `23`.

### <a name="navigating-child-properties"></a>Navegar por propiedades secundarias

También puede navegar a propiedades secundarias mediante la ruta de acceso de propiedad del modelo de vista. Considere la posibilidad de una clase de modelo más compleja que contiene un elemento secundario `Address` propiedad.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

En la vista, se enlazan a `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

El siguiente código HTML se genera para `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
   ```

### <a name="expression-names-and-collections"></a>Nombres de la expresión y colecciones

Ejemplo de un modelo que contiene una matriz de `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

El método de acción:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
   ```

El código Razor siguiente muestra cómo tener acceso a un determinado `Color` elemento:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

El *Views/Shared/EditorTemplates/String.cshtml* plantilla:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Uso de ejemplo `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

El código Razor siguiente muestra cómo recorrer en iteración una colección:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

El *Views/Shared/EditorTemplates/ToDoItem.cshtml* plantilla:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Utilice siempre `for` (y *no* `foreach`) para recorrer en iteración una lista. Evaluar un indizador en una LINQ expresión puede ser costosa y debe reducirse.

&nbsp;

>[!NOTE]
>El código de ejemplo comentado anterior muestra cómo podría reemplazar la expresión lambda con la `@` operador para tener acceso a cada uno de ellos `ToDoItem` en la lista.

## <a name="the-textarea-tag-helper"></a>La aplicación auxiliar de etiqueta Textarea

El `Textarea Tag Helper` auxiliar de etiqueta es similar a la aplicación auxiliar de etiqueta de entrada.

* Genera el `id` y `name` atributos y los atributos de validación de datos del modelo para un [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.

* Proporciona el establecimiento inflexible de tipos.

* Alternativa de la aplicación auxiliar HTML:`Html.TextAreaFor`

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Se genera el siguiente código HTML:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6, 7, 8]}} -->

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

## <a name="the-label-tag-helper"></a>La aplicación auxiliar de etiqueta de etiqueta

* Genera el título de la etiqueta y `for` del atributo en un [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) (elemento) para un nombre de expresión

* Alternativa de la aplicación auxiliar HTML: `Html.LabelFor`.

El `Label Tag Helper` proporciona las siguientes ventajas con respecto a un elemento label HTML puro:

* Obtendrá automáticamente el valor de etiqueta descriptiva de la `Display` atributo. Puede cambiar el nombre para mostrar deseado con el tiempo y la combinación de `Display` atributo y etiqueta etiqueta auxiliar se aplicarán la `Display` en cualquier lugar en el se utiliza.

* Menos marcado en el código fuente

* Establecimiento inflexible tipos con la propiedad de modelo.

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

El siguiente código HTML se genera para el `<label>` elemento:

```HTML
<label for="Email">Email Address</label>
   ```

La aplicación auxiliar de la etiqueta etiqueta genera el `for` valor de atributo de "Email", que es el identificador asociado a la `<input>` elemento. Las aplicaciones auxiliares de etiqueta generar coherente `id` y `for` elementos, para que puedan asociados correctamente. La descripción de este ejemplo proviene de la `Display` atributo. Si el modelo no contiene un `Display` atributo, el título sería el nombre de propiedad de la expresión.

## <a name="the-validation-tag-helpers"></a>Aplicaciones auxiliares de etiquetas de validación

Hay dos aplicaciones auxiliares de etiquetas de validación. El `Validation Message Tag Helper` (que muestra un mensaje de validación de una sola propiedad en el modelo) y el `Validation Summary Tag Helper` (que muestra un resumen de errores de validación). El `Input Tag Helper` agrega atributos de validación de lado de cliente de HTML5 para los elementos que basan en datos de los atributos de anotación en las clases de modelo de entrada. También se realiza la validación en el servidor. La aplicación auxiliar de etiqueta de validación muestra estos mensajes de error cuando se produce un error de validación.

### <a name="the-validation-message-tag-helper"></a>El Ayudante de etiqueta de mensaje de validación

* Agrega el [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atribuir a la [abarcan](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, que asocia los mensajes de error de validación en el campo de entrada de la propiedad de modelo especificado. Cuando se produce un error de validación del lado cliente, [jQuery](https://jquery.com/) muestra el mensaje de error en la `<span>` elemento.

* La validación también tiene lugar en el servidor. Los clientes pueden tener JavaScript deshabilitado y alguna validación solo puede realizarse en el servidor.

* Alternativa de la aplicación auxiliar HTML:`Html.ValidationMessageFor`

El `Validation Message Tag Helper` se utiliza con la `asp-validation-for` atributo en una HTML [abarcan](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.

```HTML
<span asp-validation-for="Email"></span>
   ```

El Ayudante de etiqueta de mensaje de validación generará el siguiente código HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Se usa generalmente la `Validation Message Tag Helper` después de un `Input` auxiliar de etiqueta para la misma propiedad. Si lo hace, muestra los mensajes de error de validación cerca de la entrada que produjo el error.

> [!NOTE]
> Debe tener una vista con el código de JavaScript correcto y [jQuery](https://jquery.com/) referencias en su lugar para la validación del lado cliente de script. Vea [validación del modelo](../models/validation.md) para obtener más información.

Cuando produce un error de validación del lado servidor (por ejemplo cuando tiene validación del lado servidor personalizado o de validación del lado cliente está deshabilitado), MVC coloca ese mensaje de error como el cuerpo de la `<span>` elemento.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>El Ayudante de validación del Summary (etiqueta)

* Destinos `<div>` elementos con el `asp-validation-summary` atributo

* Alternativa de la aplicación auxiliar HTML:`@Html.ValidationSummary`

El `Validation Summary Tag Helper` se usa para mostrar un resumen de mensajes de validación. La `asp-validation-summary` valor de atributo puede ser cualquiera de las siguientes acciones:

|Resumen de validación de ASP|Mensajes de validación que se muestran|
|--- |--- |
|ValidationSummary.All|Nivel de propiedad y el modelo|
|ValidationSummary.ModelOnly|Modelo|
|ValidationSummary.None|Ninguna|

### <a name="sample"></a>Ejemplo

En el ejemplo siguiente, el modelo de datos se decora con `DataAnnotation` atributos, que genera mensajes de error de validación en el `<input>` elemento.  Cuando se produce un error de validación, la aplicación auxiliar de etiqueta de validación muestra el mensaje de error:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

El código HTML generado (cuando el modelo es válido):

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 8, 9, 12, 13]}} -->

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
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

## <a name="the-select-tag-helper"></a>La aplicación auxiliar de la etiqueta Select

* Genera [seleccione](https://www.w3.org/wiki/HTML/Elements/select) y [opción](https://www.w3.org/wiki/HTML/Elements/option) elementos para las propiedades del modelo.

* Una alternativa de la aplicación auxiliar HTML `Html.DropDownListFor` y`Html.ListBoxFor`

El `Select Tag Helper` `asp-for` especifica el nombre de la propiedad de modelo para el [seleccione](https://www.w3.org/wiki/HTML/Elements/select) elemento y `asp-items` especifica la [opción](https://www.w3.org/wiki/HTML/Elements/option) elementos.  Por ejemplo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

El `Index` método inicializa la `CountryViewModel`, Establece el país seleccionado y lo pasa a la `Index` vista.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` método muestra la selección:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

El `Index` vista:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Que genera el siguiente código HTML (con "CA" seleccionada):

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6]}} -->

```HTML
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
> Se recomienda no usar `ViewBag` o `ViewData` a la aplicación auxiliar seleccione etiqueta. Un modelo de vista es más eficaz al proporcionar metadatos MVC y generalmente menos problemática.

El `asp-for` valor de atributo es un caso especial y no requiere un `Model` de prefijo, los otro no de atributos de etiqueta auxiliar (como `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Enlace de enum

A menudo resulta cómodo para utilizarlo `<select>` con una `enum` propiedad y generar el `SelectListItem` elementos desde el `enum` valores.

Ejemplo:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

El `GetEnumSelectList` método genera una `SelectList` objeto para una enumeración.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Puede decorar la lista de enumeradores con el `Display` atributo para obtener una interfaz de usuario más enriquecida:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Se genera el siguiente código HTML:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [4, 5]}} -->

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

### <a name="option-group"></a>Grupo de opciones

El código HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento se genera cuando el modelo de vista contiene uno o varios `SelectListGroup` objetos.

El `CountryViewModelGroup` grupos el `SelectListItem` elementos en los grupos "América del Norte" y "Europe":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Los dos grupos se muestran a continuación:

![ejemplo de grupo de opción](working-with-forms/_static/grp.png)

El código HTML generado:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]}} -->

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

La aplicación auxiliar seleccione etiqueta generará automáticamente la [varios = "varios"](http://w3c.github.io/html-reference/select.html) atributo si la propiedad especificada en el `asp-for` atributo es un `IEnumerable`. Por ejemplo, dado el modelo siguiente:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Con la siguiente vista:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Genera el siguiente código HTML:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3]}} -->

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

### <a name="no-selection"></a>No hay nada seleccionado

Si se encuentra con la opción "no se ha especificado" en varias páginas, puede crear una plantilla para eliminar el código HTML de repetición:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

El *Views/Shared/EditorTemplates/CountryViewModel.cshtml* plantilla:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Agregar HTML [ \<opción >](https://www.w3.org/wiki/HTML/Elements/option) elementos no se limita a la *ninguna selección* caso. Por ejemplo, el método de acción y la vista siguiente generará HTML similar al código anterior:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

El valor correcto `<option>` se seleccionará el elemento (contienen el `selected="selected"` atributo) según la actual `Country` valor.

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [5]}} -->

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

* [Aplicaciones auxiliares de etiquetas](tag-helpers/intro.md)

* [Elemento de formulario HTML](https://www.w3.org/TR/html401/interact/forms.html)

* [Solicitud de Token de comprobación](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Enlace de modelos](../models/model-binding.md)

* [Validación del modelo](../models/validation.md)

* [anotaciones de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [Fragmentos de este documento de código](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
