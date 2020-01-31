---
title: Control de errores en las aplicaciones de Blazor de ASP.NET Core
author: guardrex
description: Descubra cómo ASP.NET Core Blazor cómo Blazor administra las excepciones no controladas y cómo desarrollar aplicaciones que detecten y controlen los errores.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 7b5602d5ae5e58d1678762fe1cd2adec1f31c969
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809008"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="d12ea-103">Control de errores en las aplicaciones de Blazor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d12ea-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="d12ea-104">Por [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="d12ea-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="d12ea-105">En este artículo se describe cómo Blazor administra las excepciones no controladas y cómo desarrollar las aplicaciones que detectan y controlan los errores.</span><span class="sxs-lookup"><span data-stu-id="d12ea-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="d12ea-106">Errores detallados durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="d12ea-106">Detailed errors during development</span></span>

<span data-ttu-id="d12ea-107">Cuando una aplicación Blazor no funciona correctamente durante el desarrollo, recibir información detallada del error de la aplicación ayuda a solucionar el problema.</span><span class="sxs-lookup"><span data-stu-id="d12ea-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="d12ea-108">Cuando se produce un error, en las aplicaciones Blazor se muestra una barra dorada en la parte inferior de la pantalla:</span><span class="sxs-lookup"><span data-stu-id="d12ea-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="d12ea-109">Durante el desarrollo, la barra dorada le dirige a la consola del explorador, donde puede ver la excepción.</span><span class="sxs-lookup"><span data-stu-id="d12ea-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="d12ea-110">En producción, la barra dorada informa al usuario de que se ha producido un error y recomienda actualizar el explorador.</span><span class="sxs-lookup"><span data-stu-id="d12ea-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="d12ea-111">La interfaz de usuario para esta experiencia de control de errores forma parte de las plantillas de proyecto de Blazor.</span><span class="sxs-lookup"><span data-stu-id="d12ea-111">The UI for this error handling experience is part of the Blazor project templates.</span></span> <span data-ttu-id="d12ea-112">En una aplicación Blazor webassembly, personalice la experiencia en el archivo *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="d12ea-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="d12ea-113">En una aplicación de Blazor Server, personalice la experiencia en el archivo *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d12ea-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="d12ea-114">Los estilos incluidos en las plantillas de Blazor ocultan el elemento `blazor-error-ui` y, a continuación, se muestra cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="d12ea-114">The `blazor-error-ui` element is hidden by the styles included with the Blazor templates and then shown when an error occurs.</span></span>

## <a name="how-the-opno-locblazor-framework-reacts-to-unhandled-exceptions"></a><span data-ttu-id="d12ea-115">Cómo reacciona el marco de Blazor a las excepciones no controladas</span><span class="sxs-lookup"><span data-stu-id="d12ea-115">How the Blazor framework reacts to unhandled exceptions</span></span>

Blazor<span data-ttu-id="d12ea-116"> Server es un marco con estado.</span><span class="sxs-lookup"><span data-stu-id="d12ea-116"> Server is a stateful framework.</span></span> <span data-ttu-id="d12ea-117">Mientras los usuarios interactúan con una aplicación, mantienen una conexión con el servidor conocido como *circuito*.</span><span class="sxs-lookup"><span data-stu-id="d12ea-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="d12ea-118">El circuito contiene instancias de componentes activas, además de muchos otros aspectos del estado, como:</span><span class="sxs-lookup"><span data-stu-id="d12ea-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="d12ea-119">La salida representada más reciente de los componentes.</span><span class="sxs-lookup"><span data-stu-id="d12ea-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="d12ea-120">Conjunto actual de delegados de control de eventos que pueden ser desencadenados por eventos del cliente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="d12ea-121">Si un usuario abre la aplicación en varias pestañas del explorador, tiene varios circuitos independientes.</span><span class="sxs-lookup"><span data-stu-id="d12ea-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="d12ea-122"> trata la mayoría de las excepciones no controladas como graves para el circuito en el que se producen.</span><span class="sxs-lookup"><span data-stu-id="d12ea-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="d12ea-123">Si se termina un circuito debido a una excepción no controlada, el usuario solo puede seguir interactuando con la aplicación recargando la página para crear un nuevo circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="d12ea-124">Los circuitos fuera de la terminación, que son circuitos para otros usuarios u otras pestañas del explorador, no se ven afectados.</span><span class="sxs-lookup"><span data-stu-id="d12ea-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="d12ea-125">Este escenario es similar a una aplicación de escritorio que se bloquea&mdash;se debe reiniciar la aplicación bloqueada, pero no se ven afectadas otras aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d12ea-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="d12ea-126">Un circuito finaliza cuando se produce una excepción no controlada por los siguientes motivos:</span><span class="sxs-lookup"><span data-stu-id="d12ea-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="d12ea-127">Una excepción no controlada a menudo deja el circuito en un estado indefinido.</span><span class="sxs-lookup"><span data-stu-id="d12ea-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="d12ea-128">No se puede garantizar la operación normal de la aplicación después de una excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="d12ea-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="d12ea-129">Es posible que aparezcan vulnerabilidades de seguridad en la aplicación si el circuito continúa.</span><span class="sxs-lookup"><span data-stu-id="d12ea-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="d12ea-130">Administrar excepciones no controladas en el código de desarrollador</span><span class="sxs-lookup"><span data-stu-id="d12ea-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="d12ea-131">Para que una aplicación continúe después de un error, la aplicación debe tener una lógica de control de errores.</span><span class="sxs-lookup"><span data-stu-id="d12ea-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="d12ea-132">En secciones posteriores de este artículo se describen posibles orígenes de excepciones no controladas.</span><span class="sxs-lookup"><span data-stu-id="d12ea-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="d12ea-133">En producción, no se representan mensajes de excepción de marco o seguimientos de pila en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="d12ea-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="d12ea-134">La representación de mensajes de excepción o seguimientos de la pila podría:</span><span class="sxs-lookup"><span data-stu-id="d12ea-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="d12ea-135">Divulgar información confidencial a los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="d12ea-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="d12ea-136">Ayudar a un usuario malintencionado a detectar debilidades en una aplicación que puede poner en peligro la seguridad de la aplicación, el servidor o la red.</span><span class="sxs-lookup"><span data-stu-id="d12ea-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="d12ea-137">Registrar errores con un proveedor persistente</span><span class="sxs-lookup"><span data-stu-id="d12ea-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="d12ea-138">Si se produce una excepción no controlada, la excepción se registra en <xref:Microsoft.Extensions.Logging.ILogger> las instancias configuradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="d12ea-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="d12ea-139">De forma predeterminada, Blazor aplicaciones registran la salida de la consola con el proveedor de registro de la consola.</span><span class="sxs-lookup"><span data-stu-id="d12ea-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="d12ea-140">Considere la posibilidad de iniciar sesión en una ubicación más permanente con un proveedor que administre el tamaño y la rotación del registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="d12ea-141">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="d12ea-142">Durante el desarrollo, Blazor envía normalmente los detalles completos de las excepciones a la consola del explorador para ayudar en la depuración.</span><span class="sxs-lookup"><span data-stu-id="d12ea-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="d12ea-143">En producción, los errores detallados en la consola del explorador están deshabilitados de forma predeterminada, lo que significa que los errores no se envían a los clientes, pero los detalles completos de la excepción siguen registrándose en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="d12ea-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="d12ea-144">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="d12ea-145">Debe decidir qué incidentes registrar y el nivel de gravedad de los incidentes registrados.</span><span class="sxs-lookup"><span data-stu-id="d12ea-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="d12ea-146">Es posible que los usuarios hostiles puedan desencadenar errores deliberadamente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="d12ea-147">Por ejemplo, no registre un incidente de un error en el que se proporcione un `ProductId` desconocido en la dirección URL de un componente que muestra los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="d12ea-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="d12ea-148">No todos los errores se deben tratar como incidentes de alta gravedad para el registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="d12ea-149">Lugares donde se pueden producir errores</span><span class="sxs-lookup"><span data-stu-id="d12ea-149">Places where errors may occur</span></span>

<span data-ttu-id="d12ea-150">El código de marco de trabajo y la aplicación pueden desencadenar excepciones no controladas en cualquiera de las siguientes ubicaciones:</span><span class="sxs-lookup"><span data-stu-id="d12ea-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="d12ea-151">Creación de instancias de componentes</span><span class="sxs-lookup"><span data-stu-id="d12ea-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="d12ea-152">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="d12ea-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="d12ea-153">Lógica de representación</span><span class="sxs-lookup"><span data-stu-id="d12ea-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="d12ea-154">Controladores de eventos</span><span class="sxs-lookup"><span data-stu-id="d12ea-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="d12ea-155">Eliminación de componentes</span><span class="sxs-lookup"><span data-stu-id="d12ea-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="d12ea-156">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d12ea-156">JavaScript interop</span></span>](#javascript-interop)
* [<span data-ttu-id="d12ea-157">Controladores de circuito</span><span class="sxs-lookup"><span data-stu-id="d12ea-157">Circuit handlers</span></span>](#circuit-handlers)
* [<span data-ttu-id="d12ea-158">Eliminación de circuitos</span><span class="sxs-lookup"><span data-stu-id="d12ea-158">Circuit disposal</span></span>](#circuit-disposal)
* [<span data-ttu-id="d12ea-159">Representación previa</span><span class="sxs-lookup"><span data-stu-id="d12ea-159">Prerendering</span></span>](#prerendering)

<span data-ttu-id="d12ea-160">Las excepciones no controladas anteriores se describen en las siguientes secciones de este artículo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-160">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="d12ea-161">Creación de instancias de componentes</span><span class="sxs-lookup"><span data-stu-id="d12ea-161">Component instantiation</span></span>

<span data-ttu-id="d12ea-162">Cuando Blazor crea una instancia de un componente:</span><span class="sxs-lookup"><span data-stu-id="d12ea-162">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="d12ea-163">Se invoca el constructor del componente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-163">The component's constructor is invoked.</span></span>
* <span data-ttu-id="d12ea-164">Se invocan los constructores de cualquier servicio de DI no singleton proporcionado al constructor del componente mediante la directiva [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) o el atributo [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) .</span><span class="sxs-lookup"><span data-stu-id="d12ea-164">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="d12ea-165">Se produce un error en un circuito cuando cualquier constructor ejecutado o un establecedor para cualquier propiedad `[Inject]` produce una excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="d12ea-165">A circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="d12ea-166">La excepción es grave porque el marco no puede crear una instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-166">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="d12ea-167">Si la lógica del constructor puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-167">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="d12ea-168">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="d12ea-168">Lifecycle methods</span></span>

<span data-ttu-id="d12ea-169">Durante la vigencia de un componente, Blazor invoca los siguientes [métodos de ciclo de vida](xref:blazor/lifecycle):</span><span class="sxs-lookup"><span data-stu-id="d12ea-169">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/lifecycle):</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="d12ea-170">Si cualquier método de ciclo de vida produce una excepción, de forma sincrónica o asincrónica, la excepción es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-170">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to the circuit.</span></span> <span data-ttu-id="d12ea-171">Para que los componentes traten los errores de los métodos de ciclo de vida, agregue la lógica de control de errores.</span><span class="sxs-lookup"><span data-stu-id="d12ea-171">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="d12ea-172">En el ejemplo siguiente, donde `OnParametersSetAsync` llama a un método para obtener un producto:</span><span class="sxs-lookup"><span data-stu-id="d12ea-172">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="d12ea-173">Una instrucción `try-catch` controla una excepción que se produce en el método `ProductRepository.GetProductByIdAsync`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-173">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="d12ea-174">Cuando se ejecuta el bloque de `catch`:</span><span class="sxs-lookup"><span data-stu-id="d12ea-174">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="d12ea-175">`loadFailed` se establece en `true`, que se usa para mostrar un mensaje de error al usuario.</span><span class="sxs-lookup"><span data-stu-id="d12ea-175">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="d12ea-176">El error se registra.</span><span class="sxs-lookup"><span data-stu-id="d12ea-176">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="d12ea-177">Lógica de representación</span><span class="sxs-lookup"><span data-stu-id="d12ea-177">Rendering logic</span></span>

<span data-ttu-id="d12ea-178">El marcado declarativo de un archivo de componente de `.razor` C# se compila en un método denominado `BuildRenderTree`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-178">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="d12ea-179">Cuando se representa un componente, `BuildRenderTree` ejecuta y genera una estructura de datos que describe los elementos, el texto y los componentes secundarios del componente representado.</span><span class="sxs-lookup"><span data-stu-id="d12ea-179">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="d12ea-180">La lógica de representación puede producir una excepción.</span><span class="sxs-lookup"><span data-stu-id="d12ea-180">Rendering logic can throw an exception.</span></span> <span data-ttu-id="d12ea-181">Un ejemplo de este escenario se produce cuando se evalúa `@someObject.PropertyName` pero `@someObject` es `null`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-181">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="d12ea-182">Una excepción no controlada producida por la lógica de representación es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-182">An unhandled exception thrown by rendering logic is fatal to the circuit.</span></span>

<span data-ttu-id="d12ea-183">Para evitar una excepción de referencia nula en la lógica de representación, busque un objeto `null` antes de tener acceso a sus miembros.</span><span class="sxs-lookup"><span data-stu-id="d12ea-183">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="d12ea-184">En el ejemplo siguiente, no se tiene acceso a las propiedades de `person.Address` si `person.Address` se `null`:</span><span class="sxs-lookup"><span data-stu-id="d12ea-184">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="d12ea-185">En el código anterior se supone que `person` no está `null`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-185">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="d12ea-186">A menudo, la estructura del código garantiza que un objeto existe en el momento en que se representa el componente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-186">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="d12ea-187">En esos casos, no es necesario comprobar `null` en la lógica de representación.</span><span class="sxs-lookup"><span data-stu-id="d12ea-187">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="d12ea-188">En el ejemplo anterior, es posible que se garantice la existencia de `person` porque `person` se crea cuando se crea una instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-188">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="d12ea-189">Controladores de eventos</span><span class="sxs-lookup"><span data-stu-id="d12ea-189">Event handlers</span></span>

<span data-ttu-id="d12ea-190">El código del lado cliente desencadena invocaciones de código C# cuando se crean controladores de eventos mediante:</span><span class="sxs-lookup"><span data-stu-id="d12ea-190">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="d12ea-191">Otros atributos de `@on...`</span><span class="sxs-lookup"><span data-stu-id="d12ea-191">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="d12ea-192">El código del controlador de eventos podría producir una excepción no controlada en estos escenarios.</span><span class="sxs-lookup"><span data-stu-id="d12ea-192">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="d12ea-193">Si un controlador de eventos produce una excepción no controlada (por ejemplo, se produce un error en una consulta de base de datos), la excepción es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-193">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to the circuit.</span></span> <span data-ttu-id="d12ea-194">Si la aplicación llama a código que podría dar error por motivos externos, Capture las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-194">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="d12ea-195">Si el código de usuario no intercepta y controla la excepción, el marco de trabajo registra la excepción y finaliza el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-195">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="d12ea-196">Eliminación de componentes</span><span class="sxs-lookup"><span data-stu-id="d12ea-196">Component disposal</span></span>

<span data-ttu-id="d12ea-197">Un componente se puede quitar de la interfaz de usuario, por ejemplo, porque el usuario ha navegado a otra página.</span><span class="sxs-lookup"><span data-stu-id="d12ea-197">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="d12ea-198">Cuando un componente que implementa <xref:System.IDisposable?displayProperty=fullName> se quita de la interfaz de usuario, el marco de trabajo llama al método de <xref:System.IDisposable.Dispose*> del componente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-198">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span>

<span data-ttu-id="d12ea-199">Si el método `Dispose` del componente produce una excepción no controlada, la excepción es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-199">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to the circuit.</span></span> <span data-ttu-id="d12ea-200">Si la lógica de eliminación puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-200">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="d12ea-201">Para obtener más información sobre la eliminación de componentes, consulte <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-201">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="d12ea-202">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d12ea-202">JavaScript interop</span></span>

<span data-ttu-id="d12ea-203">`IJSRuntime.InvokeAsync<T>` permite que el código .NET realice llamadas asincrónicas al tiempo de ejecución de JavaScript en el explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="d12ea-203">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="d12ea-204">Las condiciones siguientes se aplican al control de errores con `InvokeAsync<T>`:</span><span class="sxs-lookup"><span data-stu-id="d12ea-204">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="d12ea-205">Si una llamada a `InvokeAsync<T>` produce un error sincrónicamente, se produce una excepción de .NET.</span><span class="sxs-lookup"><span data-stu-id="d12ea-205">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="d12ea-206">Se puede producir un error en una llamada a `InvokeAsync<T>`, por ejemplo, porque no se pueden serializar los argumentos proporcionados.</span><span class="sxs-lookup"><span data-stu-id="d12ea-206">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="d12ea-207">El código del desarrollador debe detectar la excepción.</span><span class="sxs-lookup"><span data-stu-id="d12ea-207">Developer code must catch the exception.</span></span> <span data-ttu-id="d12ea-208">Si el código de la aplicación en un controlador de eventos o un método de ciclo de vida de componente no controla una excepción, la excepción resultante es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-208">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to the circuit.</span></span>
* <span data-ttu-id="d12ea-209">Si se produce un error en una llamada a `InvokeAsync<T>` de forma asincrónica, se produce un error en .NET <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-209">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="d12ea-210">Una llamada a `InvokeAsync<T>` puede producir un error, por ejemplo, porque el código de JavaScript produce una excepción o devuelve un `Promise` que se ha completado como `rejected`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-210">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="d12ea-211">El código del desarrollador debe detectar la excepción.</span><span class="sxs-lookup"><span data-stu-id="d12ea-211">Developer code must catch the exception.</span></span> <span data-ttu-id="d12ea-212">Si usa el operador [Await](/dotnet/csharp/language-reference/keywords/await) , considere la posibilidad de encapsular la llamada al método en una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-212">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="d12ea-213">De lo contrario, el código que genera el error provoca una excepción no controlada que es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-213">Otherwise, the failing code results in an unhandled exception that's fatal to the circuit.</span></span>
* <span data-ttu-id="d12ea-214">De forma predeterminada, las llamadas a `InvokeAsync<T>` deben completarse en un período determinado o, de lo contrario, se agota el tiempo de espera de la llamada. El período de tiempo de espera predeterminado es de un minuto.</span><span class="sxs-lookup"><span data-stu-id="d12ea-214">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="d12ea-215">El tiempo de espera protege el código contra una pérdida en la conectividad de red o código JavaScript que nunca devuelve un mensaje de finalización.</span><span class="sxs-lookup"><span data-stu-id="d12ea-215">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="d12ea-216">Si se agota el tiempo de espera de la llamada, se produce un error en el `Task` resultante con un <xref:System.OperationCanceledException>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-216">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="d12ea-217">Capture y procese la excepción con el registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-217">Trap and process the exception with logging.</span></span>

<span data-ttu-id="d12ea-218">Del mismo modo, el código de JavaScript puede iniciar llamadas a métodos .NET indicados por el atributo [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) .</span><span class="sxs-lookup"><span data-stu-id="d12ea-218">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) attribute.</span></span> <span data-ttu-id="d12ea-219">Si estos métodos .NET producen una excepción no controlada:</span><span class="sxs-lookup"><span data-stu-id="d12ea-219">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="d12ea-220">La excepción no se trata como grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-220">The exception isn't treated as fatal to the circuit.</span></span>
* <span data-ttu-id="d12ea-221">Se rechaza el `Promise` del lado de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d12ea-221">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="d12ea-222">Tiene la opción de usar el código de control de errores en el lado de .NET o en el lado de JavaScript de la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="d12ea-222">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="d12ea-223">Para obtener más información, vea <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-223">For more information, see <xref:blazor/javascript-interop>.</span></span>

### <a name="circuit-handlers"></a><span data-ttu-id="d12ea-224">Controladores de circuito</span><span class="sxs-lookup"><span data-stu-id="d12ea-224">Circuit handlers</span></span>

Blazor<span data-ttu-id="d12ea-225"> Server permite al código definir un *controlador de circuito*, lo que permite ejecutar el código en los cambios realizados en el estado del circuito de un usuario.</span><span class="sxs-lookup"><span data-stu-id="d12ea-225"> Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="d12ea-226">Un controlador de circuito se implementa derivando de `CircuitHandler` y registrando la clase en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d12ea-226">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="d12ea-227">En el siguiente ejemplo de un controlador de circuito se realiza un seguimiento de las conexiones SignalR abiertas:</span><span class="sxs-lookup"><span data-stu-id="d12ea-227">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="d12ea-228">Los controladores de circuito se registran mediante DI.</span><span class="sxs-lookup"><span data-stu-id="d12ea-228">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="d12ea-229">Las instancias con ámbito se crean por instancia de un circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-229">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="d12ea-230">Con el `TrackingCircuitHandler` en el ejemplo anterior, se crea un servicio singleton porque se debe realizar un seguimiento del estado de todos los circuitos:</span><span class="sxs-lookup"><span data-stu-id="d12ea-230">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="d12ea-231">Si los métodos de un controlador de circuito personalizado producen una excepción no controlada, la excepción es grave para el circuito de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d12ea-231">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="d12ea-232">Para tolerar excepciones en el código de un controlador o en métodos denominados, ajuste el código en una o varias instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-232">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

### <a name="circuit-disposal"></a><span data-ttu-id="d12ea-233">Eliminación de circuitos</span><span class="sxs-lookup"><span data-stu-id="d12ea-233">Circuit disposal</span></span>

<span data-ttu-id="d12ea-234">Cuando un circuito finaliza porque un usuario se ha desconectado y el marco está limpiando el estado del circuito, el marco de trabajo desecha el ámbito de DI del circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-234">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="d12ea-235">Al desechar el ámbito, se eliminan todos los servicios DI de ámbito de circuito que implementan <xref:System.IDisposable?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-235">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="d12ea-236">Si un servicio DI produce una excepción no controlada durante la eliminación, el marco de trabajo registra la excepción.</span><span class="sxs-lookup"><span data-stu-id="d12ea-236">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

### <a name="prerendering"></a><span data-ttu-id="d12ea-237">Representación previa</span><span class="sxs-lookup"><span data-stu-id="d12ea-237">Prerendering</span></span>

Blazor<span data-ttu-id="d12ea-238"> componentes se pueden representarse mediante la aplicación auxiliar de etiquetas `Component` de modo que su marca HTML representada se devuelva como parte de la solicitud HTTP inicial del usuario.</span><span class="sxs-lookup"><span data-stu-id="d12ea-238"> components can be prerendered using the `Component` Tag Helper so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="d12ea-239">Esto funciona de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="d12ea-239">This works by:</span></span>

* <span data-ttu-id="d12ea-240">Crear un circuito nuevo para todos los componentes prerepresentados que forman parte de la misma página.</span><span class="sxs-lookup"><span data-stu-id="d12ea-240">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="d12ea-241">Generar el código HTML inicial.</span><span class="sxs-lookup"><span data-stu-id="d12ea-241">Generating the initial HTML.</span></span>
* <span data-ttu-id="d12ea-242">Tratamiento del circuito como `disconnected` hasta que el explorador del usuario establece una conexión de SignalR al mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="d12ea-242">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="d12ea-243">Cuando se establece la conexión, se reanuda la interactividad en el circuito y se actualiza el marcado HTML de los componentes.</span><span class="sxs-lookup"><span data-stu-id="d12ea-243">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="d12ea-244">Si algún componente produce una excepción no controlada durante la representación previa, por ejemplo, durante un método de ciclo de vida o en la lógica de representación:</span><span class="sxs-lookup"><span data-stu-id="d12ea-244">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="d12ea-245">La excepción es grave para el circuito.</span><span class="sxs-lookup"><span data-stu-id="d12ea-245">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="d12ea-246">La excepción se inicia en la pila de llamadas de la aplicación auxiliar de etiquetas `Component`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-246">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="d12ea-247">Por lo tanto, se produce un error en toda la solicitud HTTP a menos que el código del desarrollador detecte la excepción explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-247">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="d12ea-248">En circunstancias normales, cuando se produce un error en la representación previa, la generación y representación del componente no tiene sentido, ya que no se puede representar un componente de trabajo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-248">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="d12ea-249">Para tolerar los errores que pueden producirse durante la representación previa, la lógica de control de errores debe colocarse dentro de un componente que pueda producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="d12ea-249">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="d12ea-250">Use instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.</span><span class="sxs-lookup"><span data-stu-id="d12ea-250">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="d12ea-251">En lugar de ajustar la aplicación auxiliar de etiquetas `Component` en una instrucción `try-catch`, coloque la lógica de control de errores en el componente representado por la aplicación auxiliar de etiquetas `Component`.</span><span class="sxs-lookup"><span data-stu-id="d12ea-251">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="d12ea-252">Escenarios avanzados</span><span class="sxs-lookup"><span data-stu-id="d12ea-252">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="d12ea-253">Representación recursiva</span><span class="sxs-lookup"><span data-stu-id="d12ea-253">Recursive rendering</span></span>

<span data-ttu-id="d12ea-254">Los componentes se pueden anidar de forma recursiva.</span><span class="sxs-lookup"><span data-stu-id="d12ea-254">Components can be nested recursively.</span></span> <span data-ttu-id="d12ea-255">Esto resulta útil para representar estructuras de datos recursivas.</span><span class="sxs-lookup"><span data-stu-id="d12ea-255">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="d12ea-256">Por ejemplo, un componente de `TreeNode` puede representar más componentes `TreeNode` para cada uno de los elementos secundarios del nodo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-256">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="d12ea-257">Cuando se represente de forma recursiva, evite patrones de codificación que produzcan una recursividad infinita:</span><span class="sxs-lookup"><span data-stu-id="d12ea-257">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="d12ea-258">No represente de forma recursiva una estructura de datos que contenga un ciclo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-258">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="d12ea-259">Por ejemplo, no represente un nodo de árbol cuyos elementos secundarios se incluyan a sí mismos.</span><span class="sxs-lookup"><span data-stu-id="d12ea-259">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="d12ea-260">No cree una cadena de diseños que contengan un ciclo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-260">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="d12ea-261">Por ejemplo, no cree un diseño cuyo diseño sea el mismo.</span><span class="sxs-lookup"><span data-stu-id="d12ea-261">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="d12ea-262">No permita que un usuario final viole invariantes de recursividad (reglas) a través de entradas de datos malintencionadas o llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d12ea-262">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="d12ea-263">Bucles infinitos durante la representación:</span><span class="sxs-lookup"><span data-stu-id="d12ea-263">Infinite loops during rendering:</span></span>

* <span data-ttu-id="d12ea-264">Hace que el proceso de representación continúe indefinidamente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-264">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="d12ea-265">Es equivalente a crear un bucle sin terminar.</span><span class="sxs-lookup"><span data-stu-id="d12ea-265">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="d12ea-266">En estos casos, el circuito afectado se bloquea y el subproceso normalmente intenta:</span><span class="sxs-lookup"><span data-stu-id="d12ea-266">In these scenarios, the affected circuit hangs, and the thread usually attempts to:</span></span>

* <span data-ttu-id="d12ea-267">Consume el tiempo de CPU permitido por el sistema operativo, indefinidamente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-267">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="d12ea-268">Consume una cantidad ilimitada de memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="d12ea-268">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="d12ea-269">Consumir memoria ilimitada es equivalente al escenario en el que un bucle sin terminar agrega entradas a una colección en cada iteración.</span><span class="sxs-lookup"><span data-stu-id="d12ea-269">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="d12ea-270">Para evitar patrones infinitos de recursividad, asegúrese de que el código de representación recursivo contiene condiciones de detención adecuadas.</span><span class="sxs-lookup"><span data-stu-id="d12ea-270">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="d12ea-271">Lógica de árbol de representación personalizada</span><span class="sxs-lookup"><span data-stu-id="d12ea-271">Custom render tree logic</span></span>

<span data-ttu-id="d12ea-272">La mayoría de los componentes de Blazor se implementan como archivos *. Razor* y se compilan para generar lógica que opere en un `RenderTreeBuilder` para representar su salida.</span><span class="sxs-lookup"><span data-stu-id="d12ea-272">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="d12ea-273">Un desarrollador puede implementar manualmente `RenderTreeBuilder` lógica mediante código C# de procedimiento.</span><span class="sxs-lookup"><span data-stu-id="d12ea-273">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="d12ea-274">Para obtener más información, vea <xref:blazor/components#manual-rendertreebuilder-logic>.</span><span class="sxs-lookup"><span data-stu-id="d12ea-274">For more information, see <xref:blazor/components#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="d12ea-275">El uso de la lógica del generador de árboles de representación manual se considera un escenario avanzado y no seguro, no recomendado para el desarrollo de componentes generales.</span><span class="sxs-lookup"><span data-stu-id="d12ea-275">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="d12ea-276">Si se escribe `RenderTreeBuilder` código, el desarrollador debe garantizar la corrección del código.</span><span class="sxs-lookup"><span data-stu-id="d12ea-276">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="d12ea-277">Por ejemplo, el desarrollador debe asegurarse de que:</span><span class="sxs-lookup"><span data-stu-id="d12ea-277">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="d12ea-278">Las llamadas a `OpenElement` y `CloseElement` están equilibradas correctamente.</span><span class="sxs-lookup"><span data-stu-id="d12ea-278">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="d12ea-279">Los atributos solo se agregan en los lugares correctos.</span><span class="sxs-lookup"><span data-stu-id="d12ea-279">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="d12ea-280">Una lógica incorrecta del generador de árboles de representación manual puede producir un comportamiento indefinido arbitrario, incluidos los bloqueos, los bloqueos del servidor y las vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d12ea-280">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="d12ea-281">Considere la posibilidad de representar la lógica del generador de árbol de representación manual en el mismo nivel de complejidad y con el mismo nivel de *riesgo* que escribir manualmente código de ensamblado o instrucciones MSIL.</span><span class="sxs-lookup"><span data-stu-id="d12ea-281">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
