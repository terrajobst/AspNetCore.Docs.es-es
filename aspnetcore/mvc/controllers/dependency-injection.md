---
title: Inserción de dependencias en controladores en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo los controladores de ASP.NET Core MVC solicitan sus dependencias explícitamente a través de sus constructores por medio de la inserción de dependencias en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: c3e26d294d51dc7044158b05c1ac39015c494610
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="ff11b-103">Inserción de dependencias en controladores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff11b-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="ff11b-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ff11b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ff11b-105">Los controladores de ASP.NET Core MVC deben solicitar sus dependencias explícitamente a través de sus constructores.</span><span class="sxs-lookup"><span data-stu-id="ff11b-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="ff11b-106">En algunos casos, puede haber acciones específicas de controlador que requieran un servicio y quizá no tenga sentido realizar la solicitud en el nivel de controlador.</span><span class="sxs-lookup"><span data-stu-id="ff11b-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="ff11b-107">En este caso, también puede insertar un servicio como un parámetro del método de acción.</span><span class="sxs-lookup"><span data-stu-id="ff11b-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="ff11b-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ff11b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ff11b-109">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="ff11b-109">Dependency Injection</span></span>

<span data-ttu-id="ff11b-110">La inserción de dependencias es una técnica que sigue el [principio de inversión de dependencias](http://deviq.com/dependency-inversion-principle/), lo que permite que las aplicaciones consten de módulos de acoplamiento flexible.</span><span class="sxs-lookup"><span data-stu-id="ff11b-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="ff11b-111">ASP.NET Core tiene compatibilidad integrada para [inserción de dependencias](../../fundamentals/dependency-injection.md), lo que facilita las tareas de prueba y mantenimiento de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ff11b-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="ff11b-112">Inserción de constructores</span><span class="sxs-lookup"><span data-stu-id="ff11b-112">Constructor Injection</span></span>

<span data-ttu-id="ff11b-113">La compatibilidad integrada de ASP.NET Core con la inserción de dependencias basada en constructores se extiende a los controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="ff11b-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="ff11b-114">Simplemente con agregar un tipo de servicio al controlador como un parámetro de constructor, ASP.NET Core intentará resolver ese tipo mediante su contenedor de servicios integrado.</span><span class="sxs-lookup"><span data-stu-id="ff11b-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="ff11b-115">Normalmente, los servicios se definen mediante interfaces, aunque no siempre es así.</span><span class="sxs-lookup"><span data-stu-id="ff11b-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="ff11b-116">Por ejemplo, si la aplicación tiene lógica de negocios que depende de la hora actual, también se puede insertar un servicio que recupera la hora (en lugar de codificarla de forma rígida), lo que permitiría superar las pruebas en implementaciones que usan una hora determinada.</span><span class="sxs-lookup"><span data-stu-id="ff11b-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="ff11b-117">Implementar una interfaz como esta para que utilice el reloj del sistema en tiempo de ejecución no es nada complicado:</span><span class="sxs-lookup"><span data-stu-id="ff11b-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="ff11b-118">Teniendo esto implementado, podemos utilizar el servicio en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ff11b-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="ff11b-119">En este caso, hemos agregado lógica al método `Index` de `HomeController` para presentar un saludo al usuario en función de la hora del día.</span><span class="sxs-lookup"><span data-stu-id="ff11b-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="ff11b-120">Si se ejecuta la aplicación ahora, probablemente se producirá un error:</span><span class="sxs-lookup"><span data-stu-id="ff11b-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="ff11b-121">Este error se produce cuando no se ha configurado un servicio en el método `ConfigureServices` de nuestra clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ff11b-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="ff11b-122">Para especificar que las solicitudes de `IDateTime` deben resolverse mediante una instancia de `SystemDateTime`, agregue la línea resaltada en la siguiente lista a su método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ff11b-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="ff11b-123">Este servicio en particular podría implementarse mediante cualquiera de las diversas opciones de duración (`Transient`, `Scoped` o `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="ff11b-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="ff11b-124">Consulte [Dependency Injection](../../fundamentals/dependency-injection.md) (Inserción de dependencias) para ver cómo afectará al comportamiento de su servicio cada una de estas opciones de ámbito.</span><span class="sxs-lookup"><span data-stu-id="ff11b-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="ff11b-125">Una vez que se ha configurado el servicio, al ejecutar la aplicación y navegar a la página principal se debería mostrar el mensaje basado en la hora según lo esperado:</span><span class="sxs-lookup"><span data-stu-id="ff11b-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Saludo del servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="ff11b-127">Vea [Testing controller logic](testing.md) (Comprobación de la lógica de controlador) para obtener información sobre cómo solicitar explícitamente dependencias [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) en controladores para facilitar la comprobación de código.</span><span class="sxs-lookup"><span data-stu-id="ff11b-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="ff11b-128">La inserción de dependencias integrada de ASP.NET Core es compatible con tener un solo constructor para las clases que soliciten servicios.</span><span class="sxs-lookup"><span data-stu-id="ff11b-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="ff11b-129">Si se tiene más de un constructor, es posible recibir la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="ff11b-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="ff11b-130">Tal como indica el mensaje de error, tener un solo constructor corregiría el problema.</span><span class="sxs-lookup"><span data-stu-id="ff11b-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="ff11b-131">También se puede [reemplazar la compatibilidad de inserción de dependencias predeterminada por una implementación de otros fabricantes](../../fundamentals/dependency-injection.md#replacing-the-default-services-container) que sea compatible con varios constructores.</span><span class="sxs-lookup"><span data-stu-id="ff11b-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="ff11b-132">Inserción de acción con FromServices</span><span class="sxs-lookup"><span data-stu-id="ff11b-132">Action Injection with FromServices</span></span>

<span data-ttu-id="ff11b-133">A veces, un servicio solo es necesario para una acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ff11b-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="ff11b-134">En este caso, puede tener sentido insertar el servicio como un parámetro en el método de acción.</span><span class="sxs-lookup"><span data-stu-id="ff11b-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="ff11b-135">Para ello, se marca el parámetro con el atributo `[FromServices]`, tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ff11b-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="ff11b-136">Acceso a la configuración desde un controlador</span><span class="sxs-lookup"><span data-stu-id="ff11b-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="ff11b-137">El acceso a la configuración de la aplicación o a los valores de configuración desde un controlador es un patrón habitual.</span><span class="sxs-lookup"><span data-stu-id="ff11b-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="ff11b-138">Este acceso debe utilizar el patrón de opciones que se describe en el artículo sobre [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ff11b-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="ff11b-139">Por lo general, no es recomendable solicitar la configuración directamente desde el controlador mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="ff11b-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="ff11b-140">Un enfoque más adecuado consiste en solicitar una instancia de `IOptions<T>`, donde `T` es la clase de configuración que se necesita.</span><span class="sxs-lookup"><span data-stu-id="ff11b-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="ff11b-141">Para trabajar con el patrón de opciones, debe crear una clase como la siguiente que represente las opciones:</span><span class="sxs-lookup"><span data-stu-id="ff11b-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="ff11b-142">A continuación, necesita configurar la aplicación para que use el modelo de opciones y agregar la clase de configuración a la colección de servicios en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ff11b-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="ff11b-143">En la lista anterior, se configura la aplicación para que lea la configuración de un archivo con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="ff11b-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="ff11b-144">La configuración también se puede definir completamente en código, como se muestra en el código comentado anterior.</span><span class="sxs-lookup"><span data-stu-id="ff11b-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="ff11b-145">Consulte [Configuración](xref:fundamentals/configuration/index) para conocer más opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="ff11b-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="ff11b-146">Una vez que haya especificado un objeto de configuración fuertemente tipado (en este caso, `SampleWebSettings`) y lo haya agregado a la colección de servicios, podrá solicitarlo desde cualquier método de acción o controlador mediante la solicitud de una instancia de `IOptions<T>` (en este caso, `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="ff11b-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="ff11b-147">El código siguiente muestra cómo se podría solicitar la configuración desde un controlador:</span><span class="sxs-lookup"><span data-stu-id="ff11b-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="ff11b-148">Seguir el patrón de opciones permite desacoplar entre sí los valores y la configuración, y garantiza que el controlador respete la [separación de intereses](http://deviq.com/separation-of-concerns/), ya que no necesita saber cómo ni dónde encontrar la información de configuración.</span><span class="sxs-lookup"><span data-stu-id="ff11b-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="ff11b-149">También hace que sea más fácil realizar una prueba unitaria de la [lógica del controlador](testing.md), puesto que no hay efecto [static cling](http://deviq.com/static-cling/) ni creación directa de instancias de clases de configuración dentro de la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="ff11b-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
