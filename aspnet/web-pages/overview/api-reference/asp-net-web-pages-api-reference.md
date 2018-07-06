---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Referencia rápida de la API de (Razor) de ASP.NET Web Pages | Microsoft Docs
author: tfitzmac
description: Esta página contiene una lista con ejemplos breves de los objetos más utilizados, propiedades y métodos de programación ASP.NET Web Pages con sintaxis Razor.
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5fb5b8d29fce25fb36704a58b673f5f3bd582873
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836773"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="7be7e-103">Referencia rápida de la API de (Razor) de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="7be7e-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="7be7e-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7be7e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7be7e-105">Esta página contiene una lista con ejemplos breves de los objetos más utilizados, propiedades y métodos de programación ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="7be7e-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="7be7e-106">Descripciones de marcado con "(v2)" se introdujeron en ASP.NET Web Pages versión 2.</span><span class="sxs-lookup"><span data-stu-id="7be7e-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="7be7e-107">Para obtener documentación de referencia de API, consulte el [documentación de referencia de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=208659) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="7be7e-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="7be7e-108">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="7be7e-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="7be7e-109">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7be7e-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7be7e-110">En este tutorial también funciona con ASP.NET Web Pages 2 y las páginas Web de ASP.NET 1.0 (excepto las características marcadas v2).</span><span class="sxs-lookup"><span data-stu-id="7be7e-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="7be7e-111">Esta página contiene información de referencia para lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7be7e-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="7be7e-112">Clases</span><span class="sxs-lookup"><span data-stu-id="7be7e-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="7be7e-113">Data</span><span class="sxs-lookup"><span data-stu-id="7be7e-113">Data</span></span>](#Data)
- [<span data-ttu-id="7be7e-114">Aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="7be7e-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="7be7e-115">Validación</span><span class="sxs-lookup"><span data-stu-id="7be7e-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="7be7e-116">Clases</span><span class="sxs-lookup"><span data-stu-id="7be7e-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="7be7e-117">Contiene los datos que puedan compartirse mediante cualquiera de las páginas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="7be7e-118">Puede usar dinámico `App` propiedad para tener acceso a los mismos datos, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7be7e-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="7be7e-119">Convierte un valor de cadena en un valor booleano (true/false).</span><span class="sxs-lookup"><span data-stu-id="7be7e-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="7be7e-120">Devuelve "false" o el valor especificado si la cadena no representa true o false.</span><span class="sxs-lookup"><span data-stu-id="7be7e-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="7be7e-121">Convierte un valor de cadena en fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="7be7e-121">Converts a string value to date/time.</span></span> <span data-ttu-id="7be7e-122">Devuelve `DateTime.MinValue` o el valor especificado si la cadena no representa una fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="7be7e-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="7be7e-123">Convierte un valor de cadena en un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="7be7e-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="7be7e-124">Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="7be7e-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="7be7e-125">Convierte un valor de cadena en un valor float.</span><span class="sxs-lookup"><span data-stu-id="7be7e-125">Converts a string value to a float.</span></span> <span data-ttu-id="7be7e-126">Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="7be7e-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="7be7e-127">Convierte un valor de cadena en un entero.</span><span class="sxs-lookup"><span data-stu-id="7be7e-127">Converts a string value to an integer.</span></span> <span data-ttu-id="7be7e-128">Devuelve 0 o el valor especificado si la cadena no representa un entero.</span><span class="sxs-lookup"><span data-stu-id="7be7e-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="7be7e-129">Crea una dirección URL de compatible con el explorador desde una ruta de acceso de archivo local, con los elementos de la ruta de acceso adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="7be7e-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="7be7e-130">Representa *valor* como marca HTML en lugar de procesarlo como codificada en HTML de salida.</span><span class="sxs-lookup"><span data-stu-id="7be7e-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="7be7e-131">Devuelve true si se puede convertir el valor de una cadena al tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="7be7e-132">Devuelve true si el objeto o variable no tiene ningún valor.</span><span class="sxs-lookup"><span data-stu-id="7be7e-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="7be7e-133">Devuelve true si la solicitud es una entrada de blog.</span><span class="sxs-lookup"><span data-stu-id="7be7e-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="7be7e-134">(Las solicitudes iniciales suelen ser una operación GET.)</span><span class="sxs-lookup"><span data-stu-id="7be7e-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="7be7e-135">Especifica la ruta de acceso de una página de diseño que se aplican a esta página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="7be7e-136">Contiene datos compartidos entre la página, las páginas de diseño y las páginas parciales en la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="7be7e-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="7be7e-137">Puede usar dinámico `Page` propiedad para tener acceso a los mismos datos, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7be7e-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="7be7e-138">(Las páginas de diseño) Representa el contenido de una página de contenido que no está en cualquier sección con nombre.</span><span class="sxs-lookup"><span data-stu-id="7be7e-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="7be7e-139">Representa una página de contenido mediante la ruta de acceso especificada y los datos adicionales opcionales.</span><span class="sxs-lookup"><span data-stu-id="7be7e-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="7be7e-140">Puede obtener los valores de los parámetros adicionales de `PageData` por posición (ejemplo 1) o de clave (ejemplo 2).</span><span class="sxs-lookup"><span data-stu-id="7be7e-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="7be7e-141">(Las páginas de diseño) Representa una sección de contenido que tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="7be7e-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="7be7e-142">Establecer *requiere* False para que una sección opcional.</span><span class="sxs-lookup"><span data-stu-id="7be7e-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="7be7e-143">Obtiene o establece el valor de una cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="7be7e-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="7be7e-144">Obtiene los archivos que se han cargado en la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="7be7e-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="7be7e-145">Obtiene los datos que se ha registrado en un formulario (como cadenas).</span><span class="sxs-lookup"><span data-stu-id="7be7e-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="7be7e-146">`Request[key]` comprueba tanto el `Request.Form` y `Request.QueryString` colecciones.</span><span class="sxs-lookup"><span data-stu-id="7be7e-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="7be7e-147">Obtiene los datos que se especificó en la cadena de consulta de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7be7e-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="7be7e-148">`Request[key]` comprueba tanto el `Request.Form` y `Request.QueryString` colecciones.</span><span class="sxs-lookup"><span data-stu-id="7be7e-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="7be7e-149">Selectivamente deshabilita la validación de solicitudes para un elemento de formulario, el valor de cadena de consulta, cookie o valor del encabezado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="7be7e-150">Validación de solicitudes está habilitada de forma predeterminada y evita que los usuarios de registro marcado u otro contenido potencialmente peligroso.</span><span class="sxs-lookup"><span data-stu-id="7be7e-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="7be7e-151">Agrega un encabezado de servidor HTTP a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7be7e-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="7be7e-152">Almacena en caché el resultado de la página durante un tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="7be7e-153">Opcionalmente, establecer *deslizante* para restablecer el tiempo de espera en cada acceso a la página y *varyByParams* para almacenar en caché diferentes versiones de la página para cada cadena de consulta diferentes en la solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="7be7e-154">Redirige la solicitud del explorador a una nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="7be7e-155">Establece el código de estado HTTP enviado al explorador.</span><span class="sxs-lookup"><span data-stu-id="7be7e-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="7be7e-156">Escribe el contenido de *datos* a la respuesta con un tipo MIME opcional.</span><span class="sxs-lookup"><span data-stu-id="7be7e-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="7be7e-157">Escribe el contenido de un archivo en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7be7e-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="7be7e-158">(Las páginas de diseño) Define una sección de contenido que tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="7be7e-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="7be7e-159">Descodifica una cadena que está codificado en HTML.</span><span class="sxs-lookup"><span data-stu-id="7be7e-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="7be7e-160">Codifica una cadena para la representación en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="7be7e-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="7be7e-161">Devuelve la ruta de acceso física de servidor para la ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="7be7e-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="7be7e-162">Descodifica el texto desde una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7be7e-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="7be7e-163">Codifica el texto que se va a colocar en una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7be7e-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="7be7e-164">Obtiene o establece un valor que exista hasta que el usuario cierra el explorador.</span><span class="sxs-lookup"><span data-stu-id="7be7e-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="7be7e-165">Muestra una representación de cadena del valor de objeto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="7be7e-166">Obtiene los datos adicionales desde la dirección URL (por ejemplo, */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="7be7e-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="7be7e-167">Cambia la contraseña para el usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="7be7e-168">Confirma una cuenta mediante el token de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="7be7e-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="7be7e-169">Crea una nueva cuenta de usuario con el nombre de usuario especificado y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="7be7e-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="7be7e-170">Para requerir un token de confirmación, pasa true para *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="7be7e-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="7be7e-171">Obtiene el identificador entero para el usuario ha iniciado sesión actualmente.</span><span class="sxs-lookup"><span data-stu-id="7be7e-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="7be7e-172">Obtiene el nombre para el usuario ha iniciado sesión actualmente.</span><span class="sxs-lookup"><span data-stu-id="7be7e-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="7be7e-173">Genera un token de restablecimiento de contraseña que se puede enviar por correo electrónico a un usuario para que el usuario puede restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="7be7e-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="7be7e-174">Devuelve el identificador de usuario del nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="7be7e-175">Devuelve true si el usuario actual ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="7be7e-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="7be7e-176">Devuelve true si se ha confirmado el usuario (por ejemplo, mediante un correo electrónico de confirmación).</span><span class="sxs-lookup"><span data-stu-id="7be7e-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="7be7e-177">Devuelve true si el nombre del usuario actual coincide con el nombre de usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="7be7e-178">Registra el usuario estableciendo un token de autenticación en la cookie.</span><span class="sxs-lookup"><span data-stu-id="7be7e-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="7be7e-179">Registra al usuario cerrar mediante la eliminación de la cookie de token de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="7be7e-180">Si el usuario no está autenticado, Establece el estado de HTTP en 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="7be7e-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="7be7e-181">Si el usuario actual no es un miembro de uno de los roles especificados, Establece el estado de HTTP en 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="7be7e-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="7be7e-182">Si el usuario actual no es el usuario especificado por *username*, Establece el estado de HTTP en 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="7be7e-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="7be7e-183">Si el token de restablecimiento de contraseña es válido, cambia la contraseña del usuario a la nueva contraseña.</span><span class="sxs-lookup"><span data-stu-id="7be7e-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="7be7e-184">Datos</span><span class="sxs-lookup"><span data-stu-id="7be7e-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="7be7e-185">Ejecuta *SQLstatement* (con parámetros opcionales), como INSERT, DELETE o UPDATE y devuelve un recuento de registros afectados.</span><span class="sxs-lookup"><span data-stu-id="7be7e-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="7be7e-186">Devuelve la columna de identidad de la fila insertada más recientemente.</span><span class="sxs-lookup"><span data-stu-id="7be7e-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="7be7e-187">Se abre el archivo de base de datos especificada o la base de datos especificada mediante una cadena de conexión con nombre desde el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="7be7e-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="7be7e-188">Se abre una base de datos mediante la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="7be7e-188">Opens a database using the connection string.</span></span> <span data-ttu-id="7be7e-189">(Esto contrasta con `Database.Open`, que utiliza un nombre de la cadena de conexión.)</span><span class="sxs-lookup"><span data-stu-id="7be7e-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="7be7e-190">Consultas de la base de datos con *SQLstatement* (opcionalmente pasa parámetros) y devuelve los resultados como una colección.</span><span class="sxs-lookup"><span data-stu-id="7be7e-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="7be7e-191">Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un único registro.</span><span class="sxs-lookup"><span data-stu-id="7be7e-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="7be7e-192">Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un valor único.</span><span class="sxs-lookup"><span data-stu-id="7be7e-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="7be7e-193">Aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="7be7e-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="7be7e-194">Representa el código JavaScript de Google Analytics para el identificador especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="7be7e-195">Representa el código de JavaScript para Analytics StatCounter para el proyecto especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="7be7e-196">Representa el código de JavaScript de Yahoo Analytics para la cuenta especificada.</span><span class="sxs-lookup"><span data-stu-id="7be7e-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="7be7e-197">Pasa una búsqueda de Bing.</span><span class="sxs-lookup"><span data-stu-id="7be7e-197">Passes a search to Bing.</span></span> <span data-ttu-id="7be7e-198">Para especificar el sitio de búsqueda y un título para el cuadro de búsqueda, puede establecer el `Bing.SiteUrl` y `Bing.SiteTitle` propiedades.</span><span class="sxs-lookup"><span data-stu-id="7be7e-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="7be7e-199">Normalmente, debe establecer estas propiedades el  *\_AppStart* página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="7be7e-200">Inicializa un gráfico.</span><span class="sxs-lookup"><span data-stu-id="7be7e-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="7be7e-201">Agrega una leyenda a un gráfico.</span><span class="sxs-lookup"><span data-stu-id="7be7e-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="7be7e-202">Agrega una serie de valores al gráfico.</span><span class="sxs-lookup"><span data-stu-id="7be7e-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="7be7e-203">Devuelve un valor hash de los datos especificados.</span><span class="sxs-lookup"><span data-stu-id="7be7e-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="7be7e-204">El algoritmo predeterminado es `sha256`.</span><span class="sxs-lookup"><span data-stu-id="7be7e-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="7be7e-205">Permite a los usuarios de Facebook realizar una conexión a las páginas.</span><span class="sxs-lookup"><span data-stu-id="7be7e-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="7be7e-206">Representa la interfaz de usuario para cargar archivos.</span><span class="sxs-lookup"><span data-stu-id="7be7e-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="7be7e-207">Representa la etiqueta especificada de jugador de Xbox.</span><span class="sxs-lookup"><span data-stu-id="7be7e-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="7be7e-208">Presenta la imagen Gravatar para la dirección de correo electrónico especificada.</span><span class="sxs-lookup"><span data-stu-id="7be7e-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="7be7e-209">Convierte un objeto de datos en una cadena con el formato JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="7be7e-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="7be7e-210">Convierte una cadena de entrada codificada en JSON a un objeto de datos que puede recorrer en iteración o insertar en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7be7e-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="7be7e-211">Representa los vínculos de redes sociales con el título especificado y la dirección URL opcional.</span><span class="sxs-lookup"><span data-stu-id="7be7e-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="7be7e-212">Asocia un mensaje de error con un campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-212">Associates an error message with a form field.</span></span> <span data-ttu-id="7be7e-213">Use el `ModelState` auxiliar para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="7be7e-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="7be7e-214">Asocia un mensaje de error a un formulario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-214">Associates an error message with a form.</span></span> <span data-ttu-id="7be7e-215">Use el `ModelState` auxiliar para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="7be7e-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="7be7e-216">Devuelve true si no hay ningún error de validación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="7be7e-217">Use el `ModelState` auxiliar para tener acceso a este miembro.</span><span class="sxs-lookup"><span data-stu-id="7be7e-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="7be7e-218">Representa las propiedades y valores de un objeto y todos los objetos.</span><span class="sxs-lookup"><span data-stu-id="7be7e-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="7be7e-219">Representa la prueba de comprobación de reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="7be7e-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="7be7e-220">Establece las claves públicas y privadas para el servicio de reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="7be7e-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="7be7e-221">Normalmente, debe establecer estas propiedades el  *\_AppStart* página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="7be7e-222">Devuelve el resultado de la prueba de reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="7be7e-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="7be7e-223">Representa información de estado sobre ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7be7e-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="7be7e-224">Representa un flujo de Twitter para el usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="7be7e-225">Representa un flujo de Twitter para el texto de búsqueda especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="7be7e-226">Representa un Reproductor de vídeo Flash para el archivo especificado con opcional ancho y alto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="7be7e-227">Representa un Reproductor de Windows Media para el archivo especificado con opcional ancho y alto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="7be7e-228">Representa un reproductor Silverlight para el elemento especificado *.xap* archivo con el ancho y alto necesarios.</span><span class="sxs-lookup"><span data-stu-id="7be7e-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="7be7e-229">Devuelve el objeto especificado por *clave*, o null si no se encuentra el objeto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="7be7e-230">Quita el objeto especificado por *clave* desde la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="7be7e-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="7be7e-231">Coloca *valor* en la memoria caché en el nombre especificado por *clave*.</span><span class="sxs-lookup"><span data-stu-id="7be7e-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="7be7e-232">Crea un nuevo `WebGrid` utilizando los datos de una consulta de objeto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="7be7e-233">Presenta el marcado para mostrar datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="7be7e-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="7be7e-234">Representa un elemento de paginación la `WebGrid` objeto.</span><span class="sxs-lookup"><span data-stu-id="7be7e-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="7be7e-235">Carga una imagen de la ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7be7e-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="7be7e-236">Agrega la imagen especificada como una marca de agua.</span><span class="sxs-lookup"><span data-stu-id="7be7e-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="7be7e-237">Agrega el texto especificado a la imagen.</span><span class="sxs-lookup"><span data-stu-id="7be7e-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="7be7e-238">Voltea la imagen horizontal o verticalmente.</span><span class="sxs-lookup"><span data-stu-id="7be7e-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="7be7e-239">Carga una imagen cuando se publica una imagen a una página durante una carga de archivos.</span><span class="sxs-lookup"><span data-stu-id="7be7e-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="7be7e-240">Cambia automáticamente de tamaño una la imagen.</span><span class="sxs-lookup"><span data-stu-id="7be7e-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="7be7e-241">Gira la imagen a la izquierda o a la derecha.</span><span class="sxs-lookup"><span data-stu-id="7be7e-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="7be7e-242">Guarda la imagen en la ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7be7e-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="7be7e-243">Establece la contraseña para el servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="7be7e-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="7be7e-244">Normalmente, debe establecer esta propiedad el  *\_AppStart* página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="7be7e-245">Envía un mensaje de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="7be7e-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="7be7e-246">Establece el nombre del servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="7be7e-246">Sets the SMTP server name.</span></span> <span data-ttu-id="7be7e-247">Normalmente, debe establecer esta propiedad el<em>\_AppStart</em> página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-247">Normally you set this property in the<em>\_AppStart</em> page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="7be7e-248">Establece el nombre de usuario para el servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="7be7e-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="7be7e-249">Normalmente debería establecer esta propiedad el  *\_AppStart* página.</span><span class="sxs-lookup"><span data-stu-id="7be7e-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="7be7e-250">Validación</span><span class="sxs-lookup"><span data-stu-id="7be7e-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="7be7e-251">(v2) Representa un mensaje de error de validación para el campo especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="7be7e-252">(v2) Muestra una lista de todos los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="7be7e-253">(v2) Registra un elemento de entrada de usuario para el tipo de validación especificado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="7be7e-254">(v2) Dinámicamente representa los atributos de clase CSS para la validación del lado cliente de forma que puede dar formato a los mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="7be7e-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="7be7e-255">(Requiere que haga referencia a las bibliotecas de scripts de cliente adecuado y que define las clases CSS).</span><span class="sxs-lookup"><span data-stu-id="7be7e-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="7be7e-256">(v2) Habilita la validación del lado cliente para el campo de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="7be7e-257">(Requiere que haga referencia a las bibliotecas de scripts de cliente adecuado).</span><span class="sxs-lookup"><span data-stu-id="7be7e-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="7be7e-258">(v2) Devuelve true si todos los elementos de entrada de usuario que están registrado para la validación contienen valores válidos.</span><span class="sxs-lookup"><span data-stu-id="7be7e-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="7be7e-259">(v2) Especifica que los usuarios deben proporcionar un valor para el elemento de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="7be7e-260">(v2) Especifica que los usuarios deben proporcionar valores para cada uno de los elementos de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7be7e-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="7be7e-261">Este método no permite especificar un mensaje de error personalizado.</span><span class="sxs-lookup"><span data-stu-id="7be7e-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="7be7e-262">(v2) Especifica una prueba de validación cuando se usa el `Validation.Add` método.</span><span class="sxs-lookup"><span data-stu-id="7be7e-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
