---
title: Aplicación auxiliar de etiquetas delimitadoras en ASP.NET Core
author: pkellner
description: Descubra los atributos de la aplicación auxiliar de etiquetas delimitadoras de ASP.NET Core y el papel que desempeña cada atributo al ampliar el comportamiento de la etiqueta delimitadora de código HTML.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 31ff62b6bedb5e577a51f341c89d241d06a83ad3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899413"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="ddbe1-103">Aplicación auxiliar de etiquetas delimitadoras en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddbe1-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="ddbe1-104">De [Peter Kellner](http://peterkellner.net) y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="ddbe1-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="ddbe1-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ddbe1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ddbe1-106">La [aplicación auxiliar de etiquetas delimitadoras](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) mejora la etiqueta delimitadora de código HTML estándar (`<a ... ></a>`) agregando nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="ddbe1-107">Por convención, los nombres de atributo tienen el prefijo `asp-`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="ddbe1-108">El valor de atributo `href` del elemento delimitador representado se determina mediante los valores de los atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="ddbe1-109">En los ejemplos de todo este documento se usa *SpeakerController*:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="ddbe1-110">A continuación se proporciona un inventario de los atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="ddbe1-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="ddbe1-111">asp-controller</span></span>

<span data-ttu-id="ddbe1-112">El atributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) asigna el controlador usado para generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="ddbe1-113">En el marcado siguiente se especifican todos los altavoces:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="ddbe1-114">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="ddbe1-115">Si el atributo `asp-controller` está especificado y `asp-action` no lo está, el valor `asp-action` predeterminado es la acción del controlador asociada a la vista que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="ddbe1-116">Si se omite `asp-action` en el marcado anterior y se usa la aplicación auxiliar de etiquetas delimitadoras en la vista *Index* de *HomeController* (*/Home*), el código HTML generado es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="ddbe1-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="ddbe1-117">asp-action</span></span>

<span data-ttu-id="ddbe1-118">El valor del atributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) representa el nombre de la acción del controlador incluido en el atributo `href` generado.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="ddbe1-119">El siguiente marcado establece el valor del atributo `href` generado en la página "Speaker Evaluations":</span><span class="sxs-lookup"><span data-stu-id="ddbe1-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="ddbe1-120">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="ddbe1-121">Si no hay ningún atributo `asp-controller` especificado, se usará el controlador predeterminado que llama a la vista que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="ddbe1-122">Si el valor del atributo `asp-action` es `Index`, no se anexa ninguna acción a la dirección URL, lo que da lugar a la invocación de la acción `Index` predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="ddbe1-123">La acción especificada (o su valor predeterminado), debe existir en el controlador al que se hace referencia en `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="ddbe1-124">asp-route-{valor}</span><span class="sxs-lookup"><span data-stu-id="ddbe1-124">asp-route-{value}</span></span>

<span data-ttu-id="ddbe1-125">El atributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite indicar un prefijo de ruta comodín.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="ddbe1-126">Cualquier valor que ocupe el marcador de posición `{value}` se interpretará como un parámetro de ruta potencial.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="ddbe1-127">Si no se encuentra ninguna ruta predeterminada, este prefijo de ruta se anexará al atributo `href` generado como valor y parámetro de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="ddbe1-128">En caso contrario, se sustituirá en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="ddbe1-129">Observe la siguiente acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="ddbe1-130">Con una plantilla de ruta predeterminada definida en *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="ddbe1-131">La vista de MVC usa el modelo, proporcionado por la acción, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-131">The MVC view uses the model, provided by the action, as follows:</span></span>

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

<span data-ttu-id="ddbe1-132">Se hace coincidir el marcador de posición `{id?}` de la ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="ddbe1-133">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="ddbe1-134">Supongamos que el prefijo de ruta no forma parte de la plantilla de enrutamiento coincidente, al igual que con la siguiente vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

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

<span data-ttu-id="ddbe1-135">Se genera el siguiente código HTML porque no se encontró `speakerid` en la ruta coincidente:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="ddbe1-136">Si no se especifica `asp-controller` o `asp-action`, se sigue el mismo proceso predeterminado que en el atributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="ddbe1-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="ddbe1-137">asp-route</span></span>

<span data-ttu-id="ddbe1-138">El atributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) se usa para crear una dirección URL que se vincula directamente con una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="ddbe1-139">Mediante [atributos de enrutamiento](xref:mvc/controllers/routing#attribute-routing) puede asignarse un nombre a una ruta tal y como se muestra en `SpeakerController` y usarse en su acción `Evaluations`:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="ddbe1-140">En el siguiente marcado, el atributo `asp-route` hace referencia a la ruta con nombre:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="ddbe1-141">La aplicación auxiliar de etiquetas delimitadoras genera una ruta directamente a esa acción de controlador mediante la dirección URL */Speaker/Evaluations*.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="ddbe1-142">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="ddbe1-143">Si además de `asp-route` se especifica `asp-controller` o `asp-action`, la ruta generada puede no ser la esperada.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="ddbe1-144">Para evitar un conflicto de ruta, no se debe usar `asp-route` con los atributos `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="ddbe1-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="ddbe1-145">asp-all-route-data</span></span>

<span data-ttu-id="ddbe1-146">El atributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite crear un diccionario de pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="ddbe1-147">La clave es el nombre del parámetro, mientras que el valor es el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="ddbe1-148">En el ejemplo siguiente se inicializa un diccionario y se pasa a una vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="ddbe1-149">Los datos también se podrían pasar con el modelo.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="ddbe1-150">El código anterior genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="ddbe1-151">Se acopla el diccionario `asp-all-route-data` para generar una cadena de consulta que cumpla los requisitos de la acción `Evaluations` sobrecargada:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="ddbe1-152">Si alguna de las claves del diccionario coincide con los parámetros de ruta, esos valores se sustituirán en la ruta según corresponda.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="ddbe1-153">Los demás valores no coincidentes se generarán como parámetros de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="ddbe1-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="ddbe1-154">asp-fragment</span></span>

<span data-ttu-id="ddbe1-155">El atributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) define un fragmento de dirección URL que se anexará a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="ddbe1-156">La aplicación auxiliar de etiquetas delimitadoras agrega el carácter de almohadilla (#).</span><span class="sxs-lookup"><span data-stu-id="ddbe1-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="ddbe1-157">Observe el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="ddbe1-158">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="ddbe1-159">Las etiquetas hash son útiles al crear aplicaciones del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="ddbe1-160">Por ejemplo, se pueden usar para el marcado y la búsqueda en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="ddbe1-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="ddbe1-161">asp-area</span></span>

<span data-ttu-id="ddbe1-162">El atributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) establece el nombre de área que se usa para establecer la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="ddbe1-163">En el siguiente ejemplo se muestra cómo el atributo de área provoca una reasignación de rutas.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="ddbe1-164">Al establecer `asp-area` en "Blogs", el directorio *Areas/Blogs* se prefija en las rutas de los controladores y vistas asociados de esta etiqueta delimitadora.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="ddbe1-165">**<Nombre del proyecto\>**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-165">**<Project name\>**</span></span>
  * <span data-ttu-id="ddbe1-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-166">**wwwroot**</span></span>
  * <span data-ttu-id="ddbe1-167">**Áreas**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-167">**Areas**</span></span>
    * <span data-ttu-id="ddbe1-168">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-168">**Blogs**</span></span>
      * <span data-ttu-id="ddbe1-169">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-169">**Controllers**</span></span>
        * <span data-ttu-id="ddbe1-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="ddbe1-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="ddbe1-171">**Vistas**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-171">**Views**</span></span>
        * <span data-ttu-id="ddbe1-172">**Página principal**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-172">**Home**</span></span>
          * <span data-ttu-id="ddbe1-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ddbe1-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="ddbe1-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ddbe1-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="ddbe1-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ddbe1-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="ddbe1-176">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="ddbe1-176">**Controllers**</span></span>

<span data-ttu-id="ddbe1-177">Dada la jerarquía de directorios anterior, el marcado para hacer referencia al archivo *AboutBlog.cshtml* es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="ddbe1-178">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="ddbe1-179">Para que las áreas funcionen en una aplicación de MVC, la plantilla de ruta debe incluir una referencia al área, en el caso de que exista.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="ddbe1-180">Esta plantilla se representa mediante el segundo parámetro de la llamada de método `routes.MapRoute` en *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="ddbe1-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="ddbe1-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="ddbe1-181">asp-protocol</span></span>

<span data-ttu-id="ddbe1-182">El atributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) sirve para especificar un protocolo (por ejemplo, `https`) en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="ddbe1-183">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-183">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="ddbe1-184">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-184">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="ddbe1-185">El nombre de host del ejemplo es localhost, pero la aplicación auxiliar de etiquetas delimitadoras usa el dominio público del sitio web al generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-185">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="ddbe1-186">asp-host</span><span class="sxs-lookup"><span data-stu-id="ddbe1-186">asp-host</span></span>

<span data-ttu-id="ddbe1-187">El atributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) sirve para especificar un nombre de host en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-187">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="ddbe1-188">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-188">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="ddbe1-189">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-189">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="ddbe1-190">asp-page</span><span class="sxs-lookup"><span data-stu-id="ddbe1-190">asp-page</span></span>

<span data-ttu-id="ddbe1-191">El atributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) se usa con las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-191">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="ddbe1-192">Úselo para establecer el valor del atributo `href` de una etiqueta delimitadora en una página específica.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-192">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="ddbe1-193">La dirección URL se crea al prefijar el nombre de la página con una barra diagonal ("/").</span><span class="sxs-lookup"><span data-stu-id="ddbe1-193">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="ddbe1-194">En el ejemplo siguiente se señala a la página de Razor de asistentes:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-194">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="ddbe1-195">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-195">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="ddbe1-196">El atributo `asp-page` es mutuamente excluyente con los atributos `asp-route`, `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-196">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="ddbe1-197">Pero se puede usar `asp-page` con `asp-route-{value}` para controlar el enrutamiento, como se muestra en el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-197">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="ddbe1-198">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-198">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="ddbe1-199">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="ddbe1-199">asp-page-handler</span></span>

<span data-ttu-id="ddbe1-200">El atributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) se usa con las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-200">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="ddbe1-201">Está diseñado para crear un vínculo con controladores de página específicos.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-201">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="ddbe1-202">Observe el siguiente controlador de página:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-202">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="ddbe1-203">El marcado asociado del modelo de página se vincula con el controlador de página `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-203">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="ddbe1-204">Tenga en cuenta que el prefijo `On<Verb>` del nombre de método del controlador de página se omite en el valor del atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-204">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="ddbe1-205">Si se tratara de un método asincrónico, también se omitiría el sufijo `Async`.</span><span class="sxs-lookup"><span data-stu-id="ddbe1-205">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="ddbe1-206">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="ddbe1-206">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="ddbe1-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ddbe1-207">Additional resources</span></span>

* [<span data-ttu-id="ddbe1-208">Áreas</span><span class="sxs-lookup"><span data-stu-id="ddbe1-208">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="ddbe1-209">Introducción a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="ddbe1-209">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
