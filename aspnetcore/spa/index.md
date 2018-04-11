---
title: Uso de las plantillas de aplicación de una sola página con ASP.NET Core
author: SteveSandersonMS
description: Obtenga información sobre cómo instalar y comenzar a usar las plantillas de proyecto de aplicación de una sola página (SPA) de ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Uso de las plantillas de aplicación de una sola página con ASP.NET Core

> [!NOTE]
> El SDK de .NET Core 2.0.x publicado incluye plantillas de proyecto anteriores para Angular, React y React con Redux. En esta documentación no se tratan las plantillas de proyecto antiguas. En esta documentación se tratan las últimas plantillas para Angular, React y React con Redux, que se pueden instalar manualmente en ASP.NET Core 2.0. Las plantillas se incluyen de forma predeterminada con ASP.NET Core 2.1.

## <a name="prerequisites"></a>Requisitos previos

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), versión 6 o posterior

## <a name="installation"></a>Instalación

Si tiene ASP.NET Core 2.0, ejecute el comando siguiente para instalar las plantillas actualizadas de ASP.NET Core para Angular, React y React con Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Uso de las plantillas

- [Uso de la plantilla de proyecto Angular](xref:spa/angular)
- [Uso de la plantilla de proyecto React](xref:spa/react)
- [Uso de la plantilla de proyecto React con Redux](xref:spa/react-with-redux)
