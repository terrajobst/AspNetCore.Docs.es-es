---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665514"
---
# <a name="aspnet-core-web-api-controller-sample"></a>Ejemplo de controlador de ASP.NET Core Web API

Esta aplicación de ejemplo consta de los siguientes proyectos:

- **WebApiSample.Api.22**: proyecto de ASP.NET Core 2.2 que tiene como destino .NET Core 2.2.
- **WebApiSample.Api.21**: proyecto de ASP.NET Core 2.1 que tiene como destino .NET Core 2.1.
- **WebApiSample.Api.Pre21**: proyecto de ASP.NET Core 2.0 que tiene como destino .NET Core 2.0.
- **WebApiSample.DataAccess**: biblioteca de clases .NET Standard 2.0 que actúa como nivel de acceso a datos en ambos proyectos de Web API.

En este ejemplo se ilustran las variaciones en la creación del controlador de Web API:

- [Derivación de una clase desde ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Anotación de una clase con ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
