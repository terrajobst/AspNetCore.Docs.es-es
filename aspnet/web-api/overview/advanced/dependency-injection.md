---
uid: web-api/overview/advanced/dependency-injection
title: Inserción de dependencias en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo insertar dependencias en el controlador de ASP.NET Web API. Versiones de software que se usa en el bloque de aplicaciones de Web API 2 Unity tutorial...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 92ce5eadc7f371540295c1c4279f817dba09f8e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369177"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Inserción de dependencias en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Este tutorial muestra cómo insertar dependencias en el controlador de ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2
> - [Bloque de aplicaciones de Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (versión 5 también funciona)


## <a name="what-is-dependency-injection"></a>¿Qué es la inserción de dependencias?

Un *dependencia* es cualquier objeto que requiere otro objeto. Por ejemplo, es común para definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controla el acceso a datos. Vamos a mostrar con un ejemplo. En primer lugar, definiremos un modelo de dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Esta es una clase de repositorio simple que almacena los elementos en una base de datos mediante Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Ahora vamos a definir un controlador de API Web que admita solicitudes GET para `Product` entidades. (Dejo fuera de la publicación y otros métodos para simplificar las cosas). Este es un primer intento:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Tenga en cuenta que depende de la clase de controlador `ProductRepository`, y vamos a dejar que el controlador de crear el `ProductRepository` instancia. Sin embargo, es aconsejable codificar de forma rígida la dependencia de este modo, por varias razones.

- Si desea reemplazar `ProductRepository` con una implementación diferente, también deberá modificar la clase de controlador.
- Si el `ProductRepository` tiene dependencias, debe configurar estos dentro del controlador. Para un proyecto grande con varios controladores, su código de configuración se convierte en dispersa por todo el proyecto.
- Resulta difícil realizar pruebas unitarias, porque el controlador de forma rígida para consultar la base de datos. Para una prueba unitaria, debe usar un repositorio de código auxiliar o de simulacro, que no es posible con el diseño hágala.

Podemos abordar estos problemas por *inyectando* el repositorio en el controlador. En primer lugar, refactorizar la `ProductRepository` clase en una interfaz:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

A continuación, proporcione el `IProductRepository` como un parámetro de constructor:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Este ejemplo se utiliza [inserción del constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). También puede usar *inserción del establecedor*, donde se establece la dependencia a través de un método establecedor o una propiedad.

Pero ahora hay un problema, ya que la aplicación no crea directamente en el controlador. Web API crea el controlador al que enruta la solicitud y API Web no sabe nada sobre `IProductRepository`. Esto es donde entra en juego la resolución de dependencia de API Web.

## <a name="the-web-api-dependency-resolver"></a>La resolución de dependencia de API Web

API Web define la **IDependencyResolver** interfaz para resolver las dependencias. Esta es la definición de la interfaz:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

El **IDependencyScope** interfaz tiene dos métodos:

- **GetService** crea una instancia de un tipo.
- **GetServices** crea una colección de objetos de un tipo especificado.

El **IDependencyResolver** hereda el método **IDependencyScope** y agrega el **BeginScope** método. Hablaré sobre los ámbitos más adelante en este tutorial.

Cuando la API Web crea una instancia de controlador, primero llama **IDependencyResolver.GetService**, pasando el tipo de controlador. Puede usar este enlace de extensibilidad para crear el controlador, resolver cualquier dependencia. Si **GetService** devuelve null, Web API busca un constructor sin parámetros en la clase de controlador.

## <a name="dependency-resolution-with-the-unity-container"></a>Resolución de dependencias con el contenedor de Unity

Aunque podría escribir una completa **IDependencyResolver** implementación desde cero, la interfaz está realmente diseñada para actuar como puente entre la API Web y los contenedores de IoC existentes.

Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, usar el contenedor para crear objetos. El contenedor calcula automáticamente las relaciones de dependencia. Muchos de los contenedores de IoC también permiten controlar aspectos como la duración del objeto y el ámbito.

> [!NOTE]
> "IoC" es el acrónimo "inversión de control", que es un patrón general que llama un marco de trabajo en el código de la aplicación. Un contenedor de IoC construye los objetos, que "invierte" el flujo de control normal.


Para este tutorial, vamos a usar [Unity](https://msdn.microsoft.com/library/ff647202.aspx) desde Microsoft Patterns &amp; prácticas. (Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), y [StructureMap ](http://docs.structuremap.net/).) Puede usar el Administrador de paquetes de NuGet para instalar Unity. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Esta es una implementación de **IDependencyResolver** que ajusta un contenedor de Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Si el **GetService** método no puede resolver un tipo, debe devolver **null**. Si el **GetServices** método no puede resolver un tipo, debe devolver un objeto de colección vacía. No producir excepciones para los tipos desconocidos.


## <a name="configuring-the-dependency-resolver"></a>Configurar a la resolución de dependencia

Establecer la resolución de dependencia en el **DependencyResolver** propiedad de la información global **HttpConfiguration** objeto.

El código siguiente registra el `IProductRepository` interfaz con Unity y, a continuación, se crea un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ámbito de dependencia y duración de controlador

Los controladores se crean por solicitud. Para administrar la vigencia del objeto, **IDependencyResolver** usa el concepto de un *ámbito*.

La resolución de dependencia adjunta a la **HttpConfiguration** objeto tiene ámbito global. Cuando la API Web crea un controlador, llama a **BeginScope**. Este método devuelve un **IDependencyScope** que representa un ámbito secundario.

A continuación, llama la API Web **GetService** en el ámbito secundario para crear el controlador. Cuando se completa la solicitud, llama la API Web **Dispose** en el ámbito secundario. Use la **Dispose** método deshacerse de las dependencias del controlador.

Cómo implementar **BeginScope** depende del contenedor de IoC. Para Unity, ámbito corresponde a un contenedor secundario:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La mayoría de los contenedores de IoC tienen equivalentes similar.
