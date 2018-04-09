---
title: Utilizar la plantilla de proyecto reaccionan con reducción con ASP.NET Core
author: SteveSandersonMS
description: Obtenga información acerca de cómo empezar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para reaccionar con Redux y aplicación reaccionar crear.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="9f699-103">Utilizar la plantilla de proyecto reaccionan con reducción con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f699-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="9f699-104">Esta documentación no acerca de la plantilla de proyecto reaccionan con reducción se incluye en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="9f699-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9f699-105">Se trata de la plantilla de reaccionar con reducción más reciente en la que puede actualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="9f699-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="9f699-106">La plantilla se incluye en ASP.NET Core 2.1 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9f699-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="9f699-107">La plantilla de proyecto de reaccionar con Redux actualizada proporciona un punto de partida cómodo para aplicaciones de ASP.NET Core con reaccionan, Redux, y [aplicación reaccionar crear](https://github.com/facebookincubator/create-react-app) convenciones (CRA) para implementar una interfaz de usuario de cliente enriquecido (UI).</span><span class="sxs-lookup"><span data-stu-id="9f699-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9f699-108">A excepción del comando de creación del proyecto, toda la información sobre la plantilla de reaccionar con reducción es igual que la plantilla de reaccionar.</span><span class="sxs-lookup"><span data-stu-id="9f699-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="9f699-109">Para crear este tipo de proyecto, ejecute `dotnet new reactredux` en lugar de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="9f699-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="9f699-110">Para obtener más información acerca de la funcionalidad común a ambas plantillas basadas en reaccionar, consulte [reaccionar documentación sobre la plantilla](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="9f699-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
