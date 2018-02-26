---
title: "Uso de las plantillas de aplicación de una sola página"
author: SteveSandersonMS
description: "Obtenga información sobre cómo instalar y comenzar a usar las plantillas de proyecto de aplicación de una sola página (SPA) de ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a>Uso de las plantillas de aplicación de una sola página

> [!NOTE]
> El SDK de .NET Core 2.0.x publicado incluye plantillas de proyecto anteriores para Angular, React y React con Redux. En esta documentación no se tratan las plantillas de proyecto antiguas. En esta documentación se tratan las últimas plantillas para Angular, React y React con Redux, que se pueden instalar manualmente en ASP.NET Core 2.0. Las plantillas se incluyen de forma predeterminada con ASP.NET Core 2.1.

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET Core](https://www.microsoft.com/net/download), versión 2.0.0 o posterior
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
