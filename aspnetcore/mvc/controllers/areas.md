---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 03/21/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 8859bc52416ff657036198c73f63b8b0a0201e11
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80625927"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="6294f-103">Áreas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6294f-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="6294f-104">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6294f-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6294f-105">Las áreas son una característica ASP.NET que se utiliza para organizar la funcionalidad relacionada en un grupo como un independiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="6294f-106">Espacio de nombres para el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="6294f-106">Namespace for routing.</span></span>
* <span data-ttu-id="6294f-107">Estructura de carpetas para vistas y razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6294f-107">Folder structure for views and Razor Pages.</span></span>

<span data-ttu-id="6294f-108">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`, o bien a un elemento `page` de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="6294f-108">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="6294f-109">Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core en grupos funcionales más pequeños, cada uno con su propio conjunto de Razor Pages, controladores, vistas y modelos.</span><span class="sxs-lookup"><span data-stu-id="6294f-109">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="6294f-110">Un área es en realidad una estructura dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-110">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="6294f-111">En un proyecto web ASP.NET Core, los componentes lógicos como Páginas, Modelo, Vista y Controlador se mantienen en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="6294f-111">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="6294f-112">El runtime de ASP.NET Core usa convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="6294f-112">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="6294f-113">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="6294f-113">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="6294f-114">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como las de finalización de la compra, facturación y búsqueda.</span><span class="sxs-lookup"><span data-stu-id="6294f-114">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="6294f-115">Cada una de estas unidades tiene un área propia para controlar vistas, controladores, Razor Pages y modelos.</span><span class="sxs-lookup"><span data-stu-id="6294f-115">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="6294f-116">Considere el uso de áreas en un proyecto en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6294f-116">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="6294f-117">La aplicación está formada por varios componentes funcionales generales que se pueden separar de forma lógica.</span><span class="sxs-lookup"><span data-stu-id="6294f-117">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="6294f-118">Le interesa dividir la aplicación para que se pueda trabajar en cada área funcional de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="6294f-118">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="6294f-119">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6294f-119">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6294f-120">En el ejemplo de descarga se proporciona una aplicación básica para probar las áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-120">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="6294f-121">Si va a usar Razor Pages, consulte [Áreas con Razor Pages](#areas-with-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="6294f-121">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="6294f-122">Áreas para controladores con vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-122">Areas for controllers with views</span></span>

<span data-ttu-id="6294f-123">Una aplicación web ASP.NET Core típica en la que se usen áreas, controladores y vistas contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-123">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="6294f-124">Una [estructura de carpetas de área](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="6294f-124">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="6294f-125">Controladores [`[Area]`](#attribute) con el atributo para asociar el regulador con el área:</span><span class="sxs-lookup"><span data-stu-id="6294f-125">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/31samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="6294f-126">La ruta de [área añadida al inicio:](#add-area-route)</span><span class="sxs-lookup"><span data-stu-id="6294f-126">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/31samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="6294f-127">Estructura de carpetas de área</span><span class="sxs-lookup"><span data-stu-id="6294f-127">Area folder structure</span></span>

<span data-ttu-id="6294f-128">Considere una aplicación que tiene dos grupos lógicos, *Productos* y *Servicios*.</span><span class="sxs-lookup"><span data-stu-id="6294f-128">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="6294f-129">Mediante las áreas, la estructura de carpetas sería similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-129">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="6294f-130">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="6294f-130">Project name</span></span>
  * <span data-ttu-id="6294f-131">Áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-131">Areas</span></span>
    * <span data-ttu-id="6294f-132">Productos</span><span class="sxs-lookup"><span data-stu-id="6294f-132">Products</span></span>
      * <span data-ttu-id="6294f-133">Controladores</span><span class="sxs-lookup"><span data-stu-id="6294f-133">Controllers</span></span>
        * <span data-ttu-id="6294f-134">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-134">HomeController.cs</span></span>
        * <span data-ttu-id="6294f-135">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-135">ManageController.cs</span></span>
      * <span data-ttu-id="6294f-136">Vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-136">Views</span></span>
        * <span data-ttu-id="6294f-137">Inicio</span><span class="sxs-lookup"><span data-stu-id="6294f-137">Home</span></span>
          * <span data-ttu-id="6294f-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-138">Index.cshtml</span></span>
        * <span data-ttu-id="6294f-139">Administrar</span><span class="sxs-lookup"><span data-stu-id="6294f-139">Manage</span></span>
          * <span data-ttu-id="6294f-140">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-140">Index.cshtml</span></span>
          * <span data-ttu-id="6294f-141">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-141">About.cshtml</span></span>
    * <span data-ttu-id="6294f-142">Servicios</span><span class="sxs-lookup"><span data-stu-id="6294f-142">Services</span></span>
      * <span data-ttu-id="6294f-143">Controladores</span><span class="sxs-lookup"><span data-stu-id="6294f-143">Controllers</span></span>
        * <span data-ttu-id="6294f-144">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-144">HomeController.cs</span></span>
      * <span data-ttu-id="6294f-145">Vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-145">Views</span></span>
        * <span data-ttu-id="6294f-146">Inicio</span><span class="sxs-lookup"><span data-stu-id="6294f-146">Home</span></span>
          * <span data-ttu-id="6294f-147">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-147">Index.cshtml</span></span>

<span data-ttu-id="6294f-148">Aunque el diseño anterior es típico cuando se usan áreas, solo los archivos de vista tienen que usar esta estructura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="6294f-148">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="6294f-149">La detección de vista busca un archivo de vista de área coincidente en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-149">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="6294f-150">Asociación del controlador a un área</span><span class="sxs-lookup"><span data-stu-id="6294f-150">Associate the controller with an Area</span></span>

<span data-ttu-id="6294f-151">Los controladores de [ &lbrack;&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) área se designan con el atributo Area:</span><span class="sxs-lookup"><span data-stu-id="6294f-151">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="6294f-152">Adición de la ruta de área</span><span class="sxs-lookup"><span data-stu-id="6294f-152">Add Area route</span></span>

<span data-ttu-id="6294f-153">Las rutas de área suelen utilizar [enrutamiento convencional](xref:mvc/controllers/routing#cr) en lugar de enrutamiento de [atributos.](xref:mvc/controllers/routing#ar)</span><span class="sxs-lookup"><span data-stu-id="6294f-153">Area routes typically use  [conventional routing](xref:mvc/controllers/routing#cr) rather than [attribute routing](xref:mvc/controllers/routing#ar).</span></span> <span data-ttu-id="6294f-154">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="6294f-154">Conventional routing is order-dependent.</span></span> <span data-ttu-id="6294f-155">En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="6294f-155">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="6294f-156">Se puede `{area:...}` usar como un token en las plantillas de ruta, si el espacio de direcciones URL es uniforme en todas las áreas:</span><span class="sxs-lookup"><span data-stu-id="6294f-156">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Startup.cs?name=snippet&highlight=21-23)]

<span data-ttu-id="6294f-157">En el código anterior, `exists` aplica la restricción de que la ruta debe coincidir con un área.</span><span class="sxs-lookup"><span data-stu-id="6294f-157">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="6294f-158">Uso `{area:...}` `MapControllerRoute`con :</span><span class="sxs-lookup"><span data-stu-id="6294f-158">Using `{area:...}` with `MapControllerRoute`:</span></span>

* <span data-ttu-id="6294f-159">Es el mecanismo menos complicado para agregar enrutamiento a áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-159">Is the least complicated mechanism to adding routing to areas.</span></span>
* <span data-ttu-id="6294f-160">Coincide con todos `[Area("Area name")]` los controladores con el atributo.</span><span class="sxs-lookup"><span data-stu-id="6294f-160">Matches all controllers with the `[Area("Area name")]` attribute.</span></span>

<span data-ttu-id="6294f-161">En el código siguiente se usa <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> para crear dos rutas de área con nombre:</span><span class="sxs-lookup"><span data-stu-id="6294f-161">The following code uses <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/31samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=21-29)]

<span data-ttu-id="6294f-162">Para más información, vea [Enrutamiento de áreas](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="6294f-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="6294f-163">Generación de vínculos con áreas de MVC</span><span class="sxs-lookup"><span data-stu-id="6294f-163">Link generation with MVC areas</span></span>

<span data-ttu-id="6294f-164">En el código siguiente de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) se muestra la generación de vínculos con el área especificada:</span><span class="sxs-lookup"><span data-stu-id="6294f-164">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/31samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="6294f-165">La descarga de ejemplo incluye una [vista parcial](xref:mvc/views/partial) que contiene:</span><span class="sxs-lookup"><span data-stu-id="6294f-165">The sample download includes a [partial view](xref:mvc/views/partial) that contains:</span></span>

* <span data-ttu-id="6294f-166">Los vínculos anteriores.</span><span class="sxs-lookup"><span data-stu-id="6294f-166">The preceding links.</span></span>
* <span data-ttu-id="6294f-167">No se especifican vínculos similares a la excepción `area` anterior.</span><span class="sxs-lookup"><span data-stu-id="6294f-167">Links similar to the preceding except `area` is not specified.</span></span>

<span data-ttu-id="6294f-168">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="6294f-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="6294f-169">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página en el mismo área y controlador.</span><span class="sxs-lookup"><span data-stu-id="6294f-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="6294f-170">Cuando no se especifica el área o el controlador, el enrutamiento depende de los valores de [ambiente](xref:mvc/controllers/routing#ambient).</span><span class="sxs-lookup"><span data-stu-id="6294f-170">When the area or controller is not specified, routing depends on the [ambient](xref:mvc/controllers/routing#ambient) values.</span></span> <span data-ttu-id="6294f-171">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="6294f-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6294f-172">En muchos casos para la aplicación de ejemplo, el uso de los valores ambientales genera vínculos incorrectos con el marcado que no especifica el área.</span><span class="sxs-lookup"><span data-stu-id="6294f-172">In many cases for the sample app, using the ambient values generates incorrect links with the markup that doesn't specify the area.</span></span>

<span data-ttu-id="6294f-173">Para más información, vea [Enrutar a acciones de controlador](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="6294f-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="6294f-174">Diseño Compartido para áreas con el archivo _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="6294f-175">Para compartir un diseño común para toda la aplicación, mantenga el *_ViewStart.cshtml* en la carpeta raíz de la [aplicación.](#arf)</span><span class="sxs-lookup"><span data-stu-id="6294f-175">To share a common layout for the entire app, keep the *_ViewStart.cshtml* in the [application root folder](#arf).</span></span> <span data-ttu-id="6294f-176">Para obtener más información, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="6294f-176">For more information, see <xref:mvc/views/layout></span></span>

<a name="arf"></a>

### <a name="application-root-folder"></a><span data-ttu-id="6294f-177">Carpeta raíz de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6294f-177">Application root folder</span></span>

<span data-ttu-id="6294f-178">La carpeta raíz de la aplicación es la carpeta que contiene *Startup.cs* en la aplicación web creada con las plantillas ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6294f-178">The application root folder is the folder containing *Startup.cs* in web app created with the ASP.NET Core templates.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="6294f-179">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-179">_ViewImports.cshtml</span></span>

 <span data-ttu-id="6294f-180">*/Views/_ViewImports.cshtml*, para MVC y */Pages/_ViewImports.cshtml* para Razor Pages, no se importa a las vistas en áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-180">*/Views/_ViewImports.cshtml*, for MVC, and */Pages/_ViewImports.cshtml* for Razor Pages, is not imported to views in areas.</span></span> <span data-ttu-id="6294f-181">Utilice uno de los siguientes enfoques para proporcionar importaciones de vistas a todas las vistas:</span><span class="sxs-lookup"><span data-stu-id="6294f-181">Use one of the following approaches to provide view imports to all views:</span></span>

* <span data-ttu-id="6294f-182">Agregue *_ViewImports.cshtml* a la carpeta raíz de la [aplicación.](#arf)</span><span class="sxs-lookup"><span data-stu-id="6294f-182">Add *_ViewImports.cshtml* to the [application root folder](#arf).</span></span> <span data-ttu-id="6294f-183">Un *_ViewImports.cshtml* en la carpeta raíz de la aplicación se aplicará a todas las vistas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-183">A *_ViewImports.cshtml* in the application root folder will apply to all views in the app.</span></span>
* <span data-ttu-id="6294f-184">Copie el archivo *_ViewImports.cshtml* en la carpeta de vista adecuada en áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-184">Copy the *_ViewImports.cshtml* file to the appropriate view folder under areas.</span></span>

<span data-ttu-id="6294f-185">El archivo *_ViewImports.cshtml* normalmente contiene `@using`importaciones `@inject` de [aplicaciones auxiliares](xref:mvc/views/tag-helpers/intro) de etiquetas e instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6294f-185">The *_ViewImports.cshtml* file typically contains [Tag Helpers](xref:mvc/views/tag-helpers/intro) imports, `@using`, and `@inject` statements.</span></span> <span data-ttu-id="6294f-186">Para obtener más información, consulte [Importación de directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="6294f-186">For more information, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="6294f-187">Cambio de la carpeta de área predeterminada donde se almacenan las vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-187">Change default area folder where views are stored</span></span>

<span data-ttu-id="6294f-188">En el código siguiente se cambia la carpeta de área predeterminada de `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="6294f-188">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="6294f-189">Áreas con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6294f-189">Areas with Razor Pages</span></span>

<span data-ttu-id="6294f-190">Las áreas con `Areas/<area name>/Pages` Razor Pages requieren una carpeta en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-190">Areas with Razor Pages require an `Areas/<area name>/Pages` folder in the root of the app.</span></span> <span data-ttu-id="6294f-191">La siguiente estructura de carpetas se usa con la [aplicación de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples):</span><span class="sxs-lookup"><span data-stu-id="6294f-191">The following folder structure is used with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples):</span></span>

* <span data-ttu-id="6294f-192">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="6294f-192">Project name</span></span>
  * <span data-ttu-id="6294f-193">Áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-193">Areas</span></span>
    * <span data-ttu-id="6294f-194">Productos</span><span class="sxs-lookup"><span data-stu-id="6294f-194">Products</span></span>
      * <span data-ttu-id="6294f-195">Páginas</span><span class="sxs-lookup"><span data-stu-id="6294f-195">Pages</span></span>
        * <span data-ttu-id="6294f-196">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="6294f-196">_ViewImports</span></span>
        * <span data-ttu-id="6294f-197">Información</span><span class="sxs-lookup"><span data-stu-id="6294f-197">About</span></span>
        * <span data-ttu-id="6294f-198">Índice</span><span class="sxs-lookup"><span data-stu-id="6294f-198">Index</span></span>
    * <span data-ttu-id="6294f-199">Servicios</span><span class="sxs-lookup"><span data-stu-id="6294f-199">Services</span></span>
      * <span data-ttu-id="6294f-200">Páginas</span><span class="sxs-lookup"><span data-stu-id="6294f-200">Pages</span></span>
        * <span data-ttu-id="6294f-201">Administrar</span><span class="sxs-lookup"><span data-stu-id="6294f-201">Manage</span></span>
          * <span data-ttu-id="6294f-202">Información</span><span class="sxs-lookup"><span data-stu-id="6294f-202">About</span></span>
          * <span data-ttu-id="6294f-203">Índice</span><span class="sxs-lookup"><span data-stu-id="6294f-203">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="6294f-204">Generación de vínculos con Razor Pages y áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-204">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="6294f-205">El código siguiente de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) muestra la generación de vínculos con el área especificada (por ejemplo, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="6294f-205">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/31samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="6294f-206">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="6294f-206">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="6294f-207">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="6294f-207">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="6294f-208">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página de la misma área.</span><span class="sxs-lookup"><span data-stu-id="6294f-208">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="6294f-209">Cuando no se especifica el área, el enrutamiento depende de los valores *ambient*.</span><span class="sxs-lookup"><span data-stu-id="6294f-209">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="6294f-210">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="6294f-210">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6294f-211">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="6294f-211">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="6294f-212">Por ejemplo, tenga en cuenta los vínculos generados desde el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-212">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="6294f-213">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="6294f-213">For the preceding code:</span></span>

* <span data-ttu-id="6294f-214">El vínculo generado a partir de `<a asp-page="/Manage/About">` es correcto solo cuando la última solicitud fue de una página del área `Services`.</span><span class="sxs-lookup"><span data-stu-id="6294f-214">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="6294f-215">Por ejemplo, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="6294f-215">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="6294f-216">El vínculo generado a partir de `<a asp-page="/About">` es correcto solo cuando la última solicitud fue de una página de `/Home`.</span><span class="sxs-lookup"><span data-stu-id="6294f-216">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="6294f-217">El código está tomado de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="6294f-217">The code is from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="6294f-218">Importación del espacio de nombres y los asistentes de etiquetas con el archivo _ViewImports</span><span class="sxs-lookup"><span data-stu-id="6294f-218">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="6294f-219">Se puede agregar un archivo *_ViewImports.cshtml* a la carpeta *Pages* de cada área para importar el espacio de nombres y los asistentes de etiquetas en cada página de Razor de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="6294f-219">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="6294f-220">Tenga en cuenta el área *Services* del código de ejemplo, que no contiene un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6294f-220">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6294f-221">El marcado siguiente muestra la página de Razor */Services/Manage/About*:</span><span class="sxs-lookup"><span data-stu-id="6294f-221">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="6294f-222">En el marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="6294f-222">In the preceding markup:</span></span>

* <span data-ttu-id="6294f-223">El nombre de dominio completo debe usarse para especificar el modelo (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="6294f-223">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="6294f-224">[Las aplicaciones auxiliares](xref:mvc/views/tag-helpers/intro) de etiquetas están habilitadas por`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="6294f-224">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="6294f-225">En la descarga de ejemplo, el área Products contiene el siguiente archivo *_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6294f-225">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6294f-226">El siguiente marcado muestra la página de Razor */Products/About*:</span><span class="sxs-lookup"><span data-stu-id="6294f-226">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="6294f-227">En el archivo anterior, el espacio de nombres y la directiva `@addTagHelper` se importan en el archivo mediante el archivo *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6294f-227">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="6294f-228">Para más información, consulte [Administración del ámbito de los asistentes de etiquetas](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) y [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="6294f-228">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="6294f-229">Diseño compartido para áreas de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6294f-229">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="6294f-230">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-230">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="6294f-231">Publicación de áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-231">Publishing Areas</span></span>

<span data-ttu-id="6294f-232">Todos los archivos \*.cshtml y los archivos que formen parte del directorio *wwwroot* se publicarán como resultados si `<Project Sdk="Microsoft.NET.Sdk.Web">` está incluido en el archivo \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="6294f-232">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6294f-233">Las áreas son una característica de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).</span><span class="sxs-lookup"><span data-stu-id="6294f-233">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="6294f-234">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`, o bien a un elemento `page` de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="6294f-234">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="6294f-235">Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core en grupos funcionales más pequeños, cada uno con su propio conjunto de Razor Pages, controladores, vistas y modelos.</span><span class="sxs-lookup"><span data-stu-id="6294f-235">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="6294f-236">Un área es en realidad una estructura dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-236">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="6294f-237">En un proyecto web ASP.NET Core, los componentes lógicos como Páginas, Modelo, Vista y Controlador se mantienen en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="6294f-237">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="6294f-238">El runtime de ASP.NET Core usa convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="6294f-238">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="6294f-239">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="6294f-239">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="6294f-240">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como las de finalización de la compra, facturación y búsqueda.</span><span class="sxs-lookup"><span data-stu-id="6294f-240">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="6294f-241">Cada una de estas unidades tiene un área propia para controlar vistas, controladores, Razor Pages y modelos.</span><span class="sxs-lookup"><span data-stu-id="6294f-241">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="6294f-242">Considere el uso de áreas en un proyecto en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6294f-242">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="6294f-243">La aplicación está formada por varios componentes funcionales generales que se pueden separar de forma lógica.</span><span class="sxs-lookup"><span data-stu-id="6294f-243">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="6294f-244">Le interesa dividir la aplicación para que se pueda trabajar en cada área funcional de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="6294f-244">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="6294f-245">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6294f-245">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6294f-246">En el ejemplo de descarga se proporciona una aplicación básica para probar las áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-246">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="6294f-247">Si va a usar Razor Pages, consulte [Áreas con Razor Pages](#areas-with-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="6294f-247">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="6294f-248">Áreas para controladores con vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-248">Areas for controllers with views</span></span>

<span data-ttu-id="6294f-249">Una aplicación web ASP.NET Core típica en la que se usen áreas, controladores y vistas contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-249">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="6294f-250">Una [estructura de carpetas de área](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="6294f-250">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="6294f-251">Controladores [`[Area]`](#attribute) con el atributo para asociar el regulador con el área:</span><span class="sxs-lookup"><span data-stu-id="6294f-251">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="6294f-252">La ruta de [área añadida al inicio:](#add-area-route)</span><span class="sxs-lookup"><span data-stu-id="6294f-252">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="6294f-253">Estructura de carpetas de área</span><span class="sxs-lookup"><span data-stu-id="6294f-253">Area folder structure</span></span>

<span data-ttu-id="6294f-254">Considere una aplicación que tiene dos grupos lógicos, *Productos* y *Servicios*.</span><span class="sxs-lookup"><span data-stu-id="6294f-254">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="6294f-255">Mediante las áreas, la estructura de carpetas sería similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-255">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="6294f-256">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="6294f-256">Project name</span></span>
  * <span data-ttu-id="6294f-257">Áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-257">Areas</span></span>
    * <span data-ttu-id="6294f-258">Productos</span><span class="sxs-lookup"><span data-stu-id="6294f-258">Products</span></span>
      * <span data-ttu-id="6294f-259">Controladores</span><span class="sxs-lookup"><span data-stu-id="6294f-259">Controllers</span></span>
        * <span data-ttu-id="6294f-260">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-260">HomeController.cs</span></span>
        * <span data-ttu-id="6294f-261">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-261">ManageController.cs</span></span>
      * <span data-ttu-id="6294f-262">Vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-262">Views</span></span>
        * <span data-ttu-id="6294f-263">Inicio</span><span class="sxs-lookup"><span data-stu-id="6294f-263">Home</span></span>
          * <span data-ttu-id="6294f-264">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-264">Index.cshtml</span></span>
        * <span data-ttu-id="6294f-265">Administrar</span><span class="sxs-lookup"><span data-stu-id="6294f-265">Manage</span></span>
          * <span data-ttu-id="6294f-266">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-266">Index.cshtml</span></span>
          * <span data-ttu-id="6294f-267">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-267">About.cshtml</span></span>
    * <span data-ttu-id="6294f-268">Servicios</span><span class="sxs-lookup"><span data-stu-id="6294f-268">Services</span></span>
      * <span data-ttu-id="6294f-269">Controladores</span><span class="sxs-lookup"><span data-stu-id="6294f-269">Controllers</span></span>
        * <span data-ttu-id="6294f-270">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6294f-270">HomeController.cs</span></span>
      * <span data-ttu-id="6294f-271">Vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-271">Views</span></span>
        * <span data-ttu-id="6294f-272">Inicio</span><span class="sxs-lookup"><span data-stu-id="6294f-272">Home</span></span>
          * <span data-ttu-id="6294f-273">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-273">Index.cshtml</span></span>

<span data-ttu-id="6294f-274">Aunque el diseño anterior es típico cuando se usan áreas, solo los archivos de vista tienen que usar esta estructura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="6294f-274">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="6294f-275">La detección de vista busca un archivo de vista de área coincidente en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-275">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="6294f-276">Asociación del controlador a un área</span><span class="sxs-lookup"><span data-stu-id="6294f-276">Associate the controller with an Area</span></span>

<span data-ttu-id="6294f-277">Los controladores de [ &lbrack;&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) área se designan con el atributo Area:</span><span class="sxs-lookup"><span data-stu-id="6294f-277">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="6294f-278">Adición de la ruta de área</span><span class="sxs-lookup"><span data-stu-id="6294f-278">Add Area route</span></span>

<span data-ttu-id="6294f-279">Las rutas de área normalmente usan el enrutamiento convencional en lugar del enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="6294f-279">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="6294f-280">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="6294f-280">Conventional routing is order-dependent.</span></span> <span data-ttu-id="6294f-281">En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="6294f-281">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="6294f-282">Se puede `{area:...}` usar como un token en las plantillas de ruta, si el espacio de direcciones URL es uniforme en todas las áreas:</span><span class="sxs-lookup"><span data-stu-id="6294f-282">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="6294f-283">En el código anterior, `exists` aplica la restricción de que la ruta debe coincidir con un área.</span><span class="sxs-lookup"><span data-stu-id="6294f-283">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="6294f-284">El uso de `{area:...}` es el mecanismo menos complicado para agregar enrutamiento a las áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-284">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="6294f-285">En el código siguiente se usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> para crear dos rutas de área con nombre:</span><span class="sxs-lookup"><span data-stu-id="6294f-285">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="6294f-286">Cuando use `MapAreaRoute` con ASP.NET Core 2.2, vea [este problema de GitHub](https://github.com/dotnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="6294f-286">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="6294f-287">Para más información, vea [Enrutamiento de áreas](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="6294f-287">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="6294f-288">Generación de vínculos con áreas de MVC</span><span class="sxs-lookup"><span data-stu-id="6294f-288">Link generation with MVC areas</span></span>

<span data-ttu-id="6294f-289">En el código siguiente de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) se muestra la generación de vínculos con el área especificada:</span><span class="sxs-lookup"><span data-stu-id="6294f-289">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="6294f-290">Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-290">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="6294f-291">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="6294f-291">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="6294f-292">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="6294f-292">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="6294f-293">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página en el mismo área y controlador.</span><span class="sxs-lookup"><span data-stu-id="6294f-293">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="6294f-294">Cuando no se especifica el área o el controlador, el enrutamiento depende de los valores de *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="6294f-294">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="6294f-295">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="6294f-295">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6294f-296">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="6294f-296">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="6294f-297">Para más información, vea [Enrutar a acciones de controlador](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="6294f-297">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="6294f-298">Diseño Compartido para áreas con el archivo _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-298">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="6294f-299">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-299">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="6294f-300">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="6294f-300">_ViewImports.cshtml</span></span>

<span data-ttu-id="6294f-301">En su ubicación estándar, */Views/_ViewImports.cshtml* no se aplica a las áreas.</span><span class="sxs-lookup"><span data-stu-id="6294f-301">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="6294f-302">Para utilizar [aplicaciones auxiliares](xref:mvc/views/tag-helpers/intro)de etiquetas comunes , `@using`, o `@inject` en su área, asegúrese de que un archivo *_ViewImports.cshtml* adecuado se aplica a las vistas de [área.](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="6294f-302">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="6294f-303">Si quiere el mismo comportamiento en todas las vistas, mueva */Views/_ViewImports.cshtml* a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-303">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="6294f-304">Cambio de la carpeta de área predeterminada donde se almacenan las vistas</span><span class="sxs-lookup"><span data-stu-id="6294f-304">Change default area folder where views are stored</span></span>

<span data-ttu-id="6294f-305">En el código siguiente se cambia la carpeta de área predeterminada de `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="6294f-305">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="6294f-306">Áreas con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6294f-306">Areas with Razor Pages</span></span>

<span data-ttu-id="6294f-307">Las áreas con `Areas/<area name>/Pages` Razor Pages requieren una carpeta en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-307">Areas with Razor Pages require an `Areas/<area name>/Pages` folder in the root of the app.</span></span> <span data-ttu-id="6294f-308">La siguiente estructura de carpetas se usa con la [aplicación de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span><span class="sxs-lookup"><span data-stu-id="6294f-308">The following folder structure is used with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="6294f-309">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="6294f-309">Project name</span></span>
  * <span data-ttu-id="6294f-310">Áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-310">Areas</span></span>
    * <span data-ttu-id="6294f-311">Productos</span><span class="sxs-lookup"><span data-stu-id="6294f-311">Products</span></span>
      * <span data-ttu-id="6294f-312">Páginas</span><span class="sxs-lookup"><span data-stu-id="6294f-312">Pages</span></span>
        * <span data-ttu-id="6294f-313">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="6294f-313">_ViewImports</span></span>
        * <span data-ttu-id="6294f-314">Información</span><span class="sxs-lookup"><span data-stu-id="6294f-314">About</span></span>
        * <span data-ttu-id="6294f-315">Índice</span><span class="sxs-lookup"><span data-stu-id="6294f-315">Index</span></span>
    * <span data-ttu-id="6294f-316">Servicios</span><span class="sxs-lookup"><span data-stu-id="6294f-316">Services</span></span>
      * <span data-ttu-id="6294f-317">Páginas</span><span class="sxs-lookup"><span data-stu-id="6294f-317">Pages</span></span>
        * <span data-ttu-id="6294f-318">Administrar</span><span class="sxs-lookup"><span data-stu-id="6294f-318">Manage</span></span>
          * <span data-ttu-id="6294f-319">Información</span><span class="sxs-lookup"><span data-stu-id="6294f-319">About</span></span>
          * <span data-ttu-id="6294f-320">Índice</span><span class="sxs-lookup"><span data-stu-id="6294f-320">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="6294f-321">Generación de vínculos con Razor Pages y áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-321">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="6294f-322">El código siguiente de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) muestra la generación de vínculos con el área especificada (por ejemplo, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="6294f-322">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="6294f-323">Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-323">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="6294f-324">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="6294f-324">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="6294f-325">En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="6294f-325">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="6294f-326">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página de la misma área.</span><span class="sxs-lookup"><span data-stu-id="6294f-326">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="6294f-327">Cuando no se especifica el área, el enrutamiento depende de los valores *ambient*.</span><span class="sxs-lookup"><span data-stu-id="6294f-327">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="6294f-328">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="6294f-328">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6294f-329">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="6294f-329">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="6294f-330">Por ejemplo, tenga en cuenta los vínculos generados desde el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6294f-330">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="6294f-331">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="6294f-331">For the preceding code:</span></span>

* <span data-ttu-id="6294f-332">El vínculo generado a partir de `<a asp-page="/Manage/About">` es correcto solo cuando la última solicitud fue de una página del área `Services`.</span><span class="sxs-lookup"><span data-stu-id="6294f-332">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="6294f-333">Por ejemplo, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="6294f-333">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="6294f-334">El vínculo generado a partir de `<a asp-page="/About">` es correcto solo cuando la última solicitud fue de una página de `/Home`.</span><span class="sxs-lookup"><span data-stu-id="6294f-334">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="6294f-335">El código está tomado de la [descarga de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="6294f-335">The code is from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="6294f-336">Importación del espacio de nombres y los asistentes de etiquetas con el archivo _ViewImports</span><span class="sxs-lookup"><span data-stu-id="6294f-336">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="6294f-337">Se puede agregar un archivo *_ViewImports.cshtml* a la carpeta *Pages* de cada área para importar el espacio de nombres y los asistentes de etiquetas en cada página de Razor de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="6294f-337">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="6294f-338">Tenga en cuenta el área *Services* del código de ejemplo, que no contiene un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6294f-338">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6294f-339">El marcado siguiente muestra la página de Razor */Services/Manage/About*:</span><span class="sxs-lookup"><span data-stu-id="6294f-339">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="6294f-340">En el marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="6294f-340">In the preceding markup:</span></span>

* <span data-ttu-id="6294f-341">El nombre de dominio completo debe usarse para especificar el modelo (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="6294f-341">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="6294f-342">[Las aplicaciones auxiliares](xref:mvc/views/tag-helpers/intro) de etiquetas están habilitadas por`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="6294f-342">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="6294f-343">En la descarga de ejemplo, el área Products contiene el siguiente archivo *_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6294f-343">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6294f-344">El siguiente marcado muestra la página de Razor */Products/About*:</span><span class="sxs-lookup"><span data-stu-id="6294f-344">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="6294f-345">En el archivo anterior, el espacio de nombres y la directiva `@addTagHelper` se importan en el archivo mediante el archivo *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6294f-345">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="6294f-346">Para más información, consulte [Administración del ámbito de los asistentes de etiquetas](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) y [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="6294f-346">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="6294f-347">Diseño compartido para áreas de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6294f-347">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="6294f-348">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6294f-348">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="6294f-349">Publicación de áreas</span><span class="sxs-lookup"><span data-stu-id="6294f-349">Publishing Areas</span></span>

<span data-ttu-id="6294f-350">Todos los archivos \*.cshtml y los archivos que formen parte del directorio *wwwroot* se publicarán como resultados si `<Project Sdk="Microsoft.NET.Sdk.Web">` está incluido en el archivo \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="6294f-350">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
::: moniker-end
