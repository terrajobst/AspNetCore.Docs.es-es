---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usar ViewData e implementar clases ViewModel | Microsoft Docs
author: microsoft
description: Paso 6 se muestra cómo habilita la compatibilidad con la forma más completa edición de escenarios y también se describen dos enfoques que pueden utilizarse para pasar datos de los controladores de vistas:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827799"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Usar ViewData e implementar clases ViewModel
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 6 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 6 se muestra cómo habilita la compatibilidad con la forma más completa edición de escenarios y también se describen dos enfoques que pueden utilizarse para pasar datos de los controladores de vistas: ViewData y ViewModel.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>Paso 6 de NerdDinner: ViewData y ViewModel

Hemos tratado varios escenarios de envío de formulario y se explica cómo implementar crear, actualizar y eliminar el soporte técnico (CRUD). Ahora echaremos nuestra implementación DinnersController aún más y habilitar la compatibilidad con escenarios de edición de forma más completa. Al hacerlo se tratarán dos enfoques que pueden utilizarse para pasar datos de los controladores de vistas: ViewData y ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Pasar los datos de los controladores a las plantillas de vista

Una de las características que definen el patrón MVC es la "separación de preocupaciones" strict le ayuda a aplicar entre los distintos componentes de una aplicación. Models, Controllers y las vistas también cada uno han definido roles y responsabilidades, y se comunican entre sí de maneras bien definido. Esto ayuda a promover la capacidad de prueba y reutilización del código.

Cuando una clase de controlador decide representar una respuesta HTML a un cliente, es responsable de pasar explícitamente a la plantilla de vista de todos los datos necesitan para representar la respuesta. Plantillas de vista nunca deben realizar la lógica de aplicación o de recuperación de datos – y en su lugar, deben limitar a sí mismos para sólo tener código de representación que se basa en los datos o del modelo pasados por el controlador.

Ahora los datos del modelo que se pasan por nuestro DinnersController clase a nuestras plantillas de vista es simple y sencillo: una lista de objetos de la cena en el caso de Index() y una cena única del objeto en el caso de Details(), Edit(), Create() y Delete(). Cuando agreguemos más funcionalidades de la interfaz de usuario a nuestra aplicación, vamos a menudo a debe pasar algo más que estos datos para procesar respuestas HTML dentro de nuestras plantillas de vista. Por ejemplo, podríamos queremos cambiar el campo "País" dentro de nuestro editar y crear vistas desde la que se va a un cuadro de texto HTML en dropdownlist. En lugar de codificar de forma rígida la lista desplegable de nombres de país en la plantilla de vista, que podría desear generarlo de una lista de países admitidos que se rellenan dinámicamente. Necesitamos una manera de pasar el objeto cena *y* la lista de países admitidos desde nuestro controlador a nuestras plantillas de vista.

Echemos un vistazo a dos maneras que podemos lograrlo.

### <a name="using-the-viewdata-dictionary"></a>Mediante el diccionario ViewData

La clase base del controlador expone una propiedad de diccionario de "ViewData" que se puede usar para pasar los elementos de datos adicionales de controladores a las vistas.

Por ejemplo, para admitir el escenario donde queremos cambia el cuadro de texto "País" dentro de nuestra vista de edición de ser un cuadro de texto HTML en dropdownlist, podemos actualizar nuestro método de acción Edit() para pasar (además de un objeto de la cena) un objeto SelectList que puede usarse como la letra m odelo de dropdownlist países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

El constructor de la anterior SelectList acepta una lista de los condados para rellenar el downlist colocar con, así como el valor seleccionado actualmente.

A continuación, podemos actualizar la plantilla de vista Edit.aspx para usar el método auxiliar Html.DropDownList() en lugar del método de aplicación auxiliar Html.TextBox() que hemos usado anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

El método auxiliar Html.DropDownList() anterior toma dos parámetros. El primero es el nombre del elemento de formulario HTML para la salida. El segundo es el modelo de "SelectList" se pasa mediante el diccionario ViewData. Estamos usando C# "como" palabra clave para convertir el tipo dentro del diccionario como un SelectList.

Y ahora cuando ejecutamos nuestra aplicación y el acceso a la */Dinners/Edit/1* URL dentro de nuestro explorador veremos que nuestra interfaz de usuario de edición se ha actualizado para mostrar en dropdownlist de países en lugar de un cuadro de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Dado que también se representan la plantilla de vista de edición del método HTTP POST Editar (en escenarios cuando se producen errores), queremos asegurarse de que también se actualiza este método para agregar el SelectList a ViewData cuando la plantilla de vista se presenta en escenarios de error:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Y ahora nuestro escenario de edición DinnersController admite DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Con un patrón de ViewModel

El enfoque de diccionario ViewData tiene la ventaja de ser bastante rápido y fácil de implementar. Algunos desarrolladores no le gusta usar diccionarios basados en cadena, sin embargo, puesto que los errores tipográficos pueden conducir a errores que no se detecte en tiempo de compilación. El diccionario ViewData sin tipo también requiere utilizando el operador "as" o cuando se usa un lenguaje fuertemente tipado como C# en una plantilla de vista de conversión.

Un enfoque alternativo que podíamos usar es uno conoce a menudo como el patrón "ViewModel". Al utilizar este patrón se creación clases fuertemente tipadas que están optimizados para escenarios de nuestra vista específica y que exponen propiedades para el valores o contenido dinámico necesita nuestras plantillas de vista. Nuestras clases de controlador, a continuación, pueden rellenar y pasar estas clases optimizadas para la vista a nuestra plantilla de vista para usar. Esto permite la seguridad de tipos, comprobación en tiempo de compilación e intellisense del editor dentro de las plantillas de vista.

Por ejemplo, para habilitar escenarios de edición que podemos crear un "DinnerFormViewModel" clase como a continuación de formulario de dinner que expone dos propiedades fuertemente tipadas: un objeto de la cena y el modelo SelectList necesarios para rellenar la dropdownlist países:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Podemos actualizar, a continuación, el método de acción Edit() para crear el DinnerFormViewModel utilizando el objeto de la cena que recuperamos de nuestro repositorio y luego pasarla a nuestra plantilla de vista:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Vamos a nuestra plantilla de vista, por lo que TI espera un "DinnerFormViewModel" en lugar de una "cena" objeto cambiando el atributo "inherits" en la parte superior de la página edit.aspx de actualización, como lo:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Una vez que lo hacemos, intellisense de la propiedad "Modelo" dentro de nuestra plantilla de vista se actualizará para reflejar el modelo de objetos del tipo DinnerFormViewModel que nos estamos pasando:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

A continuación, podemos actualizar nuestro código de vista para trabajar fuera de él. Tenga en cuenta a continuación cómo nos estamos cambiando los nombres de los elementos de entrada no vamos a crear (los elementos del formulario todavía se denominarán "Title", "País"), pero estamos actualizando los métodos de aplicación auxiliar HTML para recuperar los valores de uso de la clase DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

También se actualizará nuestro método post de edición para usar la clase DinnerFormViewModel cuando errores de representación:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Podemos también actualizar nuestros métodos de acción Create() volver a utilizar exactamente igual *DinnerFormViewModel* clase para permitir a los países DropDownList dentro de esas también. Esta es la implementación HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

A continuación es la implementación del método HTTP POST a crear:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Y ahora admiten nuestras pantallas editar y crear los desplegables para seleccionar el país.

### <a name="custom-shaped-viewmodel-classes"></a>Clases de ViewModel en forma de personalizado

En el escenario anterior, nuestra clase DinnerFormViewModel expone directamente el objeto de modelo cena como una propiedad, junto con una propiedad de modelo SelectList auxiliar. Este enfoque funciona bien para escenarios donde la interfaz de usuario HTML que deseamos crear dentro de nuestra plantilla de vista corresponde relativamente estrechamente con nuestros objetos de modelo de dominio.

Para escenarios donde esto no es el caso, una opción que puede usar es crear una clase ViewModel personalizada en forma cuyo modelo de objetos de más está optimizado para su uso por la vista, y que podrían parecer completamente diferentes desde el objeto de modelo de dominio subyacente. Por ejemplo, podrían exponer los nombres de propiedad diferentes o las propiedades agregadas procedentes de varios objetos de modelo.

En forma de personalizar las clases de ViewModel pueden usar tanto para pasar datos de los controladores a las vistas para representar, así como para ayudar a controlar los datos de formulario devuelve al método de acción del controlador. En este escenario más adelante, tendrá que el método de acción Actualizar un objeto ViewModel con los datos publicados por el formulario y, a continuación, usar la instancia de ViewModel para asignar o recuperar un objeto de modelo de dominio real.

En forma de personalizar las clases de ViewModel pueden proporcionar un gran flexibilidad y son algo para investigar cada vez que se encuentra el código de representación dentro de las plantillas de vista o el código del formulario de registro dentro de los métodos de acción comienza a ser demasiado complicado. Esto suele ser un inicio de sesión que los modelos de dominio limpiamente no corresponden a la interfaz de usuario que se está generando y que puede ayudar una clase ViewModel intermedia de forma personalizada.

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos usar parciales y páginas maestras para volver a usar y compartir la interfaz de usuario en nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Siguiente](re-use-ui-using-master-pages-and-partials.md)
