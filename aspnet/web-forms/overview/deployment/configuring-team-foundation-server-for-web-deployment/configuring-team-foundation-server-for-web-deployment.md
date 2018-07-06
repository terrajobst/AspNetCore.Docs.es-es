---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configurar Team Foundation Server para la implementación Web | Microsoft Docs
author: jrjlee
description: Este tutorial le mostrará cómo configurar Team Foundation Server (TFS) 2010 para crear soluciones e implementar contenido web en distintos entornos de destino. Esto...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0155f8dc4ca05a91ed8921f83aa6fa1b0b59c1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829692"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Configurar Team Foundation Server para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo configurar Team Foundation Server (TFS) 2010 para crear soluciones e implementar contenido web en distintos entornos de destino. Esto incluye escenarios de integración continua (CI), donde distribuir contenido automáticamente cada vez que un desarrollador realiza un cambio. También puede incluir escenarios de desencadenador manual, donde un administrador puede desear para desencadenar la implementación de una compilación concreta en un entorno de ensayo una vez que haya comprobado y validado en el entorno de prueba de la compilación. Los temas de este tutorial le guiará a través del proceso de configuración completo, incluyendo:
> 
> - Cómo crear un nuevo proyecto de equipo en TFS.
> - Cómo agregar contenido al control de código fuente.
> - Cómo configurar un servidor de compilación para admitir la integración continua e implementación.
> - Cómo crear una definición de compilación que incluye la lógica de implementación.
> - Cómo configurar permisos para la implementación automatizada.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


En este tutorial se da por supuesto que ha instalado TFS 2010 y ha creado una colección de proyectos de equipo como parte del proceso de configuración inicial. El [Guía de instalación de Team Foundation para Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) proporciona una orientación completa sobre estas tareas.

## <a name="context"></a>Contexto

Esto forma parte de una serie de tutoriales en función de los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

El escenario de alto nivel para estos tutoriales se describe en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Se recomienda que revise este tema antes de comenzar este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

Si se trata de la primera vez que ha realizado las tareas descritas en este tutorial, o si desea seguir los ejemplos de uso de la solución de ejemplo, debería funcionar a través de los temas del tutorial en orden. Como alternativa, puede usar los temas individuales como guía para tareas específicas. En este tutorial se incluye en estos temas:

- [Crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md). Un proyecto de equipo es la unidad básica de control de código fuente, administración de procesos y compilación en TFS. Deberá crear un proyecto de equipo para poder agregar contenido al control de código fuente o crear definiciones de compilación.
- [Agregar contenido al Control de código fuente](adding-content-to-source-control.md). Una vez que haya creado un proyecto de equipo, puede empezar a agregar contenido al control de código fuente. Deberá agregar los proyectos y soluciones, y cualquier dependencia externa antes de empezar a configurar compilaciones.
- [Configurar un TFS el servidor de compilación para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md). Si desea generar el contenido del proyecto de equipo, deberá configurar un servidor de compilación. En la mayoría de los casos, esto debe estar en un equipo distinto de la instalación de TFS. Para configurar un servidor de compilación, deberá instalar y configurar el servicio de compilación TFS, instale Visual Studio 2010, crear controladores de compilación y agentes de compilación, instalar los productos o componentes que el código necesita para generar correctamente e instalar el Herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy).
- [Crear una definición de compilación que admite la implementación](creating-a-build-definition-that-supports-deployment.md). Antes de empezar a puesta en cola o desencadenando compilaciones en TFS, deberá crear al menos una definición de compilación del proyecto de equipo. La definición de compilación define todos los aspectos de la compilación, incluidos qué cosas se deben incluir en la compilación, qué debe desencadenar la compilación y dónde Team Build debe enviar los resultados de compilación. Puede configurar una definición de compilación para ejecutar archivos de proyecto personalizados de Microsoft Build Engine (MSBuild), que le permite incluir lógica de implementación en las compilaciones automatizadas.
- [Implementar una compilación concreta](deploying-a-specific-build.md). En muchos escenarios, desea implementar una compilación concreta, en lugar de la compilación más reciente, en un entorno de destino. En este caso, puede configurar una definición de compilación que se distribuye contenido desde una carpeta de entrega específica.
- [Configurar permisos para el equipo de implementación de compilaciones](configuring-permissions-for-team-build-deployment.md). Si el servicio de compilación es implementar el contenido como parte de un proceso de compilación automatizada, deberá conceder distintos permisos a la cuenta de servicio de compilación en los servidores web de destino y los servidores de base de datos.

## <a name="key-technologies"></a>Tecnologías clave

En este tutorial se centra en cómo usar estos productos y tecnologías para admitir las compilación automatizada y la implementación de web:

- Visual Studio Team Foundation Server 2010
- Team Build y MSBuild
- Web Deploy

El tutorial también se mencionan en el uso de ASP.NET MVC 3, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 y Windows Server 2008 R2.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales de implementación web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación de Web (WPP), Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar los servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación del paquete de web remoto mediante el servicio de agente de implementación Web (el agente remoto) o el controlador de implementación Web e implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Implementación Web de empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de implementación y tomar las aplicaciones web sin conexión durante el proceso de implementación .

> [!div class="step-by-step"]
> [Siguiente](creating-a-team-project-in-tfs.md)
