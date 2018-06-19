---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Use ViewData e implemente ViewModel clases | Documentos de Microsoft
author: microsoft
description: Paso 6 se muestra cómo habilita la compatibilidad de forma más completa que modificar escenarios y también se explican dos enfoques que pueden utilizarse para pasar datos de controladores a vistas:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879431"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Use ViewData e implemente ViewModel clases
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 6 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 6 se muestra cómo habilita la compatibilidad de forma más completa que modificar escenarios y también se explican dos enfoques que pueden utilizarse para pasar datos de controladores a vistas: ViewData y modelo de vista.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner paso 6: ViewData y ViewModel

Hemos cubierto un número de escenarios de envío de formulario y se explica cómo implementar crear, actualizar y eliminar el soporte técnico (CRUD). Ahora podrá tomamos nuestra implementación DinnersController aún más y habilitar la compatibilidad con escenarios de edición de forma más completa. Al hacer esto analizaremos dos enfoques que pueden utilizarse para pasar datos de controladores a vistas: ViewData y modelo de vista.

### <a name="passing-data-from-controllers-to-view-templates"></a>Pasar los datos de controladores en las plantillas de vista

Una de las características que definen el modelo de MVC es la "separación de aspectos" strict ayuda a aplicar entre los distintos componentes de una aplicación. Modelos, controladores y vistas de cada uno de ellos también definida roles y responsabilidades, y se comunican entre sí de maneras bien definidos. Esto ayuda a promover la capacidad de prueba y reutilización del código.

Cuando una clase de controlador decide representar una respuesta HTML a un cliente, es responsable de pasar explícitamente a la plantilla de vista de todos los datos necesitan para representar la respuesta. Plantillas de vista nunca deberían realizar cualquier lógica de aplicación o de recuperación de datos – y en su lugar, deben limitar propios para solo tiene código de representación que se basa en el modelo/datos que recibe el controlador.

Ahora los datos del modelo que se pasa por nuestro DinnersController clase a nuestras plantillas de vista es sencilla y directa: una lista de objetos de la cena en el caso de Index() y una sola cena el objeto en el caso de Details(), Edit(), Create() y Delete(). Tal y como se agregan más capacidades de interfaz de usuario para nuestra aplicación, que vamos a menudo es necesario pasar algo más que estos datos para procesar respuestas de HTML en nuestras plantillas de vista. Por ejemplo, tengamos que queremos cambiar el campo "País" dentro de nuestro editar y crear vistas ante un cuadro de texto HTML a dropdownlist. En lugar de codificar la lista desplegable de nombres de país en la plantilla de vista, tengamos que queremos generar en una lista de países admitidos que se rellenan de forma dinámica. Necesitamos una manera de pasar el objeto de cena *y* la lista de países de nuestro controlador a nuestras plantillas de vista.

Echemos un vistazo a dos maneras que podemos lograr esto.

### <a name="using-the-viewdata-dictionary"></a>Mediante el diccionario ViewData

La clase base del controlador expone una propiedad de diccionario "ViewData" que puede utilizarse para pasar los elementos de datos adicionales de controladores a vistas.

Por ejemplo, para admitir el escenario donde queremos cambiar el cuadro de texto "País" dentro de la vista de edición desde que se va a un cuadro de texto HTML en un dropdownlist, podemos actualizar el método de acción Edit() para pasar (además de un objeto de la cena) un objeto SelectList que puede usarse como m. odelo de dropdownlist países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

El constructor de la anterior SelectList acepta una lista de los condados para rellenar la colocación downlist con, así como el valor seleccionado actualmente.

A continuación, podemos actualizar nuestra plantilla de vista Edit.aspx para utilizar el método de aplicación auxiliar de Html.DropDownList() en lugar del método de aplicación auxiliar de Html.TextBox() que hemos usado anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

El método de aplicación auxiliar de Html.DropDownList() anterior toma dos parámetros. El primero es el nombre del elemento de formulario HTML para la salida. El segundo es el modelo de "SelectList" se pasan a través del diccionario ViewData. Estamos usando C# "como" (palabra clave) para convertir el tipo en el diccionario como un SelectList.

Y ahora cuando se ejecución la aplicación y el acceso del */Dinners/Edit/1* dirección URL dentro de nuestro navegador, veremos que se ha actualizado la interfaz de usuario de edición para mostrar una dropdownlist de países en lugar de un cuadro de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Dado que también se representan la plantilla de vista de edición desde el método HTTP POST Editar (en escenarios cuando se producen errores), queremos Asegúrese de que también se actualiza este método para agregar el SelectList a ViewData cuando la plantilla de vista se presenta en escenarios de error:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Y ahora nuestro escenario de edición DinnersController admite DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Utilizar un modelo de ViewModel

El diccionario ViewData tiene la ventaja de ser bastante rápida y fácil de implementar. Algunos desarrolladores no como con diccionarios basados en cadena, sin embargo, puesto que pueden provocar errores tipográficos errores que no se detectarán en tiempo de compilación. El diccionario ViewData sin tipo también requiere mediante el operador "como" o de conversión cuando se usa un lenguaje fuertemente tipado como C# en una plantilla de vista.

Un método alternativo que podríamos usar es uno conoce a menudo como el patrón "ViewModel". Si utiliza este modelo, crear clases fuertemente tipadas que están optimizados para cuatro escenarios de vista específicos, y que exponen propiedades de los valores o contenido dinámico necesita nuestras plantillas de vista. Las clases de controlador, a continuación, pueden rellenar y pasar estas clases con optimización para ver a nuestra plantilla de vista que se usará. Esto permite la seguridad de tipos, comprobar tiempo de compilación e intellisense del editor de plantillas de vista.

Por ejemplo, para habilitar escenarios de edición podemos crear un "DinnerFormViewModel" clase como a continuación de formularios de cena que expone dos propiedades fuertemente tipadas: un objeto de la cena y el modelo de SelectList necesarios para rellenar la dropdownlist países:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Podemos actualizar, a continuación, el método de acción de Edit() para crear el DinnerFormViewModel utilizando el objeto de la cena que recuperamos de nuestro repositorio y, a continuación, se pasa a la plantilla de vista:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Te enviaremos un mensaje, a continuación, nuestra plantilla de vista de modo que TI espera un "DinnerFormViewModel" en lugar de una "cena" objeto cambiando el atributo "inherits" en la parte superior de la página edit.aspx de actualización de este modo:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Después de hacer esto, se actualizará para reflejar el modelo de objetos del tipo DinnerFormViewModel que estamos pasando las características de intellisense de la propiedad "Modelo" dentro de la plantilla de vista:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

A continuación, podemos actualizar el código de vista para trabajar fuera de él. Observe a continuación cómo no cambiar los nombres de los elementos de entrada, vamos a crear (los elementos de formulario todavía se denominarán "Título", "País"), pero estamos actualizando los métodos auxiliares de HTML para recuperar los valores de uso de la clase DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

También podrá Actualizamos nuestro método post de edición para utilizar la clase DinnerFormViewModel cuando errores de representación:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Podemos también Actualizamos nuestro Create() los métodos de acción para volver a usar justo lo mismo *DinnerFormViewModel* clase para permitir a los países DropDownList dentro de las que también. A continuación se muestra la implementación de HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

A continuación se muestra la implementación del método HTTP POST crear:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Y ahora admiten nuestras pantallas editar y crear los desplegables para seleccionar el país.

### <a name="custom-shaped-viewmodel-classes"></a>Clases de ViewModel en forma de personalizado

En el escenario anterior, nuestra clase DinnerFormViewModel expone directamente el objeto de modelo de la cena como una propiedad, junto con una propiedad de modelo SelectList auxiliar. Este enfoque funciona perfectamente en escenarios donde la interfaz de usuario de HTML que desea crear dentro de la plantilla de vista corresponde relativamente estrechamente con nuestros objetos del modelo de dominio.

Para escenarios donde no es el caso, una opción que puede usar es crear una clase ViewModel en forma personalizada cuyo modelo de objetos está más optimizado para su uso por la vista – y que podría ser completamente diferente del objeto de modelo de dominio subyacente. Por ejemplo, pudieron exponer los nombres de propiedad diferente o agregadas propiedades recopiladas por múltiples objetos de modelo.

Pueden ser en forma de personalizar las clases de ViewModel usa tanto para pasar datos de controladores a las vistas para representar, así como para ayudar a administrar los datos de formulario que se devuelve al método de acción del controlador. En este escenario más adelante, puede que tenga el método de acción Actualizar un objeto de modelo de vista con los datos expuestos por el formulario y, a continuación, usar la instancia ViewModel para asignar o recuperar un objeto de modelo de dominio real.

Clases de ViewModel en forma de personalizado pueden proporcionar una gran cantidad de flexibilidad y son algo para investigar cualquier momento que se encuentra el código de representación dentro de las plantillas de vista o el código de envío de formulario dentro de los métodos de acción empezando a ser demasiado complicado. Por lo general, suele ser un inicio de sesión que los modelos de dominio correctamente no corresponden a la interfaz de usuario que se va a generar, y que puede ayudar una clase ViewModel intermedia de forma personalizada.

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos usar parciales y las páginas maestras para reutilizar y compartir la interfaz de usuario a través de nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Siguiente](re-use-ui-using-master-pages-and-partials.md)
