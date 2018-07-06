---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Escenario: Configurar un entorno de ensayo para la implementación Web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típica para un entorno de ensayo y se explica las tareas que necesita para completar con el fin de configurar un entorno similar...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9c0f12645f10820996a03c232d20ee87031aefaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820806"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Escenario: Configurar un entorno de ensayo para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típica para un entorno de ensayo y se explica las tareas que necesita para completar con el fin de configurar un entorno similar.


Muchas organizaciones utilizan entornos de ensayo para obtener una vista previa de las actualizaciones de aplicaciones web o sitios Web. Esto proporciona a las personas dentro de la organización una oportunidad para explorar y revise las nuevas funcionalidades o contenido antes de que el sitio "lanza" o en otras palabras, se implementa en un entorno de producción. El entorno de ensayo está diseñado para replicar el entorno de producción lo más fielmente posible, con el fin de proporcionar una vista previa realista. Normalmente, este tipo de entorno de ensayo tiene estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de base de datos, a menudo con agrupación en clústeres de conmutación por error y la creación de reflejo de base de datos.
- Las aplicaciones pueden implementarse manualmente por un equipo de desarrollo o automáticamente por un servidor de Team Build.
- Los usuarios o las cuentas de procesos que implementación las aplicaciones suelen tener privilegios de administrador en los servidores de ensayo.
- Cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir paso a paso o implementación automatizada.

> [!NOTE]
> Escalar horizontalmente una implementación de la base de datos en varios servidores está fuera del ámbito de este tutorial. Para obtener más información sobre este tema, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) administra la solución Contact Manager. El administrador TFS, Rob Walters, ha creado una definición de compilación que permite a los desarrolladores desencadenar una implementación en el entorno de ensayo según sea necesario.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Tenga en cuenta que en la mayoría de los casos, no necesariamente desea implementar la compilación más reciente en el entorno de ensayo. En su lugar, es mucho más probable que desee implementar una compilación concreta que ya se ha sometido a validación y comprobación del entorno de prueba.

## <a name="solution-overview"></a>Introducción a la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- La cuenta de usuario o proceso que realiza la implementación no tendrá privilegios de administrador en los servidores de ensayo, por lo que los servidores web de ensayo deben admitir la implementación sin privilegios de administrador. Por lo tanto, deberá configurar los servidores de web de ensayo para usar el controlador de implementación Web en lugar de con el agente remoto.
- El entorno de ensayo incluye varios servidores web, pero necesita admitir la implementación automatizada o de un solo clic, por lo que necesitará utilizar Web Farm Framework (WFF) para crear una granja de servidores. Con este enfoque, puede implementar una aplicación en un servidor web (el servidor principal) y WFF replicará la implementación en todos los demás servidores de web en el entorno de ensayo.
- El usuario o la cuenta de proceso que realiza la implementación debe tener permisos para crear bases de datos. Por lo tanto, deberá agregar la cuenta de la **dbcreator** rol de servidor en el servidor de base de datos, además de configurar el servidor de base de datos para admitir el acceso remoto y la implementación.

Estos temas proporcionan toda la información que necesita para completar estas tareas:

- [Crear una granja de servidores con el marco de granja de servidores Web](creating-a-server-farm-with-the-web-farm-framework.md). Este tema describe cómo crear y configurar una granja de servidores con WFF, para que los productos de la plataforma web y componentes, opciones de configuración y los sitios Web y aplicaciones que se replican entre varios servidores web con equilibrio de carga.
- [Configurar un servidor Web de publicación de la implementación Web (controlador de implementación Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Este tema describe cómo crear un servidor web que es compatible con Web Deploy de publicación, mediante el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de la implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tema describe cómo configurar un servidor de base de datos para admitir el acceso remoto y la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar un entorno de prueba del desarrollador típico, consulte [escenario: configurar un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones sobre cómo configurar un entorno de producción típica, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Siguiente](scenario-configuring-a-production-environment-for-web-deployment.md)
