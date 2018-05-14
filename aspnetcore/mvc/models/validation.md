---
title: Validación de modelos en ASP.NET Core MVC
author: rachelappel
description: Obtenga información sobre la validación de modelos en ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: 1ab19fad90eab9f2da58b4d62615a85d71894218
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="model-validation-in-aspnet-core-mvc"></a>Validación de modelos en ASP.NET Core MVC

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introducción a la validación de modelos

Antes de que una aplicación almacene datos en una base de datos, dicha aplicación debe validar los datos. Es necesario comprobar los datos para detectar posibles amenazas de seguridad, verificar que su formato es adecuado para el tipo y el tamaño y que se ajustan a las reglas. La validación es necesaria aunque su implementación puede resultar tediosa y redundante. En MVC, la validación se produce en el cliente y el servidor.

Por suerte, .NET ha abstraído la validación en atributos de validación. Estos atributos contienen el código de validación, lo que reduce la cantidad de código que debe escribirse.

[Vea o descargue el ejemplo de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Atributos de validación

Los atributos de validación son una forma de configurar la validación del modelo, por lo que conceptualmente es similar a la validación en campos de tablas de base de datos. Esto incluye las restricciones, como la asignación de tipos de datos o los campos obligatorios. Entre otros tipos de validación se encuentran el de aplicar patrones a datos para aplicar reglas de negocio, como tarjetas de crédito, números de teléfono o direcciones de correo electrónico. Con los atributos de validación, es mucho más sencillo aplicar estos requisitos y son más fáciles de usar.

Aquí mostramos un modelo `Movie` anotado desde una aplicación que almacena información sobre películas y programas de TV. La mayoría de las propiedades son obligatorias y varias propiedades de cadena tienen requisitos de longitud. Además, hay una restricción de rango numérico para la propiedad `Price` de 0 a 999,99 $, junto con un atributo de validación personalizado.

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

Con solo leer el modelo se pueden detectar las reglas sobre los datos para esta aplicación, lo que facilita mantener el código. Aquí tenemos varios atributos de validación integradas conocidos:

* `[CreditCard]`: valida que la propiedad tiene formato de tarjeta de crédito.

* `[Compare]`: valida dos propiedades en una coincidencia de modelos.

* `[EmailAddress]`: valida que la propiedad tiene formato de correo electrónico.

* `[Phone]`: valida que la propiedad tiene formato de teléfono.

* `[Range]`: valida que el valor de propiedad se encuentra dentro del intervalo especificado.

* `[RegularExpression]`: valida que los datos coinciden con la expresión regular especificada.

* `[Required]`: hace que una propiedad sea obligatoria.

* `[StringLength]`: valida que una propiedad de cadena tiene como mucho la longitud máxima determinada.

* `[Url]`: valida que la propiedad tiene un formato de URL.

MVC admite cualquier atributo que se derive de `ValidationAttribute` a efectos de validación. En el espacio de nombres [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) pueden encontrarse muchos atributos de validación útiles.

Puede haber instancias en las que necesite más características de las que proporcionan los atributos integrados. En estos casos, puede crear atributos de validación personalizados derivando a partir de `ValidationAttribute` o cambiando el modelo para implementar `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Notas sobre el uso del atributo Required

Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que no aceptan valores NULL (como `decimal`, `int`, `float` y `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `Required`. La aplicación no realiza ninguna comprobación de validación del lado servidor para los tipos que no aceptan valores NULL que están marcados como `Required`.

El enlace de modelos de MVC, que no está relacionado con la validación y los atributos de validación, rechaza el envío de un campo de formulario que contenga un valor que falta o un espacio en blanco para un tipo que no acepta valores NULL. En ausencia de un atributo `BindRequired` en la propiedad de destino, el enlace de modelos omite datos que faltan para los tipos que no aceptan valores NULL, donde el campo de formulario está ausente en los datos entrantes del formulario.

El [atributo BindRequired](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (vea también [Personalizar el comportamiento de enlace de modelo con atributos](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) es útil para garantizar que los datos de formulario se completan. Cuando se aplica a una propiedad, el sistema de enlace de modelos requiere un valor para esa propiedad. Cuando se aplica a un tipo, el sistema de enlace de modelos requiere valores para todas las propiedades de ese tipo.

Cuando se usa un tipo [Nullable\<T >](/dotnet/csharp/programming-guide/nullable-types/) (por ejemplo, `decimal?` o `System.Nullable<decimal>`) y se marca como `Required`, se realiza una comprobación de validación del lado servidor como si la propiedad fuera un tipo que acepta valores NULL estándar (por ejemplo, un `string`).

La validación del lado cliente requiere un valor para un campo de formulario que se corresponda con una propiedad del modelo que se haya marcado como `Required` y para una propiedad de tipo que no acepta valores NULL que no ha marcado como `Required`. `Required` puede usarse para controlar el mensaje de error de validación del lado cliente.

## <a name="model-state"></a>Estado del modelo

El estado del modelo representa los errores de validación en valores de formulario HTML enviados.

MVC seguirá validando campos hasta que alcance el número máximo de errores (200 de forma predeterminada). Puede configurar este número si inserta el código siguiente en el método `ConfigureServices` dentro del archivo *Startup.cs*:

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Control de errores de estado del modelo

La validación de modelos se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada. En muchos casos, la reacción adecuada consiste en devolver una respuesta de error. Lo ideal sería que detallara el motivo por el motivo del error de validación del modelo.

Algunas aplicaciones optarán por seguir una convención estándar para tratar los errores de validación de modelos, en cuyo caso un filtro podría ser el lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con estados de modelo válidos y no válidos.

## <a name="manual-validation"></a>Validación manual

Después de completarse la validación y el enlace de modelos, es posible que quiera repetir partes de estos procesos. Por ejemplo, puede que un usuario haya escrito texto en un campo esperando recibir un entero, o puede que necesite calcular un valor para la propiedad del modelo.

Es posible que tenga que ejecutar manualmente la validación. Para ello, llame al método `TryValidateModel`, como se muestra aquí:

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validación personalizada

Los atributos de validación funcionan para la mayoría de las necesidades validación. Pero algunas reglas de validación son específicas de su negocio. Es posible que las reglas no sean técnicas de validación de datos comunes, como asegurarse de que un campo es obligatorio o se ajusta a un rango de valores. Para estos escenarios, los atributos de validación personalizados son una excelente solución. Es muy fácil crear sus propios atributos de validación personalizados en MVC. Solo tiene que heredar de `ValidationAttribute` e invalidar el método `IsValid`. El método `IsValid` acepta dos parámetros: el primero es un objeto denominado *value* y el segundo es un objeto `ValidationContext` denominado *validationContext*. El objeto *value* hace referencia al valor real del campo que va a validar el validador personalizado.

En el ejemplo siguiente, una regla de negocio indica que los usuarios no pueden especificar el género *Classic* para una película estrenada después de 1960. El atributo `[ClassicMovie]` comprueba primero el género y, si es un clásico, comprueba que la fecha de estreno es posterior a 1960. Si se estrenó después de 1960, se produce un error de validación. El atributo acepta un parámetro de entero que representa el año que puede usar para validar los datos. Puede capturar el valor del parámetro en el constructor del atributo, como se muestra aquí:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

La variable `movie` anterior representa un objeto `Movie` que contiene los datos del envío del formulario para validarlos. En este caso, el código de validación comprueba la fecha y el género en el método `IsValid` de la clase `ClassicMovieAttribute` según las reglas. Después de validar correctamente `IsValid`, devuelve un código `ValidationResult.Success` y, cuando se produce un error de validación, un `ValidationResult` con un mensaje de error. Cuando un usuario modifica el campo `Genre` y envía el formulario, el método `IsValid` de `ClassicMovieAttribute` comprueba si la película es un clásico. Al igual que con cualquier atributo integrado, aplique `ClassicMovieAttribute` a una propiedad como `ReleaseDate` para asegurarse de que se produce la validación, tal como se muestra en el ejemplo de código anterior. Puesto que el ejemplo solo funciona con tipos `Movie`, una mejor opción es usar `IValidatableObject` tal y como se muestra en el párrafo siguiente.

Si lo prefiere, puede colocar este mismo código en el modelo, implementando el método `Validate` en la interfaz `IValidatableObject`. Aunque los atributos de validación personalizada funcionan bien para validar propiedades individuales, puede implementar `IValidatableObject` para implementar la validación de nivel de clase como se ve aquí.

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Validación del lado cliente

La validación del lado cliente es una gran ventaja para los usuarios. Permite ahorrar tiempo que, de lo contrario, pasarían esperando un recorrido de ida y vuelta al servidor. En términos comerciales, incluso unas pocas fracciones de segundos multiplicadas por cientos de veces al día suman una gran cantidad de tiempo, gastos y frustración. La validación inmediata y sencilla permite a los usuarios trabajar de forma más eficiente y generar una entrada y salida de datos de mayor calidad.

Debe tener una vista con las referencias de script de JavaScript adecuadas para que la validación del lado cliente funcione como se ve aquí.

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

El script [Validación discreta de jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) es una biblioteca front-end personalizada de Microsoft que se basa en el conocido complemento [Validación de jQuery](https://jqueryvalidation.org/). Si no usa Validación discreta de jQuery, deberá codificar la misma lógica de validación dos veces: una vez en los atributos de validación del lado servidor en las propiedades del modelo y luego en los scripts del lado cliente (los ejemplos del método [`validate()`](https://jqueryvalidation.org/validate/) de Validación de jQuery muestran lo complejo que podría resultar). En su lugar, las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y las [aplicaciones auxiliares de HTML](xref:mvc/views/overview) de MVC pueden usar los atributos de validación y escribir metadatos de las propiedades del modelo para representar [atributos de datos](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) HTML 5 en los elementos de formulario que necesitan validación. MVC genera los atributos `data-` para los atributos integrados y los personalizados. La Validación discreta de jQuery analiza después los atributos `data-` y pasa la lógica a Validación de jQuery. De este modo, la lógica de validación del lado servidor se "copia" de manera eficaz en el cliente. Puede mostrar errores de validación en el cliente usando las aplicaciones auxiliares de etiquetas relevantes, como se muestra aquí:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Las anteriores aplicaciones auxiliares de etiqueta representan el código HTML siguiente. Tenga en cuenta que los atributos `data-` en los resultados HTML corresponden a los atributos de validación para la propiedad `ReleaseDate`. El atributo `data-val-required` siguiente contiene un mensaje de error que se muestra si el usuario no rellena el campo de fecha de estreno. La Validación discreta de jQuery pasa este valor al método [`required()`](https://jqueryvalidation.org/required-method/) de la Validación de jQuery, que muestra un mensaje en el elemento **\<span>** que lo acompaña.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

La validación del lado cliente impide realizar el envío hasta que el formulario sea válido. El botón Enviar ejecuta JavaScript para enviar el formulario o mostrar mensajes de error.

MVC determina los valores de atributo de tipo según el tipo de datos de .NET de una propiedad, posiblemente invalidada usando los atributos `[DataType]`. El atributo `[DataType]` de base no realiza ninguna validación en el lado servidor. Los exploradores eligen sus propios mensajes de error y muestran estos errores cuando quieren, si bien el paquete Validación discreta de jQuery puede invalidar esos mensajes y mostrarlos de manera coherente con otros. Esto es más evidente cuando los usuarios aplican subclases de `[DataType]`, como por ejemplo, `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Agregar validación a formularios dinámicos

Como la Validación discreta de jQuery pasa la lógica de validación a Validación de jQuery cuando la página se carga por primera vez, los formularios generados dinámicamente no exhiben la validación automáticamente. En su lugar, hay que indicar a Validación discreta de jQuery que analice el formulario dinámico inmediatamente después de crearlo. Por ejemplo, en este código se muestra cómo es posible configurar la validación del lado cliente en un formulario agregado mediante AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

El método `$.validator.unobtrusive.parse()` acepta un selector de jQuery para su único argumento. Este método indica a Validación discreta de jQuery que analice los atributos `data-` de formularios dentro de ese selector. Los valores de estos atributos se pasan al complemento Validación de jQuery para que el formulario muestre las reglas de validación del lado cliente que quiera.

### <a name="add-validation-to-dynamic-controls"></a>Agregar validación a controles dinámicos

También puede actualizar las reglas de validación en un formulario cuando se generan dinámicamente controles individuales, como `<input/>`s y `<select/>`s. No se pueden pasar directamente selectores para estos elementos al método `parse()` porque el formulario adyacente ya se ha analizado y no se actualiza. En lugar de esto, quite primero los datos de validación existentes y vuelva a analizar todo el formulario, como se muestra aquí:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Puede crear lógica del lado cliente para el atributo personalizado y [Validación discreta](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html), que crea un adaptador para la [validación de jQuery](http://jqueryvalidation.org/documentation/), la ejecutará en el cliente automáticamente como parte de la validación. El primer paso consiste en controlar qué atributos de datos se agregan mediante la implementación de la interfaz `IClientModelValidator`, como se muestra aquí:

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Los atributos que implementan esta interfaz pueden agregar atributos HTML a los campos generados. Al examinar la salida del elemento `ReleaseDate` muestra que el HTML es similar al ejemplo anterior, excepto que ahora hay un atributo `data-val-classicmovie` que se definió en el método `AddValidation` de `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

La validación discreta usa los datos en los atributos `data-` para mostrar mensajes de error. Pero jQuery no sabe nada de reglas o mensajes hasta que estas se agregan al objeto `validator` de jQuery. Esto se muestra en el ejemplo siguiente, que agrega un método denominado `classicmovie` que contiene código de validación de cliente personalizada para el objeto `validator` de jQuery. [Aquí](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) encontrará una explicación del método unobtrusive.adapters.add.

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

Ahora jQuery tiene información para ejecutar la validación personalizada de JavaScript, así como el mensaje de error que se debe mostrar si ese código de validación devuelve false.

## <a name="remote-validation"></a>Validación remota

La validación remota es una característica excelente que se usa cuando necesita validar los datos del cliente con datos del servidor. Por ejemplo, puede que la aplicación necesite comprobar si ya se está usando un nombre de usuario o una dirección de correo, y debe consultar una gran cantidad de datos para hacerlo. El hecho de descargar grandes conjuntos de datos para validar un campo o unos pocos campos consume demasiados recursos. También podría exponer información confidencial. Una alternativa a esto consiste en realizar una solicitud de ida y vuelta para validar un campo.

Puede implementar la validación remota en un proceso de dos pasos. Primero, debe anotar el modelo con el atributo `[Remote]`. El atributo `[Remote]` acepta varias sobrecargas que puede usar para dirigir el JavaScript del lado cliente al código adecuado al que se debe llamar. El ejemplo siguiente señala al método de acción `VerifyEmail` del controlador `Users`.

[!code-csharp[](validation/sample/User.cs?range=7-8)]

El segundo paso consiste en colocar el código de validación en el método de acción correspondiente, tal como se define en el atributo `[Remote]`. Según la documentación del método [`remote()`](https://jqueryvalidation.org/remote-method/) de Validación de jQuery:

> La respuesta del lado servidor debe ser una cadena JSON que debe ser `"true"` para elementos válidos y puede ser `"false"`, `undefined` o `null` para elementos no válidos, usando el mensaje de error predeterminado. Si la respuesta del lado servidor es una cadena, como por ejemplo, `"That name is already taken, try peter123 instead"`, dicha cadena se mostrará como un mensaje de error personalizado en lugar del predeterminado.

La definición del método `VerifyEmail()` sigue estas reglas, tal y como se muestra abajo. Devuelve un mensaje de error de validación si la dirección de correo ya existe o muestra `true` si la dirección de correo está disponible, y ajusta el resultado en un objeto `JsonResult`. Después, el lado cliente puede usar el valor devuelto para continuar o mostrar el error si es necesario.

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

Cuando los usuarios escriben una dirección de correo, JavaScript en la vista hace una llamada remota para comprobar si esa dirección ya existe y, de ser así, se muestra el mensaje de error. En caso contrario, el usuario puede enviar el formulario como de costumbre.

La propiedad `AdditionalFields` del atributo `[Remote]` es útil para validar combinaciones de campos con los datos del servidor. Por ejemplo, si el modelo `User` anterior tenía dos propiedades adicionales llamadas `FirstName` y `LastName`, recomendamos comprobar que ningún usuario actual tenga ya ese par de nombres. Tendrá que definir las nuevas propiedades como se muestra en el código siguiente:

[!code-csharp[](validation/sample/User.cs?range=10-13)]

`AdditionalFields` podría haberse configurado explícitamente para las cadenas `"FirstName"` y `"LastName"`, pero al usar el operador [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) de este modo se simplifica la refactorización posterior. El método de acción para realizar la validación debe aceptar dos argumentos, uno para el valor de `FirstName` y otro para el valor de `LastName`.

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

Cuando los usuarios escriben su nombre y sus apellidos, JavaScript hace lo siguiente:

* Realiza una llamada remota para comprobar si ese par de nombres ya se está usando.
* Si el par de nombres ya existe, se muestra un mensaje de error. 
* Si está disponible, el usuario puede enviar el formulario.

Si necesita validar dos o más campos adicionales con el atributo `[Remote]`, proporciónelos como una lista delimitada por comas. Por ejemplo, para agregar una `MiddleName` propiedad en el modelo, establezca el `[Remote]` atributo tal como se muestra en el código siguiente:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, al igual que todos los argumentos de atributo, debe ser una expresión constante. Por lo tanto, no se debe usar una [cadena interpolada](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) ni llamar a [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) para inicializar `AdditionalFields`. Para cada campo adicional que agregue al atributo `[Remote]`, debe agregar otro argumento al método de acción de controlador correspondiente.
