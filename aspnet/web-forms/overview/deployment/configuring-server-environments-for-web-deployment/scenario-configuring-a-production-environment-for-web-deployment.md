---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Escenario: Configurar un entorno de producción para la implementación Web | Documentos de Microsoft'
author: jrjlee
description: En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que debe completar para configurar un proceso similar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Escenario: Configurar un entorno de producción para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que debe completar para configurar un entorno similar.


El entorno de producción es el destino final de una aplicación web o un sitio Web. Hasta este momento, la aplicación se ha sometido a pruebas, se ha implementado en un entorno de ensayo y está lista "esté activa." Las características de un entorno de producción pueden variar enormemente según la naturaleza y el propósito de su contenido web, el tamaño de su organización, la audiencia de destino y una gran cantidad de otros factores. En un escenario de escala empresarial, el entorno de producción puede tener estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de base de datos, a menudo con clústeres de conmutación por error y la creación de reflejo de base de datos.
- Si el entorno está orientado a Internet, es probable que se aísla de su red interna. Es posible en una subred diferente en una red perimetral, es posible en un dominio diferente y es posible en una infraestructura de red completamente diferente.
- Los desarrolladores y las cuentas de procesos de servidor de compilación son muy poco probable que tenga privilegios de administrador en los servidores de producción.
- Cambios en las aplicaciones se implementan con menos frecuencia de prueba o implementaciones de ensayo.

> [!NOTE]
> Escalar horizontalmente una implementación de base de datos entre varios servidores queda fuera del ámbito de este tutorial. Para obtener más información sobre esta área, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un servidor de Team Build incluye definiciones de compilación que permiten a los usuarios de compilar la solución póngase en contacto con el administrador e implementarlo en un entorno de ensayo en un solo paso. Cuando la aplicación está lista para implementarse en producción, debido a las restricciones impuestas por los requisitos de seguridad y la infraestructura de red, el administrador del entorno de producción debe copiar el paquete de web en un servidor web de producción e importe manualmente él a través del Administrador de Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Introducción a la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- Debido a restricciones de seguridad y la configuración de red, no puede configurar el entorno de producción para permitir la implementación de un solo clic o automatizada. Implementación sin conexión es el único enfoque viable en este escenario.
- El entorno de producción incluye varios servidores web, por lo que puede usar Web Farm Framework (WFF) para crear una granja de servidores. Con este enfoque, el administrador sólo tiene que importar la aplicación en un servidor web (el servidor principal) y WFF replicará la implementación en todos los demás servidores de web en el entorno de producción.

Estos temas ofrece toda la información que necesita para completar estas tareas:

- [Crear una granja de servidores con el marco de trabajo de la granja de servidores Web](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo crear y configurar una granja de servidores con WFF, para que los componentes y productos de la plataforma web, opciones de configuración y sitios Web y aplicaciones se replican a través de varios servidores web con equilibrio de carga.
- [Configurar un servidor Web de publicación (implementación sin conexión) de implementación Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). En este tema se describe cómo crear a un servidor web que permite a los administradores importarán e implementación paquetes de web de manualmente, desde una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos de publicación de implementación Web](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2 y acceso remoto.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones acerca de cómo configurar un entorno de prueba de desarrollo típicas, consulte [escenario: configuración de un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones acerca de cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configuración de un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Siguiente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
