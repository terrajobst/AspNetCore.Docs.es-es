---
title: "Diseño"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="layout"></a><span data-ttu-id="a4760-103">Diseño</span><span class="sxs-lookup"><span data-stu-id="a4760-103">Layout</span></span>

<span data-ttu-id="a4760-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a4760-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a4760-105">Vistas a menudo compartan elementos visuales y mediante programación.</span><span class="sxs-lookup"><span data-stu-id="a4760-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="a4760-106">En este artículo, aprenderá a usar diseños comunes, compartir directivas y ejecutar el código común antes de la representación de vistas en la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a4760-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="a4760-107">¿Qué es un diseño</span><span class="sxs-lookup"><span data-stu-id="a4760-107">What is a Layout</span></span>

<span data-ttu-id="a4760-108">Mayoría de las aplicaciones web tiene un diseño común que ofrece al usuario una experiencia coherente mientras navegan por las páginas.</span><span class="sxs-lookup"><span data-stu-id="a4760-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="a4760-109">El diseño suele incluir elementos de interfaz de usuario comunes como encabezado de la aplicación, navegación o elementos de menú y pie de página.</span><span class="sxs-lookup"><span data-stu-id="a4760-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Ejemplo de diseño de página](layout/_static/page-layout.png)

<span data-ttu-id="a4760-111">Estructuras de HTML comunes, como scripts y hojas de estilos también frecuente por muchas páginas dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4760-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="a4760-112">Todos estos elementos compartidos se pueden definir en un *diseño* archivo, que, a continuación, hacer referencia a cualquier vista que se utiliza dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4760-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="a4760-113">Diseños de reducen el código duplicado en vistas, que les ayudan a seguir la [principio no repita usted mismo (SECA)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="a4760-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="a4760-114">Por convención, el diseño predeterminado para una aplicación ASP.NET se denomina `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a4760-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="a4760-115">La plantilla de proyecto de Visual Studio ASP.NET Core MVC incluye este archivo de diseño en el `Views/Shared` carpeta:</span><span class="sxs-lookup"><span data-stu-id="a4760-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![carpeta de vistas en el Explorador de soluciones](layout/_static/web-project-views.png)

<span data-ttu-id="a4760-117">Este diseño define una plantilla de nivel superior para las vistas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4760-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="a4760-118">Las aplicaciones no requieren un diseño y aplicaciones pueden definir más de un diseño con distintas vistas especificar diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="a4760-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="a4760-119">Un ejemplo `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="a4760-119">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="a4760-120">Especificación de un diseño</span><span class="sxs-lookup"><span data-stu-id="a4760-120">Specifying a Layout</span></span>

<span data-ttu-id="a4760-121">Vistas de Razor tienen un `Layout` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a4760-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="a4760-122">Vistas individuales especifican un diseño mediante el establecimiento de esta propiedad:</span><span class="sxs-lookup"><span data-stu-id="a4760-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="a4760-123">El diseño especificado puede usar una ruta de acceso completa (ejemplo: `/Views/Shared/_Layout.cshtml`) o un nombre parcial (ejemplo: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="a4760-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="a4760-124">Cuando se proporciona un nombre parcial, el motor de vista Razor buscará el archivo de diseño mediante su proceso de detección estándar.</span><span class="sxs-lookup"><span data-stu-id="a4760-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="a4760-125">La carpeta de controlador asociados se busca en primer lugar, seguido por el `Shared` carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="a4760-126">Este proceso de detección es idéntico al utilizado para detectar [vistas parciales](partial.md).</span><span class="sxs-lookup"><span data-stu-id="a4760-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="a4760-127">De forma predeterminada, deben llamar todos los diseños de `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="a4760-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="a4760-128">Siempre que la llamada a `RenderBody` es colocar, se representará el contenido de la vista.</span><span class="sxs-lookup"><span data-stu-id="a4760-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="a4760-129">Secciones</span><span class="sxs-lookup"><span data-stu-id="a4760-129">Sections</span></span>

<span data-ttu-id="a4760-130">Puede hacer referencia a un diseño, opcionalmente, uno o varios *secciones*, mediante una llamada a `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="a4760-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="a4760-131">Secciones proporcionan una manera de organizar dónde se deben colocar determinados elementos de la página.</span><span class="sxs-lookup"><span data-stu-id="a4760-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="a4760-132">Cada llamada a `RenderSection` puede especificar si esa sección es obligatorio u opcional.</span><span class="sxs-lookup"><span data-stu-id="a4760-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="a4760-133">Si no se encuentra una sección obligatoria, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="a4760-133">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="a4760-134">Vistas individuales especifican el contenido que se representará dentro de una sección con la `@section` sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="a4760-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="a4760-135">Si una vista define una sección, se debe representar (o se producirá un error).</span><span class="sxs-lookup"><span data-stu-id="a4760-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="a4760-136">Un ejemplo `@section` definición en una vista:</span><span class="sxs-lookup"><span data-stu-id="a4760-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="a4760-137">En el código anterior, las secuencias de comandos de validación se agregan a la `scripts` sección en una vista que incluya un formulario.</span><span class="sxs-lookup"><span data-stu-id="a4760-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="a4760-138">Otras vistas de la misma aplicación podrían no requerir scripts adicionales y, por lo que no sería necesario definir una sección de secuencias de comandos.</span><span class="sxs-lookup"><span data-stu-id="a4760-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="a4760-139">Las secciones definidas en una vista solo están disponibles en su página de diseño inmediato.</span><span class="sxs-lookup"><span data-stu-id="a4760-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="a4760-140">Sí No se pueden hacer referencia desde parciales, componentes de la vista u otras partes del sistema de la vista.</span><span class="sxs-lookup"><span data-stu-id="a4760-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="a4760-141">Omitir secciones</span><span class="sxs-lookup"><span data-stu-id="a4760-141">Ignoring sections</span></span>

<span data-ttu-id="a4760-142">De forma predeterminada, el cuerpo y todas las secciones en una página de contenido deben todas se representan mediante la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="a4760-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="a4760-143">El motor de vista Razor exige al realizar un seguimiento si el cuerpo y cada sección se han procesado.</span><span class="sxs-lookup"><span data-stu-id="a4760-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="a4760-144">Para indicar al motor de vista para pasar por alto el cuerpo o secciones, llame a la `IgnoreBody` y `IgnoreSection` métodos.</span><span class="sxs-lookup"><span data-stu-id="a4760-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="a4760-145">El cuerpo y todas las secciones en una página de Razor deben ser representan o pasa por alto.</span><span class="sxs-lookup"><span data-stu-id="a4760-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="a4760-146">Importar directivas compartidas</span><span class="sxs-lookup"><span data-stu-id="a4760-146">Importing Shared Directives</span></span>

<span data-ttu-id="a4760-147">Vistas pueden usar las directivas de Razor para hacer muchas cosas, como la importación de espacios de nombres o realizar [inyección de dependencia](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="a4760-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="a4760-148">Se pueden especificar directivas compartidas muchas vistas en común `_ViewImports.cshtml` archivo.</span><span class="sxs-lookup"><span data-stu-id="a4760-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="a4760-149">El `_ViewImports` archivo es compatible con las siguientes directivas:</span><span class="sxs-lookup"><span data-stu-id="a4760-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="a4760-150">El archivo no es compatible con otras características de Razor, como las funciones y las definiciones de sección.</span><span class="sxs-lookup"><span data-stu-id="a4760-150">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="a4760-151">Un ejemplo de `_ViewImports.cshtml` archivo:</span><span class="sxs-lookup"><span data-stu-id="a4760-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="a4760-152">El `_ViewImports.cshtml` de archivos para una aplicación de MVC de ASP.NET Core normalmente se coloca en el `Views` carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="a4760-153">Un `_ViewImports.cshtml` archivo puede colocarse dentro de cualquier carpeta, en el que caso solo se aplicarán a vistas dentro de esa carpeta y sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="a4760-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="a4760-154">`_ViewImports`se procesan los archivos a partir del nivel raíz, y, a continuación, para cada carpeta llevan a la ubicación de la propia vista, por lo que la configuración especificada en el nivel de raíz se pueden invalidar en el nivel de carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="a4760-155">Por ejemplo, si un nivel de raíz `_ViewImports.cshtml` archivo especifica `@model` y `@addTagHelper`y otro `_ViewImports.cshtml` especifica otro archivo en la carpeta controlador de asociados de la vista `@model` y agrega otro `@addTagHelper`, la vista tendrá acceso a las aplicaciones auxiliares de etiquetas y usará el último `@model`.</span><span class="sxs-lookup"><span data-stu-id="a4760-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="a4760-156">Si hay varios `_ViewImports.cshtml` archivos se ejecutan para una vista, combina el comportamiento de las directivas que se incluyen en el `ViewImports.cshtml` archivos será la siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4760-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="a4760-157">`@addTagHelper`, `@removeTagHelper`: ejecutar, en orden</span><span class="sxs-lookup"><span data-stu-id="a4760-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="a4760-158">`@tagHelperPrefix`: lo más cercano a la vista invalida cualquier otra</span><span class="sxs-lookup"><span data-stu-id="a4760-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="a4760-159">`@model`: lo más cercano a la vista invalida cualquier otra</span><span class="sxs-lookup"><span data-stu-id="a4760-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="a4760-160">`@inherits`: lo más cercano a la vista invalida cualquier otra</span><span class="sxs-lookup"><span data-stu-id="a4760-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="a4760-161">`@using`: todos se incluyen; se omiten los duplicados</span><span class="sxs-lookup"><span data-stu-id="a4760-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="a4760-162">`@inject`: para cada propiedad, lo más cercano a la vista invalida cualquier otra con el mismo nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="a4760-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="a4760-163">Ejecutar código antes de cada vista</span><span class="sxs-lookup"><span data-stu-id="a4760-163">Running Code Before Each View</span></span>

<span data-ttu-id="a4760-164">Si tiene código deba ejecutar antes de cada vista, esto se debe colocar en el `_ViewStart.cshtml` archivo.</span><span class="sxs-lookup"><span data-stu-id="a4760-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="a4760-165">Por convención, el `_ViewStart.cshtml` archivo se encuentra en la `Views` carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="a4760-166">Las instrucciones que aparecen en `_ViewStart.cshtml` se ejecutan antes de cada vista completa (no los diseños y vistas no parciales).</span><span class="sxs-lookup"><span data-stu-id="a4760-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="a4760-167">Al igual que [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` tiene una estructura jerárquica.</span><span class="sxs-lookup"><span data-stu-id="a4760-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="a4760-168">Si un `_ViewStart.cshtml` archivo se define en la carpeta de la vista asociada de controlador, se ejecutará después de la definida en la raíz de la `Views` carpeta (si existe).</span><span class="sxs-lookup"><span data-stu-id="a4760-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="a4760-169">Un ejemplo de `_ViewStart.cshtml` archivo:</span><span class="sxs-lookup"><span data-stu-id="a4760-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="a4760-170">El archivo anterior especifica que todas las vistas van a utilizar el `_Layout.cshtml` diseño.</span><span class="sxs-lookup"><span data-stu-id="a4760-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="a4760-171">Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` normalmente se colocan en la `/Views/Shared` carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="a4760-172">Las versiones de nivel de aplicación de estos archivos deben colocarse directamente en el `/Views` carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4760-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
