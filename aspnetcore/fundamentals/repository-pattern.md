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
# <a name="repository-pattern-with-aspnet-core"></a>Patrón de repositorio con ASP.NET Core

Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) y [Luke Latham](https://github.com/guardrex)

El *patrón de repositorio* es un patrón de diseño que aísla el acceso a datos subyacente a las abstracciones de interfaz. La conexión a la base de datos y la manipulación de objetos de almacenamiento de datos se realizan a través de los métodos proporcionados por la implementación de la interfaz. En consecuencia, no hay ninguna necesidad de llamar a código para tratar problemas de la base de datos, como conexiones, comandos y lectores.

La implementación del patrón de repositorio con ASP.NET Core tiene las siguientes ventajas:

* La organización de la aplicación es menos compleja sin ninguna interdependencia directa entre el negocio y los niveles de acceso a datos.
* Es más fácil reutilizar el código de acceso de base de datos porque el código se administra centralmente mediante uno o varios repositorios.
* El dominio de negocio puede ser una unidad probada de forma independiente desde la capa de la base de datos.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa el patrón de repositorio para inicializar y mostrar una lista de nombres de personajes de una película. La aplicación usa [Entity Framework Core](/ef/core/) y una clase `ApplicationDbContext`para su persistencia de datos, pero la infraestructura de la base de datos no es evidente cuando se tiene acceso a los datos. El acceso a los datos y los objeto de base de datos se extraen detrás de un [repositorio](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Interfaz del repositorio

Una interfaz de repositorio define las propiedades y los métodos de la implementación. En la aplicación de ejemplo, la interfaz de repositorio para los datos de personajes de una película es `ICharacterRepository`. `ICharacterRepository` define los métodos `ListAll` y `Add` requeridos para trabajar con instancias `Character` en la aplicación:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` se define como:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Tipo de repositorio concreto

Esta interfaz se implementa mediante un tipo concreto. En la aplicación de ejemplo, `CharacterRepository` administra el contexto de base de datos e implementa los métodos `ListAll` y `Add`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Registro del servicio de repositorio

El repositorio y el contexto de la base de datos se registran con el contenedor de servicios en `Startup.ConfigureServices`. En la aplicación de ejemplo, `ApplicationDbContext` se configura con la llamada al método de extensión [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` se registra como un servicio con ámbito:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Inyección de una instancia del repositorio

En una clase donde se requiere acceso de la base de datos, una instancia del repositorio se solicita mediante el constructor y se asigna a un campo privado para su uso en los métodos de clase. En la aplicación de ejemplo, se utiliza `ICharacterRepository` para:

* Rellene la base de datos si no existe ningún carácter.
* Obtenga una lista de los personajes para su presentación.

Tenga en cuenta que el código de llamada solo interactúa con la implementación de la interfaz, `CharacterRepository`. El código de llamada no usa `ApplicationDbContext` directamente:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Interfaz de repositorio genérica

En este tema y en su aplicación de ejemplo se muestra la implementación más sencilla del patrón de repositorio, donde se crea un repositorio para cada objeto de negocio. Si la aplicación crece más allá de unos pocos objetos, una *interfaz de repositorio genérica* puede reducir la cantidad de código necesario para implementar el patrón de repositorio. Para más información, vea [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Patrón de repositorio: interfaz de repositorio genérica).

## <a name="additional-resources"></a>Recursos adicionales

* [DevIQ: Patrón de repositorio](https://deviq.com/repository-pattern/)
* [Inserción de dependencias](xref:fundamentals/dependency-injection)
* [Inserción de dependencias en vistas](xref:mvc/views/dependency-injection)
* [Inserción de dependencias en controladores](xref:mvc/controllers/dependency-injection)
* [Inserción de dependencias en controladores de requisitos](xref:security/authorization/dependencyinjection)
* [Inversión de los contenedores de control y el patrón de inserción de dependencias](https://www.martinfowler.com/articles/injection.html)
