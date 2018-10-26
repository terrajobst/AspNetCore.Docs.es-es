---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Consulte una introducción a ASP.NET Core, un marco multiplataforma de código abierto y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: fcd95b88b970073f4d7eddf89729683d18be449d
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090659"
---
# <a name="introduction-to-aspnet-core"></a>Introducción a ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube. Con ASP.NET Core puede hacer lo siguiente:

* Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/internet-of-things/) y back-ends móviles.
* Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.
* Efectuar implementaciones locales y en la nube.
* Ejecutarlo en [.NET Core o en .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>¿Por qué debería usar ASP.NET Core?

Millones de desarrolladores han usado [ASP.NET 4.x](/aspnet/overview) (y siguen usándolo) para crear aplicaciones web. ASP.NET Core es un nuevo diseño de ASP.NET 4.x que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC

ASP.NET Core MVC proporciona características para crear [API web](xref:tutorials/index#build-web-apis) y [aplicaciones web](xref:tutorials/index#build-web-apps):

* El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se [puedan hacer pruebas](xref:test/index) en las API web y en las aplicaciones web.
* [Páginas de Razor](xref:razor-pages/index) (novedad de ASP.NET Core 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.
* El [marcado de Razor](xref:mvc/views/razor) proporciona una sintaxis productiva para las [páginas de Razor](xref:razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).
* Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.
* La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](xref:web-api/advanced/formatting) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.
* El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.
* La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.

## <a name="client-side-development"></a>Desarrollo del lado cliente

ASP.NET Core se integra perfectamente con bibliotecas y marcos de trabajo populares del lado cliente, que incluyen [Angular](xref:spa/angular), [React](xref:spa/react) y [Bootstrap](https://getbootstrap.com/). Para obtener más información, vea [Desarrollo del lado cliente](xref:client-side/index).

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core con .NET Framework como destino

ASP.NET Core puede tener como destino .NET Core o .NET Framework. Las aplicaciones de ASP.NET Core que tienen como destino .NET Framework no son multiplataforma, sino que solo se ejecutan en Windows. No está previsto eliminar la compatibilidad con .NET Framework como destino en ASP.NET Core. Por lo general, ASP.NET Core está formado por bibliotecas de [.NET Standard](/dotnet/standard/net-standard). Las aplicaciones escritas con .NET Standard 2.0 se ejecutan en cualquier lugar en el que se admita .NET Standard 2.0.

ASP.NET Core 2.x se admite en las versiones de .NET Framework compatibles con .NET Standard 2.0:

* Se recomienda encarecidamente .NET Framework 4.7.1 y posterior.
* .NET Framework 4.6.1 y posterior.

El uso de .NET Core como destino cuenta con varias ventajas que van en aumento con cada versión. Entre las ventajas del uso de .NET Core en vez de .NET Framework se incluyen las siguientes:

* Multiplataforma. Ejecución en macOS, Linux y Windows.
* Rendimiento mejorado
* Control de versiones en paralelo.
* Nuevas API.
* Abrir origen

Estamos trabajando intensamente para cerrar la brecha de API entre .NET Framework y .NET Core. El [paquete de compatibilidad de Windows](/dotnet/core/porting/windows-compat-pack) ha permitido que miles de API solo de Windows estén disponibles en .NET Core. Estas API no estaban disponibles en .NET Core 1.x.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Tutoriales de ASP.NET Core](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Conceptos básicos de ASP.NET Core](xref:fundamentals/index)
* [La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso y los planes del equipo. Incluye nuevos blogs y nuevo software de terceros.
