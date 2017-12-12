---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "Escenario: Configurar un entorno de prueba para la implementación Web | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe un escenario de implementación web típica para desarrolladores o entornos de prueba y se explican las tareas que debe completar para configurar un si..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 008b9cd081152e6a378d0fa2e08497a6771fd9b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Escenario: Configurar un entorno de prueba para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típica para desarrolladores o entornos de prueba y se explican las tareas que debe completar para configurar un entorno similar.


Cuando los desarrolladores trabajan en aplicaciones web, a menudo ha otorgado acceso a un entorno de servidor que puede usar para probar sus aplicaciones los cambios en una opción realista. Normalmente, este tipo de entorno de desarrollo o prueba tiene estas características:

- El entorno consta de un único servidor web y un servidor de base de datos único.
- Los desarrolladores suelen tengan privilegios de administrador en los servidores, que les permiten configurar el entorno para los requisitos de sus aplicaciones.
- Cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir paso a paso o la implementación automatizada.

Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink es un desarrollador de Fabrikam, Inc. Matt estén trabajando en la solución póngase en contacto con el administrador y con regularidad debe implementar cambios en un entorno de prueba. Matt es un administrador en el servidor web de prueba y el servidor de base de datos de prueba. Inicialmente, Matt debe ser capaz de implementar la solución en el entorno de prueba directamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Como el trabajo progresa y los desarrolladores más unirán el equipo, el Administrador de contacto solución está configurada para la integración continua (CI) en Team Foundation Server (TFS). Cada vez que un desarrollador protege en el contenido, debe Team Build compilar la solución, ejecute cualquier prueba unitaria e implementar automáticamente la solución en el entorno de prueba.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Introducción a la solución

El entorno de prueba debe admitir paso a paso o automatizar la implementación desde un equipo remoto, por lo que tendrá una opción de dos enfoques principales. Puede realizar lo siguiente:

- Configurar el servidor web de prueba para permitir la implementación con el servicio de agente de implementación Web (el "agente remoto").
- Configurar el servidor web de prueba para permitir la implementación con el controlador de Web Deploy.

> [!NOTE]
> También puede usar [implementar Web a petición](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) (el "agente temp"). Esto es similar al enfoque de agente remoto en cuanto a requisitos y restricciones.


En este caso, los desarrolladores tienen privilegios de administrador en los servidores de destino y el entorno de prueba no está sujeta a restricciones de seguridad estricta, por lo que es la opción lógica configurar el servidor web de prueba para permitir la implementación con el agente remoto. Esto es menos compleja y requiere la configuración inicial menor que el método de controlador de implementación Web. También debe configurar el servidor de base de datos para admitir la implementación y acceso remoto.

Estos temas ofrece toda la información que necesita para completar estas tareas:

- [Configurar un servidor Web de publicación (agente remoto) de implementación Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). En este tema se describe cómo crear un servidor web que es compatible con Web Deploy publicando, utilizando el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2 y acceso remoto.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones acerca de cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configuración de un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obtener instrucciones acerca de cómo configurar un entorno de producción típicos, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Anterior](choosing-the-right-approach-to-web-deployment.md)
[Siguiente](scenario-configuring-a-staging-environment-for-web-deployment.md)
