---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Avanzada de implementación Web de empresa | Documentos de Microsoft
author: jrjlee
description: Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa. Para una translati italiano...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879938"
---
<a name="advanced-enterprise-web-deployment"></a>Implementación de Web de empresa avanzadas
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Esto forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

Se describe el escenario de alto nivel para estos tutoriales en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Se recomienda que revise este tema antes de empezar a trabajar en este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

- Cada uno de los temas de este tutorial es independiente entre sí y soluciona un problema que se produce en los escenarios de implementación de empresa o desafío particular. No es necesario para que funcione a través de estos temas en ningún orden concreto. Sin embargo, en este tutorial se trata algunas tareas avanzadas. Por lo tanto, debe familiarizarse con los conceptos y técnicas que los [implementación Web de la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial trata con el fin de obtener el máximo rendimiento de este contenido.
- Este tutorial incluye en estos temas:
- [Realizar una implementación de "¿Qué ocurre si"](performing-a-what-if-deployment.md). En muchos escenarios, desea determinar el impacto de una implementación propuesto en un entorno de destino o cualquier contenido existente antes de implementar los cambios. Este tema describe cómo ejecutar una implementación "¿Qué ocurre si" para generar archivos de registro y scripts de actualización de base de datos como si hubiera implementado contenido en un entorno de destino, sin realizar ningún cambio real. Analizar estos recursos puede ayudar a identificar posibles problemas antes de una implementación en vivo.
- [Personalización de las implementaciones de la base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Al implementar un proyecto de base de datos a varios destinos, a menudo desea personalizar las propiedades de implementación para cada entorno de destino. Por ejemplo, en entornos de prueba, se suele volver a la base de datos en cada implementación, mientras que en los entornos de ensayo o de producción sería mucho más probable que las actualizaciones incrementales para conservar los datos. Este tema describe cómo se pueden incorporar estos cambios de propiedad en la lógica de implementación mediante la creación de un archivo de configuración (.sqldeployment) de la implementación específica del entorno para cada entorno de destino.
- Implementación de las pertenencias a roles de base de datos en entornos de prueba. Cuando se vuelve a crear una base de datos en cada implementación&#x2014;por ejemplo, como parte de una integración continua (CI), generar e implementar en un entorno de prueba&#x2014;normalmente debe configurar las pertenencias a roles de base de datos cada vez. Por ejemplo, normalmente será necesario conceder permisos a la identidad del grupo de aplicaciones asociado a la aplicación web. Este tema describe cómo se puede automatizar este proceso mediante la adición de una secuencia de comandos SQL posterior a la implementación para la lógica de implementación.
- [Implementación de las bases de datos de pertenencia en entornos empresariales](deploying-membership-databases-to-enterprise-environments.md). Las bases de datos de pertenencia ASP.NET tienen varias características que pueden complicar el proceso de implementación. Por ejemplo, una implementación de solo esquema dejará la base de datos en un estado no operativo. En la mayoría de los escenarios, es preferible para crear una base de datos de pertenencia directamente en cada entorno de destino. Sin embargo, si tiene que implementar una base de datos de pertenencia, en este tema se describe algunos de los enfoques que puede usar para cumplir los desafíos inherentes.
- [Excluir archivos y carpetas de implementación](excluding-files-and-folders-from-deployment.md). En algunos escenarios, desea personalizar el contenido del paquete web a los entornos de destino específico. Por ejemplo, puede incluir las versiones completas de las bibliotecas de JavaScript cuando se implementa en un entorno de prueba, para admitir la depuración de cliente, pero usa reducidas versiones de las bibliotecas cuando se implementa en un entorno de ensayo o producción. Este tema describe cómo puede excluir determinados archivos y carpetas desde el proceso de creación de paquete.
- [Implementarán aplicaciones sin conexión de toma de Web con Web](taking-web-applications-offline-with-web-deploy.md). Al implementar las soluciones en un entorno de ensayo o de producción, a menudo querrá tomar las aplicaciones web sin conexión durante el proceso de implementación. En este tema se describe cómo agregar un *aplicación\_offline.htm* del archivo a la aplicación web al principio del proceso de implementación y quitar al final. Mientras el *aplicación\_offline.htm* archivo está en su lugar, cualquier usuario que vaya a la aplicación web se redirige automáticamente a la *aplicación\_offline.htm* archivo.
- [Ejecutar Scripts de Windows PowerShell de MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muchos escenarios de implementación requieren acciones posteriores a la implementación más complejas, como agregar orígenes de eventos personalizado en el registro o configurar la replicación entre instancias de SQL Server. Estas acciones se llevan a cabo a menudo a través de scripts de Windows PowerShell. En este tema se describe cómo ejecutar scripts de Windows PowerShell desde un archivo de proyecto de Microsoft Build Engine (MSBuild) como parte del proceso de compilación e implementación.
- [Solucionar problemas del proceso de empaquetado](troubleshooting-the-packaging-process.md). La canalización de publicación de Web (WPP) define una propiedad de MSBuild denominada **EnablePackageProcessLoggingAndAssert** que puede usar para generar información detallada sobre el proceso de empaquetado para proyectos de aplicación web. Este tema describe lo que hace la propiedad y cómo utilizarlo.

## <a name="key-technologies"></a>Tecnologías de clave

Este tutorial se centra en cómo utilizar estos productos y tecnologías para admitir la compilación automatizada y la implementación web:

- Visual Studio 2010 y Team Foundation Server (TFS) 2010
- MSBuild y compilación de equipo TFS
- Servicios de Internet Information Server (IIS) 7.5
- Herramienta de implementación Web de IIS (Web Deploy) 2.1
- La utilidad de implementación de la base de datos de VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales en la implementación de web de escala empresarial. Estos son otros tutoriales de la serie:

- [Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido de Introducción proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, el WPP, Web Deploy y otras tecnologías relacionadas. Se explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación de paquete de web remoto mediante el servicio Web Deployment Agent (agente remoto) o el controlador de implementar de Web y la implementación de la base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar las aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua y desencadena manualmente las implementaciones de compilaciones concretas.

> [!div class="step-by-step"]
> [Siguiente](performing-a-what-if-deployment.md)
