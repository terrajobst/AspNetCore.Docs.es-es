---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Implementar paquetes de Web | Documentos de Microsoft
author: jrjlee
description: Este tema describe cómo publicar paquetes de implementación web a un servidor remoto mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 5d3af0fdcc6e7ae20194ba658e0cf72ad22c1234
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-packages"></a>Implementar paquetes de Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo publicar paquetes de implementación web a un servidor remoto mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) 2.0.
> 
> Hay dos formas principales en el que puede implementar un paquete de web en un servidor remoto:
> 
> - Puede usar la utilidad de línea de comandos MSDeploy.exe directamente.
> - Puede ejecutar el *.deploy.cmd [nombre del proyecto]* archivo que genera el proceso de compilación.
> 
> El resultado final es el mismo independientemente de qué aproximación utilizar. Básicamente, todos los *. deploy.cmd* archivo es ejecutar MSDeploy.exe con algunos valores predeterminados, por lo que no tendrá que proporcionar tanta información con el fin de implementar el paquete. Esto simplifica el proceso de implementación. Por otro lado, usar MSDeploy.exe directamente ofrece mucha más flexibilidad sobre exactamente cómo se implementa el paquete.
> 
> Método utilizado dependerá de varios factores, incluyendo cuánto control necesita sobre el proceso de implementación y si está como destino el servicio del agente remoto de implementación Web o el controlador de implementación de Web. En este tema se explica cómo utilizar cada enfoque y se identifica cuando cada enfoque es adecuado.
> 
> Las tareas y los tutoriales en este tema se asume que:
> 
> - Se ha compilado y empaqueta la aplicación web, como se describe en [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).
> - Ha modificado el *SetParameters.xml* archivo para proporcionar los valores de parámetro correctos para su entorno de destino, como se describe en [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md).


Ejecuta el [*nombre del proyecto*]*. deploy.cmd* archivo es la manera más sencilla de implementar un paquete de web. En particular, mediante la *. deploy.cmd* archivo ofrece las siguientes ventajas con respecto a MSDeploy.exe directamente:

- No es necesario especificar la ubicación del paquete de implementación web&#x2014;los *. deploy.cmd* archivo ya sabe que resulta.
- No es necesario especificar la ubicación de la *SetParameters.xml* archivo&#x2014;la *. deploy.cmd* archivo ya sabe que resulta.
- No es necesario especificar origen y destino de los proveedores de MSDeploy&#x2014;la *. deploy.cmd* archivo ya sabe qué valores para su uso.
- No es necesario especificar la configuración de la operación de MSDeploy&#x2014;la *. deploy.cmd* archivo agrega automáticamente los valores requieren con frecuencia para el comando MSDeploy.exe.

Antes de usar el *. deploy.cmd* archivo para implementar un paquete web, debe asegurarse de que:

- El *. deploy.cmd* archivo, el [*nombre del proyecto*]. *SetParameters.xml* archivo y el paquete de web ([*nombre del proyecto*]. *zip*) están en la misma carpeta.
- Web Deploy (MSDeploy.exe) está instalado en el equipo que ejecuta el *. deploy.cmd* archivo.

El *. deploy.cmd* archivo es compatible con distintas opciones de línea de comandos. Al ejecutar el archivo desde un símbolo del sistema, ésta es la sintaxis básica:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Debe especificar un **/T** marca o un **/Y** marca para indicar si desea realizar una ejecución de prueba o una implementación en vivo respectivamente (no use ambos marcadores en el mismo comando). Esta tabla explica el propósito de cada una de estas marcas.

| Marcar | Descripción |
| --- | --- |
| **/T** | Llama a MSDeploy.exe con la **– whatif** marca, lo que indica una ejecución de prueba. En lugar de implementar el paquete, crea un informe de lo que sucedería si se implementó el paquete. |
| **/Y** | Llama a MSDeploy.exe sin la **– whatif** marca. Esto implementa el paquete en el equipo local o el servidor de destino especificado. |
| **/M** | Especifica el servidor de destino el nombre o dirección URL del servicio. Para obtener más información sobre los valores que puede proporcionar aquí, consulte la **consideraciones de punto de conexión** sección en este tema. Si se omite la **/M** marca, el paquete se implementará en el equipo local. |
| **/A** | Especifica el tipo de autenticación que debe usar MSDeploy.exe para realizar la implementación. Los valores posibles son **NTLM** y **básica**. Si se omite la **/A** marca, el tipo de autenticación predeterminado es **NTLM** para la implementación para el servicio del agente remoto de implementación Web y a **básica** para su implementación en la Web Deploy Controlador. |
| **/U** | Especifica el nombre de usuario. Esto se aplica solo si usa la autenticación básica. |
| **/P** | Especifica la contraseña. Esto se aplica solo si usa la autenticación básica. |
| **/L** | Indica que se debe implementar el paquete a la instancia local de IIS Express. |
| **/G** | Especifica que el paquete se implementa mediante el [configuración del proveedor de tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Si se omite la **/G** marca, el valor predeterminado es **false**. |

> [!NOTE]
> Cada vez que el proceso de compilación crea un paquete web, también crea un archivo denominado *[nombre del proyecto] ".deploy"-readme.txt* que describen estas opciones de implementación.


Además de estas marcas, puede especificar la configuración de operación Web Deploy como adicionales *. deploy.cmd* parámetros. Cualquier configuración adicional que especifique simplemente se pasa al comando MSDeploy.exe subyacente. Para obtener más información sobre estas opciones, consulte [Web Deploy operación Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Imagine que desea implementar el proyecto de aplicación web de ContactManager.Mvc en un entorno de prueba mediante la ejecución de la *. deploy.cmd* archivo. El entorno de prueba está configurado para utilizar el servicio del agente remoto de Web implementar, como se describe en [configurar un servidor Web de publicación de implementar en Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Para implementar la aplicación web, debe completar los pasos siguientes.

**Para implementar una aplicación web utilizando el. deploy.cmd archivo**

1. Compilar y empaquetar el proyecto de aplicación web, como se describe en [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).
2. Modificar el *ContactManager.Mvc.SetParameters.xml* archivo para que contenga los valores de parámetro correctos para su entorno de prueba, como se describe en [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y desplácese a la ubicación de la *ContactManager.Mvc.deploy.cmd* archivo.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

En este ejemplo:

- El **/Y** marca indica que desea implementar realmente el paquete, en lugar de realizar una prueba a ejecutar.
- El **/M** marca indica que desea implementar el paquete en el servidor denominado TESTWEB1. De este valor, se tratará MSDeploy.exe implementar el paquete con el servicio del agente remoto de Web implementar en http://TESTWEB1/MSDeployAgentService.
- El **/A** marca indica que desea utilizar la autenticación NTLM. Por lo tanto, no es necesario especificar un nombre de usuario y una contraseña.

Para ilustrar cómo usar el *. deploy.cmd* archivo simplifica el proceso de implementación, eche un vistazo en la línea de comandos MSDeploy.exe que obtiene generan y se ejecuta cuando se ejecuta *ContactManager.Mvc.deploy.cmd* uso de las opciones mostradas anteriormente.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Para obtener más información sobre el uso de la *. deploy.cmd* archivo para implementar un paquete web, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Utiliza MSDeploy.exe

Aunque el uso de la *. deploy.cmd* archivo generalmente simplifica el proceso de implementación, existen algunas situaciones cuando es preferible usar MSDeploy.exe directamente. Por ejemplo:

- Si desea implementar en el controlador de implementación Web como un usuario sin privilegios de administrador, no puede usar el *. deploy.cmd* archivo. Esto es debido a un error de Web Deploy 2.0, tal y como se describe en la sección **consideraciones de punto de conexión**.
- Si desea cambiar manualmente entre distintos *SetParameters.xml* archivos en distintas ubicaciones, quizás prefiera usar MSDeploy.exe directamente.
- Si desea invalidar varios argumentos de línea de comandos MSDeploy.exe, es preferible usar MSDeploy.exe directamente.

Cuando se usa MSDeploy.exe, debe proporcionar tres fragmentos de información clave:

- A **: origen** parámetro que indica de dónde provienen los datos.
- A **– dest** parámetro que indica que los datos se van a.
- A **: verbo** parámetro que indica la [operación](https://technet.microsoft.com/library/dd568989(WS.10).aspx) que desea realizar.

MSDeploy.exe se basa en [proveedores de Web Deploy](https://technet.microsoft.com/library/dd569040(WS.10).aspx) para procesar los datos de origen y de destino. Web Deploy incluye una gran cantidad de proveedores que representan el intervalo de las aplicaciones y orígenes de datos puede funcionar con&#x2014;por ejemplo, hay proveedores para las bases de datos de SQL Server, servidores web de IIS, certificados, ensamblados de ensamblado global (GAC) de la caché, varios archivos de configuración diferente y muchos otros tipos de datos. Tanto el **: origen** parámetro y el **– dest** parámetro debe especificar un proveedor, en el formulario **: origen**: [*providerName*] = [*ubicación*]. Si va a implementar un paquete de web a un sitio Web IIS, debe usar estos valores:

- El **: origen** proveedor es siempre [paquete](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- El **– dest** proveedor es siempre [automática](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- El **: verbo** siempre es **sincronización**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Además, debe especificar varias otras [configuraciones específicas del proveedor](https://technet.microsoft.com/library/dd569001(WS.10).aspx) y general [configuración de operación](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Por ejemplo, imagine que desea implementar la aplicación web ContactManager.Mvc en un entorno de ensayo. La implementación se aplicará el controlador de implementación Web y debe utilizar la autenticación básica. Para implementar la aplicación web, debe completar los pasos siguientes.

**Para implementar una aplicación web mediante MSDeploy.exe**

1. Compilar y empaquetar el proyecto de aplicación web, como se describe en [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).
2. Modificar el *ContactManager.Mvc.SetParameters.xml* archivo para que contenga los valores de parámetro correctos para su entorno de ensayo, como se describe en [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y vaya a la ubicación de MSDeploy.exe. Esta es normalmente en %PROGRAMFILES%\IIS\Microsoft Web implementar V2\msdeploy.exe.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR (pasar por alto los saltos de línea):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

En este ejemplo:

- El **: origen** parámetro especifica la **paquete** proveedor e indica la ubicación del paquete de web.
- El **– dest** parámetro especifica la **automática** proveedor. El **computerName** configuración proporciona la dirección URL del servicio del controlador de implementación Web en el servidor de destino. El **authtype** configuración indica que desea utilizar la autenticación básica y, por lo tanto deberá proporcionar un **nombre de usuario** y un **contraseña**. Por último, el **includeAcls = "False"** valor indica que no desea copiar las listas de control de acceso (ACL) de los archivos en la aplicación web de origen al servidor de destino.
- El **: verbo: sincronización** argumento indica que desea replicar el contenido de origen en el servidor de destino.
- El **: disableLink** argumentos indican que no desea replicar los grupos de aplicaciones, la configuración de directorio virtual o certificados de capa de Sockets seguros (SSL) en el servidor de destino. Para obtener más información, consulte [Web Deploy vínculo Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- El **: setParamFile** parámetro proporciona la ubicación de la *SetParameters.xml* archivo.
- El **– allowUntrusted** conmutador indica que Web Deploy debe aceptar certificados SSL que no hayan sido emitidos por una entidad de certificación de confianza. Si va a implementar en el controlador de implementación Web y ha usado un certificado autofirmado para proteger la dirección URL del servicio, debe incluir este modificador.

## <a name="automating-web-package-deployment"></a>Automatizar la implementación de paquete de Web

En muchos escenarios de empresas, desea implementar los paquetes de web como parte de un solo paso más grande o implementación automatizada. Independientemente de si elige implementar los paquetes de web mediante la ejecución de la *. deploy.cmd* de archivo o directamente con MSDeploy.exe, puede parametrizar los comandos y llamarlos desde un destino en un Microsoft Build Engine (MSBuild) archivo de proyecto.

En la solución de ejemplo póngase en contacto con el administrador, eche un vistazo a la **PublishWebPackages** de destino en la *Publish.proj* archivo. Este destino se ejecuta una vez para cada *. deploy.cmd* identificado por una lista de elementos con el nombre de archivo **PublishPackages**. El destino utiliza propiedades y metadatos de elemento para crear un conjunto completo de valores de argumento para cada *. deploy.cmd* archivo y, a continuación, utiliza el **Exec** tarea para ejecutar el comando.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Para obtener una descripción más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizadas en general, vea [comprender el archivo de proyecto](understanding-the-project-file.md) y [descripción del proceso de compilación](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Consideraciones de punto de conexión

Independientemente de si implementa el paquete de la web mediante la ejecución de la *. deploy.cmd* de archivo o directamente con MSDeploy.exe, debe especificar un nombre de equipo o un extremo de servicio para la implementación.

Si el servidor web de destino está configurado para la implementación con el servicio del agente remoto de implementación Web, especifique la dirección URL de servicio de destino como destino.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Como alternativa, puede especificar el nombre del servidor independiente como su destino y Web Deploy deducirá la dirección URL de servicio de agente remoto.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Si el servidor web de destino está configurado para la implementación con el controlador de implementación Web, debe especificar la dirección del extremo del servicio de administración de IIS Web (WMSvc) como destino. De forma predeterminada, se toma la forma:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Puede tener como destino cualquiera de estos puntos de conexión mediante el *. deploy.cmd* archivo o MSDeploy.exe directamente. Sin embargo, si desea implementar en el controlador de implementación Web como un usuario sin privilegios de administrador, como se describe en [configurar un servidor Web de publicación de implementar en Web (Web implementar controlador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), debe agregar una cadena de consulta a la dirección de punto de conexión de servicio.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Esto es porque el usuario sin privilegios de administrador no tiene acceso de nivel de servidor en IIS. sólo tiene acceso a un sitio Web IIS específico. En el momento de escribir este artículo, debido a un error en la canalización de publicación Web (WPP), no se puede ejecutar el *. deploy.cmd* archivo utilizando una dirección de punto de conexión que incluye una cadena de consulta. En este escenario, debe implementar el paquete web directamente mediante MSDeploy.exe.

> [!NOTE]
> Para obtener más información sobre el servicio del agente remoto de implementación Web y el controlador de implementación de Web, consulte [la elección del enfoque de derecha a la implementación de Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Para obtener instrucciones sobre cómo configurar los archivos de proyecto de específicas del entorno para implementar en estos puntos de conexión, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Consideraciones sobre la autenticación

Independientemente de si implementa el paquete de la web mediante la ejecución de la *. deploy.cmd* de archivo o directamente con MSDeploy.exe, debe especificar un tipo de autenticación. Web Deploy acepta dos valores posibles: **NTLM** o **básica**. Si se especifica la autenticación básica, debe proporcionar un nombre de usuario y una contraseña. Hay varios factores que debe tener en cuenta al seleccionar un tipo de autenticación:

- Si va a implementar con el servicio del agente remoto de Web implementar, debe usar la autenticación NTLM. El servicio del agente remoto no acepta credenciales de autenticación básica.
- Si va a implementar en el controlador de implementación Web, puede utilizar NTLM o autenticación básica. El valor predeterminado es la autenticación básica. Aunque la autenticación básica se basa en nombres de usuario y contraseñas que se transmiten en texto sin formato, las credenciales están protegidas como el controlador de implementación Web siempre utiliza el cifrado SSL.
- Si el paquete de web incluye una base de datos y el servidor web y el servidor de base de datos son equipos diferentes, no podrá implementar la base de datos mediante la autenticación NTLM debido a la [limitación NTLM "salto doble"](https://go.microsoft.com/?linkid=9805120). Debe usar credenciales de SQL Server en la cadena de conexión de implementación o proporcionar las credenciales de autenticación básica a Web Deploy. Este problema se describe con más detalle en [implementar bases de datos de pertenencia a los entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede implementar un paquete de web ya sea ejecutando el *. deploy.cmd* archivo o directamente mediante MSDeploy.exe. Explica si cada enfoque podría ser adecuado y describe cómo puede parametrizar y ejecutar un comando de implementación como parte de un proceso de compilación de paso a paso o automatizadas mayor.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo crear y parametrizar un paquete de implementación web, consulte [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md) y [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md). Para obtener instrucciones sobre cómo crear e implementar paquetes de web desde una instancia de Team Foundation Server (TFS), consulte [configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Para obtener información sobre cómo personalizar y solucionar problemas del proceso de implementación, consulte [excluir archivos y carpetas de implementación](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-parameters-for-web-package-deployment.md)
> [Siguiente](deploying-database-projects.md)
