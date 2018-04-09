---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Implementación Web de Enterprise: Información general del escenario | Documentos de Microsoft'
author: jrjlee
description: Este conjunto de tutoriales usa una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar a ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="enterprise-web-deployment-scenario-overview"></a>Implementación Web de empresa: Información general del escenario
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriales utiliza una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar una implementación de referencia y para proporcionar a las tareas y los tutoriales comparten un contexto común. Este tema describe el escenario del tutorial y presenta la solución de ejemplo.


## <a name="scenario-description"></a>Descripción del escenario

Fabrikam, Inc., una compañía ficticia, está creando una solución que permite a los equipos de ventas remotos almacenar y recuperar información de contacto de una interfaz web.

Los procesos de Application Lifecycle Management (ALM) en Fabrikam, Inc. requieren la solución para su implementación en tres entornos de servidor en distintas fases del proceso de desarrollo de software:

- Un entorno de prueba o "espacio aislado" de desarrollador.
- Un entorno de ensayo basados en intranet.
- Un entorno de producción a través de Internet.

Cada uno de estos entornos tiene requisitos de seguridad y de configuración diferente y cada uno puede causar problemas de implementación único.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Infraestructura de servidor

Se trata de la infraestructura de desarrollo e implementación de alto nivel en Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Las estaciones de trabajo de desarrollador, la infraestructura de control de código fuente, el entorno de prueba de desarrollador y el entorno de ensayo que residen en la red de intranet dentro del dominio Fabrikam.net. El entorno de producción se encuentra en una red perimetral (también conocida como DMZ, zona desmilitarizada y subred filtrada), que está aislada de la red de intranet por un firewall. Se trata de un escenario de implementación comunes: normalmente, para aislar los servidores de web a través de Internet de su infraestructura de servidor interno mediante el uso de firewalls o servidores de puerta de enlace.

En este ejemplo:

- Un servidor de Team Foundation Server (TFS) 2010 con un servidor de compilación independiente proporciona funcionalidad de integración continua (CI) y control de código fuente.
- El entorno de prueba de desarrollador incluye un servidor web de Internet Information Services (IIS) 7.5 y un servidor de base de datos de SQL Server 2008 R2.
- El entorno de producción incluye varios servidores web de IIS 7.5 sincronizados por un servidor de controlador de Web Farm Framework (WFF), junto con un servidor de base de datos de SQL Server 2008 R2. En la práctica, el servidor de base de datos puede utilizar clústeres o de creación de reflejo para mejorar la escalabilidad y disponibilidad.
- El entorno de ensayo está diseñado para replicar la configuración del entorno de producción lo más fielmente posible.
- Las directivas de aislamiento de red y firewall no permiten la implementación directa y automatizada desde la intranet a la red perimetral.

La configuración de cada uno de estos entornos se describe con más detalle en el segundo tutorial, [configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Roles de equipo de ALM

Estos usuarios están implicados para crear, administrar, generar y publicar la solución póngase en contacto con el administrador:

- Matt Hink es un desarrollador de aplicaciones web en Fabrikam, Inc. Es parte del equipo que se desarrolló la solución póngase en contacto con el administrador mediante el uso de Visual Studio 2010. Matt tiene derechos completos de administrador en los servidores en el entorno de prueba para desarrolladores, que le permite configurar el entorno para satisfacer sus necesidades. También tiene acceso de usuario a la instancia de Visual Studio 2010 TFS donde almacena el código fuente de la solución póngase en contacto con el administrador.
- Rob Walters es un administrador del servidor para el equipo de desarrollo de Fabrikam, Inc. Antonio tiene acceso administrativo en el servidor TFS para que pueda configurar todos los aspectos de TFS y Team Build. Antonio también tiene acceso administrativo a la prueba y los servidores web de ensayo y actúa como el Administrador de base de datos (DBA) para los servidores de base de datos de la prueba y entornos de ensayo. Antonio configuró Team Build en el servidor TFS para llevar a cabo estas tareas:

    - Compilar y ejecutar pruebas unitarias en la aplicación cada vez que un usuario protege un archivo en TFS. Esto se denomina elemento de configuración.
    - Implementar la aplicación póngase en contacto con el administrador para el entorno de prueba automáticamente una vez que la aplicación pasa las pruebas unitarias. Esto incluye la publicación de la base de datos en los servidores de prueba en la implementación inicial y las actualizaciones de la base de datos después de la implementación inicial.
    - Implementar la aplicación póngase en contacto con el administrador para el entorno de ensayo en un proceso paso a paso.
    - Crear un paquete de Web que pueden utilizar un administrador del servidor Web y un DBA para publicar la aplicación en el entorno de producción.
- Lisa Andrews es un administrador del servidor responsable de implementar aplicaciones en los servidores de producción de Fabrikam, Inc. Tiene acceso de lectura al recurso compartido en el equipo de TFS Build almacena el paquete de implementación web una vez que genere la aplicación póngase en contacto con el administrador. También tiene acceso administrativo a los servidores web de producción para que ella pueda implementar la aplicación en producción. Además, actúa como el DBA que implementa las bases de datos y las actualizaciones de base de datos en el servidor de base de datos en el entorno de producción.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La solución póngase en contacto con el administrador

La solución de ponerse en contacto con el administrador está diseñada para permitir que los usuarios registrados, ha iniciado la sesión agregar y editar información de contacto a través de una interfaz web. La solución de Contact Manager consta de cuatro proyectos individuales:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Se trata de un proyecto de aplicación web de ASP.NET MVC3 que representa el punto de entrada para la solución. Ofrece algunas funcionalidades de aplicación web básico, como proporcionar a los usuarios la capacidad de crear y ver los detalles de contacto. La aplicación se basa en un servicio de Windows Communication Foundation (WCF) para administrar contactos y una base de datos de servicios de aplicación de ASP.NET para administrar la autenticación y autorización.
- **ContactManager.Database**. Se trata de un proyecto de base de datos de Visual Studio 2010. El proyecto que define el esquema para una base de datos que almacena los detalles de contacto.
- **ContactManager.Service**. Se trata de un proyecto de servicio web WCF. La expone WCF crea un punto de conexión que permite a los llamadores realizar, recuperar, actualizar y eliminar operaciones (CRUD) en la base de datos Póngase en contacto con el administrador. El servicio se basa en la base de datos de administrador de contacto y el ensamblado ContactManager.Common.dll.
- **ContactManager.Common**. Se trata de un proyecto de biblioteca de clases. El servicio WCF se basa en los tipos definidos en este ensamblado.

Una revisión completa de la solución y a sus requisitos de implementación se proporciona en el primer tutorial de esta serie, [implementación Web de la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tareas de implementación

Hay varias tareas distintas implicados en la implementación de aplicaciones en entornos diferentes en una organización grande. Estas son las tareas claves que cubren los tutoriales:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Esta es una lista de cada paso del proceso de implementación desde la perspectiva de los usuarios descritos anteriormente en este documento:

1. Todos los miembros del equipo de revisión la solución póngase en contacto con el Administrador de Visual Studio 2010 para determinar los problemas y los requisitos de implementación clave.
2. Matt Hink puede implementar la solución de Contact Manager directamente desde la estación de trabajo de desarrollador al llevar a cabo una prueba inicial de la lógica de implementación del entorno de prueba para desarrolladores.
3. Matt Hink agrega la aplicación a control de código fuente en TFS.
4. Rob Walters crea varias definiciones de compilación para la solución póngase en contacto con el Administrador de Team Build. Una definición de compilación usa el elemento de configuración para implementar la solución en el entorno de prueba para desarrolladores cada vez que un usuario protege el código nuevo. Otra definición de compilación permite a las implementaciones de desencadenador de los usuarios en el entorno de ensayo según sea necesario.
5. Cada vez que un usuario protege el código nuevo, Team Build automáticamente crea componentes de la solución, ejecute pruebas unitarias e implementa la solución en el entorno de prueba para desarrolladores si la compilación se realizó correctamente y la fase de pruebas unitarias.
6. Cuando un usuario desencadena una implementación en el entorno de ensayo, la solución se empaqueta e implementa en un proceso paso a paso. Este proceso también genera un paquete de la implementación manual en el entorno de producción.
7. Lisa Andrews implementa la aplicación en el entorno de producción mediante la importación manualmente el paquete de web que creó en el paso 6.

### <a name="key-deployment-issues"></a>Problemas de implementación clave

La solución póngase en contacto con el administrador y el escenario de Fabrikam, Inc. resaltan diversos problemas comunes y los desafíos que puede encontrarse al implementar complejo, soluciones de escala empresarial. Por ejemplo:

- Necesario poder implementar proyectos en varios entornos, como desarrollador o entornos de pruebas de almacenamiento provisional plataformas y servidores de producción. La solución debe implementarse con distintos valores de configuración para cada entorno.
- Debe implementar varios proyectos dependientes al mismo tiempo como parte de un proceso de compilación e implementación paso a paso o automatizado.
- Debe poder realizar la implementación de la unidad de un proceso automatizado. Por ejemplo, desea utilizar un proceso de elementos de configuración para implementar aplicaciones web en un entorno de ensayo al proteger el nuevo código.
- Debe ser capaz de controlar el proceso de implementación y establecer las variables de implementación desde fuera de Visual Studio, como a los desarrolladores no suelen tener los valores de configuración correctos o las credenciales necesarias para cada entorno de destino.
- Debe implementar proyectos de base de datos basada en el esquema y conservar los datos existentes en las implementaciones posteriores.
- Debe implementar las bases de datos de pertenencia de manera ad hoc sin implementar datos de la cuenta de usuario. También tendrá que actualizar el esquema de bases de datos de pertenencia implementado sin perder datos de la cuenta de usuario existente.
- Debe excluir determinados archivos o carpetas al implementar contenido en varios entornos de destino.

Además, administración de implementación cuando haya actualizaciones incrementales y frecuentes levanta algunos desafíos adicionales. Por ejemplo:

- Ejecutar pruebas unitarias cada vez que un desarrollador protege en el nuevo código. Solo desea implementar la solución si el código pasa las pruebas unitarias.
- Al implementar una aplicación web en un entorno de ensayo o de producción, que desea redirigir a los usuarios un *aplicación\_offline.htm* archivo durante el proceso de implementación.
- Que desee registrar las actividades de implementación. El proceso de implementación debe enviar notificaciones de correo electrónico de las implementaciones correctas o incorrecto a destinatarios designados.
- Si se produce un error en una implementación automatizada, el proceso de implementación debe reintentar la implementación actual o implementar el paquete de web anterior en su lugar.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Siguiente](application-lifecycle-management-from-development-to-production.md)
