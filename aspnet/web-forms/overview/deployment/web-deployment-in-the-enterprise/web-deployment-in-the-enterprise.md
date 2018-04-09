---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implementación de la empresa Web | Documentos de Microsoft
author: jrjlee
description: Este tutorial describe cómo satisfacer una gran cantidad de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial para desarrollo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="web-deployment-in-the-enterprise"></a>Implementación de Web de la empresa
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial describe cómo satisfacer una gran cantidad de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial a los entornos de desarrollo, prueba, ensayo y producción. El tutorial incluye una solución de referencia junto con una combinación de contenido conceptual y orientada a tareas para guiarle a través de varias tareas comunes y procedimientos.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Problemas de implementación de las empresas

Las organizaciones encuentran a menudo estos desafíos cuando examinen para administrar la implementación de soluciones complejas, de escala empresarial:

- Necesario poder implementar proyectos en varios entornos, como desarrollador o entornos de pruebas de almacenamiento provisional plataformas y servidores de producción. La solución debe implementarse con distintos valores de configuración para cada entorno.
- Debe implementar varios proyectos dependientes al mismo tiempo como parte de un proceso de compilación e implementación paso a paso o automatizado.
- Debe poder realizar la implementación de la unidad de un proceso automatizado. Por ejemplo, desea utilizar un proceso de integración continua (CI) para implementar aplicaciones web en un entorno de prueba al proteger el código nuevo.
- Debe ser capaz de controlar el proceso de implementación y establecer las variables de implementación desde fuera de Visual Studio, como a los desarrolladores no suelen tener los valores de configuración correctos o las credenciales necesarias para cada entorno de destino.
- Debe implementar proyectos de base de datos basada en el esquema y conservar los datos existentes en las implementaciones posteriores.
- Debe implementar las bases de datos de pertenencia de manera ad hoc sin implementar datos de la cuenta de usuario. También tendrá que actualizar el esquema de bases de datos de pertenencia implementado sin perder datos de la cuenta de usuario existente.
- Debe excluir determinados archivos o carpetas al implementar contenido en varios entornos de destino.

## <a name="overview-of-approach"></a>Información general de enfoque

Este tutorial, junto con los otros tutoriales de esta serie, usa este enfoque de alto nivel para cumplir los desafíos que se ha descrito anteriormente.

- **Usar archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para controlar el proceso de compilación e implementación general.**
- Esto le permite crear e implementar todos los proyectos de la solución como parte de una operación única, que permiten ejecutar scripts.
- Configuración específica del entorno se configura mediante archivos de proyecto específicos de un entorno simple. A diferencia del Visual Studio centrado en el enfoque de utilizar configuraciones de soluciones y perfiles de publicación para configurar las implementaciones para entornos diferentes, este enfoque le permite configurar y administrar el proceso de implementación desde fuera de Visual Studio. Esto significa que los desarrolladores no necesitan adelantar el conocimiento de las cadenas de conexión, los extremos de servicio, las credenciales del servidor y otras variables de implementación para los entornos de destino.
- Los archivos de proyecto personalizadas se pueden invocar mediante Team Build como parte de un flujo de trabajo de Team Foundation Server (TFS). Esto le permite configurar la implementación automatizada para escenarios de integración continua.

**Utilice la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para empaquetar e implementar proyectos de aplicación web.**

- Implementación Web proporciona un marco que permite empaquetar e implementar el contenido de la aplicación web en un servidor web IIS de destino, junto con las dependencias, configuración, configuración de seguridad y otros requisitos.
- Puede controlar todo el proceso de empaquetado e implementación desde dentro de los archivos de proyecto de MSBuild personalizados. También puede manipular los valores de configuración que acompañan a su paquete de implementación web, como las cadenas de conexión, los extremos de servicio y los detalles de destino de IIS.
- Web Deploy, junto con la canalización de publicación de Web, ofrece una gran cantidad de puntos de extensibilidad que permiten personalizar las implementaciones. Por ejemplo, es fácil excluir archivos no deseados y carpetas de los paquetes de implementación web.

**Usar la utilidad VSDBCMD.exe para implementar y actualizar esquemas de base de datos.**

- VSDBCMD permite implementar bases de datos de un archivo de esquema de base de datos (.dbschema), que se genera cuando se compila un proyecto de base de datos de Visual Studio. En cambio, la funcionalidad de implementación de base de datos incluida en Web Deploy es más adecuada para la implementación de bases de datos existentes desde una instancia de SQL Server local.
- A diferencia de la funcionalidad de Visual Studio para implementar proyectos de base de datos, VSDBCMD le permite implementar actualizaciones diferenciales en una base de datos de destino existente. Esto le permite conservar los datos existentes mientras se actualiza el esquema de base de datos.
- Puede ejecutar comandos VSDBCMD desde dentro de los archivos de proyecto de MSBuild personalizados.

## <a name="content-map"></a>Mapa de contenido

Este tutorial incluye temas que se dividen en cuatro áreas principales.

Estos temas presentan la solución de referencia&#x2014;la solución póngase en contacto con el administrador&#x2014;y describen cómo descargarlo y configurarlo en el equipo local:

- [La solución Contact Manager](the-contact-manager-solution.md)
- [Configurar la solución Contact Manager](setting-up-the-contact-manager-solution.md)

Estos temas presentan los archivos de proyecto de MSBuild, se detalla cómo puede crear y usar archivos de proyecto personalizado y guiar el proceso de implementación para la solución póngase en contacto con el administrador:

- [Descripción del archivo de proyecto](understanding-the-project-file.md)
- [Descripción del proceso de compilación](understanding-the-build-process.md)

Estos temas describen la implementación de aplicaciones web, incluido cómo la compilación y empaquetado del proceso, cómo el proceso de compilación se integra con la canalización de publicación de Web, cómo modificar los parámetros de implementación y cómo implementar paquetes de web en el destino entornos:

- [Crear y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md)
- [Configurar los parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md)
- [Implementar paquetes web](deploying-web-packages.md)

- [Implementar proyectos de base de datos](deploying-database-projects.md) describe las distintas técnicas que puede usar para implementar proyectos de base de datos de Visual Studio, junto con las ventajas y desventajas de cada enfoque. [Crear y ejecutar un archivo de comandos de implementación](creating-and-running-a-deployment-command-file.md) describe cómo crear un archivo de comandos simple que encapsula la lógica de implementación y le permite implementar soluciones complejas como un proceso paso a paso.
- Por último, [instalación manual de paquetes de Web](manually-installing-web-packages.md) concluye el tutorial mostrándole para importar paquetes de web en IIS.

## <a name="key-technologies"></a>Tecnologías de clave

Los temas de este tutorial usan principalmente estas tecnologías para administrar la compilación e implementación:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- La utilidad de implementación de la base de datos de VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales en la implementación de web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido de Introducción proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación de paquete de web remoto mediante el servicio Web Deployment Agent (agente remoto) o el controlador de implementar de Web y la implementación de la base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar las aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua y desencadena manualmente las implementaciones de compilaciones concretas.
- [Avanzada de implementación Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de la base de datos para varios entornos, excluir archivos y carpetas de implementación y dejar las aplicaciones de web sin conexión durante el proceso de implementación .

> [!div class="step-by-step"]
> [Siguiente](the-contact-manager-solution.md)
