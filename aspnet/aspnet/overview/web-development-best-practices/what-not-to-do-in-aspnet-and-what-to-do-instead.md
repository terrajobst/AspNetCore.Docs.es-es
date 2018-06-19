---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Qué no hacer en ASP.NET y qué hacer en su lugar | Documentos de Microsoft
author: tfitzmac
description: Este tema describe varios errores comunes que se pueden cometer dentro de los proyectos web ASP.NET. Proporciona recomendaciones para lo que debe hacer para evitar estos cargándola...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034925"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="309ed-104">Qué no hacer en ASP.NET y qué hacer en su lugar</span><span class="sxs-lookup"><span data-stu-id="309ed-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="309ed-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="309ed-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="309ed-106">Este tema describe varios errores comunes que se pueden cometer dentro de los proyectos web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="309ed-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="309ed-107">Proporciona recomendaciones para lo que debe hacer para evitar estos errores comunes.</span><span class="sxs-lookup"><span data-stu-id="309ed-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="309ed-108">Se basa en un [presentación](http://vimeo.com/68390507) por **Damian Edwards** en noruego conferencia de desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="309ed-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="309ed-109">Declinación de responsabilidades</span><span class="sxs-lookup"><span data-stu-id="309ed-109">Disclaimer</span></span>

<span data-ttu-id="309ed-110">En este tema no está diseñada como una guía completa para garantizar que su aplicación sea segura y eficaz.</span><span class="sxs-lookup"><span data-stu-id="309ed-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="309ed-111">Deberá seguir las prácticas recomendadas para la seguridad y rendimiento que no se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="309ed-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="309ed-112">Solo se sugiere cómo evitar los errores más comunes relacionados con procesos y las clases. NET.</span><span class="sxs-lookup"><span data-stu-id="309ed-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="309ed-113">Información general</span><span class="sxs-lookup"><span data-stu-id="309ed-113">Overview</span></span>

<span data-ttu-id="309ed-114">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="309ed-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="309ed-115">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="309ed-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="309ed-116">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="309ed-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="309ed-117">Propiedades de estilo de los controles</span><span class="sxs-lookup"><span data-stu-id="309ed-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="309ed-118">Página y las devoluciones de llamada de Control</span><span class="sxs-lookup"><span data-stu-id="309ed-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="309ed-119">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="309ed-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="309ed-120">Seguridad</span><span class="sxs-lookup"><span data-stu-id="309ed-120">Security</span></span>](#security)

    - [<span data-ttu-id="309ed-121">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="309ed-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="309ed-122">Sesión y autenticación de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="309ed-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="309ed-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="309ed-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="309ed-124">Nivel de confianza medio</span><span class="sxs-lookup"><span data-stu-id="309ed-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="309ed-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="309ed-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="309ed-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="309ed-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="309ed-127">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="309ed-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="309ed-128">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="309ed-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="309ed-129">Eventos de página asincrónica con formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="309ed-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="309ed-130">Fire-and-Forget Work</span><span class="sxs-lookup"><span data-stu-id="309ed-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="309ed-131">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="309ed-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="309ed-132">Response.Redirect y Response.End</span><span class="sxs-lookup"><span data-stu-id="309ed-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="309ed-133">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="309ed-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="309ed-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="309ed-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="309ed-135">Las solicitudes de ejecución largas (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="309ed-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="309ed-136">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="309ed-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="309ed-137">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="309ed-137">Control Adapters</span></span>

<span data-ttu-id="309ed-138">Recomendación: Deje de usar adaptadores de control para la representación adaptable y, en su lugar, use las consultas de medios CSS y compatible con los estándares HTML.</span><span class="sxs-lookup"><span data-stu-id="309ed-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="309ed-139">Adaptadores de controles se introdujeron en .NET 2.0 para representar el código de presentación que se ha personalizado para diferentes dispositivos y entornos.</span><span class="sxs-lookup"><span data-stu-id="309ed-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="309ed-140">Ahora, esta representación adaptable puede realizarse con CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="309ed-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="309ed-141">Debe dejar de usar adaptadores de Control y convertir a los adaptadores existentes en CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="309ed-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="309ed-142">Para obtener más información, consulte [consultas de medios](http://www.w3.org/TR/css3-mediaqueries/) y [Cómo: agregar páginas móviles a los formularios ASP.NET Web Forms / aplicación MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="309ed-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="309ed-143">Propiedades de estilo de los controles</span><span class="sxs-lookup"><span data-stu-id="309ed-143">Style Properties on Controls</span></span>

<span data-ttu-id="309ed-144">Recomendación: Detenga establecer valores de estilo en el marcado del control y, en su lugar, establezca los valores de formato en las hojas de estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="309ed-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="309ed-145">Controles de servidor Web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea.</span><span class="sxs-lookup"><span data-stu-id="309ed-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="309ed-146">Por ejemplo, la propiedad de color de primer plano establece el color del texto de un control.</span><span class="sxs-lookup"><span data-stu-id="309ed-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="309ed-147">Puede realizar este mismo efecto más eficazmente a través de las hojas de estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="309ed-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="309ed-148">Hojas de estilos permiten centralizar los valores de estilo y no es necesario establecer estos valores a lo largo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="309ed-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="309ed-149">En el ejemplo siguiente se muestra una clase CSS el texto de los conjuntos a rojo.</span><span class="sxs-lookup"><span data-stu-id="309ed-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="309ed-150">En el ejemplo siguiente se muestra cómo aplicar dinámicamente la clase CSS.</span><span class="sxs-lookup"><span data-stu-id="309ed-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="309ed-151">Página y las devoluciones de llamada de Control</span><span class="sxs-lookup"><span data-stu-id="309ed-151">Page and Control Callbacks</span></span>

<span data-ttu-id="309ed-152">Recomendación: Deje de usar devoluciones de llamada de página y de control y, en su lugar, use cualquiera de los siguientes: AJAX, UpdatePanel, métodos de acción de MVC, Web API o SignalR.</span><span class="sxs-lookup"><span data-stu-id="309ed-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="309ed-153">En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y un Control habilitan actualizar parte de la página web sin necesidad de actualizar una página completa.</span><span class="sxs-lookup"><span data-stu-id="309ed-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="309ed-154">Ahora puede realizar actualizaciones parciales de página a través de [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="309ed-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="309ed-155">Debe detener mediante los métodos de devolución de llamada porque pueden causar problemas con direcciones URL descriptivas y enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="309ed-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="309ed-156">De forma predeterminada, los controles no habilitar métodos de devolución de llamada, pero si habilita esta característica en un control, debe deshabilitarlo.</span><span class="sxs-lookup"><span data-stu-id="309ed-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="309ed-157">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="309ed-157">Browser Capability Detection</span></span>

<span data-ttu-id="309ed-158">Recomendación: Detenga usando la detección de la capacidad de explorador estático y, en su lugar, utilizar la detección de función dinámica.</span><span class="sxs-lookup"><span data-stu-id="309ed-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="309ed-159">En versiones anteriores de ASP.NET, las características admitidas para cada explorador que se almacenaban en un archivo XML.</span><span class="sxs-lookup"><span data-stu-id="309ed-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="309ed-160">Compatibilidad de la característica de detección a través de una búsqueda estática no es el mejor método.</span><span class="sxs-lookup"><span data-stu-id="309ed-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="309ed-161">Ahora, puede detectar dinámicamente un explorador la admite características mediante un marco de trabajo de detección de función, como [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="309ed-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="309ed-162">Detección de características determina la compatibilidad con cualquier intento de usar un método o propiedad y, a continuación, comprueba si el explorador genera el resultado deseado.</span><span class="sxs-lookup"><span data-stu-id="309ed-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="309ed-163">De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="309ed-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="309ed-164">Seguridad</span><span class="sxs-lookup"><span data-stu-id="309ed-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="309ed-165">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="309ed-165">Request Validation</span></span>

<span data-ttu-id="309ed-166">Recomendación: Validar proporcionados por el usuario y codifique la salida de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="309ed-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="309ed-167">Validación de solicitud es una característica de ASP.NET que inspecciona cada solicitud y detiene la solicitud si no se encuentra una amenaza percibida.</span><span class="sxs-lookup"><span data-stu-id="309ed-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="309ed-168">No dependen de la validación de solicitudes para proteger la aplicación frente a ataques de scripts entre sitios.</span><span class="sxs-lookup"><span data-stu-id="309ed-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="309ed-169">En su lugar, validar todas las entradas de los usuarios y codificar la salida.</span><span class="sxs-lookup"><span data-stu-id="309ed-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="309ed-170">En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados que debe validar proporcionados por el usuario mediante el uso de clases de .NET que determinan si el valor coincide con valores permiten.</span><span class="sxs-lookup"><span data-stu-id="309ed-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="309ed-171">En el ejemplo siguiente se muestra cómo utilizar un método estático en la clase Uri para determinar si el Uri proporcionado por el usuario es válido.</span><span class="sxs-lookup"><span data-stu-id="309ed-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="309ed-172">Sin embargo, para comprobar lo suficientemente el Uri, también debe comprobar para asegurarse de que especifica `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="309ed-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="309ed-173">En el ejemplo siguiente se usa métodos de instancia para comprobar que el Uri es válido.</span><span class="sxs-lookup"><span data-stu-id="309ed-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="309ed-174">Antes de presentar proporcionados por el usuario como HTML o incluir proporcionados por el usuario en una consulta SQL, codificar los valores para asegurarse de que no se incluye el código malintencionado.</span><span class="sxs-lookup"><span data-stu-id="309ed-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="309ed-175">Puede HTML codificar el valor en el marcado con el &lt;%: %&gt; sintaxis, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="309ed-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="309ed-176">O bien, en la sintaxis de Razor, puede HTML codificar con @, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="309ed-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="309ed-177">El siguiente ejemplo se muestra cómo a HTML codifica un valor en el código subyacente.</span><span class="sxs-lookup"><span data-stu-id="309ed-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="309ed-178">Para codificar de forma segura un valor para los comandos SQL, use los parámetros de comando como el [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="309ed-179">Sesión y autenticación de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="309ed-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="309ed-180">Recomendación: Necesita cookies.</span><span class="sxs-lookup"><span data-stu-id="309ed-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="309ed-181">No es seguro pasar información de autenticación en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="309ed-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="309ed-182">Por lo tanto, requieren cookies cuando la aplicación incluye la autenticación.</span><span class="sxs-lookup"><span data-stu-id="309ed-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="309ed-183">Si la cookie almacena información confidencial, considere la posibilidad de exigir SSL para la cookie.</span><span class="sxs-lookup"><span data-stu-id="309ed-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="309ed-184">En el ejemplo siguiente se muestra cómo especificar en el archivo Web.config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.</span><span class="sxs-lookup"><span data-stu-id="309ed-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="309ed-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="309ed-185">EnableViewStateMac</span></span>

<span data-ttu-id="309ed-186">Recomendación: Nunca establecida en false.</span><span class="sxs-lookup"><span data-stu-id="309ed-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="309ed-187">De forma predeterminada, se establece EnbableViewStateMac en true.</span><span class="sxs-lookup"><span data-stu-id="309ed-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="309ed-188">Incluso si la aplicación no utiliza el estado de vista, no establezca EnableViewStateMac en false.</span><span class="sxs-lookup"><span data-stu-id="309ed-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="309ed-189">Establecer este valor en false, la aplicación hará que sea vulnerable a scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="309ed-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="309ed-190">A partir de ASP.NET 4.5.2, el tiempo de ejecución impone **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="309ed-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="309ed-191">Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true.</span><span class="sxs-lookup"><span data-stu-id="309ed-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="309ed-192">Para obtener más información, consulte [ASP.NET 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="309ed-193">En el ejemplo siguiente se muestra cómo establecer EnableViewStateMac en true.</span><span class="sxs-lookup"><span data-stu-id="309ed-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="309ed-194">No es necesario establecer este valor en true porque es true de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="309ed-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="309ed-195">Sin embargo, si se configura en false en cualquier página en la aplicación, debe corregir inmediatamente este valor.</span><span class="sxs-lookup"><span data-stu-id="309ed-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="309ed-196">Nivel de confianza medio</span><span class="sxs-lookup"><span data-stu-id="309ed-196">Medium Trust</span></span>

<span data-ttu-id="309ed-197">Recomendación: No dependen de nivel de confianza medio (o cualquier otro nivel de confianza) como un límite de seguridad.</span><span class="sxs-lookup"><span data-stu-id="309ed-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="309ed-198">Confianza parcial no protegen adecuadamente la aplicación y no debe usarse.</span><span class="sxs-lookup"><span data-stu-id="309ed-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="309ed-199">En su lugar, use la plena confianza y aislar las aplicaciones que no se confía en grupos de aplicaciones independientes.</span><span class="sxs-lookup"><span data-stu-id="309ed-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="309ed-200">Asimismo, ejecutar bajo una identidad única para cada grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="309ed-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="309ed-201">Para obtener más información, consulte [confianza parcial de ASP.NET no garantiza el aislamiento de aplicaciones](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="309ed-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="309ed-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="309ed-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="309ed-203">Recomendación: Deshabilitar la configuración de seguridad en &lt;appSettings&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="309ed-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="309ed-204">El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="309ed-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="309ed-205">No debe cambiar o deshabilitar estos valores.</span><span class="sxs-lookup"><span data-stu-id="309ed-205">You should not change or disable these values.</span></span> <span data-ttu-id="309ed-206">Si debe deshabilitar estos valores cuando se implementa una actualización, inmediatamente volver a habilitar después de completar la implementación.</span><span class="sxs-lookup"><span data-stu-id="309ed-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="309ed-207">Para obtener más información, consulte [appSettings ASP.NET elemento](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="309ed-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="309ed-208">UrlPathEncode</span></span>

<span data-ttu-id="309ed-209">Recomendación: Utilizar [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="309ed-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="309ed-210">El método UrlPathEncode se ha agregado a .NET Framework para resolver un problema de compatibilidad de explorador muy específico.</span><span class="sxs-lookup"><span data-stu-id="309ed-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="309ed-211">No realiza ninguna codificación adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="309ed-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="309ed-212">Nunca debería utilizarlo en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="309ed-212">You should never use it in your application.</span></span> <span data-ttu-id="309ed-213">En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="309ed-214">En el ejemplo siguiente se muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.</span><span class="sxs-lookup"><span data-stu-id="309ed-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="309ed-215">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="309ed-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="309ed-216">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="309ed-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="309ed-217">Recomendación: No use estos eventos con los módulos administrados.</span><span class="sxs-lookup"><span data-stu-id="309ed-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="309ed-218">En su lugar, escribir un módulo nativo de IIS para llevar a cabo la tarea requerida.</span><span class="sxs-lookup"><span data-stu-id="309ed-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="309ed-219">Vea [crear módulos HTTP de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="309ed-220">Puede usar el [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) eventos con los módulos nativos de IIS.</span><span class="sxs-lookup"><span data-stu-id="309ed-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="309ed-221">No utilice `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="309ed-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="309ed-222">Al establecer estas propiedades puede causar problemas con solicitudes asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="309ed-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="309ed-223">La combinación de enrutamiento solicitado aplicaciones (ARR) y websockets podría provocar excepciones de infracción de acceso que pueden causar w3wp se bloquee.</span><span class="sxs-lookup"><span data-stu-id="309ed-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="309ed-224">¡Por ejemplo, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 en iiscore.dll ha provocado una excepción de infracción de acceso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="309ed-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="309ed-225">Eventos de página asincrónica con formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="309ed-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="309ed-226">Recomendación: En formularios Web Forms, evite escribir async void métodos para eventos de ciclo de vida de la página y en su lugar, utilice [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="309ed-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="309ed-227">Cuando marca un evento de página con **async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="309ed-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="309ed-228">En su lugar, utilice Page.RegisterAsyncTask para ejecutar el código asincrónico de forma que le permite realizar un seguimiento de su finalización.</span><span class="sxs-lookup"><span data-stu-id="309ed-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="309ed-229">El siguiente ejemplo se muestra un botón, haga clic en controlador que contiene código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="309ed-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="309ed-230">Este ejemplo incluye leer de forma asincrónica, el valor de cadena que se proporciona únicamente como un ejemplo simplificado de una tarea asincrónica y no como un procedimiento recomendado.</span><span class="sxs-lookup"><span data-stu-id="309ed-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="309ed-231">Si está utilizando las tareas asincrónicas, establecer la plataforma de destino en tiempo de ejecución de Http a 4.5 en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="309ed-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="309ed-232">Establecer la plataforma de destino en 4.5 activa en el nuevo contexto de sincronización que se agregó en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="309ed-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="309ed-233">Este valor se establece de forma predeterminada en los nuevos proyectos en Visual Studio 2012, pero no es puede ser establecido si está trabajando con un proyecto existente.</span><span class="sxs-lookup"><span data-stu-id="309ed-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="309ed-234">Enviar y olvidarse de trabajo</span><span class="sxs-lookup"><span data-stu-id="309ed-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="309ed-235">Recomendación: Al procesar una solicitud de ASP.NET, evite iniciar trabajo enviar y olvidarse (este tipo llamando al método ThreadPool.QueueUserWorkItem o creando un temporizador que llama repetidamente a un delegado).</span><span class="sxs-lookup"><span data-stu-id="309ed-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="309ed-236">Si la aplicación tiene trabajo enviar y olvidarse que se ejecuta en ASP.NET, la aplicación puede obtener la sincronización. En cualquier momento, el dominio de aplicación se puedan destruir lo que significa que el proceso actual ya no puede coincidir con el estado actual de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="309ed-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="309ed-237">Debe mover este tipo de trabajo fuera de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="309ed-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="309ed-238">Puede utilizar los trabajos Web, servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código desde otro proceso.</span><span class="sxs-lookup"><span data-stu-id="309ed-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="309ed-239">Si va a realizar este trabajo en ASP.NET, puede agregar el paquete de Nuget denominado [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.</span><span class="sxs-lookup"><span data-stu-id="309ed-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="309ed-240">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="309ed-240">Request Entity Body</span></span>

<span data-ttu-id="309ed-241">Recomendación: Evite leer Request.Form o Request.InputStream antes de que el controlador ejecute eventos.</span><span class="sxs-lookup"><span data-stu-id="309ed-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="309ed-242">El más antiguo debe leer desde Request.Form o Request.InputStream es durante el controlador ejecute eventos.</span><span class="sxs-lookup"><span data-stu-id="309ed-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="309ed-243">En MVC, el controlador es el controlador y el evento de ejecución es cuando se ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="309ed-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="309ed-244">En formularios Web Forms, la página es el controlador y el evento de ejecución es cuando se desencadene el evento Page.Init.</span><span class="sxs-lookup"><span data-stu-id="309ed-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="309ed-245">Si lee el cuerpo de la entidad de solicitud antes que el evento execute, interferir con el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="309ed-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="309ed-246">Si tiene que leer el cuerpo de entidad de solicitud antes del evento execute, use [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="309ed-247">Cuando usas GetBufferlessInputStream, obtener la secuencia sin formato de la solicitud y asuma la responsabilidad de procesamiento de la solicitud completa.</span><span class="sxs-lookup"><span data-stu-id="309ed-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="309ed-248">Después de llamar a GetBufferlessInputStream, Request.Form y Request.InputStream no están disponibles porque no se han rellenado por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="309ed-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="309ed-249">Cuando usas GetBufferedInputStream, obtendrá una copia de la secuencia de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="309ed-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="309ed-250">Request.Form y Request.InputStream siguen estando disponibles más adelante en la solicitud porque ASP.NET rellena la otra copia.</span><span class="sxs-lookup"><span data-stu-id="309ed-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="309ed-251">Response.Redirect y Response.End</span><span class="sxs-lookup"><span data-stu-id="309ed-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="309ed-252">Recomendación: Tener en cuenta las diferencias en cómo se controla el subproceso después de llamar a [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="309ed-253">El [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) método llama al método Response.End.</span><span class="sxs-lookup"><span data-stu-id="309ed-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="309ed-254">En un proceso sincrónico, una llamada a Request.Redirect hace que el subproceso actual se anula inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="309ed-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="309ed-255">Sin embargo, en un proceso asincrónico, la llamada a Response.Redirect no anula el subproceso actual, por lo que continúe la ejecución del código para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="309ed-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="309ed-256">En un proceso asincrónico, se debe devolver la tarea desde el método para detener la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="309ed-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="309ed-257">En un proyecto MVC, no debería llamar a Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="309ed-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="309ed-258">En su lugar, devuelven un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="309ed-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="309ed-259">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="309ed-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="309ed-260">Recomendación: Use ViewStateMode, en lugar de EnableViewState, para proporcionar un control granular sobre el que los controles utilizan el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="309ed-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="309ed-261">Cuando EnableViewState se establece en false en la directiva de página, el estado de vista está deshabilitado para todos los controles en la página y no se puede habilitar.</span><span class="sxs-lookup"><span data-stu-id="309ed-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="309ed-262">Si desea habilitar el estado de vista de solo ciertos controles en la página, establezca ViewStateMode en deshabilitado para la página.</span><span class="sxs-lookup"><span data-stu-id="309ed-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="309ed-263">A continuación, establezca ViewStateMode habilitado sólo en los controles que realmente necesitan el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="309ed-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="309ed-264">Al habilitar el estado de vista de solo los controles que lo necesitan, puede reducir el tamaño del estado de vista para las páginas web.</span><span class="sxs-lookup"><span data-stu-id="309ed-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="309ed-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="309ed-265">SqlMembershipProvider</span></span>

<span data-ttu-id="309ed-266">Recomendación: Utilizar proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="309ed-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="309ed-267">En las plantillas de proyecto actual, SqlMembershipProvider se ha reemplazado por [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="309ed-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="309ed-268">Si usas SqlMembershipProvider en un proyecto que se compiló con una versión anterior de las plantillas, debería pasar a los proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="309ed-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="309ed-269">Los proveedores universales funcionan con todas las bases de datos que son compatibles con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="309ed-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="309ed-270">Para obtener más información, consulte [Introducción a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="309ed-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="309ed-271">Las solicitudes de ejecución prolongada (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="309ed-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="309ed-272">Recomendación: Utilizar [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) para los clientes conectados y operaciones asincrónicas de E/S de uso.</span><span class="sxs-lookup"><span data-stu-id="309ed-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="309ed-273">Las solicitudes de ejecución prolongada pueden provocar resultados imprevisibles y un rendimiento bajo en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="309ed-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="309ed-274">El valor de tiempo de espera predeterminado para una solicitud es 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="309ed-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="309ed-275">Si usas el estado de sesión con una solicitud de ejecución prolongada, ASP.NET volverá a liberar el bloqueo en el objeto de sesión después de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="309ed-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="309ed-276">Sin embargo, la aplicación puede estar en el medio de una operación en el objeto de sesión cuando se libere el bloqueo y la operación no se puede completar correctamente.</span><span class="sxs-lookup"><span data-stu-id="309ed-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="309ed-277">Si una segunda solicitud del usuario se bloquea mientras se ejecuta la primera solicitud, la segunda solicitud puede tener acceso al objeto de sesión en un estado incoherente.</span><span class="sxs-lookup"><span data-stu-id="309ed-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="309ed-278">Si la aplicación incluye las operaciones de E/S de bloqueo (o sincrónicas), la aplicación estará no responde.</span><span class="sxs-lookup"><span data-stu-id="309ed-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="309ed-279">Para mejorar el rendimiento, utilice las operaciones de E/S asincrónicas en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="309ed-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="309ed-280">Además, usar WebSockets o SignalR para conectar los clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="309ed-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="309ed-281">Estas características están diseñadas para administrar eficazmente las solicitudes de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="309ed-281">These features are designed to efficiently handle long-running requests.</span></span>
