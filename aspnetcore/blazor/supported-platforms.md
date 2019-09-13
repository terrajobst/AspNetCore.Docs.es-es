---
title: ASP.NET Core las plataformas compatibles con el increíble
author: guardrex
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core increíblemente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 042fbb1b2c7f92b7dc6443319f3f195a12a55adc
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963879"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core las plataformas compatibles con el increíble

Por [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Requisitos del explorador

### <a name="blazor-webassembly"></a>Webassembly increíblemente

| Browser                          | `Version`               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Current               |
| Mozilla Firefox                  | Current               |
| Google Chrome, incluido Android | Current               |
| Safari, incluido iOS            | Current               |
| Microsoft Internet Explorer      | No compatible&dagger; |

&dagger;Microsoft Internet Explorer no es compatible con [WebAssembly](https://webassembly.org).

### <a name="blazor-server"></a>Servidor increíble

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
