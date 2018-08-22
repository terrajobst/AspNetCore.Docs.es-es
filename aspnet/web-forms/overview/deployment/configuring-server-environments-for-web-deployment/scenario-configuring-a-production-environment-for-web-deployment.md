---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Escenario: Configurar un entorno de producción para la implementación Web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que necesita para completar con el fin de configurar un proceso similar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3627d18e1b102c574726282780239464be04c04b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828149"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Escenario: Configurar un entorno de producción para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que necesita para completar con el fin de configurar un entorno similar.


El entorno de producción es el destino final de una aplicación web o un sitio Web. En este punto, la aplicación ha sido a través de las pruebas, se ha implementado en un entorno de ensayo y está lista "." Las características de un entorno de producción pueden variar enormemente según la naturaleza y el propósito del contenido web, el tamaño de su organización, la audiencia de destino y muchos otros factores. En un escenario de escala empresarial, el entorno de producción puede tener estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de base de datos, a menudo con agrupación en clústeres de conmutación por error y la creación de reflejo de base de datos.
- Si el entorno está orientado a Internet, es probable que se pueda segregar desde la red interna. Puede ser en una subred diferente en una red perimetral, es posible en un dominio diferente y es posible en una infraestructura de red totalmente diferente.
- Los desarrolladores y las cuentas de procesos de servidor de compilación son muy poco probable que tenga privilegios de administrador en los servidores de producción.
- Cambios en las aplicaciones se implementan de forma menos frecuente que las pruebas o implementaciones de ensayo.

> [!NOTE]
> Escalar horizontalmente una implementación de la base de datos en varios servidores está fuera del ámbito de este tutorial. Para obtener más información sobre este tema, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un servidor de Team Build incluye definiciones de compilación que permiten a los usuarios compilar la solución Contact Manager e implementarlo en un entorno de ensayo en un solo paso. Cuando la aplicación está lista para implementarse en producción, debido a las restricciones impuestas por los requisitos de seguridad y la infraestructura de red, el Administrador de entorno de producción debe copiar el paquete web en un servidor web de producción e importe manualmente él a través del Administrador de Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Introducción a la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- Debido a restricciones de seguridad y la configuración de red, no se puede configurar el entorno de producción para admitir la implementación automatizada o de un solo clic. Implementación sin conexión es el único enfoque viable en este escenario.
- El entorno de producción incluye varios servidores web, por lo que puede usar Web Farm Framework (WFF) para crear una granja de servidores. Con este enfoque, el administrador sólo necesita importar la aplicación en un servidor web (el servidor principal) y WFF replicará la implementación en todos los demás servidores de web en el entorno de producción.

Estos temas proporcionan toda la información que necesita para completar estas tareas:

- [Crear una granja de servidores con el marco de granja de servidores Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tema describe cómo crear y configurar una granja de servidores con WFF, para que los productos de la plataforma web y componentes, opciones de configuración y los sitios Web y aplicaciones que se replican entre varios servidores web con equilibrio de carga.
- [Configurar un servidor Web de publicación (implementación sin conexión) de la implementación de Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Este tema describe cómo crear a un servidor web que permite a los administradores importarán e implementación de paquetes de web manualmente, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de la implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tema describe cómo configurar un servidor de base de datos para admitir el acceso remoto y la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar un entorno de prueba del desarrollador típico, consulte [escenario: configurar un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones sobre cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configurar un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Siguiente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
