---
title: Asistente de etiquetas delimitadoras en ASP.NET Core
author: pkellner
description: Descubra los atributos del asistente de etiquetas delimitadoras de ASP.NET Core y el papel que desempeña cada atributo al ampliar el comportamiento de la etiqueta delimitadora de código HTML.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 13508729c1e3b64a8b0e6965da57880738ab85c3
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325554"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="7919d-103">Asistente de etiquetas delimitadoras en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919d-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="7919d-104">De [Peter Kellner](http://peterkellner.net) y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="7919d-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="7919d-105">El [asistente de etiquetas delimitadoras](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) mejora la etiqueta delimitadora de código HTML estándar (`<a ... ></a>`) agregando nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="7919d-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="7919d-106">Por convención, los nombres de atributo tienen el prefijo `asp-`.</span><span class="sxs-lookup"><span data-stu-id="7919d-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="7919d-107">El valor de atributo `href` del elemento delimitador representado se determina mediante los valores de los atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="7919d-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="7919d-108">Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="7919d-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="7919d-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7919d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7919d-110">En los ejemplos de todo este documento se usa *SpeakerController*:</span><span class="sxs-lookup"><span data-stu-id="7919d-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="7919d-111">A continuación se proporciona un inventario de los atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="7919d-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="7919d-112">asp-controller</span><span class="sxs-lookup"><span data-stu-id="7919d-112">asp-controller</span></span>

<span data-ttu-id="7919d-113">El atributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) asigna el controlador usado para generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7919d-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="7919d-114">En el marcado siguiente se especifican todos los altavoces:</span><span class="sxs-lookup"><span data-stu-id="7919d-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="7919d-115">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="7919d-116">Si el atributo `asp-controller` está especificado y `asp-action` no lo está, el valor `asp-action` predeterminado es la acción del controlador asociada a la vista que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="7919d-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="7919d-117">Si se omite `asp-action` en el marcado anterior y se usa el asistente de etiquetas delimitadoras en la vista *Index* de *HomeController* (*/Home*), el código HTML generado es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="7919d-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="7919d-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="7919d-118">asp-action</span></span>

<span data-ttu-id="7919d-119">El valor del atributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) representa el nombre de la acción del controlador incluido en el atributo `href` generado.</span><span class="sxs-lookup"><span data-stu-id="7919d-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="7919d-120">El siguiente marcado establece el valor del atributo `href` generado en la página "Speaker Evaluations":</span><span class="sxs-lookup"><span data-stu-id="7919d-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="7919d-121">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="7919d-122">Si no hay ningún atributo `asp-controller` especificado, se usará el controlador predeterminado que llama a la vista que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="7919d-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="7919d-123">Si el valor del atributo `asp-action` es `Index`, no se anexa ninguna acción a la dirección URL, lo que da lugar a la invocación de la acción `Index` predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7919d-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="7919d-124">La acción especificada (o su valor predeterminado), debe existir en el controlador al que se hace referencia en `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="7919d-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="7919d-125">asp-route-{valor}</span><span class="sxs-lookup"><span data-stu-id="7919d-125">asp-route-{value}</span></span>

<span data-ttu-id="7919d-126">El atributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite indicar un prefijo de ruta comodín.</span><span class="sxs-lookup"><span data-stu-id="7919d-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="7919d-127">Cualquier valor que ocupe el marcador de posición `{value}` se interpretará como un parámetro de ruta potencial.</span><span class="sxs-lookup"><span data-stu-id="7919d-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="7919d-128">Si no se encuentra ninguna ruta predeterminada, este prefijo de ruta se anexará al atributo `href` generado como valor y parámetro de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7919d-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="7919d-129">En caso contrario, se sustituirá en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="7919d-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="7919d-130">Observe la siguiente acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="7919d-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="7919d-131">Con una plantilla de ruta predeterminada definida en *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="7919d-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="7919d-132">La vista de MVC usa el modelo, proporcionado por la acción, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="7919d-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="7919d-133">Se hace coincidir el marcador de posición `{id?}` de la ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7919d-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="7919d-134">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="7919d-135">Supongamos que el prefijo de ruta no forma parte de la plantilla de enrutamiento coincidente, al igual que con la siguiente vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="7919d-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="7919d-136">Se genera el siguiente código HTML porque no se encontró `speakerid` en la ruta coincidente:</span><span class="sxs-lookup"><span data-stu-id="7919d-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="7919d-137">Si no se especifica `asp-controller` o `asp-action`, se sigue el mismo proceso predeterminado que en el atributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="7919d-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="7919d-138">asp-route</span><span class="sxs-lookup"><span data-stu-id="7919d-138">asp-route</span></span>

<span data-ttu-id="7919d-139">El atributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) se usa para crear una dirección URL que se vincula directamente con una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="7919d-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="7919d-140">Mediante [atributos de enrutamiento](xref:mvc/controllers/routing#attribute-routing) puede asignarse un nombre a una ruta tal y como se muestra en `SpeakerController` y usarse en su acción `Evaluations`:</span><span class="sxs-lookup"><span data-stu-id="7919d-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="7919d-141">En el siguiente marcado, el atributo `asp-route` hace referencia a la ruta con nombre:</span><span class="sxs-lookup"><span data-stu-id="7919d-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="7919d-142">El asistente de etiquetas delimitadoras genera una ruta directamente a esa acción de controlador mediante la dirección URL */Speaker/Evaluations*.</span><span class="sxs-lookup"><span data-stu-id="7919d-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="7919d-143">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="7919d-144">Si además de `asp-route` se especifica `asp-controller` o `asp-action`, la ruta generada puede no ser la esperada.</span><span class="sxs-lookup"><span data-stu-id="7919d-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="7919d-145">Para evitar un conflicto de ruta, no se debe usar `asp-route` con los atributos `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="7919d-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="7919d-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="7919d-146">asp-all-route-data</span></span>

<span data-ttu-id="7919d-147">El atributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite crear un diccionario de pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="7919d-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="7919d-148">La clave es el nombre del parámetro, mientras que el valor es el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="7919d-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="7919d-149">En el ejemplo siguiente se inicializa un diccionario y se pasa a una vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="7919d-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="7919d-150">Los datos también se podrían pasar con el modelo.</span><span class="sxs-lookup"><span data-stu-id="7919d-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="7919d-151">El código anterior genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="7919d-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="7919d-152">Se acopla el diccionario `asp-all-route-data` para generar una cadena de consulta que cumpla los requisitos de la acción `Evaluations` sobrecargada:</span><span class="sxs-lookup"><span data-stu-id="7919d-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="7919d-153">Si alguna de las claves del diccionario coincide con los parámetros de ruta, esos valores se sustituirán en la ruta según corresponda.</span><span class="sxs-lookup"><span data-stu-id="7919d-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="7919d-154">Los demás valores no coincidentes se generarán como parámetros de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7919d-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="7919d-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="7919d-155">asp-fragment</span></span>

<span data-ttu-id="7919d-156">El atributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) define un fragmento de dirección URL que se anexará a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7919d-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="7919d-157">El asistente de etiquetas delimitadoras agrega el carácter de almohadilla (#).</span><span class="sxs-lookup"><span data-stu-id="7919d-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="7919d-158">Observe el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="7919d-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="7919d-159">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="7919d-160">Las etiquetas hash son útiles al crear aplicaciones del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7919d-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="7919d-161">Por ejemplo, se pueden usar para el marcado y la búsqueda en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7919d-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="7919d-162">asp-area</span><span class="sxs-lookup"><span data-stu-id="7919d-162">asp-area</span></span>

<span data-ttu-id="7919d-163">El atributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) establece el nombre de área que se usa para establecer la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="7919d-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="7919d-164">En el siguiente ejemplo se muestra cómo el atributo de área provoca una reasignación de rutas.</span><span class="sxs-lookup"><span data-stu-id="7919d-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="7919d-165">Al establecer `asp-area` en "Blogs", el directorio *Areas/Blogs* se prefija en las rutas de los controladores y vistas asociados de esta etiqueta delimitadora.</span><span class="sxs-lookup"><span data-stu-id="7919d-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="7919d-166">**{Nombre del proyecto}**</span><span class="sxs-lookup"><span data-stu-id="7919d-166">**{Project name}**</span></span>
  * <span data-ttu-id="7919d-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="7919d-167">**wwwroot**</span></span>
  * <span data-ttu-id="7919d-168">**Áreas**</span><span class="sxs-lookup"><span data-stu-id="7919d-168">**Areas**</span></span>
    * <span data-ttu-id="7919d-169">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="7919d-169">**Blogs**</span></span>
      * <span data-ttu-id="7919d-170">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="7919d-170">**Controllers**</span></span>
        * <span data-ttu-id="7919d-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="7919d-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="7919d-172">**Vistas**</span><span class="sxs-lookup"><span data-stu-id="7919d-172">**Views**</span></span>
        * <span data-ttu-id="7919d-173">**Página principal**</span><span class="sxs-lookup"><span data-stu-id="7919d-173">**Home**</span></span>
          * <span data-ttu-id="7919d-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7919d-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="7919d-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7919d-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="7919d-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7919d-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="7919d-177">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="7919d-177">**Controllers**</span></span>

<span data-ttu-id="7919d-178">Dada la jerarquía de directorios anterior, el marcado para hacer referencia al archivo *AboutBlog.cshtml* es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="7919d-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="7919d-179">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="7919d-180">Para que las áreas funcionen en una aplicación de MVC, la plantilla de ruta debe incluir una referencia al área, en el caso de que exista.</span><span class="sxs-lookup"><span data-stu-id="7919d-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="7919d-181">Esta plantilla se representa mediante el segundo parámetro de la llamada de método `routes.MapRoute` en *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="7919d-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="7919d-182">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="7919d-182">asp-protocol</span></span>

<span data-ttu-id="7919d-183">El atributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) sirve para especificar un protocolo (por ejemplo, `https`) en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7919d-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="7919d-184">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7919d-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="7919d-185">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="7919d-186">El nombre de host del ejemplo es localhost, pero el asistente de etiquetas delimitadoras usa el dominio público del sitio web al generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7919d-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="7919d-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="7919d-187">asp-host</span></span>

<span data-ttu-id="7919d-188">El atributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) sirve para especificar un nombre de host en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7919d-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="7919d-189">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7919d-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="7919d-190">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="7919d-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="7919d-191">asp-page</span></span>

<span data-ttu-id="7919d-192">El atributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) se usa con las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="7919d-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="7919d-193">Úselo para establecer el valor del atributo `href` de una etiqueta delimitadora en una página específica.</span><span class="sxs-lookup"><span data-stu-id="7919d-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="7919d-194">La dirección URL se crea al prefijar el nombre de la página con una barra diagonal ("/").</span><span class="sxs-lookup"><span data-stu-id="7919d-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="7919d-195">En el ejemplo siguiente se señala a la página de Razor de asistentes:</span><span class="sxs-lookup"><span data-stu-id="7919d-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="7919d-196">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="7919d-197">El atributo `asp-page` es mutuamente excluyente con los atributos `asp-route`, `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="7919d-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="7919d-198">Pero se puede usar `asp-page` con `asp-route-{value}` para controlar el enrutamiento, como se muestra en el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="7919d-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="7919d-199">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="7919d-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="7919d-200">asp-page-handler</span></span>

<span data-ttu-id="7919d-201">El atributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) se usa con las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="7919d-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="7919d-202">Está diseñado para crear un vínculo con controladores de página específicos.</span><span class="sxs-lookup"><span data-stu-id="7919d-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="7919d-203">Observe el siguiente controlador de página:</span><span class="sxs-lookup"><span data-stu-id="7919d-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="7919d-204">El marcado asociado del modelo de página se vincula con el controlador de página `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="7919d-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="7919d-205">Tenga en cuenta que el prefijo `On<Verb>` del nombre de método del controlador de página se omite en el valor del atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="7919d-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="7919d-206">Si se tratara de un método asincrónico, también se omitiría el sufijo `Async`.</span><span class="sxs-lookup"><span data-stu-id="7919d-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="7919d-207">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="7919d-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="7919d-208">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7919d-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
