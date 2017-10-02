---
title: "Introducción a ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 748c8c0b9dd0e6eab0d7347bbf89ed80c10bdb54
ms.sourcegitcommit: e4a1df2a5a85f299322548809e547a79b380bb92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="introduction-to-aspnet-core"></a>Introducción a ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube. Con ASP.NET Core puede hacer lo siguiente:

* Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/en-us/internet-of-things/) y back-ends móviles.
* Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.
* Efectuar implementaciones locales y en la nube.
* Ejecutarlo en [.NET Core o en .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>¿Por qué debería usar ASP.NET Core?

Millones de desarrolladores han usado ASP.NET (y siguen usándolo) para crear aplicaciones web. ASP.NET Core es un nuevo diseño de ASP.NET que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.

ASP.NET Core ofrece las siguientes ventajas:

* Un caso unificado para crear API web y una interfaz de usuario web.
* Integración de [marcos del lado cliente modernos](xref:client-side/index) y flujos de trabajo de desarrollo.
* Un [sistema de configuración](xref:fundamentals/configuration) basado en el entorno y preparado para la nube.
* [Inserción de dependencias](xref:fundamentals/dependency-injection) integrada.
* Una canalización de solicitudes HTTP ligera, modular y de alto rendimiento.
* Capacidad de hospedarse en IIS o de autohospedarse en su propio proceso.
* Se puede ejecutar en [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), que es compatible con un auténtico control de versiones de aplicaciones en paralelo.
* Herramientas que simplifican el desarrollo web moderno.
* Capacidad para compilarse y ejecutarse en Windows, macOS y Linux.
* De código abierto y centrado en la comunidad.

ASP.NET Core se distribuye en su totalidad como paquetes [NuGet](https://www.nuget.org/). Esto le permite optimizar la aplicación para incluir únicamente los paquetes NuGet que necesita. Entre las ventajas de una menor superficie de aplicación se incluyen una mayor seguridad, un mantenimiento reducido y un rendimiento mejorado.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC

ASP.NET Core MVC proporciona funciones que le ayudarán a crear [API web](xref:tutorials/index#building-web-apis) y [aplicaciones web](xref:tutorials/index#building-web-applications):

* El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se [puedan hacer pruebas](testing/index.md) en las API web y en las aplicaciones web.
* [Páginas de Razor](xref:mvc/razor-pages/index) (novedad de la versión 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.
* La [sintaxis de Razor](xref:mvc/views/razor) proporciona un lenguaje productivo para las [páginas de Razor](xref:mvc/razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).
* Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten al código de servidor participar en la creación y representación de elementos HTML en archivos de Razor.
* La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](mvc/models/formatting.md) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.
* El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.
* La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.

## <a name="client-side-development"></a>Desarrollo del lado cliente

ASP.NET Core se ha diseñado para integrarse a la perfección con una serie de marcos del lado cliente, como [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) y [Bootstrap](xref:client-side/bootstrap). Vea [Client-side development](client-side/index.md) (Desarrollo del lado cliente) para más información.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Tutoriales de ASP.NET Core](xref:tutorials/index)
* [Conceptos básicos de ASP.NET Core](xref:fundamentals/index)
* [La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso del equipo y planea y presenta nuevos blogs y fabricantes de software de terceros.
