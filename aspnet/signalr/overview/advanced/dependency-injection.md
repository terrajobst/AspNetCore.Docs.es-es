---
uid: signalr/overview/advanced/dependency-injection
title: Inserción de dependencias en SignalR | Microsoft Docs
author: bradygaster
description: Las versiones de software usan en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 54e263e277852d2d478ce5bccd4164254498831a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836615"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="28907-103">Inserción de dependencias en SignalR</span><span class="sxs-lookup"><span data-stu-id="28907-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="28907-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="28907-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="28907-105">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="28907-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="28907-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="28907-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="28907-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="28907-107">.NET 4.5</span></span>
> - <span data-ttu-id="28907-108">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="28907-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="28907-109">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="28907-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="28907-110">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="28907-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="28907-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="28907-111">Questions and comments</span></span>
>
> <span data-ttu-id="28907-112">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="28907-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="28907-113">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="28907-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="28907-114">Inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, facilitando la tarea para reemplazar las dependencias de un objeto, ya sea por pruebas (con objetos ficticios) o para cambiar el comportamiento de tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="28907-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="28907-115">Este tutorial muestra cómo realizar la inserción de dependencias en concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="28907-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="28907-116">También muestra cómo usar contenedores de IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="28907-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="28907-117">Un contenedor de IoC es un marco general para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="28907-118">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="28907-118">What is Dependency Injection?</span></span>

<span data-ttu-id="28907-119">Omitir esta sección si ya está familiarizado con la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="28907-120">*Inserción de dependencias* (DI) es un patrón donde los objetos no son responsables de crear sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="28907-121">Este es un ejemplo sencillo para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="28907-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="28907-122">Suponga que tiene un objeto que se debe registrar los mensajes.</span><span class="sxs-lookup"><span data-stu-id="28907-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="28907-123">Podría definir una interfaz de registro:</span><span class="sxs-lookup"><span data-stu-id="28907-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="28907-124">En el objeto, puede crear un `ILogger` para registrar los mensajes:</span><span class="sxs-lookup"><span data-stu-id="28907-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="28907-125">Esto funciona, pero no es el mejor diseño.</span><span class="sxs-lookup"><span data-stu-id="28907-125">This works, but it's not the best design.</span></span> <span data-ttu-id="28907-126">Si desea reemplazar `FileLogger` con otra `ILogger` implementación, tendrá que modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="28907-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="28907-127">Si suponemos que una gran cantidad de otros objetos utilizan `FileLogger`, deberá cambiar todas ellas.</span><span class="sxs-lookup"><span data-stu-id="28907-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="28907-128">O si decide realizar `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28907-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="28907-129">Un mejor enfoque es "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="28907-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="28907-130">Ahora, el objeto no es responsable de seleccionarlos `ILogger` a usar.</span><span class="sxs-lookup"><span data-stu-id="28907-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="28907-131">Puede swich `ILogger` implementaciones sin cambiar los objetos que dependen de él.</span><span class="sxs-lookup"><span data-stu-id="28907-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="28907-132">Este patrón se denomina [inserción del constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="28907-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="28907-133">Otro patrón es la inserción del establecedor, donde establece la dependencia a través de un método establecedor o una propiedad.</span><span class="sxs-lookup"><span data-stu-id="28907-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="28907-134">Inserción de dependencias simple en SignalR</span><span class="sxs-lookup"><span data-stu-id="28907-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="28907-135">Considere la posibilidad de la aplicación de Chat del tutorial [Introducción a SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="28907-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="28907-136">Esta es la clase de hub desde la aplicación:</span><span class="sxs-lookup"><span data-stu-id="28907-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="28907-137">Suponga que desea almacenar los mensajes de chat en el servidor antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="28907-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="28907-138">Es posible que defina una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en el `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="28907-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="28907-139">El único problema es que una aplicación de SignalR no crea directamente hubs; SignalR crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="28907-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="28907-140">De forma predeterminada, SignalR espera una clase de concentrador para tener un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="28907-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="28907-141">Sin embargo, fácilmente puede registrar una función para crear instancias de concentrador y usar esta función para realizar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="28907-142">Registre la función mediante una llamada a **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="28907-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="28907-143">Ahora SignalR invocará esta función anónima cada vez que necesita para crear un `ChatHub` instancia.</span><span class="sxs-lookup"><span data-stu-id="28907-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="28907-144">Contenedores de IoC</span><span class="sxs-lookup"><span data-stu-id="28907-144">IoC Containers</span></span>

<span data-ttu-id="28907-145">El código anterior está bien para los casos más sencillos.</span><span class="sxs-lookup"><span data-stu-id="28907-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="28907-146">Pero todavía tenía que escribir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="28907-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="28907-147">En una aplicación compleja con muchas dependencias, debe escribir una gran cantidad de este código de "conexión".</span><span class="sxs-lookup"><span data-stu-id="28907-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="28907-148">Este código puede ser difícil de mantener, especialmente si se anidan las dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="28907-149">También es difícil realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="28907-149">It is also hard to unit test.</span></span>

<span data-ttu-id="28907-150">Una solución consiste en usar un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="28907-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="28907-151">Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, usar el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="28907-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="28907-152">El contenedor calcula automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="28907-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="28907-153">Muchos de los contenedores de IoC también permiten controlar aspectos como la duración del objeto y el ámbito.</span><span class="sxs-lookup"><span data-stu-id="28907-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="28907-154">"IoC" es el acrónimo "inversión de control", que es un patrón general que llama un marco de trabajo en el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28907-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="28907-155">Un contenedor de IoC construye los objetos, que "invierte" el flujo de control normal.</span><span class="sxs-lookup"><span data-stu-id="28907-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="28907-156">Uso de contenedores de IoC en SignalR</span><span class="sxs-lookup"><span data-stu-id="28907-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="28907-157">La aplicación de Chat probablemente es demasiado simple para beneficiarse de un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="28907-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="28907-158">En su lugar, echemos un vistazo a la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ejemplo.</span><span class="sxs-lookup"><span data-stu-id="28907-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="28907-159">El ejemplo StockTicker define dos clases principales:</span><span class="sxs-lookup"><span data-stu-id="28907-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="28907-160">`StockTickerHub`: La clase de hub, que administra las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="28907-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="28907-161">`StockTicker`: Un singleton que contiene los precios de las acciones y los actualiza periódicamente.</span><span class="sxs-lookup"><span data-stu-id="28907-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="28907-162">`StockTickerHub` contiene una referencia a la `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a la **IHubConnectionContext** para el `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="28907-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="28907-163">Usa esta interfaz para comunicarse con `StockTickerHub` instancias.</span><span class="sxs-lookup"><span data-stu-id="28907-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="28907-164">(Para obtener más información, consulte [difusión de servidores con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="28907-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="28907-165">Podemos usar un contenedor de IoC ahorró un poco de estas dependencias.</span><span class="sxs-lookup"><span data-stu-id="28907-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="28907-166">En primer lugar, vamos a simplificar la `StockTickerHub` y `StockTicker` clases.</span><span class="sxs-lookup"><span data-stu-id="28907-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="28907-167">En el siguiente código, he comentado las partes que no es necesario.</span><span class="sxs-lookup"><span data-stu-id="28907-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="28907-168">Quite el constructor sin parámetros de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="28907-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="28907-169">En su lugar, usaremos siempre DI para crear el centro.</span><span class="sxs-lookup"><span data-stu-id="28907-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="28907-170">Para StockTicker, quite la instancia de singleton.</span><span class="sxs-lookup"><span data-stu-id="28907-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="28907-171">Más adelante, vamos a usar el contenedor de IoC para controlar la duración de StockTicker.</span><span class="sxs-lookup"><span data-stu-id="28907-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="28907-172">Además, hacer público el constructor.</span><span class="sxs-lookup"><span data-stu-id="28907-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="28907-173">A continuación, podemos refactorizar el código mediante la creación de una interfaz para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="28907-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="28907-174">Vamos a usar esta interfaz para desacoplar el `StockTickerHub` desde el `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="28907-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="28907-175">Visual Studio hace que este tipo de refactorización sencilla.</span><span class="sxs-lookup"><span data-stu-id="28907-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="28907-176">Abra el archivo StockTicker.cs, haga doble clic en el `StockTicker` declaración de clase y seleccione **refactorizar** ... **Extraer interfaz**.</span><span class="sxs-lookup"><span data-stu-id="28907-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="28907-177">En el **Extraer interfaz** cuadro de diálogo, haga clic en **seleccionar todo**.</span><span class="sxs-lookup"><span data-stu-id="28907-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="28907-178">Deje los demás valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="28907-178">Leave the other defaults.</span></span> <span data-ttu-id="28907-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="28907-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="28907-180">Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` derivar `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="28907-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="28907-181">Abra el archivo IStockTicker.cs y cambiar la interfaz a **pública**.</span><span class="sxs-lookup"><span data-stu-id="28907-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="28907-182">En el `StockTickerHub` clase, cambie las dos instancias de `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="28907-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="28907-183">Creación de un `IStockTicker` interfaz no es estrictamente necesaria, pero quise mostrarle cómo inserción de dependencias puede ayudar a reducir el acoplamiento entre componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28907-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="28907-184">Agregue la biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="28907-184">Add the Ninject Library</span></span>

<span data-ttu-id="28907-185">Hay muchos contenedores de IoC de código abierto para. NET.</span><span class="sxs-lookup"><span data-stu-id="28907-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="28907-186">Para este tutorial, utilizaré [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="28907-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="28907-187">(Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), y [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="28907-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="28907-188">Use el Administrador de paquetes NuGet para instalar el [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="28907-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="28907-189">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="28907-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="28907-190">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="28907-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="28907-191">Reemplace a la resolución de dependencia de SignalR</span><span class="sxs-lookup"><span data-stu-id="28907-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="28907-192">Para usar Ninject en SignalR, cree una clase que derive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="28907-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="28907-193">Esta clase invalida el **GetService** y **GetServices** métodos de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="28907-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="28907-194">SignalR se llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias del concentrador, así como varios servicios utilizados internamente por SignalR.</span><span class="sxs-lookup"><span data-stu-id="28907-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="28907-195">El **GetService** método crea una instancia única de un tipo.</span><span class="sxs-lookup"><span data-stu-id="28907-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="28907-196">Invalide este método para llamar el kernel Ninject **TryGet** método.</span><span class="sxs-lookup"><span data-stu-id="28907-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="28907-197">Si el método devuelve null, revertir a la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="28907-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="28907-198">El **GetServices** método crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="28907-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="28907-199">Invalide este método para concatenar los resultados de Ninject con los resultados de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="28907-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="28907-200">Configurar enlaces Ninject</span><span class="sxs-lookup"><span data-stu-id="28907-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="28907-201">Ahora vamos a usar Ninject para declarar los enlaces de tipo.</span><span class="sxs-lookup"><span data-stu-id="28907-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="28907-202">Abra la clase de la aplicación Startup.cs (ya sea creados manualmente según las instrucciones del paquete de `readme.txt`, o que se creó mediante la adición de autenticación a su proyecto).</span><span class="sxs-lookup"><span data-stu-id="28907-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="28907-203">En el `Startup.Configuration` método, cree el contenedor Ninject, que llama Ninject el *kernel*.</span><span class="sxs-lookup"><span data-stu-id="28907-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="28907-204">Cree una instancia de la resolución de dependencia personalizada:</span><span class="sxs-lookup"><span data-stu-id="28907-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="28907-205">Crear un enlace para `IStockTicker` como sigue:</span><span class="sxs-lookup"><span data-stu-id="28907-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="28907-206">Este código indica que dos cosas.</span><span class="sxs-lookup"><span data-stu-id="28907-206">This code is saying two things.</span></span> <span data-ttu-id="28907-207">En primer lugar, siempre que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="28907-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="28907-208">Segundo, el `StockTicker` clase debe ser un creado como un objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="28907-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="28907-209">Ninject creará una instancia del objeto y devolver la misma instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="28907-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="28907-210">Crear un enlace para **IHubConnectionContext** como sigue:</span><span class="sxs-lookup"><span data-stu-id="28907-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="28907-211">Este creatres código una función anónima que devuelve un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="28907-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="28907-212">El **WhenInjectedInto** método indica Ninject para usar esta función sólo cuando se crea `IStockTicker` instancias.</span><span class="sxs-lookup"><span data-stu-id="28907-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="28907-213">El motivo es que crea SignalR **IHubConnectionContext** instancias internamente, y no queremos invalidar cómo los crea SignalR.</span><span class="sxs-lookup"><span data-stu-id="28907-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="28907-214">Esta función solo se aplica a nuestro `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="28907-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="28907-215">Pasar la resolución de dependencia en el **MapSignalR** método mediante la adición de una configuración de concentrador:</span><span class="sxs-lookup"><span data-stu-id="28907-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="28907-216">Actualice el método Startup.ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:</span><span class="sxs-lookup"><span data-stu-id="28907-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="28907-217">Ahora va a utilizar el solucionador especificado en SignalR **MapSignalR**, en lugar de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="28907-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="28907-218">Esta es la lista de código completa `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="28907-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="28907-219">Para ejecutar la aplicación StockTicker en Visual Studio, presione F5.</span><span class="sxs-lookup"><span data-stu-id="28907-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="28907-220">En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="28907-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="28907-221">La aplicación tiene exactamente la misma funcionalidad que antes.</span><span class="sxs-lookup"><span data-stu-id="28907-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="28907-222">(Para obtener una descripción, consulte [difusión de servidores con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Que no hemos cambiado el comportamiento; acaba de crear el código más fácil de probar, mantener y evolucionar.</span><span class="sxs-lookup"><span data-stu-id="28907-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
