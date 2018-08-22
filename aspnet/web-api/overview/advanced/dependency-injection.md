---
uid: web-api/overview/advanced/dependency-injection
title: Inserción de dependencias en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo insertar dependencias en el controlador de ASP.NET Web API. Versiones de software que se usa en el bloque de aplicaciones de Web API 2 Unity tutorial...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 41db1af79ed63ff4dd12be37e9cc76e16f1bf5e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835400"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="b5efe-104">Inserción de dependencias en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="b5efe-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="b5efe-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b5efe-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b5efe-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="b5efe-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="b5efe-107">Este tutorial muestra cómo insertar dependencias en el controlador de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="b5efe-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5efe-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="b5efe-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b5efe-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="b5efe-109">Web API 2</span></span>
> - [<span data-ttu-id="b5efe-110">Bloque de aplicaciones de Unity</span><span class="sxs-lookup"><span data-stu-id="b5efe-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="b5efe-111">Entity Framework 6 (versión 5 también funciona)</span><span class="sxs-lookup"><span data-stu-id="b5efe-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="b5efe-112">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="b5efe-112">What is Dependency Injection?</span></span>

<span data-ttu-id="b5efe-113">Una *dependencia* es cualquier objeto requerido por otro objeto.</span><span class="sxs-lookup"><span data-stu-id="b5efe-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="b5efe-114">Por ejemplo, es común para definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controla el acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="b5efe-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="b5efe-115">Vamos a mostrar con un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b5efe-115">Let's illustrate with an example.</span></span> <span data-ttu-id="b5efe-116">En primer lugar, definiremos un modelo de dominio:</span><span class="sxs-lookup"><span data-stu-id="b5efe-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b5efe-117">Esta es una clase de repositorio simple que almacena los elementos en una base de datos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b5efe-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="b5efe-118">Ahora vamos a definir un controlador de API Web que admita solicitudes GET para `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="b5efe-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="b5efe-119">(Dejo fuera de la publicación y otros métodos para simplificar las cosas). Este es un primer intento:</span><span class="sxs-lookup"><span data-stu-id="b5efe-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="b5efe-120">Tenga en cuenta que depende de la clase de controlador `ProductRepository`, y vamos a dejar que el controlador de crear el `ProductRepository` instancia.</span><span class="sxs-lookup"><span data-stu-id="b5efe-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="b5efe-121">Sin embargo, es aconsejable codificar de forma rígida la dependencia de este modo, por varias razones.</span><span class="sxs-lookup"><span data-stu-id="b5efe-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="b5efe-122">Si desea reemplazar `ProductRepository` con una implementación diferente, también deberá modificar la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="b5efe-123">Si el `ProductRepository` tiene dependencias, debe configurar estos dentro del controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="b5efe-124">Para un proyecto grande con varios controladores, su código de configuración se convierte en dispersa por todo el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b5efe-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="b5efe-125">Resulta difícil realizar pruebas unitarias, porque el controlador de forma rígida para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b5efe-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="b5efe-126">Para una prueba unitaria, debe usar un repositorio de código auxiliar o de simulacro, que no es posible con el diseño hágala.</span><span class="sxs-lookup"><span data-stu-id="b5efe-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="b5efe-127">Podemos abordar estos problemas por *inyectando* el repositorio en el controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="b5efe-128">En primer lugar, refactorizar la `ProductRepository` clase en una interfaz:</span><span class="sxs-lookup"><span data-stu-id="b5efe-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="b5efe-129">A continuación, proporcione el `IProductRepository` como un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="b5efe-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="b5efe-130">Este ejemplo se utiliza [inserción del constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="b5efe-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="b5efe-131">También puede usar *inserción del establecedor*, donde se establece la dependencia a través de un método establecedor o una propiedad.</span><span class="sxs-lookup"><span data-stu-id="b5efe-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="b5efe-132">Pero ahora hay un problema, ya que la aplicación no crea directamente en el controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="b5efe-133">Web API crea el controlador al que enruta la solicitud y API Web no sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="b5efe-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="b5efe-134">Esto es donde entra en juego la resolución de dependencia de API Web.</span><span class="sxs-lookup"><span data-stu-id="b5efe-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="b5efe-135">La resolución de dependencia de API Web</span><span class="sxs-lookup"><span data-stu-id="b5efe-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="b5efe-136">API Web define la **IDependencyResolver** interfaz para resolver las dependencias.</span><span class="sxs-lookup"><span data-stu-id="b5efe-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="b5efe-137">Esta es la definición de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="b5efe-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="b5efe-138">El **IDependencyScope** interfaz tiene dos métodos:</span><span class="sxs-lookup"><span data-stu-id="b5efe-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="b5efe-139">**GetService** crea una instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="b5efe-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="b5efe-140">**GetServices** crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="b5efe-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="b5efe-141">El **IDependencyResolver** hereda el método **IDependencyScope** y agrega el **BeginScope** método.</span><span class="sxs-lookup"><span data-stu-id="b5efe-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="b5efe-142">Hablaré sobre los ámbitos más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="b5efe-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="b5efe-143">Cuando la API Web crea una instancia de controlador, primero llama **IDependencyResolver.GetService**, pasando el tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="b5efe-144">Puede usar este enlace de extensibilidad para crear el controlador, resolver cualquier dependencia.</span><span class="sxs-lookup"><span data-stu-id="b5efe-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="b5efe-145">Si **GetService** devuelve null, Web API busca un constructor sin parámetros en la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="b5efe-146">Resolución de dependencias con el contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="b5efe-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="b5efe-147">Aunque podría escribir una completa **IDependencyResolver** implementación desde cero, la interfaz está realmente diseñada para actuar como puente entre la API Web y los contenedores de IoC existentes.</span><span class="sxs-lookup"><span data-stu-id="b5efe-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="b5efe-148">Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="b5efe-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="b5efe-149">Registrar tipos con el contenedor y, a continuación, usar el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="b5efe-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="b5efe-150">El contenedor calcula automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="b5efe-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="b5efe-151">Muchos de los contenedores de IoC también permiten controlar aspectos como la duración del objeto y el ámbito.</span><span class="sxs-lookup"><span data-stu-id="b5efe-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="b5efe-152">"IoC" es el acrónimo "inversión de control", que es un patrón general que llama un marco de trabajo en el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5efe-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="b5efe-153">Un contenedor de IoC construye los objetos, que "invierte" el flujo de control normal.</span><span class="sxs-lookup"><span data-stu-id="b5efe-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="b5efe-154">Para este tutorial, vamos a usar [Unity](https://msdn.microsoft.com/library/ff647202.aspx) desde Microsoft Patterns &amp; prácticas.</span><span class="sxs-lookup"><span data-stu-id="b5efe-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="b5efe-155">(Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), y [StructureMap ](http://docs.structuremap.net/).) Puede usar el Administrador de paquetes de NuGet para instalar Unity.</span><span class="sxs-lookup"><span data-stu-id="b5efe-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="b5efe-156">Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b5efe-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b5efe-157">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b5efe-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="b5efe-158">Esta es una implementación de **IDependencyResolver** que ajusta un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b5efe-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="b5efe-159">Si el **GetService** método no puede resolver un tipo, debe devolver **null**.</span><span class="sxs-lookup"><span data-stu-id="b5efe-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="b5efe-160">Si el **GetServices** método no puede resolver un tipo, debe devolver un objeto de colección vacía.</span><span class="sxs-lookup"><span data-stu-id="b5efe-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="b5efe-161">No producir excepciones para los tipos desconocidos.</span><span class="sxs-lookup"><span data-stu-id="b5efe-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="b5efe-162">Configurar a la resolución de dependencia</span><span class="sxs-lookup"><span data-stu-id="b5efe-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="b5efe-163">Establecer la resolución de dependencia en el **DependencyResolver** propiedad de la información global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="b5efe-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="b5efe-164">El código siguiente registra el `IProductRepository` interfaz con Unity y, a continuación, se crea un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="b5efe-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="b5efe-165">Ámbito de dependencia y duración de controlador</span><span class="sxs-lookup"><span data-stu-id="b5efe-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="b5efe-166">Los controladores se crean por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5efe-166">Controllers are created per request.</span></span> <span data-ttu-id="b5efe-167">Para administrar la vigencia del objeto, **IDependencyResolver** usa el concepto de un *ámbito*.</span><span class="sxs-lookup"><span data-stu-id="b5efe-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="b5efe-168">La resolución de dependencia adjunta a la **HttpConfiguration** objeto tiene ámbito global.</span><span class="sxs-lookup"><span data-stu-id="b5efe-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="b5efe-169">Cuando la API Web crea un controlador, llama a **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="b5efe-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="b5efe-170">Este método devuelve un **IDependencyScope** que representa un ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="b5efe-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="b5efe-171">A continuación, llama la API Web **GetService** en el ámbito secundario para crear el controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="b5efe-172">Cuando se completa la solicitud, llama la API Web **Dispose** en el ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="b5efe-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="b5efe-173">Use la **Dispose** método deshacerse de las dependencias del controlador.</span><span class="sxs-lookup"><span data-stu-id="b5efe-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="b5efe-174">Cómo implementar **BeginScope** depende del contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="b5efe-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="b5efe-175">Para Unity, ámbito corresponde a un contenedor secundario:</span><span class="sxs-lookup"><span data-stu-id="b5efe-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="b5efe-176">La mayoría de los contenedores de IoC tienen equivalentes similar.</span><span class="sxs-lookup"><span data-stu-id="b5efe-176">Most IoC containers have similar equivalents.</span></span>
