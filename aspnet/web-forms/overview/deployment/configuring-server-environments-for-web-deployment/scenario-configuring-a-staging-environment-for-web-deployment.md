---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "Escenario: Configuración de un entorno de ensayo para la implementación Web | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe un escenario de implementación web típica para un entorno de ensayo y se explica las tareas que debe completar para configurar un env similar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Escenario: Configuración de un entorno de ensayo para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típica para un entorno de ensayo y se explica las tareas que debe completar para configurar un entorno similar.


Muchas organizaciones utilizan entornos de ensayo para obtener una vista previa de las actualizaciones de aplicaciones web o sitios Web. Esto da a personas dentro de la organización una oportunidad para explorar y revise la funcionalidad nueva o contenido antes de que el sitio "poner en marcha" o en otras palabras, se implementa en un entorno de producción. El entorno de ensayo está diseñado para replicar el entorno de producción lo más fielmente posible, con el fin de proporcionar una vista previa realista. Normalmente, este tipo de entorno de ensayo tiene estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de base de datos, a menudo con clústeres de conmutación por error y la creación de reflejo de base de datos.
- Las aplicaciones pueden implementarse manualmente por un equipo de desarrollo o automáticamente por un servidor de Team Build.
- Las cuentas de proceso que implementación las aplicaciones o los usuarios no suelen tener privilegios de administrador en los servidores de ensayo.
- Cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir paso a paso o la implementación automatizada.

> [!NOTE]
> Escalar horizontalmente una implementación de base de datos entre varios servidores queda fuera del ámbito de este tutorial. Para obtener más información sobre esta área, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) administra la solución póngase en contacto con el administrador. El administrador TFS, Rob Walters, ha creado una definición de compilación que permite a los programadores activar una implementación en el entorno de ensayo según sea necesario.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Tenga en cuenta que en la mayoría de los casos, no necesariamente desea implementar la compilación más reciente en el entorno de ensayo. En su lugar, es mucho más probable que desee implementar una compilación concreta que ya se ha sometido a comprobación en el entorno de prueba y validación.

## <a name="solution-overview"></a>Introducción a la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- La cuenta de usuario o proceso que realiza la implementación no tendrá privilegios de administrador en los servidores de ensayo, por lo que los servidores web de ensayo deben admitir la implementación sin privilegios de administrador. Por lo tanto, debe configurar los servidores web de ensayo para usar el controlador de implementación Web en lugar de con el agente remoto.
- El entorno de ensayo incluye varios servidores web, pero necesita admitir la implementación de un solo clic o automatizada, por lo que necesitará usar Web Farm Framework (WFF) para crear una granja de servidores. Con este enfoque, puede implementar una aplicación en un servidor web (el servidor principal) y WFF replicará la implementación en todos los demás servidores de web en el entorno de ensayo.
- El usuario o la cuenta de proceso que realiza la implementación debe tener permisos para crear bases de datos. Por lo tanto, debe agregar la cuenta a la **dbcreator** rol de servidor en el servidor de base de datos, además de configurar el servidor de base de datos para admitir el acceso remoto e implementación.

Estos temas ofrece toda la información que necesita para completar estas tareas:

- [Crear una granja de servidores con el marco de trabajo de la granja de servidores Web](creating-a-server-farm-with-the-web-farm-framework.md). En este tema se describe cómo crear y configurar una granja de servidores con WFF, para que los componentes y productos de la plataforma web, opciones de configuración y sitios Web y aplicaciones se replican a través de varios servidores web con equilibrio de carga.
- [Configurar un servidor Web de publicación de implementación Web (Web Deploy controlador)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). En este tema se describe cómo crear un servidor web que es compatible con Web Deploy publicando, utilizando el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2 y acceso remoto.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones acerca de cómo configurar un entorno de prueba de desarrollo típicas, consulte [escenario: configuración de un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones acerca de cómo configurar un entorno de producción típicos, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Anterior](scenario-configuring-a-test-environment-for-web-deployment.md)
[Siguiente](scenario-configuring-a-production-environment-for-web-deployment.md)
