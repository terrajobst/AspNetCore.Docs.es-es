---
title: Plataformas admitidas de Blazor de ASP.NET Core
author: guardrex
description: Obtenga información sobre las plataformas admitidas en Blazor de ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647111"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Plataformas admitidas de Blazor de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Requisitos del explorador

### <a name="blazor-webassembly"></a>WebAssembly de Blazor

| Explorador                          | Versión               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Current               |
| Mozilla Firefox                  | Current               |
| Google Chrome, incluido Android | Current               |
| Safari, incluido iOS            | Current               |
| Microsoft Internet Explorer      | No admitidas&dagger; |

&dagger;Microsoft Internet Explorer no admite [WebAssembly](https://webassembly.org).

### <a name="blazor-server"></a>Servidor Blazor

| Explorador                          | Versión    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Current    |
| Mozilla Firefox                  | Current    |
| Google Chrome, incluido Android | Current    |
| Safari, incluido iOS            | Current    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Se requieren más polyfill (por ejemplo, se pueden agregar promesas a través de un paquete [Polyfill.io](https://polyfill.io/v3/)).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/hosting-models>
