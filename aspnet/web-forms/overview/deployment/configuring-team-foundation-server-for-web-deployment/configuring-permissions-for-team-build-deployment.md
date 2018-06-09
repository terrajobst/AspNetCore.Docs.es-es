---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurar permisos de equipo de implementación de compilaciones | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo configurar permisos para habilitar el servidor de compilación implementar contenido en servidores web y servidores de base de datos como parte de una b automatizada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890286"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configurar permisos de equipo de implementación de compilaciones
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar permisos para habilitar el servidor de compilación implementar contenido en servidores web y servidores de base de datos como parte de un proceso automatizado de compilación.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Cuando se instala el servicio de Team Foundation Server (TFS) 2010 compilación, especifique la identidad con la que desea que el servicio para ejecutarse. De forma predeterminada, se trata de la cuenta de servicio de red. Como alternativa, puede configurar el servicio de compilación para que se ejecute con una cuenta de dominio.

Las tareas de implementación que requieren autenticación de Windows y que tiene previsto automatizar mediante Team Build, ejecutarán con la identidad de servicio de compilación. Por lo tanto, debe conceder los permisos necesarios en los servidores web y los servidores de base de datos de la identidad del servicio de compilación.

> [!NOTE]
> La cuenta de servicio de red usa la cuenta de equipo para autenticarse en otros equipos. Cuentas de equipo que adoptan la forma * [nombre de dominio]\[nombre de equipo] ***$**&#x2014;por ejemplo, **FABRIKAM\TFSBUILD$**. Por lo tanto, si el servicio de compilación se ejecuta con la identidad Network Service, debe conceder los permisos necesarios para la identidad de la cuenta de equipo para el servidor de compilación.


## <a name="configuring-web-server-permissions"></a>Configurar permisos de servidor Web

Como se describe en [la elección del enfoque de derecha a la implementación de Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), hay dos enfoques principales que puede usar si desea implementar paquetes de web en un servidor web remoto:

- Implementar la aplicación desde una ubicación remota mediante la orientación de la *servicio Web Deployment Agent* (también conocido como el agente remoto) en el servidor de destino.
- Implementar la aplicación desde una ubicación remota mediante la orientación de la *Internet Information Services* (*IIS) el controlador de implementación Web* en el servidor de destino.

El agente remoto tiene dos limitaciones más importantes en este caso:

- El agente remoto admite únicamente la autenticación NTLM. En otras palabras, la implementación debe utilizar la identidad del servicio de compilación&#x2014;no puede suplantar a otra.
- Para usar al agente remoto, la cuenta que realiza la implementación debe ser un administrador en el servidor de destino.

Juntas, estas dos limitaciones forman el enfoque de agente remoto deseable para una implementación automatizada de Team Build. Para utilizar este enfoque, se debe hacer que el servicio de compilación de la cuenta Administrador en los servidores web de destino.

En cambio, el método de controlador de implementación Web ofrece diversas ventajas:

- El controlador de implementación Web admite la autenticación básica a través de HTTPS, lo que permite pasar las credenciales de una cuenta alternativa a la herramienta de implementación Web de IIS (Web Deploy).
- Puede configurar servidores web de destino para permitir que los usuarios sin privilegios de administrador implementar contenido en sitios Web específicos de IIS mediante el controlador de implementación Web.

Como resultado, es preferible claramente como destino el controlador de implementación Web para automatizar la implementación de paquetes de web de Team Build. Éste es el proceso recomendado:

1. Cree una cuenta de dominio con pocos privilegios que se usará para la implementación.
2. Configurar el controlador de implementación Web y conceda a la cuenta los permisos necesarios para implementar contenido en un sitio Web IIS específico, como se describe en [configurar un servidor Web de publicación de implementar en Web (Web implementar controlador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invocación de Web Deploy y el controlador de implementación Web de destino, mediante la autenticación básica y proporcionar las credenciales de la cuenta de dominio ha creado, para llevar a cabo la implementación.

En el [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) ejemplo solución, especifique el tipo de autenticación (básica o NTLM), las credenciales de implementación Web y la dirección del extremo (agente remoto o el controlador de implementación Web) en el archivo de proyecto específicos del entorno. Estos valores se utilizan para formular y ejecutar un comando de Web Deploy cuando se ejecuta el archivo de proyecto. Para obtener más información, consulte [implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obtener más información acerca de cómo configurar el controlador de implementación Web, incluido cómo configurar permisos, consulte [configurar un servidor Web de publicación de implementar en Web (Web implementar controlador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obtener más información acerca de cómo configurar el agente remoto, consulte [configurar un servidor Web de publicación de implementar en Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurar permisos de servidor de base de datos

Para implementar una base de datos en SQL Server, debe:

- Cree un inicio de sesión para la cuenta de implementación en la instancia de SQL Server.
- Conceder el inicio de sesión **DBCreator** permisos en la instancia de SQL Server.
- Después de la implementación inicial, agregue el inicio de sesión para la **db\_propietario** rol en la base de datos de destino. Esto es necesario porque en las implementaciones posteriores, se está modificando una base de datos existente en lugar de crear una nueva base de datos.

Puede autenticar a una instancia de SQL Server mediante la autenticación NTLM o autenticación de SQL Server:

- Si utiliza la autenticación NTLM, deberá conceder los permisos que se ha descrito anteriormente para la cuenta de servicio de compilación.
- Si utiliza la autenticación de SQL Server, deberá conceder los permisos que se ha descrito anteriormente para la cuenta de SQL Server. También debe incluir el nombre de usuario de SQL Server y la contraseña en la cadena de conexión que se usa para implementar la base de datos.

Para obtener información detallada paso a paso sobre cómo configurar permisos para la implementación de la base de datos, vea [configurar un servidor de base de datos de publicación en Web implementar](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusión

En este punto, debe entender los permisos necesarios, junto con las opciones de autenticación abiertas, para automatizar las implementaciones de aplicación y base de datos de web de Team Build. También debe ser capaz de implementar los permisos necesarios en los servidores web de IIS y los servidores de base de datos de SQL Server.

## <a name="further-reading"></a>Información adicional

Para obtener más información acerca de cómo configurar entornos de servidor de Windows para permitir la implementación remota, vea [configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
