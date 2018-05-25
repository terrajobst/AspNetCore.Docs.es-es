---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "' S New en ASP.NET Web API 2.1 | Documentos de Microsoft"
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="ca14d-102">' S New en ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="ca14d-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="ca14d-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ca14d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="ca14d-104">Este tema describe lo que es nuevo en ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca14d-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="ca14d-105">Descarga</span><span class="sxs-lookup"><span data-stu-id="ca14d-105">Download</span></span>](#download)
- [<span data-ttu-id="ca14d-106">Documentación</span><span class="sxs-lookup"><span data-stu-id="ca14d-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="ca14d-107">Nuevas características de ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="ca14d-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="ca14d-108">Control de errores global</span><span class="sxs-lookup"><span data-stu-id="ca14d-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="ca14d-109">Atributo mejoras de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="ca14d-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="ca14d-110">Mejoras de la página de ayuda</span><span class="sxs-lookup"><span data-stu-id="ca14d-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="ca14d-111">Compatibilidad con IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="ca14d-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="ca14d-112">Formateador de tipo de medio BSON</span><span class="sxs-lookup"><span data-stu-id="ca14d-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="ca14d-113">Una mejor compatibilidad con filtros de Async</span><span class="sxs-lookup"><span data-stu-id="ca14d-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="ca14d-114">Consulta de análisis para el cliente de biblioteca de formato</span><span class="sxs-lookup"><span data-stu-id="ca14d-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="ca14d-115">Problemas conocidos y los cambios recientes</span><span class="sxs-lookup"><span data-stu-id="ca14d-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="ca14d-116">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="ca14d-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="ca14d-117">Descargar</span><span class="sxs-lookup"><span data-stu-id="ca14d-117">Download</span></span>

<span data-ttu-id="ca14d-118">Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet.</span><span class="sxs-lookup"><span data-stu-id="ca14d-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="ca14d-119">Siguen todos los paquetes en tiempo de ejecución el [control de versiones semántico](http://semver.org/) especificación.</span><span class="sxs-lookup"><span data-stu-id="ca14d-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="ca14d-120">El paquete de RTM de ASP.NET Web API 2.1 más reciente tiene la siguiente versión: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="ca14d-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="ca14d-121">Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="ca14d-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="ca14d-122">La versión también incluye paquetes localizados correspondientes en NuGet.</span><span class="sxs-lookup"><span data-stu-id="ca14d-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="ca14d-123">Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="ca14d-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="ca14d-124">Documentación</span><span class="sxs-lookup"><span data-stu-id="ca14d-124">Documentation</span></span>

<span data-ttu-id="ca14d-125">Tutoriales y otra información sobre ASP.NET Web API 2.1 RTM están disponibles desde el sitio web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="ca14d-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="ca14d-126">Nuevas características de ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="ca14d-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="ca14d-127">Control de errores global</span><span class="sxs-lookup"><span data-stu-id="ca14d-127">Global Error Handling</span></span>

<span data-ttu-id="ca14d-128">Ahora se pueden registrar todas las excepciones no controladas a través de un mecanismo central, y se puede personalizar el comportamiento de las excepciones no controladas.</span><span class="sxs-lookup"><span data-stu-id="ca14d-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="ca14d-129">El marco de trabajo admite varios registradores de excepciones, que se vea la excepción no controlada y la información sobre el contexto en el que se produjo, por ejemplo, la solicitud que se está procesando en el momento.</span><span class="sxs-lookup"><span data-stu-id="ca14d-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="ca14d-130">Por ejemplo, el código siguiente usa System.Diagnostics.TraceSource para registrar todas las excepciones no controladas:</span><span class="sxs-lookup"><span data-stu-id="ca14d-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="ca14d-131">También puede reemplazar el controlador de excepciones de manera predeterminada, por lo que puede personalizar completamente el mensaje de respuesta HTTP que se envía cuando una excepción no controlada se produce.</span><span class="sxs-lookup"><span data-stu-id="ca14d-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="ca14d-132">Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra todas las no controladas excepciones a través del marco ELMAH popular.</span><span class="sxs-lookup"><span data-stu-id="ca14d-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="ca14d-133">Atributo mejoras de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="ca14d-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="ca14d-134">Ruta de atributo ahora es compatible con las restricciones, habilitar el control de versiones y la selección de rutas basadas en el encabezado.</span><span class="sxs-lookup"><span data-stu-id="ca14d-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="ca14d-135">Además, muchos aspectos de las rutas de atributo ahora son personalizables a través de la **IDirectRouteFactory** interfaz y **RouteFactoryAttribute** clase.</span><span class="sxs-lookup"><span data-stu-id="ca14d-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="ca14d-136">El prefijo de ruta ahora es extensible a través de la **IRoutePrefix** interfaz y **RoutePrefixAttribute** clase.</span><span class="sxs-lookup"><span data-stu-id="ca14d-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="ca14d-137">Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que utiliza las restricciones para filtrar dinámicamente los controladores por un encabezado HTTP 'api-version'.</span><span class="sxs-lookup"><span data-stu-id="ca14d-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="ca14d-138">Mejoras de la página de ayuda</span><span class="sxs-lookup"><span data-stu-id="ca14d-138">Help Page Improvements</span></span>

<span data-ttu-id="ca14d-139">Web API 2.1 incluye las siguientes mejoras para [páginas de Ayuda de API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="ca14d-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="ca14d-140">Documentación de las propiedades individuales de los parámetros o tipos de valor devuelto de acciones.</span><span class="sxs-lookup"><span data-stu-id="ca14d-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="ca14d-141">Documentación de anotaciones del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="ca14d-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="ca14d-142">También se actualizó el diseño de interfaz de usuario de las páginas de ayuda, para dar cabida a estos cambios.</span><span class="sxs-lookup"><span data-stu-id="ca14d-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="ca14d-143">Compatibilidad con IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="ca14d-143">IgnoreRoute Support</span></span>

<span data-ttu-id="ca14d-144">Web API 2.1 es compatible con omitiendo los modelos de dirección URL en el enrutamiento de API Web a través de un conjunto de **IgnoreRoute** métodos de extensión en **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="ca14d-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="ca14d-145">Estos métodos generan Web API pasar por alto las direcciones URL que coinciden con una plantilla especificada y permitir que el host aplicar un procesamiento adicional si es necesario.</span><span class="sxs-lookup"><span data-stu-id="ca14d-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="ca14d-146">En el ejemplo siguiente, se omite el URI que se inician con un &quot;contenido&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="ca14d-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="ca14d-147">Formateador de tipo de medio BSON</span><span class="sxs-lookup"><span data-stu-id="ca14d-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="ca14d-148">Web API ahora admite la [BSON](http://bsonspec.org/) formato, en el cliente y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ca14d-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="ca14d-149">Para habilitar BSON en el servidor, agregue el **BsonMediaTypeFormatter** a la colección de formateadores:</span><span class="sxs-lookup"><span data-stu-id="ca14d-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="ca14d-150">Le mostramos cómo un cliente .NET puede consumir formato BSON:</span><span class="sxs-lookup"><span data-stu-id="ca14d-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="ca14d-151">Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que muestra el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="ca14d-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="ca14d-152">Para obtener más información, vea [soporte BSON en Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="ca14d-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="ca14d-153">Una mejor compatibilidad con filtros de Async</span><span class="sxs-lookup"><span data-stu-id="ca14d-153">Better Support for Async Filters</span></span>

<span data-ttu-id="ca14d-154">Web API ahora es compatible con una forma sencilla de crear filtros que se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ca14d-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="ca14d-155">Esta característica es útil es el filtro que se necesita para realizar una acción asincrónica, como el acceso a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ca14d-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="ca14d-156">Anteriormente, para crear un filtro de async, se tenían que implementar la interfaz de filtro usted mismo, ya que las clases de base de filtro solo exponen métodos sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="ca14d-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="ca14d-157">Ahora puede invalidar el virtual `On*Async` métodos del filtro de clase base.</span><span class="sxs-lookup"><span data-stu-id="ca14d-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="ca14d-158">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ca14d-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="ca14d-159">El **AuthorizationFilterAttribute**, **ActionFilterAttribute**, y **ExceptionFilterAttribute** todas clases admiten asincrónico en Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca14d-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="ca14d-160">Consulta de análisis para el cliente de biblioteca de formato</span><span class="sxs-lookup"><span data-stu-id="ca14d-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="ca14d-161">Anteriormente, **System.Net.Http.Formatting** admiten análisis y actualizar las consultas URI para el código de servidor, pero la biblioteca portable equivalente faltaba esta característica.</span><span class="sxs-lookup"><span data-stu-id="ca14d-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="ca14d-162">En la Web API 2.1, una aplicación cliente puede ahora fácilmente analizar y actualizar una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="ca14d-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="ca14d-163">Los ejemplos siguientes muestran cómo analizar, modificar y generar las consultas URI.</span><span class="sxs-lookup"><span data-stu-id="ca14d-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="ca14d-164">(Los ejemplos muestran una aplicación de consola para simplificar el trabajo).</span><span class="sxs-lookup"><span data-stu-id="ca14d-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="ca14d-165">Problemas conocidos y los cambios recientes</span><span class="sxs-lookup"><span data-stu-id="ca14d-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="ca14d-166">Esta sección describen problemas conocidos y cambios importantes en la RTM de ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca14d-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="ca14d-167">Ruta de atributo</span><span class="sxs-lookup"><span data-stu-id="ca14d-167">Attribute Routing</span></span>

<span data-ttu-id="ca14d-168">Ambigüedades en coincidencias de enrutamiento de atributo ahora notifican un error en lugar de elegir a la primera coincidencia.</span><span class="sxs-lookup"><span data-stu-id="ca14d-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="ca14d-169">Rutas de atributo se les prohíbe usar la *{controller}* parámetro y del uso de la *{action}* parámetro en las rutas se coloca en las acciones.</span><span class="sxs-lookup"><span data-stu-id="ca14d-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="ca14d-170">Estos parámetros es muy probable que hará que las ambigüedades.</span><span class="sxs-lookup"><span data-stu-id="ca14d-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="ca14d-171">Scaffolding MVC o Web API en un proyecto con 5.1 resultados de paquetes en 5.0 paquetes para los que ya no existen en el proyecto</span><span class="sxs-lookup"><span data-stu-id="ca14d-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="ca14d-172">Actualizar paquetes de NuGet para ASP.NET Web API 2.1 RTM no actualizar las herramientas de Visual Studio, como la técnica scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ca14d-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="ca14d-173">Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="ca14d-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="ca14d-174">Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos.</span><span class="sxs-lookup"><span data-stu-id="ca14d-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="ca14d-175">Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 sobrescribir los paquetes más recientes de los proyectos.</span><span class="sxs-lookup"><span data-stu-id="ca14d-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="ca14d-176">Si utiliza ASP.NET scaffolding después de actualizar los paquetes de Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y MVC son coherentes.</span><span class="sxs-lookup"><span data-stu-id="ca14d-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="ca14d-177">Cambia el nombre de tipo</span><span class="sxs-lookup"><span data-stu-id="ca14d-177">Type Renames</span></span>

<span data-ttu-id="ca14d-178">Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributo se cambió el nombre de la versión RC a RTM 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca14d-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="ca14d-179">Nombre de tipo anterior (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="ca14d-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="ca14d-180">Nuevo tipo de nombre (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="ca14d-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="ca14d-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="ca14d-181">IDirectRouteProvider</span></span> | <span data-ttu-id="ca14d-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="ca14d-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="ca14d-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="ca14d-183">RouteProviderAttribute</span></span> | <span data-ttu-id="ca14d-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="ca14d-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="ca14d-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="ca14d-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="ca14d-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="ca14d-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="ca14d-187">Los filtros de excepción desencapsular agregadas excepciones en las acciones de asincrónico</span><span class="sxs-lookup"><span data-stu-id="ca14d-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="ca14d-188">Anteriormente, si una acción asincrónica produjo una **AggregateException**, un filtro de excepción podría unwrap la excepción, y **OnException** obtendría la excepción base.</span><span class="sxs-lookup"><span data-stu-id="ca14d-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="ca14d-189">En 2.1, el filtro de excepción desencapsular, y **OnException** Obtiene la versión original **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="ca14d-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="ca14d-190">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="ca14d-190">Bug Fixes</span></span>

<span data-ttu-id="ca14d-191">Esta versión también incluye varias correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="ca14d-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="ca14d-192">Puede encontrar la lista completa aquí:</span><span class="sxs-lookup"><span data-stu-id="ca14d-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="ca14d-193">5.1.0 paquete</span><span class="sxs-lookup"><span data-stu-id="ca14d-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="ca14d-194">5.1.1 paquete de</span><span class="sxs-lookup"><span data-stu-id="ca14d-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="ca14d-195">El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="ca14d-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
