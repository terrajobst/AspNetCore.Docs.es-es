---
title: Uso de las plantillas de aplicación de una sola página con ASP.NET Core
author: SteveSandersonMS
description: Obtenga información sobre cómo instalar y comenzar a usar las plantillas de proyecto de aplicación de una sola página (SPA) de ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291448"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="6b684-103">Uso de las plantillas de aplicación de una sola página con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b684-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="6b684-104">El SDK de .NET Core 2.0.x publicado incluye plantillas de proyecto anteriores para Angular, React y React con Redux.</span><span class="sxs-lookup"><span data-stu-id="6b684-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="6b684-105">En esta documentación no se tratan las plantillas de proyecto antiguas.</span><span class="sxs-lookup"><span data-stu-id="6b684-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="6b684-106">En esta documentación se tratan las últimas plantillas para Angular, React y React con Redux, que se pueden instalar manualmente en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6b684-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="6b684-107">Las plantillas se incluyen de forma predeterminada con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6b684-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="6b684-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6b684-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="6b684-109">[Node.js](https://nodejs.org), versión 6 o posterior</span><span class="sxs-lookup"><span data-stu-id="6b684-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="6b684-110">Instalación</span><span class="sxs-lookup"><span data-stu-id="6b684-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6b684-111">Las plantillas ya están instaladas con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6b684-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6b684-112">Si tiene ASP.NET Core 2.0, ejecute el comando siguiente para instalar las plantillas actualizadas de ASP.NET Core para Angular, React y React con Redux:</span><span class="sxs-lookup"><span data-stu-id="6b684-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="6b684-113">Uso de las plantillas</span><span class="sxs-lookup"><span data-stu-id="6b684-113">Use the templates</span></span>

* [<span data-ttu-id="6b684-114">Uso de la plantilla de proyecto Angular</span><span class="sxs-lookup"><span data-stu-id="6b684-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="6b684-115">Uso de la plantilla de proyecto React</span><span class="sxs-lookup"><span data-stu-id="6b684-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="6b684-116">Uso de la plantilla de proyecto React con Redux</span><span class="sxs-lookup"><span data-stu-id="6b684-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
