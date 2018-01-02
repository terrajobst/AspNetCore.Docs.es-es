---
title: "Introducción a ASP.NET Core"
author: rick-anderson
description: "Proporciona una introducción a ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 3a18ed30819a3d395e9bfb5dba0547667a4425e8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-aspnet-core"></a>Introducción a ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube. Con ASP.NET Core puede hacer lo siguiente:

* Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/internet-of-things/) y back-ends móviles.
* Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.
* Efectuar implementaciones locales y en la nube.
* Ejecutarlo en [.NET Core o en .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>¿Por qué debería usar ASP.NET Core?

Millones de desarrolladores han usado [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) (y siguen usándolo) para crear aplicaciones web. ASP.NET Core es un nuevo diseño de ASP.NET 4.x que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.

ASP.NET Core ofrece las siguientes ventajas:

* Un caso unificado para crear API web y una interfaz de usuario web.
* Integración de [marcos del lado cliente modernos](xref:client-side/index) y flujos de trabajo de desarrollo.
* Un [sistema de configuración](xref:fundamentals/configuration/index) basado en el entorno y preparado para la nube.
* [Inserción de dependencias](xref:fundamentals/dependency-injection) integrada.
* Una canalización de solicitudes HTTP ligera, modular y de [alto rendimiento](https://github.com/aspnet/benchmarks).
* Capacidad de hospedarse en [IIS](xref:publishing/iis), [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), [Docker](xref:publishing/docker) o de autohospedarse en su propio proceso.
* Control de versiones de aplicaciones en paralelo con [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) como destino.
* Herramientas que simplifican el desarrollo web moderno.
* Capacidad para compilarse y ejecutarse en Windows, macOS y Linux.
* De código abierto y [centrado en la comunidad](https://live.asp.net/).

ASP.NET Core se distribuye en su totalidad como paquetes [NuGet](https://www.nuget.org/). Esto le permite optimizar la aplicación para incluir únicamente los paquetes NuGet necesarios. De hecho, las aplicaciones ASP.NET Core 2.x que tienen .NET Core como destino solo requieren un [paquete NuGet único](xref:fundamentals/metapackage). Entre las ventajas de una menor superficie de aplicación se incluyen una mayor seguridad, un mantenimiento reducido y un rendimiento mejorado.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC

ASP.NET Core MVC proporciona características para crear [API web](xref:tutorials/index#building-web-apis) y [aplicaciones web](xref:tutorials/index#building-web-applications):

* El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se [puedan hacer pruebas](testing/index.md) en las API web y en las aplicaciones web.
* [Páginas de Razor](xref:mvc/razor-pages/index) (novedad de ASP.NET Core 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.
* El [marcado de Razor](xref:mvc/views/razor) proporciona una sintaxis productiva para las [páginas de Razor](xref:mvc/razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).
* Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.
* La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](mvc/models/formatting.md) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.
* El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.
* La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.

## <a name="client-side-development"></a>Desarrollo del lado cliente

ASP.NET Core se integra perfectamente con bibliotecas y marcos de trabajo populares del lado cliente, que incluyen [Angular](xref:spa/angular), [React](xref:spa/react) y [Bootstrap](xref:client-side/bootstrap). Vea [Client-side development](xref:client-side/index) (Desarrollo del lado cliente) para más información.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Tutoriales de ASP.NET Core](xref:tutorials/index)
* [Conceptos básicos de ASP.NET Core](xref:fundamentals/index)
* [La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso y los planes del equipo. Incluye nuevos blogs y nuevo software de terceros.
