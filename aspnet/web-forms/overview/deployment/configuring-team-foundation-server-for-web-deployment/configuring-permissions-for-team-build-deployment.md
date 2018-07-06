---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurar permisos para el equipo de implementación de compilaciones | Microsoft Docs
author: jrjlee
description: Este tema describe cómo configurar permisos para habilitar el servidor de compilación implementar contenido en servidores web y servidores de base de datos como parte de una b automatizada...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: b577d887b4a4476b6796ae9f1df538d16eededa3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820365"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configurar permisos para el equipo de implementación de compilaciones
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo configurar permisos para habilitar el servidor de compilación implementar contenido en servidores web y servidores de base de datos como parte de un proceso de compilación automatizado.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

Cuando se instala el servicio de compilación 2010 Team Foundation Server (TFS), especifique la identidad con la que desea que el servicio para ejecutarse. De forma predeterminada, se trata de la cuenta de servicio de red. Como alternativa, puede configurar el servicio de compilación se ejecute con una cuenta de dominio.

Las tareas de implementación que requieren autenticación de Windows y que tiene previsto automatizar con Team Build, ejecutarán con la identidad de servicio de compilación. Por lo tanto, deberá conceder los permisos necesarios en los servidores web y los servidores de base de datos de la identidad de servicio de compilación.

> [!NOTE]
> La cuenta de servicio de red utiliza la cuenta de equipo para autenticarse en otros equipos. Cuentas de equipo que adoptan la forma * [nombre de dominio]\[nombre de la máquina] ***$**&#x2014;por ejemplo, **FABRIKAM\TFSBUILD$**. Por lo tanto, si el servicio de compilación se ejecuta con la identidad de servicio de red, debe conceder los permisos necesarios para la identidad de la cuenta de equipo para el servidor de compilación.


## <a name="configuring-web-server-permissions"></a>Configurar permisos de servidor Web

Como se describe en [elegir el enfoque de derecha a la implementación Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), hay dos enfoques principales que puede usar si desea implementar los paquetes de web en un servidor web remoto:

- Implementar la aplicación desde una ubicación remota estableciendo como destino el *servicio del agente de implementación Web* (también conocido como el agente remoto) en el servidor de destino.
- Implementar la aplicación desde una ubicación remota estableciendo como destino el *Internet Information Services* (*IIS) el controlador de implementación Web* en el servidor de destino.

El agente remoto tiene dos limitaciones más importantes en este caso:

- El agente remoto admite únicamente la autenticación NTLM. En otras palabras, la implementación debe usar la identidad de servicio de compilación&#x2014;no puede suplantar otra cuenta.
- Para utilizar al agente remoto, la cuenta que realiza la implementación debe ser un administrador del servidor de destino.

Juntas, estas dos limitaciones hacen que el agente remoto enfoque deseable para una implementación automatizada de Team Build. Para usar este enfoque, deberá hacer que el servicio de compilación de la cuenta de administrador en todos los servidores web de destino.

En cambio, el método de controlador de implementación Web ofrece diversas ventajas:

- El controlador de implementación Web admite la autenticación básica a través de HTTPS, que le permite pasar las credenciales de una cuenta alternativa a la herramienta de implementación Web de IIS (Web Deploy).
- Puede configurar los servidores web de destino para permitir que los usuarios sin privilegios de administrador implementar el contenido a determinados sitios Web IIS mediante el controlador de implementación Web.

Como resultado, es preferible claramente para el controlador de implementación Web de destino al automatizar la implementación de paquetes de web de Team Build. Este es el proceso recomendado:

1. Cree una cuenta de dominio con pocos privilegios que se usará para la implementación.
2. Configurar el controlador de implementación Web y conceda a la cuenta los permisos necesarios para implementar contenido en un sitio Web IIS específico, como se describe en [configurar un servidor Web para la publicación de implementar en Web (controlador de implementación Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invocación de Web Deploy y el controlador de implementación Web de destino, mediante la autenticación básica y proporcionar las credenciales de la cuenta de dominio que creó, para llevar a cabo la implementación.

En el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , solución de ejemplo especifica el tipo de autenticación (básica o NTLM), las credenciales de implementación Web y la dirección del extremo (agente remoto o el controlador de implementación Web) en el archivo de proyecto específicos del entorno. Estos valores se utilizan para formular y ejecutar un comando de Web Deploy cuando se ejecuta el archivo de proyecto. Para obtener más información, consulte [implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obtener más información sobre cómo configurar el controlador de implementación Web, incluido cómo configurar permisos, consulte [configurar un servidor Web para la publicación de implementar en Web (controlador de implementación Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obtener más información sobre cómo configurar el agente remoto, consulte [configurar un servidor Web para la publicación de implementar en Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurar permisos de servidor de base de datos

Para implementar una base de datos en SQL Server, debe:

- Cree un inicio de sesión para la cuenta de implementación en la instancia de SQL Server.
- Conceder el inicio de sesión **DBCreator** permisos en la instancia de SQL Server.
- Tras la implementación inicial, agregue el inicio de sesión para el **db\_propietario** rol en la base de datos de destino. Esto es necesario porque en las implementaciones posteriores, está modificando una base de datos existente en lugar de crear una nueva base de datos.

Puede autenticar a una instancia de SQL Server mediante la autenticación NTLM o autenticación de SQL Server:

- Si usa la autenticación NTLM, deberá conceder los permisos que se ha descrito anteriormente para la cuenta de servicio de compilación.
- Si utiliza la autenticación de SQL Server, deberá conceder los permisos que se ha descrito anteriormente para la cuenta de SQL Server. También deberá incluir el nombre de usuario de SQL Server y la contraseña en la cadena de conexión que use para implementar la base de datos.

Para obtener información detallada paso a paso sobre cómo configurar permisos para la implementación de la base de datos, vea [configurar un servidor de base de datos para publicación en Web implementar](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusión

En este momento, debe entender los permisos necesarios, junto con las opciones de autenticación abiertas para que, al automatizar las implementaciones de aplicación y base de datos de web de Team Build. También debe ser capaz de implementar los permisos necesarios en los servidores web IIS y los servidores de base de datos de SQL Server.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre cómo configurar entornos de servidor de Windows para admitir la implementación remota, vea [configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
