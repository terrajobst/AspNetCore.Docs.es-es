---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevención de ataques de inyección de código de JavaScript (VB) | Documentos de Microsoft
author: StephenWalther
description: Evitar ataques de inyección de código de JavaScript y los ataques de Scripting entre sitios de elementos no utilizados para usted. En este tutorial, Stephen Walther explica cómo puede fácilmente de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871238"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Prevención de ataques de inyección de código de JavaScript (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descarga de PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Evitar ataques de inyección de código de JavaScript y los ataques de Scripting entre sitios de elementos no utilizados para usted. En este tutorial, Stephen Walther explica cómo se pueden anular fácilmente estos tipos de ataques por el contenido de la codificación HTML.


El objetivo de este tutorial es explicar cómo se pueden impedir los ataques de inyección de código de JavaScript en las aplicaciones de ASP.NET MVC. Este tutorial describe dos enfoques para defiende su sitio Web contra un ataque de inyección de código de JavaScript. Obtenga información acerca de cómo evitar los ataques de inyección de código de JavaScript mediante la codificación de los datos que muestran. También aprenderá a evitar los ataques de inyección de código de JavaScript mediante la codificación de los datos que aceptan.

## <a name="what-is-a-javascript-injection-attack"></a>¿Qué es un ataque de inyección de código de JavaScript?

Cada vez que aceptan la entrada de usuario y volver a mostrar la entrada de usuario, abra el sitio Web a los ataques de inyección de código de JavaScript. Vamos a examinar una aplicación concreta que está abierta a los ataques de inyección de código de JavaScript.

Imagine que ha creado un sitio Web de comentarios de cliente (consulte la figura 1). Los clientes pueden visitar el sitio Web y escribir comentarios sobre su experiencia con sus productos. Cuando un cliente envíe sus comentarios, los comentarios se vuelve a mostrar en la página de comentarios.


[![Sitio Web de comentarios del cliente](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: sitio Web de comentarios del cliente ([haga clic aquí para ver la imagen a tamaño completo](preventing-javascript-injection-attacks-vb/_static/image3.png))


El sitio Web de comentarios de cliente utiliza la `controller` en la lista 1. Esto `controller` contiene dos acciones denominadas `Index()` y `Create()`.

**Lista 1: `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

El `Index()` método muestra el `Index` vista. Este método pasa todos los comentarios de los clientes anteriores a la `Index` vista mediante la recuperación de los comentarios de la base de datos (mediante una consulta de LINQ to SQL).

El `Create()` método crea un nuevo elemento de comentarios y lo agrega a la base de datos. El mensaje que entra en el cliente en el formulario se pasa a la `Create()` método en el parámetro de mensaje. Se crea un elemento de comentarios y se asigna al mensaje para el elemento de comentarios `Message` propiedad. El elemento de comentarios se envía a la base de datos con el `DataContext.SubmitChanges()` llamada al método. Por último, el visitante se redirige a la `Index` vista donde se muestran todos los comentarios.

El `Index` vista está incluida en el listado 2.

**La lista 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

El `Index` vista tiene dos secciones. La sección superior contiene el formulario de comentarios de clientes reales. En la sección inferior contiene For... Cada bucle que recorre en bucle todos los elementos de comentarios del cliente anterior y muestra las propiedades de mensaje y EntryDate para cada elemento de comentarios.

El sitio Web de comentarios de cliente es un sitio Web sencillo. Desafortunadamente, el sitio Web está abierto a los ataques de inyección de código de JavaScript.

Imagine que escribir el texto siguiente en el formulario de comentarios del cliente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Este texto representa una secuencia de comandos de JavaScript que muestra un cuadro de mensaje de alerta. Después de que alguien envía esta secuencia de comandos en los comentarios de formulario, el mensaje <em>Boo!</em> aparecerá cada vez que alguien visita el sitio Web de comentarios de cliente en el futuro (consulte la figura 2).


[![Inyección de código de JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: inyección de código de JavaScript ([haga clic aquí para ver la imagen a tamaño completo](preventing-javascript-injection-attacks-vb/_static/image6.png))


Ahora, la respuesta inicial a los ataques de inyección de código de JavaScript podría ser apathy. Puede que piense que los ataques de inyección de código de JavaScript son simplemente un tipo de *asalto* ataque. Puede que consideres que nadie puede hacer todo lo que realmente mal al confirmar un ataque de inyección de código de JavaScript.

Por desgracia, un pirata informático puede realizar algunas realmente, realmente mal cosas insertando JavaScript en un sitio Web. Puede usar un ataque de inyección de código JavaScript para realizar un ataque de Scripting entre sitios (XSS). En un ataque XSS, que le roban información confidencial del usuario y enviar la información a otro sitio Web.

Por ejemplo, un pirata informático puede utilizar un ataque de inyección de código JavaScript para robar los valores de las cookies del explorador de otros usuarios. Si se almacena información confidencial, como contraseñas, números de tarjeta de crédito o números de la seguridad social: en las cookies del explorador, un pirata informático puede usar un ataque de inyección de código JavaScript para robar esta información. O bien, si un usuario escribe información confidencial en un campo de formulario en una página que se ha puesto en peligro con un ataque de JavaScript, el pirata informático puede utilizar el código de JavaScript insertado para capturar los datos del formulario y enviarlo a otro sitio Web.

*Ten asustado*. Realizar ataques de inyección de código de JavaScript en serio y proteger la información confidencial de sus usuarios. En las dos secciones siguientes, se explican dos técnicas que puede utilizar para proteger las aplicaciones de ASP.NET MVC frente a ataques de inyección de código de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Enfoque #1: Codificación de HTML en la vista

Un método sencillo de prevención de ataques de inyección de código de JavaScript es HTML codificar los datos introducidos por los usuarios del sitio Web al volver a mostrar los datos en una vista. La actualización `Index` vista en la lista 3 sigue este enfoque.

**El listado 3 – `Index.aspx` (codificado en HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Tenga en cuenta que el valor de `feedback.Message` es codificada en HTML antes de que el valor se muestra con el código siguiente:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

¿Qué significa HTML codificar una cadena? Cuando se HTML codifica una cadena, peligroso caracteres como `<` y `>` se reemplazan por las referencias de entidad HTML como `&lt;` y `&gt;`. Por lo que cuando la cadena `<script>alert("Boo!")</script>` es HTML codificado, se convierta en `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La cadena codificada ya no se ejecuta como una secuencia de comandos de JavaScript cuando se interpreta por un explorador. En su lugar, obtendrá la página inofensiva en la figura 3.


[![Rechace el ataque de JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: rechace el ataque de JavaScript ([haga clic aquí para ver la imagen a tamaño completo](preventing-javascript-injection-attacks-vb/_static/image9.png))


Tenga en cuenta que en el `Index` ver en el listado 3 solo el valor de `feedback.Message` se codifica. El valor de `feedback.EntryDate` no es necesario codificar. Solo debe codificar los datos introducidos por un usuario. Dado que el valor de EntryDate se generó en el controlador, no necesita en HTML codificar este valor.

## <a name="approach-2-html-encode-in-the-controller"></a>Enfoque #2: Codificación de HTML en el controlador

En lugar de datos de codificación cuando se muestran los datos en una vista HTML, puede HTML codificar los datos antes de enviar los datos a la base de datos. Este segundo método se realiza en el caso de los `controller` en el listado 4.

**Listado 4 – `HomeController.cs` (codificado en HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Observe que el valor del mensaje se codifica en HTML antes de que el valor se envía a la base de datos dentro de la `Create()` acción. Cuando el mensaje se vuelve a mostrar en la vista, el mensaje está codificado en HTML y no se ejecuta todo el código JavaScript insertado en el mensaje.

Normalmente, conviene utilizar el primer enfoque descrito en este tutorial sobre este segundo enfoque. El problema con este segundo enfoque es que terminará con datos codificado en HTML en la base de datos. En otras palabras, los datos de la base de datos es desfasados con chistes caracteres la claridad.

¿Por qué es incorrecto? Si alguna vez necesita mostrar los datos de la base de datos en un valor distinto de una página web, tendrá problemas. Por ejemplo, puede mostrar fácilmente ya no los datos en una aplicación de formularios Windows Forms.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era imposibles acerca de la perspectiva de un ataque de inyección de código de JavaScript. Este tutorial describe dos enfoques para defenderse de las aplicaciones de ASP.NET MVC frente a ataques de inyección de código de JavaScript: puede ya sea HTML codificar usuario enviada datos en la vista o bien pueden HTML codificar usuario enviado datos en el controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-vb.md)
