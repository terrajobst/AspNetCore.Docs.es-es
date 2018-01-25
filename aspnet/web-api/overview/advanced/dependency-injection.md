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
<a name="dependency-injection-in-aspnet-web-api-2"></a>Inserción de dependencias en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Este tutorial muestra cómo insertar las dependencias en el controlador de ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2
> - [Bloque de aplicación de Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (versión 5 también funciona)


## <a name="what-is-dependency-injection"></a>¿Qué es la inyección de dependencia?

A *dependencia* es cualquier objeto que requiere otro objeto. Por ejemplo, es común para definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controla el acceso a datos. Vamos a mostrar con un ejemplo. En primer lugar, definiremos un modelo de dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Aquí es una clase de repositorio simple que almacena los elementos en una base de datos, mediante Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Ahora vamos a definir un controlador de API Web que admita solicitudes GET para `Product` entidades. (Estoy dejando fuera POST y otros métodos para simplificar el trabajo.) Este es el primer intento:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Tenga en cuenta que depende de la clase de controlador `ProductRepository`, y nos estamos permitiendo que el controlador de crear el `ProductRepository` instancia. Sin embargo, es una mala idea para codificar la dependencia de esta manera, por varias razones.

- Si desea reemplazar `ProductRepository` con una implementación diferente, también debe modificar la clase de controlador.
- Si el `ProductRepository` tiene dependencias, debe configurarlos en el controlador. Para un proyecto grande con varios controladores, su código de configuración deja de estar dispersos en el proyecto.
- Es difícil realizar pruebas unitarias, porque el controlador está codificado de forma rígida para consultar la base de datos. Para una prueba unitaria, debe usar un repositorio ficticios o de rutas internas, lo cual no es posible con el diseño hágala.

Podemos resolver estos problemas por *insertar* el repositorio en el controlador. En primer lugar, refactorizar el `ProductRepository` clase en una interfaz:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

A continuación, proporcione el `IProductRepository` como un parámetro de constructor:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Este ejemplo se utiliza [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). También puede usar *inyección de establecedor*, donde se establezca la dependencia a través de un método de establecedor o propiedad.

Pero ahora hay un problema, porque la aplicación no crea directamente en el controlador. API Web crea el controlador cuando enruta la solicitud y la API Web no sabe nada sobre `IProductRepository`. Esto es donde entra en juego la resolución de dependencia de la API Web.

## <a name="the-web-api-dependency-resolver"></a>La resolución de dependencia de la API Web

API Web define la **IDependencyResolver** interfaz para resolver las dependencias. Aquí está la definición de la interfaz:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

El **IDependencyScope** interfaz tiene dos métodos:

- **GetService** crea una instancia de un tipo.
- **GetServices** crea una colección de objetos de un tipo especificado.

El **IDependencyResolver** hereda del método **IDependencyScope** y agrega el **BeginScope** método. Explicaré los ámbitos más adelante en este tutorial.

Cuando la API Web crea una instancia de controlador, llama primero **IDependencyResolver.GetService**, pasando el tipo de controlador. Puede usar este enlace de la extensibilidad para crear el controlador, resolver cualquier dependencia. Si **GetService** devuelve null, API Web busca un constructor sin parámetros en la clase de controlador.

## <a name="dependency-resolution-with-the-unity-container"></a>Resolución de dependencia con el contenedor de Unity

Aunque puede escribir una completa **IDependencyResolver** implementación desde el principio, la interfaz realmente está diseñado para actuar como puente entre las API Web y los contenedores de IoC existentes.

Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, utilice el contenedor para crear objetos. El contenedor imagina automáticamente las relaciones de dependencia. Muchos de los contenedores de IoC también le permiten controlar aspectos como la duración de los objetos y el ámbito.

> [!NOTE]
> "IoC" es el acrónimo "de inversión de control", que es un patrón general donde un marco de trabajo llama al código de la aplicación. Un contenedor de IoC construye los objetos para usted, que "invierte" el flujo de control normal.


Para este tutorial, usaremos [Unity](https://msdn.microsoft.com/library/ff647202.aspx) desde Microsoft Patterns &amp; prácticas. (Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), y [StructureMap ](http://docs.structuremap.net/).) Puede usar el Administrador de paquetes de NuGet para instalar Unity. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Esta es una implementación de **IDependencyResolver** que encapsula un contenedor de Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Si el **GetService** método no puede resolver un tipo, debe devolver **null**. Si el **GetServices** método no puede resolver un tipo, debe devolver un objeto de colección vacía. No se producen excepciones para los tipos desconocidos.


## <a name="configuring-the-dependency-resolver"></a>Configurar a la resolución de dependencia

Establecer la resolución de dependencia en el **DependencyResolver** propiedad de la información global **HttpConfiguration** objeto.

El código siguiente registra el `IProductRepository` interactuar con Unity y, a continuación, se crea un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ámbito de dependencia y la duración de controlador

Los controladores se crean por solicitud. Para administrar la duración de los objetos, **IDependencyResolver** usa el concepto de un *ámbito*.

La resolución de dependencia adjunta a la **HttpConfiguration** objeto tiene ámbito global. Cuando la API Web crea un controlador, se llama a **BeginScope**. Este método devuelve un **IDependencyScope** que representa un ámbito secundario.

A continuación, llama a API Web **GetService** en el ámbito secundario para crear el controlador. Cuando se completa la solicitud, las llamadas de API de Web **Dispose** en el ámbito secundario. Use la **Dispose** método deshacerse de las dependencias del controlador.

Cómo implementar **BeginScope** depende del contenedor de IoC. Para Unity, ámbito corresponde a un contenedor secundario:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La mayoría de los contenedores de IoC tienen equivalentes similar.
