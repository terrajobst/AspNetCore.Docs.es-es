---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implementación de la empresa Web | Microsoft Docs
author: jrjlee
description: Este tutorial describe cómo satisfacer una gran cantidad de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial en desarrollo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 07c1ea9728c0130b860c0e0a64eb0751245ff840
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371147"
---
<a name="web-deployment-in-the-enterprise"></a>Implementación Web en la empresa
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial describe cómo satisfacer una gran cantidad de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial en entornos de desarrollo, prueba, ensayo y producción. El tutorial incluye una solución de referencia junto con una combinación de contenido conceptual y orientados a tareas a guiarle a través de varias tareas comunes y procedimientos.
> 
> Para una traducción de italiano de estos tutoriales, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Desafíos de la implementación empresarial

Las organizaciones a menudo encontrará con estos desafíos cuando examinen para administrar la implementación de soluciones complejas, de escala empresarial:

- Deberá ser capaz de implementar proyectos en varios entornos, como desarrollador o probar entornos, plataformas y los servidores de producción de almacenamiento provisional. La solución debe implementarse con una configuración distinta para cada entorno.
- Deberá implementar varios proyectos dependientes simultáneamente como parte del proceso de compilación e implementación automatizado o de paso a paso.
- Deberá poder realizar la implementación de la unidad desde un proceso automatizado. Por ejemplo, desea utilizar un proceso de integración continua (CI) para implementar aplicaciones web en un entorno de prueba cuando se proteja nuevo código.
- Debe ser capaz de controlar el proceso de implementación y establecer las variables de implementación desde fuera de Visual Studio, como desarrolladores están poco probable que tengan los valores de configuración correctos o las credenciales necesarias para cada entorno de destino.
- Deberá implementar proyectos de base de datos basada en el esquema y conservar los datos existentes en las implementaciones posteriores.
- Deberá implementar bases de datos de pertenencia de forma ad hoc sin necesidad de implementar datos de la cuenta de usuario. Es posible que también deberá actualizar el esquema de bases de datos de pertenencia implementado sin perder datos de la cuenta de usuario existente.
- Tendrá que excluir determinados archivos o carpetas al implementar contenido en distintos entornos de destino.

## <a name="overview-of-approach"></a>Información general de enfoque

En este tutorial, junto con los otros tutoriales de esta serie, usa este enfoque de alto nivel para cumplir los desafíos que se ha descrito anteriormente.

- **Usar archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para controlar el proceso de compilación e implementación general.**
- Esto le permite compilar e implementar todos los proyectos de la solución como parte de una única operación que permite ejecutar scripts.
- Configuración específica del entorno se configura mediante archivos de proyecto específicos del entorno simple. A diferencia de la Visual Studio centrado en el enfoque del uso de configuraciones de soluciones y perfiles de publicación para configurar las implementaciones para entornos diferentes, este enfoque le permite configurar y administrar el proceso de implementación desde fuera de Visual Studio. Esto significa que los desarrolladores no necesitan conocimientos de las cadenas de conexión, los extremos de servicio, las credenciales del servidor y otras variables de implementación para entornos de destino pase.
- Los archivos de proyecto personalizadas se pueden invocar por Team Build como parte de un flujo de trabajo de Team Foundation Server (TFS). Esto permite configurar la implementación automatizada para escenarios de integración continua.

**Utilice la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para empaquetar e implementar proyectos de aplicación web.**

- Implementación Web proporciona un marco que permite empaquetar e implementar el contenido de la aplicación web en un servidor web IIS de destino, junto con las dependencias, opciones de configuración, configuración de seguridad y otros requisitos nuevos.
- Puede controlar el proceso de empaquetado e implementación completo desde dentro de los archivos de proyecto de MSBuild personalizados. También puede manipular los valores de configuración que acompañan a su paquete de implementación web, como las cadenas de conexión, los extremos de servicio y los detalles del destino de IIS.
- Web Deploy, junto con la canalización de publicación de Web, ofrece una gran cantidad de puntos de extensibilidad que permiten personalizar sus implementaciones. Por ejemplo, es fácil excluir carpetas y archivos no deseados de sus paquetes de implementación web.

**Use la utilidad VSDBCMD.exe para implementar y actualizar esquemas de base de datos.**

- VSDBCMD permite implementar bases de datos desde un archivo de esquema de base de datos (.dbschema), que se genera al compilar un proyecto de base de datos de Visual Studio. En cambio, la funcionalidad de implementación de la base de datos incluida en Web Deploy es más adecuada para la implementación de bases de datos existentes desde una instancia de SQL Server local.
- A diferencia de la funcionalidad de Visual Studio para la implementación de proyectos de base de datos, VSDBCMD le permite implementar actualizaciones diferenciales en una base de datos de destino existente. Esto le permite conservar los datos existentes mientras se actualiza el esquema de base de datos.
- Puede ejecutar comandos VSDBCMD desde dentro de los archivos de proyecto de MSBuild personalizados.

## <a name="content-map"></a>Mapa de contenido

En este tutorial se incluye temas que se dividen en cuatro áreas principales.

Estos temas presenta la solución de referencia&#x2014;la solución Contact Manager&#x2014;y describen cómo descargarlo y configurarlo en el equipo local:

- [La solución Contact Manager](the-contact-manager-solution.md)
- [Configurar la solución Contact Manager](setting-up-the-contact-manager-solution.md)

Estos temas presentan los archivos de proyecto de MSBuild, se describe cómo crear y usar archivos de proyecto personalizado y recorra el proceso de implementación para la solución Contact Manager:

- [Descripción del archivo de proyecto](understanding-the-project-file.md)
- [Descripción del proceso de compilación](understanding-the-build-process.md)

Estos temas describen la implementación de aplicaciones web, incluido cómo la compilación y empaquetado del proceso, cómo se integra el proceso de compilación con la canalización de publicación de Web, cómo modificar los parámetros de implementación y cómo implementar paquetes web al destino entornos:

- [Crear y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md)
- [Configurar los parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md)
- [Implementar paquetes web](deploying-web-packages.md)

- [Implementación de proyectos de base de datos](deploying-database-projects.md) describe las distintas técnicas que puede usar para implementar proyectos de base de datos de Visual Studio, junto con las ventajas y desventajas de cada enfoque. [Crear y ejecutar un archivo de comandos de implementación](creating-and-running-a-deployment-command-file.md) se describe cómo crear un archivo de comandos simple que encapsula la lógica de implementación y le permite implementar soluciones complejas como un proceso paso a paso.
- Por último, [instalar manualmente los paquetes Web](manually-installing-web-packages.md) concluye el tutorial mostrándole para importar paquetes de web en IIS.

## <a name="key-technologies"></a>Tecnologías clave

Los temas de este tutorial usan principalmente estas tecnologías para administrar las compilación e implementación:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- La utilidad de implementación de la base de datos de VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales de implementación web de escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial describe cómo configurar los servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación del paquete de web remoto mediante el servicio de agente de implementación Web (el agente remoto) o el controlador de implementación Web e implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.
- [Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y desencadene manualmente las implementaciones de compilaciones concretas.
- [Implementación Web de empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial describe cómo realizar diversas tareas de implementación más avanzadas, como la personalización de las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de implementación y tomar las aplicaciones web sin conexión durante el proceso de implementación .

> [!div class="step-by-step"]
> [Siguiente](the-contact-manager-solution.md)
