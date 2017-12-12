---
title: "Inyección de dependencia en controladores"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 46b92a1cab6fb2cd06eff44feb6a55788fca5c2a
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="de3e4-103">Inyección de dependencia en controladores</span><span class="sxs-lookup"><span data-stu-id="de3e4-103">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="de3e4-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="de3e4-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="de3e4-105">Controladores MVC de ASP.NET Core deben solicitar sus dependencias explícitamente a través de sus constructores.</span><span class="sxs-lookup"><span data-stu-id="de3e4-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="de3e4-106">En algunos casos, las acciones de controlador individuales pueden requerir un servicio y puede que no tenga sentido para solicitar en el nivel de controlador.</span><span class="sxs-lookup"><span data-stu-id="de3e4-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="de3e4-107">En este caso, también puede insertar un servicio como un parámetro del método de acción.</span><span class="sxs-lookup"><span data-stu-id="de3e4-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="de3e4-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de3e4-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="de3e4-109">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="de3e4-109">Dependency Injection</span></span>

<span data-ttu-id="de3e4-110">Inserción de dependencias es una técnica que sigue a la [principio de inversión de dependencia](http://deviq.com/dependency-inversion-principle/), lo cual permite aplicaciones componerse de módulos de acoplamiento flexible.</span><span class="sxs-lookup"><span data-stu-id="de3e4-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="de3e4-111">ASP.NET Core tiene compatibilidad integrada para [inyección de dependencia](../../fundamentals/dependency-injection.md), lo que facilita las aplicaciones probar y mantener.</span><span class="sxs-lookup"><span data-stu-id="de3e4-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="de3e4-112">Inyección de constructor</span><span class="sxs-lookup"><span data-stu-id="de3e4-112">Constructor Injection</span></span>

<span data-ttu-id="de3e4-113">Compatibilidad de ASP.NET Core para la inyección de dependencia basado en constructores se extiende a controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="de3e4-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="de3e4-114">Al agregar simplemente un tipo de servicio para el controlador como un parámetro de constructor, ASP.NET Core intentará resolver ese tipo mediante su integración en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="de3e4-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="de3e4-115">Servicios son normalmente, pero no siempre, definir mediante interfaces.</span><span class="sxs-lookup"><span data-stu-id="de3e4-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="de3e4-116">Por ejemplo, si la aplicación tiene lógica de negocios que depende de la hora actual, también puede insertar un servicio que recupera el tiempo (en lugar de codificar de forma rígida), lo que permitiría las pruebas pasar de las implementaciones que usan una hora determinada.</span><span class="sxs-lookup"><span data-stu-id="de3e4-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="de3e4-117">Implementar una interfaz como este para que utilice el reloj del sistema en tiempo de ejecución es trivial:</span><span class="sxs-lookup"><span data-stu-id="de3e4-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="de3e4-118">Teniendo esto en su lugar, podemos utilizar el servicio en el controlador.</span><span class="sxs-lookup"><span data-stu-id="de3e4-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="de3e4-119">En este caso, hemos agregado lógica para la `HomeController` `Index` método para presentar un saludo al usuario en función de la hora del día.</span><span class="sxs-lookup"><span data-stu-id="de3e4-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="de3e4-120">Si se ejecuta la aplicación ahora, probablemente se producirá un error:</span><span class="sxs-lookup"><span data-stu-id="de3e4-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="de3e4-121">Este error se produce cuando no se ha configurado un servicio en la `ConfigureServices` método en nuestro `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="de3e4-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="de3e4-122">Para especificar que las solicitudes para `IDateTime` debe resolverse mediante una instancia de `SystemDateTime`, agregue la línea resaltada en la siguiente lista para su `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="de3e4-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="de3e4-123">Este servicio en particular podría implementarse mediante cualquiera de las diversas opciones de duración distinta (`Transient`, `Scoped`, o `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="de3e4-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="de3e4-124">Vea [inyección de dependencia](../../fundamentals/dependency-injection.md) para comprender cómo cada una de estas opciones de ámbito afectará el comportamiento del servicio.</span><span class="sxs-lookup"><span data-stu-id="de3e4-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="de3e4-125">Una vez que se ha configurado el servicio, ejecutar la aplicación y navegar a la página principal deberían mostrar el mensaje de tiempo según lo esperado:</span><span class="sxs-lookup"><span data-stu-id="de3e4-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Saludo del servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="de3e4-127">Vea [lógica del controlador de pruebas](testing.md) para obtener información sobre cómo solicitar explícitamente dependencias [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) en los controladores de facilita código de prueba.</span><span class="sxs-lookup"><span data-stu-id="de3e4-127">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="de3e4-128">Inyección de dependencia integrados de ASP.NET Core admite solo un constructor único para las clases que soliciten servicios.</span><span class="sxs-lookup"><span data-stu-id="de3e4-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="de3e4-129">Si tiene más de un constructor, es posible recibir una excepción que indica:</span><span class="sxs-lookup"><span data-stu-id="de3e4-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="de3e4-130">Como indica el mensaje de error, puede corregir este problema tiene solo un constructor único.</span><span class="sxs-lookup"><span data-stu-id="de3e4-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="de3e4-131">También puede [reemplazar la compatibilidad de inyección de dependencia predeterminada con una implementación de otros fabricantes](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), muchas de que admite varios constructores.</span><span class="sxs-lookup"><span data-stu-id="de3e4-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="de3e4-132">Inyección de acción con FromServices</span><span class="sxs-lookup"><span data-stu-id="de3e4-132">Action Injection with FromServices</span></span>

<span data-ttu-id="de3e4-133">A veces, no es necesario un servicio para más de una acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="de3e4-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="de3e4-134">En este caso, puede tener sentido insertar el servicio como un parámetro al método de acción.</span><span class="sxs-lookup"><span data-stu-id="de3e4-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="de3e4-135">Esto se debe marcar el parámetro con el atributo `[FromServices]` como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="de3e4-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="de3e4-136">Acceso a la configuración de un controlador</span><span class="sxs-lookup"><span data-stu-id="de3e4-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="de3e4-137">Acceso a la configuración de aplicación o la configuración de un controlador es un patrón común.</span><span class="sxs-lookup"><span data-stu-id="de3e4-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="de3e4-138">Este acceso debe utilizar el patrón de opciones se describen en [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="de3e4-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="de3e4-139">Por lo general debe no solicitar configuración directamente desde el controlador mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="de3e4-139">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="de3e4-140">Un enfoque más adecuado consiste en solicitud una `IOptions<T>` instancia, donde `T` es la clase de configuración que necesite.</span><span class="sxs-lookup"><span data-stu-id="de3e4-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="de3e4-141">Para trabajar con el patrón de opciones, debe crear una clase que representa las opciones, como esta:</span><span class="sxs-lookup"><span data-stu-id="de3e4-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="de3e4-142">Es necesario que configure la aplicación para utilizar el modelo de opciones y agregar la clase de configuración a la colección de servicios en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="de3e4-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="de3e4-143">En la lista anterior, nos estamos configurar la aplicación para leer la configuración de un archivo con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="de3e4-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="de3e4-144">También puede configurar la configuración completamente en código, como se muestra en el código comentado anterior.</span><span class="sxs-lookup"><span data-stu-id="de3e4-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="de3e4-145">Vea [configuración](xref:fundamentals/configuration/index) para obtener más opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="de3e4-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="de3e4-146">Una vez que haya especificado un objeto de configuración fuertemente tipada (en este caso, `SampleWebSettings`) y agregarlo a la colección de servicios, se puede solicitar desde cualquier método de acción o controlador que solicita una instancia de `IOptions<T>` (en este caso, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="de3e4-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="de3e4-147">El código siguiente muestra cómo uno podría solicitar la configuración de un controlador:</span><span class="sxs-lookup"><span data-stu-id="de3e4-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="de3e4-148">Seguir el patrón de opciones permite y la configuración se desacople entre sí y garantiza que el controlador es posterior [separación de intereses](http://deviq.com/separation-of-concerns/), ya que no es necesario saber cómo o dónde encontrar la configuración información.</span><span class="sxs-lookup"><span data-stu-id="de3e4-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="de3e4-149">También facilita el controlador de la prueba unitaria [lógica del controlador de pruebas](testing.md), ya que no hay ningún [estáticos adhesivos para sus](http://deviq.com/static-cling/) o creación directa de instancias de clases de configuración dentro de la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="de3e4-149">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
