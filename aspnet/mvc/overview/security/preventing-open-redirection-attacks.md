---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "Prevención de ataques de redirección abierta (C#) | Documentos de Microsoft"
author: jongalloway
description: "Este tutorial le explica cómo se pueden impedir los ataques de redirección abiertos en sus aplicaciones de ASP.NET MVC. Este tutorial describe los cambios que se han realizado..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 17944c0600a174176e3e9940f414b34f0835b800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Prevención de ataques de redirección abierta (C#)
====================
por [Jon Galloway](https://github.com/jongalloway)

> Este tutorial le explica cómo se pueden impedir los ataques de redirección abiertos en sus aplicaciones de ASP.NET MVC. Este tutorial describe los cambios realizados en el AccountController en ASP.NET MVC 3 y muestra cómo puede aplicar estos cambios en la versión existente de ASP.NET MVC 1.0 y 2 aplicaciones.


## <a name="what-is-an-open-redirection-attack"></a>¿Qué es un ataque de redirección abierta?

Cualquier aplicación web que se redirige a una dirección URL que se especifica a través de la solicitud como la cadena de consulta o un formulario de datos potencialmente puede alterado para redirigir a los usuarios a una dirección URL externa, malintencionada. Esta modificación se llama a un ataque de redirección abierta.

Cada vez que la lógica de aplicación se redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado. El inicio de sesión utilizado en el valor predeterminado AccountController para ASP.NET MVC 1.0 y ASP.NET MVC 2 es vulnerable a ataques de redirección de abrir. Afortunadamente, es fácil de actualizar las aplicaciones existentes para usar las correcciones de la versión preliminar de ASP.NET MVC 3.

Para entender la vulnerabilidad, echemos un vistazo a cómo funciona la redirección de inicio de sesión en un proyecto de aplicación Web de ASP.NET MVC 2 predeterminado. En esta aplicación, intentar visitar una acción de controlador que tiene el atributo [Authorize] redirigirá los usuarios no autorizados a la vista de /Account/LogOn. Esta redirección a /Account/LogOn incluirá un parámetro de cadena de consulta returnUrl para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente.

¿En la siguiente captura de pantalla, podemos ver que intenta acceder a la vista de /Account/ChangePassword cuando no ha iniciado sesión los resultados en una redirección a /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: página de inicio de sesión con una redirección abierta

Puesto que no se valida el parámetro de cadena de consulta ReturnUrl, un atacante puede modificar para insertar cualquier dirección URL en el parámetro para realizar un ataque de redirección abierta. Para demostrar esto, se podrá modificar el parámetro ReturnUrl [http://bing.com](http://bing.com), por lo que la dirección URL de inicio de sesión resultante será/Account/inicio de sesión? ReturnUrl = http://www.bing.com/. Tras iniciar sesión correctamente el sitio, estamos redirigidos a [http://bing.com](http://bing.com). Puesto que no se valida esta redirección, podría señalar en su lugar a un sitio malintencionado que intenta engañar al usuario.

### <a name="a-more-complex-open-redirection-attack"></a>Un ataque de redirección abierta más complejos

Los ataques de redirección abierta son especialmente peligrosos, porque un atacante sabe que estamos intentando iniciar sesión en un sitio Web específico, lo que nos hace vulnerables a un [ataque de suplantación de identidad](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por ejemplo, un atacante podría enviar mensajes de correo electrónico malintencionados a usuarios del sitio Web en un intento para capturar sus contraseñas. Echemos un vistazo a cómo funcionaría en el sitio NerdDinner. (Tenga en cuenta que se ha actualizado el sitio de NerdDinner activo para protegerse frente a ataques de redirección abierta).

En primer lugar, un atacante envía es un vínculo a la página de inicio de sesión en NerdDinner que incluye un redireccionamiento para sus páginas falsificadas:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Tenga en cuenta que la dirección URL de retorno señala a nerddiner.com, que le falta una "n" de la cena de word. En este ejemplo, se trata de un dominio que controla el atacante. Cuando se tiene acceso al vínculo anterior, nos estamos pasará a la página de inicio de sesión de NerdDinner.com legítima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: página de inicio de sesión de NerdDinner con una redirección abierta

Cuando se inicia sesión correctamente, acción de inicio de sesión del AccountController de ASP.NET MVC nos redirige a la dirección URL especificada en el parámetro de cadena de consulta returnUrl. En este caso, es la dirección URL que ha escrito el atacante, que es [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). A menos que estamos muy por ejemplo, es muy probable que no perciba este comportamiento, sobre todo porque el atacante ha sido cuidado para asegurarse de que su página falsificado es exactamente igual que la página de inicio de sesión legítimo. Esta página de inicio de sesión incluye un mensaje de error que solicita que se sesión de nuevo. Difíciles de manejar us, nos debemos haya escrito incorrectamente la contraseña.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: pantalla de inicio de sesión de NerdDinner falsificado

Cuando se vuelva a escribir el nombre de usuario y una contraseña, la página de inicio de sesión falsificados guarda la información y nos envía al sitio NerdDinner.com legítimo. En este momento, el sitio de NerdDinner.com ya ha autenticado us, por lo que puede redirigir la página de inicio de sesión falsificados directamente a esa página. El resultado final es que el atacante tiene el nombre de usuario y contraseña, y no son conscientes de que nos hemos proporcionado a ellos.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Examinando el código vulnerable en la acción de inicio de sesión de AccountController

El código para la acción de inicio de sesión en una aplicación de ASP.NET MVC 2 se muestra a continuación. Tenga en cuenta que cuando inician una sesión correctamente, el controlador devuelve un redireccionamiento para el elemento returnUrl. Puede ver que no se realiza ninguna validación con respecto al parámetro returnUrl.

**Lista 1: acción de inicio de sesión de ASP.NET MVC 2 en`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Ahora Echemos un vistazo a los cambios a la acción de inicio de sesión de ASP.NET MVC 3. Este código se ha cambiado para validar el parámetro returnUrl al llamar a un nuevo método de la clase de aplicación auxiliar de System.Web.Mvc.Url denominada `IsLocalUrl()`.

**La lista 2: acción de inicio de sesión de ASP.NET MVC 3 en`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Esto se ha cambiado para validar el parámetro de dirección URL devuelto mediante una llamada a un nuevo método en la clase de aplicación auxiliar System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Proteger sus MVC de ASP.NET 1.0 y MVC 2 de aplicaciones

Podemos realizar ventaja de los cambios de ASP.NET MVC 3 en nuestra versión existente de ASP.NET MVC 1.0 y 2 aplicaciones agregando el método de aplicación auxiliar de IsLocalUrl() y actualizar la acción de inicio de sesión para validar el parámetro returnUrl.

El método UrlHelper IsLocalUrl() realmente simplemente llamar a un método en System.Web.WebPages, como esta validación también se utiliza en aplicaciones de ASP.NET Web Pages.

**Lista 3: el método IsLocalUrl() desde el UrlHelper de ASP.NET MVC 3`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

El método IsUrlLocalToHost contiene la lógica de validación real, tal como se muestra en el listado 4.

**Enumerar 4: método IsUrlLocalToHost() de la clase System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

En nuestro MVC de ASP.NET 1.0 o aplicación 2, vamos a agregar un método IsLocalUrl() a la AccountController, pero le anima a agregar a una clase auxiliar independiente si es posible. Se realizará dos pequeños cambios en la versión de ASP.NET MVC 3 de IsLocalUrl() para que funcione dentro de la AccountController. En primer lugar, cambiaremos, desde un método público a un método privado, ya que pueden tener acceso a métodos públicos en los controladores como acciones del controlador. En segundo lugar, modificaremos la llamada que comprueba el host de la dirección URL en el host de aplicación. Que hace que el uso de un RequestContext local llamada campo en la clase UrlHelper. En lugar de usar esto. RequestContext.HttpContext.Request.Url.Host, se usará. Request.Url.Host. El código siguiente muestra el método IsLocalUrl() modificado para su uso con una clase de controlador en ASP.NET MVC 1.0 y 2 aplicaciones.

**Enumerar 5: método IsLocalUrl(), que se modifica para su uso con una clase de controlador de MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ahora que el método IsLocalUrl() está en su lugar, podemos llamarlo desde la acción de inicio de sesión para validar el parámetro returnUrl, tal como se muestra en el código siguiente.

**Enumerar 6: método de inicio de sesión actualizadas que valida el parámetro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Ahora podemos probar un ataque de redirección abierta intentando iniciar sesión con una dirección URL de retorno externa. Vamos a usar/Account/inicio de sesión? ReturnUrl = http://www.bing.com/ de nuevo.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: para probar la acción de inicio de sesión actualizada

Después de iniciar sesión correctamente, nos estamos redirige a la acción del controlador Home/Index en lugar de la dirección URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: en contra de ataques de redirección abierta

## <a name="summary"></a>Resumen

Los ataques de redirección abierta pueden producirse cuando las direcciones URL de redirección se pasan como parámetros en la dirección URL para una aplicación. ASP.NET MVC 3 plantilla incluye código para protegerse frente a abrir los ataques de redirección. Puede agregar este código con alguna modificación a ASP.NET MVC 1.0 y 2 aplicaciones. Para protegerse contra los ataques de redirección abierta al iniciar sesión en ASP.NET 1.0 y 2 aplicaciones, agregue un método IsLocalUrl() y validar el parámetro returnUrl en la acción de inicio de sesión.
