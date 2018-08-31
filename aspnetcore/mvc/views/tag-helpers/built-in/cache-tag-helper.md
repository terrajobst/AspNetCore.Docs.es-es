---
title: Aplicación auxiliar de etiquetas de caché en ASP.NET Core MVC
author: pkellner
description: Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751661"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="62fab-103">Aplicación auxiliar de etiquetas de caché en ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="62fab-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="62fab-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="62fab-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="62fab-105">La aplicación auxiliar de etiqueta de caché proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core al permitir almacenar en memoria caché su contenido en el proveedor de caché interno de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62fab-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="62fab-106">El motor de visualización Razor establece el valor predeterminado `expires-after` en veinte minutos.</span><span class="sxs-lookup"><span data-stu-id="62fab-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="62fab-107">El siguiente marcado de Razor almacena en caché la fecha y hora:</span><span class="sxs-lookup"><span data-stu-id="62fab-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="62fab-108">La primera solicitud a la página que contiene `CacheTagHelper` mostrará la fecha y hora actuales.</span><span class="sxs-lookup"><span data-stu-id="62fab-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="62fab-109">Las solicitudes adicionales mostrarán el valor almacenado en caché hasta que la memoria caché expira (el valor predeterminado es 20 minutos) o se expulsa por la presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="62fab-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="62fab-110">Puede establecer la duración de la caché con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="62fab-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="62fab-111">Atributos de la aplicación auxiliar de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="62fab-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="62fab-112">enabled</span><span class="sxs-lookup"><span data-stu-id="62fab-112">enabled</span></span>    


| <span data-ttu-id="62fab-113">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-113">Attribute Type</span></span>    | <span data-ttu-id="62fab-114">Valores válidos</span><span class="sxs-lookup"><span data-stu-id="62fab-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="62fab-115">booleano</span><span class="sxs-lookup"><span data-stu-id="62fab-115">boolean</span></span>           | <span data-ttu-id="62fab-116">"true" (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="62fab-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="62fab-117">"false"</span><span class="sxs-lookup"><span data-stu-id="62fab-117">"false"</span></span>   |


<span data-ttu-id="62fab-118">Determina si el contenido incluido en la aplicación auxiliar de etiqueta de caché se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="62fab-119">De manera predeterminada, es `true`.</span><span class="sxs-lookup"><span data-stu-id="62fab-119">The default is `true`.</span></span>  <span data-ttu-id="62fab-120">Si se establece en `false`, la aplicación auxiliar de etiqueta de caché no tiene ningún efecto de almacenamiento en caché en la salida representada.</span><span class="sxs-lookup"><span data-stu-id="62fab-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="62fab-121">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="62fab-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="62fab-122">expires-on</span></span> 

| <span data-ttu-id="62fab-123">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-123">Attribute Type</span></span> |           <span data-ttu-id="62fab-124">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="62fab-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="62fab-125">DateTimeOffset</span></span> | <span data-ttu-id="62fab-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="62fab-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="62fab-127">Establece una fecha de expiración absoluta.</span><span class="sxs-lookup"><span data-stu-id="62fab-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="62fab-128">En el ejemplo siguiente, se almacenará en memoria caché el contenido de la aplicación auxiliar de etiqueta de caché hasta las 17:02 del 29 de enero de 2025.</span><span class="sxs-lookup"><span data-stu-id="62fab-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="62fab-129">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="62fab-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="62fab-130">expires-after</span></span>

| <span data-ttu-id="62fab-131">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-131">Attribute Type</span></span> |        <span data-ttu-id="62fab-132">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="62fab-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="62fab-133">TimeSpan</span></span>    | <span data-ttu-id="62fab-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="62fab-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="62fab-135">Establece el período de tiempo desde la primera solicitud para almacenar en caché el contenido.</span><span class="sxs-lookup"><span data-stu-id="62fab-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="62fab-136">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="62fab-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="62fab-137">expires-sliding</span></span>

| <span data-ttu-id="62fab-138">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-138">Attribute Type</span></span> |        <span data-ttu-id="62fab-139">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="62fab-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="62fab-140">TimeSpan</span></span>    | <span data-ttu-id="62fab-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="62fab-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="62fab-142">Establece el tiempo en que se debe expulsar una entrada de caché si no se ha accedido a ella.</span><span class="sxs-lookup"><span data-stu-id="62fab-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="62fab-143">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="62fab-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="62fab-144">vary-by-header</span></span>

| <span data-ttu-id="62fab-145">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-145">Attribute Type</span></span>    | <span data-ttu-id="62fab-146">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-147">String</span><span class="sxs-lookup"><span data-stu-id="62fab-147">String</span></span>            | <span data-ttu-id="62fab-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="62fab-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="62fab-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="62fab-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="62fab-150">Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambian.</span><span class="sxs-lookup"><span data-stu-id="62fab-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="62fab-151">En el ejemplo siguiente se supervisa el valor del encabezado `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="62fab-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="62fab-152">En el ejemplo se almacenará en memoria caché el contenido de cada `User-Agent` que se muestre al servidor web.</span><span class="sxs-lookup"><span data-stu-id="62fab-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="62fab-153">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="62fab-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="62fab-154">vary-by-query</span></span>

| <span data-ttu-id="62fab-155">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-155">Attribute Type</span></span>    | <span data-ttu-id="62fab-156">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-157">String</span><span class="sxs-lookup"><span data-stu-id="62fab-157">String</span></span>            | <span data-ttu-id="62fab-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="62fab-158">"Make"</span></span>                |
|                   | <span data-ttu-id="62fab-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="62fab-159">"Make,Model"</span></span> |

<span data-ttu-id="62fab-160">Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambia el valor de encabezado.</span><span class="sxs-lookup"><span data-stu-id="62fab-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="62fab-161">En el ejemplo siguiente se examinan los valores de `Make` y `Model`.</span><span class="sxs-lookup"><span data-stu-id="62fab-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="62fab-162">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="62fab-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="62fab-163">vary-by-route</span></span>

| <span data-ttu-id="62fab-164">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-164">Attribute Type</span></span>    | <span data-ttu-id="62fab-165">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-166">String</span><span class="sxs-lookup"><span data-stu-id="62fab-166">String</span></span>            | <span data-ttu-id="62fab-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="62fab-167">"Make"</span></span>                |
|                   | <span data-ttu-id="62fab-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="62fab-168">"Make,Model"</span></span> |

<span data-ttu-id="62fab-169">Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando se produce un cambio en los valores de parámetro de datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="62fab-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="62fab-170">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-170">Example:</span></span>

<span data-ttu-id="62fab-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="62fab-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="62fab-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62fab-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="62fab-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="62fab-173">vary-by-cookie</span></span>

| <span data-ttu-id="62fab-174">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-174">Attribute Type</span></span>    | <span data-ttu-id="62fab-175">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-176">String</span><span class="sxs-lookup"><span data-stu-id="62fab-176">String</span></span>            | <span data-ttu-id="62fab-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="62fab-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="62fab-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="62fab-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="62fab-179">Acepta un valor de encabezado único o una lista separada por comas con los valores de encabezado que desencadenan una actualización de la caché cuando cambian los valores de encabezado.</span><span class="sxs-lookup"><span data-stu-id="62fab-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="62fab-180">En el ejemplo siguiente se examina la cookie asociada con ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="62fab-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="62fab-181">Cuando un usuario se autentica, la cookie de solicitud que se establece desencadena una actualización de la caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="62fab-182">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="62fab-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="62fab-183">vary-by-user</span></span>

| <span data-ttu-id="62fab-184">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-184">Attribute Type</span></span>    | <span data-ttu-id="62fab-185">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-186">Booleano</span><span class="sxs-lookup"><span data-stu-id="62fab-186">Boolean</span></span>             | <span data-ttu-id="62fab-187">"true"</span><span class="sxs-lookup"><span data-stu-id="62fab-187">"true"</span></span>                  |
|                     | <span data-ttu-id="62fab-188">"false" (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="62fab-188">"false" (default)</span></span> |

<span data-ttu-id="62fab-189">Especifica si debe restablecerse la memoria caché cuando el usuario que ha iniciado la sesión (o la entidad de seguridad del contexto) cambia.</span><span class="sxs-lookup"><span data-stu-id="62fab-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="62fab-190">El usuario actual también se conoce como entidad de seguridad del contexto de solicitud y puede verse en una vista Razor mediante una referencia a `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="62fab-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="62fab-191">En el ejemplo siguiente se examina el usuario conectado actualmente.</span><span class="sxs-lookup"><span data-stu-id="62fab-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="62fab-192">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="62fab-193">Con este atributo, se mantiene el contenido en caché a través de un ciclo de inicio y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="62fab-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="62fab-194">Al utilizar `vary-by-user="true"`, una acción de inicio y cierre de sesión invalida la caché para el usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="62fab-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="62fab-195">Se invalida la memoria caché porque se genera un nuevo valor único de cookie al iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="62fab-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="62fab-196">Se mantiene la memoria caché para el estado anónimo cuando no hay ninguna cookie o la cookie ha expirado.</span><span class="sxs-lookup"><span data-stu-id="62fab-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="62fab-197">Esto significa que si ningún usuario ha iniciado sesión, se mantendrá la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="62fab-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="62fab-198">vary-by</span></span>

| <span data-ttu-id="62fab-199">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-199">Attribute Type</span></span> | <span data-ttu-id="62fab-200">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="62fab-201">String</span><span class="sxs-lookup"><span data-stu-id="62fab-201">String</span></span>     |    <span data-ttu-id="62fab-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="62fab-202">"@Model"</span></span>    |

<span data-ttu-id="62fab-203">Permite la personalización de los datos que se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="62fab-204">Cuando el objeto al que hace referencia el valor de cadena del atributo cambia, el contenido de la aplicación auxiliar de etiqueta de caché se actualiza.</span><span class="sxs-lookup"><span data-stu-id="62fab-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="62fab-205">A menudo se asignan a este atributo una concatenación de cadenas de valores del modelo.</span><span class="sxs-lookup"><span data-stu-id="62fab-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="62fab-206">De hecho, eso significa que una actualización de cualquiera de los valores concatenados invalida la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="62fab-207">En el ejemplo siguiente se supone que el método de controlador que representa la vista suma el valor del entero de los dos parámetros de ruta, `myParam1` y `myParam2`, y devuelve el resultado como la propiedad de modelo simple.</span><span class="sxs-lookup"><span data-stu-id="62fab-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="62fab-208">Cuando se cambia esta suma, el contenido de la aplicación auxiliar de etiqueta de caché se representa y almacena en caché de nuevo.</span><span class="sxs-lookup"><span data-stu-id="62fab-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="62fab-209">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-209">Example:</span></span>

<span data-ttu-id="62fab-210">Acción:</span><span class="sxs-lookup"><span data-stu-id="62fab-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="62fab-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62fab-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="62fab-212">priority</span><span class="sxs-lookup"><span data-stu-id="62fab-212">priority</span></span>

| <span data-ttu-id="62fab-213">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="62fab-213">Attribute Type</span></span>    | <span data-ttu-id="62fab-214">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62fab-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62fab-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="62fab-215">CacheItemPriority</span></span>  | <span data-ttu-id="62fab-216">"High"</span><span class="sxs-lookup"><span data-stu-id="62fab-216">"High"</span></span>                   |
|                    | <span data-ttu-id="62fab-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="62fab-217">"Low"</span></span> |
|                    | <span data-ttu-id="62fab-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="62fab-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="62fab-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="62fab-219">"Normal"</span></span> |

<span data-ttu-id="62fab-220">Proporciona instrucciones de expulsión de caché para el proveedor de caché integrado.</span><span class="sxs-lookup"><span data-stu-id="62fab-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="62fab-221">El servidor web expulsará primero las entradas de caché `Low` cuando esté bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="62fab-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="62fab-222">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62fab-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="62fab-223">El atributo `priority` no garantiza un nivel específico de retención de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="62fab-224">`CacheItemPriority` es solo una sugerencia.</span><span class="sxs-lookup"><span data-stu-id="62fab-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="62fab-225">Establecer este atributo en `NeverRemove` no garantiza que siempre se conservará la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="62fab-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="62fab-226">Para más información, consulte [Recursos adicionales](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="62fab-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="62fab-227">La aplicación auxiliar de etiqueta de caché es dependiente del [servicio de caché de memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="62fab-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="62fab-228">La aplicación auxiliar de etiqueta de caché agrega el servicio si no se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="62fab-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62fab-229">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="62fab-229">Additional resources</span></span>

* [<span data-ttu-id="62fab-230">Almacenamiento en caché en memoria</span><span class="sxs-lookup"><span data-stu-id="62fab-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="62fab-231">Introducción a Identity</span><span class="sxs-lookup"><span data-stu-id="62fab-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
