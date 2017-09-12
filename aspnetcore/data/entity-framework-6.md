---
title: "Introducción a ASP.NET Core y Entity Framework 6"
author: tdykstra
description: "Este artículo muestra cómo usar Entity Framework 6 en una aplicación de ASP.NET Core."
keywords: "Núcleo de ASP.NET, Entity Framework, EF 6"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: fdc24ed9b6b2d412b09871302b5478da4d81ec28
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="ca2b5-104">Introducción a ASP.NET Core y Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ca2b5-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="ca2b5-105">Por [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ca2b5-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ca2b5-106">Este artículo muestra cómo usar Entity Framework 6 en una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="ca2b5-107">Información general</span><span class="sxs-lookup"><span data-stu-id="ca2b5-107">Overview</span></span>

<span data-ttu-id="ca2b5-108">Para usar Entity Framework 6, el proyecto tiene se compilen con .NET Framework, como Entity Framework 6 no es compatible con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="ca2b5-109">Si necesita usar características de multiplataforma debe actualizar a [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="ca2b5-110">La manera recomendada de usar Entity Framework 6 en una aplicación de ASP.NET Core es poner el contexto de EF6 y clases del modelo en una biblioteca de clases del proyecto que tiene como destino la versión completa de framework.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="ca2b5-111">Agregue una referencia a la biblioteca de clases desde el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="ca2b5-112">Vea el ejemplo [solución de Visual Studio con proyectos EF6 y ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="ca2b5-113">No se puede colocar un contexto EF6 en un proyecto de ASP.NET Core porque los proyectos de .NET Core no admiten toda la funcionalidad que EF6 comandos como *Enable-Migrations* requieren.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="ca2b5-114">Independientemente del tipo de proyecto en el que buscar el contexto de EF6, EF6 sólo herramientas de línea de comandos funcionan con un contexto de EF6.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="ca2b5-115">Por ejemplo, `Scaffold-DbContext` sólo está disponible en Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="ca2b5-116">Si necesita la aplicación de ingeniería inversa de una base de datos en un modelo de EF6, consulte [Code First para una base de datos](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="ca2b5-117">Marco de referencia completa y EF6 en el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca2b5-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="ca2b5-118">El proyecto de ASP.NET Core debe hacer referencia a .NET framework y EF6.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="ca2b5-119">Por ejemplo, el *.csproj* archivo del proyecto ASP.NET Core tendrá un aspecto similar al ejemplo siguiente (se muestran solo las partes relevantes del archivo).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="ca2b5-120">Si va a crear un nuevo proyecto, use la **aplicación Web de ASP.NET Core (.NET Framework)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="ca2b5-121">Controlar las cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="ca2b5-121">Handle connection strings</span></span>

<span data-ttu-id="ca2b5-122">Las herramientas de línea de comandos de EF6 que va a utilizar en el proyecto de biblioteca de clases de EF6 requieren un constructor predeterminado, por lo que pueden crear instancias del contexto.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="ca2b5-123">Pero, probablemente deseará especificar la cadena de conexión se utiliza en el proyecto de ASP.NET Core, en cuyo caso el constructor de contexto debe tener un parámetro que le permite pasar en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="ca2b5-124">Este es un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="ca2b5-125">Puesto que el contexto de EF6 no tiene un constructor sin parámetros, el proyecto EF6 tiene que proporcionar una implementación de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="ca2b5-126">Las herramientas de línea de comandos EF6 encontrará y utilizar esa implementación, por lo que puede crear instancias del contexto.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="ca2b5-127">Este es un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="ca2b5-128">En este ejemplo de código, el `IDbContextFactory` implementación pasa en una cadena de conexión codificadas de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="ca2b5-129">Se trata de la cadena de conexión que se va a usar las herramientas de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="ca2b5-130">Desea implementar una estrategia para asegurarse de que la biblioteca de clases utiliza la misma cadena de conexión que utiliza la aplicación que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="ca2b5-131">Por ejemplo, podría obtener el valor de una variable de entorno en los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="ca2b5-132">Configurar la inserción de dependencias en el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca2b5-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="ca2b5-133">En el proyecto principal *Startup.cs* archivo, establecer el contexto de EF6 para la inyección de dependencia (DI) `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="ca2b5-134">Deben configurar el ámbito de los objetos de contexto EF durante un período de duración de cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="ca2b5-135">A continuación, puede obtener una instancia del contexto en los controladores mediante DI.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="ca2b5-136">El código es similar a lo que habría que escribir para un contexto de EF principales:</span><span class="sxs-lookup"><span data-stu-id="ca2b5-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="ca2b5-137">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ca2b5-137">Sample application</span></span>

<span data-ttu-id="ca2b5-138">Para una aplicación de ejemplo de trabajo, consulte la [solución de Visual Studio de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) que muestra en este artículo.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="ca2b5-139">En este ejemplo se puede crear desde cero mediante los pasos siguientes en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ca2b5-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="ca2b5-140">Crear una solución.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-140">Create a solution.</span></span>

* <span data-ttu-id="ca2b5-141">**Agregar nuevo proyecto > Web > aplicación Web de ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="ca2b5-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="ca2b5-142">**Agregar nuevo proyecto > escritorio clásico de Windows > (.NET Framework) de la biblioteca de clases**</span><span class="sxs-lookup"><span data-stu-id="ca2b5-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="ca2b5-143">En **Package Manager Console** (PMC) para ambos proyectos, ejecute el comando `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="ca2b5-144">En el proyecto de biblioteca de clases, crear clases de modelo de datos y una clase de contexto y una implementación de `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="ca2b5-145">En PMC para el proyecto de biblioteca de clases, ejecute los comandos `Enable-Migrations` y `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="ca2b5-146">Si ha configurado el proyecto de ASP.NET Core como proyecto de inicio, agregue `-StartupProjectName EF6` a estos comandos.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="ca2b5-147">En el proyecto principal, agregue una referencia de proyecto al proyecto de biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="ca2b5-148">En el proyecto principal, en *Startup.cs*, registrar el contexto para DI.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="ca2b5-149">En el proyecto principal, en *appSettings.JSON que se*, agregue la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="ca2b5-150">En el proyecto principal, agregue un controlador y vistas para comprobar que puede leer y escribir datos.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="ca2b5-151">(Tenga en cuenta que scaffolding de núcleo de ASP.NET MVC no funcionará con el contexto de EF6 hace referencia desde la biblioteca de clases).</span><span class="sxs-lookup"><span data-stu-id="ca2b5-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="ca2b5-152">Resumen</span><span class="sxs-lookup"><span data-stu-id="ca2b5-152">Summary</span></span>

<span data-ttu-id="ca2b5-153">En este artículo se proporciona instrucciones básicas para el uso de Entity Framework 6 en una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca2b5-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca2b5-154">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ca2b5-154">Additional Resources</span></span>

* [<span data-ttu-id="ca2b5-155">Entity Framework - configuración basada en código</span><span class="sxs-lookup"><span data-stu-id="ca2b5-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
