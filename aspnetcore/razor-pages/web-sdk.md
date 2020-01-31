---
title: SDK Web de ASP.NET Core
author: Rick-Anderson
description: Información general de Microsoft. NET. SDK. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869771"
---
# <a name="aspnet-core-web-sdk"></a>SDK Web de ASP.NET Core

### <a name="overview"></a>Información general del

`Microsoft.NET.Sdk.Web` es un [SDK de proyecto de MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) para compilar aplicaciones de ASP.net Core. No obstante, es posible compilar una aplicación ASP.NET Core sin este SDK:

* Adaptado para proporcionar una experiencia de primera clase.
* El destino recomendado para la mayoría de los usuarios.

Usar Web. SDK en un proyecto:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Características habilitadas mediante el SDK Web:

* Los proyectos que tienen como destino .NET Core 3,0 o posterior hacen referencia implícitamente:

  * [ASP.net Core marco de trabajo compartido](xref:fundamentals/metapackage-app).
  * [Analizadores](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) diseñados para compilar ASP.net Core aplicaciones.
* El SDK web importa los destinos de MSBuild que habilitan el uso de perfiles de publicación y publicación con WebDeploy.

### <a name="properties"></a>Propiedades

| La propiedad | Descripción |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Deshabilita la referencia implícita a la `Microsoft.AspNetCore.App` marco de trabajo compartido. |
| `DisableImplicitAspNetCoreAnalyzers` | Deshabilita la referencia implícita a los analizadores de ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Deshabilita la referencia implícita a los analizadores de componentes de Razor al compilar aplicaciones Blazor (servidor). |
