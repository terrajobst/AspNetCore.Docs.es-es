---
title: "Introducción a ASP.NET Core y Entity Framework 6"
author: tdykstra
description: "Este artículo muestra cómo usar Entity Framework 6 en una aplicación de ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 7f3c1f28c1e0b3a68db7f6f84c56b18643b56cc8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Introducción a ASP.NET Core y Entity Framework 6

Por [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), y [Tom Dykstra](https://github.com/tdykstra)

Este artículo muestra cómo usar Entity Framework 6 en una aplicación de ASP.NET Core.

## <a name="overview"></a>Información general

Para usar Entity Framework 6, el proyecto tiene se compilen con .NET Framework, como Entity Framework 6 no es compatible con .NET Core. Si necesita usar características de multiplataforma debe actualizar a [Entity Framework Core](https://docs.microsoft.com/ef/).

La manera recomendada de usar Entity Framework 6 en una aplicación de ASP.NET Core es poner el contexto de EF6 y clases del modelo en una biblioteca de clases del proyecto que tiene como destino la versión completa de framework. Agregue una referencia a la biblioteca de clases desde el proyecto de ASP.NET Core. Vea el ejemplo [solución de Visual Studio con proyectos EF6 y ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

No se puede colocar un contexto EF6 en un proyecto de ASP.NET Core porque los proyectos de .NET Core no admiten toda la funcionalidad que EF6 comandos como *Enable-Migrations* requieren.

Independientemente del tipo de proyecto en el que buscar el contexto de EF6, EF6 sólo herramientas de línea de comandos funcionan con un contexto de EF6. Por ejemplo, `Scaffold-DbContext` sólo está disponible en Entity Framework Core. Si necesita la aplicación de ingeniería inversa de una base de datos en un modelo de EF6, consulte [Code First para una base de datos](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Marco de referencia completa y EF6 en el proyecto de ASP.NET Core

El proyecto de ASP.NET Core debe hacer referencia a .NET framework y EF6. Por ejemplo, el *.csproj* archivo del proyecto ASP.NET Core tendrá un aspecto similar al ejemplo siguiente (se muestran solo las partes relevantes del archivo).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Si va a crear un nuevo proyecto, use la **aplicación Web de ASP.NET Core (.NET Framework)** plantilla.

## <a name="handle-connection-strings"></a>Controlar las cadenas de conexión

Las herramientas de línea de comandos de EF6 que va a utilizar en el proyecto de biblioteca de clases de EF6 requieren un constructor predeterminado, por lo que pueden crear instancias del contexto. Pero, probablemente deseará especificar la cadena de conexión se utiliza en el proyecto de ASP.NET Core, en cuyo caso el constructor de contexto debe tener un parámetro que le permite pasar en la cadena de conexión. Este es un ejemplo.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Puesto que el contexto de EF6 no tiene un constructor sin parámetros, el proyecto EF6 tiene que proporcionar una implementación de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Las herramientas de línea de comandos EF6 encontrará y utilizar esa implementación, por lo que puede crear instancias del contexto. Este es un ejemplo.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

En este ejemplo de código, el `IDbContextFactory` implementación pasa en una cadena de conexión codificadas de forma rígida. Se trata de la cadena de conexión que se va a usar las herramientas de línea de comandos. Desea implementar una estrategia para asegurarse de que la biblioteca de clases utiliza la misma cadena de conexión que utiliza la aplicación que realiza la llamada. Por ejemplo, podría obtener el valor de una variable de entorno en los dos proyectos.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Configurar la inserción de dependencias en el proyecto de ASP.NET Core

En el proyecto principal *Startup.cs* archivo, establecer el contexto de EF6 para la inyección de dependencia (DI) `ConfigureServices`. Deben configurar el ámbito de los objetos de contexto EF durante un período de duración de cada solicitud.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

A continuación, puede obtener una instancia del contexto en los controladores mediante DI. El código es similar a lo que habría que escribir para un contexto de EF principales:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Aplicación de ejemplo

Para una aplicación de ejemplo de trabajo, consulte la [solución de Visual Studio de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) que muestra en este artículo.

En este ejemplo se puede crear desde cero mediante los pasos siguientes en Visual Studio:

* Crear una solución.

* **Agregar nuevo proyecto > Web > aplicación Web de ASP.NET Core (.NET Framework)**

* **Agregar nuevo proyecto > escritorio clásico de Windows > (.NET Framework) de la biblioteca de clases**

* En **Package Manager Console** (PMC) para ambos proyectos, ejecute el comando `Install-Package Entityframework`.

* En el proyecto de biblioteca de clases, crear clases de modelo de datos y una clase de contexto y una implementación de `IDbContextFactory`.

* En PMC para el proyecto de biblioteca de clases, ejecute los comandos `Enable-Migrations` y `Add-Migration Initial`. Si ha configurado el proyecto de ASP.NET Core como proyecto de inicio, agregue `-StartupProjectName EF6` a estos comandos.

* En el proyecto principal, agregue una referencia de proyecto al proyecto de biblioteca de clases.

* En el proyecto principal, en *Startup.cs*, registrar el contexto para DI.

* En el proyecto principal, en *appSettings.JSON que se*, agregue la cadena de conexión.

* En el proyecto principal, agregue un controlador y vistas para comprobar que puede leer y escribir datos. (Tenga en cuenta que scaffolding de núcleo de ASP.NET MVC no funcionará con el contexto de EF6 hace referencia desde la biblioteca de clases).

## <a name="summary"></a>Resumen

En este artículo se proporciona instrucciones básicas para el uso de Entity Framework 6 en una aplicación de ASP.NET Core.

## <a name="additional-resources"></a>Recursos adicionales

* [Entity Framework - configuración basada en código](https://msdn.microsoft.com/data/jj680699.aspx)
