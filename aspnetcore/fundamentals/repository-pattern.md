---
title: Patrón de repositorio con ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo implementar el patrón de diseño de la aplicación de repositorio en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342696"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="8867e-103">Patrón de repositorio con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8867e-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="8867e-104">Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8867e-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8867e-105">El *patrón de repositorio* es un patrón de diseño que aísla el acceso a datos subyacente a las abstracciones de interfaz.</span><span class="sxs-lookup"><span data-stu-id="8867e-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="8867e-106">La conexión a la base de datos y la manipulación de objetos de almacenamiento de datos se realizan a través de los métodos proporcionados por la implementación de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="8867e-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="8867e-107">En consecuencia, no hay ninguna necesidad de llamar a código para tratar problemas de la base de datos, como conexiones, comandos y lectores.</span><span class="sxs-lookup"><span data-stu-id="8867e-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="8867e-108">La implementación del patrón de repositorio con ASP.NET Core tiene las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="8867e-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="8867e-109">La organización de la aplicación es menos compleja sin ninguna interdependencia directa entre el negocio y los niveles de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="8867e-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="8867e-110">Es más fácil reutilizar el código de acceso de base de datos porque el código se administra centralmente mediante uno o varios repositorios.</span><span class="sxs-lookup"><span data-stu-id="8867e-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="8867e-111">El dominio de negocio puede ser una unidad probada de forma independiente desde la capa de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8867e-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="8867e-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8867e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8867e-113">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa el patrón de repositorio para inicializar y mostrar una lista de nombres de personajes de una película.</span><span class="sxs-lookup"><span data-stu-id="8867e-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="8867e-114">La aplicación usa [Entity Framework Core](/ef/core/) y una clase `ApplicationDbContext`para su persistencia de datos, pero la infraestructura de la base de datos no es evidente cuando se tiene acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="8867e-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="8867e-115">El acceso a los datos y los objeto de base de datos se extraen detrás de un [repositorio](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="8867e-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="8867e-116">Interfaz del repositorio</span><span class="sxs-lookup"><span data-stu-id="8867e-116">Repository interface</span></span>

<span data-ttu-id="8867e-117">Una interfaz de repositorio define las propiedades y los métodos de la implementación.</span><span class="sxs-lookup"><span data-stu-id="8867e-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="8867e-118">En la aplicación de ejemplo, la interfaz de repositorio para los datos de personajes de una película es `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="8867e-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="8867e-119">`ICharacterRepository` define los métodos `ListAll` y `Add` requeridos para trabajar con instancias `Character` en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8867e-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8867e-120">`Character` se define como:</span><span class="sxs-lookup"><span data-stu-id="8867e-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="8867e-121">Tipo de repositorio concreto</span><span class="sxs-lookup"><span data-stu-id="8867e-121">Repository concrete type</span></span>

<span data-ttu-id="8867e-122">Esta interfaz se implementa mediante un tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="8867e-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="8867e-123">En la aplicación de ejemplo, `CharacterRepository` administra el contexto de base de datos e implementa los métodos `ListAll` y `Add`:</span><span class="sxs-lookup"><span data-stu-id="8867e-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="8867e-124">Registro del servicio de repositorio</span><span class="sxs-lookup"><span data-stu-id="8867e-124">Register the repository service</span></span>

<span data-ttu-id="8867e-125">El repositorio y el contexto de la base de datos se registran con el contenedor de servicios en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8867e-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8867e-126">En la aplicación de ejemplo, `ApplicationDbContext` se configura con la llamada al método de extensión [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="8867e-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="8867e-127">`ICharacterRepository` se registra como un servicio con ámbito:</span><span class="sxs-lookup"><span data-stu-id="8867e-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="8867e-128">Inyección de una instancia del repositorio</span><span class="sxs-lookup"><span data-stu-id="8867e-128">Inject an instance of the repository</span></span>

<span data-ttu-id="8867e-129">En una clase donde se requiere acceso de la base de datos, una instancia del repositorio se solicita mediante el constructor y se asigna a un campo privado para su uso en los métodos de clase.</span><span class="sxs-lookup"><span data-stu-id="8867e-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="8867e-130">En la aplicación de ejemplo, se utiliza `ICharacterRepository` para:</span><span class="sxs-lookup"><span data-stu-id="8867e-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="8867e-131">Rellene la base de datos si no existe ningún carácter.</span><span class="sxs-lookup"><span data-stu-id="8867e-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="8867e-132">Obtenga una lista de los personajes para su presentación.</span><span class="sxs-lookup"><span data-stu-id="8867e-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="8867e-133">Tenga en cuenta que el código de llamada solo interactúa con la implementación de la interfaz, `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="8867e-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="8867e-134">El código de llamada no usa `ApplicationDbContext` directamente:</span><span class="sxs-lookup"><span data-stu-id="8867e-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="8867e-135">Interfaz de repositorio genérica</span><span class="sxs-lookup"><span data-stu-id="8867e-135">Generic repository interface</span></span>

<span data-ttu-id="8867e-136">En este tema y en su aplicación de ejemplo se muestra la implementación más sencilla del patrón de repositorio, donde se crea un repositorio para cada objeto de negocio.</span><span class="sxs-lookup"><span data-stu-id="8867e-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="8867e-137">Si la aplicación crece más allá de unos pocos objetos, una *interfaz de repositorio genérica* puede reducir la cantidad de código necesario para implementar el patrón de repositorio.</span><span class="sxs-lookup"><span data-stu-id="8867e-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="8867e-138">Para más información, vea [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Patrón de repositorio: interfaz de repositorio genérica).</span><span class="sxs-lookup"><span data-stu-id="8867e-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8867e-139">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8867e-139">Additional resources</span></span>

* [<span data-ttu-id="8867e-140">DevIQ: Patrón de repositorio</span><span class="sxs-lookup"><span data-stu-id="8867e-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="8867e-141">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8867e-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="8867e-142">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="8867e-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="8867e-143">Inserción de dependencias en controladores</span><span class="sxs-lookup"><span data-stu-id="8867e-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="8867e-144">Inserción de dependencias en controladores de requisitos</span><span class="sxs-lookup"><span data-stu-id="8867e-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="8867e-145">Inversión de los contenedores de control y el patrón de inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8867e-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
