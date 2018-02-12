---
title: "Aplicación auxiliar de etiquetas delimitadoras en ASP.NET Core"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiquetas delimitadoras"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="9f389-103">Aplicación auxiliar de etiquetas delimitadoras</span><span class="sxs-lookup"><span data-stu-id="9f389-103">Anchor Tag Helper</span></span>

<span data-ttu-id="9f389-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="9f389-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="9f389-105">La aplicación auxiliar de etiquetas delimitadoras mejora la etiqueta delimitadora HTML (`<a ... ></a>`) mediante la adición de nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="9f389-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="9f389-106">El vínculo generado (en la etiqueta `href`) se crea con los nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="9f389-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="9f389-107">Esa dirección URL puede incluir un protocolo opcional como https.</span><span class="sxs-lookup"><span data-stu-id="9f389-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="9f389-108">El siguiente controlador de altavoz se utiliza en los ejemplos de este documento.</span><span class="sxs-lookup"><span data-stu-id="9f389-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="9f389-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="9f389-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="9f389-110">Atributos de aplicación auxiliar de etiquetas delimitadoras</span><span class="sxs-lookup"><span data-stu-id="9f389-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="9f389-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="9f389-111">asp-controller</span></span>

<span data-ttu-id="9f389-112">`asp-controller` se utiliza para asociar el controlador que se usará para generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9f389-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="9f389-113">Los controladores especificados deben existir en el proyecto actual.</span><span class="sxs-lookup"><span data-stu-id="9f389-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="9f389-114">El código siguiente enumera todos los altavoces:</span><span class="sxs-lookup"><span data-stu-id="9f389-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="9f389-115">El marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="9f389-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="9f389-116">Si se especifica `asp-controller` pero no `asp-action`, el valor predeterminado `asp-action` será el método de controlador predeterminado de la vista que se está ejecutando en ese momento.</span><span class="sxs-lookup"><span data-stu-id="9f389-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="9f389-117">Es decir, en el ejemplo anterior, si se omite `asp-action`, y esta aplicación auxiliar de etiquetas delimitadoras se genera desde la vista `Index` de *HomeController* (**/Home**), el marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="9f389-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="9f389-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="9f389-118">asp-action</span></span>

<span data-ttu-id="9f389-119">`asp-action` es el nombre del método de acción en el controlador que se incluirá en el `href` generado.</span><span class="sxs-lookup"><span data-stu-id="9f389-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="9f389-120">Por ejemplo, el código siguiente establece el `href` generado para que apunte a la página de detalles del altavoz:</span><span class="sxs-lookup"><span data-stu-id="9f389-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="9f389-121">El marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="9f389-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="9f389-122">Si no hay ningún atributo `asp-controller` especificado, se usará el controlador predeterminado que llama a la vista que se ejecuta actualmente.</span><span class="sxs-lookup"><span data-stu-id="9f389-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="9f389-123">Si el atributo `asp-action` es `Index`, no se anexa ninguna acción a la dirección URL, por lo que se llama al método `Index` predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9f389-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="9f389-124">La acción especificada (o su valor predeterminado), debe existir en el controlador al que se hace referencia en `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="9f389-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="9f389-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="9f389-125">asp-page</span></span>

<span data-ttu-id="9f389-126">Use el atributo `asp-page` en una etiqueta delimitadora para establecer su dirección URL de modo que señale a una página específica.</span><span class="sxs-lookup"><span data-stu-id="9f389-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="9f389-127">La dirección URL se crea al prefijar el nombre de la página con una barra diagonal "/".</span><span class="sxs-lookup"><span data-stu-id="9f389-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="9f389-128">La dirección URL del ejemplo siguiente apunta a la página "Altavoz" en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9f389-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="9f389-129">El atributo `asp-page` en el ejemplo de código anterior representa una salida HTML en la vista similar al fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f389-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="9f389-130">El atributo `asp-page` es mutuamente excluyente con los atributos `asp-route`, `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="9f389-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="9f389-131">Pero `asp-page` puede utilizarse con `asp-route-id` para controlar el enrutamiento, como se muestra en el ejemplo de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f389-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="9f389-132">`asp-route-id` genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="9f389-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="9f389-133">Para usar el atributo `asp-page` en las páginas de Razor, las direcciones URL deben ser una ruta de acceso relativa, por ejemplo `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="9f389-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="9f389-134">Las rutas de acceso relativas en el atributo `asp-page` no están disponibles en las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="9f389-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="9f389-135">En su lugar, use la sintaxis "/" para las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="9f389-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="9f389-136">asp-route-{valor}</span><span class="sxs-lookup"><span data-stu-id="9f389-136">asp-route-{value}</span></span>

<span data-ttu-id="9f389-137">`asp-route-` es un prefijo de ruta comodín.</span><span class="sxs-lookup"><span data-stu-id="9f389-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="9f389-138">Cualquier valor que se coloque después del guion final se interpretará como un parámetro de ruta potencial.</span><span class="sxs-lookup"><span data-stu-id="9f389-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="9f389-139">Si no se encuentra una ruta predeterminada, este prefijo de ruta se anexará a la etiqueta href generada como un parámetro de solicitud y un valor.</span><span class="sxs-lookup"><span data-stu-id="9f389-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="9f389-140">En caso contrario, se sustituirá en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="9f389-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="9f389-141">Suponiendo que tenga un método de controlador definido del modo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f389-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="9f389-142">Y que haya definido la plantilla de ruta predeterminada en su *Startup.cs* como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="9f389-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="9f389-143">A continuación se muestra el archivo **cshtml** que contiene la aplicación auxiliar de etiquetas delimitadoras necesaria para utilizar el parámetro de modelo **speaker** que se pasa desde el controlador a la vista:</span><span class="sxs-lookup"><span data-stu-id="9f389-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9f389-144">El código HTML generado será como sigue, dado que **id** se encontró en la ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9f389-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="9f389-145">Si el prefijo de ruta no forma parte de la plantilla de enrutamiento encontrada, como es el caso del siguiente archivo **cshtml**:</span><span class="sxs-lookup"><span data-stu-id="9f389-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9f389-146">El código HTML generado será como sigue, dado que **speakerid** no se encontró en la ruta coincidente:</span><span class="sxs-lookup"><span data-stu-id="9f389-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="9f389-147">Si no se especifica `asp-controller` o `asp-action`, se sigue el mismo proceso predeterminado que en el atributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="9f389-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="9f389-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="9f389-148">asp-route</span></span>

<span data-ttu-id="9f389-149">`asp-route` proporciona una manera de crear una dirección URL que se vincula directamente a una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="9f389-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="9f389-150">Utilizando atributos de enrutamiento, puede asignarse el nombre a una ruta tal y como se muestra en `SpeakerController` y usarse en su método `Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="9f389-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="9f389-151">`Name = "speakerevals"` indica a la aplicación auxiliar de etiquetas delimitadoras que genere una ruta directa a ese método de controlador usando la dirección URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="9f389-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="9f389-152">Si además de `asp-route` se especifica `asp-controller` o `asp-action`, la ruta generada puede no ser la esperada.</span><span class="sxs-lookup"><span data-stu-id="9f389-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="9f389-153">`asp-route` no debe usarse con ninguno de los atributos `asp-controller` o `asp-action` para evitar un conflicto de ruta.</span><span class="sxs-lookup"><span data-stu-id="9f389-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="9f389-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="9f389-154">asp-all-route-data</span></span>

<span data-ttu-id="9f389-155">`asp-all-route-data` permite crear un diccionario de pares clave/valor donde la clave es el nombre del parámetro y el valor es el valor asociado a esa clave.</span><span class="sxs-lookup"><span data-stu-id="9f389-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="9f389-156">Como se muestra en el ejemplo siguiente, se crea un diccionario insertado y los datos se pasan a la vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="9f389-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="9f389-157">También hay la opción de pasar los datos con su modelo.</span><span class="sxs-lookup"><span data-stu-id="9f389-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="9f389-158">El código anterior genera la siguiente dirección URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="9f389-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="9f389-159">Cuando se hace clic en el vínculo, se llama al método de controlador `EvaluationsCurrent`.</span><span class="sxs-lookup"><span data-stu-id="9f389-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="9f389-160">La llamada se realiza porque ese controlador tiene dos parámetros de cadena que coinciden con lo que se ha creado a partir del diccionario `asp-all-route-data`.</span><span class="sxs-lookup"><span data-stu-id="9f389-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="9f389-161">Si alguna de las claves del diccionario coincide con los parámetros de ruta, se sustituirán los valores en la ruta según corresponda y los demás valores no coincidentes se generarán como parámetros de solicitud.</span><span class="sxs-lookup"><span data-stu-id="9f389-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="9f389-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="9f389-162">asp-fragment</span></span>

<span data-ttu-id="9f389-163">`asp-fragment` define un fragmento de dirección URL que se anexará a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9f389-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="9f389-164">La aplicación auxiliar de etiquetas delimitadoras agregará el carácter de almohadilla (#).</span><span class="sxs-lookup"><span data-stu-id="9f389-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="9f389-165">Si crea una etiqueta:</span><span class="sxs-lookup"><span data-stu-id="9f389-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="9f389-166">La dirección URL generada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="9f389-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="9f389-167">Las etiquetas hash son útiles al crear aplicaciones del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="9f389-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="9f389-168">Por ejemplo, se pueden usar para el marcado y la búsqueda en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9f389-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="9f389-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="9f389-169">asp-area</span></span>

<span data-ttu-id="9f389-170">`asp-area` establece el nombre del área que ASP.NET Core utiliza para establecer la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="9f389-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="9f389-171">A continuación se muestran ejemplos de cómo el atributo de área hace una reasignación de rutas.</span><span class="sxs-lookup"><span data-stu-id="9f389-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="9f389-172">Al establecer `asp-area` en Blogs, el directorio `Areas/Blogs` se prefija en las rutas de los controladores y vistas asociados de esta etiqueta delimitadora.</span><span class="sxs-lookup"><span data-stu-id="9f389-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="9f389-173">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="9f389-173">Project name</span></span>
  * <span data-ttu-id="9f389-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="9f389-174">wwwroot</span></span>
  * <span data-ttu-id="9f389-175">Áreas</span><span class="sxs-lookup"><span data-stu-id="9f389-175">Areas</span></span>
    * <span data-ttu-id="9f389-176">Blogs</span><span class="sxs-lookup"><span data-stu-id="9f389-176">Blogs</span></span>
      * <span data-ttu-id="9f389-177">Controladores</span><span class="sxs-lookup"><span data-stu-id="9f389-177">Controllers</span></span>
        * <span data-ttu-id="9f389-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="9f389-178">HomeController.cs</span></span>
      * <span data-ttu-id="9f389-179">Vistas</span><span class="sxs-lookup"><span data-stu-id="9f389-179">Views</span></span>
        * <span data-ttu-id="9f389-180">Página principal</span><span class="sxs-lookup"><span data-stu-id="9f389-180">Home</span></span>
          * <span data-ttu-id="9f389-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="9f389-181">Index.cshtml</span></span>
          * <span data-ttu-id="9f389-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="9f389-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="9f389-183">Controladores</span><span class="sxs-lookup"><span data-stu-id="9f389-183">Controllers</span></span>

<span data-ttu-id="9f389-184">Especificar una etiqueta de área que sea válida, como ```area="Blogs"``` al hacer referencia al archivo ```AboutBlog.cshtml```, tendrá un aspecto similar al siguiente mediante la aplicación auxiliar de etiquetas delimitadoras.</span><span class="sxs-lookup"><span data-stu-id="9f389-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="9f389-185">El código HTML generado incluirá el segmento de áreas y será así:</span><span class="sxs-lookup"><span data-stu-id="9f389-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="9f389-186">Para que las áreas MVC funcionen en una aplicación web, la plantilla de ruta debe incluir una referencia al área si existe.</span><span class="sxs-lookup"><span data-stu-id="9f389-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="9f389-187">Esa plantilla, que es el segundo parámetro de la llamada al método `routes.MapRoute`, se mostrará como: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="9f389-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="9f389-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="9f389-188">asp-protocol</span></span>

<span data-ttu-id="9f389-189">`asp-protocol` sirve para especificar un protocolo (como `https`) en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9f389-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="9f389-190">El siguiente es un ejemplo de aplicación auxiliar de etiquetas delimitadoras que incluya el protocolo:</span><span class="sxs-lookup"><span data-stu-id="9f389-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="9f389-191">Este será el código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="9f389-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="9f389-192">El dominio del ejemplo es el host local, pero la aplicación auxiliar de etiquetas delimitadoras utiliza el dominio público del sitio web al generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9f389-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f389-193">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9f389-193">Additional resources</span></span>

* [<span data-ttu-id="9f389-194">Áreas</span><span class="sxs-lookup"><span data-stu-id="9f389-194">Areas</span></span>](xref:mvc/controllers/areas)
