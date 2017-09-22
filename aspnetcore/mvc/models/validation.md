---
title: "Validación del modelo en MVC de ASP.NET Core"
author: rick-anderson
description: "Presenta la validación del modelo en MVC de ASP.NET Core."
keywords: "Validación de ASP.NET Core, MVC,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0874d3b677cee2859da9eb85b0573811abbed12a
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
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

[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]

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

## <a name="model-state"></a>Estado del modelo

Estado del modelo representa los errores de validación en valores de formulario HTML enviados.

MVC continuará validando campos hasta que alcanza el número máximo de errores (200 de forma predeterminada). Puede configurar este número insertando el código siguiente en el `ConfigureServices` método en el *Startup.cs* archivo:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Estado del modelo de control de errores

Validación del modelo se produce antes de cada acción de controlador que se invoca y es responsabilidad del método de acción para inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada. En muchos casos, la reacción adecuada es devolver algún tipo de respuesta de error, lo ideal sería que detalla el motivo por el motivo del error de validación del modelo.

Algunas aplicaciones se elegirá seguir una convención estándar para tratar los errores de validación del modelo, en cuyo caso un filtro puede ser un lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con los Estados del modelo válidos y no válidos.

## <a name="manual-validation"></a>Validación manual

Cuando se completen la validación y el enlace de modelos, puede que desee repetir partes de él. Por ejemplo, un usuario ha escrito texto en un campo que se esperaba un entero, o puede que necesite calcular el valor de propiedad del modelo.

Debe ejecutar manualmente la validación. Para ello, llame a la `TryValidateModel` método, como se muestra aquí:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validación personalizada

Funcionan los atributos de validación para la mayoría de las necesidades validación. Sin embargo, algunas reglas de validación son específicos para su negocio, como se convierten en no solo se requiere validación de datos genéricos como asegurarse de un campo o que se ajusta a un intervalo de valores. Para estos escenarios, atributos de validación personalizados son una excelente solución. Es fácil crear sus propios atributos de validación personalizada en MVC. Solo se heredan de la `ValidationAttribute`e invalidar el `IsValid` método. El `IsValid` método acepta dos parámetros: el primero es un objeto denominado *valor* y el segundo es un `ValidationContext` objeto denominado *validationContext*. *Valor* hace referencia al valor real del campo que se está validando el validador personalizado.

En el ejemplo siguiente, una regla de negocios indica que los usuarios no pueden establecer el género como *clásico* a una película publicada después de 1960. El `[ClassicMovie]` atributo comprueba en primer lugar el género y, si es un estándar, a continuación, comprueba la fecha de lanzamiento que es posterior a 1960. Si se libera después de 1960, se produce un error de validación. El atributo acepta un parámetro de número entero que representa el año que puede usar para validar los datos. Puede capturar el valor del parámetro en el constructor del atributo, como se muestra aquí:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

El `movie` variable anteriormente representa un `Movie` objeto que contiene los datos desde el envío del formulario para validar. En este caso, el código de validación comprueba la fecha y el género en el `IsValid` método de la `ClassicMovieAttribute` clase según las reglas. Después de validar correctamente `IsValid` devuelve un `ValidationResult.Success` código, y cuando se produce un error en la validación, un `ValidationResult` con un mensaje de error. Cuando un usuario modifica el `Genre` campo y envía el formulario, el `IsValid` método de la `ClassicMovieAttribute` comprobará si la película es un clásico. Al igual que cualquier atributo integrado, aplicar el `ClassicMovieAttribute` a una propiedad como `ReleaseDate` para garantizar que se produce la validación, tal como se muestra en el ejemplo de código anterior. Puesto que el ejemplo solo funciona con `Movie` tipos, una mejor opción es usar `IValidatableObject` tal y como se muestra en el párrafo siguiente.

Como alternativa, podría colocarse este mismo código en el modelo mediante la implementación de la `Validate` método en el `IValidatableObject` interfaz. Mientras que los atributos de validación personalizada funcionan bien para validar las propiedades individuales, implementar `IValidatableObject` puede usarse para implementar la validación de nivel de clase como se ve aquí.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a>Validación del lado cliente

Validación del lado cliente es una gran ventaja para los usuarios. Ahorra tiempo en caso contrario, se gastan esperando de ida y vuelta al servidor. En términos comerciales, incluso unos fracciones de segundos multiplicados cientos de veces al día hasta agrega una gran cantidad de tiempo y gastos, frustración. La validación inmediata y sencilla permite a los usuarios trabajar de forma más eficiente y generar una mejor calidad de entrada y salida.

Debe tener una vista con las referencias de script de JavaScript adecuadas en su lugar para la validación del lado cliente para que funcione como ve aquí.

[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

MVC usa los atributos de validación además de los metadatos de tipo de las propiedades del modelo para validar los datos y mostrar los mensajes de error con JavaScript. Cuando usas MVC para representar los elementos de formulario de un modelo con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) o [aplicaciones auxiliares HTML](xref:mvc/views/overview) agregará HTML 5 [atributos de datos](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) en los elementos de formulario que necesitan validación, como se muestra a continuación. MVC se genera el `data-` atributos para los atributos integrados y personalizados. Puede mostrar errores de validación en el cliente mediante las aplicaciones auxiliares de etiqueta relevantes como se muestra aquí:

[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Las aplicaciones auxiliares de etiqueta anteriores representan el código HTML siguiente. Tenga en cuenta que la `data-` atributos en el código HTML de salida se corresponden con los atributos de validación para el `ReleaseDate` propiedad. El `data-val-required` atributo siguiente contiene un mensaje de error para mostrar si el usuario no rellene el campo de fecha de lanzamiento, y ese mensaje se muestra en el que lo acompaña `<span>` elemento.

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

Puede implementar la validación remota en un proceso de dos pasos. En primer lugar, debe anotar el modelo con el `[Remote]` atributo. El `[Remote]` atributo acepta varias sobrecargas que puede usar para dirigir el JavaScript del lado cliente para el código adecuado para llamar a. En el ejemplo se apunta a la `VerifyEmail` método de acción de la `Users` controlador.

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

El segundo paso es colocar el código de validación en el método de acción correspondiente, tal como se define en el `[Remote]` atributo. Devuelve un `JsonResult` que el cliente puede utilizar para continuar o pause y muestre un error si es necesario.

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

Ahora cuando los usuarios escriben un correo electrónico, JavaScript en la vista realiza una llamada remota para ver si se ha hecho que correo electrónico y si es así, a continuación, muestra el mensaje de error. En caso contrario, el usuario puede enviar el formulario como de costumbre.
