---
title: "Información general de vistas"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="d7533-103">Representación HTML con vistas de MVC de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7533-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="d7533-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d7533-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="d7533-105">Controladores de núcleo MVC de ASP.NET pueden devolver resultados con formato mediante *vistas*.</span><span class="sxs-lookup"><span data-stu-id="d7533-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="d7533-106">¿Qué son las vistas?</span><span class="sxs-lookup"><span data-stu-id="d7533-106">What are Views?</span></span>

<span data-ttu-id="d7533-107">En el modelo Model-View-Controller (MVC), el *vista* encapsula los detalles de la presentación de la interacción del usuario con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7533-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="d7533-108">Las vistas son plantillas HTML con código incrustado que generan contenido para enviar al cliente.</span><span class="sxs-lookup"><span data-stu-id="d7533-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="d7533-109">Vistas usan [sintaxis Razor](razor.md), que permite que el código para interactuar con HTML con un mínimo de código o ceremonia.</span><span class="sxs-lookup"><span data-stu-id="d7533-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="d7533-110">Vistas ASP.NET MVC Core son *.cshtml* archivos almacenados de forma predeterminada en un *vistas* carpeta dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7533-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="d7533-111">Normalmente, cada controlador tendrá su propia carpeta, en el que son para las acciones de un controlador específico.</span><span class="sxs-lookup"><span data-stu-id="d7533-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![Carpeta de vistas en el Explorador de soluciones](overview/_static/views_solution_explorer.png)

<span data-ttu-id="d7533-113">Además de las vistas específicas de la acción, [vistas parciales](partial.md), [diseños y otros archivos de vista especial](layout.md) puede utilizarse para ayudar a reducir la repetición y permitir su reutilización en las vistas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7533-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="d7533-114">Ventajas del uso de vistas</span><span class="sxs-lookup"><span data-stu-id="d7533-114">Benefits of Using Views</span></span>

<span data-ttu-id="d7533-115">Las vistas proporcionan [separación de intereses](http://deviq.com/separation-of-concerns/) dentro de una aplicación MVC, la encapsulación de marcado de nivel de interfaz de usuario por separado de la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="d7533-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="d7533-116">Vistas de MVC de ASP.NET usan [sintaxis Razor](razor.md) para agilizar el cambio entre HTML marcado y servidor lógica sencilla.</span><span class="sxs-lookup"><span data-stu-id="d7533-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="d7533-117">Aspectos comunes y repetitivas de interfaz de usuario de la aplicación pueden ser reutilizados entre vistas con [diseño y directivas compartidas](layout.md) o [vistas parciales](partial.md).</span><span class="sxs-lookup"><span data-stu-id="d7533-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="d7533-118">Crear una vista</span><span class="sxs-lookup"><span data-stu-id="d7533-118">Creating a View</span></span>

<span data-ttu-id="d7533-119">Se crean vistas que son específicas para un controlador en el *vistas / [ControllerName]* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d7533-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="d7533-120">Vistas que se comparten entre los controladores se colocan en la */vistas/Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d7533-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="d7533-121">Asignar nombre al archivo de vista igual que su acción de controlador asociado y agregue el *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="d7533-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="d7533-122">Por ejemplo, para crear una vista para la *sobre* acción en el *inicio* controlador, debe crear el *About.cshtml* un archivo en el   */vistas/inicio*carpeta.</span><span class="sxs-lookup"><span data-stu-id="d7533-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="d7533-123">Un ejemplo de archivo de vista (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="d7533-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="d7533-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d7533-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="d7533-125">*Razor* código se indica mediante el `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="d7533-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="d7533-126">Instrucciones de C# se ejecutan dentro de bloques de código desactivar llaves de Razor (`{` `}`), como la asignación de "About" para el `ViewData["Title"]` elemento mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7533-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="d7533-127">Razor puede utilizarse para mostrar valores en HTML haciendo referencia simplemente el valor con el `@` de símbolos, como se muestra en el `<h2>` y `<h3>` elementos anteriores.</span><span class="sxs-lookup"><span data-stu-id="d7533-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="d7533-128">Esta vista se centra en sólo la parte de la salida para el que es responsable.</span><span class="sxs-lookup"><span data-stu-id="d7533-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="d7533-129">El resto del diseño de la página y otros aspectos comunes de la vista, se especifican en otro lugar.</span><span class="sxs-lookup"><span data-stu-id="d7533-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="d7533-130">Obtenga más información sobre [diseño y la lógica de vista compartida](layout.md).</span><span class="sxs-lookup"><span data-stu-id="d7533-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="d7533-131">¿Cómo especificar vistas de controladores?</span><span class="sxs-lookup"><span data-stu-id="d7533-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="d7533-132">Vistas normalmente se devuelven de acciones como un `ViewResult`.</span><span class="sxs-lookup"><span data-stu-id="d7533-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="d7533-133">El método de acción puede crear y devolver un `ViewResult` directamente, pero normalmente si el controlador se hereda de `Controller`, simplemente deberá usar el `View` método auxiliar, como en este ejemplo se muestra:</span><span class="sxs-lookup"><span data-stu-id="d7533-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="d7533-134">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="d7533-134">*HomeController.cs*</span></span>

<span data-ttu-id="d7533-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="d7533-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="d7533-136">El `View` método auxiliar tiene varias sobrecargas para facilitar la devolución de vistas para los desarrolladores de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d7533-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="d7533-137">También puede especificar una vista para devolver, así como un objeto de modelo para pasar a la vista.</span><span class="sxs-lookup"><span data-stu-id="d7533-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="d7533-138">Esta acción devuelve el *About.cshtml* se representa la vista mostrada anteriormente:</span><span class="sxs-lookup"><span data-stu-id="d7533-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![Acerca de la página](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="d7533-140">Detección de vista</span><span class="sxs-lookup"><span data-stu-id="d7533-140">View Discovery</span></span>

<span data-ttu-id="d7533-141">Cuando una acción devuelve una vista, un proceso llamado *detección de vista* tiene lugar.</span><span class="sxs-lookup"><span data-stu-id="d7533-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="d7533-142">Este proceso determina qué archivo de vista se usará.</span><span class="sxs-lookup"><span data-stu-id="d7533-142">This process determines which view file will be used.</span></span> <span data-ttu-id="d7533-143">A menos que se especifica un archivo de vista específicos, el runtime busca una vista específica del controlador, a continuación, buscará en primer nombre de la vista de búsqueda de coincidencias en la *Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d7533-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="d7533-144">Cuando una acción devuelve el `View` método, como `return View();`, el nombre de acción se usa como el nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="d7533-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="d7533-145">Por ejemplo, si esto se le llama desde un método de acción denominado "Índice", sería equivalente a pasar un nombre de vista de "Índice".</span><span class="sxs-lookup"><span data-stu-id="d7533-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="d7533-146">Un nombre de vista se puede pasar explícitamente al método (`return View("SomeView");`).</span><span class="sxs-lookup"><span data-stu-id="d7533-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="d7533-147">En ambos casos, ver detección busca un archivo de vista de búsqueda de coincidencias en:</span><span class="sxs-lookup"><span data-stu-id="d7533-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="d7533-148">Vistas /<ControllerName>/<ViewName>.cshtml</span><span class="sxs-lookup"><span data-stu-id="d7533-148">Views/<ControllerName>/<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="d7533-149">Vistas/compartida/<ViewName>.cshtml</span><span class="sxs-lookup"><span data-stu-id="d7533-149">Views/Shared/<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="d7533-150">Recomendamos seguir la convención de devolver simplemente `View()` de acciones siempre que sea posible, como resultado más flexible y fácil a refactorizar el código.</span><span class="sxs-lookup"><span data-stu-id="d7533-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="d7533-151">Se puede proporcionar una ruta de acceso del archivo de vista, en lugar de un nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="d7533-151">A view file path can be provided, instead of a view name.</span></span> <span data-ttu-id="d7533-152">En este caso, el *.cshtml* extensión debe especificarse como parte de la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="d7533-152">In this case, the *.cshtml* extension must be specified as part of the file path.</span></span> <span data-ttu-id="d7533-153">La ruta de acceso debe ser relativa a la raíz de la aplicación (y, opcionalmente, puede comenzar con "/" o "~ /").</span><span class="sxs-lookup"><span data-stu-id="d7533-153">The path should be relative to the application root (and can optionally start with "/" or "~/").</span></span> <span data-ttu-id="d7533-154">Por ejemplo: `return View("Views/Home/About.cshtml");`.</span><span class="sxs-lookup"><span data-stu-id="d7533-154">For example: `return View("Views/Home/About.cshtml");`</span></span>

> [!NOTE]
> <span data-ttu-id="d7533-155">[Las vistas parciales](partial.md) y [ver componentes](view-components.md) utilizar mecanismos de detección similares (pero no idénticos).</span><span class="sxs-lookup"><span data-stu-id="d7533-155">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="d7533-156">Puede personalizar la convención predeterminada con respecto a dónde se encuentran dentro de la aplicación vistas mediante el comparador `IViewLocationExpander`.</span><span class="sxs-lookup"><span data-stu-id="d7533-156">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="d7533-157">Nombres de vista pueden ser entre mayúsculas y minúsculas según el sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="d7533-157">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="d7533-158">Para la compatibilidad entre los sistemas operativos, siempre Coincidir mayúsculas y minúsculas entre el controlador y los nombres de acción y las carpetas de la vista asociada y nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="d7533-158">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="d7533-159">Pasar datos a las vistas</span><span class="sxs-lookup"><span data-stu-id="d7533-159">Passing Data to Views</span></span>

<span data-ttu-id="d7533-160">Puede pasar datos a vistas que utilizan varios mecanismos.</span><span class="sxs-lookup"><span data-stu-id="d7533-160">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="d7533-161">El enfoque más eficaz consiste en especificar un *modelo* tipo en la vista (suele conocerse como un *viewmodel*, para distinguirlo de los tipos de modelo de dominio de negocio) y, a continuación, pase una instancia de este tipo a la vista de la acción.</span><span class="sxs-lookup"><span data-stu-id="d7533-161">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="d7533-162">Se recomienda que usar un modelo o el modelo de vista para pasar datos a una vista.</span><span class="sxs-lookup"><span data-stu-id="d7533-162">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="d7533-163">Esto permite que la vista aprovechar las ventajas de la comprobación de tipo seguro.</span><span class="sxs-lookup"><span data-stu-id="d7533-163">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="d7533-164">Puede especificar un modelo para una vista mediante el `@model` directiva:</span><span class="sxs-lookup"><span data-stu-id="d7533-164">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="d7533-165">Una vez que se ha especificado un modelo para obtener una vista, puede tener acceso a la instancia que se envían a la vista en un modo fuertemente tipado mediante `@Model` como se indicó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7533-165">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="d7533-166">Para proporcionar una instancia del tipo de modelo a la vista, el controlador lo pasa como un parámetro:</span><span class="sxs-lookup"><span data-stu-id="d7533-166">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

<span data-ttu-id="d7533-167">No hay ninguna restricción sobre los tipos que se pueden proporcionar para una vista como un modelo.</span><span class="sxs-lookup"><span data-stu-id="d7533-167">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="d7533-168">Se recomienda pasar objetos de CLR antiguos sin formato (POCO) ver modelos con el comportamiento de poca o ninguna, por lo que la lógica de negocios se puede encapsular en otra parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7533-168">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="d7533-169">Un ejemplo de este enfoque es la *dirección* viewmodel usado en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="d7533-169">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> <span data-ttu-id="d7533-170">Nada le impide usar las mismas clases como sus tipos de modelo de negocio y los tipos de modelo de presentación.</span><span class="sxs-lookup"><span data-stu-id="d7533-170">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="d7533-171">Sin embargo, consigue que sigan siendo independientes permite a las vistas variar de forma independiente desde el modelo de dominio o la persistencia y puede ofrecer también algunas ventajas de seguridad (para los modelos que se enviarán a los usuarios a la aplicación con [enlace de modelo](../models/model-binding.md)).</span><span class="sxs-lookup"><span data-stu-id="d7533-171">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="d7533-172">Datos débilmente tipadas</span><span class="sxs-lookup"><span data-stu-id="d7533-172">Loosely Typed Data</span></span>

<span data-ttu-id="d7533-173">Además de vistas fuertemente tipadas, todas las vistas tienen acceso a una colección de datos débilmente tipada.</span><span class="sxs-lookup"><span data-stu-id="d7533-173">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="d7533-174">Puede hacer referencia a esta misma colección a través del `ViewData` o `ViewBag` propiedades en los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="d7533-174">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="d7533-175">El `ViewBag` propiedad es un contenedor alrededor de `ViewData` que proporciona una vista dinámica con respecto a esa colección.</span><span class="sxs-lookup"><span data-stu-id="d7533-175">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="d7533-176">No es una colección independiente.</span><span class="sxs-lookup"><span data-stu-id="d7533-176">It is not a separate collection.</span></span>

<span data-ttu-id="d7533-177">`ViewData`es un objeto de diccionario tiene acceso a través de `string` claves.</span><span class="sxs-lookup"><span data-stu-id="d7533-177">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="d7533-178">Puede almacenar y recuperar los objetos en ella, y deberá convertirlos a un tipo específico cuando se extrae de ellos.</span><span class="sxs-lookup"><span data-stu-id="d7533-178">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="d7533-179">Puede usar `ViewData` para pasar datos de un controlador de vistas, así como en vistas (vistas parciales y diseños).</span><span class="sxs-lookup"><span data-stu-id="d7533-179">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="d7533-180">Datos de cadena se pueden almacenar y utilizar directamente, sin necesidad de una conversión de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7533-180">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="d7533-181">Establecer algunos valores para `ViewData` en acción:</span><span class="sxs-lookup"><span data-stu-id="d7533-181">Set some values for `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

<span data-ttu-id="d7533-182">Trabajar con los datos en una vista:</span><span class="sxs-lookup"><span data-stu-id="d7533-182">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="d7533-183">El `ViewBag` objetos proporciona acceso dinámico a los objetos almacenados en `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="d7533-183">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="d7533-184">Esto puede ser más cómodo trabajar con, ya que no requiere conversión.</span><span class="sxs-lookup"><span data-stu-id="d7533-184">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="d7533-185">El mismo ejemplo que antes, con `ViewBag` en lugar de un fuertemente tipado `address` instancia en la vista:</span><span class="sxs-lookup"><span data-stu-id="d7533-185">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="d7533-186">Dado que ambos hacen referencia al mismo subyacente `ViewData` colección, puede mezclar y combinar entre `ViewData` y `ViewBag` al leer y escribir valores, si resulta adecuado.</span><span class="sxs-lookup"><span data-stu-id="d7533-186">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="d7533-187">Vistas dinámicas</span><span class="sxs-lookup"><span data-stu-id="d7533-187">Dynamic Views</span></span>

<span data-ttu-id="d7533-188">Vistas que no se declaran un tipo de modelo, pero tienen una instancia de modelo que se les pasan pueden hacer referencia a esta instancia dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="d7533-188">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="d7533-189">Por ejemplo, si una instancia de `Address` se pasa a una vista que no declara un `@model`, todavía sería podemos hacer referencia a las propiedades de la instancia dinámicamente a medida que se muestra la vista:</span><span class="sxs-lookup"><span data-stu-id="d7533-189">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="d7533-190">Esta característica puede ofrecer cierta flexibilidad, pero no proporciona ninguna protección de compilación ni en IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="d7533-190">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="d7533-191">Si la propiedad no existe, se producirá un error en la página en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7533-191">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="d7533-192">Más características de vista</span><span class="sxs-lookup"><span data-stu-id="d7533-192">More View Features</span></span>

<span data-ttu-id="d7533-193">[Aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) resulta fácil agregar el comportamiento de servidor a las etiquetas HTML existentes, evitando la necesidad de utilizar código personalizado o aplicaciones auxiliares en las vistas.</span><span class="sxs-lookup"><span data-stu-id="d7533-193">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="d7533-194">Aplicaciones auxiliares de etiquetas se aplican como atributos a elementos HTML, que hace caso omiso de editores que no están familiarizados con ellas, lo que permite ver las marcas editarse y representan en una variedad de herramientas.</span><span class="sxs-lookup"><span data-stu-id="d7533-194">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="d7533-195">Aplicaciones auxiliares de etiquetas tienen muchos usos y, en concreto puede realizar [trabajar con formularios](working-with-forms.md) mucho más fácil.</span><span class="sxs-lookup"><span data-stu-id="d7533-195">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="d7533-196">Generar marcado HTML personalizado se puede lograr con muchas aplicaciones auxiliares de HTML integrados y una lógica más compleja de interfaz de usuario (posiblemente con sus propios requisitos de datos) se puede encapsular en [ver componentes](view-components.md).</span><span class="sxs-lookup"><span data-stu-id="d7533-196">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="d7533-197">Componentes de la vista proporcionan la misma separación de aspectos que ofrecen controladores y vistas y pueden eliminar la necesidad de acciones y vistas para tratar con los datos usados por los elementos de interfaz de usuario comunes.</span><span class="sxs-lookup"><span data-stu-id="d7533-197">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="d7533-198">Al igual que muchos otros aspectos de ASP.NET Core, vistas admiten [inyección de dependencia](../../fundamentals/dependency-injection.md), lo que permite a los servicios estén [insertado en vistas](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="d7533-198">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
