---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: Prevención de ataques de inyección de código de JavaScript (C#) | Microsoft Docs
author: StephenWalther
description: Evitar los ataques por inyección de código de JavaScript y realización de ataques entre sitios suceda para usted. En este tutorial, Stephen Walther explica cómo le resultará muy fácil de...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 77d0f0346e9eff756cd74c64c310918f3c367ab1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827550"
---
<a name="preventing-javascript-injection-attacks-c"></a>Prevención de ataques de inyección de código de JavaScript (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descargar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> Evitar los ataques por inyección de código de JavaScript y realización de ataques entre sitios suceda para usted. En este tutorial, Stephen Walther explica cómo se pueden anular con facilidad estos tipos de ataques mediante la codificación del contenido HTML.


El objetivo de este tutorial es explicar cómo puede impedir los ataques de inyección de código de JavaScript en sus aplicaciones de ASP.NET MVC. En este tutorial se describe dos enfoques para defenderse frente a su sitio Web contra un ataque de inyección de código de JavaScript. Aprenda a evitar ataques de inyección de código de JavaScript mediante la codificación de los datos que mostrar. También aprenderá a evitar ataques de inyección de código de JavaScript mediante la codificación de los datos que aceptan.

## <a name="what-is-a-javascript-injection-attack"></a>¿Qué es un ataque de inyección de código JavaScript?

Cada vez que se aceptan la entrada de usuario y volver a mostrar la entrada del usuario, abra el sitio Web a los ataques de inyección de código JavaScript. Vamos a examinar una aplicación concreta que está abierta a ataques de inyección de código de JavaScript.

Imagine que ha creado un sitio Web de comentarios de cliente (consulte la figura 1). Los clientes pueden visitar el sitio Web y escribir comentarios sobre su experiencia con sus productos. Cuando un cliente envíe sus comentarios, los comentarios se vuelve a mostrar en la página de comentarios.


[![Sitio Web de comentarios del cliente](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**Figura 01**: sitio Web de comentarios del cliente ([haga clic aquí para ver imagen en tamaño completo](preventing-javascript-injection-attacks-cs/_static/image3.png))


El sitio Web de comentarios de cliente utiliza el `controller` en el listado 1. Esto `controller` contiene dos acciones denominadas `Index()` y `Create()`.

**Listado 1: `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

El `Index()` método muestra el `Index` vista. Este método pasa todos los comentarios del cliente anterior a la `Index` vista mediante la recuperación de los comentarios de la base de datos (mediante una consulta de LINQ to SQL).

El `Create()` método crea un nuevo elemento de comentarios y lo agrega a la base de datos. El mensaje que entra en el cliente en el formulario se pasa a la `Create()` método en el parámetro de mensaje. Se crea un elemento de comentarios y se asigna al mensaje para el elemento de comentarios `Message` propiedad. El elemento de comentarios se envía a la base de datos con el `DataContext.SubmitChanges()` llamada al método. Por último, el visitante se redirige a la `Index` vista donde todos los comentarios se muestran.

El `Index` vista está incluida en el listado 2.

**Listado 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

El `Index` vista tiene dos secciones. La sección superior contiene el formulario de comentarios de clientes reales. La sección inferior contiene un For... Cada bucle que recorre en bucle todos los elementos de comentarios de clientes anterior y muestra las propiedades de mensaje y EntryDate para cada elemento de comentarios.

El sitio Web de comentarios de clientes es un sitio Web simple. Lamentablemente, el sitio Web está abierto a ataques de inyección de código de JavaScript.

Imagine que escriba el texto siguiente en el formulario de comentarios de clientes:

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

Este texto representa una secuencia de comandos de JavaScript que muestra un cuadro de mensaje de alerta. Después de que alguien envía esta secuencia de comandos en los comentarios de formulario, el mensaje <em>Boo!</em> aparecerá cada vez que alguien visita el sitio Web de comentarios de clientes en el futuro (consulte la figura 2).


[![Inserción de JavaScript](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**Figura 02**: inserción de JavaScript ([haga clic aquí para ver imagen en tamaño completo](preventing-javascript-injection-attacks-cs/_static/image6.png))


Ahora, la respuesta inicial a los ataques de inyección de código de JavaScript podría ser apathy. Es posible que piense que los ataques de inyección de código de JavaScript son simplemente un tipo de *pretendían* ataque. Puede que consideres que nadie puede hacer nada malo realmente mediante la confirmación de un ataque de inyección de código de JavaScript.

Lamentablemente, un hacker puede realizar algunas realmente, las cosas realmente evil mediante la inserción de JavaScript en un sitio Web. Puede utilizar un ataque de inyección de código de JavaScript para realizar un ataque de Scripting entre sitios (XSS). En un ataque XSS, robar información confidencial del usuario y la información de envío a otro sitio Web.

Por ejemplo, un hacker puede utilizar un ataque de inyección de código de JavaScript para robar los valores de las cookies del explorador a otros usuarios. Si se almacena información confidencial, como contraseñas, números de tarjeta de crédito o números del seguro social: en las cookies del explorador, un hacker puede utilizar un ataque de inyección de código de JavaScript para robar esta información. O bien, si un usuario escribe la información confidencial en un campo de formulario en una página que se ha visto comprometida con un ataque de JavaScript, el hacker puede usar el código JavaScript insertado para tomar los datos del formulario y enviarlo a otro sitio Web.

*Por favor, tenga miedo de mezclarlas*. Tomar en serio los ataques de inyección de código de JavaScript y proteger información confidencial del usuario. En las dos secciones siguientes, se describen dos técnicas que puede usar para proteger las aplicaciones de ASP.NET MVC frente a ataques de inyección de código de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Método #1: Codifica como HTML en la vista

Un método sencillo para prevenir ataques por inyección de código de JavaScript es HTML codifican los datos introducidos por los usuarios del sitio Web al volver a mostrar los datos en una vista. La actualización `Index` vista en el listado 3 sigue este enfoque.

**Listado 3 – `Index.aspx` (codificado en HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

Tenga en cuenta que el valor de `feedback.Message` es codificada en HTML antes de que se muestra el valor con el código siguiente:

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

¿Qué significa para HTML codificar una cadena? Cuando se HTML codifica una cadena, peligroso caracteres como `<` y `>` se reemplazan por las referencias de entidad HTML como `&lt;` y `&gt;`. Por lo que cuando la cadena `<script>alert("Boo!")</script>` es HTML codificado, se convierten en `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La cadena codificada ya no se ejecuta como un script de JavaScript cuando se interpreta mediante un explorador. En su lugar, obtener la página inofensiva en la figura 3.


[![Rechace ataque de JavaScript](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**Figura 03**: rechaza ataque de JavaScript ([haga clic aquí para ver imagen en tamaño completo](preventing-javascript-injection-attacks-cs/_static/image9.png))


Tenga en cuenta que en el `Index` ver en el listado 3, solo el valor de `feedback.Message` está codificado. El valor de `feedback.EntryDate` no está codificado. Solo tiene que codificar datos introducidos por el usuario. Dado que el valor de EntryDate se generó en el controlador, no necesita a HTML codifica este valor.

## <a name="approach-2-html-encode-in-the-controller"></a>Método #2: Codificación de HTML en el controlador

En lugar de codificar los datos al mostrar los datos en una vista HTML, puede HTML codifican los datos antes de enviar los datos a la base de datos. Este segundo método se realiza en el caso de los `controller` en el listado 4.

**Listado 4 – `HomeController.cs` (codificado en HTML)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

Observe que el valor del mensaje se codifica en HTML antes de que el valor se envía a la base de datos dentro de la `Create()` acción. Cuando el mensaje se vuelve a mostrar en la vista, el mensaje está codificado en HTML y no se ejecutará cualquier código JavaScript insertado en el mensaje.

Por lo general, debería favorecer el primer enfoque se describe en este tutorial con este segundo método. El problema con este segundo método es que acaba con datos codificado en HTML en la base de datos. En otras palabras, los datos de la base de datos es desfasados con graciosos caracteres atractivos.

¿Por qué es incorrecta? Si alguna vez necesita mostrar los datos de la base de datos en un valor distinto de una página web, tendrá problemas. Por ejemplo, puede mostrar fácilmente ya no los datos en una aplicación de Windows Forms.

## <a name="summary"></a>Resumen

El propósito de este tutorial era imposibles acerca de la perspectiva de un ataque de inyección de código de JavaScript. En este tutorial se describe dos enfoques para defenderse frente a las aplicaciones de ASP.NET MVC frente a ataques de inyección de código de JavaScript: puede cualquier HTML codificar usuario enviado datos en la vista o HTML pueden codificar usuario enviado datos en el controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-cs.md)
> [Siguiente](authenticating-users-with-forms-authentication-vb.md)
