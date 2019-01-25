---
uid: signalr/overview/older-versions/dependency-injection
title: Inserción de dependencias en SignalR 1.x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: c8aac09c3d3e06529f7834eb3f60dca2f3073922
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836298"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="c59ea-102">Inserción de dependencias en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="c59ea-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="c59ea-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c59ea-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="c59ea-104">Inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, facilitando la tarea para reemplazar las dependencias de un objeto, ya sea por pruebas (con objetos ficticios) o para cambiar el comportamiento de tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="c59ea-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="c59ea-105">Este tutorial muestra cómo realizar la inserción de dependencias en concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c59ea-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="c59ea-106">También muestra cómo usar contenedores de IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="c59ea-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="c59ea-107">Un contenedor de IoC es un marco general para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="c59ea-108">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="c59ea-108">What is Dependency Injection?</span></span>

<span data-ttu-id="c59ea-109">Omitir esta sección si ya está familiarizado con la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="c59ea-110">*Inserción de dependencias* (DI) es un patrón donde los objetos no son responsables de crear sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="c59ea-111">Este es un ejemplo sencillo para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="c59ea-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="c59ea-112">Suponga que tiene un objeto que se debe registrar los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c59ea-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="c59ea-113">Podría definir una interfaz de registro:</span><span class="sxs-lookup"><span data-stu-id="c59ea-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c59ea-114">En el objeto, puede crear un `ILogger` para registrar los mensajes:</span><span class="sxs-lookup"><span data-stu-id="c59ea-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c59ea-115">Esto funciona, pero no es el mejor diseño.</span><span class="sxs-lookup"><span data-stu-id="c59ea-115">This works, but it's not the best design.</span></span> <span data-ttu-id="c59ea-116">Si desea reemplazar `FileLogger` con otra `ILogger` implementación, tendrá que modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="c59ea-117">Si suponemos que una gran cantidad de otros objetos utilizan `FileLogger`, deberá cambiar todas ellas.</span><span class="sxs-lookup"><span data-stu-id="c59ea-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="c59ea-118">O si decide realizar `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c59ea-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="c59ea-119">Un mejor enfoque es "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="c59ea-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c59ea-120">Ahora, el objeto no es responsable de seleccionarlos `ILogger` a usar.</span><span class="sxs-lookup"><span data-stu-id="c59ea-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="c59ea-121">Puede swich `ILogger` implementaciones sin cambiar los objetos que dependen de él.</span><span class="sxs-lookup"><span data-stu-id="c59ea-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c59ea-122">Este patrón se denomina [inserción del constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c59ea-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c59ea-123">Otro patrón es la inserción del establecedor, donde establece la dependencia a través de un método establecedor o una propiedad.</span><span class="sxs-lookup"><span data-stu-id="c59ea-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="c59ea-124">Inserción de dependencias simple en SignalR</span><span class="sxs-lookup"><span data-stu-id="c59ea-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="c59ea-125">Considere la posibilidad de la aplicación de Chat del tutorial [Introducción a SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c59ea-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="c59ea-126">Esta es la clase de hub desde la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c59ea-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c59ea-127">Suponga que desea almacenar los mensajes de chat en el servidor antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="c59ea-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="c59ea-128">Es posible que defina una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en el `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="c59ea-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c59ea-129">El único problema es que una aplicación de SignalR no crea directamente hubs; SignalR crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c59ea-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="c59ea-130">De forma predeterminada, SignalR espera una clase de concentrador para tener un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="c59ea-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="c59ea-131">Sin embargo, fácilmente puede registrar una función para crear instancias de concentrador y usar esta función para realizar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="c59ea-132">Registre la función mediante una llamada a **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="c59ea-133">Ahora SignalR invocará esta función anónima cada vez que necesita para crear un `ChatHub` instancia.</span><span class="sxs-lookup"><span data-stu-id="c59ea-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="c59ea-134">Contenedores de IoC</span><span class="sxs-lookup"><span data-stu-id="c59ea-134">IoC Containers</span></span>

<span data-ttu-id="c59ea-135">El código anterior está bien para los casos más sencillos.</span><span class="sxs-lookup"><span data-stu-id="c59ea-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="c59ea-136">Pero todavía tenía que escribir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c59ea-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="c59ea-137">En una aplicación compleja con muchas dependencias, debe escribir una gran cantidad de este código de "conexión".</span><span class="sxs-lookup"><span data-stu-id="c59ea-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="c59ea-138">Este código puede ser difícil de mantener, especialmente si se anidan las dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="c59ea-139">También es difícil realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-139">It is also hard to unit test.</span></span>

<span data-ttu-id="c59ea-140">Una solución consiste en usar un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="c59ea-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="c59ea-141">Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, usar el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="c59ea-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c59ea-142">El contenedor calcula automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="c59ea-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c59ea-143">Muchos de los contenedores de IoC también permiten controlar aspectos como la duración del objeto y el ámbito.</span><span class="sxs-lookup"><span data-stu-id="c59ea-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c59ea-144">"IoC" es el acrónimo "inversión de control", que es un patrón general que llama un marco de trabajo en el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c59ea-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c59ea-145">Un contenedor de IoC construye los objetos, que "invierte" el flujo de control normal.</span><span class="sxs-lookup"><span data-stu-id="c59ea-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="c59ea-146">Uso de contenedores de IoC en SignalR</span><span class="sxs-lookup"><span data-stu-id="c59ea-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="c59ea-147">La aplicación de Chat probablemente es demasiado simple para beneficiarse de un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="c59ea-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="c59ea-148">En su lugar, echemos un vistazo a la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ejemplo.</span><span class="sxs-lookup"><span data-stu-id="c59ea-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="c59ea-149">El ejemplo StockTicker define dos clases principales:</span><span class="sxs-lookup"><span data-stu-id="c59ea-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="c59ea-150">`StockTickerHub`: La clase de hub, que administra las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="c59ea-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="c59ea-151">`StockTicker`: Un singleton que contiene los precios de las acciones y los actualiza periódicamente.</span><span class="sxs-lookup"><span data-stu-id="c59ea-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="c59ea-152">`StockTickerHub` contiene una referencia a la `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a la **IHubConnectionContext** para el `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="c59ea-153">Usa esta interfaz para comunicarse con `StockTickerHub` instancias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="c59ea-154">(Para obtener más información, consulte [difusión de servidores con ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="c59ea-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="c59ea-155">Podemos usar un contenedor de IoC ahorró un poco de estas dependencias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="c59ea-156">En primer lugar, vamos a simplificar la `StockTickerHub` y `StockTicker` clases.</span><span class="sxs-lookup"><span data-stu-id="c59ea-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="c59ea-157">En el siguiente código, he comentado las partes que no es necesario.</span><span class="sxs-lookup"><span data-stu-id="c59ea-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="c59ea-158">Quite el constructor sin parámetros de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="c59ea-159">En su lugar, usaremos siempre DI para crear el centro.</span><span class="sxs-lookup"><span data-stu-id="c59ea-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="c59ea-160">Para StockTicker, quite la instancia de singleton.</span><span class="sxs-lookup"><span data-stu-id="c59ea-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="c59ea-161">Más adelante, vamos a usar el contenedor de IoC para controlar la duración de StockTicker.</span><span class="sxs-lookup"><span data-stu-id="c59ea-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="c59ea-162">Además, hacer público el constructor.</span><span class="sxs-lookup"><span data-stu-id="c59ea-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="c59ea-163">A continuación, podemos refactorizar el código mediante la creación de una interfaz para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="c59ea-164">Vamos a usar esta interfaz para desacoplar el `StockTickerHub` desde el `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="c59ea-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="c59ea-165">Visual Studio hace que este tipo de refactorización sencilla.</span><span class="sxs-lookup"><span data-stu-id="c59ea-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="c59ea-166">Abra el archivo StockTicker.cs, haga doble clic en el `StockTicker` declaración de clase y seleccione **refactorizar** ... **Extraer interfaz**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="c59ea-167">En el **Extraer interfaz** cuadro de diálogo, haga clic en **seleccionar todo**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="c59ea-168">Deje los demás valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="c59ea-168">Leave the other defaults.</span></span> <span data-ttu-id="c59ea-169">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="c59ea-170">Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` derivar `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="c59ea-171">Abra el archivo IStockTicker.cs y cambiar la interfaz a **pública**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c59ea-172">En el `StockTickerHub` clase, cambie las dos instancias de `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="c59ea-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="c59ea-173">Creación de un `IStockTicker` interfaz no es estrictamente necesaria, pero quise mostrarle cómo inserción de dependencias puede ayudar a reducir el acoplamiento entre componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c59ea-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="c59ea-174">Agregue la biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="c59ea-174">Add the Ninject Library</span></span>

<span data-ttu-id="c59ea-175">Hay muchos contenedores de IoC de código abierto para. NET.</span><span class="sxs-lookup"><span data-stu-id="c59ea-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="c59ea-176">Para este tutorial, utilizaré [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="c59ea-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="c59ea-177">(Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), y [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="c59ea-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="c59ea-178">Use el Administrador de paquetes NuGet para instalar el [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="c59ea-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="c59ea-179">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="c59ea-180">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c59ea-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="c59ea-181">Reemplace a la resolución de dependencia de SignalR</span><span class="sxs-lookup"><span data-stu-id="c59ea-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="c59ea-182">Para usar Ninject en SignalR, cree una clase que derive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="c59ea-183">Esta clase invalida el **GetService** y **GetServices** métodos de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="c59ea-184">SignalR se llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias del concentrador, así como varios servicios utilizados internamente por SignalR.</span><span class="sxs-lookup"><span data-stu-id="c59ea-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="c59ea-185">El **GetService** método crea una instancia única de un tipo.</span><span class="sxs-lookup"><span data-stu-id="c59ea-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="c59ea-186">Invalide este método para llamar el kernel Ninject **TryGet** método.</span><span class="sxs-lookup"><span data-stu-id="c59ea-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="c59ea-187">Si el método devuelve null, revertir a la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c59ea-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="c59ea-188">El **GetServices** método crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="c59ea-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="c59ea-189">Invalide este método para concatenar los resultados de Ninject con los resultados de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c59ea-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="c59ea-190">Configurar enlaces Ninject</span><span class="sxs-lookup"><span data-stu-id="c59ea-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="c59ea-191">Ahora vamos a usar Ninject para declarar los enlaces de tipo.</span><span class="sxs-lookup"><span data-stu-id="c59ea-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="c59ea-192">Abra el archivo RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="c59ea-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="c59ea-193">En el `RegisterHubs.Start` método, cree el contenedor Ninject, que llama Ninject el *kernel*.</span><span class="sxs-lookup"><span data-stu-id="c59ea-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="c59ea-194">Cree una instancia de la resolución de dependencia personalizada:</span><span class="sxs-lookup"><span data-stu-id="c59ea-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="c59ea-195">Crear un enlace para `IStockTicker` como sigue:</span><span class="sxs-lookup"><span data-stu-id="c59ea-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="c59ea-196">Este código indica que dos cosas.</span><span class="sxs-lookup"><span data-stu-id="c59ea-196">This code is saying two things.</span></span> <span data-ttu-id="c59ea-197">En primer lugar, siempre que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="c59ea-198">Segundo, el `StockTicker` clase debe ser un creado como un objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="c59ea-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="c59ea-199">Ninject creará una instancia del objeto y devolver la misma instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="c59ea-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="c59ea-200">Crear un enlace para **IHubConnectionContext** como sigue:</span><span class="sxs-lookup"><span data-stu-id="c59ea-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="c59ea-201">Este creatres código una función anónima que devuelve un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="c59ea-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="c59ea-202">El **WhenInjectedInto** método indica Ninject para usar esta función sólo cuando se crea `IStockTicker` instancias.</span><span class="sxs-lookup"><span data-stu-id="c59ea-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="c59ea-203">El motivo es que crea SignalR **IHubConnectionContext** instancias internamente, y no queremos invalidar cómo los crea SignalR.</span><span class="sxs-lookup"><span data-stu-id="c59ea-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="c59ea-204">Esta función solo se aplica a nuestro `StockTicker` clase.</span><span class="sxs-lookup"><span data-stu-id="c59ea-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="c59ea-205">Pasar la resolución de dependencia en el **MapHubs** método:</span><span class="sxs-lookup"><span data-stu-id="c59ea-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="c59ea-206">Ahora va a utilizar el solucionador especificado en SignalR **MapHubs**, en lugar de la resolución predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c59ea-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="c59ea-207">Esta es la lista de código completa `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="c59ea-208">Para ejecutar la aplicación StockTicker en Visual Studio, presione F5.</span><span class="sxs-lookup"><span data-stu-id="c59ea-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="c59ea-209">En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="c59ea-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="c59ea-210">La aplicación tiene exactamente la misma funcionalidad que antes.</span><span class="sxs-lookup"><span data-stu-id="c59ea-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="c59ea-211">(Para obtener una descripción, consulte [difusión de servidores con ASP.NET SignalR](index.md).) Que no hemos cambiado el comportamiento; acaba de crear el código más fácil de probar, mantener y evolucionar.</span><span class="sxs-lookup"><span data-stu-id="c59ea-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
