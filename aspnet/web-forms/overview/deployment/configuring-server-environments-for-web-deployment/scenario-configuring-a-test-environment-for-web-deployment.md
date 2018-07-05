---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Escenario: Configurar un entorno de prueba para la implementación Web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típico para desarrolladores o entornos de prueba y se explican las tareas que necesita para completar con el fin de configurar un si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382499"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Escenario: Configurar un entorno de prueba para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típico para desarrolladores o entornos de prueba y se explican las tareas que necesita para completar con el fin de configurar un entorno similar.


Cuando los desarrolladores trabajan en aplicaciones web, a menudo se les otorga acceso a un entorno de servidor que pueden usar para probar los cambios en sus aplicaciones en una configuración realista. Este tipo de entorno de desarrollo o prueba normalmente tiene estas características:

- El entorno consta de un único servidor web y un servidor de base de datos única.
- Los desarrolladores suelen tengan privilegios de administrador en los servidores, para permitirle configurar el entorno de los requisitos de sus aplicaciones.
- Cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir paso a paso o implementación automatizada.

Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink es desarrollador en Fabrikam, Inc. Matt está trabajando en la solución Contact Manager y con regularidad debe implementar los cambios en un entorno de prueba. Matt es un administrador en el servidor web de prueba y el servidor de base de datos de prueba. Inicialmente, Matt debe ser capaz de implementar la solución en el entorno de prueba directamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Cuando el trabajo progresa y más desarrolladores Únase al equipo, la solución se configura para la integración continua (CI) en Team Foundation Server (TFS) de Contact Manager. Cada vez que un desarrollador protege contenido, debe Team Build compile la solución, ejecute cualquier prueba unitaria e implementar automáticamente la solución en el entorno de prueba.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Introducción a la solución

El entorno de prueba debe admitir paso a paso o automatizar la implementación desde un equipo remoto, por lo que tiene una opción de dos enfoques principales. Puede realizar lo siguiente:

- Configurar el servidor de prueba web para admitir la implementación mediante el servicio de agente de implementación Web (el "agente remoto").
- Configurar el servidor de prueba web para admitir la implementación mediante el controlador de Web Deploy.

> [!NOTE]
> También puede usar [implementar Web bajo demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (el "agente temp"). Esto es similar al enfoque de agente remoto en cuanto a los requisitos y restricciones.


En este caso, los desarrolladores tienen privilegios de administrador en los servidores de destino y el entorno de prueba no está sujeto a restricciones de seguridad estricta, por lo que es la elección lógica configurar el servidor de prueba web para admitir la implementación mediante el agente remoto. Esto es menos complejo y requiere la configuración inicial menor que el método de controlador de implementación Web. También deberá configurar el servidor de base de datos para admitir la implementación y el acceso remoto.

Estos temas proporcionan toda la información que necesita para completar estas tareas:

- [Configurar un servidor Web de publicación (agente remoto) de la implementación de Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Este tema describe cómo crear un servidor web que es compatible con Web Deploy de publicación, mediante el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de la implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tema describe cómo configurar un servidor de base de datos para admitir el acceso remoto y la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configurar un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obtener instrucciones sobre cómo configurar un entorno de producción típica, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Siguiente](scenario-configuring-a-staging-environment-for-web-deployment.md)
