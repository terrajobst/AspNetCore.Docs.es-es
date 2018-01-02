---
title: Anclar etiqueta auxiliar | Documentos de Microsoft
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiquetas de delimitador"
keywords: "ASP.NET Core, aplicación auxiliar de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 503ad7c4ce8c4f08b2a06dbe9f985566f54d3ca2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="47ffd-104">Aplicación auxiliar de etiquetas de delimitador</span><span class="sxs-lookup"><span data-stu-id="47ffd-104">Anchor Tag Helper</span></span>

<span data-ttu-id="47ffd-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="47ffd-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="47ffd-106">La aplicación auxiliar de etiquetas de delimitador mejora el delimitador HTML (`<a ... ></a>`) etiqueta mediante la adición de nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="47ffd-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="47ffd-107">El vínculo generado (en la `href` etiqueta) se crea con los nuevos atributos.</span><span class="sxs-lookup"><span data-stu-id="47ffd-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="47ffd-108">Esa dirección URL puede incluir un protocolo como https opcional.</span><span class="sxs-lookup"><span data-stu-id="47ffd-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="47ffd-109">El controlador de altavoz siguiente se utiliza en los ejemplos de este documento.</span><span class="sxs-lookup"><span data-stu-id="47ffd-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="47ffd-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="47ffd-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="47ffd-111">Atributos de aplicación auxiliar de etiqueta de anclaje</span><span class="sxs-lookup"><span data-stu-id="47ffd-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="47ffd-112">controlador de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-112">asp-controller</span></span>

<span data-ttu-id="47ffd-113">`asp-controller`se utiliza para asociar el controlador que se usarán para generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="47ffd-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="47ffd-114">Los controladores especificados deben existir en el proyecto actual.</span><span class="sxs-lookup"><span data-stu-id="47ffd-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="47ffd-115">El código siguiente enumera todos los altavoces:</span><span class="sxs-lookup"><span data-stu-id="47ffd-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="47ffd-116">El marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="47ffd-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="47ffd-117">Si el `asp-controller` se especifica y `asp-action` no lo es, el valor predeterminado `asp-action` será el método de controlador predeterminado de la vista está ejecutando actualmente.</span><span class="sxs-lookup"><span data-stu-id="47ffd-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="47ffd-118">Que es, en el ejemplo anterior, si `asp-action` se omite, y se genera esta aplicación auxiliar de etiquetas de delimitador de *HomeController*del `Index` vista (**/Home**), el marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="47ffd-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="47ffd-119">acción de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-119">asp-action</span></span>

<span data-ttu-id="47ffd-120">`asp-action`es el nombre del método de acción en el controlador que se van a incluir en los botones generados `href`.</span><span class="sxs-lookup"><span data-stu-id="47ffd-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="47ffd-121">Por ejemplo, el código siguiente establece el generado `href` para que apunte a la página de detalles de altavoces:</span><span class="sxs-lookup"><span data-stu-id="47ffd-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="47ffd-122">El marcado generado será:</span><span class="sxs-lookup"><span data-stu-id="47ffd-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="47ffd-123">Si no hay ningún `asp-controller` atributo se especifica, se usará el controlador de forma predeterminada al llamar a la vista ejecutar la vista actual.</span><span class="sxs-lookup"><span data-stu-id="47ffd-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="47ffd-124">Si el atributo `asp-action` es `Index`, a continuación, no se anexa ninguna acción a la dirección URL, lo que el valor predeterminado `Index` llamada al método.</span><span class="sxs-lookup"><span data-stu-id="47ffd-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="47ffd-125">La acción especificada (o su valor predeterminado), debe existir en el controlador al que hace referencia en `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="47ffd-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="47ffd-126">página ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-126">asp-page</span></span>

<span data-ttu-id="47ffd-127">Use la `asp-page` atributo en una etiqueta delimitadora para establecer su dirección URL para que señale a una página específica.</span><span class="sxs-lookup"><span data-stu-id="47ffd-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="47ffd-128">El nombre de la página con una barra diagonal "/" crea la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="47ffd-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="47ffd-129">La dirección URL en el ejemplo siguiente señala a la página "Altavoz" en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="47ffd-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="47ffd-130">El `asp-page` atributo en el ejemplo de código anterior representa la salida HTML en la vista que es similar al fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="47ffd-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```
<span data-ttu-id="47ffd-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">altavoces de vista</a></span><span class="sxs-lookup"><span data-stu-id="47ffd-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a></span></span>
```

The `asp-route-id` produces the following output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```


### <a name="asp-route-value"></a><span data-ttu-id="47ffd-132">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="47ffd-132">asp-route-{value}</span></span>

<span data-ttu-id="47ffd-133">`asp-route-`es un prefijo de ruta comodín.</span><span class="sxs-lookup"><span data-stu-id="47ffd-133">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="47ffd-134">Cualquier valor que se coloca después de que el guión al final se interpretará como un parámetro de ruta posibles.</span><span class="sxs-lookup"><span data-stu-id="47ffd-134">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="47ffd-135">Si no se encuentra una ruta predeterminada, este prefijo de ruta se anexará a la etiqueta href generada como un parámetro de solicitud y un valor.</span><span class="sxs-lookup"><span data-stu-id="47ffd-135">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="47ffd-136">En caso contrario, se sustituirá en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="47ffd-136">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="47ffd-137">Suponiendo que haya un método de controlador define como sigue:</span><span class="sxs-lookup"><span data-stu-id="47ffd-137">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="47ffd-138">Y haga que la plantilla de ruta predeterminados definida en su *Startup.cs* como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="47ffd-138">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="47ffd-139">El **cshtml** archivo que contiene la aplicación auxiliar etiqueta de anclaje necesarios para utilizar el **altavoces** es el parámetro de modelo que se pasan desde el controlador a la vista siguiente:</span><span class="sxs-lookup"><span data-stu-id="47ffd-139">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="47ffd-140">El código HTML generado, a continuación, será la siguiente porque **identificador** se encontró en la ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="47ffd-140">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="47ffd-141">Si el prefijo de ruta no forma parte de la plantilla de enrutamiento, que es el caso de los siguientes **cshtml** archivo:</span><span class="sxs-lookup"><span data-stu-id="47ffd-141">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="47ffd-142">El código HTML generado, a continuación, será la siguiente porque **speakerid** no se encontró en la ruta coincida:</span><span class="sxs-lookup"><span data-stu-id="47ffd-142">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="47ffd-143">Si el valor `asp-controller` o `asp-action` no se especifica, se sigue el mismo proceso predeterminado tal y como se encuentra en la `asp-route` atributo.</span><span class="sxs-lookup"><span data-stu-id="47ffd-143">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="47ffd-144">ruta de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-144">asp-route</span></span>

<span data-ttu-id="47ffd-145">`asp-route`Proporciona una manera de crear una dirección URL que se vincula directamente a una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="47ffd-145">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="47ffd-146">Uso de atributos de enrutamientos, una ruta puede tener el nombre tal y como se muestra en el `SpeakerController` y se utiliza en su `Evaluations` método.</span><span class="sxs-lookup"><span data-stu-id="47ffd-146">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="47ffd-147">`Name = "speakerevals"`indica la etiqueta de anclaje de aplicación auxiliar para generar una ruta directa a ese método de controlador utilizando la dirección URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="47ffd-147">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="47ffd-148">Si `asp-controller` o `asp-action` se especifica además `asp-route`, la ruta generadas no puede ser los esperados.</span><span class="sxs-lookup"><span data-stu-id="47ffd-148">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="47ffd-149">`asp-route`no debe usarse con cualquiera de los atributos `asp-controller` o `asp-action` para evitar un conflicto de ruta.</span><span class="sxs-lookup"><span data-stu-id="47ffd-149">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="47ffd-150">ASP-all-datos de ruta</span><span class="sxs-lookup"><span data-stu-id="47ffd-150">asp-all-route-data</span></span>

<span data-ttu-id="47ffd-151">`asp-all-route-data`permite crear un diccionario de pares clave / valor donde la clave es el nombre del parámetro y el valor es el valor asociado a esa clave.</span><span class="sxs-lookup"><span data-stu-id="47ffd-151">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="47ffd-152">Como muestra el ejemplo siguiente, se crea un diccionario en línea y los datos se pasan a la vista de razor.</span><span class="sxs-lookup"><span data-stu-id="47ffd-152">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="47ffd-153">Como alternativa, también podrían pasar los datos con el modelo.</span><span class="sxs-lookup"><span data-stu-id="47ffd-153">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="47ffd-154">El código anterior genera la siguiente dirección URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="47ffd-154">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="47ffd-155">Cuando se hace clic el vínculo, el método del controlador `EvaluationsCurrent` se llama.</span><span class="sxs-lookup"><span data-stu-id="47ffd-155">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="47ffd-156">Se denomina porque ese controlador tiene dos parámetros de cadena que coincide con lo que se ha creado a partir del `asp-all-route-data` diccionario.</span><span class="sxs-lookup"><span data-stu-id="47ffd-156">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="47ffd-157">Si todas las claves de la búsqueda de coincidencias de diccionario de parámetros de ruta, se sustituirán los valores en la ruta según corresponda y los demás valores no coincidentes se generará como parámetros de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="47ffd-157">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="47ffd-158">fragmento de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-158">asp-fragment</span></span>

<span data-ttu-id="47ffd-159">`asp-fragment`define un fragmento de dirección URL que se anexará a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="47ffd-159">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="47ffd-160">La aplicación auxiliar de etiquetas de delimitador agregará el carácter de almohadilla (#).</span><span class="sxs-lookup"><span data-stu-id="47ffd-160">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="47ffd-161">Si crea una etiqueta:</span><span class="sxs-lookup"><span data-stu-id="47ffd-161">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="47ffd-162">La dirección URL generada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="47ffd-162">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="47ffd-163">Etiquetas de hash son útiles al crear aplicaciones del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="47ffd-163">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="47ffd-164">Se puede usar para marcar y buscar en JavaScript, por ejemplo fácilmente.</span><span class="sxs-lookup"><span data-stu-id="47ffd-164">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="47ffd-165">área de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-165">asp-area</span></span>

<span data-ttu-id="47ffd-166">`asp-area`establece el nombre del área que ASP.NET Core se utiliza para establecer la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="47ffd-166">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="47ffd-167">A continuación se muestran ejemplos de cómo el atributo de área hace una reasignación de rutas.</span><span class="sxs-lookup"><span data-stu-id="47ffd-167">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="47ffd-168">Establecer `asp-area` a Blogs prefijos el directorio `Areas/Blogs` a las rutas de los controladores asociados y vistas de esta etiqueta delimitadora.</span><span class="sxs-lookup"><span data-stu-id="47ffd-168">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="47ffd-169">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="47ffd-169">Project name</span></span>
  * <span data-ttu-id="47ffd-170">wwwroot</span><span class="sxs-lookup"><span data-stu-id="47ffd-170">wwwroot</span></span>
  * <span data-ttu-id="47ffd-171">Áreas</span><span class="sxs-lookup"><span data-stu-id="47ffd-171">Areas</span></span>
    * <span data-ttu-id="47ffd-172">Blogs</span><span class="sxs-lookup"><span data-stu-id="47ffd-172">Blogs</span></span>
      * <span data-ttu-id="47ffd-173">Controladores</span><span class="sxs-lookup"><span data-stu-id="47ffd-173">Controllers</span></span>
        * <span data-ttu-id="47ffd-174">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="47ffd-174">HomeController.cs</span></span>
      * <span data-ttu-id="47ffd-175">Vistas</span><span class="sxs-lookup"><span data-stu-id="47ffd-175">Views</span></span>
        * <span data-ttu-id="47ffd-176">Página principal</span><span class="sxs-lookup"><span data-stu-id="47ffd-176">Home</span></span>
          * <span data-ttu-id="47ffd-177">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="47ffd-177">Index.cshtml</span></span>
          * <span data-ttu-id="47ffd-178">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="47ffd-178">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="47ffd-179">Controladores</span><span class="sxs-lookup"><span data-stu-id="47ffd-179">Controllers</span></span>

<span data-ttu-id="47ffd-180">Especificar una etiqueta de área que es válida, como ```area="Blogs"``` al hacer referencia a la ```AboutBlog.cshtml``` archivo tendrá un aspecto similar al siguiente utilizando la aplicación auxiliar de etiquetas de delimitador.</span><span class="sxs-lookup"><span data-stu-id="47ffd-180">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="47ffd-181">El código HTML generado incluirá el segmento de áreas y será como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="47ffd-181">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="47ffd-182">Para que las áreas MVC trabajar en una aplicación web, la plantilla de ruta debe incluir una referencia al área si existe.</span><span class="sxs-lookup"><span data-stu-id="47ffd-182">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="47ffd-183">Esa plantilla, que es el segundo parámetro de la `routes.MapRoute` llamada al método, se mostrarán como:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="47ffd-183">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="47ffd-184">Protocolo de ASP</span><span class="sxs-lookup"><span data-stu-id="47ffd-184">asp-protocol</span></span>

<span data-ttu-id="47ffd-185">El `asp-protocol` es para especificar un protocolo (como `https`) en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="47ffd-185">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="47ffd-186">Un ejemplo de aplicación auxiliar de etiquetas de delimitador que incluya el protocolo será como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="47ffd-186">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="47ffd-187">y generará HTML de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="47ffd-187">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="47ffd-188">El dominio en el ejemplo es el host local, pero la aplicación auxiliar de etiquetas de delimitador de dominio del sitio Web público se utiliza al generar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="47ffd-188">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47ffd-189">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="47ffd-189">Additional resources</span></span>

* [<span data-ttu-id="47ffd-190">Áreas</span><span class="sxs-lookup"><span data-stu-id="47ffd-190">Areas</span></span>](xref:mvc/controllers/areas)
