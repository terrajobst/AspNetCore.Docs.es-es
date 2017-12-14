---
title: "Almacenar en caché auxiliar de etiqueta en el núcleo de ASP.NET MVC"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 1710a5781fb69aaa6101270d6b4fd44f92c7f06c
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="8d66f-104">Almacenar en caché auxiliar de etiqueta en el núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8d66f-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="8d66f-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="8d66f-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="8d66f-106">La aplicación auxiliar de etiqueta de caché proporciona la capacidad para mejorar drásticamente el rendimiento de la aplicación de ASP.NET Core almacenando en memoria caché su contenido para el proveedor de caché de ASP.NET Core interno.</span><span class="sxs-lookup"><span data-stu-id="8d66f-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="8d66f-107">El motor de vista Razor establece el valor predeterminado `expires-after` veinte minutos.</span><span class="sxs-lookup"><span data-stu-id="8d66f-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="8d66f-108">El siguiente marcado de Razor almacena en caché la fecha y hora:</span><span class="sxs-lookup"><span data-stu-id="8d66f-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="8d66f-109">La primera solicitud a la página que contiene `CacheTagHelper` mostrará la fecha y hora actuales.</span><span class="sxs-lookup"><span data-stu-id="8d66f-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="8d66f-110">Las solicitudes adicionales mostrarán el valor almacenado en caché hasta que la memoria caché expira (el valor predeterminado es 20 minutos) o se expulsa a la presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="8d66f-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="8d66f-111">Puede establecer la duración de la caché con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="8d66f-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="8d66f-112">Atributos de la aplicación auxiliar de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="8d66f-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="8d66f-113">enabled</span><span class="sxs-lookup"><span data-stu-id="8d66f-113">enabled</span></span>    


| <span data-ttu-id="8d66f-114">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-114">Attribute Type</span></span>    | <span data-ttu-id="8d66f-115">Valores válidos</span><span class="sxs-lookup"><span data-stu-id="8d66f-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="8d66f-116">booleano</span><span class="sxs-lookup"><span data-stu-id="8d66f-116">boolean</span></span>           | <span data-ttu-id="8d66f-117">"true" (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="8d66f-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="8d66f-118">"false"</span><span class="sxs-lookup"><span data-stu-id="8d66f-118">"false"</span></span>   |


<span data-ttu-id="8d66f-119">Determina si el contenido incluido en la aplicación auxiliar de etiqueta de caché se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="8d66f-120">De manera predeterminada, es `true`.</span><span class="sxs-lookup"><span data-stu-id="8d66f-120">The default is `true`.</span></span>  <span data-ttu-id="8d66f-121">Si establece en `false` esta aplicación auxiliar de etiqueta de caché no tiene ningún efecto de almacenamiento en caché en la salida representada.</span><span class="sxs-lookup"><span data-stu-id="8d66f-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="8d66f-122">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="8d66f-123">caduca en</span><span class="sxs-lookup"><span data-stu-id="8d66f-123">expires-on</span></span> 

| <span data-ttu-id="8d66f-124">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-124">Attribute Type</span></span>    | <span data-ttu-id="8d66f-125">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8d66f-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8d66f-126">DateTimeOffset</span></span>    | <span data-ttu-id="8d66f-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="8d66f-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="8d66f-128">Establece una fecha de expiración absoluta.</span><span class="sxs-lookup"><span data-stu-id="8d66f-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="8d66f-129">En el ejemplo siguiente, se almacenará en memoria caché el contenido de la aplicación auxiliar de etiqueta de caché hasta 5:02 P.M. el 29 de enero de 2025.</span><span class="sxs-lookup"><span data-stu-id="8d66f-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="8d66f-130">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="8d66f-131">expira después</span><span class="sxs-lookup"><span data-stu-id="8d66f-131">expires-after</span></span>

| <span data-ttu-id="8d66f-132">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-132">Attribute Type</span></span>    | <span data-ttu-id="8d66f-133">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8d66f-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8d66f-134">TimeSpan</span></span>    | <span data-ttu-id="8d66f-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="8d66f-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="8d66f-136">Establece el período de tiempo desde la primera vez de solicitud para almacenar en caché el contenido.</span><span class="sxs-lookup"><span data-stu-id="8d66f-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="8d66f-137">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-137">Example:</span></span>

```cshtml
<Cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="8d66f-138">deslizando expira</span><span class="sxs-lookup"><span data-stu-id="8d66f-138">expires-sliding</span></span>

| <span data-ttu-id="8d66f-139">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-139">Attribute Type</span></span>    | <span data-ttu-id="8d66f-140">Valor de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="8d66f-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8d66f-141">TimeSpan</span></span>    | <span data-ttu-id="8d66f-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="8d66f-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="8d66f-143">Establece el tiempo que se debe expulsar una entrada de caché si no se ha accedido.</span><span class="sxs-lookup"><span data-stu-id="8d66f-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="8d66f-144">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="8d66f-145">por encabezado Vary</span><span class="sxs-lookup"><span data-stu-id="8d66f-145">vary-by-header</span></span>

| <span data-ttu-id="8d66f-146">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-146">Attribute Type</span></span>    | <span data-ttu-id="8d66f-147">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-148">Cadena</span><span class="sxs-lookup"><span data-stu-id="8d66f-148">String</span></span>            | <span data-ttu-id="8d66f-149">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="8d66f-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="8d66f-150">"User-Agent, content-encoding"</span><span class="sxs-lookup"><span data-stu-id="8d66f-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="8d66f-151">Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambian.</span><span class="sxs-lookup"><span data-stu-id="8d66f-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="8d66f-152">En el ejemplo siguiente se supervisa el valor del encabezado `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="8d66f-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="8d66f-153">En el ejemplo se almacenará en memoria caché el contenido de cada diferentes `User-Agent` muestre en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="8d66f-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="8d66f-154">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="8d66f-155">variar por consulta</span><span class="sxs-lookup"><span data-stu-id="8d66f-155">vary-by-query</span></span>

| <span data-ttu-id="8d66f-156">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-156">Attribute Type</span></span>    | <span data-ttu-id="8d66f-157">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-158">Cadena</span><span class="sxs-lookup"><span data-stu-id="8d66f-158">String</span></span>            | <span data-ttu-id="8d66f-159">"Hacer"</span><span class="sxs-lookup"><span data-stu-id="8d66f-159">"Make"</span></span>                |
|                   | <span data-ttu-id="8d66f-160">"Asegúrese, modelo"</span><span class="sxs-lookup"><span data-stu-id="8d66f-160">"Make,Model"</span></span> |

<span data-ttu-id="8d66f-161">Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambia el valor de encabezado.</span><span class="sxs-lookup"><span data-stu-id="8d66f-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="8d66f-162">En el ejemplo siguiente se busca en los valores de `Make` y `Model`.</span><span class="sxs-lookup"><span data-stu-id="8d66f-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="8d66f-163">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="8d66f-164">variar ruta</span><span class="sxs-lookup"><span data-stu-id="8d66f-164">vary-by-route</span></span>

| <span data-ttu-id="8d66f-165">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-165">Attribute Type</span></span>    | <span data-ttu-id="8d66f-166">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-167">Cadena</span><span class="sxs-lookup"><span data-stu-id="8d66f-167">String</span></span>            | <span data-ttu-id="8d66f-168">"Hacer"</span><span class="sxs-lookup"><span data-stu-id="8d66f-168">"Make"</span></span>                |
|                   | <span data-ttu-id="8d66f-169">"Asegúrese, modelo"</span><span class="sxs-lookup"><span data-stu-id="8d66f-169">"Make,Model"</span></span> |

<span data-ttu-id="8d66f-170">Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando cambio de valores de parámetro de ruta de datos.</span><span class="sxs-lookup"><span data-stu-id="8d66f-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="8d66f-171">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-171">Example:</span></span>

<span data-ttu-id="8d66f-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="8d66f-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="8d66f-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d66f-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="8d66f-174">variar cookie</span><span class="sxs-lookup"><span data-stu-id="8d66f-174">vary-by-cookie</span></span>

| <span data-ttu-id="8d66f-175">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-175">Attribute Type</span></span>    | <span data-ttu-id="8d66f-176">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-177">Cadena</span><span class="sxs-lookup"><span data-stu-id="8d66f-177">String</span></span>            | <span data-ttu-id="8d66f-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="8d66f-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="8d66f-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="8d66f-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="8d66f-180">Acepta un valor de encabezado único o una lista separada por comas de los valores de encabezado que desencadenan una actualización de la caché cuando los valores de encabezado cambio (s).</span><span class="sxs-lookup"><span data-stu-id="8d66f-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="8d66f-181">En el ejemplo siguiente se examina la cookie asociada con la identidad de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8d66f-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="8d66f-182">Cuando un usuario se autentica la cookie de solicitud para establecer lo que desencadena una actualización de la caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="8d66f-183">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="8d66f-184">variar por usuario</span><span class="sxs-lookup"><span data-stu-id="8d66f-184">vary-by-user</span></span>

| <span data-ttu-id="8d66f-185">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-185">Attribute Type</span></span>    | <span data-ttu-id="8d66f-186">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-187">Booleano</span><span class="sxs-lookup"><span data-stu-id="8d66f-187">Boolean</span></span>             | <span data-ttu-id="8d66f-188">"true"</span><span class="sxs-lookup"><span data-stu-id="8d66f-188">"true"</span></span>                  |
|                     | <span data-ttu-id="8d66f-189">"false" (predeterminado)</span><span class="sxs-lookup"><span data-stu-id="8d66f-189">"false" (default)</span></span> |

<span data-ttu-id="8d66f-190">Especifica si debe restablecer la memoria caché cuando el usuario ha iniciado la sesión (o la entidad de contexto) cambia.</span><span class="sxs-lookup"><span data-stu-id="8d66f-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="8d66f-191">El usuario actual es también conocida como la entidad de contexto de solicitud y puede verse en una vista Razor haciendo referencia a `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="8d66f-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="8d66f-192">En el ejemplo siguiente se examina el usuario conectado actualmente.</span><span class="sxs-lookup"><span data-stu-id="8d66f-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="8d66f-193">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="8d66f-194">Con este atributo, mantiene el contenido en caché a través de un ciclo de inicio de sesión y registro de salida.</span><span class="sxs-lookup"><span data-stu-id="8d66f-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="8d66f-195">Al utilizar `vary-by-user="true"`, una acción de inicio de sesión y registro horizontal invalida la caché para el usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="8d66f-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="8d66f-196">Se invalida la memoria caché porque se genera un nuevo valor de cookie único inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="8d66f-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="8d66f-197">Memoria caché se mantiene para el estado anónimo cuando ninguna cookie está presente o ha expirado.</span><span class="sxs-lookup"><span data-stu-id="8d66f-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="8d66f-198">Esto significa que si ningún usuario ha iniciado sesión, se mantendrá la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="8d66f-199">variar por</span><span class="sxs-lookup"><span data-stu-id="8d66f-199">vary-by</span></span>

| <span data-ttu-id="8d66f-200">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-200">Attribute Type</span></span>    | <span data-ttu-id="8d66f-201">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-202">Cadena</span><span class="sxs-lookup"><span data-stu-id="8d66f-202">String</span></span>             | <span data-ttu-id="8d66f-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="8d66f-203">"@Model"</span></span>                 |


<span data-ttu-id="8d66f-204">Permite la personalización de los datos que se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="8d66f-205">Cuando se actualiza el objeto al que hace referencia cambia de valor de cadena del atributo, el contenido de la aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="8d66f-206">A menudo una concatenación de cadenas de valores del modelo se asignan a este atributo.</span><span class="sxs-lookup"><span data-stu-id="8d66f-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="8d66f-207">De hecho, que significa que una actualización a cualquiera de los valores concatenados invalida la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="8d66f-208">El ejemplo siguiente supone que el método de controlador representar el valor del entero de los dos parámetros de ruta, de las sumas de vista `myParam1` y `myParam2`y que devuelve como la propiedad de modelo simple.</span><span class="sxs-lookup"><span data-stu-id="8d66f-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="8d66f-209">Cuando se cambia esta suma, el contenido de la aplicación auxiliar de etiqueta de caché se representan y almacenado en caché nuevo.</span><span class="sxs-lookup"><span data-stu-id="8d66f-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="8d66f-210">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-210">Example:</span></span>

<span data-ttu-id="8d66f-211">Acción:</span><span class="sxs-lookup"><span data-stu-id="8d66f-211">Action:</span></span>

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

<span data-ttu-id="8d66f-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8d66f-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="8d66f-213">priority</span><span class="sxs-lookup"><span data-stu-id="8d66f-213">priority</span></span>

| <span data-ttu-id="8d66f-214">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="8d66f-214">Attribute Type</span></span>    | <span data-ttu-id="8d66f-215">Valores de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8d66f-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="8d66f-216">Enumeración CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="8d66f-216">CacheItemPriority</span></span>  | <span data-ttu-id="8d66f-217">"Alto"</span><span class="sxs-lookup"><span data-stu-id="8d66f-217">"High"</span></span>                   |
|                    | <span data-ttu-id="8d66f-218">"Bajo"</span><span class="sxs-lookup"><span data-stu-id="8d66f-218">"Low"</span></span> |
|                    | <span data-ttu-id="8d66f-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="8d66f-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="8d66f-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="8d66f-220">"Normal"</span></span> |

<span data-ttu-id="8d66f-221">Proporciona instrucciones de expulsión de caché para el proveedor de caché integrada.</span><span class="sxs-lookup"><span data-stu-id="8d66f-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="8d66f-222">El servidor web expulsará `Low` entradas de caché de primero cuando está bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="8d66f-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="8d66f-223">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8d66f-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="8d66f-224">El `priority` atributos no garantizan un nivel específico de retención de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="8d66f-225">`CacheItemPriority`es sólo una sugerencia.</span><span class="sxs-lookup"><span data-stu-id="8d66f-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="8d66f-226">Establecer este atributo en `NeverRemove` no garantiza que siempre se conservará la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="8d66f-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="8d66f-227">Vea [recursos adicionales](#additional-resources) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8d66f-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="8d66f-228">La aplicación auxiliar de etiqueta de caché es dependiente de la [servicio de caché de memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="8d66f-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="8d66f-229">La aplicación auxiliar de etiqueta de caché agrega el servicio si no se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="8d66f-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d66f-230">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8d66f-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
