---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Escenario: Configurar un entorno de prueba para la implementación Web | Documentos de Microsoft'
author: jrjlee
description: En este tema se describe un escenario de implementación web típica para desarrolladores o entornos de prueba y se explican las tareas que debe completar para configurar un si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="71cb4-103">Escenario: Configurar un entorno de prueba para la implementación Web</span><span class="sxs-lookup"><span data-stu-id="71cb4-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="71cb4-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="71cb4-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="71cb4-105">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="71cb4-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="71cb4-106">En este tema se describe un escenario de implementación web típica para desarrolladores o entornos de prueba y se explican las tareas que debe completar para configurar un entorno similar.</span><span class="sxs-lookup"><span data-stu-id="71cb4-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="71cb4-107">Cuando los desarrolladores trabajan en aplicaciones web, a menudo ha otorgado acceso a un entorno de servidor que puede usar para probar sus aplicaciones los cambios en una opción realista.</span><span class="sxs-lookup"><span data-stu-id="71cb4-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="71cb4-108">Normalmente, este tipo de entorno de desarrollo o prueba tiene estas características:</span><span class="sxs-lookup"><span data-stu-id="71cb4-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="71cb4-109">El entorno consta de un único servidor web y un servidor de base de datos único.</span><span class="sxs-lookup"><span data-stu-id="71cb4-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="71cb4-110">Los desarrolladores suelen tengan privilegios de administrador en los servidores, que les permiten configurar el entorno para los requisitos de sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="71cb4-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="71cb4-111">Cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir paso a paso o la implementación automatizada.</span><span class="sxs-lookup"><span data-stu-id="71cb4-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="71cb4-112">Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink es un desarrollador de Fabrikam, Inc. Matt estén trabajando en la solución póngase en contacto con el administrador y con regularidad debe implementar cambios en un entorno de prueba.</span><span class="sxs-lookup"><span data-stu-id="71cb4-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="71cb4-113">Matt es un administrador en el servidor web de prueba y el servidor de base de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="71cb4-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="71cb4-114">Inicialmente, Matt debe ser capaz de implementar la solución en el entorno de prueba directamente.</span><span class="sxs-lookup"><span data-stu-id="71cb4-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="71cb4-115">Como el trabajo progresa y los desarrolladores más unirán el equipo, el Administrador de contacto solución está configurada para la integración continua (CI) en Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="71cb4-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="71cb4-116">Cada vez que un desarrollador protege en el contenido, debe Team Build compilar la solución, ejecute cualquier prueba unitaria e implementar automáticamente la solución en el entorno de prueba.</span><span class="sxs-lookup"><span data-stu-id="71cb4-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="71cb4-117">Introducción a la solución</span><span class="sxs-lookup"><span data-stu-id="71cb4-117">Solution Overview</span></span>

<span data-ttu-id="71cb4-118">El entorno de prueba debe admitir paso a paso o automatizar la implementación desde un equipo remoto, por lo que tendrá una opción de dos enfoques principales.</span><span class="sxs-lookup"><span data-stu-id="71cb4-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="71cb4-119">Puede realizar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="71cb4-119">You can:</span></span>

- <span data-ttu-id="71cb4-120">Configurar el servidor web de prueba para permitir la implementación con el servicio de agente de implementación Web (el "agente remoto").</span><span class="sxs-lookup"><span data-stu-id="71cb4-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="71cb4-121">Configurar el servidor web de prueba para permitir la implementación con el controlador de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="71cb4-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="71cb4-122">También puede usar [implementar Web a petición](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (el "agente temp").</span><span class="sxs-lookup"><span data-stu-id="71cb4-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="71cb4-123">Esto es similar al enfoque de agente remoto en cuanto a requisitos y restricciones.</span><span class="sxs-lookup"><span data-stu-id="71cb4-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="71cb4-124">En este caso, los desarrolladores tienen privilegios de administrador en los servidores de destino y el entorno de prueba no está sujeta a restricciones de seguridad estricta, por lo que es la opción lógica configurar el servidor web de prueba para permitir la implementación con el agente remoto.</span><span class="sxs-lookup"><span data-stu-id="71cb4-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="71cb4-125">Esto es menos compleja y requiere la configuración inicial menor que el método de controlador de implementación Web.</span><span class="sxs-lookup"><span data-stu-id="71cb4-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="71cb4-126">También debe configurar el servidor de base de datos para admitir la implementación y acceso remoto.</span><span class="sxs-lookup"><span data-stu-id="71cb4-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="71cb4-127">Estos temas ofrece toda la información que necesita para completar estas tareas:</span><span class="sxs-lookup"><span data-stu-id="71cb4-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="71cb4-128">[Configurar un servidor Web de publicación (agente remoto) de implementación Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="71cb4-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="71cb4-129">En este tema se describe cómo crear un servidor web que es compatible con Web Deploy publicando, utilizando el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="71cb4-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="71cb4-130">[Configurar un servidor de base de datos de publicación de implementación Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="71cb4-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="71cb4-131">En este tema se describe cómo configurar un servidor de base de datos para admitir la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2 y acceso remoto.</span><span class="sxs-lookup"><span data-stu-id="71cb4-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="71cb4-132">Información adicional</span><span class="sxs-lookup"><span data-stu-id="71cb4-132">Further Reading</span></span>

<span data-ttu-id="71cb4-133">Para obtener instrucciones acerca de cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configuración de un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="71cb4-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="71cb4-134">Para obtener instrucciones acerca de cómo configurar un entorno de producción típicos, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="71cb4-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71cb4-135">[Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Siguiente](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="71cb4-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
