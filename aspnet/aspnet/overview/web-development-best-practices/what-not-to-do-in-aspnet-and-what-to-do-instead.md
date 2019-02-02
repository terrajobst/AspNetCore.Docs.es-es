---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Lo que no se debe hacer en ASP.NET y qué hacer en su lugar | Microsoft Docs
author: Rick-Anderson
description: En este tema se describe varios errores comunes que las personas dentro de los proyectos web ASP.NET. Se proporcionan recomendaciones sobre lo que debe hacer para evitar estos cargándola...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667718"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="5566c-104">Qué no se debe hacer en ASP.NET y qué hacer en su lugar</span><span class="sxs-lookup"><span data-stu-id="5566c-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="5566c-105">En este tema se describe varios errores comunes que las personas dentro de los proyectos web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5566c-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="5566c-106">Se proporcionan recomendaciones sobre lo que debe hacer para evitar estos errores comunes.</span><span class="sxs-lookup"><span data-stu-id="5566c-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="5566c-107">Se basa en un [presentación](http://vimeo.com/68390507) por **Damian Edwards** en corona conferencia para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="5566c-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="5566c-108">Declinación de responsabilidades</span><span class="sxs-lookup"><span data-stu-id="5566c-108">Disclaimer</span></span>

<span data-ttu-id="5566c-109">En este tema no está pensada como una guía completa para asegurarse de que la aplicación es segura y eficaz.</span><span class="sxs-lookup"><span data-stu-id="5566c-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="5566c-110">Aun así deberá seguir los procedimientos recomendados de seguridad y rendimiento que no se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="5566c-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="5566c-111">Solo sugiere cómo evitar errores comunes relacionados con los procesos y las clases. NET.</span><span class="sxs-lookup"><span data-stu-id="5566c-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="5566c-112">Información general</span><span class="sxs-lookup"><span data-stu-id="5566c-112">Overview</span></span>

<span data-ttu-id="5566c-113">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="5566c-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5566c-114">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="5566c-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="5566c-115">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="5566c-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="5566c-116">Propiedades de estilo de los controles</span><span class="sxs-lookup"><span data-stu-id="5566c-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="5566c-117">Página y devoluciones de llamada de Control</span><span class="sxs-lookup"><span data-stu-id="5566c-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="5566c-118">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="5566c-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="5566c-119">Seguridad</span><span class="sxs-lookup"><span data-stu-id="5566c-119">Security</span></span>](#security)

    - [<span data-ttu-id="5566c-120">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="5566c-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="5566c-121">Sesión y autenticación de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="5566c-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="5566c-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="5566c-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="5566c-123">Nivel de confianza medio</span><span class="sxs-lookup"><span data-stu-id="5566c-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="5566c-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="5566c-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="5566c-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="5566c-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="5566c-126">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="5566c-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="5566c-127">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="5566c-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="5566c-128">Eventos de página asincrónica con formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="5566c-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="5566c-129">Enviar y olvidarse de trabajo</span><span class="sxs-lookup"><span data-stu-id="5566c-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="5566c-130">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="5566c-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="5566c-131">Response.Redirect y Response.End</span><span class="sxs-lookup"><span data-stu-id="5566c-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="5566c-132">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="5566c-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="5566c-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="5566c-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="5566c-134">Las solicitudes de ejecución largo (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="5566c-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="5566c-135">Cumplimiento de estándares</span><span class="sxs-lookup"><span data-stu-id="5566c-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="5566c-136">Adaptadores de control</span><span class="sxs-lookup"><span data-stu-id="5566c-136">Control adapters</span></span>

<span data-ttu-id="5566c-137">Recomendación: Dejar de usar adaptadores de control para procesamiento adaptable y, en su lugar, use las consultas de medios CSS y HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="5566c-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="5566c-138">Adaptadores de controles se introdujeron en .NET 2.0 para representar el código de presentación que se ha personalizado para dispositivos diferentes y entornos.</span><span class="sxs-lookup"><span data-stu-id="5566c-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="5566c-139">Ahora, se puede lograr este procesamiento adaptable con CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="5566c-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="5566c-140">Debe dejar de usar adaptadores de Control y convertir a todos los adaptadores existentes en CSS y HTML.</span><span class="sxs-lookup"><span data-stu-id="5566c-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="5566c-141">Para obtener más información, consulte [las consultas de medios](http://www.w3.org/TR/css3-mediaqueries/) y [How To: Agregar páginas móviles a los formularios Web Forms ASP.NET / MVC aplicación](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="5566c-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="5566c-142">Propiedades de estilo de los controles</span><span class="sxs-lookup"><span data-stu-id="5566c-142">Style properties on controls</span></span>

<span data-ttu-id="5566c-143">Recomendación: Dejar de establecer los valores de estilo en el marcado del control y, en su lugar, establezca los valores de formato en las hojas de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="5566c-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="5566c-144">Controles de servidor Web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea.</span><span class="sxs-lookup"><span data-stu-id="5566c-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="5566c-145">Por ejemplo, la propiedad ForeColor establece el color del texto de un control.</span><span class="sxs-lookup"><span data-stu-id="5566c-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="5566c-146">Puede realizar este mismo efecto más eficazmente a través de las hojas de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="5566c-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="5566c-147">Las hojas de estilo le permiten centralizar los valores de estilo y evite establecer estos valores en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5566c-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="5566c-148">En el ejemplo siguiente se muestra una clase CSS el texto de los conjuntos a rojo.</span><span class="sxs-lookup"><span data-stu-id="5566c-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="5566c-149">En el ejemplo siguiente se muestra cómo se aplican dinámicamente a la clase CSS.</span><span class="sxs-lookup"><span data-stu-id="5566c-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="5566c-150">Devoluciones de llamada de la página y control</span><span class="sxs-lookup"><span data-stu-id="5566c-150">Page and control callbacks</span></span>

<span data-ttu-id="5566c-151">Recomendación: Dejar de utilizar las devoluciones de llamada de la página y de control y, en su lugar, use cualquiera de los siguientes: AJAX, UpdatePanel, MVC métodos de acción, Web API y SignalR.</span><span class="sxs-lookup"><span data-stu-id="5566c-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="5566c-152">En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y Control habilitado para actualizar parte de la página web sin actualizar una página completa.</span><span class="sxs-lookup"><span data-stu-id="5566c-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="5566c-153">Ahora puede realizar actualizaciones parciales de página a través de [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="5566c-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="5566c-154">Debe detenerse mediante los métodos de devolución de llamada porque pueden causar problemas con direcciones URL descriptivas y enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="5566c-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="5566c-155">De forma predeterminada, los controles no permiten a los métodos de devolución de llamada, pero si habilita esta característica en un control, debe deshabilitarlo.</span><span class="sxs-lookup"><span data-stu-id="5566c-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="5566c-156">Detección de capacidad del explorador</span><span class="sxs-lookup"><span data-stu-id="5566c-156">Browser capability detection</span></span>

<span data-ttu-id="5566c-157">Recomendación: Dejar de usar la detección de capacidad del explorador estático y, en su lugar, use la detección dinámica de características.</span><span class="sxs-lookup"><span data-stu-id="5566c-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="5566c-158">En versiones anteriores de ASP.NET, las características admitidas para cada explorador se almacenaban en un archivo XML.</span><span class="sxs-lookup"><span data-stu-id="5566c-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="5566c-159">Detectar compatibilidad con las características a través de una búsqueda estática no es el mejor enfoque.</span><span class="sxs-lookup"><span data-stu-id="5566c-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="5566c-160">Ahora, puede detectar dinámicamente un explorador las características compatibles con un marco de trabajo de detección de características, como [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="5566c-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="5566c-161">Detección de características determina la compatibilidad con cualquier intento de usar un método o propiedad y, a continuación, comprueba si el explorador produce el resultado deseado.</span><span class="sxs-lookup"><span data-stu-id="5566c-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="5566c-162">De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="5566c-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="5566c-163">Seguridad</span><span class="sxs-lookup"><span data-stu-id="5566c-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="5566c-164">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="5566c-164">Request validation</span></span>

<span data-ttu-id="5566c-165">Recomendación: Validar entrada de usuario y codifique la salida de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="5566c-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="5566c-166">Validación de la solicitud es una característica de ASP.NET que inspecciona cada solicitud y se detiene la solicitud si no se encuentra una amenaza percibida.</span><span class="sxs-lookup"><span data-stu-id="5566c-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="5566c-167">No dependen de la validación de solicitudes para proteger la aplicación frente a ataques de scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="5566c-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="5566c-168">En su lugar, validar todas las entradas de los usuarios y codifique la salida.</span><span class="sxs-lookup"><span data-stu-id="5566c-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="5566c-169">En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados que debe validar entrada de usuario mediante las clases de .NET que determinan si el valor coincide con los valores permitidos.</span><span class="sxs-lookup"><span data-stu-id="5566c-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="5566c-170">El ejemplo siguiente muestra cómo usar un método estático en la clase Uri para determinar si el Uri proporcionado por el usuario es válido.</span><span class="sxs-lookup"><span data-stu-id="5566c-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="5566c-171">Sin embargo, para comprobar lo suficientemente el Uri, también debe comprobar para asegurarse de que especifica `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="5566c-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="5566c-172">El ejemplo siguiente utiliza los métodos de instancia para comprobar que el Uri es válido.</span><span class="sxs-lookup"><span data-stu-id="5566c-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="5566c-173">Antes de representar la entrada del usuario como HTML o como entrada del usuario en una consulta SQL, codificar los valores para asegurarse de que no se incluye el código malintencionado.</span><span class="sxs-lookup"><span data-stu-id="5566c-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="5566c-174">Puede HTML codificar el valor en el marcado con el &lt;%: %&gt; sintaxis, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5566c-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="5566c-175">O bien, en la sintaxis de Razor, puede HTML codificar con @, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5566c-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="5566c-176">El ejemplo siguiente se muestra cómo a HTML codifica un valor en el código subyacente.</span><span class="sxs-lookup"><span data-stu-id="5566c-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="5566c-177">Para codificar de forma segura un valor para los comandos SQL, use los parámetros de comando como el [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="5566c-178">Sesión y autenticación de formularios sin cookies</span><span class="sxs-lookup"><span data-stu-id="5566c-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="5566c-179">Recomendación: Necesita cookies.</span><span class="sxs-lookup"><span data-stu-id="5566c-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="5566c-180">No es seguro pasar información de autenticación en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="5566c-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="5566c-181">Por lo tanto, necesita cookies cuando la aplicación incluye la autenticación.</span><span class="sxs-lookup"><span data-stu-id="5566c-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="5566c-182">Si la cookie que almacena información confidencial, considere la posibilidad de que se requiere SSL para la cookie.</span><span class="sxs-lookup"><span data-stu-id="5566c-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="5566c-183">El ejemplo siguiente muestra cómo se especifica en el archivo Web.config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.</span><span class="sxs-lookup"><span data-stu-id="5566c-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="5566c-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="5566c-184">EnableViewStateMac</span></span>

<span data-ttu-id="5566c-185">Recomendación: Nunca se establece en false.</span><span class="sxs-lookup"><span data-stu-id="5566c-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="5566c-186">De forma predeterminada, EnbableViewStateMac se establece en true.</span><span class="sxs-lookup"><span data-stu-id="5566c-186">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="5566c-187">Incluso si la aplicación no usa el estado de vista, no establezca EnableViewStateMac en false.</span><span class="sxs-lookup"><span data-stu-id="5566c-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="5566c-188">Establecer este valor en false para que su aplicación sea vulnerable a XSS.</span><span class="sxs-lookup"><span data-stu-id="5566c-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="5566c-189">A partir de ASP.NET 4.5.2, el tiempo de ejecución impone **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="5566c-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="5566c-190">Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true.</span><span class="sxs-lookup"><span data-stu-id="5566c-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="5566c-191">Para obtener más información, consulte [ASP.NET 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="5566c-192">El ejemplo siguiente muestra cómo establecer EnableViewStateMac en true.</span><span class="sxs-lookup"><span data-stu-id="5566c-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="5566c-193">No es necesario establecer este valor en true porque es true de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5566c-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="5566c-194">Sin embargo, si se se establece en false en cualquier página en la aplicación, debe corregir inmediatamente este valor.</span><span class="sxs-lookup"><span data-stu-id="5566c-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="5566c-195">Nivel de confianza medio</span><span class="sxs-lookup"><span data-stu-id="5566c-195">Medium trust</span></span>

<span data-ttu-id="5566c-196">Recomendación: No dependen de nivel de confianza medio (o cualquier otro nivel de confianza) como un límite de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5566c-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="5566c-197">Confianza parcial no protegen adecuadamente la aplicación y no debe usarse.</span><span class="sxs-lookup"><span data-stu-id="5566c-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="5566c-198">En su lugar, use la plena confianza y aislar las aplicaciones que no se confía en grupos de aplicaciones independientes.</span><span class="sxs-lookup"><span data-stu-id="5566c-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="5566c-199">Además, ejecutar cada grupo de aplicaciones con una identidad única.</span><span class="sxs-lookup"><span data-stu-id="5566c-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="5566c-200">Para obtener más información, consulte [ASP.NET de confianza parcial no garantiza el aislamiento de la aplicación](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="5566c-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="5566c-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="5566c-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="5566c-202">Recomendación: Deshabilitar la configuración de seguridad en &lt;appSettings&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="5566c-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="5566c-203">El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5566c-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="5566c-204">No debe cambiar o deshabilitar estos valores.</span><span class="sxs-lookup"><span data-stu-id="5566c-204">You should not change or disable these values.</span></span> <span data-ttu-id="5566c-205">Si debe deshabilitar estos valores al implementar una actualización, inmediatamente volver a habilitar después de completar la implementación.</span><span class="sxs-lookup"><span data-stu-id="5566c-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="5566c-206">Para obtener más información, consulte [elemento appSettings de ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="5566c-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="5566c-207">UrlPathEncode</span></span>

<span data-ttu-id="5566c-208">Recomendación: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="5566c-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="5566c-209">Se agregó el método UrlPathEncode a .NET Framework para resolver un problema de compatibilidad de explorador muy específico.</span><span class="sxs-lookup"><span data-stu-id="5566c-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="5566c-210">No realiza ninguna codificación adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="5566c-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="5566c-211">Nunca se debe usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5566c-211">You should never use it in your application.</span></span> <span data-ttu-id="5566c-212">En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="5566c-213">El ejemplo siguiente muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.</span><span class="sxs-lookup"><span data-stu-id="5566c-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="5566c-214">Confiabilidad y rendimiento</span><span class="sxs-lookup"><span data-stu-id="5566c-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="5566c-215">PreSendRequestHeaders y PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="5566c-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="5566c-216">Recomendación: No use estos eventos con módulos administrados.</span><span class="sxs-lookup"><span data-stu-id="5566c-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="5566c-217">En su lugar, escribir un módulo nativo de IIS para llevar a cabo la tarea requerida.</span><span class="sxs-lookup"><span data-stu-id="5566c-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="5566c-218">Consulte [crear módulos HTTP de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="5566c-219">Puede usar el [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) eventos con los módulos nativos de IIS.</span><span class="sxs-lookup"><span data-stu-id="5566c-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="5566c-220">No use `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="5566c-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="5566c-221">Establecer estas propiedades puede causar problemas con las solicitudes asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="5566c-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="5566c-222">La combinación de enrutamiento solicitado aplicaciones (ARR) y websockets podría provocar excepciones de infracción de acceso que pueden causar w3wp se bloquee.</span><span class="sxs-lookup"><span data-stu-id="5566c-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="5566c-223">¡Por ejemplo, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 en iiscore.dll ha provocado una excepción de infracción de acceso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="5566c-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="5566c-224">Eventos de página asincrónica con formularios web forms</span><span class="sxs-lookup"><span data-stu-id="5566c-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="5566c-225">Recomendación: En formularios Web Forms, evite escribir async void métodos para los eventos de ciclo de vida de página y, en su lugar use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="5566c-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="5566c-226">Al marcar un evento de página con **async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="5566c-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="5566c-227">En su lugar, utilice Page.RegisterAsyncTask para ejecutar el código asincrónico de forma que permite realizar un seguimiento de su finalización.</span><span class="sxs-lookup"><span data-stu-id="5566c-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="5566c-228">El ejemplo siguiente se muestra un botón, haga clic en el controlador que contiene el código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="5566c-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="5566c-229">Este ejemplo incluye la lectura asincrónica, de un valor de cadena que se proporciona sólo como un ejemplo simplificado de una tarea asincrónica y no como una práctica recomendada.</span><span class="sxs-lookup"><span data-stu-id="5566c-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="5566c-230">Si usa tareas asincrónicas, establezca la plataforma de destino en tiempo de ejecución de Http a 4.5 (o posterior) en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="5566c-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="5566c-231">Establecer la plataforma de destino en 4.5 activa en el nuevo contexto de sincronización que se agregó en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5566c-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="5566c-232">Este valor se establece de forma predeterminada en los nuevos proyectos en Visual Studio, pero no es estar establecida si está trabajando con un proyecto existente.</span><span class="sxs-lookup"><span data-stu-id="5566c-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="5566c-233">Enviar y olvidarse de trabajo</span><span class="sxs-lookup"><span data-stu-id="5566c-233">Fire-and-forget work</span></span>

<span data-ttu-id="5566c-234">Recomendación: Al procesar una solicitud de ASP.NET, evitará iniciar trabajo fire and forget (este tipo llamando al método ThreadPool.QueueUserWorkItem o crear un temporizador que llama repetidamente a un delegado).</span><span class="sxs-lookup"><span data-stu-id="5566c-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="5566c-235">Si la aplicación tiene trabajo fire and forget que se ejecuta en ASP.NET, la aplicación puede obtener sincronizada. En cualquier momento, se puede destruir el dominio de aplicación lo que significa que el proceso continuo ya no puede coincidir con el estado actual de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5566c-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="5566c-236">Debe mover este tipo de trabajo fuera de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5566c-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="5566c-237">Puede usar los trabajos Web, servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código de otro proceso.</span><span class="sxs-lookup"><span data-stu-id="5566c-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="5566c-238">Si se debe realizar este trabajo en ASP.NET, puede agregar el paquete Nuget llamado [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.</span><span class="sxs-lookup"><span data-stu-id="5566c-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="5566c-239">Cuerpo de la entidad de solicitud</span><span class="sxs-lookup"><span data-stu-id="5566c-239">Request entity body</span></span>

<span data-ttu-id="5566c-240">Recomendación: Evitar la lectura Request.Form o Request.InputStream antes de ejecuta el evento al controlador.</span><span class="sxs-lookup"><span data-stu-id="5566c-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="5566c-241">Lo antes posible, debe leer desde Request.Form o Request.InputStream es durante el controlador ejecutar el evento.</span><span class="sxs-lookup"><span data-stu-id="5566c-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="5566c-242">En MVC, el controlador es el controlador y el evento de ejecución es cuando se ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="5566c-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="5566c-243">En formularios Web Forms, la página es el controlador y el evento de ejecución es cuando se desencadene el evento Page.Init.</span><span class="sxs-lookup"><span data-stu-id="5566c-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="5566c-244">Si lee el cuerpo de entidad de solicitud antes que el evento de ejecución, interferir con el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5566c-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="5566c-245">Si tiene que leer el cuerpo de entidad de solicitud antes del evento execute, usar [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="5566c-246">Cuando usas GetBufferlessInputStream, obtener la cadena sin formato de la solicitud y asuma la responsabilidad para procesar la solicitud completa.</span><span class="sxs-lookup"><span data-stu-id="5566c-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="5566c-247">Después de llamar a GetBufferlessInputStream, Request.Form y Request.InputStream no están disponibles porque no se ha rellenado por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5566c-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="5566c-248">Cuando usas GetBufferedInputStream, obtener una copia de la secuencia de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5566c-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="5566c-249">Request.Form y Request.InputStream siguen estando disponibles más adelante en la solicitud porque ASP.NET rellena la otra copia.</span><span class="sxs-lookup"><span data-stu-id="5566c-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="5566c-250">Response.Redirect y Response.End</span><span class="sxs-lookup"><span data-stu-id="5566c-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="5566c-251">Recomendación: Tenga en cuenta las diferencias en cómo se controla el subproceso después de llamar a [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="5566c-252">El [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) método llama al método Response.End.</span><span class="sxs-lookup"><span data-stu-id="5566c-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="5566c-253">En un proceso sincrónico, una llamada a Request.Redirect hace que el subproceso actual se anula inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="5566c-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="5566c-254">Sin embargo, en un proceso asincrónico, la llamada a Response.Redirect no anula el subproceso actual, por lo que continúa la ejecución de código para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5566c-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="5566c-255">En un proceso asincrónico, debe devolver la tarea desde el método para detener la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="5566c-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="5566c-256">En un proyecto MVC, no debe llamar a Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="5566c-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="5566c-257">En su lugar, devuelven un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="5566c-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="5566c-258">EnableViewState y ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="5566c-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="5566c-259">Recomendación: Use ViewStateMode, en lugar de EnableViewState, para proporcionar control granular sobre qué controles utilizar el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="5566c-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="5566c-260">Cuando EnableViewState se establece en false en la directiva de página, el estado de vista está deshabilitado para todos los controles dentro de la página y no se puede habilitar.</span><span class="sxs-lookup"><span data-stu-id="5566c-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="5566c-261">Si desea habilitar el estado de vista para solo ciertos controles en la página, establezca ViewStateMode en deshabilitado para la página.</span><span class="sxs-lookup"><span data-stu-id="5566c-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="5566c-262">A continuación, establezca ViewStateMode habilitado sólo en los controles que necesitan realmente el estado de vista.</span><span class="sxs-lookup"><span data-stu-id="5566c-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="5566c-263">Al habilitar el estado de vista de solo los controles que lo necesiten, puede reducir el tamaño del estado de vista para las páginas web.</span><span class="sxs-lookup"><span data-stu-id="5566c-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="5566c-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="5566c-264">SqlMembershipProvider</span></span>

<span data-ttu-id="5566c-265">Recomendación: Usar proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="5566c-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="5566c-266">En las plantillas de proyecto actual, SqlMembershipProvider ha sido reemplazado por [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="5566c-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="5566c-267">Si utilizas SqlMembershipProvider en un proyecto que se creó con una versión anterior de las plantillas, debe cambiar a los proveedores universales.</span><span class="sxs-lookup"><span data-stu-id="5566c-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="5566c-268">Los proveedores universales funcionan con todas las bases de datos que son compatibles con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5566c-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="5566c-269">Para obtener más información, consulte [Introducción a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="5566c-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="5566c-270">Solicitudes de ejecución prolongada (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="5566c-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="5566c-271">Recomendación: Usar [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) para los clientes conectados y use operaciones asincrónicas de E/S.</span><span class="sxs-lookup"><span data-stu-id="5566c-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="5566c-272">Solicitudes de ejecución prolongada pueden causar resultados imprevisibles y un rendimiento deficiente en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="5566c-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="5566c-273">El valor de tiempo de espera predeterminado para una solicitud es de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="5566c-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="5566c-274">Si usa el estado de sesión con una solicitud de ejecución prolongada, ASP.NET volverá a liberar el bloqueo en el objeto de sesión después de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="5566c-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="5566c-275">Sin embargo, la aplicación puede estar en el medio de una operación en el objeto de sesión cuando se libere el bloqueo y la operación no se puede completar correctamente.</span><span class="sxs-lookup"><span data-stu-id="5566c-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="5566c-276">Si una segunda solicitud del usuario se bloquea mientras se ejecuta la primera solicitud, la segunda solicitud puede tener acceso al objeto de sesión en un estado incoherente.</span><span class="sxs-lookup"><span data-stu-id="5566c-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="5566c-277">Si la aplicación incluye las operaciones de E/S de bloqueo (o sincrónicas), no responderá la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5566c-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="5566c-278">Para mejorar el rendimiento, utilice las operaciones de E/S asincrónicas en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5566c-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="5566c-279">Además, usar SignalR o WebSockets para conectar clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="5566c-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="5566c-280">Estas características están diseñadas para manejar de manera eficaz las solicitudes de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="5566c-280">These features are designed to efficiently handle long-running requests.</span></span>
