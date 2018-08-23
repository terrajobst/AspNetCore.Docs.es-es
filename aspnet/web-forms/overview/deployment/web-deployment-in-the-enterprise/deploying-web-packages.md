---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Implementar paquetes Web | Microsoft Docs
author: jrjlee
description: Este tema describe cómo puede publicar paquetes de implementación web en un servidor remoto mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836883"
---
<a name="deploying-web-packages"></a>Implementar paquetes Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo puede publicar paquetes de implementación web en un servidor remoto mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) 2.0.
> 
> Hay dos formas principales en el que puede implementar un paquete de web a un servidor remoto:
> 
> - Puede usar la utilidad de línea de comandos MSDeploy.exe directamente.
> - Puede ejecutar el *.deploy.cmd [nombre de proyecto]* archivo que genera el proceso de compilación.
> 
> El resultado final es el mismo independientemente del método que use. Básicamente, todos los *. deploy.cmd* archivo consiste en ejecutar MSDeploy.exe con algunos valores predeterminados, por lo que no tendrá que proporcionar tanta información con el fin de implementar el paquete. Esto simplifica el proceso de implementación. Por otro lado, usar MSDeploy.exe directamente le ofrece mucha más flexibilidad sobre exactamente cómo se implementa el paquete.
> 
> Método utilizado dependerá de diversos factores, incluido cuánto control necesita sobre el proceso de implementación y si escribimos para el servicio del agente remoto de implementación Web o el controlador de implementación Web. En este tema se explica cómo usar cada enfoque e identifica cuándo es apropiado cada enfoque.
> 
> Las tareas y los tutoriales en este tema se suponen:
> 
> - Ha compilado y empaqueta la aplicación web, como se describe en [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md).
> - Ha modificado el *SetParameters.xml* archivo para proporcionar los valores de parámetro correctos para su entorno de destino, como se describe en [configurar parámetros para la implementación del paquete Web](configuring-parameters-for-web-package-deployment.md).


Ejecutar el [*nombre del proyecto*]*. deploy.cmd* archivo es la manera más sencilla de implementar un paquete de web. En particular, mediante la *. deploy.cmd* archivo ofrece estas ventajas con respecto a MSDeploy.exe directamente:

- No es necesario especificar la ubicación del paquete de implementación web&#x2014;el *. deploy.cmd* archivo ya sepa dónde está.
- No es necesario especificar la ubicación de la *SetParameters.xml* archivo&#x2014;el *. deploy.cmd* archivo ya sepa dónde está.
- No es necesario especificar origen y destino proveedores MSDeploy&#x2014;el *. deploy.cmd* archivo ya sabe qué valores para su uso.
- No es necesario especificar la configuración de la operación de MSDeploy&#x2014;el *. deploy.cmd* archivo agrega automáticamente los valores requeridos habitualmente para el comando MSDeploy.exe.

Antes de usar el *. deploy.cmd* archivo para implementar un paquete web, debe asegurarse de que:

- El *. deploy.cmd* archivo, el [*nombre del proyecto*]. *SetParameters.xml* archivo y el paquete de web ([*nombre del proyecto*]. *zip*) están en la misma carpeta.
- Web Deploy (MSDeploy.exe) está instalado en el equipo que ejecuta el *. deploy.cmd* archivo.

El *. deploy.cmd* archivo es compatible con distintas opciones de línea de comandos. Al ejecutar el archivo desde un símbolo del sistema, ésta es la sintaxis básica:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Debe especificar un **/T** marca o un **/Y** marca para indicar si desea realizar una ejecución de prueba o una implementación en vivo, respectivamente (no utilice ambos marcadores en el mismo comando). Esta tabla explica el propósito de cada una de estas marcas.

| Marcar | Descripción |
| --- | --- |
| **/T** | Llama a MSDeploy.exe con la **– whatif** marca, que indica una ejecución de prueba. En lugar de implementar el paquete, crea un informe de lo que sucedería si se implementó el paquete. |
| **/Y** | Llama a MSDeploy.exe sin la **– whatif** marca. El paquete se implementa en el equipo local o el servidor de destino especificado. |
| **/M** | Especifica el servidor de destino el nombre o dirección URL del servicio. Para obtener más información sobre los valores que puede proporcionar aquí, consulte el **consideraciones de punto de conexión** en este tema. Si se omite el **/M** marca, el paquete se implementará en el equipo local. |
| **/A** | Especifica el tipo de autenticación que debe usar MSDeploy.exe para realizar la implementación. Los valores posibles son **NTLM** y **básica**. Si se omite el **/A** marca, el tipo de autenticación predeterminado es **NTLM** para la implementación en el servicio del agente remoto de implementación Web y **básica** para su implementación en la Web Deploy Controlador. |
| **/U** | Especifica el nombre de usuario. Esto se aplica solo si usa la autenticación básica. |
| **/P** | Especifica la contraseña. Esto se aplica solo si usa la autenticación básica. |
| **/ L** | Indica que se debe implementar el paquete a la instancia local de IIS Express. |
| **/ G** | Especifica que el paquete se implementa mediante el [configuración de proveedor tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Si se omite el **/G** marca, el valor predeterminado es **false**. |

> [!NOTE]
> Cada vez que el proceso de compilación crea un paquete web, también crea un archivo denominado *[nombre de proyecto] Deploy-readme.txt* que describen estas opciones de implementación.


Además de estas marcas, puede especificar la configuración de operación de Web Deploy como adicionales *. deploy.cmd* parámetros. Cualquier configuración adicional que especifique simplemente se pasa al comando MSDeploy.exe subyacente. Para obtener más información sobre estas opciones, consulte [Web implementar la configuración de operación](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Suponga que desea implementar el proyecto de aplicación web ContactManager.Mvc en un entorno de prueba mediante la ejecución de la *. deploy.cmd* archivo. El entorno de prueba está configurado para usar el servicio de agente remoto de implementación Web, como se describe en [configurar un servidor Web para la publicación de implementar en Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Para implementar la aplicación web, es preciso completar los pasos siguientes.

**Para implementar una aplicación web mediante el. archivo deploy.cmd**

1. Compilar y empaquetar el proyecto de aplicación web, como se describe en [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md).
2. Modificar el *ContactManager.Mvc.SetParameters.xml* contenga los valores de parámetro correctos para su entorno de prueba, tal como se describe en el archivo [configurar parámetros para la implementación del paquete Web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y desplácese hasta la ubicación de la *ContactManager.Mvc.deploy.cmd* archivo.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

En este ejemplo:

- El **/Y** marca indica que desea implementar realmente el paquete, en lugar de realizar una evaluación a ejecutar.
- El **/M** marca indica que desea implementar el paquete en el servidor denominado TESTWEB1. De este valor, se intentará implementar el paquete en el servicio del agente remoto de implementación Web en MSDeploy.exe http://TESTWEB1/MSDeployAgentService.
- El **/A** marca indica que desea utilizar la autenticación NTLM. Por lo tanto, no es necesario especificar un nombre de usuario y una contraseña.

Para ilustrar cómo, al utilizar el *. deploy.cmd* archivo simplifica el proceso de implementación, eche un vistazo en la línea de comandos MSDeploy.exe que obtiene genera y ejecuta al ejecutar *ContactManager.Mvc.deploy.cmd* uso de las opciones mostradas anteriormente.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Para obtener más información sobre el uso de la *. deploy.cmd* archivo para implementar un paquete de web, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Utiliza MSDeploy.exe

Aunque el uso de la *. deploy.cmd* archivo generalmente simplifica el proceso de implementación, existen algunas situaciones, cuando es preferible usar MSDeploy.exe directamente. Por ejemplo:

- Si desea implementar en el controlador de implementación Web como un usuario sin privilegios de administrador, no puede usar el *. deploy.cmd* archivo. Esto es debido a un error en la Web Deploy 2.0, como se describe en **consideraciones de punto de conexión**.
- Si desea cambiar manualmente entre distintos *SetParameters.xml* archivos en diferentes ubicaciones, quizás prefiera usar MSDeploy.exe directamente.
- Si desea invalidar varios argumentos de línea de comandos MSDeploy.exe, prefiere usar MSDeploy.exe directamente.

Cuando se usa MSDeploy.exe, deberá proporcionar tres piezas clave de información:

- Un **: origen** parámetro que indica de dónde provienen los datos.
- Un **– dest** parámetro que indica que los datos se van a.
- Un **: verbo** parámetro que indica la [operación](https://technet.microsoft.com/library/dd568989(WS.10).aspx) que desea realizar.

MSDeploy.exe se basa en [proveedores de Web Deploy](https://technet.microsoft.com/library/dd569040(WS.10).aspx) para procesar los datos de origen y destino. Implementación Web incluye una gran cantidad de proveedores que representa la gama de orígenes de datos y las aplicaciones puede trabajar con&#x2014;por ejemplo, existen proveedores para las bases de datos de SQL Server, servidores web IIS, certificados, los ensamblados de global de ensamblados (GAC) de la caché, varios archivos de configuración diferente y muchos otros tipos de datos. Tanto el **: origen** parámetro y el **– dest** parámetro debe especificar un proveedor, en el formulario **: origen**: [*providerName*] = [*ubicación*]. Al implementar un paquete web en un sitio Web IIS, debe usar estos valores:

- El **: origen** proveedor es siempre [paquete](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- El **– dest** proveedor es siempre [automática](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- El **: verbo** siempre **sincronización**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Además, deberá especificar varios otros [configuraciones específicas del proveedor](https://technet.microsoft.com/library/dd569001(WS.10).aspx) generales y [configuración de la operación](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Por ejemplo, suponga que desea implementar la aplicación web de ContactManager.Mvc en un entorno de ensayo. La implementación tendrá como destino el controlador de implementación Web y debe usar la autenticación básica. Para implementar la aplicación web, es preciso completar los pasos siguientes.

**Para implementar una aplicación web mediante MSDeploy.exe**

1. Compilar y empaquetar el proyecto de aplicación web, como se describe en [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md).
2. Modificar el *ContactManager.Mvc.SetParameters.xml* contenga los valores de parámetro correctos para su entorno de ensayo, como se describe en archivo [configurar parámetros para la implementación del paquete Web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y vaya a la ubicación de MSDeploy.exe. Esta es normalmente en %PROGRAMFILES%\IIS\Microsoft Web implementar V2\msdeploy.exe.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR (pasar por alto los saltos de línea):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

En este ejemplo:

- El **: origen** parámetro especifica el **paquete** proveedor e indica la ubicación del paquete web.
- El **– dest** parámetro especifica el **automática** proveedor. El **computerName** configuración proporciona la dirección URL del servicio del controlador de implementación Web en el servidor de destino. El **authtype** configuración indica que desea usar la autenticación básica y, por lo tanto debe proporcionar un **username** y un **contraseña**. Por último, el **includeAcls = "False"** valor indica que no desea copiar las listas de control de acceso (ACL) de los archivos en la aplicación web de origen al servidor de destino.
- El **: verbo: sincronización** argumento indica que desea replicar el contenido de origen en el servidor de destino.
- El **disableLink:** argumentos indican que no desea replicar los grupos de aplicaciones, la configuración de directorio virtual o certificados de capa de Sockets seguros (SSL) en el servidor de destino. Para obtener más información, consulte [Web Deploy vínculo Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- El **– setParamFile** parámetro proporciona la ubicación de la *SetParameters.xml* archivo.
- El **– allowUntrusted** modificador indica que Web Deploy debe aceptar certificados SSL que no hayan sido emitidos por una entidad de certificación de confianza. Si va a implementar en el controlador de implementación Web y ha usado un certificado autofirmado para proteger la dirección URL del servicio, deberá incluir este modificador.

## <a name="automating-web-package-deployment"></a>Automatizar la implementación de paquetes Web

En muchos escenarios empresariales, desea implementar los paquetes de web como parte de un solo paso más grande o implementación automatizada. Independientemente de si elige implementar los paquetes de web mediante la ejecución de la *. deploy.cmd* de archivo o directamente con MSDeploy.exe, puede parametrizar los comandos y llamarlos desde un destino en una Microsoft Build Engine (MSBuild) archivo de proyecto.

En la solución Contact Manager de ejemplo, eche un vistazo a la **PublishWebPackages** tener como destino en el *Publish.proj* archivo. Este destino se ejecuta una vez para cada *. deploy.cmd* archivo identificado por una lista de elementos denominada **PublishPackages**. El destino utiliza las propiedades y metadatos de elementos para crear un conjunto completo de valores de argumento para cada *. deploy.cmd* archivo y, a continuación, usa el **Exec** tarea para ejecutar el comando.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Información general más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizadas en general, consulte [descripción del archivo de proyecto](understanding-the-project-file.md) y [descripción del proceso de compilación](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Consideraciones de punto de conexión

Independientemente de si implementa el paquete web ejecutando el *. deploy.cmd* de archivo o directamente con MSDeploy.exe, deberá especificar un nombre de equipo o un punto de conexión de servicio para la implementación.

Si el servidor web de destino está configurado para la implementación con el servicio del agente remoto de implementación Web, especifique la dirección URL de servicio de destino como el destino.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Como alternativa, puede especificar el nombre del servidor por sí sola como destino y Web Deploy deducirá la dirección URL del servicio del agente remoto.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Si el servidor web de destino está configurado para la implementación con el controlador de implementación Web, deberá especificar la dirección del extremo del servicio de administración de IIS Web (WMSvc) como destino. De forma predeterminada, esto toma la forma:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Puede dirigirse a cualquiera de estos puntos de conexión mediante el *. deploy.cmd* MSDeploy.exe directamente o archivo. Sin embargo, si desea implementar en el controlador de implementación Web como un usuario sin privilegios de administrador, como se describe en [configurar un servidor Web para la publicación de implementar en Web (controlador de implementación Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), deberá agregar una cadena de consulta a la dirección del punto de conexión de servicio.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Esto es porque el usuario sin privilegios de administrador no tiene acceso de nivel de servidor en IIS. solo tiene acceso a un sitio Web IIS específico. En el momento de escribir, debido a un error en la canalización de publicación Web (WPP), no se puede ejecutar el *. deploy.cmd* de archivos mediante una dirección de punto de conexión que incluye una cadena de consulta. En este escenario, deberá implementar el paquete web directamente mediante MSDeploy.exe.

> [!NOTE]
> Para obtener más información sobre el servicio del agente remoto de implementación Web y el controlador de implementación de Web, consulte [elegir el enfoque de derecha a la implementación Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Para obtener instrucciones sobre cómo configurar los archivos de proyecto específicos del entorno para implementar en estos puntos de conexión, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Consideraciones sobre la autenticación

Independientemente de si implementa el paquete web ejecutando el *. deploy.cmd* de archivo o directamente con MSDeploy.exe, deberá especificar un tipo de autenticación. Web Deploy acepta dos valores posibles: **NTLM** o **básica**. Si especifica la autenticación básica, también deberá proporcionar un nombre de usuario y una contraseña. Hay diversos factores que deben tener en cuenta al seleccionar un tipo de autenticación:

- Si va a implementar el servicio del agente remoto de implementación Web, debe usar la autenticación NTLM. El servicio del agente remoto no aceptó las credenciales de autenticación básica.
- Si va a implementar en el controlador de implementación Web, puede utilizar NTLM o autenticación básica. El valor predeterminado es autenticación básica. Aunque la autenticación básica se basa en los nombres de usuario y contraseñas que se transmiten en texto sin formato, las credenciales están protegidas como el controlador de implementación Web siempre utiliza el cifrado SSL.
- Si el paquete web incluye una base de datos y el servidor web y el servidor de base de datos son equipos independientes, no podrá implementar la base de datos mediante la autenticación NTLM debido a la [limitación NTLM "salto doble"](https://go.microsoft.com/?linkid=9805120). Debe usar credenciales de SQL Server en la cadena de conexión de la implementación o proporcionar credenciales de autenticación básica a Web Deploy. Este problema se describe con más detalle en [implementar bases de datos de pertenencia a entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede implementar un paquete web ya sea ejecutando el *. deploy.cmd* archivo o directamente mediante MSDeploy.exe. Explica cuándo cada enfoque podría ser adecuado y describe cómo puede parametrizar y ejecutar un comando de implementación como parte de un proceso de compilación automatizada o de paso a paso más grande.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo crear y parametrizar un paquete de implementación web, consulte [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md) y [configurar parámetros para la implementación del paquete Web](configuring-parameters-for-web-package-deployment.md). Para obtener instrucciones sobre cómo compilar e implementar paquetes web desde una instancia de Team Foundation Server (TFS), consulte [configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Para obtener información sobre cómo personalizar y solucionar problemas del proceso de implementación, consulte [excluir archivos y carpetas de implementación](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-parameters-for-web-package-deployment.md)
> [Siguiente](deploying-database-projects.md)
