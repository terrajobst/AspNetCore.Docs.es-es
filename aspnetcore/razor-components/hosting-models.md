---
title: Los modelos de hospedaje de componentes de Razor
author: guardrex
description: Comprender Blazor del lado cliente y servidor ASP.NET Core Razor componentes, modelos de hospedaje.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515649"
---
# <a name="razor-components-hosting-models"></a>Los modelos de hospedaje de componentes de Razor

Por [Daniel Roth](https://github.com/danroth27)

Componentes de Razor es un marco web diseñado para ejecutarse del lado cliente en el explorador en un [WebAssembly](http://webassembly.org/)-basado en tiempo de ejecución .NET (*Blazor*) o de servidor en ASP.NET Core (*Razor de ASP.NET Core Componentes*). Independientemente de los modelos de hospedaje de modelo, la aplicación y el componente *siguen siendo los mismos*. En este artículo se describe los modelos de hospedaje disponibles:

* [Blazor del lado cliente](#client-side-hosting-model)
* [Razor Components de ASP.NET Core del lado servidor](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>Modelo de hospedaje del lado cliente

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

El modelo de hospedaje principal para Blazor es la ejecución del cliente en el explorador en WebAssembly. La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador. La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Las actualizaciones de la interfaz de usuario y control de eventos se producen dentro del mismo proceso. Recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de atender contenido estático a los clientes.

![Blazor de cliente: La aplicación Blazor se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/client-side.png)

Para crear una aplicación de Blazor con el modelo de hospedaje del lado cliente, use cualquiera de las siguientes plantillas:

* **Blazor** ([dotnet nuevo blazor](/dotnet/core/tools/dotnet-new)) &ndash; implementado como un conjunto de archivos estáticos.
* **Blazor (ASP.NET Core se hospedan)** ([dotnet nuevo blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; hospedadas por un servidor de ASP.NET Core. La aplicación de ASP.NET Core sirve la aplicación Blazor a los clientes. La aplicación de Blazor del lado cliente puede interactuar con el servidor a través de la red mediante llamadas a la API web o [SignalR](xref:signalr/introduction).

Las plantillas incluyen el *components.webassembly.js* que controla la secuencia de comandos:

* Descargando el tiempo de ejecución. NET, la aplicación y las dependencias de la aplicación.
* Inicialización del tiempo de ejecución para ejecutar la aplicación.

El modelo de hospedaje del lado cliente ofrece varias ventajas. Blazor del lado cliente:

* No tiene ninguna dependencia de .NET del lado servidor.
* Aprovecha completamente las capacidades y los recursos del cliente.
* Las descargas de trabajan desde el servidor al cliente.
* Es compatible con escenarios sin conexión.

Existen desventajas al hospedaje del lado cliente. Blazor del lado cliente:

* Restringe la aplicación a las capacidades del explorador.
* Requiere hardware de cliente compatible con y software (por ejemplo, WebAssembly soporte).
* Tiene un tamaño de descarga y aplicación mayor tiempo de carga.
* Tiene menos madurando en tiempo de ejecución de .NET y la compatibilidad con las herramientas (por ejemplo, las limitaciones de [.NET Standard](/dotnet/standard/net-standard) soporte técnico y depuración).

## <a name="server-side-hosting-model"></a>Modelo de hospedaje del lado servidor

Con el modelo de hospedaje de servidor de componentes de Razor de ASP.NET Core, la aplicación se ejecuta en el servidor desde dentro de una aplicación ASP.NET Core. Actualizaciones de la interfaz de usuario, el control de eventos y llamadas de JavaScript se controlan a través de un [SignalR](xref:signalr/introduction) conexión.

![ASP.NET Core Razor componentes del lado servidor: El explorador se interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de una conexión de SignalR.](hosting-models/_static/server-side.png)

Para crear una aplicación de los componentes de Razor con el modelo de hospedaje de servidor, use ASP.NET Core **Razor componentes** plantilla ([dotnet nuevo razorcomponents](/dotnet/core/tools/dotnet-new)). La aplicación de ASP.NET Core hospeda la aplicación de servidor de componentes de Razor y establece el punto de conexión de SignalR que se conectan los clientes. La aplicación hace referencia a la aplicación de ASP.NET Core `Startup` clase para agregar:

* Servicios de componentes de Razor del lado servidor.
* La aplicación a la solicitud de canalización de control.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

El *components.server.js* script&dagger; establece la conexión de cliente. Es responsabilidad de la aplicación para conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en el caso de una conexión de red perdidas).

El modelo de hospedaje de servidor ofrece varias ventajas. Componentes del lado servidor Razor:

* Tiene un tamaño de la aplicación significativamente menor que una aplicación de cliente Blazor y mucho más rápida.
* Puede aprovechar al máximo las capacidades de servidor, incluido el uso de las API compatibles de .NET Core.
* Ejecutar en .NET Core en el servidor, por lo que las herramientas, como la depuración, de .NET existente funciona según lo previsto.
* Funciona con clientes ligeros (por ejemplo, los exploradores que no son compatibles con WebAssembly y recursos restringir dispositivos).
* .NET /C# código base, incluido el código de componente de la aplicación, no se proporcione a los clientes.

Existen desventajas al lado del servidor de hospedaje. Componentes del lado servidor Razor:

* Tienen una latencia mayor: Cada interacción del usuario implica un salto de red.
* No hay compatibilidad sin conexión con la oferta: Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.
* Ha reducido la escalabilidad: El servidor debe administrar varias conexiones de cliente y administrar el estado de cliente.
* Requiere que un servidor de ASP.NET Core para atender la aplicación. No es posible la implementación sin un servidor (por ejemplo, desde una red CDN).

&dagger;El *components.server.js* script se publica en la siguiente ruta: *bin / {depurar | La versión} / {.NET FRAMEWORK de destino} /publish/ {nombre de la aplicación}. Dist/aplicación/platafor_ma*.
