---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management: Entornos de desarrollo a producción | Documentos de Microsoft'
author: jrjlee
description: Este tema muestra cómo una compañía ficticia administra la implementación de una aplicación web ASP.NET a través de entornos de prueba, ensayo y producción como par...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 8beeffb374df09c6695a1845199d30006ddcc1b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887294"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management: Entornos de desarrollo a producción
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema muestra cómo una compañía ficticia administra la implementación de una aplicación web ASP.NET a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo. En el tema, se proporcionan vínculos para obtener más información y los tutoriales sobre cómo realizar tareas específicas.
> 
> El tema está diseñado para proporcionar una visión general de alto nivel para un [serie de tutoriales](deploying-web-applications-in-enterprise-scenarios.md) en la implementación de web de la empresa. No se preocupe si no está familiarizado con algunos de los conceptos descritos aquí&#x2014;los tutoriales siguientes proporcionan información detallada sobre todas estas tareas y técnicas.
> 
> > [!NOTE]
> > Parael simplificar, este tema no describen las bases de datos de actualización como parte del proceso de implementación. Sin embargo, realizar actualizaciones incrementales en las características de las bases de datos es un requisito de muchos escenarios de implementación de empresa y encontrará instrucciones sobre cómo lograr esto más adelante en esta serie de tutoriales. Para obtener más información, consulte [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Información general

El proceso de implementación se ilustra a continuación se basa en el escenario de implementación de Fabrikam, Inc. descrito en [implementación Web de empresa: información general del escenario](enterprise-web-deployment-scenario-overview.md). Lea la información general del escenario antes de estudiar en este tema. En esencia, el escenario examina cómo una organización administra la implementación de una aplicación web bastante complejas, el [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), a través de distintas fases en un entorno empresarial típico.

En un nivel alto, la solución de Contact Manager pasa por estas fases como parte del proceso de implementación y desarrollo:

1. Un desarrollador protege algún código en Team Foundation Server (TFS) 2010.
2. TFS compila el código y ejecuta cualquier prueba unitaria asociado al proyecto de equipo.
3. TFS implementa la solución en el entorno de prueba.
4. El equipo de desarrolladores comprueba y valida la solución en el entorno de prueba.
5. El Administrador de entorno de ensayo realiza una implementación de "¿Qué ocurre si" en el entorno de ensayo, para establecer si la implementación causa ningún problema.
6. El Administrador de entorno de ensayo realiza una implementación en vivo en el entorno de ensayo.
7. La solución se somete a pruebas en el entorno de ensayo de aceptación de usuario.
8. Los paquetes de implementación web se importa manualmente en el entorno de producción.

Estas fases forman parte de un ciclo de desarrollo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

En la práctica, el proceso es ligeramente más complicado que éste, tal y como se verá cuando adentrarnos en cada fase con más detalle. Fabrikam, Inc. utiliza un enfoque diferente para la implementación para cada entorno de destino.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

El resto de este tema examina estas fases claves de este ciclo de vida de implementación:

- **Requisitos previos**: cómo debe configurar su infraestructura de servidor antes de poner la lógica de implementación en su lugar.
- **Inicial de desarrollo e implementación**: ¿qué debe hacer antes de implementar la solución por primera vez.
- **Implementación para probar**: cómo empaquetar e implementar contenido en un entorno de prueba automáticamente cuando un desarrollador protege en el nuevo código.
- **Implementación de ensayo**: cómo implementar específico genera un entorno de ensayo y cómo realizar "¿Qué ocurre si" implementaciones para asegurarse de que una implementación no causa ningún problema.
- **Implementación en producción**: cómo importar paquetes de web en un entorno de producción cuando la infraestructura de red impide la implementación remota.

## <a name="prerequisites"></a>Requisitos previos

La primera tarea en cualquier escenario de implementación es asegurarse de que su infraestructura de servidor cumple los requisitos de las técnicas y herramientas de implementación. En este caso, Fabrikam, Inc. haya configurado su infraestructura de servidor similar al siguiente:

- TFS se configura para incluir una colección de proyectos de equipo, controladores de compilación y agentes de compilación. Vea [configurar Team Foundation Server para automatizar la implementación de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) para obtener más información.
- El entorno de prueba está configurado para aceptar las implementaciones remotas con el servicio de agente de implementación Web (el "agente remoto"), como se describe en [escenario: configuración de un entorno de prueba para la implementación Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) y [ Configurar un servidor Web de publicación (agente remoto) de implementación Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- El entorno de ensayo está configurado para aceptar las implementaciones remotas con el punto de conexión del controlador de Web implementar, como se describe en [escenario: configuración de un entorno de ensayo para la implementación Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) y [configurar un servidor Web Publicación de implementación de Web (Web Deploy controlador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- El entorno de producción está configurado para permitir que un administrador puede importar manualmente los paquetes de implementación web en Internet Information Services (IIS), como se describe en [escenario: configurar un entorno de producción para la implementación de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) y [configurar un servidor Web de publicación (implementación sin conexión) de implementación Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Implementación y desarrollo inicial

Antes de que el equipo de desarrollo de Fabrikam, Inc. puede implementar la solución de Contact Manager por primera vez, debe realizar estas tareas:

- Crear un nuevo proyecto de equipo en TFS.
- Crear los archivos de proyecto de Microsoft Build Engine (MSBuild) que contienen la lógica de implementación.
- Crear las definiciones de compilación TFS que desencadenan los procesos de implementación.

### <a name="create-a-new-team-project"></a>Cree un nuevo proyecto de equipo

- El administrador TFS, Rob Walters, crea un nuevo proyecto de equipo para la aplicación, como se describe en [crea un proyecto de equipo en TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). A continuación, el jefe de desarrollo, Matt Hink, crea una solución de esqueleto. Proteger sus archivos en el nuevo proyecto de equipo en TFS, tal y como se describe en [agregar contenido al Control de código fuente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Crear la lógica de implementación

Matt Hink crea varios archivos de proyecto de MSBuild personalizados, utilizando el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un archivo de proyecto denominado *Publish.proj* que ejecuta el proceso de implementación. Este archivo contiene los destinos de MSBuild que compilación los proyectos de la solución, crean paquetes de web e implementación los paquetes en un entorno de servidor de destino.
- Archivos de proyecto específicas del entorno con el nombre *Env Dev.proj* y *Stage.proj Env*. Estos contienen valores que son específicos para el entorno de prueba y el entorno de ensayo respectivamente, como cadenas de conexión, los extremos de servicio y los detalles del servicio remoto que va a recibir el paquete de web. Para obtener instrucciones sobre cómo elegir la configuración de derechos para entornos de destino específica, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Para ejecutar la implementación, un usuario ejecuta la *Publish.proj* archivo utilizando MSBuild o Team Build y especifica la ubicación del archivo de proyecto específicas del entorno correspondiente (*Dev.proj Env* o *Env Stage.proj*) como un argumento de línea de comandos. El *Publish.proj* archivo, a continuación, importa el archivo de proyecto específicas del entorno para crear un conjunto completo de instrucciones para cada entorno de destino de publicación.

> [!NOTE]
> El funcionamiento de estos archivos de proyecto personalizado es independiente del mecanismo que se utiliza para invocar MSBuild. Por ejemplo, puede usar la línea de comandos de MSBuild directamente, como se describe en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Puede ejecutar los archivos de proyecto desde un archivo de comandos, como se describe en [crear y ejecutar un archivo de comandos de implementación](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Como alternativa, puede ejecutar los archivos de proyecto desde una definición de compilación en TFS, tal y como se describe en [crear una definición de compilación que admite la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> En cada caso, el resultado final es el mismo&#x2014;MSBuild ejecuta el archivo de proyecto combinada e implementa la solución en el entorno de destino. Esto le brinda una gran cantidad de flexibilidad en cómo desencadenar el proceso de publicación.


Una vez que ha creado los archivos de proyecto personalizado, Matt agrega a una carpeta de soluciones y comprueba en el control de código fuente.

### <a name="create-build-definitions"></a>Crear definiciones de compilación

Como una tarea de preparación final, Matt y Rob funcionan conjuntamente para crear tres definiciones de compilación para el nuevo proyecto de equipo:

- **DeployToTest**. Se compila la solución póngase en contacto con el administrador y lo implementa en el entorno de prueba en cada vez en el repositorio se produce.
- **DeployToStaging**. Esto implementa recursos desde una compilación anterior especificado en el entorno de ensayo cuando un desarrollador pone en cola la compilación.
- **DeployToStaging-WhatIf**. Esto lleva a cabo una implementación de "¿Qué ocurre si" en el entorno de ensayo cuando un desarrollador pone en cola la compilación.

Las secciones siguientes proporcionan más detalles sobre cada una de estas definiciones de compilación.

## <a name="deployment-to-test"></a>Implementación de prueba

El equipo de desarrollo en Fabrikam, Inc. mantiene entornos de prueba para llevar a cabo una variedad de actividades, como la comprobación y validación, pruebas de facilidad de uso, probar la compatibilidad y pruebas ad hoc o exploración de pruebas de software.

El equipo de desarrollo ha creado una definición de compilación en TFS denominado **DeployToTest**. Esta definición de compilación usa un desencadenador de integración continua, lo que significa que el proceso de compilación se ejecuta cada vez que un miembro del equipo de desarrollo de Fabrikam, Inc. lleva a cabo en el repositorio. Cuando se desencadena una compilación, la definición de compilación hará lo siguiente:

- Compile la solución ContactManager.sln. Esto a su vez compila todos los proyectos dentro de la solución.
- Ejecute cualquier prueba unitaria en la estructura de carpetas de solución (si la solución se compila correctamente).
- Ejecute los archivos de proyecto personalizadas que controlan el proceso de implementación (si la solución se compila correctamente y pasa cualquier prueba unitaria).

El resultado final es que si la solución se compila correctamente y pasa las pruebas unitarias, los paquetes de web y otros recursos de implementación se implementan en el entorno de prueba.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El **DeployToTest** fuentes de definición de compilación de estos argumentos a MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


El **DeployOnBuild = true** y **DeployTarget = paquete** propiedades se utilizan cuando Team Build compila los proyectos dentro de la solución. Cuando el proyecto es un proyecto de aplicación web, estas propiedades indicar a MSBuild para crear un paquete de implementación web para el proyecto. El **TargetEnvPropsFile** propiedad indica la *Publish.proj* dónde encontrar el archivo de proyecto específicas del entorno para importar de archivos.

> [!NOTE]
> Para obtener un tutorial detallado sobre cómo crear una definición de compilación similar al siguiente, vea [crear una definición de compilación que admite la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


El *Publish.proj* archivo contiene los destinos que compila cada proyecto de la solución. Sin embargo, también incluye una lógica condicional que omite estos destinos de generación si está ejecutando el archivo en Team Build. Esto le permite aprovechar las ventajas de la funcionalidad de compilación adicionales que Team Build ofrece, como la capacidad de ejecutar pruebas unitarias. Si la compilación de soluciones o la unidad de prueba por error, el *Publish.proj* archivo no se ejecutará y no se implementará la aplicación.

La lógica condicional se lleva a cabo mediante la evaluación de la **BuildingInTeamBuild** propiedad. Se trata de una propiedad de MSBuild que se establece automáticamente en **true** cuando se utiliza Team Build para compilar los proyectos.

## <a name="deployment-to-staging"></a>Implementación de ensayo

Cuando una compilación cumple todos los requisitos del equipo, desarrollador en el entorno de prueba, el equipo puede que desee implementar la misma compilación en un entorno de ensayo. Entornos de almacenamiento provisional se suelen configurar para que coincida con las características de la producción o "activo" entorno como estrechamente como sea posible, por ejemplo, en cuanto a especificaciones de servidor, sistemas operativos y software y configuración de red. Entornos de almacenamiento provisional se suelen usar para pruebas de carga, pruebas de aceptación de usuario y revisiones internas más amplias. Compilaciones se implementan en el entorno de ensayo directamente desde el servidor de compilación.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Las definiciones de compilación que se utilizan para implementar la solución en el entorno de ensayo, **DeployToStaging-WhatIf** y **DeployToStaging**, comparten estas características:

- Que no crean realmente nada. Cuando Rob implementa la solución en el entorno de ensayo, desea implementar una compilación concreta, existente que ya se ha verificado y validado en el entorno de prueba. Las definiciones de compilación solo tiene que ejecutar los archivos de proyecto personalizadas que controlan el proceso de implementación.
- Cuando Rob desencadena una compilación, utiliza los parámetros de compilación para especificar qué compilación contiene los recursos que desea implementar desde el servidor de compilación.
- Las definiciones de compilación no se activan automáticamente. Rob pone en cola manualmente una compilación cuando quiere implementar la solución en el entorno de ensayo.

Este es el proceso de alto nivel para una implementación en el entorno de ensayo:

1. El Administrador de entorno provisional, Rob Walters, pone en cola una compilación utilizando la **DeployToStaging-WhatIf** definición de compilación. Antonio utiliza los parámetros de la definición de compilación para especificar qué compilación que desea implementar.
2. El **DeployToStaging-WhatIf** ejecuciones de definición de compilación de los archivos de proyecto personalizadas en el modo de "¿Qué ocurre si". Esto genera archivos de registro como si Rob estaba llevando a cabo una implementación en vivo, pero realmente no realiza cambios en el entorno de destino.
3. Rob revisa los archivos de registro para determinar los efectos de la implementación en el entorno de ensayo. En concreto, Rob desea comprobar qué se agregará, lo que se actualizará y lo que se eliminará.
4. Si es Rob satisface que la implementación no realizar ningún cambio no desea a los recursos existentes o los datos, pone en cola una compilación utilizando la **DeployToStaging** definición de compilación.
5. El **DeployToStaging** ejecuciones de definición de compilación de los archivos de proyecto personalizado. Estos publicación los recursos de implementación en el servidor web principal en el entorno de ensayo.
6. El controlador de Web Farm Framework (WFF) sincroniza los servidores web en el entorno de ensayo. Esto hace que la aplicación esté disponible en todos los servidores web en la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El **DeployToStaging** fuentes de definición de compilación de estos argumentos a MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


El **TargetEnvPropsFile** propiedad indica la *Publish.proj* dónde encontrar el archivo de proyecto específicas del entorno para importar de archivos. El **OutputRoot** propiedad invalida el valor integrado e indica la ubicación de la carpeta de compilación que contiene los recursos que se va a implementar. Cuando Rob pone en cola la compilación, utiliza el **parámetros** ficha para proporcionar un valor actualizado de la **OutputRoot** propiedad.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Para obtener más información sobre cómo crear una definición de compilación similar al siguiente, vea [implementar una compilación concreta](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


El **DeployToStaging-WhatIf** definición de compilación contiene la misma lógica de implementación como el **DeployToStaging** definición de compilación. Sin embargo, incluye el argumento adicional **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


En el *Publish.proj* archivo, el **WhatIf** propiedad indica que todos los recursos de implementación deben publicarse en el modo de "¿Qué ocurre si". En otras palabras, se generan archivos de registro como si la implementación quedaran con antelación, pero en realidad es cambiar nada en el entorno de destino. Esto le permite evaluar el impacto de una implementación propuesto&#x2014;en particular, lo que se agregarán, lo que se actualizarán y lo que se eliminen&#x2014;antes de implementar los cambios.

> [!NOTE]
> Para obtener más información sobre cómo configurar las implementaciones de "¿Qué ocurre si", consulte [realizar una implementación de "¿Qué ocurre si"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Una vez que haya implementado la aplicación en el servidor web principal en el entorno de ensayo, el WFF sincronizará automáticamente la aplicación en todos los servidores en la granja de servidores.

> [!NOTE]
> Para obtener más información acerca de cómo configurar el WFF para sincronizar los servidores web, consulte [crear una granja de servidores con el marco de trabajo de la granja de servidores Web](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Implementación en producción

Cuando se haya aprobado una compilación en el entorno de ensayo, el equipo de Fabrikam, Inc. puede publicar la aplicación en el entorno de producción. El entorno de producción es que la aplicación esté "activa" y llega a su audiencia de destino de los usuarios finales.

El entorno de producción está en una red de perímetro con conexión a Internet. Esto está aislada de la red interna que contiene el servidor de compilación. El administrador del entorno de producción, Lisa Andrews, manualmente debe copiar los paquetes de implementación web desde el servidor de compilación e importarlas en IIS en el servidor web de producción principal.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Este es el proceso de alto nivel para una implementación en el entorno de producción:

1. El equipo de desarrolladores incluyas a Lisa que una compilación está lista para su implementación en producción. El equipo le informa de Lisa de la ubicación de los paquetes de implementación web dentro de la carpeta de buzón en el servidor de compilación.
2. Lisa recopila los paquetes de web desde el servidor de compilación y las copia en el servidor web principal en el entorno de producción.
3. Lisa usa el Administrador de IIS para importar y publicar los paquetes de web en el servidor web principal.
4. El controlador WFF sincroniza los servidores web en el entorno de producción. Esto hace que la aplicación esté disponible en todos los servidores web en la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El Administrador de IIS incluye a un asistente Importar aplicación de paquete que facilita el proceso publicar paquetes de web en un sitio Web de IIS. Para ver un tutorial sobre cómo llevar a cabo este procedimiento, consulte [instalación manual de paquetes de Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona una ilustración del ciclo de vida de implementación para una aplicación web de escala empresarial típico.

Este tema forma parte de una serie de tutoriales que proporcionan instrucciones sobre distintos aspectos de la implementación de aplicaciones web. En la práctica, hay una gran cantidad de consideraciones y tareas adicionales en cada fase del proceso de implementación y no es posible abarcarlas en un tutorial de inicio único. Para obtener más información, consulte estos tutoriales:

- [Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial proporciona una introducción completa a técnicas de implementación web utilizando MSBuild y la herramienta de implementación Web de IIS (Web Deploy).
- [Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo configurar entornos de servidor de Windows para admitir diferentes escenarios de implementación.
- [Configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo integrar la lógica de implementación en los procesos de compilación TFS.
- [Avanzada de implementación Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial proporciona instrucciones sobre cómo cumplir algunos de los desafíos de implementación más complejos que se enfrentan las organizaciones.

> [!div class="step-by-step"]
> [Anterior](enterprise-web-deployment-scenario-overview.md)
