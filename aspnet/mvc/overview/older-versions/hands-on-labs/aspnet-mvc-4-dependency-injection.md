---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserción de dependencias de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: Esta práctica se supone que tiene conocimientos básicos de los filtros de MVC de ASP.NET y ASP.NET MVC 4. Si no ha usado los filtros de ASP.NET MVC 4 antes, se rec...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 44c8f2055fb62d589e874683cbf43eed87a8c447
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812349"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="07b3a-104">Inserción de dependencias de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="07b3a-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="07b3a-105">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="07b3a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="07b3a-106">Descargue el Kit de aprendizaje de campamentos de Web</span><span class="sxs-lookup"><span data-stu-id="07b3a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="07b3a-107">Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC** y **filtros de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="07b3a-108">Si no ha usado **filtros de ASP.NET MVC 4** antes, le recomendamos que repase **filtros de acción personalizada de ASP.NET MVC** laboratorios prácticos.</span><span class="sxs-lookup"><span data-stu-id="07b3a-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-109">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web campamentos, disponible desde en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="07b3a-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="07b3a-110">Está disponible en el proyecto específico para este laboratorio [inserción de dependencias de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="07b3a-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="07b3a-111">En **programación orientada a objetos** paradigma, objetos trabajan juntos en un modelo de colaboración donde hay colaboradores y los consumidores.</span><span class="sxs-lookup"><span data-stu-id="07b3a-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="07b3a-112">Naturalmente, este modelo de comunicación genera las dependencias entre objetos y componentes, cada vez son difíciles de administrar cuando aumenta la complejidad.</span><span class="sxs-lookup"><span data-stu-id="07b3a-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="07b3a-113">![Clase dependencias y la complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "clase dependencias y la complejidad del modelo")</span><span class="sxs-lookup"><span data-stu-id="07b3a-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="07b3a-114">*Dependencias de clase y la complejidad de modelo*</span><span class="sxs-lookup"><span data-stu-id="07b3a-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="07b3a-115">Probablemente habrá oído hablar acerca de la **patrón Factory** y la separación entre la interfaz y la implementación mediante los servicios, donde los objetos de cliente a menudo son responsables de la ubicación del servicio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="07b3a-116">El patrón de inserción de dependencias es una implementación determinada de inversión de Control.</span><span class="sxs-lookup"><span data-stu-id="07b3a-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="07b3a-117">**Inversión de Control (IoC)** significa que los objetos no crear otros objetos que dependen para hacer su trabajo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="07b3a-118">Obtiene los objetos que necesitan desde un origen externo (por ejemplo, un archivo de configuración xml).</span><span class="sxs-lookup"><span data-stu-id="07b3a-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="07b3a-119">**Inserción de dependencias (DI)** significa que esto se realiza sin la intervención del objeto, normalmente por un componente de marco de trabajo que pasa los parámetros del constructor y establecer las propiedades.</span><span class="sxs-lookup"><span data-stu-id="07b3a-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="07b3a-120">El patrón de diseño de dependencia (DI) de la inyección de código</span><span class="sxs-lookup"><span data-stu-id="07b3a-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="07b3a-121">En un nivel alto, el objetivo de inserción de dependencias es que una clase de cliente (por ejemplo, *el jugador*) necesita algo que satisface una interfaz (por ejemplo, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="07b3a-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="07b3a-122">No le importa cuál es el tipo concreto (por ejemplo, *WedgeClub WoodClub, IronClub,* o *PutterClub*), que otra persona para controlar que desee (por ejemplo, un buen *portadora*).</span><span class="sxs-lookup"><span data-stu-id="07b3a-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="07b3a-123">La resolución de dependencias en ASP.NET MVC puede permitirle registrar la lógica de dependencia en otro lugar (por ejemplo, un contenedor o un *bolsa de tréboles*).</span><span class="sxs-lookup"><span data-stu-id="07b3a-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="07b3a-124">![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustración de inserción de dependencias")</span><span class="sxs-lookup"><span data-stu-id="07b3a-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="07b3a-125">*Inserción de dependencias - analogía de Golf*</span><span class="sxs-lookup"><span data-stu-id="07b3a-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="07b3a-126">Las ventajas de usar el patrón de inserción de dependencias y la inversión de Control son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="07b3a-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="07b3a-127">Reduce el acoplamiento de clases</span><span class="sxs-lookup"><span data-stu-id="07b3a-127">Reduces class coupling</span></span>
- <span data-ttu-id="07b3a-128">Aumenta la reutilización de código</span><span class="sxs-lookup"><span data-stu-id="07b3a-128">Increases code reusing</span></span>
- <span data-ttu-id="07b3a-129">Mejora el mantenimiento del código</span><span class="sxs-lookup"><span data-stu-id="07b3a-129">Improves code maintainability</span></span>
- <span data-ttu-id="07b3a-130">Mejora de las pruebas de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="07b3a-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-131">Inserción de dependencias a veces se compara con el patrón de diseño Abstract Factory, pero hay una pequeña diferencia entre ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="07b3a-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="07b3a-132">Inserción de dependencias tiene un marco de trabajo subyacente para resolver las dependencias mediante una llamada a los generadores y los servicios registrados.</span><span class="sxs-lookup"><span data-stu-id="07b3a-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="07b3a-133">Ahora que comprende el patrón de inserción de dependencia, aprenderá a lo largo de este laboratorio aplicarlo en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07b3a-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="07b3a-134">Se iniciará mediante la inserción de dependencia en el **controladores** para incluir un servicio de acceso de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="07b3a-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="07b3a-135">A continuación, se aplicará la inserción de dependencias para el **vistas** para consumir un servicio y mostrar información.</span><span class="sxs-lookup"><span data-stu-id="07b3a-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="07b3a-136">Por último, extenderá la inserción de dependencias a los filtros de ASP.NET MVC 4, inyección de un filtro de acción personalizada en la solución.</span><span class="sxs-lookup"><span data-stu-id="07b3a-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="07b3a-137">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="07b3a-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="07b3a-138">Integración de ASP.NET MVC 4 con Unity para la inserción de dependencias con paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="07b3a-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="07b3a-139">Usar inserción de dependencias dentro de un controlador ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="07b3a-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="07b3a-140">Usar inserción de dependencias dentro de una vista de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07b3a-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="07b3a-141">Usar inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07b3a-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-142">Este laboratorio usa Unity.Mvc3 paquete de NuGet para la resolución de dependencia, pero es posible adaptar cualquier marco de inserción de dependencia para que funcione con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07b3a-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="07b3a-143">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="07b3a-143">Prerequisites</span></span>

<span data-ttu-id="07b3a-144">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="07b3a-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="07b3a-145">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="07b3a-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="07b3a-146">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="07b3a-146">Setup</span></span>

<span data-ttu-id="07b3a-147">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="07b3a-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="07b3a-148">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="07b3a-149">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="07b3a-150">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="07b3a-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="07b3a-151">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="07b3a-151">Exercises</span></span>

<span data-ttu-id="07b3a-152">Esta práctica se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="07b3a-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="07b3a-153">Ejercicio 1: Insertar un controlador</span><span class="sxs-lookup"><span data-stu-id="07b3a-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="07b3a-154">Ejercicio 2: Inserción de una vista</span><span class="sxs-lookup"><span data-stu-id="07b3a-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="07b3a-155">Ejercicio 3: Inserción de filtros</span><span class="sxs-lookup"><span data-stu-id="07b3a-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="07b3a-156">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="07b3a-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="07b3a-157">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="07b3a-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="07b3a-158">Tiempo estimado para completar esta práctica: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="07b3a-159">Ejercicio 1: Insertar un controlador</span><span class="sxs-lookup"><span data-stu-id="07b3a-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="07b3a-160">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en controladores de MVC de ASP.NET mediante la integración de Unity mediante un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="07b3a-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="07b3a-161">Por ese motivo, se van a incluir servicios en los controladores MvcMusicStore para separar la lógica de acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="07b3a-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="07b3a-162">Los servicios creación una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la Ayuda de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="07b3a-163">Este enfoque le mostrará cómo generar menos aplicaciones acopladas, que son más flexibles y fáciles de mantener y probar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="07b3a-164">También aprenderá a integrar ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="07b3a-165">Acerca del servicio StoreManager</span><span class="sxs-lookup"><span data-stu-id="07b3a-165">About StoreManager Service</span></span>

<span data-ttu-id="07b3a-166">El Store de música de MVC proporciona ahora en la solución de inicio incluye un servicio que administra los datos de Store controlador denominados **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="07b3a-167">A continuación encontrará la implementación del servicio Store.</span><span class="sxs-lookup"><span data-stu-id="07b3a-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="07b3a-168">Tenga en cuenta que todos los métodos devuelven las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="07b3a-169">**StoreController** desde begin solución ahora consume **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="07b3a-170">Se quitaron todas las referencias de datos **StoreController**y ahora es posible modificar el proveedor de acceso de datos actual sin cambiar cualquier método que consume **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="07b3a-171">A continuación, encontrará el **StoreController** implementación tiene una dependencia con **StoreService** dentro del constructor de clase.</span><span class="sxs-lookup"><span data-stu-id="07b3a-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-172">Está relacionado con la dependencia que se introdujo en este ejercicio **inversión de Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="07b3a-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="07b3a-173">El **StoreController** constructor de clase recibe un **IStoreService** parámetro de tipo, lo cual es esencial para realizar llamadas a servicios desde dentro de la clase.</span><span class="sxs-lookup"><span data-stu-id="07b3a-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="07b3a-174">Sin embargo, **StoreController** no implementa el constructor predeterminado (sin parámetros) que cualquier controlador debe tener para que funcione con ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="07b3a-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="07b3a-175">Para resolver la dependencia, el controlador tiene que va a crear una fábrica abstracta (una clase que devuelve cualquier objeto del tipo especificado).</span><span class="sxs-lookup"><span data-stu-id="07b3a-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="07b3a-176">Se producirá un error cuando la clase intenta crear el StoreController sin necesidad de enviar el objeto de servicio, ya que no hay ningún constructor sin parámetros declarado.</span><span class="sxs-lookup"><span data-stu-id="07b3a-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="07b3a-177">Tarea 1: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="07b3a-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="07b3a-178">En esta tarea, ejecutará la aplicación de inicio, que incluye el servicio en el controlador de Store que separa el acceso a datos de la lógica de aplicación.</span><span class="sxs-lookup"><span data-stu-id="07b3a-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="07b3a-179">Al ejecutar la aplicación, recibirá una excepción, como el servicio de controlador no se pasa como un parámetro de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="07b3a-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="07b3a-180">Abra el **comenzar** solución se encuentra en **Controller\Begin inyección de Source\Ex01**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="07b3a-181">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07b3a-182">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="07b3a-183">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="07b3a-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="07b3a-184">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="07b3a-185">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07b3a-186">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07b3a-187">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="07b3a-188">Presione **Ctrl + F5** para ejecutar la aplicación sin depuración.</span><span class="sxs-lookup"><span data-stu-id="07b3a-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="07b3a-189">Obtendrá el mensaje de error &quot; **ningún constructor sin parámetros definido para este objeto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="07b3a-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="07b3a-190">![Error al ejecutar la aplicación de ASP.NET MVC comenzar](aspnet-mvc-4-dependency-injection/_static/image3.png "Error mientras se ejecuta la aplicación de ASP.NET MVC comenzar")</span><span class="sxs-lookup"><span data-stu-id="07b3a-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="07b3a-191">*Error al ejecutar la aplicación de ASP.NET MVC comenzar*</span><span class="sxs-lookup"><span data-stu-id="07b3a-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="07b3a-192">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="07b3a-192">Close the browser.</span></span>

<span data-ttu-id="07b3a-193">En los pasos siguientes funcionará en la solución de Music Store para insertar la dependencia que tiene este controlador.</span><span class="sxs-lookup"><span data-stu-id="07b3a-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="07b3a-194">Tarea 2 - Unity incluidos en la solución MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="07b3a-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="07b3a-195">En esta tarea, se van a incluir **Unity.Mvc3** paquete NuGet para la solución.</span><span class="sxs-lookup"><span data-stu-id="07b3a-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-196">Paquete Unity.Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07b3a-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="07b3a-197">Unity es un contenedor de inserción de dependencias ligero y extensible con compatibilidad opcional por ejemplo y la intercepción de tipos.</span><span class="sxs-lookup"><span data-stu-id="07b3a-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="07b3a-198">Es un contenedor de uso general para su uso en cualquier tipo de aplicación. NET.</span><span class="sxs-lookup"><span data-stu-id="07b3a-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="07b3a-199">Proporciona todas las características comunes de mecanismos de inyección de dependencia como: creación de objetos, la abstracción de los requisitos mediante la especificación de dependencias en tiempo de ejecución y la flexibilidad, mediante el aplazamiento de la configuración del componente al contenedor.</span><span class="sxs-lookup"><span data-stu-id="07b3a-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="07b3a-200">Instalar **Unity.Mvc3** paquete NuGet en el **MvcMusicStore** proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="07b3a-201">Para ello, abra el **Package Manager Console** desde **vista** | **Other Windows**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="07b3a-202">Ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="07b3a-202">Run the following command.</span></span>

    <span data-ttu-id="07b3a-203">PMC</span><span class="sxs-lookup"><span data-stu-id="07b3a-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="07b3a-204">![Instalar el paquete de NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalar el paquete de NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="07b3a-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="07b3a-205">*Instalar el paquete de NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="07b3a-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="07b3a-206">Una vez el **Unity.Mvc3** está instalado el paquete, explorar los archivos y carpetas se agrega automáticamente con el fin de simplificar la configuración de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="07b3a-207">![Instalado el paquete Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "paquete Unity.Mvc3 instalado")</span><span class="sxs-lookup"><span data-stu-id="07b3a-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="07b3a-208">*Paquete Unity.Mvc3 instalado*</span><span class="sxs-lookup"><span data-stu-id="07b3a-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="07b3a-209">Tarea 3: registro de Unity en la aplicación de Global.asax.cs\_iniciar</span><span class="sxs-lookup"><span data-stu-id="07b3a-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="07b3a-210">En esta tarea, actualizará la **aplicación\_iniciar** método ubicado en **Global.asax.cs** llamar el inicializador de programa previo de Unity y, a continuación, actualizar el registro de archivo de programa previo el servicio y el controlador que usará para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="07b3a-211">Ahora, se enlazará el programa previo que es el archivo que se inicializa el contenedor de Unity y resolución de dependencia.</span><span class="sxs-lookup"><span data-stu-id="07b3a-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="07b3a-212">Para ello, abra **Global.asax.cs** y agregue el código resaltado siguiente dentro de la **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="07b3a-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="07b3a-213">(Código de fragmento de código - *laboratorio de inyección de dependencia ASP.NET - Ex01 - inicializar Unity*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="07b3a-214">Abra **Bootstrapper.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="07b3a-215">Incluir los espacios de nombres siguientes: **MvcMusicStore.Services** y **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="07b3a-216">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex01 - programa previo agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="07b3a-217">Reemplace **BuildUnityContainer** del contenido con el siguiente código que registra el controlador de Store y el servicio Store método.</span><span class="sxs-lookup"><span data-stu-id="07b3a-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="07b3a-218">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex01 - Store Register controlador y el servicio*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="07b3a-219">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="07b3a-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="07b3a-220">En esta tarea, ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="07b3a-221">Presione **F5** para ejecutar la aplicación, la aplicación debe cargar ahora sin mostrar ningún mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="07b3a-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="07b3a-222">![Ejecutar la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "ejecutando la aplicación con la inserción de dependencias")</span><span class="sxs-lookup"><span data-stu-id="07b3a-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="07b3a-223">*Aplicación en ejecución con inserción de dependencias*</span><span class="sxs-lookup"><span data-stu-id="07b3a-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="07b3a-224">Vaya a **/Store**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-224">Browse to **/Store**.</span></span> <span data-ttu-id="07b3a-225">Esto invocará **StoreController**, que ya se ha creado mediante **Unity**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="07b3a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "Store música de MVC")</span><span class="sxs-lookup"><span data-stu-id="07b3a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="07b3a-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="07b3a-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="07b3a-228">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="07b3a-228">Close the browser.</span></span>

<span data-ttu-id="07b3a-229">En los ejercicios siguientes obtendrá información sobre cómo ampliar el ámbito de la inserción de dependencias para usarlo dentro de las vistas de MVC de ASP.NET y los filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="07b3a-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="07b3a-230">Ejercicio 2: Inserción de una vista</span><span class="sxs-lookup"><span data-stu-id="07b3a-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="07b3a-231">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="07b3a-232">Para ello, llamará a un servicio personalizado dentro de la vista de examinar Store, que mostrará un mensaje y una imagen de abajo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="07b3a-233">A continuación, se integran el proyecto con Unity y crear una resolución de dependencia personalizadas para insertar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="07b3a-234">Tarea 1: crear una vista que consume un servicio</span><span class="sxs-lookup"><span data-stu-id="07b3a-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="07b3a-235">En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia.</span><span class="sxs-lookup"><span data-stu-id="07b3a-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="07b3a-236">El servicio se compone de un servicio de mensajería simple incluido en esta solución.</span><span class="sxs-lookup"><span data-stu-id="07b3a-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="07b3a-237">Abra el **comenzar** solución se encuentra en la **inyección de Source\Ex02 View\Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="07b3a-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="07b3a-238">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="07b3a-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="07b3a-239">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07b3a-240">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="07b3a-241">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="07b3a-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="07b3a-242">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="07b3a-243">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07b3a-244">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07b3a-245">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="07b3a-246">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="07b3a-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="07b3a-247">Incluyen la **MessageService.cs** y el **IMessageService.cs** clases se encuentran en el **origen \Assets** carpeta en **/servicios**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="07b3a-248">Para ello, haga clic en **servicios** carpeta y seleccione **Agregar elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="07b3a-249">Vaya a la ubicación de los archivos y los incluye.</span><span class="sxs-lookup"><span data-stu-id="07b3a-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="07b3a-250">![Agregar servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Agregar servicio de mensajes y la interfaz de servicio")</span><span class="sxs-lookup"><span data-stu-id="07b3a-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="07b3a-251">*Agregar servicio de mensajes y la interfaz de servicio*</span><span class="sxs-lookup"><span data-stu-id="07b3a-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="07b3a-252">El **IMessageService** interfaz define dos propiedades implementadas por el **mensaje** clase.</span><span class="sxs-lookup"><span data-stu-id="07b3a-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="07b3a-253">Estas propiedades -**mensaje** y **ImageUrl**-almacenar el mensaje y la dirección URL de la imagen que se muestre.</span><span class="sxs-lookup"><span data-stu-id="07b3a-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="07b3a-254">Cree la carpeta **/páginas** en el proyecto de la carpeta raíz y, a continuación, agregue la clase existente **MyBasePage.cs** desde **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="07b3a-255">La página base de que se heredarán tiene la siguiente estructura.</span><span class="sxs-lookup"><span data-stu-id="07b3a-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="07b3a-256">![Carpeta páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "carpeta páginas")</span><span class="sxs-lookup"><span data-stu-id="07b3a-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="07b3a-257">Abra **Browse.cshtml** desde la vista **/Views/Store** carpeta y hacer que heredan de **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="07b3a-258">En el **examinar** ver, agregar una llamada a **mensaje** para mostrar una imagen y un mensaje recuperado por el servicio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="07b3a-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="07b3a-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="07b3a-260">Tarea 2: como una resolución de dependencia personalizada y un activador de página de vista personalizada</span><span class="sxs-lookup"><span data-stu-id="07b3a-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="07b3a-261">En la tarea anterior, se inserta una nueva dependencia dentro de una vista para realizar una llamada de servicio dentro de él.</span><span class="sxs-lookup"><span data-stu-id="07b3a-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="07b3a-262">Ahora, se resolverá esa dependencia implementando las interfaces de inserción de dependencias de MVC de ASP.NET **IViewPageActivator** y **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="07b3a-263">Se van a incluir en la solución de una implementación de **IDependencyResolver** que se ocupará la recuperación del servicio mediante el uso de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="07b3a-264">A continuación, se van a incluir otra implementación personalizada de **IViewPageActivator** interfaz destinadas a resolver la creación de las vistas.</span><span class="sxs-lookup"><span data-stu-id="07b3a-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="07b3a-265">Desde ASP.NET MVC 3, la implementación para la inyección de dependencia tenía simplificado las interfaces para registrar los servicios.</span><span class="sxs-lookup"><span data-stu-id="07b3a-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="07b3a-266">**IDependencyResolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="07b3a-267">**-IDependencyResolver** interfaz reemplaza el IMvcServiceLocator anterior.</span><span class="sxs-lookup"><span data-stu-id="07b3a-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="07b3a-268">Los implementadores de IDependencyResolver deben devolver una instancia del servicio o una colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="07b3a-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="07b3a-269">**-IViewPageActivator** interfaz proporciona un mayor control sobre cómo se crean instancias de las páginas de vista a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="07b3a-270">Las clases que implementan **IViewPageActivator** interfaz puede crear instancias de vista utilizando la información de contexto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="07b3a-271">Crear el /**generadores** carpeta en la carpeta raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="07b3a-272">Incluir **CustomViewPageActivator.cs** a la solución de **/orígenes/recursos/** a **generadores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="07b3a-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="07b3a-273">Para ello, haga clic en el **/Factories** carpeta, seleccione **Add | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="07b3a-274">Esta clase implementa la **IViewPageActivator** interfaz para albergar el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="07b3a-275">**CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="07b3a-276">Incluir **UnityDependencyResolver.cs** de archivos de **/Sources/activos** a **/Factories** carpeta.</span><span class="sxs-lookup"><span data-stu-id="07b3a-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="07b3a-277">Para ello, haga clic en el **/Factories** carpeta, seleccione **Add | Elemento existente** y, a continuación, seleccione **UnityDependencyResolver.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="07b3a-278">**UnityDependencyResolver** clase es un DependencyResolver personalizado para Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="07b3a-279">Cuando un servicio no se encuentra dentro del contenedor de Unity, la resolución base es invocated.</span><span class="sxs-lookup"><span data-stu-id="07b3a-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="07b3a-280">En la siguiente tarea ambas implementaciones se registrará para permitir que el modelo se conoce la ubicación de los servicios y las vistas.</span><span class="sxs-lookup"><span data-stu-id="07b3a-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="07b3a-281">Tarea 3: registro de inserción de dependencias dentro del contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="07b3a-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="07b3a-282">En esta tarea, se colocará todo lo anterior para realizar la inserción de dependencias funcione.</span><span class="sxs-lookup"><span data-stu-id="07b3a-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="07b3a-283">Hasta ahora, la solución tiene los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="07b3a-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="07b3a-284">Un **examinar** vista que hereda de **MyBaseClass** y consume **mensaje**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="07b3a-285">Una clase intermedia -**MyBaseClass**-que se declara de la interfaz de servicio de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="07b3a-286">Un servicio - **mensaje** - y su interfaz **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="07b3a-287">Una resolución de dependencia personalizadas para Unity: **UnityDependencyResolver** -que se ocupa de recuperación de servicio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="07b3a-288">Un activador de página de vista - **CustomViewPageActivator** -que crea la página.</span><span class="sxs-lookup"><span data-stu-id="07b3a-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="07b3a-289">Insertar **examinar** vista, ahora se registrará la resolución de dependencia personalizadas en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="07b3a-290">Abra **Bootstrapper.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="07b3a-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="07b3a-291">Registrar una instancia de **mensaje** en el contenedor de Unity para inicializar el servicio:</span><span class="sxs-lookup"><span data-stu-id="07b3a-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="07b3a-292">(Código de fragmento de código - *servicio de mensaje de registro de laboratorio - Ex02 - inserción de dependencias ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="07b3a-293">Agregue una referencia a **MvcMusicStore.Factories** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="07b3a-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="07b3a-294">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex02 - generadores Namespace*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="07b3a-295">Registrar **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:</span><span class="sxs-lookup"><span data-stu-id="07b3a-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="07b3a-296">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="07b3a-297">Reemplace la resolución de dependencia predeterminada ASP.NET MVC 4 con una instancia de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="07b3a-298">Para ello, reemplace **Initialise** método contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="07b3a-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="07b3a-299">(Código de fragmento de código - *resolución de dependencia de actualización ASP.NET dependencia inyección laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="07b3a-300">ASP.NET MVC proporciona una clase de resolución de dependencia predeterminada.</span><span class="sxs-lookup"><span data-stu-id="07b3a-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="07b3a-301">Para trabajar con las resoluciones de dependencia personalizadas que hemos creado por unity, esta resolución debe reemplazarse.</span><span class="sxs-lookup"><span data-stu-id="07b3a-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="07b3a-302">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="07b3a-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="07b3a-303">En esta tarea, ejecutará la aplicación para comprobar que el Explorador de Store consume el servicio y muestra la imagen y el mensaje recuperado:</span><span class="sxs-lookup"><span data-stu-id="07b3a-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="07b3a-304">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07b3a-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="07b3a-305">Haga clic en **Rock** dentro del menú de géneros y vea cómo el **mensaje** insertado a la vista y se cargan el mensaje de bienvenida y la imagen.</span><span class="sxs-lookup"><span data-stu-id="07b3a-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="07b3a-306">En este ejemplo, estamos entrando a &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="07b3a-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="07b3a-307">![MVC Music Store - inserción de vista](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - inserción de vista")</span><span class="sxs-lookup"><span data-stu-id="07b3a-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="07b3a-308">*MVC Music Store - inserción de vista*</span><span class="sxs-lookup"><span data-stu-id="07b3a-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="07b3a-309">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="07b3a-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="07b3a-310">Ejercicio 3: Inserción de los filtros de acción</span><span class="sxs-lookup"><span data-stu-id="07b3a-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="07b3a-311">En la práctica anterior **filtros de acción personalizados** ha trabajado con la personalización de los filtros y por inyección de código.</span><span class="sxs-lookup"><span data-stu-id="07b3a-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="07b3a-312">En este ejercicio, obtendrá información sobre cómo insertar los filtros con inserción de dependencias mediante el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="07b3a-313">Para ello, se agregará a la solución de Music Store un filtro de acción personalizada que se trazará la actividad del sitio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="07b3a-314">Tarea 1: incluido el filtro de seguimiento de la solución</span><span class="sxs-lookup"><span data-stu-id="07b3a-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="07b3a-315">En esta tarea, se van a incluir en el Store música un filtro de acción personalizada para los eventos de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="07b3a-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="07b3a-316">Como filtro de acción personalizado ya se tratan los conceptos en la práctica anterior &quot;filtros de acción personalizado&quot;, que solo incluya la clase de filtro de la carpeta recursos de este laboratorio y, a continuación, crear un proveedor de filtros para Unity:</span><span class="sxs-lookup"><span data-stu-id="07b3a-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="07b3a-317">Abra el **comenzar** solución se encuentra en la **Source\Ex03 - Insertar acción Filter\Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="07b3a-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="07b3a-318">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="07b3a-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="07b3a-319">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07b3a-320">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="07b3a-321">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="07b3a-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="07b3a-322">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="07b3a-323">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07b3a-324">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07b3a-325">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="07b3a-326">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="07b3a-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="07b3a-327">Incluir **TraceActionFilter.cs** de archivos de **/Sources/activos** a **/filtra** carpeta.</span><span class="sxs-lookup"><span data-stu-id="07b3a-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="07b3a-328">Este filtro de acción personalizada realiza el seguimiento en ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07b3a-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="07b3a-329">Puede comprobar &quot;local de ASP.NET MVC 4 y los filtros de acción dinámica&quot; laboratorio para obtener más referencias.</span><span class="sxs-lookup"><span data-stu-id="07b3a-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="07b3a-330">Agregue la clase vacía **FilterProvider.cs** al proyecto en la carpeta   **/filtros.**</span><span class="sxs-lookup"><span data-stu-id="07b3a-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="07b3a-331">Agregar el **System.Web.Mvc** y **Microsoft.Practices.Unity** espacios de nombres en **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="07b3a-332">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="07b3a-333">Hacer que herede de la clase **IFilterProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="07b3a-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="07b3a-334">Agregar un **IUnityContainer** propiedad en el **FilterProvider** clase y, a continuación, cree un constructor de clase para asignar el contenedor.</span><span class="sxs-lookup"><span data-stu-id="07b3a-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="07b3a-335">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor Constructor*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="07b3a-336">El constructor de clase de proveedor de filtro no está creando un **nuevo** dentro de objeto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="07b3a-337">El contenedor se pasa como parámetro y se soluciona la dependencia de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="07b3a-338">En el **FilterProvider** class, implemente el método **GetFilters** desde **IFilterProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="07b3a-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="07b3a-339">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Filtro proveedor GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="07b3a-340">Tarea 2: registrar y habilitar el filtro</span><span class="sxs-lookup"><span data-stu-id="07b3a-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="07b3a-341">En esta tarea, se habilita el seguimiento del sitio.</span><span class="sxs-lookup"><span data-stu-id="07b3a-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="07b3a-342">Para ello, se registrará el filtro en **Bootstrapper.cs BuildUnityContainer** método para iniciar el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="07b3a-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="07b3a-343">Abra **Web.config** ubicado en la raíz del proyecto y habilitar el seguimiento de traza en el grupo de System.Web.</span><span class="sxs-lookup"><span data-stu-id="07b3a-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="07b3a-344">Abra **Bootstrapper.cs** en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="07b3a-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="07b3a-345">Agregue una referencia a la **MvcMusicStore.Filters** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="07b3a-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="07b3a-346">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - programa previo agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="07b3a-347">Seleccione el **BuildUnityContainer** método y registrar el filtro en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="07b3a-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="07b3a-348">Tendrá que registrar el proveedor de filtros, así como el filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="07b3a-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="07b3a-349">(Código de fragmento de código - *ASP.NET dependencia inyección laboratorio - Ex03 - Register FilterProvider y ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="07b3a-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="07b3a-350">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="07b3a-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="07b3a-351">En esta tarea, ejecutará la aplicación y que el filtro de acción personalizada seguimiento la actividad de prueba:</span><span class="sxs-lookup"><span data-stu-id="07b3a-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="07b3a-352">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07b3a-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="07b3a-353">Haga clic en **Rock** en el menú de géneros.</span><span class="sxs-lookup"><span data-stu-id="07b3a-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="07b3a-354">Puede examinar varios géneros si desea.</span><span class="sxs-lookup"><span data-stu-id="07b3a-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="07b3a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="07b3a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="07b3a-356">*Tienda de música*</span><span class="sxs-lookup"><span data-stu-id="07b3a-356">*Music Store*</span></span>
3. <span data-ttu-id="07b3a-357">Vaya a **/Trace.axd** para ver el seguimiento de la aplicación de página y, a continuación, haga clic en **ver detalles**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="07b3a-358">![Registro de seguimiento de la aplicación](aspnet-mvc-4-dependency-injection/_static/image12.png "registro de seguimiento de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="07b3a-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="07b3a-359">*Registro de seguimiento de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="07b3a-359">*Application Trace Log*</span></span>

    <span data-ttu-id="07b3a-360">![Seguimiento de la aplicación - detalles de la solicitud](aspnet-mvc-4-dependency-injection/_static/image13.png "aplicación Trace - detalles de la solicitud")</span><span class="sxs-lookup"><span data-stu-id="07b3a-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="07b3a-361">*Seguimiento de la aplicación - detalles de la solicitud*</span><span class="sxs-lookup"><span data-stu-id="07b3a-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="07b3a-362">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="07b3a-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="07b3a-363">Resumen</span><span class="sxs-lookup"><span data-stu-id="07b3a-363">Summary</span></span>

<span data-ttu-id="07b3a-364">Al completar este laboratorio práctico ha aprendido a usar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity mediante un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="07b3a-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="07b3a-365">Para lograrlo, ha usado la inserción de dependencias dentro de los controladores, vistas y los filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="07b3a-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="07b3a-366">Se tratan los conceptos siguientes:</span><span class="sxs-lookup"><span data-stu-id="07b3a-366">The following concepts were covered:</span></span>

- <span data-ttu-id="07b3a-367">Características de inserción de dependencias de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="07b3a-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="07b3a-368">Integración de Unity mediante el paquete de NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="07b3a-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="07b3a-369">Inserción de dependencias en controladores</span><span class="sxs-lookup"><span data-stu-id="07b3a-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="07b3a-370">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="07b3a-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="07b3a-371">Inserción de dependencias de los filtros de acción</span><span class="sxs-lookup"><span data-stu-id="07b3a-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="07b3a-372">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="07b3a-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="07b3a-373">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="07b3a-374">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="07b3a-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="07b3a-375">Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="07b3a-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="07b3a-376">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="07b3a-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="07b3a-377">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-377">Click on **Install Now**.</span></span> <span data-ttu-id="07b3a-378">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="07b3a-379">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="07b3a-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="07b3a-380">![Instale Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="07b3a-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="07b3a-381">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="07b3a-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="07b3a-382">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="07b3a-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="07b3a-384">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="07b3a-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="07b3a-385">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="07b3a-385">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="07b3a-387">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="07b3a-387">*Installation progress*</span></span>
6. <span data-ttu-id="07b3a-388">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-388">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="07b3a-390">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="07b3a-390">*Installation completed*</span></span>
7. <span data-ttu-id="07b3a-391">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="07b3a-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="07b3a-392">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="07b3a-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="07b3a-394">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="07b3a-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="07b3a-395">Apéndice B: uso de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="07b3a-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="07b3a-396">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="07b3a-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="07b3a-397">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="07b3a-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="07b3a-398">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="07b3a-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="07b3a-399">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="07b3a-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="07b3a-400">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="07b3a-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="07b3a-401">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="07b3a-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="07b3a-402">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="07b3a-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="07b3a-403">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="07b3a-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="07b3a-404">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="07b3a-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="07b3a-405">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="07b3a-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="07b3a-406">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="07b3a-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="07b3a-407">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="07b3a-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="07b3a-408">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-dependency-injection/_static/image21.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="07b3a-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="07b3a-409">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="07b3a-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="07b3a-410">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-dependency-injection/_static/image22.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="07b3a-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="07b3a-411">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="07b3a-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="07b3a-412">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="07b3a-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="07b3a-413">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="07b3a-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="07b3a-414">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="07b3a-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="07b3a-415">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="07b3a-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="07b3a-416">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-dependency-injection/_static/image23.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="07b3a-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="07b3a-417">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="07b3a-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="07b3a-418">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-dependency-injection/_static/image24.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="07b3a-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="07b3a-419">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="07b3a-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
