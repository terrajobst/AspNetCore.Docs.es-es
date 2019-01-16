---
title: Uso de la plantilla de proyecto React-with-Redux con ASP.NET Core
author: SteveSandersonMS
description: Aprenda cómo comenzar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para React with Redux y create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341625"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="8680b-103">Uso de la plantilla de proyecto React-with-Redux con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8680b-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="8680b-104">Esta documentación no pertenecen a la plantilla de proyecto React con Redux incluida en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8680b-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="8680b-105">Que pertenece a la plantilla de React con Redux más reciente que se puede actualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="8680b-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="8680b-106">La plantilla está disponible en ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="8680b-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="8680b-107">La plantilla de proyecto actualizada de React-with-Redux ofrece un práctico punto de partida para las aplicaciones ASP.NET Core que usan React, Redux y las convenciones [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) para implementar una completa interfaz de usuario (UI) en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="8680b-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="8680b-108">A excepción del comando de creación del proyecto, toda la información sobre la plantilla de React-with-Redux es igual que la de la plantilla de React.</span><span class="sxs-lookup"><span data-stu-id="8680b-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="8680b-109">Para crear este tipo de proyecto, ejecute `dotnet new reactredux` en lugar de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="8680b-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="8680b-110">Para más información sobre la funcionalidad común a ambas plantillas basadas en React, consulte la [documentación de la plantilla de React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="8680b-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="8680b-111">Para obtener información sobre cómo configurar una aplicación secundaria React con Redux en IIS, consulte [ReactRedux plantilla 2.1: No se puede usar SPA en IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span><span class="sxs-lookup"><span data-stu-id="8680b-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
