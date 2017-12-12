---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Usar AJAX para entregar las actualizaciones dinámicas | Documentos de Microsoft"
author: microsoft
description: "Paso 10 implementa la compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena, mediante un enfoque basado en Ajax integrado dentro de los detalles de la cena..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Usar AJAX para entregar las actualizaciones dinámicas
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 10 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 10 implementa la compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena, mediante un enfoque basado en Ajax integrado dentro de la página de detalles de la cena.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner paso 10: Acepta de AJAX habilitar confirmaciones de asistencia

Implementemos ahora compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena. Se deberá habilitar esta opción mediante un enfoque basado en AJAX integrado dentro de la página de detalles de la cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Que indica si el usuario es para RSVP

Los usuarios pueden visitar el */Dinners/detalles / [Id. de*] dirección URL para ver detalles sobre una cena determinada:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

El Details() se implementa el método de acción de este modo:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nuestro primer paso para implementar la compatibilidad RSVP será para agregar un método de aplicación auxiliar de "IsUserRegistered(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que se generó anteriormente). Este método auxiliar devuelve true o false, según si el usuario actualmente para RSVP de la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

A continuación, podemos agregar el código siguiente a la plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Y ahora cuando un usuario visita una cena están registrados para verá este mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Y cuando visite una cena no están registrados para verán el mensaje siguiente:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementar el método de acción de registro

Ahora agreguemos la funcionalidad necesaria para permitir a los usuarios a RSVP para una cena desde la página de detalles.

Para hacerlo, vamos a crear una nueva clase de "RSVPController", el botón secundario en el directorio \Controllers y elija Agregar -&gt;comando de menú del controlador.

Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto de cena adecuado, comprueba si el usuario ha iniciado la sesión está actualmente en la lista de usuarios que se hayan registrado para él y si no agrega un objeto RSVP para ellos:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Tenga en cuenta sobre cómo nos estamos devolver una cadena simple como el resultado del método de acción. Se pudo ha incrustado este mensaje dentro de una plantilla de vista: pero ya es tan pequeña, utilizaremos el método de aplicación auxiliar de Content() en la clase de base de controlador y devolver un mensaje de cadena, como por encima.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Una llamada al método de acción de RSVPForEvent con AJAX

Vamos a usar AJAX para invocar el método de acción de registro de la vista de detalles. Esta implementación es bastante fácil. Primero vamos a agregar dos referencias de biblioteca de secuencia de comandos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La primera biblioteca hace referencia a la biblioteca básica de script de cliente AJAX de ASP.NET. Este archivo es aproximadamente 24 KB de tamaño (comprimido) y contiene la funcionalidad de AJAX de cliente principal. La segunda biblioteca contiene funciones de utilidad que se integran con integrados AJAX auxiliar métodos de ASP.NET MVC (que vamos a usar en breve).

Se puede, a continuación, el código de plantilla de la vista se agregó anteriormente para que en lugar de outputing un mensaje "No se ha registrado para este evento", en su lugar se procesará un vínculo que cuando envíe la actualización realiza una llamada de AJAX que invoca el método de acción RSVPForEvent en nuestro controlador RSVP y RSVPs el usuario:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

El método de aplicación auxiliar de Ajax.ActionLink() usado anteriormente está integrada en ASP.NET MVC y es similar al método auxiliar Html.ActionLink() salvo que en lugar de realizar una exploración estándar realiza una llamada AJAX para el método de acción cuando se hace clic en el vínculo. Anteriormente estamos llamando al método de acción de "Register" en el controlador "RSVP" y pasarle el DinnerID como el parámetro "id". El último parámetro AjaxOptions pasamos indica que deseamos tomar el contenido devuelto por el método de acción y actualizar el código HTML &lt;div&gt; elemento de la página cuyo identificador es "rsvpmsg".

Y ahora cuando un usuario se desplaza a una cena que no están registrados para aún verá un vínculo a RSVP para él:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Si hacen clic en el vínculo "RSVP para este evento" realizará una llamada AJAX para el método de acción de registro en el controlador RSVP y, cuando se complete verán un mensaje actualizado como las siguientes:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

El tráfico implicados cuando se realiza esta llamada de AJAX y ancho de banda de red es realmente ligero. Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una solicitud de red de HTTP POST pequeña para la */Dinners/Register/1* dirección URL que parece que a continuación en la conexión:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Y la respuesta desde el método de acción de registro es simplemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.

### <a name="adding-a-jquery-animation"></a>Agregar una animación de jQuery

La funcionalidad de AJAX implementamos funciona bien y rápido. A veces puede ocurrir tan rápido, no obstante, que un usuario no puede observar que el vínculo RSVP se ha reemplazado por otro nuevo. Hacer que el resultado un poco más obvio que podemos agregar una animación sencilla para llamar la atención sobre el mensaje de actualización.

El valor predeterminado de plantilla de proyecto de ASP.NET MVC incluye jQuery: una biblioteca de JavaScript de código abierto excelente (y muy popular) que también es compatible con Microsoft. jQuery proporciona una serie de características, como una biblioteca "nice" de selección y efectos de DOM de HTML.

Para usar jQuery vamos a agregar una referencia de script a él. Dado que vamos a usar jQuery en diversos lugares dentro de nuestro sitio, vamos a agregar la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas pueden utilizarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Sugerencia: asegúrese de que ha instalado la revisión de intellisense de JavaScript para VS 2008 SP1 que permite una compatibilidad de intellisense para los archivos JavaScript (incluidos jQuery). Puede descargarlo desde: http://tinyurl.com/vs2008javascripthotfix*

El código escrito mediante JQuery a menudo utiliza un "$ ()" global método JavaScript que recupera uno o varios elementos HTML mediante un selector CSS. Por ejemplo, *$("#rsvpmsg")* selecciona cualquier elemento HTML con el Id. de rsvpmsg, mientras que *$(".something")* seleccionaría todos los elementos con el "elemento" CSS nombre de clase. También puede escribir consultas más avanzadas, como "se deben devolver todos los botones de radio comprobado" mediante una consulta de selector como: *$("entrada [@type= radio] [@checked]")*.

Una vez que haya seleccionado los elementos, puede llamar a métodos en ellas para actuar como ocultarlas: *$("#rsvpmsg").hide();*

En nuestro caso RSVP, se definirá una función de JavaScript simple denominada "AnimateRSVPMessage" que selecciona "rsvpmsg" &lt;div&gt; y anima el tamaño de su contenido de texto. El siguiente código inicia el texto pequeño y, a continuación, hace que aumente a través de un período de tiempo de 400 milisegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Le podemos, a continuación, el cable telefónico esta función de JavaScript que se llama después de la llamada de AJAX se complete correctamente pasando su nombre para el método de aplicación auxiliar de Ajax.ActionLink() (a través de la AjaxOptions "OnSuccess" propiedad del evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Ahora cuando se hace clic en el vínculo "RSVP para este evento" y la llamada de AJAX completa correctamente, el mensaje contenido enviado back se y animar aumentan de tamaño:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Además de proporcionar un evento de "OnSuccess", el objeto AjaxOptions expone métodos OnBegin, OnFailure y OnComplete eventos que puede controlar (junto con una variedad de otras propiedades y acciones útiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpieza - refactorizar una vista parcial RSVP

La plantilla de vista de detalles está empezando a un poco tiempo y las horas extraordinarias realizará un poco más difícil de comprender. Para ayudar a mejorar la legibilidad del código, vamos a terminar mediante la creación de una vista parcial – RSVPStatus.ascx – que encapsulan todo el código de la vista RSVP para nuestra página de detalles.

Podemos hacer esto con el botón secundario en la carpeta \Views\Dinners y, a continuación, elija Agregar -&gt;ver el comando de menú. Haremos lo que toman un objeto de la cena como su modelo de vista fuertemente tipada. Le podemos, a continuación, copiar y pegar el contenido RSVP de nuestra vista Details.aspx en él.

Una vez que hemos hecho, vamos a crear otra vista parcial: EditAndDeleteLinks.ascx - que encapsula el código de vista de vínculo Editar y eliminar. Se tendrá toman un objeto de la cena como su modelo de vista fuertemente tipada y copiar y pegar la lógica de editar y eliminar de la vista Details.aspx en él.

Nuestro detalles ver plantilla puede, a continuación, simplemente incluyen dos llamadas al método Html.RenderPartial() en la parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Esto hace que el código más limpia de leer y mantener.

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos usar AJAX aún más y agregar compatibilidad con la asignación interactivo a nuestra aplicación.

>[!div class="step-by-step"]
[Anterior](secure-applications-using-authentication-and-authorization.md)
[Siguiente](use-ajax-to-implement-mapping-scenarios.md)
