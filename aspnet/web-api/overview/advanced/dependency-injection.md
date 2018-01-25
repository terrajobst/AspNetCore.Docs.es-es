---
uid: web-api/overview/advanced/dependency-injection
title: "Inserción de dependencias en ASP.NET Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: "Este tutorial muestra cómo insertar las dependencias en el controlador de ASP.NET Web API. Versiones de software que se usa en el bloque de aplicaciones de Web API 2 Unity tutorial..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="ebe47-104">Inserción de dependencias en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ebe47-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="ebe47-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ebe47-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ebe47-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ebe47-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="ebe47-107">Este tutorial muestra cómo insertar las dependencias en el controlador de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ebe47-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ebe47-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ebe47-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ebe47-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="ebe47-109">Web API 2</span></span>
> - [<span data-ttu-id="ebe47-110">Bloque de aplicación de Unity</span><span class="sxs-lookup"><span data-stu-id="ebe47-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="ebe47-111">Entity Framework 6 (versión 5 también funciona)</span><span class="sxs-lookup"><span data-stu-id="ebe47-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="ebe47-112">¿Qué es la inyección de dependencia?</span><span class="sxs-lookup"><span data-stu-id="ebe47-112">What is Dependency Injection?</span></span>

<span data-ttu-id="ebe47-113">A *dependencia* es cualquier objeto que requiere otro objeto.</span><span class="sxs-lookup"><span data-stu-id="ebe47-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="ebe47-114">Por ejemplo, es común para definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controla el acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="ebe47-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="ebe47-115">Vamos a mostrar con un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ebe47-115">Let's illustrate with an example.</span></span> <span data-ttu-id="ebe47-116">En primer lugar, definiremos un modelo de dominio:</span><span class="sxs-lookup"><span data-stu-id="ebe47-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="ebe47-117">Aquí es una clase de repositorio simple que almacena los elementos en una base de datos, mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ebe47-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="ebe47-118">Ahora vamos a definir un controlador de API Web que admita solicitudes GET para `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="ebe47-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="ebe47-119">(Estoy dejando fuera POST y otros métodos para simplificar el trabajo.) Este es el primer intento:</span><span class="sxs-lookup"><span data-stu-id="ebe47-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="ebe47-120">Tenga en cuenta que depende de la clase de controlador `ProductRepository`, y nos estamos permitiendo que el controlador de crear el `ProductRepository` instancia.</span><span class="sxs-lookup"><span data-stu-id="ebe47-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="ebe47-121">Sin embargo, es una mala idea para codificar la dependencia de esta manera, por varias razones.</span><span class="sxs-lookup"><span data-stu-id="ebe47-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="ebe47-122">Si desea reemplazar `ProductRepository` con una implementación diferente, también debe modificar la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="ebe47-123">Si el `ProductRepository` tiene dependencias, debe configurarlos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="ebe47-124">Para un proyecto grande con varios controladores, su código de configuración deja de estar dispersos en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ebe47-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="ebe47-125">Es difícil realizar pruebas unitarias, porque el controlador está codificado de forma rígida para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ebe47-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="ebe47-126">Para una prueba unitaria, debe usar un repositorio ficticios o de rutas internas, lo cual no es posible con el diseño hágala.</span><span class="sxs-lookup"><span data-stu-id="ebe47-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="ebe47-127">Podemos resolver estos problemas por *insertar* el repositorio en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="ebe47-128">En primer lugar, refactorizar el `ProductRepository` clase en una interfaz:</span><span class="sxs-lookup"><span data-stu-id="ebe47-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="ebe47-129">A continuación, proporcione el `IProductRepository` como un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="ebe47-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="ebe47-130">Este ejemplo se utiliza [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="ebe47-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="ebe47-131">También puede usar *inyección de establecedor*, donde se establezca la dependencia a través de un método de establecedor o propiedad.</span><span class="sxs-lookup"><span data-stu-id="ebe47-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="ebe47-132">Pero ahora hay un problema, porque la aplicación no crea directamente en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="ebe47-133">API Web crea el controlador cuando enruta la solicitud y la API Web no sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ebe47-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="ebe47-134">Esto es donde entra en juego la resolución de dependencia de la API Web.</span><span class="sxs-lookup"><span data-stu-id="ebe47-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="ebe47-135">La resolución de dependencia de la API Web</span><span class="sxs-lookup"><span data-stu-id="ebe47-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="ebe47-136">API Web define la **IDependencyResolver** interfaz para resolver las dependencias.</span><span class="sxs-lookup"><span data-stu-id="ebe47-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="ebe47-137">Aquí está la definición de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="ebe47-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="ebe47-138">El **IDependencyScope** interfaz tiene dos métodos:</span><span class="sxs-lookup"><span data-stu-id="ebe47-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="ebe47-139">**GetService** crea una instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="ebe47-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="ebe47-140">**GetServices** crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="ebe47-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="ebe47-141">El **IDependencyResolver** hereda del método **IDependencyScope** y agrega el **BeginScope** método.</span><span class="sxs-lookup"><span data-stu-id="ebe47-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="ebe47-142">Explicaré los ámbitos más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ebe47-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="ebe47-143">Cuando la API Web crea una instancia de controlador, llama primero **IDependencyResolver.GetService**, pasando el tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="ebe47-144">Puede usar este enlace de la extensibilidad para crear el controlador, resolver cualquier dependencia.</span><span class="sxs-lookup"><span data-stu-id="ebe47-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="ebe47-145">Si **GetService** devuelve null, API Web busca un constructor sin parámetros en la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="ebe47-146">Resolución de dependencia con el contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="ebe47-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="ebe47-147">Aunque puede escribir una completa **IDependencyResolver** implementación desde el principio, la interfaz realmente está diseñado para actuar como puente entre las API Web y los contenedores de IoC existentes.</span><span class="sxs-lookup"><span data-stu-id="ebe47-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="ebe47-148">Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="ebe47-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="ebe47-149">Registrar tipos con el contenedor y, a continuación, utilice el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="ebe47-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="ebe47-150">El contenedor imagina automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="ebe47-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="ebe47-151">Muchos de los contenedores de IoC también le permiten controlar aspectos como la duración de los objetos y el ámbito.</span><span class="sxs-lookup"><span data-stu-id="ebe47-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="ebe47-152">"IoC" es el acrónimo "de inversión de control", que es un patrón general donde un marco de trabajo llama al código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ebe47-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="ebe47-153">Un contenedor de IoC construye los objetos para usted, que "invierte" el flujo de control normal.</span><span class="sxs-lookup"><span data-stu-id="ebe47-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="ebe47-154">Para este tutorial, usaremos [Unity](https://msdn.microsoft.com/library/ff647202.aspx) desde Microsoft Patterns &amp; prácticas.</span><span class="sxs-lookup"><span data-stu-id="ebe47-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="ebe47-155">(Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), y [StructureMap ](http://docs.structuremap.net/).) Puede usar el Administrador de paquetes de NuGet para instalar Unity.</span><span class="sxs-lookup"><span data-stu-id="ebe47-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="ebe47-156">Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="ebe47-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ebe47-157">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ebe47-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="ebe47-158">Esta es una implementación de **IDependencyResolver** que encapsula un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="ebe47-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="ebe47-159">Si el **GetService** método no puede resolver un tipo, debe devolver **null**.</span><span class="sxs-lookup"><span data-stu-id="ebe47-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="ebe47-160">Si el **GetServices** método no puede resolver un tipo, debe devolver un objeto de colección vacía.</span><span class="sxs-lookup"><span data-stu-id="ebe47-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="ebe47-161">No se producen excepciones para los tipos desconocidos.</span><span class="sxs-lookup"><span data-stu-id="ebe47-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="ebe47-162">Configurar a la resolución de dependencia</span><span class="sxs-lookup"><span data-stu-id="ebe47-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="ebe47-163">Establecer la resolución de dependencia en el **DependencyResolver** propiedad de la información global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="ebe47-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="ebe47-164">El código siguiente registra el `IProductRepository` interactuar con Unity y, a continuación, se crea un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="ebe47-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="ebe47-165">Ámbito de dependencia y la duración de controlador</span><span class="sxs-lookup"><span data-stu-id="ebe47-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="ebe47-166">Los controladores se crean por solicitud.</span><span class="sxs-lookup"><span data-stu-id="ebe47-166">Controllers are created per request.</span></span> <span data-ttu-id="ebe47-167">Para administrar la duración de los objetos, **IDependencyResolver** usa el concepto de un *ámbito*.</span><span class="sxs-lookup"><span data-stu-id="ebe47-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="ebe47-168">La resolución de dependencia adjunta a la **HttpConfiguration** objeto tiene ámbito global.</span><span class="sxs-lookup"><span data-stu-id="ebe47-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="ebe47-169">Cuando la API Web crea un controlador, se llama a **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="ebe47-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="ebe47-170">Este método devuelve un **IDependencyScope** que representa un ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="ebe47-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="ebe47-171">A continuación, llama a API Web **GetService** en el ámbito secundario para crear el controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="ebe47-172">Cuando se completa la solicitud, las llamadas de API de Web **Dispose** en el ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="ebe47-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="ebe47-173">Use la **Dispose** método deshacerse de las dependencias del controlador.</span><span class="sxs-lookup"><span data-stu-id="ebe47-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="ebe47-174">Cómo implementar **BeginScope** depende del contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="ebe47-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="ebe47-175">Para Unity, ámbito corresponde a un contenedor secundario:</span><span class="sxs-lookup"><span data-stu-id="ebe47-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="ebe47-176">La mayoría de los contenedores de IoC tienen equivalentes similar.</span><span class="sxs-lookup"><span data-stu-id="ebe47-176">Most IoC containers have similar equivalents.</span></span>
