---
title: "Diseño"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a><span data-ttu-id="bbe81-102">Diseño</span><span class="sxs-lookup"><span data-stu-id="bbe81-102">Layout</span></span>

<span data-ttu-id="bbe81-103">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bbe81-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bbe81-104">Las vistas a menudo comparten elementos visuales y elementos mediante programación.</span><span class="sxs-lookup"><span data-stu-id="bbe81-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="bbe81-105">En este artículo aprenderá a usar diseños comunes, compartir directivas y ejecutar código común antes de representar vistas en una aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bbe81-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="bbe81-106">Qué es un diseño</span><span class="sxs-lookup"><span data-stu-id="bbe81-106">What is a Layout</span></span>

<span data-ttu-id="bbe81-107">La mayoría de las aplicaciones web tienen un diseño común que ofrece al usuario una experiencia coherente mientras navegan por sus páginas.</span><span class="sxs-lookup"><span data-stu-id="bbe81-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="bbe81-108">El diseño suele incluir elementos comunes en la interfaz de usuario, como el encabezado, los elementos de navegación o de menú y el pie de página de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bbe81-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Ejemplo de diseño de página](layout/_static/page-layout.png)

<span data-ttu-id="bbe81-110">Las estructuras HTML comunes, como los scripts y las hojas de estilos también se usan con frecuencia en muchas páginas dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="bbe81-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="bbe81-111">Todos estos elementos compartidos se pueden definir en un archivo de *diseño*, al que se puede hacer referencia por cualquier vista que se use en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bbe81-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="bbe81-112">Los diseños reducen el código duplicado en las vistas y ayudan a seguir el principio [Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="bbe81-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="bbe81-113">Por convención, el diseño predeterminado para una aplicación ASP.NET se denomina `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="bbe81-114">La plantilla de proyecto de Visual Studio ASP.NET Core MVC incluye este archivo de diseño en la carpeta `Views/Shared`:</span><span class="sxs-lookup"><span data-stu-id="bbe81-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![carpeta de vistas en el Explorador de soluciones](layout/_static/web-project-views.png)

<span data-ttu-id="bbe81-116">Este diseño define una plantilla de nivel superior para las vistas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bbe81-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="bbe81-117">Las aplicaciones no necesitan un diseño y las aplicaciones pueden definir más de un diseño con distintas vistas que especifiquen diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="bbe81-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="bbe81-118">Un ejemplo `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="bbe81-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="bbe81-119">Especificar un diseño</span><span class="sxs-lookup"><span data-stu-id="bbe81-119">Specifying a Layout</span></span>

<span data-ttu-id="bbe81-120">Las vistas de Razor tienen una propiedad `Layout`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="bbe81-121">Las vistas individuales especifican un diseño al configurar esta propiedad:</span><span class="sxs-lookup"><span data-stu-id="bbe81-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="bbe81-122">El diseño especificado puede usar una ruta de acceso completa (ejemplo: `/Views/Shared/_Layout.cshtml`) o un nombre parcial (ejemplo: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="bbe81-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="bbe81-123">Cuando se proporciona un nombre parcial, el motor de vista de Razor buscará el archivo de diseño mediante su proceso de detección estándar.</span><span class="sxs-lookup"><span data-stu-id="bbe81-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="bbe81-124">Primero se busca en la carpeta asociada al controlador y después en la carpeta `Shared`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="bbe81-125">Este proceso de detección es idéntico al usado para detectar [vistas parciales](partial.md).</span><span class="sxs-lookup"><span data-stu-id="bbe81-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="bbe81-126">De forma predeterminada, todos los diseños deben llamar a `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="bbe81-127">Cada vez que se realiza la llamada a `RenderBody`, se representa el contenido de la vista.</span><span class="sxs-lookup"><span data-stu-id="bbe81-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="bbe81-128">Secciones</span><span class="sxs-lookup"><span data-stu-id="bbe81-128">Sections</span></span>

<span data-ttu-id="bbe81-129">Opcionalmente, un diseño puede hacer referencia a una o varias *secciones* mediante una llamada a `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="bbe81-130">Las secciones permiten organizar dónde se deben colocar determinados elementos de la página.</span><span class="sxs-lookup"><span data-stu-id="bbe81-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="bbe81-131">Cada llamada a `RenderSection` puede especificar si esa sección es obligatoria u opcional.</span><span class="sxs-lookup"><span data-stu-id="bbe81-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="bbe81-132">Si no se encuentra una sección obligatoria, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="bbe81-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="bbe81-133">Las vistas individuales especifican el contenido que se va a representar dentro de una sección con la sintaxis `@section` de Razor.</span><span class="sxs-lookup"><span data-stu-id="bbe81-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="bbe81-134">Si una vista define una sección, se debe representar (o se producirá un error).</span><span class="sxs-lookup"><span data-stu-id="bbe81-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="bbe81-135">Ejemplo de definición de `@section` en una vista:</span><span class="sxs-lookup"><span data-stu-id="bbe81-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="bbe81-136">En el código anterior, se agregan scripts de validación a la sección `scripts` en una vista que incluye un formulario.</span><span class="sxs-lookup"><span data-stu-id="bbe81-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="bbe81-137">Es posible que otras vistas de la misma aplicación no necesiten scripts adicionales, por lo que no sería necesario definir una sección de scripts.</span><span class="sxs-lookup"><span data-stu-id="bbe81-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="bbe81-138">Las secciones definidas en una vista solo están disponibles en su página de diseño inmediato.</span><span class="sxs-lookup"><span data-stu-id="bbe81-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="bbe81-139">No se puede hacer referencia a ellas desde líneas de código parcialmente ejecutadas, componentes de vista u otros elementos del sistema de vistas.</span><span class="sxs-lookup"><span data-stu-id="bbe81-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="bbe81-140">Omitir secciones</span><span class="sxs-lookup"><span data-stu-id="bbe81-140">Ignoring sections</span></span>

<span data-ttu-id="bbe81-141">De forma predeterminada, el cuerpo y todas las secciones de una página de contenido deben representarse mediante la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="bbe81-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="bbe81-142">Para cumplir con esto, el motor de vistas de Razor comprueba si el cuerpo y cada sección se han representado.</span><span class="sxs-lookup"><span data-stu-id="bbe81-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="bbe81-143">Para indicar al motor de vistas que pase por alto el cuerpo o las secciones, llame a los métodos `IgnoreBody` y `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="bbe81-144">El cuerpo y todas las secciones de una página de Razor deben representarse o pasarse por alto.</span><span class="sxs-lookup"><span data-stu-id="bbe81-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="bbe81-145">Importar directivas compartidas</span><span class="sxs-lookup"><span data-stu-id="bbe81-145">Importing Shared Directives</span></span>

<span data-ttu-id="bbe81-146">Las vistas pueden usar directivas de Razor para hacer muchas cosas, como importar espacios de nombres o realizar una [inserción de dependencias](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bbe81-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="bbe81-147">Se pueden especificar varias directivas compartidas por muchas vistas en un archivo `_ViewImports.cshtml` común.</span><span class="sxs-lookup"><span data-stu-id="bbe81-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="bbe81-148">El archivo `_ViewImports` es compatible con estas directivas:</span><span class="sxs-lookup"><span data-stu-id="bbe81-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="bbe81-149">El archivo no es compatible con otras características de Razor, como las funciones y las definiciones de sección.</span><span class="sxs-lookup"><span data-stu-id="bbe81-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="bbe81-150">Archivo `_ViewImports.cshtml` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bbe81-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="bbe81-151">El archivo `_ViewImports.cshtml` para una aplicación ASP.NET Core MVC normalmente se coloca en la carpeta `Views`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="bbe81-152">Un archivo `_ViewImports.cshtml` puede colocarse dentro de cualquier carpeta, en cuyo caso solo se aplicará a vistas dentro de esa carpeta y sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="bbe81-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="bbe81-153">Los archivos `_ViewImports` se procesan a partir del nivel de raíz y después para cada carpeta que lleva hasta la ubicación de la propia vista, por lo que la configuración especificada en el nivel de raíz es posible que se invalide en el nivel de carpeta.</span><span class="sxs-lookup"><span data-stu-id="bbe81-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="bbe81-154">Por ejemplo, si un archivo `_ViewImports.cshtml` de nivel de raíz especifica `@model` y `@addTagHelper`, y otro archivo `_ViewImports.cshtml` en la carpeta asociada al controlador de la vista especifica un `@model` diferente y agrega otro `@addTagHelper`, la vista tendrá acceso a las aplicaciones auxiliares de etiquetas y usará el último `@model`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="bbe81-155">Si se ejecutan varios archivos `_ViewImports.cshtml` para una vista, el comportamiento combinado de las directivas incluidas en los archivos `ViewImports.cshtml` será el siguiente:</span><span class="sxs-lookup"><span data-stu-id="bbe81-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="bbe81-156">`@addTagHelper`, `@removeTagHelper`: todos se ejecutan en orden</span><span class="sxs-lookup"><span data-stu-id="bbe81-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="bbe81-157">`@tagHelperPrefix`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="bbe81-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="bbe81-158">`@model`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="bbe81-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="bbe81-159">`@inherits`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="bbe81-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="bbe81-160">`@using`: todos se incluyen y se omiten los duplicados</span><span class="sxs-lookup"><span data-stu-id="bbe81-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="bbe81-161">`@inject`: para cada propiedad, la más cercana a la vista invalida cualquier otra con el mismo nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="bbe81-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="bbe81-162">Ejecutar código antes de cada vista</span><span class="sxs-lookup"><span data-stu-id="bbe81-162">Running Code Before Each View</span></span>

<span data-ttu-id="bbe81-163">Si tiene código que debe ejecutar antes de cada vista, debe colocarlo en el archivo `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="bbe81-164">Por convención, el archivo `_ViewStart.cshtml` se encuentra en la carpeta `Views`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="bbe81-165">Las instrucciones que aparecen en `_ViewStart.cshtml` se ejecutan antes de cada vista completa (no los diseños ni las vistas parciales).</span><span class="sxs-lookup"><span data-stu-id="bbe81-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="bbe81-166">Al igual que [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` tiene una estructura jerárquica.</span><span class="sxs-lookup"><span data-stu-id="bbe81-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="bbe81-167">Si se define un archivo `_ViewStart.cshtml` en la carpeta de vista asociada al controlador, se ejecutará después del que está definido en la raíz de la carpeta `Views` (si existe).</span><span class="sxs-lookup"><span data-stu-id="bbe81-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="bbe81-168">Archivo `_ViewStart.cshtml` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bbe81-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="bbe81-169">El archivo anterior especifica que todas las vistas usarán el diseño `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="bbe81-170">Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` se colocan normalmente en la carpeta `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="bbe81-171">Las versiones de nivel de aplicación de estos archivos deben colocarse directamente en la carpeta `/Views`.</span><span class="sxs-lookup"><span data-stu-id="bbe81-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
