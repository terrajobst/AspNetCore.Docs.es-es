---
title: "Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core"
author: steve-smith
description: "Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 079c36535b8c9e7229952a2f7bcd53174effa6af
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), y [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>¿Qué ataque evita antifalsificación?

Falsificación de solicitudes entre sitios (también conocido como XSRF o CSRF, pronunciado *vea navegación*) es un ataque contra las aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en la interacción entre un explorador del cliente y un sitio web que se confía ese explorador. Estos ataques se realizan porque algunos tipos de tokens de autenticación automáticamente con cada solicitud de envío de los exploradores web a un sitio web. Esta forma de vulnerabilidad de seguridad del también se denomina un *ataque de un solo clic* o como *sesión riding*, ya que el ataque se aprovecha del usuario del autenticado previamente la sesión.

Un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en `www.example.com`, utilizando la autenticación de formularios.
2. El servidor autentica al usuario y emite una respuesta que incluye una cookie de autenticación.
3. El usuario visita un sitio malintencionado.

   El sitio malintencionado contiene un formulario HTML similar al siguiente:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Tenga en cuenta que la acción de formulario se envía al sitio vulnerable, no en el sitio malintencionado. Esta es la parte "cross-site" de CSRF.

4. El usuario hace clic en el botón Enviar. El explorador incluye automáticamente la cookie de autenticación para el dominio solicitado (es decir, el sitio sea vulnerable en este caso) con la solicitud.
5. La solicitud se ejecuta en el servidor con el contexto de autenticación del usuario y puede realizar cualquier acción que un usuario autenticado puede hacer.

Este ejemplo requiere que el usuario haga clic en el botón del formulario. La página malintencionada podría:

* Ejecutar un script que envía automáticamente el formulario.
* Envía un envío de formulario como una solicitud AJAX. 
* Usar un formulario oculto con CSS. 

Uso de SSL no impide que un ataque CSRF, el sitio malintencionado puede enviar un `https://` solicitud. 

Algunos ataques de destino los puntos de conexión de sitio que respondan a `GET` solicitudes, en cuyo caso una etiqueta de imagen puede se utiliza para realizar la acción (esta forma de ataque es común en los sitios de foro que permiten imágenes pero bloquean JavaScript). Las aplicaciones que cambian el estado con `GET` solicitudes son vulnerables frente a ataques malintencionados.

Los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes para el sitio web de destino. Sin embargo, los ataques CSRF no están limitados a aprovecharse de las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Una vez que un usuario inicia sesión con autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

Nota: en este contexto, *sesión* hace referencia a la sesión de cliente durante el cual el usuario está autenticado. Es no relacionados con las sesiones de servidor o [sesión middleware](xref:fundamentals/app-state).

Los usuarios pueden protegerse contra vulnerabilidades CSRF por:
* Registro fuera de sitios web cuando termine de utilizarlos.
* Borrar las cookies de su explorador periódicamente.

Sin embargo, las vulnerabilidades CSRF son fundamentalmente un problema con la aplicación web, no el usuario final.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>¿Cómo satisface MVC de ASP.NET Core CSRF?

> [!WARNING]
> ASP.NET Core implementa anti-la falsificación de solicitud usando el [pila de protección de datos de ASP.NET Core](xref:security/data-protection/introduction). Protección de datos de ASP.NET Core debe configurarse para que funcione en una granja de servidores. Vea [configurando la protección de datos](xref:security/data-protection/configuration/overview) para obtener más información.

Configuración de protección de datos de ASP.NET Core anti-la falsificación de solicitud predeterminado 

En el núcleo de ASP.NET MVC 2.0 la [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserta tokens antifalsificación para los elementos de formulario HTML. Por ejemplo, el marcado siguiente en un archivo Razor generará automáticamente tokens antifalsificación:

```html
<form method="post">
  <!-- form markup -->
</form>
```

La generación automática de tokens antifalsificación para los elementos de formulario HTML ocurre cuando:

* El `form` etiqueta contiene el `method="post"` atributo AND

  * El atributo de acción está vacío. ( `action=""`) O
  * No se especifica el atributo de acción. (`<form method="post">`)

Puede deshabilitar la generación automática de tokens antifalsificación para los elementos de formulario HTML mediante:

* Deshabilita explícitamente `asp-antiforgery`. Por ejemplo

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* Elegir el elemento de formulario de aplicaciones auxiliares de etiquetas mediante el uso de la aplicación auxiliar de etiqueta [! desactivación símbolo](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Quitar el `FormTagHelper` de la vista. Puede quitar el `FormTagHelper` desde una vista agregando la siguiente directiva a la vista Razor:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Las páginas de Razor](xref:mvc/razor-pages/index) están protegidos automáticamente frente a XSRF/CSRF. No tienes que escribir ningún código adicional. Vea [XSRF/CSRF y páginas de Razor](xref:mvc/razor-pages/index#xsrf) para obtener más información.

El enfoque más común para defenderse contra ataques CSRF es el patrón del token Sincronizador (STP). STP es una técnica que se utiliza cuando el usuario solicita una página con datos del formulario. El servidor envía un token asociado con la identidad del usuario actual al cliente. El cliente devuelve el token en el servidor para la comprobación. Si el servidor recibe un token que no coincide con la identidad del usuario autenticado, se rechaza la solicitud. El token es único y no previstos. El token también puede usarse para asegurarse de la secuencia correcta de una serie de solicitudes (página 2 que precede a la página 3 que precede a garantizar página 1). Todas las formas en las plantillas de MVC de ASP.NET Core generan tokens antiforgery. Los dos ejemplos siguientes de lógica de vista generan tokens antiforgery:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Puede agregar explícitamente un token antiforgery a una ``<form>`` elemento sin usar aplicaciones auxiliares de etiquetas con la aplicación auxiliar HTML ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

En cada uno de los casos anteriores, ASP.NET Core se agregará un campo de formulario oculto similar al siguiente:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core incluye tres [filtros](xref:mvc/controllers/filters) para trabajar con tokens antiforgery: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, y ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

El ``ValidateAntiForgeryToken`` es un filtro de acción que se puede aplicar a una acción individual, un controlador, o de forma global. Se bloquearán las solicitudes realizadas a las acciones que tienen este filtro se aplica a menos que la solicitud incluye un token de antiforgery válido.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

El ``ValidateAntiForgeryToken`` atributo requiere un token para las solicitudes a los métodos de acción decora, incluidos `HTTP GET` solicitudes. Si se aplica ampliamente, puede reemplazarlo con el ``IgnoreAntiforgeryToken`` atributo.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Las aplicaciones de ASP.NET Core generalmente no generan antiforgery tokens para los métodos de prueba de errores de HTTP (GET, HEAD, opciones y seguimiento). En lugar de aplicar ampliamente el ``ValidateAntiForgeryToken`` atributo y, a continuación, reemplazar con ``IgnoreAntiforgeryToken`` atributos, puede utilizar el ``AutoValidateAntiforgeryToken`` atributo. Este atributo funciona de forma idéntica a la ``ValidateAntiForgeryToken`` de atributo, salvo que no requiere tokens para las solicitudes realizadas con los siguientes métodos HTTP:

* GET
* HEAD
* OPCIONES
* TRACE

Le recomendamos que use ``AutoValidateAntiforgeryToken`` ampliamente para escenarios de API no. Esto garantiza que las acciones de entrada están protegidas de forma predeterminada. La alternativa es omitir antiforgery símbolos (tokens) de forma predeterminada, a menos que ``ValidateAntiForgeryToken`` se aplica al método de acción individuales. Es más probable en este escenario para un método de acción de POST como izquierda sin protección, salir de la aplicación sea vulnerable a ataques CSRF. ENTRADAS incluso anónimos deben enviar el token antiforgery.

Nota: Las API no tienen un mecanismo automático para el envío de la parte no cookie del token; es probable que dependerán de su implementación en la implementación de código de cliente. A continuación se muestran algunos ejemplos.


Ejemplo (clase):

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Ejemplo (global):

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

El ``IgnoreAntiforgeryToken`` filtro se utiliza para eliminar la necesidad de un token antiforgery estén presentes para una acción determinada (o controlador). Cuando se aplica, reemplazará este filtro ``ValidateAntiForgeryToken`` o ``AutoValidateAntiforgeryToken`` filtros especificados en un nivel más alto (global o en un controlador).

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX y SPAs

En las aplicaciones tradicionales basadas en HTML, antiforgery símbolos (tokens) se pasa al servidor mediante campos ocultos de formulario. En las aplicaciones modernas basadas en JavaScript y aplicaciones de una página (SPAs), muchas de las solicitudes se realizan mediante programación. Estas solicitudes de AJAX pueden usar otras técnicas (por ejemplo, los encabezados de solicitud o cookies) para enviar el token. Si se usan cookies para almacenar los tokens de autenticación y para autenticar las solicitudes de API en el servidor, CSRF será un problema potencial. Sin embargo, si se usa almacenamiento local para almacenar el token, CSRF vulnerabilidad puede ser mitigada, puesto que no se envían automáticamente los valores desde el almacenamiento local en el servidor con cada nueva solicitud. Por lo tanto, puede utilizar almacenamiento local para almacenar el token antiforgery en el cliente y enviar el token como un encabezado de solicitud es un enfoque recomendado.

### <a name="angularjs"></a>AngularJS

AngularJS usa una convención para dirección CSRF. Si el servidor envía una cookie con el nombre ``XSRF-TOKEN``, el Angular ``$http`` servicio agregará el valor de esta cookie a un encabezado cuando envía una solicitud a este servidor. Este proceso es automático; no es necesario establecer explícitamente el encabezado. El nombre de encabezado es ``X-XSRF-TOKEN``. El servidor debe detectar este encabezado y validar su contenido.

Para la API de ASP.NET básica trabajar con esta convención:

* Configurar la aplicación para proporcionar un token en una cookie denominada``XSRF-TOKEN``
* Configurar el servicio para que busque un encabezado denominado antiforgery``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Ejemplo de vista](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

Uso de JavaScript con vistas, puede crear el token con un servicio en la vista de. Para ello, insertar el `Microsoft.AspNetCore.Antiforgery.IAntiforgery` servicio en la vista y llame a `GetAndStoreTokens`, como se muestra:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Este enfoque elimina la necesidad de tratar directamente con la configuración de cookies del servidor o para leerlos desde el cliente.

JavaScript puede tener acceso a tokens que proporcionó en las cookies y, a continuación, usar el contenido de la cookie para crear un encabezado con el valor del token, tal y como se muestra a continuación.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

A continuación, suponiendo que crear su script solicita para enviar el token en un encabezado denominado ``X-CSRF-TOKEN``, configurar el servicio antiforgery para buscar la ``X-CSRF-TOKEN`` encabezado:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

En el ejemplo siguiente se utiliza jQuery para realizar una solicitud de AJAX con el encabezado adecuado:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Configurar Antiforgery

`IAntiforgery`proporciona la API para configurar el sistema antiforgery. Se puede solicitar en el `Configure` método de la `Startup` clase. En el ejemplo siguiente se usa el middleware de página principal de la aplicación para generar un token de antiforgery y enviarlo en la respuesta como una cookie (mediante la convención de nomenclatura de manera predeterminada Angular que se ha descrito anteriormente):


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Opciones

Puede personalizar [opciones antiforgery](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) en `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Opción        | Descripción |
|------------- | ----------- |
|CookieDomain  | El dominio de la cookie. Tiene como valor predeterminado `null`. |
|CookieName    | El nombre de la cookie. Si no se establece, el sistema genera un nombre único que comienza con la `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | La ruta de acceso establecido en la cookie. |
|FormFieldName | El nombre del campo oculto del formulario utilizado por el sistema antiforgery para representar antiforgery tokens en las vistas. |
|HeaderName    | El nombre del encabezado utilizado por el sistema antiforgery. Si `null`, el sistema considerará únicamente datos del formulario. |
|RequireSsl    | Especifica si se requiere SSL por el sistema antiforgery. Tiene como valor predeterminado `false`. Si `true`, se producirá un error en las solicitudes sin SSL. |
|SuppressXFrameOptionsHeader  | Especifica si se debe suprimir la generación de la `X-Frame-Options` encabezado. De forma predeterminada, el encabezado se genera con un valor de "SAMEORIGIN". Tiene como valor predeterminado `false`. |

Consulte https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions para obtener más información.

### <a name="extending-antiforgery"></a>Extender Antiforgery

El [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite a los desarrolladores extender el comportamiento del sistema anti-XSRF por los datos adicionales de ida y vuelta de cada token. El [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado. Un implementador podría devolver una marca de tiempo, un nonce o cualquier otro valor y, a continuación, llame a [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) para validar estos datos cuando se valida el token. Nombre de usuario del cliente ya está incrustada en los tokens generados, por lo que no es necesario incluir esta información. Si un token incluye datos suplementarios pero no `IAntiForgeryAdditionalDataProvider` ha sido configurado, no validados los datos suplementarios.

## <a name="fundamentals"></a>Aspectos básicos

Ataques CSRF se basan en el comportamiento del explorador predeterminado de enviar cookies asociadas a un dominio con cada solicitud realizada a ese dominio. Estas cookies se almacenan en el explorador. Con frecuencia incluyen cookies de sesión para los usuarios autenticados. Autenticación basada en cookies es una forma popular de autenticación. Sistemas de autenticación basada en autorización token han ido aumentando popular, especialmente para SPAs y otros escenarios "smart client".

### <a name="cookie-based-authentication"></a>Autenticación basada en cookies

Una vez que un usuario se autentica con su nombre de usuario y contraseña, le emite un token que puede usarse para identificarlos y validar que se han autenticado. El token se almacena como hace que una cookie que acompaña a cada solicitud del cliente. Generar y validar esta cookie se realiza mediante el middleware de autenticación de la cookie. ASP.NET Core proporciona una cookie [middleware](xref:fundamentals/middleware/index) que serializa una entidad de seguridad de usuario en una cookie cifrada y, a continuación, en solicitudes posteriores, valida la cookie, vuelve a crear la entidad de seguridad y lo asigna a la `User` propiedad `HttpContext`.

Cuando se utiliza una cookie, la cookie de autenticación es simplemente un contenedor para el vale de autenticación de formularios. El vale se pasa como el valor de la cookie de autenticación de formularios con cada solicitud y autenticación de formularios, en el servidor, se usa para identificar un usuario autenticado.

Cuando un usuario inicia sesión un sistema, una sesión de usuario se crea en el servidor y se almacena en una base de datos u otro almacén persistente. El sistema genera una clave de sesión que hacen referencia a la sesión actual en el almacén de datos y se envía como una cookie del lado cliente. El servidor web comprobará esta clave de sesión siempre que un usuario solicita un recurso que requiera una autorización. El sistema comprueba si la sesión de usuario asociado tiene el privilegio de acceso al recurso solicitado. Si es así, se continúa la solicitud. En caso contrario, la solicitud se devuelve como no autorizados. En este enfoque, las cookies se utilizan para hacer que la aplicación parece ser con estado, ya que es capaz de "recordar" el usuario ha autenticado previamente con el servidor.

### <a name="user-tokens"></a>Tokens de usuario

Autenticación basada en token no almacena la sesión en el servidor. Cuando un usuario ha iniciado sesión, le emite un token (no un token antiforgery). Este token contiene los datos que se necesita para validar el token. También contiene información de usuario en forma de [notificaciones](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Cuando un usuario desea tener acceso a un recurso de servidor que requiera autenticación, el token se envía al servidor con un encabezado de autorización adicionales en forma de portador {token}. Esto hace que la aplicación sin estado ya que en cada solicitud posterior se pasa el token de la solicitud de validación del lado servidor. Este token no es *cifrados*; en su lugar es *codificado*. En el servidor, se puede descodificar el token para tener acceso a la información sin procesar en el token. Para enviar el token en solicitudes posteriores, o bien almacenarla en el almacenamiento local del explorador o en una cookie. No se preocupe acerca de una vulnerabilidad XSRF si el token se almacena en el almacenamiento local, pero es un problema si el token está almacenado en una cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Varias aplicaciones se hospedan en un dominio

Aunque `example1.cloudapp.net` y `example2.cloudapp.net` son diferentes de los hosts, hay una relación de confianza implícita entre los hosts bajo la `*.cloudapp.net` dominio. Esta relación de confianza implícita permite a los hosts no sea de confianza influir en sus respectivas cookies (las directivas de mismo origen que rigen las solicitudes AJAX necesariamente no se aplican a las cookies HTTP). El tiempo de ejecución de ASP.NET Core proporciona algunos mitigación en que el nombre de usuario se incrusta en el token de campo. Incluso si hay un subdominio malintencionado puede sobrescribir un token de sesión, que no puede generar un token de campo válido para el usuario. Cuando se hospeda en un entorno de este tipo, las rutinas de anti-XSRF integrado aún no defienden contra secuestro de sesión o inicio de sesión CSRF ataques. Entornos de hospedaje compartidos son vulnerable a secuestro de sesión, inicio de sesión CSRF y otros ataques.

### <a name="additional-resources"></a>Recursos adicionales

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) en [Abrir proyecto de seguridad de aplicación Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
