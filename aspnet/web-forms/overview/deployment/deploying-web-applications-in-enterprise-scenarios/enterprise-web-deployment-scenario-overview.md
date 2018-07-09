---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Implementación Web de empresa: Información general del escenario | Microsoft Docs'
author: jrjlee
description: Este conjunto de tutoriales usa una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar a una referencia...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ce533b246b14c9e041c89a778c022784f09d01a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836751"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Implementación Web de empresa: Información general del escenario
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriales usa una solución de ejemplo con un nivel de complejidad, junto con un escenario de implementación de la empresa ficticia, realista para proporcionar una implementación de referencia y para proporcionar a las tareas y los tutoriales de un contexto común. En este tema se describe el escenario del tutorial y presenta la solución de ejemplo.


## <a name="scenario-description"></a>Descripción del escenario

Fabrikam, Inc., una compañía ficticia, está creando una solución que permite a los equipos de ventas remotos almacenar y recuperar información de contacto de una interfaz web.

Los procesos de Application Lifecycle Management (ALM) en Fabrikam, Inc. requieren la solución para su implementación en tres entornos de servidor en distintas fases del proceso de desarrollo de software:

- Un entorno de prueba o "espacio aislado" para desarrolladores.
- Un entorno de ensayo basados en intranet.
- Un entorno de producción a través de Internet.

Cada uno de estos entornos tiene diferentes configuraciones y los requisitos de seguridad, y cada uno presenta desafíos de implementación único.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam, Inc. Infraestructura de servidor

Se trata de la infraestructura de desarrollo e implementación de alto nivel en Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Las estaciones de trabajo de desarrollador, la infraestructura de control de origen, el entorno de prueba para desarrolladores y el entorno de ensayo que residen en la red de intranet dentro del dominio Fabrikam.net. El entorno de producción se encuentra en una red perimetral (también conocida como DMZ, zona desmilitarizada y subred filtrada), que está aislada de la red de intranet por un firewall. Se trata de un escenario de implementación comunes: normalmente, para aislar los servidores de web con conexión a Internet desde su infraestructura de servidor interno mediante el uso de firewalls o servidores de puerta de enlace.

En este ejemplo:

- Un servidor de Team Foundation Server (TFS) 2010 con un servidor de compilación independiente proporciona control de código fuente y la funcionalidad de integración continua (CI).
- El entorno de prueba para desarrolladores incluye un servidor web de Internet Information Services (IIS) 7.5 y un servidor de base de datos de SQL Server 2008 R2.
- El entorno de producción incluye varios servidores web de IIS 7.5 sincronizados por un servidor de controlador de Web Farm Framework (WFF), junto con un servidor de base de datos de SQL Server 2008 R2. En la práctica, el servidor de base de datos puede usar clústeres o la creación de reflejo para mejorar la escalabilidad y disponibilidad.
- El entorno de ensayo está diseñado para replicar la configuración del entorno de producción lo más fielmente posible.
- Las directivas de aislamiento de red y firewall no permiten la implementación directa y automatizada desde la intranet a la red perimetral.

La configuración de cada uno de estos entornos se describe con más detalle en el segundo tutorial, [configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Roles de equipo para ALM

Estos usuarios implicados en la creación, administración, compilar y publicar la solución Contact Manager:

- Matt Hink es un desarrollador de aplicaciones web en Fabrikam, Inc. Es parte del equipo que desarrolla la solución Contact Manager con Visual Studio 2010. Matt tenga derechos administrativos completos en los servidores en el entorno de prueba para desarrolladores, que le permite configurar el entorno para satisfacer sus necesidades. También tiene acceso de usuario a la instancia de Visual Studio 2010 TFS donde almacena el código fuente de la solución Contact Manager.
- Rob Walters es un administrador del servidor para el equipo de desarrollo de Fabrikam, Inc. Rob tiene acceso administrativo en el servidor TFS para que pueda configurar todos los aspectos de TFS y Team Build. Rob también tiene acceso administrativo a la prueba y los servidores web de ensayo y actúa como el Administrador de base de datos (DBA) para los servidores de base de datos en entornos de ensayo y la prueba. Rob configuró Team Build en el servidor TFS para llevar a cabo estas tareas:

    - Compilar y ejecutar pruebas unitarias en la aplicación cada vez que un usuario protege un archivo a TFS. Esto se denomina CI.
    - Implementar la aplicación de Contact Manager en el entorno de prueba automáticamente una vez que la aplicación pasa las pruebas unitarias. Esto incluye la publicación de la base de datos en los servidores de prueba en la implementación inicial y las actualizaciones de la base de datos después de la implementación inicial.
    - Implementar la aplicación de Contact Manager en el entorno de ensayo en un proceso paso a paso.
    - Crear un paquete de Web que pueden usar un administrador del servidor Web y un DBA para publicar la aplicación en el entorno de producción.
- Lisa Andrews es responsable de implementar aplicaciones en los servidores de producción de Fabrikam, Inc., un administrador del servidor. Tiene acceso de lectura al recurso compartido donde TFS Team Build almacena el paquete de implementación web una vez que compila la aplicación de Contact Manager. También tiene acceso administrativo a los servidores web de producción para que ella pueda implementar la aplicación en producción. Además, actúa como el DBA que implementa las bases de datos y las actualizaciones de la base de datos en el servidor de base de datos en el entorno de producción.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La solución Contact Manager

La solución Contact Manager está diseñada para permitir que los usuarios registrados, ha iniciado sesión agregar y editar información de contacto a través de una interfaz web. La solución Contact Manager consta de cuatro proyectos individuales:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Se trata de un proyecto de aplicación web de ASP.NET MVC3 que representa el punto de entrada para la solución. Ofrece algunas características de aplicación web básica, como proporcionar a los usuarios la capacidad de crear y ver los detalles de contacto. La aplicación se basa en un servicio de Windows Communication Foundation (WCF) para administrar los contactos y una base de datos de servicios de aplicación de ASP.NET para administrar la autenticación y autorización.
- **ContactManager.Database**. Se trata de un proyecto de base de datos de Visual Studio 2010. El proyecto define el esquema de una base de datos que almacena los detalles de contacto.
- **ContactManager.Service**. Se trata de un proyecto de servicio web WCF. El expone WCF crea un punto de conexión que permite a los llamadores realizar, recuperar, actualizar y eliminar operaciones (CRUD) en la base de datos de Contact Manager. El servicio se basa en la base de datos de Contact Manager y el ensamblado ContactManager.Common.dll.
- **ContactManager.Common**. Se trata de un proyecto de biblioteca de clases. El servicio de WCF se basa en tipos definidos en este ensamblado.

Una revisión completa de la solución y sus requisitos de implementación se proporciona en el primer tutorial de esta serie, [implementación Web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tareas de implementación

Hay varias tareas distintas necesarios para implementar aplicaciones en distintos entornos en una organización grande. Estas son las tareas claves que cubren los tutoriales:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Esta es una lista de cada paso del proceso de implementación desde la perspectiva de los usuarios que se describió anteriormente en este documento:

1. Todos los miembros del equipo de revisión de la solución Contact Manager en Visual Studio 2010 para determinar los requisitos de implementación de la clave y problemas.
2. Matt Hink puede implementar la solución Contact Manager directamente desde la estación de trabajo de desarrollador para el entorno de prueba para desarrolladores, para llevar a cabo una prueba inicial de la lógica de implementación.
3. Matt Hink agrega la aplicación para el control de código fuente en TFS.
4. Rob Walters crea varias definiciones de compilación para la solución Contact Manager en Team Build. Una definición de compilación usa CI para implementar la solución en el entorno de prueba para desarrolladores cada vez que un usuario proteja nuevo código. Otra definición de compilación permite a los usuarios desencadenar implementaciones en el entorno de ensayo según sea necesario.
5. Cada vez que un usuario proteja nuevo código, Team Build automáticamente los componentes de la solución se basa, ejecuta las pruebas unitarias e implementa la solución en el entorno de prueba para desarrolladores si la compilación se realizó correctamente y la superación de las pruebas unitarias.
6. Cuando un usuario activa una implementación en el entorno de ensayo, la solución se empaqueta e implementa en un proceso paso a paso. Este proceso también genera un paquete de la implementación manual en el entorno de producción.
7. Lisa Andrews implementa la aplicación en el entorno de producción mediante la importación manualmente el paquete de web que creó en el paso 6.

### <a name="key-deployment-issues"></a>Problemas de implementación clave

La solución Contact Manager y el escenario de Fabrikam, Inc. resaltan diversos problemas comunes y retos que pueden surgir al implementar complejo, soluciones de escala empresarial. Por ejemplo:

- Deberá ser capaz de implementar proyectos en varios entornos, como desarrollador o probar entornos, plataformas y los servidores de producción de almacenamiento provisional. La solución debe implementarse con una configuración distinta para cada entorno.
- Deberá implementar varios proyectos dependientes simultáneamente como parte del proceso de compilación e implementación automatizado o de paso a paso.
- Deberá poder realizar la implementación de la unidad desde un proceso automatizado. Por ejemplo, desea utilizar un proceso de CI para implementar aplicaciones web en un entorno de ensayo cuando se proteja nuevo código.
- Debe ser capaz de controlar el proceso de implementación y establecer las variables de implementación desde fuera de Visual Studio, como desarrolladores están poco probable que tengan los valores de configuración correctos o las credenciales necesarias para cada entorno de destino.
- Deberá implementar proyectos de base de datos basada en el esquema y conservar los datos existentes en las implementaciones posteriores.
- Deberá implementar bases de datos de pertenencia de forma ad hoc sin necesidad de implementar datos de la cuenta de usuario. Es posible que también deberá actualizar el esquema de bases de datos de pertenencia implementado sin perder datos de la cuenta de usuario existente.
- Tendrá que excluir determinados archivos o carpetas al implementar contenido en distintos entornos de destino.

Además, administrar la implementación cuando hay actualizaciones incrementales y frecuentes levanta algunos retos adicionales. Por ejemplo:

- Ejecutar pruebas unitarias cada vez que un desarrollador protege en el nuevo código. Solo desea implementar la solución si el código pasa las pruebas unitarias.
- Al implementar una aplicación web en un entorno de ensayo o producción, que desee redirigir a los usuarios un *aplicación\_offline.htm* archivo para la duración del proceso de implementación.
- Desea registrar las actividades de implementación. El proceso de implementación debe enviar notificaciones de correo electrónico de las implementaciones correctas o erróneas a destinatarios designados.
- Si se produce un error en una implementación automatizada, el proceso de implementación debe reintentar la implementación actual o implementar el paquete web anterior en su lugar.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Siguiente](application-lifecycle-management-from-development-to-production.md)
