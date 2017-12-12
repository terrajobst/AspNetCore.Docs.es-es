---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Implementar aplicaciones Web en escenarios empresariales mediante Visual Studio 2010 | Documentos de Microsoft
author: jrjlee
description: "Este conjunto de tutoriales describe herramientas y técnicas que puede usar para implementar aplicaciones web en distintos escenarios de empresa. Se explica cómo realizar un mejor uso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implementar aplicaciones Web en escenarios empresariales mediante Visual Studio 2010
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriales describe herramientas y técnicas que puede usar para implementar aplicaciones web en distintos escenarios de empresa. Se explica cómo hacer el mejor uso de tecnologías, como Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, la herramienta de implementación Web de IIS (Web Deploy), Web Farm Framework (WFF) y utilidades como VSDBCMD.exe a simplificar y administrar el proceso de implementación. Incluye información general conceptual y orientación orientados a tareas que le ayudará a:
> 
> - Revise y establecer los requisitos de implementación para una aplicación web de escala empresarial.
> - Configurar la prueba, ensayo y producción entornos de servidor web para admitir la implementación web.
> - Configurar procesos de integración continua (CI) de Team Foundation Server (TFS) para permitir la implementación de web automatizadas.
> - Implementar aplicaciones web a escala empresarial para distintos entornos de servidor con diferentes requisitos y restricciones.
> - Implementar cambios en las aplicaciones web que se ejecutan en distintos entornos de servidor.
> 
> > [!NOTE]
> > Mientras estos tutoriales describen el uso de TFS como un servidor de integración continua, las instrucciones se adaptación fácilmente a cualquier servidor de integración continua. No es necesario un conocimiento detallado de TFS para entender y aprovechar los tutoriales.
> 
> 
> Para una traducción de italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Acerca de los autores

Jason Lee es un entidad de seguridad técnico con [contenido Master](http://www.contentmaster.com/) donde ha trabajado con productos y tecnologías, especialmente en SharePoint y ASP.NET, Microsoft desde hace varios años. Jason tiene un doctorado en informática y actualmente MCPD y MCT certificados. Puede leer el blog técnica de Jason en [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry es un técnico de entidad de seguridad con [contenido Master](http://www.contentmaster.com/) que ha escrito notas del producto, documentación del SDK, presentaciones de PowerPoint y cursos instructor y en línea durante su carrera. Un miembro original del equipo de documentación de ASP.NET, ha trabajado con las tecnologías web de Microsoft para más de una década.

## <a name="target-audience"></a>Audiencia de destino

Este conjunto de tutoriales es para desarrolladores de aplicaciones web ASP.NET y arquitectos de soluciones que usan Visual Studio 2010 para crear aplicaciones web a escala empresarial. Para obtener el máximo partido del contenido, debe estar familiarizado con el uso de Visual Studio 2010 y tiene un conocimiento básico de TFS, junto con un conocimiento de las tecnologías de plataforma web de Microsoft como ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Servidor y proyectos de base de datos de Visual Studio. Sin embargo, es necesario estar familiarizado con las tecnologías y herramientas de implementación o necesita saber cómo configurar los sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir los tutoriales y realizar las tareas que describen estos tutoriales, debe instalar este software en el equipo de desarrollo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Para llevar a cabo los pasos de implementación descritos en estos tutoriales, debe tener acceso a los entornos de implementación de aplicación Web de ejemplo. Para obtener mejores resultados, estos entornos deben reflejar el patrón de implementación de empresa de su organización. A continuación, puede modificar los tutoriales proporcionados en esta documentación para reflejar los entornos de implementación y los requisitos de su propia organización.

## <a name="series-contents"></a>Contenido de la serie

En esta sección introductoria consta de dos temas adicionales. Éstos están diseñados para proporcionar algún contexto más amplio para los tutoriales siguientes:

- [Implementación Web de empresa: Información general del escenario](enterprise-web-deployment-scenario-overview.md). En este tema se describe el escenario que es la base de cada uno de los tutoriales de esta serie. El escenario se centra en los requisitos de Application Lifecycle Management (ALM) de una compañía ficticia denominada Fabrikam, Inc. conforme se desarrolla una aplicación web de escala empresarial.
- [Application Lifecycle Management: Entornos de desarrollo a producción](application-lifecycle-management-from-development-to-production.md). Este tema proporciona una introducción de alto nivel, to-end de un proceso de implementación. Ilustra cómo Fabrikam, Inc. se mueve a una aplicación de web ASP.NET en la escala de empresa a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo.

La serie incluye cuatro conjuntos de tutorial. Cada uno de ellos se centra en diferentes aspectos de implementación web:

- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación de Web, Web Deploy y otras tecnologías relacionadas. Se explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación de paquete de web remoto mediante el servicio de agente de implementación Web (el "agente remoto") o el controlador de implementar de Web y la implementación de la base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo utilizar el WFF para replicar las aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua y desencadena manualmente las implementaciones de compilaciones concretas.
- [Avanzada de implementación Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de la base de datos para varios entornos, excluir archivos y carpetas de implementación y dejar las aplicaciones de web sin conexión durante el proceso de implementación .

## <a name="where-to-start"></a>Dónde empezar

Este conjunto de tutoriales utiliza una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar una implementación de referencia y para proporcionar a las tareas y los tutoriales comparten un contexto común. El siguiente tema, [implementación Web de empresa: información general del escenario](enterprise-web-deployment-scenario-overview.md), presenta el escenario y la solución de ejemplo. Desde allí, puede trabajar con los tutoriales y los temas que mejor coincida con sus necesidades.

>[!div class="step-by-step"]
[Siguiente](enterprise-web-deployment-scenario-overview.md)
