---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurar entornos de servidor para la implementación Web | Microsoft Docs
author: jrjlee
description: Este tutorial le mostrará cómo configurar entornos de servidor para compatibilidad con un solo clic, o automatizada, la implementación del sitio Web y la publicación en diversos escenario diferente...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835568"
---
<a name="configuring-server-environments-for-web-deployment"></a>Configurar entornos de servidor para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo configurar entornos de servidor para compatibilidad con un solo clic, o automatizada, la implementación del sitio Web y la publicación en varios escenarios diferentes. El tutorial incluye temas para guiarle por completar varias tareas, como la configuración de un servidor web para admitir los enfoques específicos para la implementación y la configuración de una granja de servidores Web Farm Framework (WFF), junto con información general basadas en escenarios que proporcionan instrucciones de alto nivel-to-end.
> 
> El tutorial usa se describe en el escenario de implementación de Fabrikam, Inc. [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) como punto de referencia para ver ejemplos y la infraestructura de red.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


En este tutorial se incluye en estos temas:

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

El primer tema, [elegir el enfoque de derecha a la implementación Web](choosing-the-right-approach-to-web-deployment.md), se describen los principales enfoques que puede usar para publicar aplicaciones web mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) 2.0. También identifica los escenarios que se asignan a cada enfoque. Desde aquí, el tema de cada escenario proporciona una descripción general de las tareas que necesita para completar e identifica los temas que necesitará para trabajar a través que le ayudarán a completar estas tareas.

Si está usando el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md) para compilar e implementar la solución, el tema final, [configurar propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md), se describe cómo configurar los archivos de proyecto específicos del entorno para la implementación en distintos entornos de destino.

## <a name="key-technologies"></a>Tecnologías clave

En este tutorial se centra en cómo usar estos productos y tecnologías para admitir la implementación de web:

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- Servicio de administración de IIS Web (WMSvc)

El tutorial también trata sobre el uso de Windows Server 2008 R2, SQL Server 2008 R2, 4.0 de ASP.NET y ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales de implementación web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de Microsoft Build Engine (MSBuild), la canalización de publicación de Web, Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar Team Foundation Server (TFS) para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua (CI) y desencadene manualmente las implementaciones de compilaciones concretas.
- [Implementación Web de empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de implementación y tomar las aplicaciones web sin conexión durante el proceso de implementación .

> [!div class="step-by-step"]
> [Siguiente](choosing-the-right-approach-to-web-deployment.md)
