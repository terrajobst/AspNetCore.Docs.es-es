---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Elegir el enfoque adecuado para la implementación Web | Microsoft Docs
author: jrjlee
description: Cuando se trabaja con (Web Deploy) la herramienta de implementación de Internet Information Services (IIS) Web 2.0 o posterior, hay tres enfoques principales que puede usar para obtener...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367750"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Elegir el enfoque adecuado para la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cuando se trabaja con (Web Deploy) la herramienta de implementación de Internet Information Services (IIS) Web 2.0 o posterior, hay tres enfoques principales que puede usar para obtener las aplicaciones web empaquetados en un servidor web. Puede:
> 
> - Implementar la aplicación desde una ubicación remota estableciendo como destino el *servicio del agente de implementación Web* (también conocido como el "agente remoto") en el servidor de destino.
> - Implementar la aplicación desde una ubicación remota mediante la implementación de Web a petición (también conocido como "temporal agente").
> - Implementar la aplicación desde una ubicación remota estableciendo como destino el *controlador de implementación Web de IIS* en el servidor de destino.
> - Implemente la aplicación manualmente, copiar el paquete web en el servidor de destino e importándolos a través del Administrador de IIS.
> 
> Cómo configurar los servidores web de destino dependerá de qué enfoque de implementación que desea usar. En este tema le ayudará a decidir qué enfoque de implementación es adecuada para usted.


Esta tabla muestra las principales ventajas y desventajas de cada enfoque de implementación, junto con los escenarios que normalmente se adapten a cada enfoque.

| Enfoque | Ventajas | Desventajas | Escenarios típicos |
| --- | --- | --- | --- |
| Agente remoto | Es fácil de configurar. Es adecuado para las actualizaciones periódicas para las aplicaciones web y el contenido. | El usuario debe ser administrador en el servidor de destino. el usuario no puede proporcionar credenciales alternativas. | Entornos de desarrollo. Entornos de prueba. |
| Agente temporal | No hay ninguna necesidad de instalar Web Deploy en el equipo de destino. Automáticamente se usa la versión más reciente de Web Deploy. | El usuario debe ser administrador en el servidor de destino. El usuario no puede proporcionar credenciales alternativas. | Entornos de desarrollo. Entornos de prueba. |
| Controlador de implementación Web | Los usuarios no administradores pueden implementar el contenido. Es adecuado para las actualizaciones periódicas para las aplicaciones web y el contenido. | Es mucho más difícil de configurar. | Entornos de ensayo. Entornos de producción de la intranet. Entornos hospedados. |
| Implementación sin conexión | Es muy fácil de configurar. Es adecuado para entornos aislados. | El administrador del servidor manualmente debe copiar e importar el paquete web cada vez. | Entornos de producción a través de Internet. Entornos aislados de red. |
  

## <a name="using-the-remote-agent"></a>Mediante el agente remoto

Al instalar Web Deploy con la configuración predeterminada en un servidor de destino, el servicio de agente de implementación Web (el "agente remoto") automáticamente instalado e iniciado. De forma predeterminada, el agente remoto expone un extremo HTTP en esta dirección:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Puede reemplazar [*server*] con el nombre del equipo del servidor web, una dirección IP para el servidor web o un nombre de host que se resuelve en el servidor web.


Los administradores de servidor pueden implementar paquetes web desde una ubicación remota, como una máquina de desarrollo o en un servidor de compilación especificando la dirección de punto de conexión. Por ejemplo, suponga que Matt Hink en Fabrikam, Inc. ha generado el proyecto de aplicación web ContactManager.Mvc en su máquina de desarrollador. El proceso de compilación genera un paquete web, junto con un *. deploy.cmd* archivo que contiene los comandos de Web Deploy, debe para instalar el paquete. Si Matt es un administrador del servidor en el servidor TESTWEB1, puede implementar la aplicación web para el servidor web de prueba, ejecute este comando en su equipo de desarrollo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


En realidad, el archivo ejecutable de Web Deploy puede deducir la dirección de punto de conexión del agente remoto si proporciona el nombre del equipo, por lo que solo debe Matt escriba esto:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Para obtener más información sobre la sintaxis de línea de comandos de Web Deploy y *. deploy.cmd* archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


El agente remoto ofrece una manera sencilla de implementar el contenido desde una ubicación remota, y este enfoque puede funcionar bien con la implementación automatizada o de un solo clic. Sin embargo, el usuario que ejecuta el comando de implementación también debe ser un administrador de dominio o un miembro del grupo Administradores local en el servidor de destino. Además, el agente remoto no admite la autenticación básica, por lo que no se puede pasar credenciales alternativas en la línea de comandos.

El agente remoto proporciona un enfoque útil para la implementación en escenarios de prueba, donde no es raro que los desarrolladores a tener control de administrador total en un entorno de servidor de prueba y las aplicaciones normalmente se vuelve a generar y volver a implementar muy o desarrollo con frecuencia. Sin embargo, este enfoque es generalmente menos aceptable para entornos de ensayo o producción.

Para obtener un ejemplo de extremo a otro de un escenario que usa el enfoque de agente remoto, consulte [escenario: configurar un entorno de prueba para la implementación de Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Mediante el agente de Temp

El enfoque de agente temporal para la implementación es similar al enfoque de agente remoto. Sin embargo, en contraste con el enfoque de agente remoto, no necesita instalar Web Deploy en el servidor web de destino. En su lugar, al realizar la implementación, Web Deploy se instalará una versión temporal del servicio del agente de implementación web en el servidor de destino y lo usará para implementar su contenido en IIS. Una vez completada la implementación, se quitan todos los archivos temporales.

Si desea utilizar la configuración de proveedor temporal del agente, agregue el **/g** marca a su comando de implementación:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> No puede usar al agente temporal si está instalado el servicio del agente de implementación web en el equipo de destino, incluso si no se está ejecutando el servicio.


La ventaja de este enfoque es que no es necesario mantener las instalaciones de Web Deploy en los servidores de destino. Además, no es necesario para asegurarse de que los equipos de origen y destino se está ejecutando la misma versión de Web Deploy. Sin embargo, este método tiene las mismas limitaciones de entidad de seguridad que el enfoque de agente remoto, es decir, que debe ser un administrador local en el servidor de destino con el fin de implementar el contenido y se admite únicamente la autenticación NTLM. El enfoque de agente temporal también requiere configuración mucho más inicial del entorno de destino.

Para obtener más información acerca de cómo utilizar el agente temporal, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx) y [implementar Web bajo demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Uso de la Web Deploy controlador

Para IIS 7 y versiones posteriores, Web Deploy ofrece un enfoque de implementación alternativo a través del controlador de implementación Web de IIS. El controlador de implementación Web está estrechamente integrado con el servicio de administración IIS Web (WMSvc), que está diseñado para permitir a los usuarios administrar sitios Web de IIS desde ubicaciones remotas.

De forma predeterminada, el agente remoto expone un extremo HTTP en esta dirección:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Puede reemplazar [*server*] con el nombre del equipo del servidor web, una dirección IP para el servidor web o un nombre de host que se resuelve en el servidor web.


La gran ventaja del controlador de implementación Web sobre el agente remoto y el agente temporal, es que puede configurar IIS para permitir que los usuarios sin privilegios de administrador implementar aplicaciones y contenido a determinados sitios Web IIS. El controlador de implementación Web también admite la autenticación básica, por lo que puede proporcionar credenciales alternativas como parámetros en los comandos de Web Deploy. El principal inconveniente es que el controlador de implementación Web es inicialmente mucho más complicado instalar y configurar.

En el caso de los usuarios sin privilegios de administrador, el servicio de administración Web (WMSvc) sólo permitirá al usuario para conectarse a IIS mediante una conexión de nivel de sitio, en lugar de una conexión de nivel de servidor. Para obtener acceso a un sitio determinado, puede incluir una cadena de consulta específica del sitio en la dirección del extremo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Por ejemplo, suponga que un proceso de compilación está configurado para implementar automáticamente una aplicación web en un entorno de ensayo después de cada compilación correcta. Si utiliza el enfoque de agente remoto, deberá realizar un administrador de la identidad del proceso de compilación en los servidores de destino. En cambio, con el enfoque de implementación de controlador de Web puede dar un usuario sin privilegios de administrador&#x2014;**FABRIKAM\stagingdeployer** en este caso&#x2014;permiso para un específico sitio Web de IIS solo y el proceso de compilación puede proporcionar estos credenciales para implementar el paquete de web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Para obtener más información sobre las operaciones de línea de comandos de Web Deploy y la sintaxis, vea [referencia de línea de comandos de implementación Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obtener más información sobre el uso de la *. deploy.cmd* de archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


El controlador de implementación Web proporciona un enfoque útil para la implementación en entornos, entornos hospedados y entornos de producción basados en intranet, donde el acceso remoto al servidor está disponible, pero las credenciales de administrador no son de almacenamiento provisional.

Para obtener un ejemplo de extremo a otro de un escenario que usa el enfoque de implementación de controlador de Web, consulte [escenario: configurar un entorno de ensayo para la implementación de Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Mediante la implementación sin conexión

En algunos casos, no es posible ni práctico implementar aplicaciones y contenido en un sitio Web de IIS desde una ubicación remota. Por ejemplo, los equipos de origen y de destino pueden estar en redes aisladas o segmentos de red o directiva de firewall podría no permitir el acceso remoto.

En escenarios como estos, todavía puede usar el empaquetado y publicación de las capacidades de Web Deploy; simplemente no puede usarlos desde una ubicación remota. En su lugar, un administrador del servidor de destino debe copiar el paquete web en el servidor e impórtelo a través del Administrador de IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

El enfoque de implementación sin conexión es suele ser útil en entornos de producción a través de Internet, donde los servidores en una red perimetral que tiene una conectividad restringida con los equipos de la red interna.

Para obtener un ejemplo de extremo a otro de un escenario que usa el enfoque de implementación sin conexión, consulte [escenario: configurar un entorno de producción para la implementación Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre las operaciones de línea de comandos de Web Deploy y la sintaxis, vea [referencia de línea de comandos de implementación Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obtener más información sobre el uso de la *. deploy.cmd* de archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Para obtener instrucciones más general sobre las diferentes maneras en las que puede implementar paquetes web desde un equipo remoto, consulte [utilizando Web implementar remotamente](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Para obtener más información sobre el uso de implementación Web a petición, consulte [implementar Web bajo demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-server-environments-for-web-deployment.md)
> [Siguiente](scenario-configuring-a-test-environment-for-web-deployment.md)
