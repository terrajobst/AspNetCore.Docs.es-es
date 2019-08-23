---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545807"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="a4732-103">Áreas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4732-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="a4732-104">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4732-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4732-105">Las áreas son una característica de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).</span><span class="sxs-lookup"><span data-stu-id="a4732-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="a4732-106">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`, o bien a un elemento `page` de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a4732-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="a4732-107">Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core en grupos funcionales más pequeños, cada uno con su propio conjunto de Razor Pages, controladores, vistas y modelos.</span><span class="sxs-lookup"><span data-stu-id="a4732-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="a4732-108">Un área es en realidad una estructura dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="a4732-109">En un proyecto web ASP.NET Core, los componentes lógicos como Páginas, Modelo, Vista y Controlador se mantienen en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="a4732-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="a4732-110">El runtime de ASP.NET Core usa convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="a4732-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="a4732-111">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="a4732-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="a4732-112">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como las de finalización de la compra, facturación y búsqueda.</span><span class="sxs-lookup"><span data-stu-id="a4732-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="a4732-113">Cada una de estas unidades tiene un área propia para controlar vistas, controladores, Razor Pages y modelos.</span><span class="sxs-lookup"><span data-stu-id="a4732-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="a4732-114">Considere el uso de áreas en un proyecto en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4732-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="a4732-115">La aplicación está formada por varios componentes funcionales generales que se pueden separar de forma lógica.</span><span class="sxs-lookup"><span data-stu-id="a4732-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="a4732-116">Le interesa dividir la aplicación para que se pueda trabajar en cada área funcional de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="a4732-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="a4732-117">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a4732-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a4732-118">En el ejemplo de descarga se proporciona una aplicación básica para probar las áreas.</span><span class="sxs-lookup"><span data-stu-id="a4732-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="a4732-119">Si va a usar Razor Pages, consulte [Áreas con Razor Pages](#areas-with-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="a4732-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="a4732-120">Áreas para controladores con vistas</span><span class="sxs-lookup"><span data-stu-id="a4732-120">Areas for controllers with views</span></span>

<span data-ttu-id="a4732-121">Una aplicación web ASP.NET Core típica en la que se usen áreas, controladores y vistas contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4732-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="a4732-122">Una [estructura de carpetas de área](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="a4732-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="a4732-123">Controladores decorados con el atributo [&lbrack;Area&rbrack;](#attribute) para asociar el controlador con el área:</span><span class="sxs-lookup"><span data-stu-id="a4732-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="a4732-124">La [ruta de área agregada al inicio](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="a4732-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="a4732-125">Estructura de carpetas de área</span><span class="sxs-lookup"><span data-stu-id="a4732-125">Area folder structure</span></span>

<span data-ttu-id="a4732-126">Considere una aplicación que tiene dos grupos lógicos, *Productos* y *Servicios*.</span><span class="sxs-lookup"><span data-stu-id="a4732-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="a4732-127">Mediante las áreas, la estructura de carpetas sería similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4732-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="a4732-128">Nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="a4732-128">Project name</span></span>
  * <span data-ttu-id="a4732-129">Áreas</span><span class="sxs-lookup"><span data-stu-id="a4732-129">Areas</span></span>
    * <span data-ttu-id="a4732-130">Productos</span><span class="sxs-lookup"><span data-stu-id="a4732-130">Products</span></span>
      * <span data-ttu-id="a4732-131">Controladores</span><span class="sxs-lookup"><span data-stu-id="a4732-131">Controllers</span></span>
        * <span data-ttu-id="a4732-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="a4732-132">HomeController.cs</span></span>
        * <span data-ttu-id="a4732-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="a4732-133">ManageController.cs</span></span>
      * <span data-ttu-id="a4732-134">Vistas</span><span class="sxs-lookup"><span data-stu-id="a4732-134">Views</span></span>
        * <span data-ttu-id="a4732-135">Página principal</span><span class="sxs-lookup"><span data-stu-id="a4732-135">Home</span></span>
          * <span data-ttu-id="a4732-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-136">Index.cshtml</span></span>
        * <span data-ttu-id="a4732-137">Administrar</span><span class="sxs-lookup"><span data-stu-id="a4732-137">Manage</span></span>
          * <span data-ttu-id="a4732-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-138">Index.cshtml</span></span>
          * <span data-ttu-id="a4732-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-139">About.cshtml</span></span>
    * <span data-ttu-id="a4732-140">Servicios</span><span class="sxs-lookup"><span data-stu-id="a4732-140">Services</span></span>
      * <span data-ttu-id="a4732-141">Controladores</span><span class="sxs-lookup"><span data-stu-id="a4732-141">Controllers</span></span>
        * <span data-ttu-id="a4732-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="a4732-142">HomeController.cs</span></span>
      * <span data-ttu-id="a4732-143">Vistas</span><span class="sxs-lookup"><span data-stu-id="a4732-143">Views</span></span>
        * <span data-ttu-id="a4732-144">Página principal</span><span class="sxs-lookup"><span data-stu-id="a4732-144">Home</span></span>
          * <span data-ttu-id="a4732-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-145">Index.cshtml</span></span>

<span data-ttu-id="a4732-146">Aunque el diseño anterior es típico cuando se usan áreas, solo los archivos de vista tienen que usar esta estructura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="a4732-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="a4732-147">La detección de vista busca un archivo de vista de área coincidente en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4732-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="a4732-148">La ubicación de las carpetas que no son de vista, como *Controllers* y *Models*, **no** importa.</span><span class="sxs-lookup"><span data-stu-id="a4732-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="a4732-149">Por ejemplo, las carpetas *Controllers* y *Models* no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="a4732-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="a4732-150">El contenido de *Controllers* y *Models* es código que se compila en un archivo .dll.</span><span class="sxs-lookup"><span data-stu-id="a4732-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="a4732-151">El contenido de *Views* no se compila hasta que se haya realizado una solicitud a esa vista.</span><span class="sxs-lookup"><span data-stu-id="a4732-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="a4732-152">Asociación del controlador a un área</span><span class="sxs-lookup"><span data-stu-id="a4732-152">Associate the controller with an Area</span></span>

<span data-ttu-id="a4732-153">Los controladores de área se designan con el atributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="a4732-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="a4732-154">Adición de la ruta de área</span><span class="sxs-lookup"><span data-stu-id="a4732-154">Add Area route</span></span>

<span data-ttu-id="a4732-155">Las rutas de área normalmente usan el enrutamiento convencional en lugar del enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a4732-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="a4732-156">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="a4732-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="a4732-157">En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="a4732-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="a4732-158">Se puede `{area:...}` usar como un token en las plantillas de ruta, si el espacio de direcciones URL es uniforme en todas las áreas:</span><span class="sxs-lookup"><span data-stu-id="a4732-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="a4732-159">En el código anterior, `exists` aplica la restricción de que la ruta debe coincidir con un área.</span><span class="sxs-lookup"><span data-stu-id="a4732-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="a4732-160">El uso de `{area:...}` es el mecanismo menos complicado para agregar enrutamiento a las áreas.</span><span class="sxs-lookup"><span data-stu-id="a4732-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="a4732-161">En el código siguiente se usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> para crear dos rutas de área con nombre:</span><span class="sxs-lookup"><span data-stu-id="a4732-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="a4732-162">Cuando use `MapAreaRoute` con ASP.NET Core 2.2, vea [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="a4732-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="a4732-163">Para más información, vea [Enrutamiento de áreas](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="a4732-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="a4732-164">Generación de vínculos con áreas de MVC</span><span class="sxs-lookup"><span data-stu-id="a4732-164">Link generation with MVC areas</span></span>

<span data-ttu-id="a4732-165">En el código siguiente de la [descarga de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) se muestra la generación de vínculos con el área especificada:</span><span class="sxs-lookup"><span data-stu-id="a4732-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="a4732-166">Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="a4732-167">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="a4732-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="a4732-168">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="a4732-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="a4732-169">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página en el mismo área y controlador.</span><span class="sxs-lookup"><span data-stu-id="a4732-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="a4732-170">Cuando no se especifica el área o el controlador, el enrutamiento depende de los valores de *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="a4732-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="a4732-171">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="a4732-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="a4732-172">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="a4732-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="a4732-173">Para más información, vea [Enrutar a acciones de controlador](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="a4732-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="a4732-174">Diseño Compartido para áreas con el archivo _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="a4732-175">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="a4732-176">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="a4732-176">_ViewImports.cshtml</span></span>

<span data-ttu-id="a4732-177">En su ubicación estándar, */Views/_ViewImports.cshtml* no se aplica a las áreas.</span><span class="sxs-lookup"><span data-stu-id="a4732-177">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="a4732-178">Para usar [Asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) comunes, `@using` o `@inject` en la zona, asegúrese de que un archivo *_ViewImports.cshtml* adecuado [se aplica a las vistas del área](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="a4732-178">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="a4732-179">Si quiere el mismo comportamiento en todas las vistas, mueva */Views/_ViewImports.cshtml* a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-179">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="a4732-180">Cambio de la carpeta de área predeterminada donde se almacenan las vistas</span><span class="sxs-lookup"><span data-stu-id="a4732-180">Change default area folder where views are stored</span></span>

<span data-ttu-id="a4732-181">En el código siguiente se cambia la carpeta de área predeterminada de `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="a4732-181">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="a4732-182">Áreas con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a4732-182">Areas with Razor Pages</span></span>

<span data-ttu-id="a4732-183">Las áreas con Razor Pages requieren una carpeta *Areas/<area name>/Pages* en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-183">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="a4732-184">La siguiente estructura de carpetas se usa con la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span><span class="sxs-lookup"><span data-stu-id="a4732-184">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="a4732-185">Nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="a4732-185">Project name</span></span>
  * <span data-ttu-id="a4732-186">Áreas</span><span class="sxs-lookup"><span data-stu-id="a4732-186">Areas</span></span>
    * <span data-ttu-id="a4732-187">Productos</span><span class="sxs-lookup"><span data-stu-id="a4732-187">Products</span></span>
      * <span data-ttu-id="a4732-188">Páginas</span><span class="sxs-lookup"><span data-stu-id="a4732-188">Pages</span></span>
        * <span data-ttu-id="a4732-189">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="a4732-189">_ViewImports</span></span>
        * <span data-ttu-id="a4732-190">Acerca de</span><span class="sxs-lookup"><span data-stu-id="a4732-190">About</span></span>
        * <span data-ttu-id="a4732-191">Índice</span><span class="sxs-lookup"><span data-stu-id="a4732-191">Index</span></span>
    * <span data-ttu-id="a4732-192">Servicios</span><span class="sxs-lookup"><span data-stu-id="a4732-192">Services</span></span>
      * <span data-ttu-id="a4732-193">Páginas</span><span class="sxs-lookup"><span data-stu-id="a4732-193">Pages</span></span>
        * <span data-ttu-id="a4732-194">Administrar</span><span class="sxs-lookup"><span data-stu-id="a4732-194">Manage</span></span>
          * <span data-ttu-id="a4732-195">Acerca de</span><span class="sxs-lookup"><span data-stu-id="a4732-195">About</span></span>
          * <span data-ttu-id="a4732-196">Índice</span><span class="sxs-lookup"><span data-stu-id="a4732-196">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="a4732-197">Generación de vínculos con Razor Pages y áreas</span><span class="sxs-lookup"><span data-stu-id="a4732-197">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="a4732-198">El código siguiente de la [descarga de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) muestra la generación de vínculos con el área especificada (por ejemplo, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="a4732-198">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="a4732-199">Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-199">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="a4732-200">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="a4732-200">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="a4732-201">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="a4732-201">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="a4732-202">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página de la misma área.</span><span class="sxs-lookup"><span data-stu-id="a4732-202">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="a4732-203">Cuando no se especifica el área, el enrutamiento depende de los valores *ambient*.</span><span class="sxs-lookup"><span data-stu-id="a4732-203">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="a4732-204">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="a4732-204">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="a4732-205">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="a4732-205">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="a4732-206">Por ejemplo, tenga en cuenta los vínculos generados desde el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4732-206">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="a4732-207">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="a4732-207">For the preceding code:</span></span>

* <span data-ttu-id="a4732-208">El vínculo generado a partir de `<a asp-page="/Manage/About">` es correcto solo cuando la última solicitud fue de una página del área `Services`.</span><span class="sxs-lookup"><span data-stu-id="a4732-208">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="a4732-209">Por ejemplo, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="a4732-209">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="a4732-210">El vínculo generado a partir de `<a asp-page="/About">` es correcto solo cuando la última solicitud fue de una página de `/Home`.</span><span class="sxs-lookup"><span data-stu-id="a4732-210">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="a4732-211">El código está tomado de la [descarga de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="a4732-211">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="a4732-212">Importación del espacio de nombres y los asistentes de etiquetas con el archivo _ViewImports</span><span class="sxs-lookup"><span data-stu-id="a4732-212">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="a4732-213">Se puede agregar un archivo *_ViewImports.cshtml* a la carpeta *Pages* de cada área para importar el espacio de nombres y los asistentes de etiquetas en cada página de Razor de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="a4732-213">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="a4732-214">Tenga en cuenta el área *Services* del código de ejemplo, que no contiene un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4732-214">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a4732-215">El marcado siguiente muestra la página de Razor */Services/Manage/About*:</span><span class="sxs-lookup"><span data-stu-id="a4732-215">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="a4732-216">En el marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="a4732-216">In the preceding markup:</span></span>

* <span data-ttu-id="a4732-217">El nombre de dominio completo debe usarse para especificar el modelo (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="a4732-217">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="a4732-218">Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) se habilitan mediante `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="a4732-218">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="a4732-219">En la descarga de ejemplo, el área Products contiene el siguiente archivo *_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4732-219">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="a4732-220">El siguiente marcado muestra la página de Razor */Products/About*:</span><span class="sxs-lookup"><span data-stu-id="a4732-220">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="a4732-221">En el archivo anterior, el espacio de nombres y la directiva `@addTagHelper` se importan en el archivo mediante el archivo *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a4732-221">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="a4732-222">Para más información, consulte [Administración del ámbito de los asistentes de etiquetas](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) y [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="a4732-222">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="a4732-223">Diseño compartido para áreas de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a4732-223">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="a4732-224">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4732-224">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="a4732-225">Publicación de áreas</span><span class="sxs-lookup"><span data-stu-id="a4732-225">Publishing Areas</span></span>

<span data-ttu-id="a4732-226">Todos los archivos \*.cshtml y los archivos que formen parte del directorio *wwwroot* se publicarán como resultados si `<Project Sdk="Microsoft.NET.Sdk.Web">` está incluido en el archivo \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="a4732-226">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
