---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validar la entrada del usuario en ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: Este artículo describe cómo validar la información que se obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escribir válido información en formato HTML, constituye un sistema autónomo....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899183"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validar la entrada del usuario en sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo validar la información que se obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escribir válido información en formato HTML, constituye un sitio de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo comprobar que una entrada del usuario coincide con los criterios de validación que se definan.
> - Cómo determinar si han pasado todas las pruebas de validación.
> - Cómo mostrar errores de validación (y cómo darles formato).
> - Cómo validar los datos que no proceden directamente de los usuarios.
> 
> Estos son los conceptos presentados en el artículo de programación de ASP.NET:
> 
> - El `Validation` auxiliar.
> - El `Html.ValidationSummary` y `Html.ValidationMessage` métodos.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


Este artículo contiene las siguientes secciones:

- [Información general sobre validación de entrada de usuario](#Overview_of_User_Input_Validation)
- [Validar la entrada del usuario](#Validating_User_Input)
- [Agregar la validación del lado cliente](#Adding_Client-Side_Validation)
- [Aplicar formato a los errores de validación](#Formatting_Validation_Errors)
- [Validación de datos que no proceden directamente de los usuarios](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Información general sobre validación de entrada de usuario

Si pida a los usuarios especificar información en una página, por ejemplo, en un formulario, es importante asegurarse de que los valores especificados por el son válidos. Por ejemplo, no desea procesar un formulario que le falta información crítica.

Cuando los usuarios escriben valores en un formulario HTML, los valores especificados por el son cadenas. En muchos casos, los valores que necesarios son algunos otros tipos de datos, como enteros o fechas. Por lo tanto, también tiene que asegurarse de que los valores que especifican los usuarios se pueden convertir correctamente a los tipos de datos adecuado.

También podría tener ciertas restricciones en los valores. Incluso si los usuarios escriben correctamente un entero, por ejemplo, deberá asegurarse de que el valor se encuentra dentro de un intervalo determinado.

![Errores de validación que utilizan las clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** validar proporcionados por el usuario también es importante para la seguridad. Al restringir los valores que los usuarios pueden especificar en los formularios, se reduce la posibilidad de que un usuario puede escribir un valor que puede poner en peligro la seguridad del sitio.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validar la entrada del usuario

En ASP.NET Web Pages 2, puede usar el `Validator` auxiliares para probar proporcionados por el usuario. El enfoque básico consiste en hacer lo siguiente:

1. Determinar qué entrada elementos (campos) que desee validar.

    Normalmente validar los valores de `<input>` elementos en un formulario. Sin embargo, resulta una buena práctica para validar todas las entradas, incluso de entrada que procede de un elemento restringido como un `<select>` lista. Esto ayuda a asegurarse de que los usuarios no omitir los controles de una página y enviar un formulario.
2. En el código de la página, agregue comprobaciones de validación individual para cada elemento de entrada mediante el uso de métodos de la `Validation` auxiliar.

    Para comprobar si los campos obligatorios, utilice `Validation.RequireField(field, [error message])` (para un campo individual) o `Validation.RequireFields(field1, field2, ...))` (para obtener una lista de campos). Para otros tipos de validación, utilice `Validation.Add(field, ValidationType)`. Para `ValidationType`, puede utilizar estas opciones:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Cuando se envía la página, compruebe si ha pasado la validación mediante la comprobación de `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Si hay errores de validación, omitir el procesamiento normal de página. Por ejemplo, si el propósito de la página consiste en actualizar una base de datos, no hacerlo hasta que se han corregido todos los errores de validación.
4. Si hay errores de validación, mostrar mensajes de error en el marcado de la página mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o ambos.

En el ejemplo siguiente se muestra una página que muestra estos pasos.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Para ver cómo funciona la validación, ejecute esta página y deliberadamente cometer errores. Por ejemplo, este es aspecto de la página como si se olvida de escribir un nombre de curso, si escribe una, y si escribe una fecha no válida:

![Errores de validación en la página representada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Agregar la validación del lado cliente

De forma predeterminada, se validan proporcionados por el usuario después de que los usuarios envíen la página, es decir, la validación se realiza en el código de servidor. Una desventaja de este enfoque es que los usuarios no sepan que ha realizado un error hasta después de que envíe la página. Si un formulario es larga o compleja, informan de errores solo una vez enviada la página puede ser un problema al usuario.

Puede agregar compatibilidad para realizar la validación en el script de cliente. En ese caso, la validación se realiza conforme los usuarios trabajan en el explorador. Por ejemplo, imagine que especifica que un valor debe ser un entero. Si un usuario escribe un valor no entero, se notifica el error tan pronto como el usuario deja el campo de entrada. Los usuarios reciben una respuesta inmediata, que es conveniente para ellos. Validación basada en el cliente también puede reducir el número de veces que el usuario debe enviar el formulario para corregir los errores varios.

> [!NOTE]
> Incluso si utiliza validación del lado cliente, siempre también se realiza la validación en el código de servidor. Realizar la validación en el código de servidor es una medida de seguridad, en caso de que los usuarios omiten la validación basada en cliente.


1. Registrar las siguientes bibliotecas de JavaScript en la página:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Dos de las bibliotecas son puede cargar desde una red de entrega de contenido (CDN), por lo que no necesariamente deben tener en su equipo o servidor. Sin embargo, debe tener una copia local de *jquery.validate.unobtrusive.js*. Si no ya se encuentra trabajando con una plantilla de WebMatrix (como **Starter Site** ) que incluye la biblioteca, cree un sitio de páginas Web que se basa en **Starter Site**. A continuación, copie el *.js* archivo al sitio actual.
2. En el marcado, para cada elemento que se está validando, agregue una llamada a `Validation.For(field)`. Este método emite atributos de validación del lado cliente. (En lugar de emitir el código de JavaScript real, el método emite atributos como `data-val-...`. Estos atributos admiten la validación de cliente discretos que utiliza jQuery para realizar el trabajo.)

La página siguiente muestra cómo agregar características de validación de cliente al ejemplo mostrado anteriormente.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

No todas las comprobaciones de validación se ejecute en el cliente. En concreto, validación de tipo de datos (entero, fecha etc.) no se ejecutan en el cliente. Las siguientes comprobaciones funcionan en el cliente y el servidor:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

En este ejemplo, no funcionará la prueba para una fecha válida en el código de cliente. Sin embargo, la prueba se realizará en el código de servidor.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Aplicar formato a los errores de validación

Puede controlar cómo se muestran los errores de validación mediante la definición de clases CSS que tienen los nombres reservados siguientes:

- `field-validation-error`. Define la salida de la `Html.ValidationMessage` método cuando se está mostrando un error.
- `field-validation-valid`. Define la salida de la `Html.ValidationMessage` método cuando no se produce ningún error.
- `input-validation-error`. Define cómo `<input>` se representan los elementos cuando se produce un error. (Por ejemplo, puede utilizar esta clase para establecer el color de fondo de un &lt;entrada&gt; elemento a un color diferente si su valor no es válido.) Esta clase CSS que se usa solo durante la validación del cliente (en ASP.NET Web Pages 2).
- `input-validation-valid`. Define el aspecto de `<input>` elementos cuando no se produce ningún error.
- `validation-summary-errors`. Define la salida de la `Html.ValidationSummary` método muestra una lista de errores.
- `validation-summary-valid`. Define la salida de la `Html.ValidationSummary` método cuando no se produce ningún error.

El siguiente `<style>` bloque muestra las reglas de las condiciones de error.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Si incluye este bloque de estilo en las páginas de ejemplo de anteriormente en este artículo, la presentación de errores tendrá un aspecto similar de la ilustración siguiente:

![Errores de validación que utilizan las clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Si no está utilizando la validación del cliente en ASP.NET Web Pages 2, el código CSS clases para la `<input>` elementos (`input-validation-error` y `input-validation-valid` no tiene ningún efecto.


### <a name="static-and-dynamic-error-display"></a>Presentación de errores estáticos y dinámicos

Las reglas de CSS vienen en parejas, como `validation-summary-errors` y `validation-summary-valid`. Estos pares le permiten definir reglas para las dos condiciones: una condición de error y una condición (no de error) "normal". Es importante comprender que siempre se representa el marcado para la presentación de errores, incluso si no hay ningún error. Por ejemplo, si una página tiene un `Html.ValidationSummary` método en el marcado, el origen de la página contendrá el siguiente marcado incluso cuando se solicita la página por primera vez:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

En otras palabras, el `Html.ValidationSummary` método siempre representa un `<div>` elemento y una lista, incluso si la lista de errores está vacía. De forma similar, el `Html.ValidationMessage` método siempre representa un `<span>` elemento como un marcador de posición para un error de campo individual, incluso si no hay ningún error.

En algunas situaciones, mostrar un mensaje de error, puede hacer que la página reajuste y pueden producir elementos en la página para desplazarse por. Las reglas de CSS que terminan en `-valid` le permiten definir un diseño que puede ayudar a evitar este problema. Por ejemplo, puede definir `field-validation-error` y `field-validation-valid` a ambos tienen el mismo fijos tamaño. De este modo, el área de presentación para el campo es estático y no cambiará el flujo de la página si se muestra un mensaje de error.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validación de datos que no proceden directamente de los usuarios

A veces deberá validar la información que no procedan directamente de un formulario HTML. Un ejemplo típico es una página donde se pasa un valor en una cadena de consulta, como en el ejemplo siguiente:

`http://server/myapp/EditClassInformation?classid=1022`

En este caso, desea asegurarse de que el valor que se pasa a la página (aquí, 1022 para el valor de `classid`) es válido. No se puede usar directamente el `Validation` auxiliar para realizar esta validación. Sin embargo, puede usar otras características del sistema de validación, como la capacidad para mostrar mensajes de error de validación.

> [!NOTE] 
> 
> **Importante** validar siempre los valores que obtienen de *cualquier* origen, incluidos los valores de campo de formulario, los valores de cadena de consulta y valores de cookie. Es fácil para los usuarios cambiar estos valores (quizás para propósitos malintencionados). Por lo que debe comprobar estos valores con el fin de proteger la aplicación.


En el ejemplo siguiente se muestra cómo se puede validar un valor que se pasa en una cadena de consulta. El código comprueba que el valor no está vacío y que es un entero.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Tenga en cuenta que la prueba se realiza cuando la solicitud no es un envío de formulario (`if(!IsPost)`). Esta prueba pasaría la primera vez que se solicita la página, pero no cuando la solicitud es un envío de formulario.

Para mostrar este error, puede agregar el error a la lista de errores de validación mediante una llamada a `Validation.AddFormError("message")`. Si la página contiene una llamada a la `Html.ValidationSummary` método, el error se muestra allí, al igual que un error de validación de entrada del usuario.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Trabajar con formularios HTML en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202892)
