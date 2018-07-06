---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para entregar actualizaciones dinámicas | Microsoft Docs
author: microsoft
description: Paso 10 implementa compatibilidad con los usuarios conectados a RSVP su interés por asistir a una cena, mediante un enfoque basado en Ajax integrado en los detalles de la cena...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825206"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Usar AJAX para entregar actualizaciones dinámicas
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 10 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 10 implementa compatibilidad con los usuarios conectados a RSVP su interés por asistir a una cena, mediante un enfoque basado en Ajax integrado en la página de detalles de la cena.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner paso 10: Acepta habilitación de confirmaciones de asistencia de AJAX

Implementemos ahora soporte técnico para los usuarios conectados a RSVP su interés por asistir a una cena. Esto Habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Que indica si el usuario está confirmado

Los usuarios pueden visitar el */dinners/detalles / [Id. de*] dirección URL para ver detalles sobre una cena en particular:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

El Details() se implementa el método de acción de este modo:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nuestro primer paso para implementar la compatibilidad RSVP será agregar un método de aplicación auxiliar "IsUserRegistered(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente). Este método auxiliar devuelve true o false dependiendo de si el usuario está confirmado actualmente para la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

El código siguiente, a continuación, podemos agregar a nuestra plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Y ahora cuando un usuario visita una cena están registrados para verá este mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Y cuando se visite una cena no están registrados para que verá el siguiente mensaje:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementación del método de acción de registro

Ahora agreguemos la funcionalidad necesaria para permitir que los usuarios a RSVP una cena desde la página de detalles.

Para implementar esto, vamos a crear una nueva clase "RSVPController" con el botón secundario en el directorio \Controllers y eligiendo agregar -&gt;comando de menú del controlador.

Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto apropiado de cena, comprueba si el usuario ha iniciado sesión actualmente está en la lista de usuarios que se han registrado para él y si no se agrega un objeto de confirmación de asistencia para ellos:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Tenga en cuenta sobre cómo nos estamos devolver una cadena sencilla como la salida del método de acción. Nos podríamos tener insertados este mensaje en una plantilla de vista: pero, ya que es tan pequeño, utilizaremos el método auxiliar Content() en la clase base del controlador y devuelven un mensaje de cadena del tipo anteriormente.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Una llamada al método de acción RSVPForEvent con AJAX

Vamos a usar AJAX para invocar el método de acción Register de la vista de detalles. Esta implementación es bastante fácil. En primer lugar, vamos a agregar dos referencias de biblioteca de secuencias de comandos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La primera biblioteca hace referencia a la biblioteca de script de cliente AJAX de ASP.NET core. Este archivo es aproximadamente 24 KB de tamaño (comprimido) y contiene la funcionalidad de AJAX del lado cliente. La segunda biblioteca contiene funciones de utilidad que se integran con integrados auxiliar los métodos de AJAX de ASP.NET MVC (lo que vamos a usar en breve).

Se puede, a continuación, se agregó anteriormente para que en lugar de un mensaje "No están registrados para este evento" outputing, en su lugar representamos un vínculo que cuando inserta el código de plantilla de vista de actualización realiza una llamada de AJAX que invoca el método de acción RSVPForEvent en nuestro controlador de RSVP y RSVPs el usuario:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

El método auxiliar de Ajax.ActionLink() usado anteriormente está integrada en ASP.NET MVC y es similar al método auxiliar Html.ActionLink(), salvo que en lugar de realizar una exploración estándar realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo. Anteriormente estamos llamando al método de acción de "Register" en el controlador "RSVP" y pasándole el DinnerID como el parámetro "id". El último parámetro AjaxOptions que estamos pasando indica que deseamos tomar el contenido devuelto por el método de acción y actualizar el código HTML &lt;div&gt; elemento en la página cuyo identificador es "rsvpmsg".

Y ahora, cuando un usuario explora una cena no están registrados para aún verán un vínculo a RSVP para él:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Si hace clic en el vínculo "RSVP para este evento" hará una llamada AJAX al método de acción de registro en el controlador de RSVP, y cuando se complete, verán un mensaje actualizado como el siguiente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

El ancho de banda y el tráfico que tienen lugar al realizar esta llamada AJAX está realmente ligera. Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una solicitud de red pequeña de HTTP POST a la */Dinners/Register/1* dirección URL que se parece a continuación en la conexión:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Y la respuesta de nuestro método de acción Register es simplemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.

### <a name="adding-a-jquery-animation"></a>Adición de una animación de jQuery

La funcionalidad de AJAX que implementamos funciona bien y rápida. A veces puede ocurrir tan rápido, sin embargo, que un usuario no es posible que observe que el vínculo RSVP ha sido reemplazado por texto nuevo. Para hacer que el resultado sea un poco más obvio podemos agregar una animación sencilla para llamar la atención sobre el mensaje de actualización.

El valor predeterminado de la plantilla de proyecto de ASP.NET MVC incluye jQuery: una biblioteca de JavaScript de código abierto de excelente (y muy popular) que también es compatible con Microsoft. jQuery proporciona una serie de características, como una biblioteca de efectos y selección de DOM de HTML agradable.

Para usar jQuery vamos a agregar una referencia de script a ella. Puesto que vamos a usar jQuery en una variedad de lugares dentro de nuestro sitio, vamos a agregar la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas pueden usarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Sugerencia: asegúrese de que ha instalado la revisión de intellisense de JavaScript de VS 2008 SP1 que permite mayor compatibilidad de intellisense para archivos de JavaScript (como jQuery). Puede descargarlo desde: http://tinyurl.com/vs2008javascripthotfix*

El código escrito mediante JQuery a menudo usa un "$ ()" global método JavaScript que recupera uno o varios elementos HTML mediante un selector de CSS. Por ejemplo, <em>$("#rsvpmsg")</em> selecciona cualquier elemento HTML con el Id. de rsvpmsg, mientras que <em>$(".something")</em> seleccionaría todos los elementos con el "algo" CSS nombre de clase. También puede escribir consultas más avanzadas, como "devolver todos los botones de radio checked" mediante una consulta de selector como: <em>$("entrada [@type= radio] [@checked]")</em>.

Una vez que haya seleccionado los elementos, puede llamar a métodos en ellos para llevar a cabo acciones como ocultarlas: *"$(#rsvpmsg").hide();*

Este escenario de RSVP, definiremos una simple función de JavaScript denominada "AnimateRSVPMessage" que selecciona el "rsvpmsg" &lt;div&gt; y anima el tamaño de su contenido de texto. El código siguiente inicia el texto pequeño y, a continuación, las causas que aumente su a través de un período de 400 milisegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Nos podemos, a continuación, cable copia de esta función de JavaScript que se llamará después nuestra llamada AJAX finaliza correctamente pasando su nombre al método auxiliar Ajax.ActionLink() (a través de la AjaxOptions "OnSuccess" propiedad del evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Y ahora cuando se hace clic en el vínculo "RSVP para este evento" y nuestra llamada AJAX finaliza correctamente, el mensaje contenido enviado back se animar y aumentan de tamaño:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Además de proporcionar un evento "OnSuccess", el objeto AjaxOptions expone métodos OnBegin OnFailure y OnComplete eventos que puede controlar (junto con una variedad de otras propiedades y opciones útiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpieza - refactorizar una vista parcial de RSVP

Nuestra plantilla de vista de detalles está comenzando a obtener un poco largo, qué horas extra resultará un poco más difícil de entender. Para ayudar a mejorar la legibilidad del código, vamos a terminar mediante la creación de una vista parcial: RSVPStatus.ascx – que encapsular todo el código de la vista RSVP para nuestra página de detalles.

Podemos hacer esto con el botón secundario en la carpeta \Views\Dinners y, a continuación, eligiendo Add -&gt;ver el comando de menú. Él tendremos toman un objeto de la cena como el modelo de vista fuertemente tipada. Se puede, a continuación, copiar y pegar el contenido RSVP desde nuestra vista Details.aspx en él.

Una vez hecho eso, vamos a crear también otra vista parcial: EditAndDeleteLinks.ascx - que encapsula nuestra vínculo Editar y eliminar, ver código. También tendremos toman un objeto de la cena como el modelo de vista fuertemente tipada y copiar y pegar la lógica de edición y eliminación de nuestra vista Details.aspx en él.

Nuestros detalles de la plantilla puede de ver a continuación, simplemente incluyen dos llamadas al método Html.RenderPartial() en la parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Esto hace que el código más limpio para leer y mantener.

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos usar AJAX aún más lejos y agregar compatibilidad con la asignación interactivo a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)
