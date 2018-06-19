---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserción de dependencias de MVC de ASP.NET 4 | Documentos de Microsoft
author: rick-anderson
description: 'Nota: Este laboratorio práctico sobre supone que tiene conocimientos básicos de los filtros ASP.NET MVC y ASP.NET MVC 4. Si no ha utilizado filtros de ASP.NET MVC 4 antes de, rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877663"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="b9269-104">Inserción de dependencias de MVC de ASP.NET 4</span><span class="sxs-lookup"><span data-stu-id="b9269-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="b9269-105">Por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b9269-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b9269-106">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="b9269-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b9269-107">Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC** y **filtros de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="b9269-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="b9269-108">Si no ha usado **filtros de ASP.NET MVC 4** antes, le recomendamos que repase **filtros de acción de ASP.NET MVC personalizado** laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="b9269-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-109">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b9269-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b9269-110">Está disponible en el proyecto específico para este laboratorio [inyección de dependencia de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="b9269-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="b9269-111">En **la programación orientada a objetos de objeto** paradigma, objetos trabajan juntos en un modelo de colaboración donde hay colaboradores y los consumidores.</span><span class="sxs-lookup"><span data-stu-id="b9269-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="b9269-112">Naturalmente, este modelo de comunicación genera las dependencias entre los objetos y componentes, pase a ser difíciles de administrar cuando aumenta la complejidad.</span><span class="sxs-lookup"><span data-stu-id="b9269-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="b9269-113">![Clase dependencias y la complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "clase dependencias y la complejidad del modelo")</span><span class="sxs-lookup"><span data-stu-id="b9269-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="b9269-114">*Dependencias de la clase y la complejidad de modelo*</span><span class="sxs-lookup"><span data-stu-id="b9269-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="b9269-115">Probablemente habrá oído hablar sobre la **modelo de generador** y la separación entre la interfaz y la implementación mediante los servicios, donde los objetos de cliente a menudo son responsables de la ubicación del servicio.</span><span class="sxs-lookup"><span data-stu-id="b9269-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="b9269-116">El modelo de inserción de dependencias es una implementación concreta de inversión de Control.</span><span class="sxs-lookup"><span data-stu-id="b9269-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="b9269-117">**Inversión de Control (IoC)** significa que los objetos no crear otros objetos en el que se basan para hacer su trabajo.</span><span class="sxs-lookup"><span data-stu-id="b9269-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="b9269-118">En su lugar, obtienen los objetos que necesitan de un origen externo (por ejemplo, un archivo de configuración xml).</span><span class="sxs-lookup"><span data-stu-id="b9269-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="b9269-119">**Inyección de dependencia (DI)** significa que esto se realiza sin la intervención del objeto, normalmente por un componente de marco de trabajo que pasa los parámetros de constructor y establecer las propiedades.</span><span class="sxs-lookup"><span data-stu-id="b9269-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="b9269-120">El modelo de diseño de dependencia (DI) por inyección de código</span><span class="sxs-lookup"><span data-stu-id="b9269-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="b9269-121">En un nivel alto, el objetivo de inserción de dependencias es que una clase de cliente (por ejemplo, *el golfista*) necesita algo que satisface una interfaz (por ejemplo, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="b9269-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="b9269-122">No le importa qué es un tipo concreto (p. ej. *WoodClub, IronClub, WedgeClub* o *PutterClub*), que alguien pueda controlar que desee (por ejemplo, un buen *portadora*).</span><span class="sxs-lookup"><span data-stu-id="b9269-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="b9269-123">La resolución de dependencia en ASP.NET MVC le permiten registrar su lógica de dependencia en otro lugar (por ejemplo, un contenedor o un *bolsa de tréboles*).</span><span class="sxs-lookup"><span data-stu-id="b9269-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="b9269-124">![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustración de inyección de dependencia")</span><span class="sxs-lookup"><span data-stu-id="b9269-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="b9269-125">*Inserción de dependencias - analogía de Golf*</span><span class="sxs-lookup"><span data-stu-id="b9269-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="b9269-126">Las ventajas de utilizar el modelo de inserción de dependencias y la inversión de Control son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="b9269-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="b9269-127">Reduce el acoplamiento de clase</span><span class="sxs-lookup"><span data-stu-id="b9269-127">Reduces class coupling</span></span>
- <span data-ttu-id="b9269-128">Aumenta la reutilización de código</span><span class="sxs-lookup"><span data-stu-id="b9269-128">Increases code reusing</span></span>
- <span data-ttu-id="b9269-129">Mejora el mantenimiento del código</span><span class="sxs-lookup"><span data-stu-id="b9269-129">Improves code maintainability</span></span>
- <span data-ttu-id="b9269-130">Mejora de pruebas de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9269-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-131">Inyección de dependencia a veces se compara con el patrón de diseño Factory abstracta, pero hay una ligera diferencia entre ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="b9269-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="b9269-132">DI tiene un marco de trabajo detrás para resolver las dependencias mediante una llamada a los generadores y los servicios registrados.</span><span class="sxs-lookup"><span data-stu-id="b9269-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="b9269-133">Ahora que comprende el patrón de inyección de dependencia, aprenderá a lo largo de este laboratorio que se aplicará en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b9269-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="b9269-134">Se iniciará mediante la inserción de dependencia en el **controladores** para incluir un servicio de acceso de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b9269-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="b9269-135">A continuación, se aplicará la inyección de dependencia para el **vistas** para consumir un servicio y mostrar información.</span><span class="sxs-lookup"><span data-stu-id="b9269-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="b9269-136">Por último, se ampliará la DI a los filtros de ASP.NET MVC 4, inserte un filtro de acción personalizada en la solución.</span><span class="sxs-lookup"><span data-stu-id="b9269-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="b9269-137">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="b9269-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="b9269-138">Integración de ASP.NET MVC 4 con Unity para la inyección de dependencia con los paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="b9269-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="b9269-139">Usar la inserción de dependencias dentro de un controlador de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9269-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="b9269-140">Usar la inserción de dependencias dentro de una vista de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9269-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="b9269-141">Usar la inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9269-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-142">Esta práctica es usar Unity.Mvc3 paquete de NuGet para la resolución de dependencia, pero es posible adaptar cualquier marco de inyección de dependencia para que funcione con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b9269-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b9269-143">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b9269-143">Prerequisites</span></span>

<span data-ttu-id="b9269-144">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="b9269-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b9269-145">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="b9269-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b9269-146">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="b9269-146">Setup</span></span>

<span data-ttu-id="b9269-147">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="b9269-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="b9269-148">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9269-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b9269-149">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="b9269-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b9269-150">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9269-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b9269-151">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="b9269-151">Exercises</span></span>

<span data-ttu-id="b9269-152">Este laboratorio práctico se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="b9269-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b9269-153">Ejercicio 1: Insertar un controlador</span><span class="sxs-lookup"><span data-stu-id="b9269-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="b9269-154">Ejercicio 2: Inserte una vista</span><span class="sxs-lookup"><span data-stu-id="b9269-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="b9269-155">Ejercicio 3: Insertar filtros</span><span class="sxs-lookup"><span data-stu-id="b9269-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b9269-156">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="b9269-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b9269-157">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="b9269-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b9269-158">Tiempo estimado para completar esta práctica: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="b9269-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="b9269-159">Ejercicio 1: Insertar un controlador</span><span class="sxs-lookup"><span data-stu-id="b9269-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="b9269-160">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en los controladores de MVC de ASP.NET mediante la integración de Unity mediante un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9269-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b9269-161">Por esta razón, se van a incluir servicios en los controladores de MvcMusicStore para separar la lógica desde el acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="b9269-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="b9269-162">Los servicios creará una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la Ayuda de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b9269-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="b9269-163">Este enfoque le mostrará cómo generar menos aplicaciones acopladas, que son más flexibles y fáciles de mantener y probar.</span><span class="sxs-lookup"><span data-stu-id="b9269-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="b9269-164">También obtendrá información sobre cómo integrar ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="b9269-165">Acerca del servicio de StoreManager</span><span class="sxs-lookup"><span data-stu-id="b9269-165">About StoreManager Service</span></span>

<span data-ttu-id="b9269-166">La tienda de música de MVC proporciona ahora en la solución de inicio incluye un servicio que administra los datos de almacén controlador denominados **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b9269-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="b9269-167">A continuación encontrará la implementación del servicio de almacén.</span><span class="sxs-lookup"><span data-stu-id="b9269-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="b9269-168">Tenga en cuenta que todos los métodos devuelven las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="b9269-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b9269-169">**StoreController** desde el inicio solución consume ahora **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b9269-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="b9269-170">Se quitaron todas las referencias de datos **StoreController**y ahora es posible modificar el proveedor de acceso de datos actual sin cambiar cualquier método que consume **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="b9269-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="b9269-171">Encontrará a continuación que el **StoreController** implementación tiene una dependencia con **StoreService** dentro del constructor de clase.</span><span class="sxs-lookup"><span data-stu-id="b9269-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-172">Está relacionado con la dependencia que se introdujo en este ejercicio **inversión de Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="b9269-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="b9269-173">El **StoreController** constructor de clase recibe un **IStoreService** parámetro de tipo, que es esencial para realizar llamadas de servicio desde dentro de la clase.</span><span class="sxs-lookup"><span data-stu-id="b9269-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="b9269-174">Sin embargo, **StoreController** no implementa el constructor predeterminado (sin ningún parámetro) que debe tener un controlador para que funcione con ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b9269-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="b9269-175">Para resolver la dependencia, el controlador debe crearse mediante un generador abstracto (una clase que devuelve cualquier objeto del tipo especificado).</span><span class="sxs-lookup"><span data-stu-id="b9269-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="b9269-176">Obtendrá un error cuando la clase intenta crear la StoreController sin necesidad de enviar el objeto de servicio, porque no hay ningún constructor sin parámetros declarado.</span><span class="sxs-lookup"><span data-stu-id="b9269-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="b9269-177">Tarea 1: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9269-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="b9269-178">En esta tarea, se ejecutará la aplicación de inicio, que incluye el servicio en el controlador de almacén que separa el acceso a los datos de la lógica de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9269-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="b9269-179">Cuando se ejecuta la aplicación, recibirá una excepción, como el servicio del controlador no se pasa como un parámetro de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="b9269-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="b9269-180">Abra la **comenzar** soluciones se encuentran en **Controller\Begin insertar Source\Ex01**.</span><span class="sxs-lookup"><span data-stu-id="b9269-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="b9269-181">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b9269-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9269-182">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9269-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9269-183">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="b9269-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9269-184">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="b9269-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9269-185">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9269-186">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="b9269-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9269-187">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="b9269-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b9269-188">Presione **Ctrl + F5** para ejecutar la aplicación sin depuración.</span><span class="sxs-lookup"><span data-stu-id="b9269-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="b9269-189">Se le devolverá el mensaje de error &quot; **ningún constructor sin parámetros definido para este objeto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b9269-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="b9269-190">![Error al ejecutar la aplicación de ASP.NET MVC comenzar](aspnet-mvc-4-dependency-injection/_static/image3.png "Error al ejecutar la aplicación de ASP.NET MVC comenzar")</span><span class="sxs-lookup"><span data-stu-id="b9269-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="b9269-191">*Error al ejecutar la aplicación de ASP.NET MVC comenzar*</span><span class="sxs-lookup"><span data-stu-id="b9269-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="b9269-192">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="b9269-192">Close the browser.</span></span>

<span data-ttu-id="b9269-193">En los pasos siguientes funcionará en la solución de tienda de música para insertar la dependencia de que este controlador es necesario.</span><span class="sxs-lookup"><span data-stu-id="b9269-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="b9269-194">Tarea 2 - Unity incluidos en la solución MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="b9269-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="b9269-195">En esta tarea, se incluirá la **Unity.Mvc3** paquete NuGet para la solución.</span><span class="sxs-lookup"><span data-stu-id="b9269-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-196">Paquete de Unity.Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b9269-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="b9269-197">Unity es un contenedor de inyección de dependencia ligeras y extensibles con compatibilidad opcional para la instancia y escriba interceptación.</span><span class="sxs-lookup"><span data-stu-id="b9269-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="b9269-198">Es un contenedor de uso general para su uso en cualquier tipo de aplicación. NET.</span><span class="sxs-lookup"><span data-stu-id="b9269-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="b9269-199">Proporciona todas las características comunes de mecanismos de inyección de dependencia incluidos: creación de objetos, la abstracción de requisitos mediante la especificación de dependencias en tiempo de ejecución y la flexibilidad, ya que aplazan la configuración del componente para el contenedor.</span><span class="sxs-lookup"><span data-stu-id="b9269-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="b9269-200">Instalar **Unity.Mvc3** paquete de NuGet en la **MvcMusicStore** proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="b9269-201">Para ello, abra el **Package Manager Console** de **vista** | **otras ventanas**.</span><span class="sxs-lookup"><span data-stu-id="b9269-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="b9269-202">Ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="b9269-202">Run the following command.</span></span>

    <span data-ttu-id="b9269-203">PMC</span><span class="sxs-lookup"><span data-stu-id="b9269-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="b9269-204">![Instalando paquete NuGet de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalar paquete de NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="b9269-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="b9269-205">*Instalando paquete NuGet de Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="b9269-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="b9269-206">Una vez el **Unity.Mvc3** está instalado el paquete, explorar los archivos y carpetas que agrega automáticamente con el fin de simplificar la configuración de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="b9269-207">![Instalado el paquete de Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "paquete Unity.Mvc3 instalado")</span><span class="sxs-lookup"><span data-stu-id="b9269-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="b9269-208">*Paquete de Unity.Mvc3 instalado*</span><span class="sxs-lookup"><span data-stu-id="b9269-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="b9269-209">Tarea 3: registro de Unity en aplicación Global.asax.cs\_iniciar</span><span class="sxs-lookup"><span data-stu-id="b9269-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="b9269-210">En esta tarea, actualizará la **aplicación\_iniciar** método ubicado en **Global.asax.cs** para llamar el inicializador de Unity del programa previo y, a continuación, actualizar el registro de archivo de programa previo el servicio y el controlador que se va a usar para la inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="b9269-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="b9269-211">Ahora, enlazará el programa previo que es el archivo que inicializa el contenedor de Unity y resolución de dependencia.</span><span class="sxs-lookup"><span data-stu-id="b9269-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="b9269-212">Para ello, abra **Global.asax.cs** y agregue el código que aparece resaltado en el **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="b9269-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="b9269-213">(Código de fragmento de código: *laboratorio de inyección de dependencia ASP.NET - Ex01 - inicializar Unity*)</span><span class="sxs-lookup"><span data-stu-id="b9269-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="b9269-214">Abra **Bootstrapper.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="b9269-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="b9269-215">Incluya los espacios de nombres siguientes: **MvcMusicStore.Services** y **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="b9269-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="b9269-216">(Código de fragmento de código: *agregar espacios de nombres ASP.NET dependencia inyección laboratorio - Ex01 - arranque*)</span><span class="sxs-lookup"><span data-stu-id="b9269-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="b9269-217">Reemplace **BuildUnityContainer** método del contenido con el código siguiente a la que se registra el controlador de almacén y el servicio de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="b9269-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="b9269-218">(Código de fragmento de código: *servicio y controlador de almacén de registro ASP.NET dependencia inyección laboratorio - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b9269-219">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9269-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="b9269-220">En esta tarea, se ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="b9269-221">Presione **F5** para ejecutar la aplicación, la aplicación debe cargar ahora sin mostrar ningún mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b9269-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="b9269-222">![Ejecutar la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "ejecutando la aplicación con la inserción de dependencias")</span><span class="sxs-lookup"><span data-stu-id="b9269-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="b9269-223">*Aplicación en ejecución con la inserción de dependencias*</span><span class="sxs-lookup"><span data-stu-id="b9269-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="b9269-224">Vaya a **/almacén**.</span><span class="sxs-lookup"><span data-stu-id="b9269-224">Browse to **/Store**.</span></span> <span data-ttu-id="b9269-225">Esto invocará **StoreController**, que se ha creado con **Unity**.</span><span class="sxs-lookup"><span data-stu-id="b9269-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="b9269-226">![Tienda de música de MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "tienda de música de MVC")</span><span class="sxs-lookup"><span data-stu-id="b9269-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="b9269-227">*Tienda de música de MVC*</span><span class="sxs-lookup"><span data-stu-id="b9269-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="b9269-228">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="b9269-228">Close the browser.</span></span>

<span data-ttu-id="b9269-229">En los siguientes ejercicios obtendrá información sobre cómo ampliar el ámbito de inyección de dependencia para usarlo dentro de las vistas de MVC de ASP.NET y los filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="b9269-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="b9269-230">Ejercicio 2: Inserte una vista</span><span class="sxs-lookup"><span data-stu-id="b9269-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="b9269-231">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias de una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="b9269-232">Para ello, se llamará a un servicio personalizado dentro de la vista de examinar de almacén, que mostrará un mensaje y una imagen que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="b9269-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="b9269-233">A continuación, se integran el proyecto con Unity y crear una resolución de dependencia personalizado para insertar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="b9269-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="b9269-234">Tarea 1: crear una vista que consume un servicio</span><span class="sxs-lookup"><span data-stu-id="b9269-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="b9269-235">En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia.</span><span class="sxs-lookup"><span data-stu-id="b9269-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="b9269-236">El servicio está compuesto de un servicio de mensajería simple incluido en esta solución.</span><span class="sxs-lookup"><span data-stu-id="b9269-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="b9269-237">Abra el **comenzar** soluciones se encuentran en la **View\Begin inyectar Source\Ex02** carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9269-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="b9269-238">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="b9269-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9269-239">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b9269-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9269-240">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9269-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9269-241">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="b9269-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9269-242">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="b9269-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9269-243">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9269-244">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="b9269-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9269-245">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="b9269-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b9269-246">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b9269-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b9269-247">Incluir el **MessageService.cs** y la **IMessageService.cs** clases se encuentran en el **origen \Assets** carpeta en **/servicios**.</span><span class="sxs-lookup"><span data-stu-id="b9269-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="b9269-248">Para ello, haga clic en **servicios** carpeta y seleccione **Agregar elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="b9269-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="b9269-249">Vaya a la ubicación de los archivos y los incluye.</span><span class="sxs-lookup"><span data-stu-id="b9269-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="b9269-250">![Agregar servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Agregar servicio de mensajes y la interfaz de servicio")</span><span class="sxs-lookup"><span data-stu-id="b9269-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="b9269-251">*Agregar servicio de mensajes y la interfaz de servicio*</span><span class="sxs-lookup"><span data-stu-id="b9269-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9269-252">El **IMessageService** interfaz define dos propiedades implementadas por el **MessageService** clase.</span><span class="sxs-lookup"><span data-stu-id="b9269-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="b9269-253">Estas propiedades -**mensaje** y **ImageUrl**-almacenar el mensaje y la dirección URL de la imagen que se muestre.</span><span class="sxs-lookup"><span data-stu-id="b9269-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="b9269-254">Cree la carpeta **/páginas** en el proyecto de la carpeta raíz y, a continuación, agregue la clase existente **MyBasePage.cs** de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="b9269-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="b9269-255">La página base de que heredará tiene la siguiente estructura.</span><span class="sxs-lookup"><span data-stu-id="b9269-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="b9269-256">![Carpeta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "carpeta de páginas")</span><span class="sxs-lookup"><span data-stu-id="b9269-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="b9269-257">Abra **Browse.cshtml** ver desde **/vistas/almacén** carpeta y hacer que se herede de **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9269-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="b9269-258">En el **examinar** ver, agregar una llamada a **MessageService** para mostrar una imagen y un mensaje recuperado por el servicio.</span><span class="sxs-lookup"><span data-stu-id="b9269-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="b9269-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="b9269-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="b9269-260">Tarea 2: incluyendo una resolución de dependencia personalizados y un activador de página de vista personalizada</span><span class="sxs-lookup"><span data-stu-id="b9269-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="b9269-261">En la tarea anterior, se insertado una dependencia nuevo dentro de una vista para realizar una llamada de servicio dentro de él.</span><span class="sxs-lookup"><span data-stu-id="b9269-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="b9269-262">Ahora, se resolverá esa dependencia implementando las interfaces de inserción de dependencias de MVC de ASP.NET **IViewPageActivator** y **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b9269-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="b9269-263">Se incluirá en la solución de una implementación de **IDependencyResolver** que abordará la recuperación de servicio mediante el uso de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="b9269-264">A continuación, se van a incluir otra implementación personalizada de **IViewPageActivator** interfaz que resolverá la creación de las vistas.</span><span class="sxs-lookup"><span data-stu-id="b9269-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="b9269-265">Desde ASP.NET MVC 3, la implementación para la inyección de dependencia tenía simplificado las interfaces para registrar los servicios.</span><span class="sxs-lookup"><span data-stu-id="b9269-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="b9269-266">**Objeto IDependencyResolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="b9269-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="b9269-267">**-IDependencyResolver** interfaz reemplaza el IMvcServiceLocator anterior.</span><span class="sxs-lookup"><span data-stu-id="b9269-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="b9269-268">Los implementadores de IDependencyResolver deben devolver una instancia del servicio o una colección de servicio.</span><span class="sxs-lookup"><span data-stu-id="b9269-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="b9269-269">**-IViewPageActivator** interfaz proporciona un mayor control sobre cómo se crean instancias de páginas de vista a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b9269-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="b9269-270">Las clases que implementan **IViewPageActivator** interfaz puede crear instancias de vista utilizando la información de contexto.</span><span class="sxs-lookup"><span data-stu-id="b9269-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="b9269-271">Crear la /**generadores** carpeta en la carpeta raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="b9269-272">Incluir **CustomViewPageActivator.cs** a la solución de **/orígenes/activos/** a **generadores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9269-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="b9269-273">Para ello, haga clic en el **/Factories** carpeta, seleccione **agregar | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9269-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="b9269-274">Esta clase implementa la **IViewPageActivator** interfaz para albergar el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9269-275">**CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="b9269-276">Incluir **UnityDependencyResolver.cs** archivo **/orígenes/activos** a **/Factories** carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9269-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="b9269-277">Para ello, haga clic en el **/Factories** carpeta, seleccione **agregar | Elemento existente** y, a continuación, seleccione **UnityDependencyResolver.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="b9269-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9269-278">**UnityDependencyResolver** clase es un DependencyResolver personalizado para Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="b9269-279">Cuando un servicio no se encuentra dentro del contenedor de Unity, se invocated la resolución de base.</span><span class="sxs-lookup"><span data-stu-id="b9269-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="b9269-280">En la siguiente tarea ambas implementaciones se registrará para permitir que el modelo se conoce la ubicación de los servicios y las vistas.</span><span class="sxs-lookup"><span data-stu-id="b9269-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="b9269-281">Tarea 3: registrar para la inyección de dependencia en el contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="b9269-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="b9269-282">En esta tarea, se coloca todo lo anterior para que la inserción de dependencias funcione.</span><span class="sxs-lookup"><span data-stu-id="b9269-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="b9269-283">Hasta ahora la solución consta de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="b9269-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="b9269-284">A **examinar** vista que hereda de **MyBaseClass** y consume **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="b9269-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="b9269-285">Una clase intermedia -**MyBaseClass**-que se declara para la interfaz de servicio de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b9269-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="b9269-286">Un servicio - **MessageService** - y su interfaz **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="b9269-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="b9269-287">Una resolución de dependencia personalizados para Unity - **UnityDependencyResolver** -que abordan la recuperación de servicio.</span><span class="sxs-lookup"><span data-stu-id="b9269-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="b9269-288">Un activador de página de vista - **CustomViewPageActivator** -que crea la página.</span><span class="sxs-lookup"><span data-stu-id="b9269-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="b9269-289">Insertar **examinar** vista, ahora se registrará la resolución de dependencia personalizados en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="b9269-290">Abra **Bootstrapper.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="b9269-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="b9269-291">Registrar una instancia de **MessageService** en el contenedor de Unity para inicializar el servicio:</span><span class="sxs-lookup"><span data-stu-id="b9269-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="b9269-292">(Código de fragmento de código: *servicio de mensajes de registro ASP.NET dependencia por inyección de código laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="b9269-293">Agregue una referencia a **MvcMusicStore.Factories** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="b9269-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="b9269-294">(Código de fragmento de código: *Namespace de generadores ASP.NET dependencia inyección laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="b9269-295">Registrar **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:</span><span class="sxs-lookup"><span data-stu-id="b9269-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="b9269-296">(Código de fragmento de código: *CustomViewPageActivator de registro ASP.NET dependencia inyección laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="b9269-297">Reemplace la resolución de dependencia predeterminada de ASP.NET MVC 4 con una instancia de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b9269-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="b9269-298">Para ello, reemplace **Initialise** método contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9269-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="b9269-299">(Código de fragmento de código: *resolución de dependencias de actualización ASP.NET dependencia inyección laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9269-300">ASP.NET MVC proporciona una clase de resolución de dependencia de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b9269-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="b9269-301">Para trabajar con resoluciones de dependencia personalizados que hemos creado para unity, esta resolución debe reemplazarse.</span><span class="sxs-lookup"><span data-stu-id="b9269-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b9269-302">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9269-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="b9269-303">En esta tarea, ejecutará la aplicación para comprobar que el Explorador de almacenamiento consume el servicio y se muestra la imagen y el mensaje recuperado:</span><span class="sxs-lookup"><span data-stu-id="b9269-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="b9269-304">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9269-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b9269-305">Haga clic en **Rock** en el menú de géneros y vea cómo el **MessageService** insertado a la vista y cargar el mensaje de bienvenida y la imagen.</span><span class="sxs-lookup"><span data-stu-id="b9269-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="b9269-306">En este ejemplo, estamos escriba con &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="b9269-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="b9269-307">![Tienda de música de MVC - vista inyección](aspnet-mvc-4-dependency-injection/_static/image10.png "tienda de música de MVC - inyección de vista")</span><span class="sxs-lookup"><span data-stu-id="b9269-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="b9269-308">*Tienda de música de MVC - inyección de vista*</span><span class="sxs-lookup"><span data-stu-id="b9269-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="b9269-309">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="b9269-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="b9269-310">Ejercicio 3: Insertar filtros de acción</span><span class="sxs-lookup"><span data-stu-id="b9269-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="b9269-311">En la práctica anterior **filtros de acción personalizados** ha trabajado con inserción y personalización de filtros.</span><span class="sxs-lookup"><span data-stu-id="b9269-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="b9269-312">En este ejercicio, obtendrá información sobre cómo insertar filtros con inyección de dependencia mediante el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="b9269-313">Para ello, se agregará a la solución de la tienda de música un filtro de acción personalizada que se realizará un seguimiento la actividad del sitio.</span><span class="sxs-lookup"><span data-stu-id="b9269-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="b9269-314">Tarea 1: incluido el filtro de seguimiento de la solución</span><span class="sxs-lookup"><span data-stu-id="b9269-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="b9269-315">En esta tarea, se incluirá en la tienda de música un filtro de acción personalizado para eventos de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="b9269-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="b9269-316">Como filtro de acción personalizado ya se tratan conceptos en la práctica anterior &quot;filtros de acción personalizado&quot;, que solo incluya la clase de filtro de la carpeta de activos de este laboratorio y, a continuación, crear un proveedor de filtro para Unity:</span><span class="sxs-lookup"><span data-stu-id="b9269-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="b9269-317">Abra la **comenzar** soluciones se encuentran en la **Source\Ex03 - Filter\Begin de acción de insertar** carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9269-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="b9269-318">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="b9269-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b9269-319">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b9269-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b9269-320">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9269-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b9269-321">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="b9269-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b9269-322">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="b9269-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9269-323">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b9269-324">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="b9269-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b9269-325">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="b9269-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b9269-326">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b9269-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b9269-327">Incluir **TraceActionFilter.cs** archivo **/orígenes/activos** a **/filtros** carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9269-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9269-328">Este filtro de acción personalizado realiza el seguimiento de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9269-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="b9269-329">Puede comprobar &quot;local de ASP.NET MVC 4 y filtros de acción dinámicos&quot; laboratorio como referencia más.</span><span class="sxs-lookup"><span data-stu-id="b9269-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="b9269-330">Agregue la clase vacía **FilterProvider.cs** al proyecto en la carpeta   **/filtros.**</span><span class="sxs-lookup"><span data-stu-id="b9269-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="b9269-331">Agregar el **System.Web.Mvc** y **Microsoft.Practices.Unity** espacios de nombres en **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9269-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="b9269-332">(Código de fragmento de código: *ASP.NET dependencia inyección laboratorio - Ex03 - el proveedor de filtros agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="b9269-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="b9269-333">Haga que la clase herede de **IFilterProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="b9269-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="b9269-334">Agregar un **IUnityContainer** propiedad en el **FilterProvider** clase y, a continuación, cree un constructor de clase para asignar el contenedor.</span><span class="sxs-lookup"><span data-stu-id="b9269-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="b9269-335">(Código de fragmento de código: *Constructor de proveedor de filtro ASP.NET dependencia inyección laboratorio - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="b9269-336">El constructor de clase de proveedor de filtro no está creando un **nuevo** dentro del objeto.</span><span class="sxs-lookup"><span data-stu-id="b9269-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="b9269-337">El contenedor se pasa como un parámetro y se resuelve la dependencia de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="b9269-338">En el **FilterProvider** clase, implemente el método **GetFilters** de **IFilterProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="b9269-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="b9269-339">(Código de fragmento de código: *GetFilters de proveedor de filtro ASP.NET dependencia inyección laboratorio - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="b9269-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="b9269-340">Tarea 2: registrar y habilitar el filtro</span><span class="sxs-lookup"><span data-stu-id="b9269-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="b9269-341">En esta tarea, se habilita el seguimiento de sitio.</span><span class="sxs-lookup"><span data-stu-id="b9269-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="b9269-342">Para ello, se registrará el filtro en **Bootstrapper.cs BuildUnityContainer** método para iniciar la traza:</span><span class="sxs-lookup"><span data-stu-id="b9269-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="b9269-343">Abra **Web.config** ubicado en el directorio raíz del proyecto y habilitar el seguimiento de traza en el grupo de System.Web.</span><span class="sxs-lookup"><span data-stu-id="b9269-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="b9269-344">Abra **Bootstrapper.cs** en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9269-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="b9269-345">Agregue una referencia a la **MvcMusicStore.Filters** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="b9269-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="b9269-346">(Código de fragmento de código: *agregar espacios de nombres ASP.NET dependencia inyección laboratorio - Ex03 - arranque*)</span><span class="sxs-lookup"><span data-stu-id="b9269-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="b9269-347">Seleccione el **BuildUnityContainer** método y registrar el filtro en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="b9269-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="b9269-348">Tendrá que registrar el proveedor de filtros, así como el filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="b9269-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="b9269-349">(Código de fragmento de código: *ASP.NET dependencia inyección laboratorio - Ex03 - registro FilterProvider y ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="b9269-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b9269-350">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9269-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="b9269-351">En esta tarea, ejecutará la aplicación y que el filtro de acción personalizada traza la actividad de prueba:</span><span class="sxs-lookup"><span data-stu-id="b9269-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="b9269-352">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9269-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b9269-353">Haga clic en **Rock** en el menú de géneros.</span><span class="sxs-lookup"><span data-stu-id="b9269-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="b9269-354">Puede examinar más géneros si desea.</span><span class="sxs-lookup"><span data-stu-id="b9269-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="b9269-355">![Tienda de música](aspnet-mvc-4-dependency-injection/_static/image11.png "tienda de música")</span><span class="sxs-lookup"><span data-stu-id="b9269-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="b9269-356">*Tienda de música*</span><span class="sxs-lookup"><span data-stu-id="b9269-356">*Music Store*</span></span>
3. <span data-ttu-id="b9269-357">Vaya a **/Trace.axd** para ver el seguimiento de la aplicación de página y, a continuación, haga clic en **ver detalles**.</span><span class="sxs-lookup"><span data-stu-id="b9269-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="b9269-358">![Registro de seguimiento de la aplicación](aspnet-mvc-4-dependency-injection/_static/image12.png "registro de seguimiento de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="b9269-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="b9269-359">*Registro de seguimiento de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="b9269-359">*Application Trace Log*</span></span>

    <span data-ttu-id="b9269-360">![Seguimiento de la aplicación - detalles de la solicitud](aspnet-mvc-4-dependency-injection/_static/image13.png "aplicación Trace - detalles de la solicitud")</span><span class="sxs-lookup"><span data-stu-id="b9269-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="b9269-361">*Seguimiento de la aplicación - detalles de la solicitud*</span><span class="sxs-lookup"><span data-stu-id="b9269-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="b9269-362">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="b9269-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b9269-363">Resumen</span><span class="sxs-lookup"><span data-stu-id="b9269-363">Summary</span></span>

<span data-ttu-id="b9269-364">Al completar este laboratorio práctico ha aprendido cómo utilizar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity mediante un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9269-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="b9269-365">Para lograrlo, ha utilizado la inyección de dependencia dentro de los controladores, vistas y filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="b9269-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="b9269-366">Se tratan los conceptos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b9269-366">The following concepts were covered:</span></span>

- <span data-ttu-id="b9269-367">Características de inserción de dependencias de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b9269-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="b9269-368">Integración de Unity con paquete de NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="b9269-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="b9269-369">Inserción de dependencias en los controladores</span><span class="sxs-lookup"><span data-stu-id="b9269-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="b9269-370">Inserción de dependencias en las vistas</span><span class="sxs-lookup"><span data-stu-id="b9269-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="b9269-371">Inyección de dependencia de los filtros de acción</span><span class="sxs-lookup"><span data-stu-id="b9269-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b9269-372">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="b9269-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b9269-373">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b9269-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b9269-374">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b9269-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b9269-375">Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b9269-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b9269-376">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9269-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b9269-377">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="b9269-377">Click on **Install Now**.</span></span> <span data-ttu-id="b9269-378">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="b9269-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b9269-379">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="b9269-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b9269-380">![Instale Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b9269-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b9269-381">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b9269-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b9269-382">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="b9269-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="b9269-384">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="b9269-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b9269-385">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="b9269-385">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="b9269-387">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="b9269-387">*Installation progress*</span></span>
6. <span data-ttu-id="b9269-388">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="b9269-388">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="b9269-390">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="b9269-390">*Installation completed*</span></span>
7. <span data-ttu-id="b9269-391">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="b9269-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b9269-392">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="b9269-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="b9269-394">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="b9269-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="b9269-395">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="b9269-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="b9269-396">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="b9269-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b9269-397">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="b9269-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b9269-398">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="b9269-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b9269-399">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="b9269-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b9269-400">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="b9269-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b9269-401">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="b9269-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b9269-402">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="b9269-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b9269-403">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="b9269-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b9269-404">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="b9269-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b9269-405">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="b9269-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b9269-406">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="b9269-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b9269-407">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="b9269-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="b9269-408">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-dependency-injection/_static/image21.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="b9269-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b9269-409">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="b9269-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b9269-410">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-dependency-injection/_static/image22.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="b9269-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b9269-411">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="b9269-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b9269-412">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b9269-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b9269-413">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="b9269-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b9269-414">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="b9269-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b9269-415">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="b9269-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b9269-416">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-dependency-injection/_static/image23.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="b9269-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b9269-417">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="b9269-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b9269-418">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-dependency-injection/_static/image24.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="b9269-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b9269-419">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="b9269-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
