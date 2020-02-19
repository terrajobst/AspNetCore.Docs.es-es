---
title: Globalización y localización de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo poner los componentes de Razor a disposición de los usuarios en varias culturas e idiomas.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453191"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a>Globalización y localización de ASP.NET Core Blazor

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

Los componentes de Razor pueden ponerse a disposición de los usuarios en varias culturas e idiomas. Están disponibles los siguientes escenarios de globalización y localización de .NET:

* . Sistema de recursos de la red
* Formato de fecha y número específico de la referencia cultural

Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:

* `IStringLocalizer<>` *se admite* en las aplicaciones de Blazor.
* la localización de `IHtmlLocalizer<>`, `IViewLocalizer<>`y las anotaciones de datos es ASP.NET Core escenarios MVC y **no se admiten** en Blazor aplicaciones.

Para más información, consulte <xref:fundamentals/localization>.

## <a name="globalization"></a>Globalización

la funcionalidad de `@bind` de Blazorrealiza formatos y analiza valores para su presentación en función de la referencia cultural actual del usuario.

Se puede tener acceso a la referencia cultural actual desde la propiedad <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo (`<input type="{TYPE}" />`):

* `date`
* `number`

Los tipos de campo anteriores:

* Se muestran con sus reglas de formato basadas en explorador adecuadas.
* No puede contener texto de forma libre.
* Proporcionar características de interacción con el usuario en función de la implementación del explorador.

Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten actualmente en Blazor porque no son compatibles con todos los exploradores principales:

* `datetime-local`
* `month`
* `week`

`@bind` admite el parámetro `@bind:culture` para proporcionar un <xref:System.Globalization.CultureInfo?displayProperty=fullName> para analizar y dar formato a un valor. No se recomienda especificar una referencia cultural al usar los tipos de campo `date` y `number`. `date` y `number` tienen compatibilidad integrada con Blazor que proporciona la referencia cultural necesaria.

## <a name="localization"></a>Localización

las aplicaciones de Blazor Server se localizan mediante [middleware de localización](xref:fundamentals/localization#localization-middleware). El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.

La referencia cultural se puede establecer utilizando uno de los métodos siguientes:

* [Cookies](#cookies)
* [Proporcionar la interfaz de usuario para elegir la referencia cultural](#provide-ui-to-choose-the-culture)

Para más información y ejemplos, consulte <xref:fundamentals/localization>.

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a>Configurar el enlazador para la internacionalización (Blazor webassembly)

De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones WebAssembly de Blazor quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita. Para obtener más información e instrucciones sobre cómo controlar el comportamiento del enlazador, vea <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

### <a name="cookies"></a>Cookies

Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario. La cookie se crea mediante el método `OnGet` de la página host de la aplicación (*pages/host. cshtml. CS*). El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario. 

El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural. Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural. Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.

Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización. Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.

En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización. Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación de Blazor Server:

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

La aplicación controla la localización en la siguiente secuencia de eventos:

1. El explorador envía una solicitud HTTP inicial a la aplicación.
1. El middleware de localización asigna la referencia cultural.
1. El método `OnGet` en *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.
1. El explorador abre una conexión WebSocket para crear una sesión interactiva de Blazor Server.
1. El middleware de localización lee la cookie y asigna la referencia cultural.
1. La sesión del servidor de Blazor comienza con la referencia cultural correcta.

### <a name="provide-ui-to-choose-the-culture"></a>Proporcionar la interfaz de usuario para elegir la referencia cultural

Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* . El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso seguro&mdash;se redirige al usuario a una página de inicio de sesión y, a continuación, se redirige de nuevo al recurso original. 

La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador. El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.

Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> Use el resultado de la acción `LocalRedirect` para evitar ataques de redireccionamiento abierto. Para más información, consulte <xref:security/preventing-open-redirects>.

El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/localization>
