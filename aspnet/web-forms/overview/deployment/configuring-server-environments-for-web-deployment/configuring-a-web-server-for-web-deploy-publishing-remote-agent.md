---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: "Configurar un servidor Web para Web de publicación (agente remoto) de implementación | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo configurar un servidor web de Internet Information Services (IIS) para admitir la publicación de web y la implementación mediante la implementación de Web de IIS..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 23195949121cd13ca4b1809b8db91a7320d1fdd2
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Configurar un servidor Web de publicación (agente remoto) de implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar un servidor web de Internet Information Services (IIS) para admitir la publicación en web y la implementación con el servicio de agente remoto de la herramienta de implementación Web de IIS (Web Deploy).
> 
> Cuando se trabaja con Web Deploy 2.0 o posterior, hay tres enfoques principales que puede usar para obtener las aplicaciones o sitios en un servidor web. Puede realizar lo siguiente:
> 
> - Use la *WebDeploy servicio del agente remoto*. Este enfoque requiere menos configuración del servidor web, pero debe proporcionar las credenciales de administrador del servidor local con el fin de implementar nada en el servidor.
> - Use la *WebDeploy controlador*. Este enfoque es mucho más compleja y requiere más esfuerzo inicial para configurar el servidor web. Sin embargo, cuando se usa este enfoque, puede configurar IIS para permitir que los usuarios sin privilegios de administrador realizar la implementación. El controlador de implementación Web solo está disponible en IIS 7 o posterior.
> - Use *implementación sin conexión*. Este enfoque requiere la menor configuración del servidor web, pero un administrador del servidor manualmente debe copiar el paquete de web en el servidor e importarlo a través del Administrador de IIS.
> 
> Para obtener más información sobre las principales características, ventajas y desventajas de estos enfoques, consulte [la elección del enfoque de derecha a la implementación de Web](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>¿Es el WebDeploy a agente remoto el enfoque correcto para usted?

Sí, si el usuario que va a implementar el contenido puede proporcionar las credenciales de administrador en el servidor de destino. Este enfoque es a menudo deseable en estos tipos de escenarios:

- Entornos de desarrollo o de prueba, donde el desarrollador tiene control total sobre el servidor web de destino y el servidor de base de datos.
- Organizaciones más pequeñas en la que un único usuario o un pequeño grupo de usuarios tiene control sobre el ciclo de vida de toda la aplicación.

En una gran cantidad de organizaciones más grandes y, particularmente para los entornos de ensayo o de producción, normalmente no es realista para proporcionar a los usuarios derechos de administrador en los servidores web. En el caso de servidores web hospedado, esto es especialmente probable que sea el caso. Además, si está planeando automatizar la implementación de un servidor de compilación, no puede usar las credenciales de administrador para el proceso de implementación. En estos casos, configuración de los servidores web para admitir la implementación mediante la [controlador de implementación Web](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) puede proporcionar una opción más satisfactoria.

## <a name="task-overview"></a>Información general sobre tareas

En este tema se describe cómo configurar un servidor web de Internet Information Services (IIS) 7.5 para aceptar e implementar paquetes de web desde un equipo remoto mediante el enfoque de implementación Web de agente remoto. Necesitará:

- Instalar IIS 7.5 y la configuración recomendada de IIS 7.
- Instalar Web Deploy 2.1 o posterior.
- Crear un sitio Web IIS para hospedar el contenido distribuido.
- Asegúrese de que se está ejecutando el servicio del agente de implementación Web.

Para hospedar la solución de ejemplo en concreto, también necesitará:

- Instale .NET Framework 4.0.
- Instalar ASP.NET MVC 3.

En este tema le mostrará cómo realizar cada uno de estos procedimientos. Las tareas y los tutoriales en este tema se suponen que está empezando con una compilación limpia servidor ejecuta Windows Server 2008 R2. Antes de continuar, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido al dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información sobre unir equipos a un dominio, consulte [unir equipos al dominio e iniciar sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información acerca de cómo configurar direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). El servicio del agente remoto es compatible con IIS 6 y versiones posteriores y no requiere que esté unido a un dominio. Sin embargo, los pasos descritos en este tutorial han desarrollado y probado en IIS 7.5 y procedimientos para otras versiones pueden variar.


## <a name="install-products-and-components"></a>Instalar productos y componentes

En esta sección le ayudará a instalar los componentes y los productos necesarios en el servidor web. Antes de empezar, es una buena práctica ejecutar Windows Update para asegurarse de que el servidor está totalmente al día.

En este caso, debe instalar estas cosas:

- **Configuración recomendada de IIS 7**. Esto permite la **servidor Web (IIS)** rol en el servidor web e instala el conjunto de módulos IIS y los componentes que necesita para hospedar una aplicación de ASP.NET.
- **.NET framework 4.0**. Esto es necesario para ejecutar aplicaciones que se han generado en esta versión de .NET Framework.
- **Herramienta de implementación 2.1 o posterior de Web**. Esto instala Web Deploy (y el archivo ejecutable subyacente, MSDeploy.exe) en el servidor. Como parte de este proceso, instala y se inicia el servicio del agente de implementación Web. Este servicio le permite implementar paquetes de web desde un equipo remoto.
- **ASP.NET MVC 3**. Esto instala a los ensamblados que necesita para ejecutar aplicaciones de MVC 3.

> [!NOTE]
> Este tutorial describe el uso del instalador de plataforma Web para instalar y configurar los componentes necesarios. Aunque no tiene que usar al instalador de plataforma Web, simplifica el proceso de instalación al detectar dependencias automáticamente y asegurarse de que siempre obtendrá las versiones más recientes del producto. Para obtener más información, consulte [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).


**Para instalar los componentes y productos necesarios**

1. Descargue e instale el [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118).
2. Cuando una vez completada la instalación, se iniciará automáticamente el instalador de plataforma Web.

    > [!NOTE]
    > Ahora puede iniciar el instalador de plataforma Web en cualquier momento desde el **iniciar** menú. Para ello, en el **iniciar** menú, haga clic en **todos los programas**y, a continuación, haga clic en **instalador de plataforma Web de Microsoft**.
3. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
4. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos**.
5. En el **Microsoft .NET Framework 4** de fila, si ya no está instalada .NET Framework, haga clic en **agregar**.

    > [!NOTE]
    > Quizás ha instalado .NET Framework 4.0 a través de Windows Update. Si ya está instalado un producto o componente, el instalador de plataforma Web lo indicará si se reemplaza el **agregar** botón con el texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. En el **ASP.NET MVC 3 (Visual Studio 2010)** la fila, haga clic en **agregar**.
7. En el panel de navegación, haga clic en **Server**.
8. En el **configuración recomendada para IIS 7** la fila, haga clic en **agregar**.
9. En el **2.1 de herramienta de implementación Web** la fila, haga clic en **agregar**.
10. Haga clic en **Instalar**. El instalador de plataforma Web mostrará una lista de productos &#x2014; junto con las dependencias asociadas &#x2014; esté instalado y se le pedirá que acepte los términos de licencia.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Revise los términos de licencia y, si acepta los términos, haga clic en **acepto**.
12. Una vez completada la instalación, haga clic en **finalizar**y, a continuación, cierre el **instalador de plataforma Web 3.0** ventana.

Si instaló .NET Framework 4.0 antes de instalar IIS, será necesario ejecutar el [herramienta de registro de IIS de ASP.NET](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) para registrar la versión más reciente de ASP.NET con IIS. Si no lo hace, encontrará que IIS pueda servir contenido estático (como archivos HTML) sin problemas, pero devolverá **404.0 de Error de HTTP: no se encontró** al intentar examinar al contenido de ASP.NET. Puede usar este procedimiento para asegurarse de que se ha registrado ASP.NET 4.0.

**Para registrar ASP.NET 4.0 con IIS**

1. Haga clic en **iniciar**y, a continuación, escriba **símbolo**.
2. En los resultados de búsqueda, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.
3. En la ventana de símbolo del sistema, navegue hasta la **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Si tiene previsto para hospedar aplicaciones de web de 64 bits en cualquier momento, se debe registrar la versión de 64 bits de ASP.NET con IIS. Para ello, en la ventana de símbolo del sistema, navegue hasta la **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Como una buena práctica, use Windows Update nuevo en este momento para descargar e instalar las actualizaciones disponibles para los nuevos productos y componentes que haya instalado.

## <a name="configure-the-iis-website"></a>Configurar el sitio Web de IIS

Antes de poder implementar contenido web en el servidor, debe crear y configurar un sitio Web IIS para hospedar el contenido. Web Deploy solo puede implementar paquetes de web en un sitio Web IIS existente; no puede crear el sitio Web para usted. En un nivel alto, debe completar estas tareas:

- Cree una carpeta en el sistema de archivos para hospedar el contenido.
- Crear un sitio Web IIS para servir el contenido y asociarla a la carpeta local.
- Conceder permisos de lectura para la identidad del grupo de aplicaciones en la carpeta local.

Aunque no hay nada que le impida de implementación de contenido del sitio Web predeterminado en IIS, no se recomienda este enfoque para algo distinto de los escenarios de prueba o demostración. Para simular un entorno de producción, debe crear un nuevo sitio Web IIS con valores que son específicos de los requisitos de la aplicación.

**Para crear y configurar un sitio Web IIS**

1. En el sistema de archivos local, cree una carpeta para almacenar su contenido (por ejemplo, **C:\DemoSite**).
2. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Internet Information Services (IIS) Manager**.
3. En el Administrador de IIS, en la **conexiones** panel, expanda el nodo del servidor (por ejemplo, **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Haga clic en el **sitios** nodo y, a continuación, haga clic en **agregar sitio Web**.
5. En el **nombre del sitio** , escriba un nombre para el sitio Web IIS (por ejemplo, **DemoSite**).
6. En el **ruta de acceso física** cuadro, escriba (o busque) la ruta de acceso a la carpeta local (por ejemplo, **C:\DemoSite**).
7. En el **puerto** , escriba el número de puerto en el que desea hospedar el sitio Web (por ejemplo, **85**).

    > [!NOTE]
    > Los números de puerto estándar son 80 para HTTP y 443 para HTTPS. Sin embargo, si hospeda este sitio Web en el puerto 80, debe detener el sitio Web predeterminado antes de que puede tener acceso a su sitio.
8. Deje el **nombre de Host** cuadro en blanco, a menos que desee configurar un registro de sistema de nombres de dominio (DNS) para el sitio Web y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > En un entorno de producción, probablemente deseará hospedar el sitio Web en el puerto 80 y configurar un encabezado de host, junto con los registros DNS coincidentes. Para obtener más información acerca de cómo configurar los encabezados de host en IIS 7, consulte [configurar un encabezado de Host para un sitio Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obtener más información sobre el rol de servidor DNS en Windows Server 2008 R2, consulte [Introducción al servidor DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) y [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. En el **acciones** panel, en **editar sitio**, haga clic en **enlaces**.
10. En el **enlaces de sitios** cuadro de diálogo, haga clic en **agregar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. En el **Agregar enlace de sitio** cuadro de diálogo, establezca la **dirección IP** y **puerto** para que coincida con la configuración del sitio existente.
12. En el **nombre de Host** , escriba el nombre del servidor web (por ejemplo, **TESTWEB1**) y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > El primer enlace de sitio le permite tener acceso al sitio localmente mediante la dirección IP y el puerto o `http://localhost:85`. El segundo enlace del sitio le permite tener acceso al sitio desde otros equipos en el dominio con el nombre del equipo (por ejemplo, http://testweb1:85).
13. En el **enlaces de sitios** cuadro de diálogo, haga clic en **cerrar**.
14. En el **conexiones** panel, haga clic en **grupos de aplicaciones**.
15. En el **grupos de aplicaciones** panel, haga clic en el nombre de su grupo de aplicaciones y, a continuación, haga clic en **configuración básica**. De forma predeterminada, el nombre de su grupo de aplicaciones coincidirá con el nombre del sitio Web (por ejemplo, **DemoSite**).
16. En el **versión de .NET Framework** lista, seleccione **.NET Framework v4.0.30319**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > La solución de ejemplo requiere .NET Framework 4.0. Esto no es un requisito de Web Deploy en general.

En orden para el sitio Web servir el contenido, la identidad del grupo de aplicaciones debe tener permisos de lectura en la carpeta local que almacena el contenido. En IIS 7.5, los grupos de aplicaciones se ejecutan con una identidad del grupo de aplicaciones único predeterminada (a diferencia de las versiones anteriores de IIS, donde grupos de aplicaciones normalmente ejecutarán con la cuenta de servicio de red). La identidad del grupo de aplicaciones no es una cuenta de usuario real y no se muestra en las listas de usuarios o grupos &#x2014; en su lugar, se crea dinámicamente cuando se inicia el grupo de aplicaciones. Cada identidad de grupo de aplicaciones se agrega a la variable local **IIS\_IUSRS** grupo de seguridad como un elemento oculto.

Para conceder permisos a una identidad del grupo de aplicación en un archivo o carpeta, tiene dos opciones:

- Asignar permisos a la identidad del grupo de aplicación directamente, con el formato **IIS AppPool\***[nombre de grupo de aplicaciones]*(por ejemplo, **IIS AppPool\DemoSite**).
- Asignar permisos para la **IIS\_IUSRS** grupo.

El enfoque más común consiste en asignar permisos a la variable local **IIS\_IUSRS** agrupar debido a este enfoque le permite cambiar los grupos de aplicaciones sin volver a configurar los permisos de sistema de archivos. El siguiente procedimiento usa este enfoque basado en grupos.

> [!NOTE]
> Para obtener más información sobre las identidades del grupo de aplicaciones en IIS 7.5, vea [identidades del grupo de aplicaciones](https://go.microsoft.com/?linkid=9805123).


**Para configurar los permisos de carpeta para un sitio Web IIS**

1. En el Explorador de Windows, vaya a la ubicación de la carpeta local.
2. Haga clic en la carpeta y, a continuación, haga clic en **propiedades**.
3. En el **seguridad** , haga clic en **editar**y, a continuación, haga clic en **agregar**.
4. Haga clic en **ubicaciones**. En el **ubicaciones** cuadro de diálogo, seleccione el servidor local y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. En el **Seleccionar usuarios o grupos** cuadro de diálogo, escriba **IIS\_IUSRS**, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar**.
6. En el **permisos para *** [nombre de la carpeta]*cuadro de diálogo, tenga en cuenta que el nuevo grupo se ha asignado la **lectura &amp; ejecutar**, **mostrar el contenido de la carpeta**, y **Lectura** permisos de forma predeterminada. Deja sin modificar y haga clic en **Aceptar**.
7. Haga clic en **Aceptar** para cerrar el *[nombre de la carpeta] *** propiedades** cuadro de diálogo.

Como una tarea final antes de intentar implementar los paquetes de web en el servidor, debe asegurarse de que se está ejecutando el servicio del agente de implementación Web. Al implementar un paquete desde un equipo remoto, el servicio del agente de implementación Web es responsable de extraer e instalar el contenido del paquete. El servicio se inicia de forma predeterminada cuando se instala la herramienta de implementación Web y se ejecuta bajo la identidad Network Service.

Puede comprobar si está ejecutando un servicio de varias maneras diferentes, con varias utilidades de línea de comandos o los cmdlets de Windows PowerShell. Este procedimiento describe un método sencillo basado en la interfaz de usuario.

**Para comprobar que se está ejecutando el servicio del agente de implementación Web**

1. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Services**.
2. Busque la **servicio Web Deployment Agent** de fila y compruebe que la **estado** está establecido en **iniciado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Si el servicio no se ha iniciado, haga clic en **iniciar**.

## <a name="configure-firewall-exceptions"></a>Configurar excepciones de Firewall

De forma predeterminada, el servicio del agente remoto escucha en el puerto TCP 80, en esta dirección URL:

http:// [*nombre del servidor*] / MSDEPLOYAGENTSERVICE

En la mayoría de los casos, no necesitará configurar las reglas de firewall adicionales para el servicio del agente remoto porque los servidores web suelen realizar escuchas para las solicitudes HTTP en el puerto 80. Si ha personalizado la instalación para que escuche en un puerto no estándar, debe configurar excepciones de firewall según sea necesario.

## <a name="conclusion"></a>Conclusión

En este momento, el servidor web está listo para aceptar e instalar paquetes de web desde un equipo remoto. Antes de intentar implementar una aplicación web en el servidor, puede que desee comprobar estos puntos clave:

- ¿Ha registrado ASP.NET 4.0 con IIS?
- ¿La identidad del grupo de aplicaciones tiene acceso de lectura a la carpeta de origen para el sitio Web?
- ¿Está ejecutando el servicio del agente de implementación Web?

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar los archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para implementar paquetes de web con el servicio del agente remoto, consulte [configurar propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md).

>[!div class="step-by-step"]
[Anterior](scenario-configuring-a-production-environment-for-web-deployment.md)
[Siguiente](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
