---
uid: mvc/overview/releases/mvc51-release-notes
title: Novedades de ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828640"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="e9c61-102">Novedades de ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="e9c61-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="e9c61-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e9c61-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="e9c61-104">Este tema describe cuáles son las novedades para ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="e9c61-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="e9c61-105">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="e9c61-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="e9c61-106">Descarga</span><span class="sxs-lookup"><span data-stu-id="e9c61-106">Download</span></span>](#download)
- [<span data-ttu-id="e9c61-107">Documentación</span><span class="sxs-lookup"><span data-stu-id="e9c61-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="e9c61-108">Nuevas características de ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="e9c61-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="e9c61-109">Mejoras de enrutamiento de atributo</span><span class="sxs-lookup"><span data-stu-id="e9c61-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="e9c61-110">Compatibilidad de arranque para plantillas de editor</span><span class="sxs-lookup"><span data-stu-id="e9c61-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="e9c61-111">Compatibilidad de enumeraciones en las vistas</span><span class="sxs-lookup"><span data-stu-id="e9c61-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="e9c61-112">Validación discreta de los atributos de MinLength/MaxLength</span><span class="sxs-lookup"><span data-stu-id="e9c61-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="e9c61-113">Compatibilidad con el contexto 'this' en Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="e9c61-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="e9c61-114">[Problemas conocidos y cambios importantes](#KnownBreakingChanges)- [correcciones de errores](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="e9c61-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="e9c61-115">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="e9c61-115">Software Requirements</span></span>

- <span data-ttu-id="e9c61-116">Visual Studio 2012: Descargar [ASP.NET y Web Tools 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="e9c61-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="e9c61-117">Visual Studio 2013: Descarga [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="e9c61-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="e9c61-118">Esta actualización es necesaria para editar las vistas de Razor de ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="e9c61-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="e9c61-119">Descargar</span><span class="sxs-lookup"><span data-stu-id="e9c61-119">Download</span></span>

<span data-ttu-id="e9c61-120">Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet.</span><span class="sxs-lookup"><span data-stu-id="e9c61-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="e9c61-121">Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación.</span><span class="sxs-lookup"><span data-stu-id="e9c61-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="e9c61-122">El paquete más reciente de ASP.NET MVC 5.1 RTM tiene la siguiente versión: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="e9c61-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="e9c61-123">Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="e9c61-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="e9c61-124">La versión también incluye los correspondientes paquetes localizados en NuGet.</span><span class="sxs-lookup"><span data-stu-id="e9c61-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="e9c61-125">Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="e9c61-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="e9c61-126">Documentación</span><span class="sxs-lookup"><span data-stu-id="e9c61-126">Documentation</span></span>

<span data-ttu-id="e9c61-127">Tutoriales y otra información sobre ASP.NET MVC 5.1 RTM están disponibles desde el sitio web ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="e9c61-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="e9c61-128">Nuevas características de ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="e9c61-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="e9c61-129">Mejoras de enrutamiento de atributo</span><span class="sxs-lookup"><span data-stu-id="e9c61-129">Attribute routing improvements</span></span>

 <span data-ttu-id="e9c61-130">Enrutamiento ahora es compatible con restricciones, habilitar el control de versiones y el encabezado de atributos en función de selección de la ruta.</span><span class="sxs-lookup"><span data-stu-id="e9c61-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="e9c61-131">Muchos aspectos de las rutas de atributo ahora son personalizables a través de la `IDirectRouteFactory` interfaz y `RouteFactoryAttribute` clase.</span><span class="sxs-lookup"><span data-stu-id="e9c61-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="e9c61-132">El prefijo de ruta ahora es extensible a través de la `IRoutePrefix` interfaz y `RoutePrefixAttribute` clase.</span><span class="sxs-lookup"><span data-stu-id="e9c61-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="e9c61-133">Compatibilidad de enumeraciones en las vistas</span><span class="sxs-lookup"><span data-stu-id="e9c61-133">Enum support in views</span></span>

1. <span data-ttu-id="e9c61-134">Nuevo `@Html.EnumDropDownListFor()` métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="e9c61-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="e9c61-135">Deben utilizarse como la mayoría de las aplicaciones auxiliares HTML con la salvedad de que la expresión debe evaluarse como un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) donde `T` es un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo.</span><span class="sxs-lookup"><span data-stu-id="e9c61-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="e9c61-136">Use `EnumHelper.IsValidForEnumHelper()` para comprobar estos requisitos.</span><span class="sxs-lookup"><span data-stu-id="e9c61-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="e9c61-137">Nuevo `EnumHelper.GetSelectList()` métodos que devuelven un `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="e9c61-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="e9c61-138">Esto es útil cuando deba manipular una lista de selección antes de llamar a, por ejemplo, `@Html.DropDownListFor()`, o cuando desee mostrar los nombres que `@Html.EnumDropDownListFor()` muestra.</span><span class="sxs-lookup"><span data-stu-id="e9c61-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="e9c61-139">El código siguiente muestra estas API.</span><span class="sxs-lookup"><span data-stu-id="e9c61-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="e9c61-140">Puede ver un ejemplo completo [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="e9c61-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="e9c61-141">Compatibilidad de arranque para plantillas de editor</span><span class="sxs-lookup"><span data-stu-id="e9c61-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="e9c61-142">Ahora permitimos pasar en los atributos HTML en [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como un [objeto anónimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9c61-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="e9c61-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e9c61-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="e9c61-144">Validación discreta para MinLengthAttribute y MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="e9c61-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="e9c61-145">Ahora, se admitirá la validación del lado cliente para los tipos string y array para Propiedades decoran con el [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) y [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.</span><span class="sxs-lookup"><span data-stu-id="e9c61-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="e9c61-146">Compatibilidad con el contexto 'this' en Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="e9c61-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="e9c61-147">Las funciones de devolución de llamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) ahora podrán localizar el elemento de invocación a través de la `this` contexto.</span><span class="sxs-lookup"><span data-stu-id="e9c61-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="e9c61-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e9c61-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="e9c61-149">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="e9c61-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="e9c61-150">Enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="e9c61-150">Attribute Routing</span></span>

<span data-ttu-id="e9c61-151">Ambigüedades en coincidencias de enrutamiento de atributos ahora notificará un error en lugar de elegir a la primera coincidencia.</span><span class="sxs-lookup"><span data-stu-id="e9c61-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="e9c61-152">Las rutas de atributo se prohíbe la `{controller}` parámetro y del uso de la `{action}` parámetro en las rutas se coloca en las acciones.</span><span class="sxs-lookup"><span data-stu-id="e9c61-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="e9c61-153">Usos de estos parámetros es muy probable que daría lugar a ambigüedades.</span><span class="sxs-lookup"><span data-stu-id="e9c61-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="e9c61-154">Scaffolding de MVC o API Web en un proyecto con 5.1 da como resultado paquetes 5.0 paquetes para las que aún no existen en el proyecto</span><span class="sxs-lookup"><span data-stu-id="e9c61-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="e9c61-155">Actualizar paquetes de NuGet para ASP.NET MVC 5.1 RTM no actualiza las herramientas de Visual Studio como el scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9c61-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="e9c61-156">Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="e9c61-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="e9c61-157">Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos.</span><span class="sxs-lookup"><span data-stu-id="e9c61-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="e9c61-158">Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 no sobrescribe los paquetes más recientes en sus proyectos.</span><span class="sxs-lookup"><span data-stu-id="e9c61-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="e9c61-159">Si usa scaffolding de ASP.NET después de actualizar los paquetes de los proyectos de Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y ASP.NET MVC son coherentes.</span><span class="sxs-lookup"><span data-stu-id="e9c61-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="e9c61-160">Resaltado de sintaxis para las vistas de Razor en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e9c61-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="e9c61-161">Si actualiza a ASP.NET MVC 5.1 RTM sin actualizar Visual Studio 2013, no obtendrá compatibilidad con el editor Visual Studio para resaltar la sintaxis mientras se editan las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="e9c61-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="e9c61-162">Deberá actualizar Visual Studio 2013 para obtener esta compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="e9c61-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="e9c61-163">Cambia el nombre de tipo</span><span class="sxs-lookup"><span data-stu-id="e9c61-163">Type Renames</span></span>

<span data-ttu-id="e9c61-164">Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributos se cambia el nombre 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="e9c61-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="e9c61-165">**Nombre de tipo anterior (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="e9c61-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="e9c61-166">**Nuevo tipo de nombre (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="e9c61-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="e9c61-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="e9c61-167">IDirectRouteProvider</span></span> | <span data-ttu-id="e9c61-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="e9c61-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="e9c61-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="e9c61-169">RouteProviderAttribute</span></span> | <span data-ttu-id="e9c61-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="e9c61-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="e9c61-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="e9c61-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="e9c61-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="e9c61-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="e9c61-173">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="e9c61-173">Bug Fixes</span></span>

<span data-ttu-id="e9c61-174">Esta versión también incluye varias correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="e9c61-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="e9c61-175">Puede encontrar una lista completa aquí:</span><span class="sxs-lookup"><span data-stu-id="e9c61-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="e9c61-176">5.1.0 paquete</span><span class="sxs-lookup"><span data-stu-id="e9c61-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="e9c61-177">5.1.1 paquete</span><span class="sxs-lookup"><span data-stu-id="e9c61-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="e9c61-178">El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="e9c61-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
