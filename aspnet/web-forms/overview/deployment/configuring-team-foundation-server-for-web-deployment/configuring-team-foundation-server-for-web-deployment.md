---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "Configurar Team Foundation Server para la implementación Web | Documentos de Microsoft"
author: jrjlee
description: "Este tutorial le mostrará cómo configurar Team Foundation Server (TFS) 2010 para compilar soluciones e implementar contenido web en varios entornos de destino. Esto..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Configurar Team Foundation Server para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo configurar Team Foundation Server (TFS) 2010 para compilar soluciones e implementar contenido web en varios entornos de destino. Esto incluye los escenarios de integración continua (CI), en el que implementar contenido automáticamente cada vez que un programador que realiza un cambio. También puede incluir los escenarios de desencadenador manual, donde un administrador puede desear para desencadenar la implementación de una compilación concreta en un entorno de ensayo una vez que la compilación se ha comprobado y validado en el entorno de prueba. Los temas de este tutorial le guiará a través del proceso de configuración todo, incluidos:
> 
> - Cómo crear un nuevo proyecto de equipo en TFS.
> - Cómo agregar contenido a control de código fuente.
> - Cómo configurar un servidor de compilación para admitir elementos de configuración e implementación.
> - Cómo crear una definición de compilación que incluye la lógica de implementación.
> - Cómo configurar permisos para la implementación automatizada.
> 
> Para una traducción de italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).


Este tutorial se supone que ha instalado TFS 2010 y ha creado una colección de proyectos de equipo como parte del proceso de configuración inicial. El [Guía de instalación de Team Foundation para Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) proporciona una guía completa sobre estas tareas.

## <a name="context"></a>Contexto

Esto forma parte de una serie de tutoriales en función de los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución & #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

Se describe el escenario de alto nivel para estos tutoriales en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Se recomienda que revise este tema antes de empezar a trabajar en este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

Si se trata de la primera vez que ha realizado las tareas descritas en este tutorial, o si desea seguir los ejemplos utilizando la solución de ejemplo, debe trabajar a través de los temas del tutorial en orden. Como alternativa, puede utilizar temas individuales como guía para tareas específicas. Este tutorial incluye en estos temas:

- [Crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md). Un proyecto de equipo es la unidad básica de control de código fuente, la administración de procesos y la compilación en TFS. Debe crear un proyecto de equipo para poder agregar contenido a control de código fuente o crear definiciones de compilación.
- [Agregar contenido a Control de código fuente](adding-content-to-source-control.md). Una vez que haya creado un proyecto de equipo, puede empezar a agregar contenido al control de código fuente. Debe agregar los proyectos y soluciones, junto con las dependencias externas, antes de empezar a configurar compilaciones.
- [Configurar un TFS el servidor de compilación para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md). Si desea generar el contenido del proyecto de equipo, debe configurar un servidor de compilación. En la mayoría de los casos, esto debe estar en un equipo independiente de la instalación de TFS. Para configurar un servidor de compilación, debe instalar y configurar el servicio de compilación TFS, instale Visual Studio 2010, crear controladores de compilación y agentes de compilación, instalar los productos o componentes que necesita el código con el fin de generar correctamente e instalar el Herramienta de implementación de Web de servicios de Internet Information Server (IIS) (Web Deploy).
- [Crear una definición de compilación que admite la implementación](creating-a-build-definition-that-supports-deployment.md). Para poder empezar Queue o desencadena de compilaciones en TFS, debe crear al menos una definición de compilación para el proyecto de equipo. La definición de compilación define todos los aspectos de la compilación, incluidos qué elementos deben incluirse en la compilación, qué debe desencadenar la compilación y dónde Team Build debería enviar los resultados de compilación. Puede configurar una definición de compilación para ejecutar archivos de proyecto personalizados de Microsoft Build Engine (MSBuild), que permite incluir lógica de implementación en las compilaciones automatizadas.
- [Implementar una compilación concreta](deploying-a-specific-build.md). En muchos escenarios, debe tener implementar una compilación concreta, en lugar de la compilación más reciente, en un entorno de destino. En este caso, puede configurar una definición de compilación que distribuye contenido desde una carpeta de entrega específica.
- [Configurar permisos de equipo de implementación de compilaciones](configuring-permissions-for-team-build-deployment.md). Si es el servicio de compilación implementar contenido como parte de un proceso automatizado de compilación, deberá conceder permisos distintos para la cuenta de servicio de compilación en los servidores web de destino y los servidores de base de datos.

## <a name="key-technologies"></a>Tecnologías de clave

Este tutorial se centra en cómo utilizar estos productos y tecnologías para admitir la compilación automatizada y la implementación web:

- Visual Studio Team Foundation Server 2010
- Team Build y con MSBuild
- Web Deploy

El tutorial también se mencionan en el uso de ASP.NET MVC 3, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 y Windows Server 2008 R2.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales en la implementación de web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido de Introducción proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación de Web (WPP), Web Deploy y otras tecnologías relacionadas. Se explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación de paquete de web remoto mediante el servicio Web Deployment Agent (agente remoto) o el controlador de implementar de Web y la implementación de la base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar las aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Avanzada de implementación Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de la base de datos para varios entornos, excluir archivos y carpetas de implementación y dejar las aplicaciones de web sin conexión durante el proceso de implementación .

>[!div class="step-by-step"]
[Siguiente](creating-a-team-project-in-tfs.md)
