---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management: Entornos de desarrollo a producción | Microsoft Docs'
author: jrjlee
description: En este tema se muestra cómo una compañía ficticia administra la implementación de una aplicación web ASP.NET a través de entornos de prueba, ensayo y producción como par...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 7cb9c949936c3af73d4c904d401c36d4d83f3e18
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838547"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management: Entornos de desarrollo a producción
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se muestra cómo una compañía ficticia administra la implementación de una aplicación web ASP.NET a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo. En el tema, se proporcionan vínculos para obtener más información y tutoriales sobre cómo realizar tareas específicas.
> 
> El tema está diseñado para proporcionar una descripción general de un [serie de tutoriales](deploying-web-applications-in-enterprise-scenarios.md) en la implementación web en la empresa. No se preocupe si no está familiarizado con algunos de los conceptos descritos aquí&#x2014;los tutoriales siguientes proporcionan información detallada sobre todas estas tareas y técnicas.
> 
> > [!NOTE]
> > Parael simplificar, este tema no habla de actualización de bases de datos como parte del proceso de implementación. Sin embargo, realizar actualizaciones incrementales en las características de las bases de datos es un requisito para muchos escenarios de implementación empresarial, y puede encontrar instrucciones sobre cómo lograr esto más adelante en esta serie de tutoriales. Para obtener más información, consulte [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Información general

El proceso de implementación se muestra aquí es según se describe en el escenario de implementación de Fabrikam, Inc. [implementación Web de empresa: información general del escenario](enterprise-web-deployment-scenario-overview.md). Debe leer la información general del escenario antes de estudiar este tema. En esencia, el escenario examina cómo una organización administra la implementación de una aplicación web razonablemente compleja, el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), por distintas fases en un entorno empresarial típico.

En un nivel alto, la solución Contact Manager pasa por estas fases como parte del proceso de implementación y desarrollo:

1. Un desarrollador protege algo de código en Team Foundation Server (TFS) 2010.
2. TFS compila el código y ejecuta cualquier prueba unitaria asociada con el proyecto de equipo.
3. TFS implementa la solución en el entorno de prueba.
4. El equipo de desarrollo se comprueba y valida la solución en el entorno de prueba.
5. El Administrador de entorno de ensayo realiza una implementación "what if" en el entorno de ensayo, para establecer si la implementación, producirá ningún problema.
6. El Administrador de entorno de ensayo, realiza una implementación en vivo en el entorno de ensayo.
7. La solución se somete a prueba en el entorno de ensayo de aceptación del usuario.
8. Los paquetes de implementación web se puede importados manualmente en el entorno de producción.

Estas fases forman parte de un ciclo de desarrollo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

En la práctica, el proceso es ligeramente más complicado que éste, como se verá cuando nos centramos en cada fase con más detalle. Fabrikam, Inc., se usa un enfoque diferente para la implementación para cada entorno de destino.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

El resto de este tema examina estas fases claves de este ciclo de vida de implementación:

- **Requisitos previos**: cómo debe configurar su infraestructura de servidor antes de poner la lógica de implementación en su lugar.
- **Desarrollo y la implementación inicial**: lo que necesita hacer antes de implementar la solución por primera vez.
- **Implementación para pruebas**: cómo empaquetar e implementar contenido en un entorno de prueba automáticamente cuando un desarrollador protege en el nuevo código.
- **Implementación en ensayo**: cómo implementar específico basa en un entorno de ensayo y cómo realizar "what if" implementaciones para asegurarse de que una implementación no causa ningún problema.
- **Implementación en producción**: cómo importar paquetes de web en un entorno de producción cuando la infraestructura de red impide la implementación remota.

## <a name="prerequisites"></a>Requisitos previos

La primera tarea en cualquier escenario de implementación consiste en asegurarse de que su infraestructura de servidor cumple los requisitos de las técnicas y herramientas de implementación. En este caso, Fabrikam, Inc. ha configurado su infraestructura de servidor similar al siguiente:

- TFS se configura para incluir una colección de proyectos de equipo, los controladores de compilación y agentes de compilación. Consulte [configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) para obtener más información.
- El entorno de prueba está configurado para aceptar implementaciones remotas mediante el servicio de agente de implementación Web (el "agente remoto"), como se describe en [escenario: configurar un entorno de prueba para la implementación de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) y [ Configurar un servidor Web de publicación (agente remoto) de la implementación de Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- El entorno de ensayo está configurado para aceptar implementaciones remotas con el punto de conexión del controlador de implementación Web, como se describe en [escenario: configurar un entorno de ensayo para la implementación de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) y [configurar un servidor Web para Web publicación de la implementación (controlador de implementación Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- El entorno de producción está configurado para permitir que un administrador importar manualmente los paquetes de implementación web en Internet Information Services (IIS), como se describe en [escenario: configurar un entorno de producción para la implementación de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) y [configurar un servidor Web de publicación (implementación sin conexión) de la implementación de Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Implementación y desarrollo inicial

Antes de que el equipo de desarrollo de Fabrikam, Inc. puede implementar la solución Contact Manager por primera vez, debe realizar estas tareas:

- Cree un nuevo proyecto de equipo en TFS.
- Cree los archivos de proyecto de Microsoft Build Engine (MSBuild) que contienen la lógica de implementación.
- Crear las definiciones de compilación TFS que desencadenan los procesos de implementación.

### <a name="create-a-new-team-project"></a>Crear un nuevo proyecto de equipo

- El administrador TFS, Rob Walters, crea un nuevo proyecto de equipo para la aplicación, como se describe en [crear un proyecto de equipo en TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). A continuación, el jefe de desarrollo, Matt Hink, crea una solución de esqueleto. Comprueba sus archivos en el nuevo proyecto de equipo en TFS, como se describe en [agregar contenido al Control de código fuente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Crear la lógica de implementación

Matt Hink crea varios archivos de proyecto de MSBuild personalizados, el enfoque de archivo de proyecto división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un archivo de proyecto denominado *Publish.proj* que ejecuta el proceso de implementación. Este archivo contiene destinos de MSBuild que compilación los proyectos de la solución, crean paquetes de web e implementación los paquetes en un entorno de servidor de destino.
- Archivos de proyecto específicos del entorno denominados *Env Dev.proj* y *Env Stage.proj*. Éstos contienen valores que son específicos para el entorno de prueba y el entorno de ensayo, respectivamente, al igual que las cadenas de conexión, los extremos de servicio y los detalles del servicio remoto que va a recibir el paquete de web. Para obtener instrucciones sobre cómo elegir la configuración adecuada para entornos de destino específica, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Para ejecutar la implementación, un usuario ejecuta el *Publish.proj* de archivos mediante MSBuild o Team Build y especifica la ubicación del archivo de proyecto específicos del entorno correspondiente (*Env Dev.proj* o *Env-Stage.proj*) como un argumento de línea de comandos. El *Publish.proj* archivo, a continuación, importa el archivo de proyecto específicos del entorno para crear un conjunto completo de la publicación de las instrucciones para cada entorno de destino.

> [!NOTE]
> El funcionamiento de estos archivos de proyecto personalizadas es independiente del mecanismo de que usar para invocar MSBuild. Por ejemplo, puede usar la línea de comandos de MSBuild directamente, como se describe en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Puede ejecutar los archivos de proyecto desde un archivo de comandos, como se describe en [crear y ejecutar un archivo de comandos de implementación](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Como alternativa, puede ejecutar los archivos de proyecto desde una definición de compilación en TFS, como se describe en [crear una definición de compilación que admite la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> En cada caso, el resultado final es el mismo&#x2014;MSBuild ejecuta el archivo de proyecto combinadas y la solución se implementa en el entorno de destino. Esto le brinda un gran flexibilidad en cómo desencadena el proceso de publicación.


Una vez que ha creado los archivos de proyecto personalizados, Matt agregarlos a una carpeta de soluciones y comprueba en el control de código fuente.

### <a name="create-build-definitions"></a>Crear definiciones de compilación

Como una tarea de preparación final, Matt y Rob funcionan conjuntamente para crear tres definiciones de compilación para el nuevo proyecto de equipo:

- **DeployToTest**. Se compila la solución Contact Manager y lo implementa en el entorno de prueba cada vez en la comprobación se produce.
- **DeployToStaging**. Recursos de una compilación anterior especificado en el entorno de ensayo se implementa cuando un desarrollador pone en cola la compilación.
- **DeployToStaging-WhatIf**. Esto lleva a cabo una implementación "what if" en el entorno de ensayo cuando un desarrollador pone en cola la compilación.

Las secciones siguientes proporcionan más detalles de cada una de estas definiciones de compilación.

## <a name="deployment-to-test"></a>Implementación de prueba

El equipo de desarrollo de Fabrikam, Inc. mantiene los entornos de prueba para llevar a cabo una variedad de actividades, como la comprobación y validación, las pruebas de facilidad de uso, las pruebas de compatibilidad y las pruebas exploratorias o ad hoc de pruebas de software.

El equipo de desarrollo ha creado una definición de compilación en TFS denominada **DeployToTest**. Esta definición de compilación usa un desencadenador de integración continua, lo que significa que el proceso de compilación se ejecuta cada vez que un miembro del equipo de desarrollo de Fabrikam, Inc. realiza una inserción en el repositorio. Cuando se desencadena una compilación, la definición de compilación hará lo siguiente:

- Compile la solución ContactManager.sln. Esto a su vez compila cada proyecto dentro de la solución.
- Ejecutar pruebas unitarias en la estructura de carpetas de solución (si la solución se compila correctamente).
- Ejecute los archivos de proyecto personalizadas que controlan el proceso de implementación (si la solución se compila correctamente y supera las pruebas unitarias).

El resultado final es que si la solución se compila correctamente y pasa las pruebas unitarias, los paquetes de web y otros recursos de implementación se implementan en el entorno de prueba.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El **DeployToTest** suministros de definición de compilación de estos argumentos a MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


El **DeployOnBuild = true** y **DeployTarget = paquete** propiedades se utilizan cuando Team Build compila los proyectos dentro de la solución. Cuando el proyecto es un proyecto de aplicación web, estas propiedades indicar a MSBuild para crear un paquete de implementación web para el proyecto. El **TargetEnvPropsFile** propiedad indica la *Publish.proj* dónde encontrar el archivo de proyecto específicos del entorno para importar de archivo.

> [!NOTE]
> Para obtener un tutorial detallado sobre cómo crear una definición de compilación como esta, consulte [crear una definición de compilación que admite la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


El *Publish.proj* archivo contiene los destinos que compila cada proyecto de la solución. Sin embargo, también incluye la lógica condicional que omite estos compilación destinos si está ejecutando el archivo en Team Build. Esto le permite aprovechar la funcionalidad de compilación adicionales Team Build ofrece, como la capacidad de ejecutar pruebas unitarias. Si la compilación de soluciones o la unidad de pruebas producen un error, el *Publish.proj* archivo no se ejecutará y no se implementará la aplicación.

La lógica condicional se logra evaluando la **BuildingInTeamBuild** propiedad. Esta es una propiedad de MSBuild que se establece automáticamente en **true** cuando usas Team Build para compilar sus proyectos.

## <a name="deployment-to-staging"></a>Implementación en entorno de ensayo

Cuando una compilación cumple todos los requisitos del equipo en el entorno de prueba para desarrolladores, el equipo es posible que desee implementar la misma compilación en un entorno de ensayo. Entornos de ensayo normalmente se configuran para que coincida con las características de la producción o el entorno "activo" como cerca como sea posible, por ejemplo, en cuanto a las especificaciones del servidor, los sistemas operativos y software y configuración de red. Entornos de ensayo a menudo se usan para las pruebas de carga, pruebas de aceptación de usuario y revisiones internas más amplias. Las compilaciones se implementan en el entorno de ensayo directamente desde el servidor de compilación.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Las definiciones de compilación que se usa para implementar la solución en el entorno de ensayo, **DeployToStaging-WhatIf** y **DeployToStaging**, comparten estas características:

- No crean realmente nada. Rob implementa la solución en el entorno de ensayo, quiere implementar una compilación específica existente que ya se ha comprobado y validado en el entorno de prueba. Las definiciones de compilación basta con ejecutar los archivos de proyecto personalizadas que controlan el proceso de implementación.
- Cuando se Rob desencadena una compilación, utiliza los parámetros de compilación para especificar qué compilación contiene los recursos que desea implementar desde el servidor de compilación.
- Las definiciones de compilación no se activan automáticamente. Rob pone en cola manualmente una compilación cuando desea implementar la solución en el entorno de ensayo.

Este es el proceso de alto nivel para una implementación en el entorno de ensayo:

1. El administrador del entorno de ensayo, Rob Walters, pone en cola una compilación utilizando la **DeployToStaging-WhatIf** definición de compilación. Rob utiliza los parámetros de la definición de compilación para especificar qué compilación que desea implementar.
2. El **DeployToStaging-WhatIf** ejecuciones de la definición de compilación de los archivos de proyecto personalizadas en modo "what if". Esto genera archivos de registro como si Rob estaba realizando una implementación activa, pero lo realmente no hace ningún cambio en el entorno de destino.
3. Rob revisa los archivos de registro para determinar los efectos de la implementación en el entorno de ensayo. En concreto, Rob desea comprobar lo que se agregarán, lo que se actualizará y qué se eliminará.
4. Si es Rob satisfecho de que la implementación no realizar ningún cambio no deseada a los recursos existentes o los datos, pone en cola una compilación utilizando la **DeployToStaging** definición de compilación.
5. El **DeployToStaging** ejecuciones de la definición de compilación de los archivos de proyecto personalizado. Estos publicación los recursos de implementación en el servidor web principal en el entorno de ensayo.
6. El controlador de Web Farm Framework (WFF) sincroniza los servidores web en el entorno de ensayo. Esto hace que la aplicación esté disponible en todos los servidores web en la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El **DeployToStaging** suministros de definición de compilación de estos argumentos a MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


El **TargetEnvPropsFile** propiedad indica la *Publish.proj* dónde encontrar el archivo de proyecto específicos del entorno para importar de archivo. El **OutputRoot** propiedad invalida el valor integrado e indica la ubicación de la carpeta de compilación que contiene los recursos que desea implementar. Cuando Rob pone en cola la compilación, usa el **parámetros** tab para proporcionar un valor actualizado de la **OutputRoot** propiedad.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Para obtener más información sobre cómo crear una definición de compilación como esta, consulte [implementar una compilación específica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


El **DeployToStaging-WhatIf** definición de compilación contiene la misma lógica de implementación como el **DeployToStaging** definición de compilación. Sin embargo, incluye el argumento adicional **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


Dentro de la *Publish.proj* archivo, el **WhatIf** propiedad indica que se deben publicar todos los recursos de implementación en modo "what if". En otras palabras, los archivos de registro se generan como si hubiera continué la implementación, pero nada cambia realmente en el entorno de destino. Esto le permite evaluar el impacto de una implementación propuesto&#x2014;en particular, lo que se agregarán, lo que se actualizará y lo que se eliminará&#x2014;antes de realizar los cambios.

> [!NOTE]
> Para obtener más información sobre cómo configurar las implementaciones de "what if", consulte [realizando una implementación "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Cuando se haya implementado la aplicación en el servidor web principal en el entorno de ensayo, el WFF sincronizará automáticamente la aplicación en todos los servidores de la granja de servidores.

> [!NOTE]
> Para obtener más información sobre cómo configurar el WFF para sincronizar los servidores web, consulte [crear una granja de servidores con el marco de granja de servidores Web](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Implementación en producción

Cuando se ha aprobado una compilación en el entorno de ensayo, el equipo de Fabrikam, Inc. puede publicar la aplicación en el entorno de producción. El entorno de producción es donde la aplicación esté "activa" y llega a su audiencia de destino de los usuarios finales.

El entorno de producción está en una red de perímetro a través de Internet. Esto está aislada de la red interna que contiene el servidor de compilación. El Administrador de entorno de producción, Lisa Andrews, manualmente debe copiar los paquetes de implementación web desde el servidor de compilación e importarlos en IIS en el servidor web de producción principal.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Este es el proceso de alto nivel para una implementación en el entorno de producción:

1. El equipo de desarrolladores, aconseja a Lisa que una compilación está lista para su implementación en producción. El equipo, aconseja a Lisa de la ubicación de los paquetes de implementación web dentro de la carpeta de entrega en el servidor de compilación.
2. Lisa recopila los paquetes de web del servidor de compilación y los copia en el servidor web principal en el entorno de producción.
3. Lisa usa el Administrador de IIS para importar y publicar los paquetes de web en el servidor web principal.
4. El controlador de WFF sincroniza los servidores web en el entorno de producción. Esto hace que la aplicación esté disponible en todos los servidores web en la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El Administrador de IIS incluye a un asistente Importar aplicación de paquete que facilita la publicación de paquetes de web en un sitio Web IIS. Para ver un tutorial sobre cómo llevar a cabo este procedimiento, consulte [instalar manualmente los paquetes Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona una ilustración del ciclo de vida de implementación para una aplicación web de escala empresarial típico.

En este tema forma parte de una serie de tutoriales que proporcionan instrucciones sobre distintos aspectos de la implementación de aplicaciones web. En la práctica, hay una gran cantidad de consideraciones y tareas adicionales en cada fase del proceso de implementación y no es posible analizarlas todas en un tutorial de inicio único. Para obtener más información, consulte estos tutoriales:

- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción completa a las técnicas de implementación web utilizando MSBuild y la herramienta de implementación Web de IIS (Web Deploy).
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo configurar entornos de servidor de Windows para admitir diferentes escenarios de implementación.
- [Configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo integrar la lógica de implementación en los procesos de compilación TFS.
- [Implementación Web de empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo cumplir algunos de los desafíos de implementación más complejos que enfrentan las organizaciones.

> [!div class="step-by-step"]
> [Anterior](enterprise-web-deployment-scenario-overview.md)
