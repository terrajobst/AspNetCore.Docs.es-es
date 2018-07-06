---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Implementación Web de empresa avanzada | Microsoft Docs
author: jrjlee
description: Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa. Para un translati italiano...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: adadd921a96b5643eb54cd6f6384ef881ca6ba50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835087"
---
<a name="advanced-enterprise-web-deployment"></a>Implementación Web de empresa avanzada
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Esto forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

El escenario de alto nivel para estos tutoriales se describe en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Se recomienda que revise este tema antes de comenzar este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

- Cada uno de los temas de este tutorial es independiente entre sí y soluciona un problema que se produce en los escenarios de implementación de enterprise o un desafío particular. No es necesario trabajar con estos temas en ningún orden concreto. Sin embargo, este tutorial trata algunas tareas avanzadas. Por lo tanto, debe familiarizarse con los conceptos y técnicas que la [implementación Web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial trata con el fin de obtener el máximo beneficio de este contenido.
- En este tutorial se incluye en estos temas:
- [Realización de una implementación "What If"](performing-a-what-if-deployment.md). En muchos escenarios, desea determinar el impacto de una implementación propuesto en un entorno de destino o cualquier contenido existente antes de realizar los cambios. Este tema describe cómo puede ejecutar una implementación "what if" para generar archivos de registro y los scripts de actualización de base de datos como si hubiera implementado contenido en un entorno de destino, sin realizar cambios reales. Analizar estos recursos puede ayudarle a identificar problemas potenciales antes de una implementación activa.
- [Personalización de las implementaciones de base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Al implementar un proyecto de base de datos en varios destinos, a menudo desea personalizar las propiedades de implementación para cada entorno de destino. Por ejemplo, en entornos de prueba normalmente vuelva a crear la base de datos en cada implementación, mientras que en los entornos de ensayo o producción sería mucho más probable que realice actualizaciones incrementales para conservar los datos. Este tema describe cómo puede incorporar estos cambios de propiedad en la lógica de implementación mediante la creación de un archivo de configuración (.sqldeployment) de la implementación específica del entorno para cada entorno de destino.
- Implementación de las pertenencias a roles de base de datos en entornos de prueba. Cuando se vuelve a crear una base de datos en cada implementación&#x2014;por ejemplo, como parte de una integración continua (CI), compilar e implementar en un entorno de prueba&#x2014;normalmente necesitará configurar las pertenencias a roles de base de datos cada vez. Por ejemplo, normalmente deberá conceder permisos a la identidad del grupo de aplicaciones asociado con la aplicación web. Este tema describe cómo puede automatizar este proceso mediante la adición de un script posterior a la implementación de SQL para la lógica de implementación.
- [Implementación de bases de datos de pertenencia en entornos empresariales](deploying-membership-databases-to-enterprise-environments.md). Las bases de datos de pertenencia ASP.NET tienen varias características que pueden complicar el proceso de implementación. Por ejemplo, una implementación solo de esquema dejará la base de datos en un estado no operativo. En la mayoría de los escenarios, es preferible crear una base de datos de pertenencia directamente en cada entorno de destino. Sin embargo, si es necesario que implementar una base de datos de pertenencia, en este tema se describe algunos de los enfoques que puede usar para cumplir los desafíos inherentes.
- [Excluir archivos y carpetas de implementación](excluding-files-and-folders-from-deployment.md). En algunos escenarios, desea adaptar el contenido del paquete web en entornos de destino específico. Por ejemplo, es posible que desee incluir versiones completas de las bibliotecas de JavaScript cuando se implementa en un entorno de prueba, para admitir la depuración del lado cliente, pero usar versiones reducidas de las bibliotecas cuando se implementa en un entorno de ensayo o producción. Este tema describe cómo puede excluir determinados archivos y carpetas desde el proceso de creación del paquete.
- [Desconectar aplicaciones Web con Web implementarán](taking-web-applications-offline-with-web-deploy.md). Al implementar soluciones en un entorno de ensayo o producción, a menudo desee aprovechar las aplicaciones web sin conexión para la duración del proceso de implementación. Este tema describe cómo se puede agregar un *aplicación\_offline.htm* del archivo a la aplicación web al principio del proceso de implementación y lo elimina al final. Mientras el *aplicación\_offline.htm* archivo está en su lugar, cualquier usuario que vaya a la aplicación web se redirijan automáticamente a la *aplicación\_offline.htm* archivo.
- [Ejecutar secuencias de comandos de Windows PowerShell de MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muchos escenarios de implementación, requieren acciones posteriores a la implementación más complejas, como agregar orígenes de eventos personalizados en el registro o configurar la replicación entre instancias de SQL Server. Estas acciones se realizan a menudo a través de scripts de Windows PowerShell. Este tema describe cómo ejecutar scripts de Windows PowerShell desde un archivo de proyecto de Microsoft Build Engine (MSBuild) como parte del proceso de compilación e implementación.
- [Solucionar problemas del proceso de empaquetado](troubleshooting-the-packaging-process.md). La canalización de publicación de Web (WPP) define una propiedad de MSBuild denominada **EnablePackageProcessLoggingAndAssert** que puede usar para generar información detallada sobre el proceso de empaquetado de proyectos de aplicación web. Este tema describe lo que hace la propiedad y cómo usarlo.

## <a name="key-technologies"></a>Tecnologías clave

En este tutorial se centra en cómo usar estos productos y tecnologías para admitir las compilación automatizada y la implementación de web:

- Visual Studio 2010 y Team Foundation Server (TFS) 2010
- MSBuild y TFS Team Build
- Internet Information Services (IIS) 7.5
- Herramienta de implementación Web de IIS (Web Deploy) 2.1
- La utilidad de implementación de la base de datos de VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales de implementación web de escala empresarial. Estos son otros tutoriales de la serie:

- [Implementación de aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, el WPP, Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar los servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación del paquete de web remoto mediante el servicio de agente de implementación Web (el agente remoto) o el controlador de implementación Web e implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y desencadene manualmente las implementaciones de compilaciones concretas.

> [!div class="step-by-step"]
> [Siguiente](performing-a-what-if-deployment.md)
