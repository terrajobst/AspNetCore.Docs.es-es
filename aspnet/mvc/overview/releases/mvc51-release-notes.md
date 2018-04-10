---
uid: mvc/overview/releases/mvc51-release-notes
title: "' S New en ASP.NET MVC 5.1 | Documentos de Microsoft"
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="54106-102">' S New en ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="54106-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="54106-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="54106-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="54106-104">Este tema describe lo que es nuevo en ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="54106-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="54106-105">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="54106-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="54106-106">Descarga</span><span class="sxs-lookup"><span data-stu-id="54106-106">Download</span></span>](#download)
- [<span data-ttu-id="54106-107">Documentación</span><span class="sxs-lookup"><span data-stu-id="54106-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="54106-108">Nuevas características de ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="54106-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="54106-109">Atributo mejoras de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="54106-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="54106-110">Compatibilidad de arranque para plantillas de editor</span><span class="sxs-lookup"><span data-stu-id="54106-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="54106-111">Compatibilidad de enumeraciones en las vistas</span><span class="sxs-lookup"><span data-stu-id="54106-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="54106-112">Validación discreto para los atributos de MinLength y MaxLength</span><span class="sxs-lookup"><span data-stu-id="54106-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="54106-113">Compatibilidad con el contexto de 'this' en Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="54106-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="54106-114">[Problemas conocidos y cambios importantes](#KnownBreakingChanges)- [correcciones de errores](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="54106-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="54106-115">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="54106-115">Software Requirements</span></span>

- <span data-ttu-id="54106-116">Visual Studio 2012: Descargar [Web ASP.NET y herramientas 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="54106-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="54106-117">Visual Studio 2013: Descarga [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="54106-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="54106-118">Esta actualización es necesaria para editar las vistas de Razor de ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="54106-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="54106-119">Descargar</span><span class="sxs-lookup"><span data-stu-id="54106-119">Download</span></span>

<span data-ttu-id="54106-120">Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet.</span><span class="sxs-lookup"><span data-stu-id="54106-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="54106-121">Siguen todos los paquetes en tiempo de ejecución el [control de versiones semántico](http://semver.org/) especificación.</span><span class="sxs-lookup"><span data-stu-id="54106-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="54106-122">El paquete más reciente de RTM de ASP.NET MVC 5.1 tiene la siguiente versión: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="54106-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="54106-123">Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="54106-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="54106-124">La versión también incluye paquetes localizados correspondientes en NuGet.</span><span class="sxs-lookup"><span data-stu-id="54106-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="54106-125">Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="54106-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="54106-126">Documentación</span><span class="sxs-lookup"><span data-stu-id="54106-126">Documentation</span></span>

<span data-ttu-id="54106-127">Tutoriales y otra información sobre ASP.NET MVC 5.1 RTM están disponibles desde el sitio web ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="54106-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="54106-128">Nuevas características de ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="54106-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="54106-129">Atributo mejoras de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="54106-129">Attribute routing improvements</span></span>

 <span data-ttu-id="54106-130">Atributo enrutamiento ahora es compatible con restricciones, habilitar el control de versiones y encabezado según la selección de la ruta.</span><span class="sxs-lookup"><span data-stu-id="54106-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="54106-131">Muchos aspectos de las rutas de atributo ahora son personalizables a través de la `IDirectRouteFactory` interfaz y `RouteFactoryAttribute` clase.</span><span class="sxs-lookup"><span data-stu-id="54106-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="54106-132">El prefijo de ruta ahora es extensible a través de la `IRoutePrefix` interfaz y `RoutePrefixAttribute` clase.</span><span class="sxs-lookup"><span data-stu-id="54106-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="54106-133">Compatibilidad de enumeraciones en las vistas</span><span class="sxs-lookup"><span data-stu-id="54106-133">Enum support in views</span></span>

1. <span data-ttu-id="54106-134">Nueva `@Html.EnumDropDownListFor()` métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="54106-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="54106-135">Deben usarse como la mayoría de las aplicaciones auxiliares HTML con la salvedad de que la expresión debe evaluarse como un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) donde `T` es un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo.</span><span class="sxs-lookup"><span data-stu-id="54106-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="54106-136">Utilice `EnumHelper.IsValidForEnumHelper()` para comprobar estos requisitos.</span><span class="sxs-lookup"><span data-stu-id="54106-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="54106-137">Nueva `EnumHelper.GetSelectList()` métodos que devuelven un `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="54106-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="54106-138">Esto es útil cuando es necesario manipular una lista de selección antes de llamar a, por ejemplo, `@Html.DropDownListFor()`, o cuando desee mostrar los nombres que `@Html.EnumDropDownListFor()` muestra.</span><span class="sxs-lookup"><span data-stu-id="54106-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="54106-139">El código siguiente muestra estas API.</span><span class="sxs-lookup"><span data-stu-id="54106-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="54106-140">Puede ver un ejemplo completo [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="54106-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="54106-141">Compatibilidad de arranque para plantillas de editor</span><span class="sxs-lookup"><span data-stu-id="54106-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="54106-142">Se permite ahora pasar en los atributos HTML en [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como un [objeto anónimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="54106-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="54106-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54106-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="54106-144">Validación discreto para MinLengthAttribute y MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="54106-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="54106-145">Ahora, se admitirá la validación del lado cliente para los tipos string y array para propiedades decorada con el [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) y [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.</span><span class="sxs-lookup"><span data-stu-id="54106-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="54106-146">Compatibilidad con el contexto de 'this' en Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="54106-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="54106-147">Las funciones de devolución de llamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) ahora podrá buscar el elemento invocar a través de la `this` contexto.</span><span class="sxs-lookup"><span data-stu-id="54106-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="54106-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54106-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="54106-149">Problemas conocidos y los cambios recientes</span><span class="sxs-lookup"><span data-stu-id="54106-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="54106-150">Ruta de atributo</span><span class="sxs-lookup"><span data-stu-id="54106-150">Attribute Routing</span></span>

<span data-ttu-id="54106-151">Ambigüedades en coincidencias de enrutamiento de atributo ahora notificará un error en lugar de elegir a la primera coincidencia.</span><span class="sxs-lookup"><span data-stu-id="54106-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="54106-152">Rutas de atributo se les prohíbe usar la `{controller}` parámetro y del uso de la `{action}` parámetro en las rutas se coloca en las acciones.</span><span class="sxs-lookup"><span data-stu-id="54106-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="54106-153">Usos de estos parámetros muy probablemente conduciría a ambigüedades.</span><span class="sxs-lookup"><span data-stu-id="54106-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="54106-154">Scaffolding MVC o Web API en un proyecto con 5.1 resultados de paquetes en 5.0 paquetes para los que ya no existen en el proyecto</span><span class="sxs-lookup"><span data-stu-id="54106-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="54106-155">Actualizar paquetes de NuGet para ASP.NET MVC 5.1 RTM no actualizar las herramientas de Visual Studio como scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="54106-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="54106-156">Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="54106-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="54106-157">Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos.</span><span class="sxs-lookup"><span data-stu-id="54106-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="54106-158">Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 sobrescribir los paquetes más recientes de los proyectos.</span><span class="sxs-lookup"><span data-stu-id="54106-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="54106-159">Si utiliza ASP.NET scaffolding después de actualizar los paquetes de los proyectos Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y MVC de ASP.NET son coherentes.</span><span class="sxs-lookup"><span data-stu-id="54106-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="54106-160">Resaltado de sintaxis para las vistas de Razor en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54106-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="54106-161">Si ha actualizado a RTM de ASP.NET MVC 5.1 sin actualizar Visual Studio 2013, no obtendrá la compatibilidad con el editor de Visual Studio para resaltar la sintaxis mientras edita las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="54106-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="54106-162">Debe actualizar Visual Studio 2013 para obtener esta compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="54106-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="54106-163">Cambia el nombre de tipo</span><span class="sxs-lookup"><span data-stu-id="54106-163">Type Renames</span></span>

<span data-ttu-id="54106-164">Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributo se cambia el nombre RTM 5.1.</span><span class="sxs-lookup"><span data-stu-id="54106-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="54106-165">**Nombre de tipo anterior (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="54106-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="54106-166">**Nuevo tipo de nombre (RTM 5.1)**</span><span class="sxs-lookup"><span data-stu-id="54106-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="54106-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="54106-167">IDirectRouteProvider</span></span> | <span data-ttu-id="54106-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="54106-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="54106-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="54106-169">RouteProviderAttribute</span></span> | <span data-ttu-id="54106-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="54106-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="54106-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="54106-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="54106-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="54106-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="54106-173">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="54106-173">Bug Fixes</span></span>

<span data-ttu-id="54106-174">Esta versión también incluye varias correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="54106-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="54106-175">Puede encontrar la lista completa aquí:</span><span class="sxs-lookup"><span data-stu-id="54106-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="54106-176">5.1.0 paquete</span><span class="sxs-lookup"><span data-stu-id="54106-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="54106-177">5.1.1 paquete de</span><span class="sxs-lookup"><span data-stu-id="54106-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="54106-178">El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="54106-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
