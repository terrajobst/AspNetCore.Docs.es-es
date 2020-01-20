---
title: ASP.NET Core Blazor plataformas admitidas
author: guardrex
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160137"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>ASP.NET Core Blazor plataformas admitidas

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Requisitos del explorador

### <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

| Explorador                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Actual               |
| Mozilla Firefox                  | Actual               |
| Google Chrome, incluido Android | Actual               |
| Safari, incluido iOS            | Actual               |
| Microsoft Internet Explorer      | No compatible&dagger; |

&dagger;Microsoft Internet Explorer no es compatible con [WebAssembly](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Servidor de Blazor

| Explorador                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Actual    |
| Mozilla Firefox                  | Actual    |
| Google Chrome, incluido Android | Actual    |
| Safari, incluido iOS            | Actual    |
| Microsoft Internet Explorer      | 11&dagger; |

se requieren &dagger;polyfill adicionales (por ejemplo, se pueden agregar promesas a través de un paquete de [polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/hosting-models>
