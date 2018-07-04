---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Novedades de ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377446"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="77891-102">Novedades de ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="77891-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="77891-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="77891-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="77891-104">Este tema describe cuáles son las novedades para ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="77891-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="77891-105">Descarga</span><span class="sxs-lookup"><span data-stu-id="77891-105">Download</span></span>](#download)
- [<span data-ttu-id="77891-106">Documentación</span><span class="sxs-lookup"><span data-stu-id="77891-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="77891-107">Nuevas características en ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="77891-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="77891-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="77891-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="77891-109">Mejoras de enrutamiento de atributo</span><span class="sxs-lookup"><span data-stu-id="77891-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="77891-110">Soporte de cliente de API Web para Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="77891-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="77891-111">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="77891-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="77891-112">Correcciones de errores</span><span class="sxs-lookup"><span data-stu-id="77891-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="77891-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="77891-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="77891-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="77891-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="77891-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="77891-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="77891-116">Descargar</span><span class="sxs-lookup"><span data-stu-id="77891-116">Download</span></span>

<span data-ttu-id="77891-117">Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet.</span><span class="sxs-lookup"><span data-stu-id="77891-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="77891-118">Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación.</span><span class="sxs-lookup"><span data-stu-id="77891-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="77891-119">El paquete más reciente de ASP.NET Web API 2.2 tiene la siguiente versión: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="77891-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="77891-120">Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="77891-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="77891-121">La versión también incluye los correspondientes paquetes localizados en NuGet.</span><span class="sxs-lookup"><span data-stu-id="77891-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="77891-122">Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="77891-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="77891-123">Documentación</span><span class="sxs-lookup"><span data-stu-id="77891-123">Documentation</span></span>

<span data-ttu-id="77891-124">Tutoriales y otra información sobre ASP.NET Web API 2.2 están disponibles desde el sitio web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="77891-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="77891-125">Nuevas características en ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="77891-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="77891-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="77891-126">OData v4</span></span>

<span data-ttu-id="77891-127">Esta versión agrega compatibilidad con el Protocolo OData v4.</span><span class="sxs-lookup"><span data-stu-id="77891-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="77891-128">Para obtener más información, consulte el [documentación de Web API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="77891-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="77891-129">Estas son algunas de las características clave y los cambios de OData v4:</span><span class="sxs-lookup"><span data-stu-id="77891-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="77891-130">Compatibilidad con las propiedades de alias en el modelo de OData</span><span class="sxs-lookup"><span data-stu-id="77891-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="77891-131">Compatibilidad con ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute y ConcurrencyCheckAttribute en ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="77891-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="77891-132">Proporcionar la capacidad de proporcionar un título descriptivo para las acciones</span><span class="sxs-lookup"><span data-stu-id="77891-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="77891-133">Integrar con ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="77891-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="77891-134">Compatibilidad con [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contención](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) y [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="77891-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="77891-135">Compatibilidad con conversión de tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="77891-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="77891-136">Se ha agregado compatibilidad de la función de OData</span><span class="sxs-lookup"><span data-stu-id="77891-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="77891-137">Alias de parámetro de soporte técnico para las llamadas de función</span><span class="sxs-lookup"><span data-stu-id="77891-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="77891-138">Admite la convención de nomenclatura de mayúsculas y minúsculas en el modelo</span><span class="sxs-lookup"><span data-stu-id="77891-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="77891-139">Compatibilidad con cast() en $filter</span><span class="sxs-lookup"><span data-stu-id="77891-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="77891-140">Soporte técnico para el tipo complejo abierto</span><span class="sxs-lookup"><span data-stu-id="77891-140">Support for open complex type</span></span>
- <span data-ttu-id="77891-141">Quitado EntitySetController y AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="77891-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="77891-142">$Link modificadas a $ref</span><span class="sxs-lookup"><span data-stu-id="77891-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="77891-143">Se ha agregado compatibilidad con enrutamiento del atributo</span><span class="sxs-lookup"><span data-stu-id="77891-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="77891-144">Usa las bibliotecas de OData Core 6.4.0</span><span class="sxs-lookup"><span data-stu-id="77891-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="77891-145">Mejoras de enrutamiento de atributo</span><span class="sxs-lookup"><span data-stu-id="77891-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="77891-146">Enrutamiento mediante atributos ahora proporciona un punto de extensibilidad llamado IDirectRouteProvider, lo que permite un control total sobre cómo se detectan y configurar las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="77891-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="77891-147">Un IDirectRouteProvider es responsable de proporcionar una lista de acciones y los controladores, junto con información de ruta asociada para especificar exactamente qué configuración de enrutamiento se desea para esas acciones.</span><span class="sxs-lookup"><span data-stu-id="77891-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="77891-148">Una implementación IDirectRouteProvider puede especificarse cuando se llama a MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="77891-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="77891-149">Personalizar IDirectRouteProvider será más fácil extendiendo nuestra implementación predeterminada, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="77891-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="77891-150">Esta clase proporciona métodos virtuales que se puede invalidar independientes para cambiar la lógica para detectar atributos, crear entradas de ruta y detección de prefijo de ruta y el prefijo de área.</span><span class="sxs-lookup"><span data-stu-id="77891-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="77891-151">Estos son algunos ejemplos en lo que podría hacer con este nuevo punto de extensibilidad:</span><span class="sxs-lookup"><span data-stu-id="77891-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="77891-152">Admite la herencia de atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="77891-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="77891-153">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="77891-153">Example:</span></span>

    <span data-ttu-id="77891-154">Aquí una solicitud like "/ api/10/valores" devolvería correctamente "Éxito: 10"</span><span class="sxs-lookup"><span data-stu-id="77891-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="77891-155">Proporcione un nombre de ruta predeterminada para las rutas de atributo siguiendo alguna convención que desee.</span><span class="sxs-lookup"><span data-stu-id="77891-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="77891-156">De forma predeterminada, el enrutamiento mediante atributos no crea automáticamente los nombres de las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="77891-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="77891-157">Modificar plantilla de ruta de las rutas de atributo en un lugar central antes de acabar en la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="77891-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="77891-158">Compatibilidad de cliente de la API de Web para Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="77891-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="77891-159">Ahora puede usar el paquete NuGet de Web API cliente para implementar la lógica de cliente de API Web cuando el destino es Windows Phone 8.1 o desde dentro de una aplicación Universal.</span><span class="sxs-lookup"><span data-stu-id="77891-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="77891-160">Problemas conocidos y cambios importantes</span><span class="sxs-lookup"><span data-stu-id="77891-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="77891-161">Esta sección describen problemas conocidos y cambios importantes en la ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="77891-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="77891-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="77891-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="77891-163">Generador de modelos</span><span class="sxs-lookup"><span data-stu-id="77891-163">Model builder</span></span>

<span data-ttu-id="77891-164">Problema: No se pudieran exponer funciones sobrecargadas como FunctionImport</span><span class="sxs-lookup"><span data-stu-id="77891-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="77891-165">Si hay 2 funciones sobrecargadas y también son FunctionImport tal como se muestra a continuación, a continuación, solicitar resultados ~/GetAllConventionCustomers(CustomerName={customerName}) en System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="77891-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="77891-166">Solución alternativa: La solución alternativa para este problema consiste en agregar ambos las sobrecargas de función como FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="77891-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="77891-167">Enrutamiento de OData</span><span class="sxs-lookup"><span data-stu-id="77891-167">OData Routing</span></span>

<span data-ttu-id="77891-168">Literales de cadena que incluyen la dirección URL codifican de barra diagonal (% 2F) y backslash(%5C) generar un error 404 cuando se usan en las rutas de acceso del recurso de OData.</span><span class="sxs-lookup"><span data-stu-id="77891-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="77891-169">Por ejemplo, los literales de cadena se pueden usar en las rutas de acceso del recurso de OData como parámetros de funciones o valores de clave de conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="77891-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="77891-170">/Employees/total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="77891-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="77891-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="77891-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="77891-172">Cuando dichas solicitudes el escape de Naciones Unidas de voluntad de hosts de recepción de servicios las secuencias de escape de antes de pasarlas al tiempo de ejecución de API Web.</span><span class="sxs-lookup"><span data-stu-id="77891-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="77891-173">Esto protege contra ataques similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="77891-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="77891-174">Esto hace que la pila de OData de Web API devolver un error 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="77891-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="77891-175">Para evitar este error, el cliente debería utilizar las secuencias de escape doble barra diagonal (% 252F) y barra diagonal inversa (% C de 255).</span><span class="sxs-lookup"><span data-stu-id="77891-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="77891-176">Esto no ocurre para las cadenas de consulta como /Employees? $filter = nombre eq 'Nombre % 2F'</span><span class="sxs-lookup"><span data-stu-id="77891-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="77891-177">**Tenga en cuenta las barras diagonales sin escape ('/') y barras diagonales inversas (") no son válidas en literales de cadena de ruta de acceso de recurso de OData. Las barras diagonales deben aparecer sólo como separadores de ruta de acceso y barras diagonales inversas no deben aparecer en la ruta de acceso del recurso de OData en absoluto. (Ambos son utilizables en algunas partes de una cadena de consulta de OData).**</span><span class="sxs-lookup"><span data-stu-id="77891-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="77891-178">Solución alternativa: Podría invalidar el método Parse de DefaultODataPathHandler a la barra diagonal y una barra diagonal inversa en literales de cadena de escape antes de analizarlos realmente.</span><span class="sxs-lookup"><span data-stu-id="77891-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="77891-179">Puede encontrar un ejemplo de este enfoque aquí.</span><span class="sxs-lookup"><span data-stu-id="77891-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="77891-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="77891-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="77891-181">[Queryable]</span><span class="sxs-lookup"><span data-stu-id="77891-181">[Queryable]</span></span>

<span data-ttu-id="77891-182">El atributo [Queryable] está desusado.</span><span class="sxs-lookup"><span data-stu-id="77891-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="77891-183">Deben usar nuevas aplicaciones de OData v3 **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="77891-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="77891-184">El **ODataHttpConfigurationExtensions.EnableQuerySupport** método de extensión se agrega ahora un **EnableQueryAttribute** a la colección de filtros globales.</span><span class="sxs-lookup"><span data-stu-id="77891-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="77891-185">Si tiene alguno de los controladores del **[Queryable]** de atributo, una llamada a `config.EnableQuerySupport()` hará que el **[Queryable]** atributo a un error</span><span class="sxs-lookup"><span data-stu-id="77891-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="77891-186">La manera recomendada para resolver este problema es reemplazar todas las instancias de **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="77891-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="77891-187">Una solución alternativa es usar el código siguiente en la configuración de la API Web:</span><span class="sxs-lookup"><span data-stu-id="77891-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="77891-188">Enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="77891-188">Attribute Routing</span></span>

<span data-ttu-id="77891-189">Problema: Enlace de modelos del tipo complejo que se decora con el atributo FromUri tiene un comportamiento diferente cuando se usa el enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="77891-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="77891-190">Vínculo siguiente está realizando un seguimiento del problema y también incluye detalles sobre una solución alternativa.</span><span class="sxs-lookup"><span data-stu-id="77891-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="77891-191">Problema: Scaffolding MVC o API Web en un proyecto sin 5.2.0 los resultados de los paquetes en 5.1.2 paquetes para las que aún no existen en el proyecto</span><span class="sxs-lookup"><span data-stu-id="77891-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="77891-192">Actualizar paquetes de NuGet para ASP.NET MVC 5.2 no actualiza las herramientas de Visual Studio como el scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77891-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="77891-193">Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (por ejemplo, 5.1.2 en Update 2).</span><span class="sxs-lookup"><span data-stu-id="77891-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="77891-194">Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (por ejemplo, 5.1.2 en Update 2) de los paquetes necesarios, si aún no están disponibles en los proyectos.</span><span class="sxs-lookup"><span data-stu-id="77891-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="77891-195">Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 no sobrescribe los paquetes más recientes en sus proyectos.</span><span class="sxs-lookup"><span data-stu-id="77891-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="77891-196">Si usa scaffolding de ASP.NET después de actualizar los paquetes de los proyectos de Web API 2.2 o ASP.NET MVC 5.2, asegúrese de que las versiones de API Web y ASP.NET MVC son coherentes.</span><span class="sxs-lookup"><span data-stu-id="77891-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="77891-197">Correcciones de errores y las actualizaciones de características secundarias</span><span class="sxs-lookup"><span data-stu-id="77891-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="77891-198">Esta versión también incluye varias correcciones de errores y una característica secundaria actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="77891-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="77891-199">Puede encontrar una lista completa aquí:</span><span class="sxs-lookup"><span data-stu-id="77891-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="77891-200">paquete 5.2</span><span class="sxs-lookup"><span data-stu-id="77891-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="77891-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="77891-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="77891-202">El paquete Microsoft.AspNet.OData 5.2.1 contiene actualizaciones de dependencias de NuGet, pero no hay correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="77891-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="77891-203">Con esta actualización, ya no hay una dependencia estricta en Microsoft.OData.Core 6.4.0, pero uno puede actualizar a cualquier versión entre 6.4.0 y 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="77891-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="77891-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="77891-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="77891-205">En esta versión hemos creado una dependencia de cambio para `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="77891-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="77891-206">Para obtener más información sobre lo que es nuevo en esta versión de `Json.NET`, consulte [Json.NET 6.0 Release 4 - JSON Merge, inserción de dependencias](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="77891-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="77891-207">Esta versión no tiene otras características nuevas ni correcciones de errores en la API Web.</span><span class="sxs-lookup"><span data-stu-id="77891-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="77891-208">Hemos actualizado posteriormente todos los demás paquetes dependientes que poseemos para depender de esta nueva versión de API Web.</span><span class="sxs-lookup"><span data-stu-id="77891-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="77891-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="77891-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="77891-210">Puede leer sobre la versión [aquí](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="77891-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="77891-211">Esta versión contiene correcciones de errores solo.</span><span class="sxs-lookup"><span data-stu-id="77891-211">This release contains only bug fixes.</span></span> <span data-ttu-id="77891-212">Puede usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver la lista de los problemas corregidos en esta versión.</span><span class="sxs-lookup"><span data-stu-id="77891-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
