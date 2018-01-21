---
title: "Validación del modelo en MVC de ASP.NET Core"
author: rachelappel
description: "Obtenga información acerca de la validación del modelo en MVC de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 91db17e103723ac411a2ad4f3f9549860f250cce
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Introducción a la validación del modelo en MVC de ASP.NET Core

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introducción a la validación del modelo

Antes de que una aplicación almacena datos en una base de datos, la aplicación debe validar los datos. Datos se deben proteger de posibles amenazas de seguridad, comprobadas que formatearlo correctamente por tipo y tamaño, y debe ajustarse a las reglas. La validación es necesaria aunque puede ser tediosa y redundantes. En MVC, la validación se produce en el cliente y el servidor.

Afortunadamente, .NET se abstrae la validación en los atributos de validación. Estos atributos contienen el código de validación, lo que reduce la cantidad de código que debe escribirse.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Atributos de validación

Atributos de validación son una forma de configurar la validación del modelo por lo que es similar conceptualmente a la validación en los campos de tablas de base de datos. Esto incluye las restricciones como la asignación de tipos de datos o campos obligatorios. Otros tipos de validación son aplicar patrones a datos para aplicar las reglas de negocios, como una tarjeta de crédito, número de teléfono o dirección de correo electrónico. Atributos de validación Asegúrese de aplicar estos requisitos mucho más sencillas y más fáciles de usar.

A continuación se muestra un anotado `Movie` modelo desde una aplicación que almacena información acerca de películas y programas de TV. La mayoría de las propiedades es necesaria y varias propiedades de cadena tienen requisitos de longitud. Además, hay una restricción de rango numérico en su lugar para la `Price` propiedad de 0 a 999,99 pesos, junto con un atributo de validación personalizado.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

Simplemente leer a través del modelo revela las reglas sobre los datos para esta aplicación, lo que facilita mantener el código. A continuación se muestran varios atributos de validación integradas populares:

* `[CreditCard]`: Valida la propiedad tiene un formato de tarjeta de crédito.

* `[Compare]`: Valida dos propiedades en una coincidencia de modelos.

* `[EmailAddress]`: Valida la propiedad tiene un formato de correo electrónico.

* `[Phone]`: Valida la propiedad tiene un formato de teléfono.

* `[Range]`: Valida el valor de propiedad se encuentra dentro del intervalo especificado.

* `[RegularExpression]`: Valida que los datos coincide con la expresión regular especificada.

* `[Required]`: Hace que una propiedad necesaria.

* `[StringLength]`: Valida que una propiedad de cadena a lo sumo tiene la longitud máxima determinada.

* `[Url]`: Valida la propiedad tiene un formato de dirección URL.

MVC admite cualquier atributo que se deriva de `ValidationAttribute` para la validación. Muchos de los atributos de validación útil pueden encontrarse en el [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) espacio de nombres.

Puede haber instancias donde necesita más características que proporcionan atributos integrados. En esas ocasiones, puede crear atributos de validación personalizado derivando de `ValidationAttribute` o cambiar el modelo para implementar `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Notas sobre el uso del atributo Required

Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que no aceptan valores NULL (como `decimal`, `int`, `float` y `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `Required`. La aplicación realiza ninguna comprobación de validación del lado servidor para los tipos que no aceptan valores null que se marcan `Required`.

Enlace de modelo MVC, que no está relacionada con la validación y los atributos de validación, rechaza el envío de un campo de formulario que contiene un valor o un espacio en blanco para un tipo que no acepta valores null que faltan. En ausencia de un `BindRequired` atributo en la propiedad de destino, el enlace de modelos omite datos que faltan para los tipos que no aceptan valores NULL, donde existe el campo de formulario de los datos entrantes del formulario.

El [BindRequired atributo](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (consulte también [personalizar el comportamiento de enlace de modelo con atributos](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) es útil para garantizar que los datos de formulario finalice. Cuando se aplica a una propiedad, el sistema de enlace de modelo requiere un valor para esa propiedad. Cuando se aplica a un tipo, el sistema de enlace de modelo requiere valores para todas las propiedades de ese tipo.

Cuando se usa un [Nullable\<T > tipo](/dotnet/csharp/programming-guide/nullable-types/) (por ejemplo, `decimal?` o `System.Nullable<decimal>`) y marcarla `Required`, se realiza una comprobación de validación del lado servidor como si fuera de la propiedad de un tipo que acepta valores NULL estándar (por ejemplo, un `string`).

Validación del lado cliente requiere un valor para un campo de formulario que se corresponde con una propiedad de modelo que se ha marcado `Required` y para una propiedad de tipo que no acepta valores null que no ha marcado `Required`. `Required`puede usarse para controlar el mensaje de error de validación del lado cliente.

## <a name="model-state"></a>Estado del modelo

Estado del modelo representa los errores de validación en valores de formulario HTML enviados.

MVC continuará validando campos hasta que alcanza el número máximo de errores (200 de forma predeterminada). Puede configurar este número insertando el código siguiente en el `ConfigureServices` método en el *Startup.cs* archivo:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Estado del modelo de control de errores

Validación del modelo se produce antes de cada acción de controlador que se invoca y es responsabilidad del método de acción para inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada. En muchos casos, la reacción adecuada es devolver una respuesta de error, lo ideal sería que detalla el motivo por el motivo del error de validación del modelo.

Algunas aplicaciones se elegirá seguir una convención estándar para tratar los errores de validación del modelo, en cuyo caso un filtro puede ser un lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con los Estados del modelo válidos y no válidos.

## <a name="manual-validation"></a>Validación manual

Cuando se completen la validación y el enlace de modelos, puede que desee repetir partes de él. Por ejemplo, un usuario ha escrito texto en un campo que se esperaba un entero, o puede que necesite calcular el valor de propiedad del modelo.

Debe ejecutar manualmente la validación. Para ello, llame a la `TryValidateModel` método, como se muestra aquí:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validación personalizada

Funcionan los atributos de validación para la mayoría de las necesidades validación. Sin embargo, algunas reglas de validación son específicos de su negocio. Las reglas no estén técnicas de validación de datos comunes como garantizar un campo es necesario o que se ajusta a un intervalo de valores. Para estos escenarios, atributos de validación personalizados son una excelente solución. Es fácil crear sus propios atributos de validación personalizada en MVC. Solo se heredan de la `ValidationAttribute`e invalidar el `IsValid` método. El `IsValid` método acepta dos parámetros: el primero es un objeto denominado *valor* y el segundo es un `ValidationContext` objeto denominado *validationContext*. *Valor* hace referencia al valor real del campo que se está validando el validador personalizado.

En el ejemplo siguiente, una regla de negocios indica que los usuarios no pueden establecer el género como *clásico* a una película publicada después de 1960. El `[ClassicMovie]` atributo comprueba en primer lugar el género y, si es un estándar, a continuación, comprueba la fecha de lanzamiento que es posterior a 1960. Si se libera después de 1960, se produce un error de validación. El atributo acepta un parámetro de número entero que representa el año que puede usar para validar los datos. Puede capturar el valor del parámetro en el constructor del atributo, como se muestra aquí:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

El `movie` variable anteriormente representa un `Movie` objeto que contiene los datos desde el envío del formulario para validar. En este caso, el código de validación comprueba la fecha y el género en el `IsValid` método de la `ClassicMovieAttribute` clase según las reglas. Después de validar correctamente `IsValid` devuelve un `ValidationResult.Success` código, y cuando se produce un error en la validación, un `ValidationResult` con un mensaje de error. Cuando un usuario modifica el `Genre` campo y envía el formulario, el `IsValid` método de la `ClassicMovieAttribute` comprobará si la película es un clásico. Al igual que cualquier atributo integrado, aplicar el `ClassicMovieAttribute` a una propiedad como `ReleaseDate` para garantizar que se produce la validación, tal como se muestra en el ejemplo de código anterior. Puesto que el ejemplo solo funciona con `Movie` tipos, una mejor opción es usar `IValidatableObject` tal y como se muestra en el párrafo siguiente.

Como alternativa, podría colocarse este mismo código en el modelo mediante la implementación de la `Validate` método en el `IValidatableObject` interfaz. Mientras que los atributos de validación personalizada funcionan bien para validar las propiedades individuales, implementar `IValidatableObject` puede usarse para implementar la validación de nivel de clase como se ve aquí.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Validación del lado cliente

Validación del lado cliente es una gran ventaja para los usuarios. Ahorra tiempo en caso contrario, se gastan esperando de ida y vuelta al servidor. En términos comerciales, incluso unos fracciones de segundos multiplicados cientos de veces al día hasta agrega una gran cantidad de tiempo y gastos, frustración. La validación inmediata y sencilla permite a los usuarios trabajar de forma más eficiente y generar una mejor calidad de entrada y salida.

Debe tener una vista con las referencias de script de JavaScript adecuadas en su lugar para la validación del lado cliente para que funcione como ve aquí.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

El [jQuery validación discreto](https://github.com/aspnet/jquery-validation-unobtrusive) secuencia de comandos es una biblioteca de Microsoft front-end personalizada que se basa en la conocida [jQuery validar](https://jqueryvalidation.org/) complemento. Sin jQuery validación discreto, deberá codificar la misma lógica de validación en dos lugares: una vez en los atributos de validación del lado servidor en las propiedades del modelo y, a continuación, nuevo en los scripts del lado cliente (los ejemplos de jQuery validar [ `validate()` ](https://jqueryvalidation.org/validate/) método muestra su complejidad Esto podría ser). En su lugar, de MVC [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y [aplicaciones auxiliares HTML](xref:mvc/views/overview) pueden utilizar los atributos de validación y escriba los metadatos de propiedades del modelo para representar HTML 5 [atributos de datos](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) en los elementos de formulario que necesitan validación. MVC se genera el `data-` atributos para los atributos integrados y personalizados. jQuery validación discreto, a continuación, analiza thes `data-` atributos y pasa la lógica para jQuery validar, "Copiar" eficazmente la lógica de validación del lado servidor al cliente. Puede mostrar errores de validación en el cliente mediante las aplicaciones auxiliares de etiqueta relevantes como se muestra aquí:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Las aplicaciones auxiliares de etiqueta anteriores representan el código HTML siguiente. Tenga en cuenta que la `data-` atributos en el código HTML de salida se corresponden con los atributos de validación para el `ReleaseDate` propiedad. El `data-val-required` atributo siguiente contiene un mensaje de error que se mostrará si el usuario no rellene el campo de fecha de lanzamiento. jQuery validación discreto pasa este valor a la validación de jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) método, que se muestra a continuación, ese mensaje en el que lo acompaña  **\<abarcan >** elemento.

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

Validación del lado cliente impide el envío hasta el formulario es válido. El botón Enviar ejecuta JavaScript que envía el formulario o muestra mensajes de error.

MVC determina los valores de atributo de tipo según el tipo de datos de .NET de una propiedad, posiblemente se invalide utilizando `[DataType]` atributos. La base de `[DataType]` atributo no realiza ninguna validación de servidor real. Exploradores elegir sus propios mensajes de error y mostrar los errores pero que deseen, sin embargo el paquete discreto de validación de jQuery puede invalidar los mensajes y mostrarlos de forma coherente con otros usuarios. Esto sucede más evidente cuando los usuarios aplicar `[DataType]` subclases como `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Agregar validación a formularios dinámicos

Debido a que jQuery validación discreto pasa lógica de validación y los parámetros para jQuery validar cuando se carga la página por primera vez, forms generados de forma dinámica no presentará automáticamente validación. En su lugar, debe indicar jQuery discreto validación analizar de forma dinámica inmediatamente después de crearla. Por ejemplo, el código siguiente muestra cómo es posible configurar la validación del lado cliente en un formulario que se agregan mediante AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

El `$.validator.unobtrusive.parse()` método acepta un selector de jQuery para único argumento. Este método indica jQuery validación discreto para analizar el `data-` atributos de formas dentro de ese selector. Los valores de estos atributos, a continuación, se pasan al complemento de validación jQuery para que el formulario muestra las reglas de validación del lado cliente que desee.

### <a name="add-validation-to-dynamic-controls"></a>Agregar validación a los controles dinámicos

También puede actualizar las reglas de validación en un formulario cuando controles individuales, como `<input/>`s y `<select/>`s, se generan dinámicamente. No se puede pasar selectores de estos elementos para el `parse()` directo del método porque el formulario adyacente ya se ha analizado y no se actualizarán. En su lugar, primero quite los datos de validación existentes y análisis todo el formulario, tal y como se muestra a continuación:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Puede crear lógica del lado cliente para el atributo personalizado, y [validación discreto](http://jqueryvalidation.org/documentation/) ejecutará en el cliente para que automáticamente como parte de la validación. El primer paso consiste en controlar qué atributos de datos se agregan mediante la implementación de la `IClientModelValidator` interfaz tal y como se muestra aquí:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Atributos que implementan esta interfaz pueden agregar atributos HTML a los campos generados. Examinar la salida de la `ReleaseDate` elemento revela HTML que es similar al ejemplo anterior, excepto que ahora hay un `data-val-classicmovie` atributo que se definió en el `AddValidation` método `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Validación discreto utiliza los datos en el `data-` atributos para mostrar mensajes de error. Sin embargo, jQuery no sabe acerca de las reglas o los mensajes hasta que se agregan a los de jQuery `validator` objeto. Esto se muestra en el ejemplo siguiente que agrega un método denominado `classicmovie` que contienen código de validación de cliente personalizada para jQuery `validator` objeto.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery tiene ahora la información que se va a ejecutar la validación personalizada de JavaScript, así como el mensaje de error que se mostrará si ese código de validación devuelve false.

## <a name="remote-validation"></a>Validación remota

Validación remota es una característica excelente que se usará cuando se necesita validar los datos en el cliente con datos en el servidor. Por ejemplo, la aplicación necesite comprobar si un nombre de usuario o de correo electrónico ya está en uso, y debe consultar una gran cantidad de datos que se va a hacerlo. Descargar grandes conjuntos de datos para validar uno o algunos campos consume demasiados recursos. También puede exponer información confidencial. Una alternativa consiste en realizar una solicitud de ida y vuelta para validar un campo.

Puede implementar la validación remota en un proceso de dos pasos. En primer lugar, debe anotar el modelo con el `[Remote]` atributo. El `[Remote]` atributo acepta varias sobrecargas que puede usar para dirigir el JavaScript del lado cliente para el código adecuado para llamar a. En el ejemplo siguiente señala a la `VerifyEmail` método de acción de la `Users` controlador.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

El segundo paso es colocar el código de validación en el método de acción correspondiente, tal como se define en el `[Remote]` atributo. Según la validación de jQuery [ `remote()` ](https://jqueryvalidation.org/remote-method/) documentación del método:

> La respuesta serverside debe ser una cadena JSON que debe ser `"true"` de elementos válidos y puede ser `"false"`, `undefined`, o `null` para elementos no válidos, usando el mensaje de error predeterminado. Si la respuesta serverside es una cadena, p. ej. `"That name is already taken, try peter123 instead"`, esta cadena se mostrará como un mensaje de error personalizado en lugar del predeterminado.

La definición de la `VerifyEmail()` método sigue estas reglas, tal y como se muestra a continuación. Devuelve un error de validación del mensaje si no se realiza el correo electrónico, o `true` si el correo electrónico es gratuito y ajusta el resultado en un `JsonResult` objeto. El lado del cliente, a continuación, puede usar el valor devuelto para continuar o mostrar el error si es necesario.

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Ahora cuando los usuarios escriben un correo electrónico, JavaScript en la vista hace una llamada remota para ver si se ha hecho que correo electrónico y, si es así, muestra el mensaje de error. En caso contrario, el usuario puede enviar el formulario como de costumbre.

El `AdditionalFields` propiedad de la `[Remote]` atributo es útil para validar combinaciones de campos con los datos en el servidor. Por ejemplo, si la `User` modelo anterior tenía dos propiedades adicionales que se llama `FirstName` y `LastName`, debe comprobar que ningún usuario existente ya tiene ese par de nombres. Definir las nuevas propiedades tal como se muestra en el código siguiente:

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`podría se han establecido explícitamente a las cadenas de `"FirstName"` y `"LastName"`, pero con la [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) simplifica el operador similar a la refactorización más adelante. El método de acción para realizar la validación, a continuación, debe aceptar los dos argumentos, uno para el valor de `FirstName` y otro para el valor de `LastName`.


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

Ahora cuando los usuarios escribir un nombre y apellido, JavaScript:

* Realiza una llamada remota para ver si se ha hecho que los dos nombres.
* Si ha tomado el par, se muestra un mensaje de error. 
* Si no se realizó, el usuario puede enviar el formulario.

Si necesita validar dos o más campos adicionales con el `[Remote]` atributo, que les proporcionó como una lista delimitada por comas. Por ejemplo, para agregar una `MiddleName` propiedad en el modelo, establezca el `[Remote]` atributo tal como se muestra en el código siguiente:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, al igual que todos los argumentos de atributo, debe ser una expresión constante. Por lo tanto, no debe utilizar un [interpolan cadena](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) o llamar a [ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) para inicializar `AdditionalFields`. Para cada campo adicional que agregue a la `[Remote]` atributo, debe agregar otro argumento al método de acción de controlador correspondiente.
