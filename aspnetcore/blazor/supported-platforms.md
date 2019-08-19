---
title: ASP.NET Core las plataformas compatibles con el increíble
author: guardrex
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core increíblemente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948255"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core las plataformas compatibles con el increíble

Por [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Requisitos del explorador

### <a name="blazor-client-side"></a>Lado cliente de Blazor

| Browser                          | `Version`               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Current               |
| Mozilla Firefox                  | Current               |
| Google Chrome, incluido Android | Current               |
| Safari, incluido iOS            | Current               |
| Microsoft Internet Explorer      | No compatible&dagger; |

&dagger;Microsoft Internet Explorer no es compatible con [WebAssembly](https://webassembly.org).

### <a name="blazor-server-side"></a>Lado servidor de Blazor

| Browser                          | `Version`    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Current    |
| Mozilla Firefox                  | Current    |
| Google Chrome, incluido Android | Current    |
| Safari, incluido iOS            | Current    |
| Microsoft Internet Explorer      | 279&dagger; |

&dagger;Se requieren polyfill adicionales (por ejemplo, se pueden agregar promesas a través de un paquete de [polyfill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/hosting-models>
