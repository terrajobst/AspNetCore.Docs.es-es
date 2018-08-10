---
title: DevOps con ASP.NET Core y Azure
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722554"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="ffac0-103">DevOps con ASP.NET Core y Azure</span><span class="sxs-lookup"><span data-stu-id="ffac0-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="ffac0-104">Le damos la bienvenida a la guía de Ciclo de vida de desarrollo de Azure para .NET.</span><span class="sxs-lookup"><span data-stu-id="ffac0-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="ffac0-105">En esta guía le mostraremos los conceptos básicos de creación de un ciclo de vida de desarrollo en torno a Azure con herramientas y procesos de .NET.</span><span class="sxs-lookup"><span data-stu-id="ffac0-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="ffac0-106">Cuando haya terminado con esta guía, podrá aprovechar las ventajas de una cadena de herramientas madura de DevOps.</span><span class="sxs-lookup"><span data-stu-id="ffac0-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="ffac0-107">Destinatarios de esta guía</span><span class="sxs-lookup"><span data-stu-id="ffac0-107">Who this guide is for</span></span>

<span data-ttu-id="ffac0-108">Debería ser un desarrollador de ASP.NET experimentado (nivel 200 o 300).</span><span class="sxs-lookup"><span data-stu-id="ffac0-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="ffac0-109">No es necesario que tenga conocimientos de Azure, ya que está incluido en esta introducción.</span><span class="sxs-lookup"><span data-stu-id="ffac0-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="ffac0-110">Esta guía también podría ser útil para ingenieros de DevOps, cuyo trabajo está más relacionado con las operación que con el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ffac0-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="ffac0-111">Esta guía está destinada a desarrolladores para Windows.</span><span class="sxs-lookup"><span data-stu-id="ffac0-111">This guide targets Windows developers.</span></span> <span data-ttu-id="ffac0-112">Sin embargo, .NET Core es completamente compatible con Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="ffac0-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="ffac0-113">Para adaptar esta guía para Linux o macOS, mire las llamadas en las que se indican las diferencias para Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="ffac0-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="ffac0-114">Aspectos no tratados en esta guía</span><span class="sxs-lookup"><span data-stu-id="ffac0-114">What this guide doesn't cover</span></span>

<span data-ttu-id="ffac0-115">Esta guía está centrada en una experiencia de desarrollo continuo de un extremo a otro para desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="ffac0-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="ffac0-116">No es una guía exhaustiva de todo Azure y no se profundiza particularmente en API de .NET para servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="ffac0-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="ffac0-117">El énfasis está en la integración, la implementación, la supervisión y la depuración continuas.</span><span class="sxs-lookup"><span data-stu-id="ffac0-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="ffac0-118">Casi al final de la guía podrá ver recomendaciones para los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="ffac0-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="ffac0-119">En las sugerencias se incluyen servicios de plataformas de Azure que son útiles para desarrolladores de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ffac0-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="ffac0-120">Qué se incluye en esta guía</span><span class="sxs-lookup"><span data-stu-id="ffac0-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="ffac0-121">Herramientas y descargas</span><span class="sxs-lookup"><span data-stu-id="ffac0-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="ffac0-122">Obtenga información sobre dónde adquirir las herramientas que se usan en esta guía.</span><span class="sxs-lookup"><span data-stu-id="ffac0-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="ffac0-123">Implementación en App Service</span><span class="sxs-lookup"><span data-stu-id="ffac0-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="ffac0-124">Obtenga información sobre los distintos métodos para implementar una aplicación ASP.NET Core en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ffac0-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="ffac0-125">Integración e implementación continuas</span><span class="sxs-lookup"><span data-stu-id="ffac0-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="ffac0-126">Cree una solución de implementación e integración continuas de un extremo a otro para su aplicación ASP.NET Core con GitHub, VSTS y Azure.</span><span class="sxs-lookup"><span data-stu-id="ffac0-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="ffac0-127">Supervisión y depuración</span><span class="sxs-lookup"><span data-stu-id="ffac0-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="ffac0-128">Use las herramientas de Azure para supervisar la aplicación, solucionar problemas y ajustarla.</span><span class="sxs-lookup"><span data-stu-id="ffac0-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="ffac0-129">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ffac0-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="ffac0-130">Otras rutas de aprendizaje para los desarrolladores de ASP.NET Core que están aprendiendo sobre Azure.</span><span class="sxs-lookup"><span data-stu-id="ffac0-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="ffac0-131">Agradecimientos</span><span class="sxs-lookup"><span data-stu-id="ffac0-131">Acknowledgments</span></span>

<span data-ttu-id="ffac0-132">Gracias a todos los usuarios de la comunidad de .NET que han contribuido a esta guía con sugerencias útiles.</span><span class="sxs-lookup"><span data-stu-id="ffac0-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="ffac0-133">También queremos mostrar nuestro agradecimiento específicamente a los siguientes miembros de la comunidad, que han contribuido a la revisión final de este contenido:</span><span class="sxs-lookup"><span data-stu-id="ffac0-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="ffac0-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="ffac0-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="ffac0-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="ffac0-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="ffac0-136">Conclusión</span><span class="sxs-lookup"><span data-stu-id="ffac0-136">Conclusion</span></span>

<span data-ttu-id="ffac0-137">Esta guía le preparará para crear un ciclo de vida de desarrollo de integración continua alrededor de ASP.NET Core y Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ffac0-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="ffac0-138">Lecturas adicionales</span><span class="sxs-lookup"><span data-stu-id="ffac0-138">Additional reading</span></span>

* [<span data-ttu-id="ffac0-139">¿Qué es la informática en la nube?</span><span class="sxs-lookup"><span data-stu-id="ffac0-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="ffac0-140">Ejemplos de informática en la nube</span><span class="sxs-lookup"><span data-stu-id="ffac0-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="ffac0-141">¿Qué es una IaaS?</span><span class="sxs-lookup"><span data-stu-id="ffac0-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="ffac0-142">¿Qué es un PaaS?</span><span class="sxs-lookup"><span data-stu-id="ffac0-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
