---
title: Diseño en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar diseños comunes, compartir directivas y ejecutar código común antes de representar vistas en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274123"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="4db43-103">Diseño en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4db43-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="4db43-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4db43-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4db43-105">Las vistas a menudo comparten elementos visuales y elementos mediante programación.</span><span class="sxs-lookup"><span data-stu-id="4db43-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="4db43-106">En este artículo aprenderá a usar diseños comunes, compartir directivas y ejecutar código común antes de representar vistas en una aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4db43-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="4db43-107">Qué es un diseño</span><span class="sxs-lookup"><span data-stu-id="4db43-107">What is a Layout</span></span>

<span data-ttu-id="4db43-108">La mayoría de las aplicaciones web tienen un diseño común que ofrece al usuario una experiencia coherente mientras navegan por sus páginas.</span><span class="sxs-lookup"><span data-stu-id="4db43-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="4db43-109">El diseño suele incluir elementos comunes en la interfaz de usuario, como el encabezado, los elementos de navegación o de menú y el pie de página de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4db43-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Ejemplo de diseño de página](layout/_static/page-layout.png)

<span data-ttu-id="4db43-111">Las estructuras HTML comunes, como los scripts y las hojas de estilos también se usan con frecuencia en muchas páginas dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="4db43-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="4db43-112">Todos estos elementos compartidos se pueden definir en un archivo de *diseño*, al que se puede hacer referencia por cualquier vista que se use en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4db43-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="4db43-113">Los diseños reducen el código duplicado en las vistas y ayudan a seguir el principio [Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="4db43-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="4db43-114">Por convención, el diseño predeterminado para una aplicación ASP.NET se denomina `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="4db43-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="4db43-115">La plantilla de proyecto de Visual Studio ASP.NET Core MVC incluye este archivo de diseño en la carpeta `Views/Shared`:</span><span class="sxs-lookup"><span data-stu-id="4db43-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![carpeta de vistas en el Explorador de soluciones](layout/_static/web-project-views.png)

<span data-ttu-id="4db43-117">Este diseño define una plantilla de nivel superior para las vistas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4db43-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="4db43-118">Las aplicaciones no necesitan un diseño y las aplicaciones pueden definir más de un diseño con distintas vistas que especifiquen diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="4db43-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="4db43-119">Un ejemplo `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="4db43-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="4db43-120">Especificar un diseño</span><span class="sxs-lookup"><span data-stu-id="4db43-120">Specifying a Layout</span></span>

<span data-ttu-id="4db43-121">Las vistas de Razor tienen una propiedad `Layout`.</span><span class="sxs-lookup"><span data-stu-id="4db43-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="4db43-122">Las vistas individuales especifican un diseño al configurar esta propiedad:</span><span class="sxs-lookup"><span data-stu-id="4db43-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="4db43-123">El diseño especificado puede usar una ruta de acceso completa (ejemplo: `/Views/Shared/_Layout.cshtml`) o un nombre parcial (ejemplo: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="4db43-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="4db43-124">Cuando se proporciona un nombre parcial, el motor de vista de Razor buscará el archivo de diseño mediante su proceso de detección estándar.</span><span class="sxs-lookup"><span data-stu-id="4db43-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="4db43-125">Primero se busca en la carpeta asociada al controlador y después en la carpeta `Shared`.</span><span class="sxs-lookup"><span data-stu-id="4db43-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="4db43-126">Este proceso de detección es idéntico al usado para detectar [vistas parciales](partial.md).</span><span class="sxs-lookup"><span data-stu-id="4db43-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="4db43-127">De forma predeterminada, todos los diseños deben llamar a `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="4db43-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="4db43-128">Cada vez que se realiza la llamada a `RenderBody`, se representa el contenido de la vista.</span><span class="sxs-lookup"><span data-stu-id="4db43-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="4db43-129">Secciones</span><span class="sxs-lookup"><span data-stu-id="4db43-129">Sections</span></span>

<span data-ttu-id="4db43-130">Opcionalmente, un diseño puede hacer referencia a una o varias *secciones* mediante una llamada a `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="4db43-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="4db43-131">Las secciones permiten organizar dónde se deben colocar determinados elementos de la página.</span><span class="sxs-lookup"><span data-stu-id="4db43-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="4db43-132">Cada llamada a `RenderSection` puede especificar si esa sección es obligatoria u opcional.</span><span class="sxs-lookup"><span data-stu-id="4db43-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="4db43-133">Si no se encuentra una sección obligatoria, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="4db43-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="4db43-134">Las vistas individuales especifican el contenido que se va a representar dentro de una sección con la sintaxis `@section` de Razor.</span><span class="sxs-lookup"><span data-stu-id="4db43-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="4db43-135">Si una vista define una sección, se debe representar (o se producirá un error).</span><span class="sxs-lookup"><span data-stu-id="4db43-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="4db43-136">Ejemplo de definición de `@section` en una vista:</span><span class="sxs-lookup"><span data-stu-id="4db43-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="4db43-137">En el código anterior, se agregan scripts de validación a la sección `scripts` en una vista que incluye un formulario.</span><span class="sxs-lookup"><span data-stu-id="4db43-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="4db43-138">Es posible que otras vistas de la misma aplicación no necesiten scripts adicionales, por lo que no sería necesario definir una sección de scripts.</span><span class="sxs-lookup"><span data-stu-id="4db43-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="4db43-139">Las secciones definidas en una vista solo están disponibles en su página de diseño inmediato.</span><span class="sxs-lookup"><span data-stu-id="4db43-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="4db43-140">No se puede hacer referencia a ellas desde líneas de código parcialmente ejecutadas, componentes de vista u otros elementos del sistema de vistas.</span><span class="sxs-lookup"><span data-stu-id="4db43-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="4db43-141">Omitir secciones</span><span class="sxs-lookup"><span data-stu-id="4db43-141">Ignoring sections</span></span>

<span data-ttu-id="4db43-142">De forma predeterminada, el cuerpo y todas las secciones de una página de contenido deben representarse mediante la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="4db43-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="4db43-143">Para cumplir con esto, el motor de vistas de Razor comprueba si el cuerpo y cada sección se han representado.</span><span class="sxs-lookup"><span data-stu-id="4db43-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="4db43-144">Para indicar al motor de vistas que pase por alto el cuerpo o las secciones, llame a los métodos `IgnoreBody` y `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="4db43-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="4db43-145">El cuerpo y todas las secciones de una página de Razor deben representarse o pasarse por alto.</span><span class="sxs-lookup"><span data-stu-id="4db43-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="4db43-146">Importar directivas compartidas</span><span class="sxs-lookup"><span data-stu-id="4db43-146">Importing Shared Directives</span></span>

<span data-ttu-id="4db43-147">Las vistas pueden usar directivas de Razor para hacer muchas cosas, como importar espacios de nombres o realizar una [inserción de dependencias](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4db43-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="4db43-148">Se pueden especificar varias directivas compartidas por muchas vistas en un archivo `_ViewImports.cshtml` común.</span><span class="sxs-lookup"><span data-stu-id="4db43-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="4db43-149">El archivo `_ViewImports` es compatible con estas directivas:</span><span class="sxs-lookup"><span data-stu-id="4db43-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="4db43-150">El archivo no es compatible con otras características de Razor, como las funciones y las definiciones de sección.</span><span class="sxs-lookup"><span data-stu-id="4db43-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="4db43-151">Archivo `_ViewImports.cshtml` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4db43-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="4db43-152">El archivo `_ViewImports.cshtml` para una aplicación ASP.NET Core MVC normalmente se coloca en la carpeta `Views`.</span><span class="sxs-lookup"><span data-stu-id="4db43-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="4db43-153">Un archivo `_ViewImports.cshtml` puede colocarse dentro de cualquier carpeta, en cuyo caso solo se aplicará a vistas dentro de esa carpeta y sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="4db43-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="4db43-154">Los archivos `_ViewImports` se procesan a partir del nivel de raíz y después para cada carpeta que lleva hasta la ubicación de la propia vista, por lo que la configuración especificada en el nivel de raíz es posible que se invalide en el nivel de carpeta.</span><span class="sxs-lookup"><span data-stu-id="4db43-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="4db43-155">Por ejemplo, si un archivo `_ViewImports.cshtml` de nivel de raíz especifica `@model` y `@addTagHelper`, y otro archivo `_ViewImports.cshtml` en la carpeta asociada al controlador de la vista especifica un `@model` diferente y agrega otro `@addTagHelper`, la vista tendrá acceso a las aplicaciones auxiliares de etiquetas y usará el último `@model`.</span><span class="sxs-lookup"><span data-stu-id="4db43-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="4db43-156">Si se ejecutan varios archivos `_ViewImports.cshtml` para una vista, el comportamiento combinado de las directivas incluidas en los archivos `ViewImports.cshtml` será el siguiente:</span><span class="sxs-lookup"><span data-stu-id="4db43-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="4db43-157">`@addTagHelper`, `@removeTagHelper`: todos se ejecutan en orden</span><span class="sxs-lookup"><span data-stu-id="4db43-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="4db43-158">`@tagHelperPrefix`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="4db43-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4db43-159">`@model`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="4db43-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4db43-160">`@inherits`: el más cercano a la vista invalida los demás</span><span class="sxs-lookup"><span data-stu-id="4db43-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4db43-161">`@using`: todos se incluyen y se omiten los duplicados</span><span class="sxs-lookup"><span data-stu-id="4db43-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="4db43-162">`@inject`: para cada propiedad, la más cercana a la vista invalida cualquier otra con el mismo nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="4db43-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="4db43-163">Ejecutar código antes de cada vista</span><span class="sxs-lookup"><span data-stu-id="4db43-163">Running Code Before Each View</span></span>

<span data-ttu-id="4db43-164">Si tiene código que debe ejecutar antes de cada vista, debe colocarlo en el archivo `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="4db43-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="4db43-165">Por convención, el archivo `_ViewStart.cshtml` se encuentra en la carpeta `Views`.</span><span class="sxs-lookup"><span data-stu-id="4db43-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="4db43-166">Las instrucciones que aparecen en `_ViewStart.cshtml` se ejecutan antes de cada vista completa (no los diseños ni las vistas parciales).</span><span class="sxs-lookup"><span data-stu-id="4db43-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="4db43-167">Al igual que [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` tiene una estructura jerárquica.</span><span class="sxs-lookup"><span data-stu-id="4db43-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="4db43-168">Si se define un archivo `_ViewStart.cshtml` en la carpeta de vista asociada al controlador, se ejecutará después del que está definido en la raíz de la carpeta `Views` (si existe).</span><span class="sxs-lookup"><span data-stu-id="4db43-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="4db43-169">Archivo `_ViewStart.cshtml` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4db43-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="4db43-170">El archivo anterior especifica que todas las vistas usarán el diseño `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="4db43-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="4db43-171">Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` se colocan normalmente en la carpeta `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="4db43-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="4db43-172">Las versiones de nivel de aplicación de estos archivos deben colocarse directamente en la carpeta `/Views`.</span><span class="sxs-lookup"><span data-stu-id="4db43-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
