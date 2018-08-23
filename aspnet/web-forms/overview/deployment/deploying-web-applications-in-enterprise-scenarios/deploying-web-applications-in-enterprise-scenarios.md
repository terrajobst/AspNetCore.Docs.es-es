---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Implementación de aplicaciones Web en escenarios empresariales mediante Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Este conjunto de tutoriales describe herramientas y técnicas que puede usar para implementar aplicaciones web en distintos escenarios de empresa. Se explica cómo aprovechar al máximo...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: fa06538dcd9e087df52a76588e084a527867bf84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828592"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implementación de aplicaciones Web en escenarios empresariales mediante Visual Studio 2010
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriales describe herramientas y técnicas que puede usar para implementar aplicaciones web en distintos escenarios de empresa. Se explica cómo hacer mejor uso de tecnologías como Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, la herramienta de implementación Web de IIS (Web Deploy), la Web Farm Framework (WFF) y utilidades como VSDBCMD.exe a simplificar y administrar el proceso de implementación. Incluye información general conceptual y orientación orientados a tareas que le ayudarán a:
> 
> - Revise y establecer los requisitos de implementación para una aplicación web de escala empresarial.
> - Configuración de pruebas, ensayo y producción entornos de servidor web para admitir la implementación web.
> - Configurar los procesos de integración continua (CI) de Team Foundation Server (TFS) para admitir la implementación web automatizados.
> - Implementar aplicaciones web a escala empresarial en entornos de servidor diferente con diferentes requisitos y restricciones.
> - Implementar los cambios en las aplicaciones web que se ejecutan en distintos entornos de servidor.
> 
> > [!NOTE]
> > Aunque estos tutoriales describen el uso de TFS como un servidor CI, la orientación fácilmente está adaptada a cualquier servidor CI. No es necesario un conocimiento detallado de TFS para entender y aprovechar los tutoriales.
> 
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Acerca de los autores

Jason Lee es un tecnólogo principal con [contenido maestro](http://www.contentmaster.com/) donde ha trabajado con productos y tecnologías, especialmente en SharePoint y ASP.NET, Microsoft durante varios años. Jason tiene un doctorado en informática y actualmente MCPD y MCTS certificadas. Puede leer el blog técnico de Jason en [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry es un tecnólogo principal con [contenido maestro](http://www.contentmaster.com/) que ha escrito artículos, documentación del SDK, presentaciones de PowerPoint y cursos de formación presencial y en línea durante su carrera. Un miembro del equipo de documentación de ASP.NET original, ha trabajado con las tecnologías web de Microsoft para más de una década.

## <a name="target-audience"></a>Audiencia de destino

Este conjunto de tutoriales es para desarrolladores de aplicaciones web ASP.NET y los arquitectos de soluciones que usan Visual Studio 2010 para crear aplicaciones web a escala empresarial. Para obtener el máximo provecho del contenido, debe estar familiarizado con el uso de Visual Studio 2010 y tiene cierta familiaridad básica con TFS, junto con un conocimiento de las tecnologías de plataforma web de Microsoft, como ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Servidor y los proyectos de base de datos de Visual Studio. Sin embargo, no es necesario estar familiarizado con las tecnologías y herramientas de implementación como si necesita saber cómo configurar los sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir los tutoriales y realizar las tareas que describen estos tutoriales, necesita instalar este software en el equipo de desarrollo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Para llevar a cabo los pasos de implementación descritos en estos tutoriales, necesitará tener acceso a los entornos de implementación de aplicación Web de ejemplo. Para obtener mejores resultados, estos entornos deben reflejar el modelo de implementación empresarial de su organización. A continuación, puede modificar los tutoriales proporcionados en esta documentación para reflejar los entornos de implementación y los requisitos de su organización.

## <a name="series-contents"></a>Contenido de la serie

En esta sección introductoria consta de dos temas adicionales. Están diseñadas para ofrecer cierto contexto más amplio para los tutoriales siguientes:

- [Implementación Web de empresa: Información general del escenario](enterprise-web-deployment-scenario-overview.md). En este tema se describe el escenario que sustenta cada uno de los tutoriales de esta serie. El escenario se centra en los requisitos de Application Lifecycle Management (ALM) de una compañía ficticia denominada Fabrikam, Inc. conforme se desarrolla una aplicación web de escala empresarial.
- [Application Lifecycle Management: Entornos de desarrollo a producción](application-lifecycle-management-from-development-to-production.md). En este tema se proporciona una introducción de alto nivel, to-end de un proceso de implementación. Ilustra cómo Fabrikam, Inc. se mueve a una aplicación de web ASP.NET de escala empresarial a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo.

La serie incluye cuatro conjuntos de tutorial. Cada uno se centra en aspectos diferentes de implementación web:

- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación de Web, Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar los servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación del paquete de web remoto mediante el servicio de agente de implementación Web (el "agente remoto") o el controlador de implementación Web e implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y describe cómo utilizar el WFF replicar aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y desencadene manualmente las implementaciones de compilaciones concretas.
- [Implementación Web de empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de implementación y tomar las aplicaciones web sin conexión durante el proceso de implementación .

## <a name="where-to-start"></a>Por dónde comenzar

Este conjunto de tutoriales usa una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar una implementación de referencia y para proporcionar a las tareas y los tutoriales de un contexto común. El tema siguiente, [implementación Web de empresa: información general del escenario](enterprise-web-deployment-scenario-overview.md), presenta el escenario y la solución de ejemplo. Desde allí puede realizar los tutoriales y temas que más se aproximen a sus necesidades.

> [!div class="step-by-step"]
> [Siguiente](enterprise-web-deployment-scenario-overview.md)
