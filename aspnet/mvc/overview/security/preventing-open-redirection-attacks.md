---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevención de ataques de redireccionamiento abierto (C#) | Microsoft Docs
author: jongalloway
description: Este tutorial explica cómo puede impedir los ataques de redireccionamiento abierto en sus aplicaciones de ASP.NET MVC. En este tutorial se describe los cambios que se han realizado...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 767f9c85527fbcdf34e700eb32fe0c6cad30bf0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828762"
---
<a name="preventing-open-redirection-attacks-c"></a>Prevención de ataques de redireccionamiento abierto (C#)
====================
por [Jon Galloway](https://github.com/jongalloway)

> Este tutorial explica cómo puede impedir los ataques de redireccionamiento abierto en sus aplicaciones de ASP.NET MVC. Este tutorial describe los cambios que se han realizado en AccountController en ASP.NET MVC 3 y muestra cómo se pueden aplicar estos cambios en la versión existente de ASP.NET MVC 1.0 y 2 aplicaciones.


## <a name="what-is-an-open-redirection-attack"></a>¿Qué es un ataque de redireccionamiento abierto?

Cualquier aplicación web que se redirige a una dirección URL que se especifica a través de la solicitud, como los datos de cadena de consulta o formulario potencialmente ser alterado para redirigir usuarios a una dirección URL externa, malintencionada. Esta modificación se llama a un ataque de redireccionamiento abierto.

Cada vez que la lógica de aplicación redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado. El inicio de sesión utilizado en el valor predeterminado de AccountController para ASP.NET MVC 1.0 y ASP.NET MVC 2 es vulnerable a ataques de redirección de abrir. Afortunadamente, es fácil de actualizar las aplicaciones existentes para usar las correcciones desde la versión preliminar de ASP.NET MVC 3.

Para entender la vulnerabilidad, echemos un vistazo a cómo funciona la redirección de inicio de sesión en un proyecto de aplicación Web de ASP.NET MVC 2 predeterminado. En esta aplicación, al intentar visitar una acción de controlador que tiene el atributo [Authorize] redirigirá los usuarios no autorizados a la vista /Account/LogOn. Esta redirección a /Account/LogOn incluirá un parámetro de cadena de consulta returnUrl para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente.

¿En la siguiente captura de pantalla, podemos ver que un intento de acceder a la vista de /Account/ChangePassword cuando no ha iniciado sesión los resultados en una redirección a /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: página de inicio de sesión con una redirección abierta

Puesto que no se valida el parámetro de cadena de consulta ReturnUrl, un atacante puede modificar para insertar cualquier dirección URL en el parámetro para llevar a cabo un ataque de redireccionamiento abierto. ¿Para demostrar esto, podemos modificar el parámetro ReturnUrl [ http://bing.com ](http://bing.com), por lo que la dirección URL de inicio de sesión resultante será/cuenta/inicio de sesión? ReturnUrl =<http://www.bing.com/>. Tras iniciar sesión correctamente el sitio, estamos redirigidos a [ http://bing.com ](http://bing.com). Puesto que no se valida la esta redirección, en su lugar podría señalar a un sitio malintencionado que intenta engañar al usuario.

### <a name="a-more-complex-open-redirection-attack"></a>Un ataque de redireccionamiento abierto más complejos

Los ataques de redireccionamiento abierto son especialmente peligrosos porque un atacante sabe que estamos intentando iniciar sesión en un sitio Web específico, lo que nos permite ser vulnerable a un [ataque de suplantación de identidad](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por ejemplo, un atacante podría enviar mensajes de correo electrónico a los usuarios del sitio Web en un intento de capturar sus contraseñas. Echemos un vistazo a cómo funciona en el sitio de NerdDinner. (Tenga en cuenta que se ha actualizado el sitio activo de NerdDinner para protegerse frente a ataques de redireccionamiento abierto).

En primer lugar, un atacante envía nos un vínculo a la página de inicio de sesión de NerdDinner que incluye un redireccionamiento para sus páginas falsificadas:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Tenga en cuenta que la dirección URL de retorno apunta a nerddiner.com, que le falta una "n" de la cena de word. En este ejemplo, se trata de un dominio que controla el atacante. Cuando se accede al vínculo anterior, nos estamos dirigirá a la página de inicio de sesión de NerdDinner.com legítima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: página de inicio de sesión de NerdDinner con un redireccionamiento abierto

Cuando se inician correctamente, acción de inicio de sesión de ASP.NET MVC AccountController nos redirige a la dirección URL especificada en el parámetro de cadena de consulta returnUrl. En este caso, es la dirección URL que el atacante ha entrado, que es [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). A menos que estamos muy atentos a los posibles, es muy probable que no tenga en cuenta esto, especialmente porque el atacante ha cuidadoso para asegurarse de que sus páginas falsificadas busca exactamente igual que la página de inicio de sesión legítima. Esta página de inicio de sesión incluye un mensaje de error que solicita que se iniciar sesión de nuevo. El torpe nosotros, nos debemos haber escrito incorrectamente la contraseña.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: pantalla de inicio de sesión de NerdDinner falsificado

Cuando se vuelva a escribir el nombre de usuario y contraseña, la página de inicio de sesión falsificados guarda la información y nos envía al sitio NerdDinner.com legítimo. En este momento, el sitio NerdDinner.com ya autenticado nosotros, por lo que puede redirigir la página de inicio de sesión falsificados directamente a esa página. El resultado final es que el atacante tiene nuestro nombre de usuario y contraseña, y somos conscientes de que nos hemos proporcionado a ellos.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Examinar el código vulnerable en la acción de inicio de sesión de AccountController

El código para la acción de inicio de sesión en una aplicación ASP.NET MVC 2 se muestra a continuación. Tenga en cuenta que cuando inician una sesión correctamente, el controlador devuelve un redireccionamiento a URL devuelto. Puede ver que no se realiza ninguna validación con respecto al parámetro returnUrl.

**Listado 1: acción de inicio de sesión de ASP.NET MVC 2 en `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Ahora Echemos un vistazo a los cambios a la acción de inicio de sesión de ASP.NET MVC 3. Este código se ha cambiado para validar el parámetro returnUrl mediante una llamada a un nuevo método en la clase de aplicación auxiliar System.Web.Mvc.Url denominada `IsLocalUrl()`.

**Listado 2: acción de inicio de sesión de ASP.NET MVC 3 en `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Esto se ha cambiado para validar el parámetro de dirección URL de devolución mediante una llamada a un nuevo método en la clase auxiliar System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protección de ASP.NET MVC 1.0 y MVC 2 aplicaciones

Nos podemos aprovechar los cambios de ASP.NET MVC 3 en nuestra versión existente de ASP.NET MVC 1.0 y 2 aplicaciones agregando el método auxiliar IsLocalUrl() y actualizar la acción de inicio de sesión para validar el parámetro returnUrl.

También se usa el método UrlHelper IsLocalUrl() simplemente llamar a un método en System.Web.WebPages, como esta validación por las aplicaciones de ASP.NET Web Pages.

**Listado 3: el método IsLocalUrl() desde el UrlHelper de ASP.NET MVC 3 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

El método IsUrlLocalToHost contiene la lógica de validación real, como se muestra en el listado 4.

**Listado 4: método IsUrlLocalToHost() desde la clase System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

En nuestro ASP.NET MVC 1.0 o una aplicación de 2, vamos a agregar un método IsLocalUrl() a AccountController, pero le recomendamos que agregue a una clase auxiliar independiente si es posible. Se realizará dos pequeños cambios en la versión de ASP.NET MVC 3 de IsLocalUrl() para que funcione dentro de AccountController. En primer lugar, cambiaremos, desde un método público a un método privado, ya que pueden tener acceso a los métodos públicos en los controladores como acciones de controlador. En segundo lugar, modificaremos la llamada que comprueba el host de dirección URL en el host de aplicación. Que llamada hace uso de un local RequestContext campo en la clase UrlHelper. En lugar de usar esto. RequestContext.HttpContext.Request.Url.Host, se usará. Request.Url.Host. El código siguiente muestra el método IsLocalUrl() modificado para su uso con una clase de controlador en ASP.NET MVC 1.0 y 2 aplicaciones.

**Listado 5 – método IsLocalUrl(), que se modifica para su uso con una clase de controlador de MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ahora que el método IsLocalUrl() está en su lugar, podemos llamarlo desde la acción de inicio de sesión para validar el parámetro returnUrl, tal como se muestra en el código siguiente.

**Listado 6: método de inicio de sesión de actualizado que valida el parámetro de returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Ahora podemos probar un ataque de redireccionamiento abierto intentando iniciar sesión con una dirección URL de retorno externa. ¿Vamos a usar /Account/LogOn? ReturnUrl =<http://www.bing.com/> nuevo.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: para probar la acción de inicio de sesión actualizada

Después de iniciar sesión correctamente, nos estamos redirige a la acción del controlador Home/Index en lugar de la dirección URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: en contra de ataques de redireccionamiento abierto

## <a name="summary"></a>Resumen

Los ataques de redireccionamiento abierto pueden producirse cuando las direcciones URL de redirección se pasan como parámetros en la dirección URL para una aplicación. La plantilla incluye código para protegerse frente a ASP.NET MVC 3 abra los ataques de redireccionamiento. Puede agregar este código con algunas modificaciones en ASP.NET MVC 1.0 y 2 aplicaciones. Para protegerse contra los ataques de redireccionamiento abierto al iniciar sesión en ASP.NET 1.0 y 2 aplicaciones, agregue un método IsLocalUrl() y validar el parámetro returnUrl en la acción de inicio de sesión.
