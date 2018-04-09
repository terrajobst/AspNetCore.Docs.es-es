---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurar entornos de servidor para la implementación Web | Documentos de Microsoft
author: jrjlee
description: Este tutorial le mostrará cómo configurar entornos de servidor compatible con un solo clic, o automatizada, implementación de sitio Web y publicación en diversos escenario diferentes...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>Configurar entornos de servidor para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo configurar entornos de servidor para compatibilidad con un solo clic, o automatizada, la implementación del sitio Web y la publicación en varios escenarios diferentes. El tutorial incluye temas que le guíe a través de completar varias tareas, como la configuración de un servidor web para admitir enfoques específicos para la implementación y la configuración de una granja de servidores Web Farm Framework (WFF), junto con información general basada en escenario que proporcionan instrucciones de alto nivel-to-end.
> 
> El tutorial utiliza el escenario de implementación de Fabrikam, Inc. descrito en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) como punto de referencia para obtener ejemplos y la infraestructura de red.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Este tutorial incluye en estos temas:

- [Elegir el enfoque adecuado para la implementación web](choosing-the-right-approach-to-web-deployment.md)
- [Escenario: Configurar un entorno de prueba para la implementación web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Escenario: Configurar un entorno de ensayo para la implementación web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Escenario: Configurar un entorno de producción para la implementación web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configurar un servidor web para la publicación de la implementación web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configurar un servidor web para la publicación de la implementación web (controlador de implementación web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configurar un servidor web para la publicación de la implementación web (implementación sin conexión)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configurar un servidor de base de datos para la publicación de la implementación web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Crear una granja de servidores con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configurar las propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md)

El primer tema, [la elección del enfoque de derecha a la implementación de Web](choosing-the-right-approach-to-web-deployment.md), se describen los enfoques principales que puede usar para publicar aplicaciones web mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) 2.0. También se identifican los escenarios que se asignan a cada enfoque. Desde aquí, cada tema escenario ofrece una descripción general de las tareas que necesarias para completar e identifica los temas que necesite trabajar con para ayudarle a completar estas tareas.

Si está usando el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md) para compilar e implementar la solución, el tema final, [configurar propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md), se describe cómo configurar los archivos de proyecto específicas del entorno para la implementación en distintos entornos de destino.

## <a name="key-technologies"></a>Tecnologías de clave

Este tutorial se centra en cómo utilizar estos productos y tecnologías para admitir la implementación de web:

- IIS 7.5
- WebDeploy 2.x
- WFF 2.x
- Servicio de administración de IIS Web (WMSvc)

El tutorial también se mencionan en el uso de Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 y ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales en la implementación de web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido de Introducción proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de Microsoft Build Engine (MSBuild), la canalización de publicación de Web, Web Deploy y otras tecnologías relacionadas. Se explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar Team Foundation Server (TFS) para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua (CI) y se desencadena manualmente las implementaciones de compilaciones concretas.
- [Avanzada de implementación Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de la base de datos para varios entornos, excluir archivos y carpetas de implementación y dejar las aplicaciones de web sin conexión durante el proceso de implementación .

> [!div class="step-by-step"]
> [Siguiente](choosing-the-right-approach-to-web-deployment.md)
