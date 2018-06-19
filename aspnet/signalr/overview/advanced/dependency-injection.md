---
uid: signalr/overview/advanced/dependency-injection
title: Inserción de dependencias en SignalR | Documentos de Microsoft
author: MikeWasson
description: Versiones de software emplea en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504184"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="90ef2-103">Inserción de dependencias en SignalR</span><span class="sxs-lookup"><span data-stu-id="90ef2-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="90ef2-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="90ef2-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="90ef2-105">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="90ef2-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="90ef2-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="90ef2-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="90ef2-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="90ef2-107">.NET 4.5</span></span>
> - <span data-ttu-id="90ef2-108">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="90ef2-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="90ef2-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="90ef2-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="90ef2-110">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="90ef2-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="90ef2-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="90ef2-111">Questions and comments</span></span>
> 
> <span data-ttu-id="90ef2-112">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="90ef2-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="90ef2-113">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="90ef2-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="90ef2-114">Inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, lo que facilita para sustituir las dependencias de un objeto, ya sea para realizar pruebas (con objetos ficticios) o para cambiar el comportamiento de tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="90ef2-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="90ef2-115">Este tutorial muestra cómo realizar la inserción de dependencias en concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="90ef2-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="90ef2-116">También muestra cómo utilizar los contenedores de IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="90ef2-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="90ef2-117">Un contenedor de IoC es un marco de trabajo general para la inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="90ef2-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="90ef2-118">¿Qué es la inyección de dependencia?</span><span class="sxs-lookup"><span data-stu-id="90ef2-118">What is Dependency Injection?</span></span>

<span data-ttu-id="90ef2-119">Omitir esta sección si ya está familiarizado con la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="90ef2-120">*Inyección de dependencia* (DI) es un modelo donde los objetos no son responsables de crear sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="90ef2-121">Este es un ejemplo simple para motivar a DI.</span><span class="sxs-lookup"><span data-stu-id="90ef2-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="90ef2-122">Suponga que tiene un objeto que deba registrar los mensajes.</span><span class="sxs-lookup"><span data-stu-id="90ef2-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="90ef2-123">Puede definir una interfaz de registro:</span><span class="sxs-lookup"><span data-stu-id="90ef2-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="90ef2-124">En el objeto, puede crear un `ILogger` para registrar los mensajes:</span><span class="sxs-lookup"><span data-stu-id="90ef2-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="90ef2-125">Esto funciona, pero no es el mejor diseño.</span><span class="sxs-lookup"><span data-stu-id="90ef2-125">This works, but it's not the best design.</span></span> <span data-ttu-id="90ef2-126">Si desea reemplazar `FileLogger` con otro `ILogger` implementación, tendrá que modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="90ef2-127">Suponiendo que un lote de otros objetos `FileLogger`, deberá cambiar todas ellas.</span><span class="sxs-lookup"><span data-stu-id="90ef2-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="90ef2-128">O si decide hacer `FileLogger` un singleton, que también necesite realizar cambios en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="90ef2-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="90ef2-129">Es un enfoque más adecuado para "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="90ef2-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="90ef2-130">Ahora, el objeto no es responsable de seleccionar que `ILogger` a usar.</span><span class="sxs-lookup"><span data-stu-id="90ef2-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="90ef2-131">También puede swich `ILogger` implementaciones sin cambiar los objetos que dependen de él.</span><span class="sxs-lookup"><span data-stu-id="90ef2-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="90ef2-132">Este patrón se denomina [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="90ef2-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="90ef2-133">Otro patrón es la inserción de establecedor, donde establecer la dependencia a través de un método de establecedor o propiedad.</span><span class="sxs-lookup"><span data-stu-id="90ef2-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="90ef2-134">Inyección de dependencia simple en SignalR</span><span class="sxs-lookup"><span data-stu-id="90ef2-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="90ef2-135">Considere la posibilidad de la aplicación de Chat del tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="90ef2-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="90ef2-136">Esta es la clase de base de datos central de esa aplicación:</span><span class="sxs-lookup"><span data-stu-id="90ef2-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="90ef2-137">Suponga que desea almacenar los mensajes de chat en el servidor antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="90ef2-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="90ef2-138">Puede definir una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en la `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="90ef2-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="90ef2-139">El único problema es que una aplicación de SignalR no crea directamente de los centros; SignalR las creará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="90ef2-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="90ef2-140">De forma predeterminada, SignalR espera que una clase de concentrador para tener un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="90ef2-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="90ef2-141">Sin embargo, fácilmente puede registrar una función para crear instancias de base de datos central y usar esta función para realizar DI.</span><span class="sxs-lookup"><span data-stu-id="90ef2-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="90ef2-142">Registre la función mediante una llamada a **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="90ef2-143">Ahora SignalR invocará esta función anónima cuando sea necesario crear un `ChatHub` instancia.</span><span class="sxs-lookup"><span data-stu-id="90ef2-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="90ef2-144">Contenedores de IoC</span><span class="sxs-lookup"><span data-stu-id="90ef2-144">IoC Containers</span></span>

<span data-ttu-id="90ef2-145">El código anterior es correcto para los casos más sencillos.</span><span class="sxs-lookup"><span data-stu-id="90ef2-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="90ef2-146">Pero todavía tenía que escribir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="90ef2-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="90ef2-147">En una aplicación compleja con muchas dependencias, deberá escribir una gran cantidad de este código de "conexión".</span><span class="sxs-lookup"><span data-stu-id="90ef2-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="90ef2-148">Este código puede ser difícil de mantener, especialmente si se anidan las dependencias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="90ef2-149">También es difícil realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-149">It is also hard to unit test.</span></span>

<span data-ttu-id="90ef2-150">Una solución consiste en usar un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="90ef2-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="90ef2-151">Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, utilice el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="90ef2-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="90ef2-152">El contenedor imagina automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="90ef2-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="90ef2-153">Muchos de los contenedores de IoC también le permiten controlar aspectos como la duración de los objetos y el ámbito.</span><span class="sxs-lookup"><span data-stu-id="90ef2-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="90ef2-154">"IoC" es el acrónimo "de inversión de control", que es un patrón general donde un marco de trabajo llama al código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="90ef2-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="90ef2-155">Un contenedor de IoC construye los objetos para usted, que "invierte" el flujo de control normal.</span><span class="sxs-lookup"><span data-stu-id="90ef2-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="90ef2-156">Uso de contenedores de IoC en SignalR</span><span class="sxs-lookup"><span data-stu-id="90ef2-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="90ef2-157">La aplicación de Chat probablemente es demasiado simple para beneficiarse de un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="90ef2-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="90ef2-158">En su lugar, echemos un vistazo a la [indicador de cotizaciones](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ejemplo.</span><span class="sxs-lookup"><span data-stu-id="90ef2-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="90ef2-159">El ejemplo de indicador de cotizaciones define dos clases principales:</span><span class="sxs-lookup"><span data-stu-id="90ef2-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="90ef2-160">`StockTickerHub`: La clase de base de datos central, que administra las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="90ef2-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="90ef2-161">`StockTicker`: Un singleton que contiene los precios de las acciones y los actualiza periódicamente.</span><span class="sxs-lookup"><span data-stu-id="90ef2-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="90ef2-162">`StockTickerHub`contiene una referencia a la `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a la **IHubConnectionContext** para el `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="90ef2-163">Usa esta interfaz para comunicarse con `StockTickerHub` instancias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="90ef2-164">(Para obtener más información, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="90ef2-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="90ef2-165">Podemos usar un contenedor de IoC para resolver estas dependencias un poco.</span><span class="sxs-lookup"><span data-stu-id="90ef2-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="90ef2-166">En primer lugar, vamos a simplificar la `StockTickerHub` y `StockTicker` clases.</span><span class="sxs-lookup"><span data-stu-id="90ef2-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="90ef2-167">En el código siguiente, he comentadas las partes que no es necesario.</span><span class="sxs-lookup"><span data-stu-id="90ef2-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="90ef2-168">Quite el constructor sin parámetros de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="90ef2-169">En su lugar, usaremos siempre DI para crear el concentrador.</span><span class="sxs-lookup"><span data-stu-id="90ef2-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="90ef2-170">Para el indicador de cotizaciones, quite la instancia de singleton.</span><span class="sxs-lookup"><span data-stu-id="90ef2-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="90ef2-171">Más adelante, vamos a usar el contenedor de IoC para controlar la duración de indicador de cotizaciones.</span><span class="sxs-lookup"><span data-stu-id="90ef2-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="90ef2-172">Además, marque el constructor público.</span><span class="sxs-lookup"><span data-stu-id="90ef2-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="90ef2-173">A continuación, se puede refactorizar el código mediante la creación de una interfaz para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="90ef2-174">Vamos a usar esta interfaz para desacoplar el `StockTickerHub` desde el `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="90ef2-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="90ef2-175">Visual Studio hace que este tipo de refactorización la forma más fácil.</span><span class="sxs-lookup"><span data-stu-id="90ef2-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="90ef2-176">Abra el archivo StockTicker.cs, haga doble clic en el `StockTicker` declaración de clase y seleccione **refactorizar** ... **Extraer interfaz**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="90ef2-177">En el **Extraer interfaz** cuadro de diálogo, haga clic en **seleccionar todo**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="90ef2-178">Deje los demás valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="90ef2-178">Leave the other defaults.</span></span> <span data-ttu-id="90ef2-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="90ef2-180">Visual Studio crea una nueva interfaz denominada `IStockTicker`y también se cambia `StockTicker` pueden derivar `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="90ef2-181">Abra el archivo IStockTicker.cs y cambiar la interfaz a **público**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="90ef2-182">En el `StockTickerHub` clase, cambie las dos instancias de `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="90ef2-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="90ef2-183">Crear un `IStockTicker` interfaz no es estrictamente necesaria, pero quise mostrarle cómo DI puede ayudar a reducir el acoplamiento entre los componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="90ef2-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="90ef2-184">Agregue la biblioteca de Ninject</span><span class="sxs-lookup"><span data-stu-id="90ef2-184">Add the Ninject Library</span></span>

<span data-ttu-id="90ef2-185">Hay muchos contenedores de IoC de código abierto para. NET.</span><span class="sxs-lookup"><span data-stu-id="90ef2-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="90ef2-186">En este tutorial, utilizará [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="90ef2-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="90ef2-187">(Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), y [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="90ef2-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="90ef2-188">Use el Administrador de paquetes de NuGet para instalar el [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="90ef2-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="90ef2-189">En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="90ef2-190">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="90ef2-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="90ef2-191">Reemplace a la resolución de dependencia de SignalR</span><span class="sxs-lookup"><span data-stu-id="90ef2-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="90ef2-192">Para utilizar Ninject en SignalR, cree una clase que deriva de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="90ef2-193">Esta clase reemplaza la **GetService** y **GetServices** métodos de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="90ef2-194">SignalR llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias de base de datos central, así como varios servicios utilizados internamente por SignalR.</span><span class="sxs-lookup"><span data-stu-id="90ef2-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="90ef2-195">El **GetService** método crea una única instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="90ef2-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="90ef2-196">Invalide este método para llamar el kernel Ninject **TryGet** método.</span><span class="sxs-lookup"><span data-stu-id="90ef2-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="90ef2-197">Si el método devuelve null, revertir a la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="90ef2-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="90ef2-198">El **GetServices** método crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="90ef2-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="90ef2-199">Invalide este método para concatenar los resultados de Ninject con los resultados de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="90ef2-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="90ef2-200">Configurar los enlaces de Ninject</span><span class="sxs-lookup"><span data-stu-id="90ef2-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="90ef2-201">Ahora vamos a usar Ninject para declarar enlaces de tipo.</span><span class="sxs-lookup"><span data-stu-id="90ef2-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="90ef2-202">Abra la clase de la aplicación Startup.cs (uno creado manualmente según las instrucciones de paquete de `readme.txt`, o que se creó mediante la adición de autenticación para el proyecto).</span><span class="sxs-lookup"><span data-stu-id="90ef2-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="90ef2-203">En el `Startup.Configuration` (método), crear el contenedor Ninject, que llama a Ninject el *kernel*.</span><span class="sxs-lookup"><span data-stu-id="90ef2-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="90ef2-204">Cree una instancia de la resolución de dependencia personalizados:</span><span class="sxs-lookup"><span data-stu-id="90ef2-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="90ef2-205">Cree un enlace para `IStockTicker` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="90ef2-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="90ef2-206">Este código está diciendo dos cosas.</span><span class="sxs-lookup"><span data-stu-id="90ef2-206">This code is saying two things.</span></span> <span data-ttu-id="90ef2-207">En primer lugar, siempre que la aplicación necesita una `IStockTicker`, el kernel debe crearse una instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="90ef2-208">Segundo, la `StockTicker` clase debe ser otra creada como un objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="90ef2-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="90ef2-209">Ninject creará una instancia del objeto y devolver la misma instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="90ef2-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="90ef2-210">Cree un enlace para **IHubConnectionContext** como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="90ef2-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="90ef2-211">Este creatres código una función anónima que devuelve un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="90ef2-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="90ef2-212">El **WhenInjectedInto** método indica Ninject para usar esta función sólo cuando se crea `IStockTicker` instancias.</span><span class="sxs-lookup"><span data-stu-id="90ef2-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="90ef2-213">La razón es que crea SignalR **IHubConnectionContext** instancias internamente, y no desea reemplazar el modo en que crea SignalR.</span><span class="sxs-lookup"><span data-stu-id="90ef2-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="90ef2-214">Esta función solo se aplica a nuestro `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="90ef2-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="90ef2-215">Pasar la resolución de dependencia en el **MapSignalR** método mediante la adición de una configuración de base de datos central:</span><span class="sxs-lookup"><span data-stu-id="90ef2-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="90ef2-216">Actualice el método Startup.ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:</span><span class="sxs-lookup"><span data-stu-id="90ef2-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="90ef2-217">Ahora va a utilizar el solucionador especificado en SignalR **MapSignalR**, en lugar de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="90ef2-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="90ef2-218">Esta es la lista de código completa `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="90ef2-219">Presione F5 para ejecutar la aplicación de indicador de cotizaciones en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90ef2-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="90ef2-220">En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="90ef2-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="90ef2-221">La aplicación tiene exactamente la misma funcionalidad que antes.</span><span class="sxs-lookup"><span data-stu-id="90ef2-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="90ef2-222">(Para obtener una descripción, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) No hemos cambiamos el comportamiento; acaba de crear más fácil probar, mantener y desarrollar el código.</span><span class="sxs-lookup"><span data-stu-id="90ef2-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
