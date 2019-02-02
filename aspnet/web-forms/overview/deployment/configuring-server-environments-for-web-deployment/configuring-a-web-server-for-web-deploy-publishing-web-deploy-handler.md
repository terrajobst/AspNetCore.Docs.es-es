---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configurar un servidor Web para Web publicación de la implementación (controlador de implementación Web) | Microsoft Docs
author: jrjlee
description: Este tema describe cómo configurar un servidor web de Internet Information Services (IIS) para admitir la publicación en web y la implementación mediante el patrón de implementación Web de IIS...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: cf18a8860d34daa23f61e3dde13c2c79c6c0d4a5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667328"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configurar un servidor web para la publicación de la implementación web (controlador de implementación web)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo configurar un servidor web de Internet Information Services (IIS) para admitir la publicación en web y la implementación mediante el controlador de implementación Web de IIS.
> 
> Cuando se trabaja con Web Deploy 2.0 o versiones posteriores, hay tres enfoques principales que puede usar para obtener sus aplicaciones o sitios en un servidor web. Puede realizar lo siguiente:
> 
> - Use la *Web implementar el servicio del agente remoto*. Este enfoque requiere menos configuración del servidor web, pero deberá proporcionar las credenciales de administrador del servidor local con el fin de implementar cualquier cosa en el servidor.
> - Use la *controlador de implementación Web*. Este enfoque es mucho más complejo y requiere más esfuerzo inicial para configurar el servidor web. Sin embargo, cuando se usa este enfoque, puede configurar IIS para permitir que los usuarios sin privilegios de administrador realizar la implementación. El controlador de implementación Web solo está disponible en IIS 7 o posterior.
> - Use *implementación sin conexión*. Este enfoque requiere una configuración mínima del servidor web, pero un administrador del servidor manualmente debe copiar el paquete web en el servidor e importarlo a través del Administrador de IIS.
> 
> Para obtener más información sobre las características clave, ventajas y desventajas de estos enfoques, vea [elegir el enfoque de derecha a la implementación Web](choosing-the-right-approach-to-web-deployment.md).


Sí, si desea permitir que los usuarios sin privilegios de administrador implementar el contenido a determinados sitios Web IIS. Este enfoque suele ser deseable en estos tipos de escenarios:

- Entornos de ensayo o producción, donde la cuenta de la persona o servicio que desencadena la implementación remota es poco probable que tenga acceso a las credenciales de un administrador del servidor.
- Entornos hospedados, donde desea ofrecer a los usuarios remotos la capacidad para actualizar sus sitios Web sin concederles control total de los servidores web (o el acceso a sitios Web de otra persona).

En escenarios de desarrollo o prueba, o en las organizaciones más pequeñas, implementación de contenido con credenciales de administrador del servidor suele ser menor contenciosas. En estos casos, configuración de los servidores web para admitir la implementación mediante la [Web Deploy Remote Agent Service](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) ofrece un enfoque más sencillo.

## <a name="task-overview"></a>Información general de tarea

Para configurar el servidor web para aceptar e implementar paquetes de web desde un equipo remoto mediante el método de controlador de implementación Web, deberá:

- Cree o elija una cuenta de usuario de dominio (el "usuario sin privilegios de administrador") cuyas credenciales que usará para realizar implementaciones.
- Instalar IIS 7.5, incluido el servicio de administración de Web y el módulo de autenticación básica.
- Instalar Web Deploy 2.1 o posterior.
- Configurar el servicio de administración Web para permitir conexiones remotas e iniciar el servicio.
- Crear un sitio Web de IIS para hospedar el contenido distribuido.
- Conceda los permisos de usuario sin privilegios de administrador en su sitio Web en el Administrador de IIS.
- Asegúrese de que el servicio de administración Web reglas de delegación permitan que el servicio para agregar y cambiar el contenido del sitio Web mediante su cuenta de usuario sin privilegios de administrador.
- Configurar los firewalls para permitir conexiones entrantes en el puerto 8172.

Para hospedar la solución de ejemplo ContactManager en concreto, también deberá:

- Instale .NET Framework 4.0.
- Instalar ASP.NET MVC 3.

En este tema le mostrará cómo realizar cada uno de estos procedimientos. Las tareas y los tutoriales en este tema se suponen que está empezando con una compilación limpia del servidor que ejecuta Windows Server 2016. Antes de continuar, asegúrese de que:

- Windows Server 2016
- El servidor está unido al dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información sobre la unión a los equipos a un dominio, consulte [unir equipos al dominio e iniciar sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre cómo configurar direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Instalar productos y componentes

En esta sección le ayudará a instalar los componentes y los productos necesarios en el servidor web. Antes de comenzar, es una buena práctica ejecutar Windows Update para asegurarse de que el servidor está totalmente al día.

En este caso, deberá instalar estas cosas:

- **Configuración recomendada de IIS 7**. Esto permite la **servidor Web (IIS)** rol en el servidor web e instala el conjunto de módulos de IIS y los componentes que necesita para hospedar una aplicación ASP.NET.
- **IIS: Servicio de administración**. Esto instala el servicio de administración Web (WMSvc) en IIS. Este servicio permite la administración remota de sitios Web de IIS y expone el punto de conexión del controlador de implementación Web a los clientes.
- **IIS: Autenticación básica**. Esto instala el módulo de autenticación básica de IIS. Este servicio de administración Web (WMSvc) permite autenticar las credenciales que proporcione.
- **Herramienta de implementación 2.1 o posterior Web**. Web Deploy (y su archivo ejecutable subyacente, MSDeploy.exe) se instala en el servidor. Como parte de este proceso, instala al controlador de implementación Web y se integra con el servicio de administración Web.
- **.NET Framework 4.0**. Esto es necesario para ejecutar aplicaciones creadas en esta versión de .NET Framework.
- **ASP.NET MVC 3**. Esto instala a los ensamblados que necesarios para ejecutar aplicaciones de MVC 3.

> [!NOTE]
> Este tutorial describe el uso del instalador de plataforma Web para instalar y configurar los distintos componentes. Aunque no tiene que usar al instalador de plataforma Web, simplifica el proceso de instalación al detectar las dependencias automáticamente y lo que garantiza que siempre obtendrá las versiones más recientes del producto. Para obtener más información, consulte [Microsoft Web Platform Installer](https://go.microsoft.com/?linkid=9805118).


**Para instalar los componentes y productos necesarios**

1. Descargue e instale el [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118).
2. Cuando se complete la instalación, se iniciará automáticamente el instalador de plataforma Web.

    > [!NOTE]
    > Ahora puede iniciar el instalador de plataforma Web en cualquier momento desde el **iniciar** menú. Para ello, en el **iniciar** menú, haga clic en **todos los programas**y, a continuación, haga clic en **Microsoft Web Platform Installer**.
3. En la parte superior de la **instalador de plataforma Web** ventana, haga clic en **productos**.
4. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos**.
5. En el **Microsoft .NET Framework 4** fila, si ya no está instalada .NET Framework, haga clic en **agregar**.

    > [!NOTE]
    > Es posible que ya ha instalado .NET Framework 4.0 a través de Windows Update. Si ya está instalado un producto o componente, el instalador de plataforma Web indicará esto reemplazando el **agregar** botón con el texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. En el **ASP.NET MVC 3 (Visual Studio 2010)** la fila, haga clic en **agregar**.
7. En el panel de navegación, haga clic en **Server**.
8. En el **configuración recomendada de IIS 7** la fila, haga clic en **agregar**.
9. En el **2.1 de herramienta de implementación Web** la fila, haga clic en **agregar**.
10. En el **IIS: Autenticación básica** la fila, haga clic en **agregar**.
11. En el **IIS: Servicio de administración** la fila, haga clic en **agregar**.
12. Haga clic en **Instalar**. El instalador de plataforma Web mostrará una lista de productos &#x2014; junto con las dependencias asociadas &#x2014; esté instalado y se le pedirá que acepte los términos de licencia.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Revise los términos de licencia y, si acepta los términos, haga clic en **acepto**.
14. Una vez completada la instalación, haga clic en **finalizar**y, a continuación, cierre el **instalador de plataforma Web** ventana.

Si instaló .NET Framework 4.0 antes de instalar IIS, deberá ejecutar el [herramienta de registro de IIS de ASP.NET](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) para registrar la versión más reciente de ASP.NET con IIS. Si no lo hace, encontrará que IIS pueda servir contenido estático (como archivos HTML) sin problemas, pero devolverá **404.0 de Error HTTP: no se encuentra** al intentar examinar el contenido de ASP.NET. Puede usar el procedimiento siguiente para asegurarse de que se ha registrado ASP.NET 4.0.

**Para registrar ASP.NET 4.0 con IIS**

1. Haga clic en **iniciar**y, a continuación, escriba **símbolo**.
2. En los resultados de búsqueda, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.
3. En la ventana de símbolo del sistema, navegue hasta la **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Si va a hospedar aplicaciones de web de 64 bits en cualquier momento, también debe registrar la versión de 64 bits de ASP.NET con IIS. Para ello, en la ventana de símbolo del sistema, navegue hasta la **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Como una buena práctica, usar Windows Update nuevo en este momento para descargar e instalar las actualizaciones disponibles para los nuevos productos y componentes que ha instalado.

## <a name="configure-the-web-management-service"></a>Configurar el servicio de administración Web

Ahora que ha instalado todo lo que necesita, el siguiente paso es configurar el servicio de administración de Web en IIS. En un nivel alto, necesita completar estas tareas:

- Habilitar la autenticación básica en el nivel de servidor.
- Configurar el servicio de administración Web para aceptar conexiones remotas.
- Inicie el servicio de administración Web.
- Compruebe que las reglas de delegación del servicio de administración necesarias están en vigor.

**Para configurar el servicio de administración Web**

1. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Internet Information Services (IIS) Manager**.
2. En el Administrador de IIS, en el **conexiones** panel, haga clic en el nodo del servidor (por ejemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. En el panel central, en **IIS**, haga doble clic en **autenticación**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Haga clic en **autenticación básica**y, a continuación, haga clic en **habilitar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. En el **conexiones** panel, haga clic en el nodo del servidor para volver a la configuración de nivel superior.
6. En el panel central, en **administración**, haga doble clic en **servicio de administración**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. En el panel central, seleccione **habilitar conexiones remotas**.

    > [!NOTE]
    > Si ya se está ejecutando el servicio de administración Web, debe detenerlo primero.
8. En el **acciones** panel, haga clic en **iniciar** para iniciar el servicio de administración Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Si se le pedirá que guarde la configuración, haga clic en **Sí**.

    > [!NOTE]
    > También puede configurar el servicio se inicie automáticamente. Para ello, abra la consola Servicios, haga clic en **servicio de administración Web**y, a continuación, haga clic en **propiedades**. En el **tipo de inicio** lista desplegable, seleccione **automática**y, a continuación, haga clic en **Aceptar**.
10. En el **conexiones** panel, haga clic en el nodo del servidor para volver a la configuración de nivel superior.
11. En el panel central, en **administración**, haga doble clic en **delegación de administración de servicio**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Compruebe que el panel central contiene un conjunto de reglas.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Estas reglas permiten a los usuarios autorizados de servicio de administración Web utilizar distintos proveedores de Web Deploy. Por ejemplo, para implementar contenido y aplicaciones web en IIS mediante el controlador de implementación Web, debe haber una regla de delegación que permite que todos los usuarios del servicio de administración Web para usar autenticados el **contentPath** y **iisApp**  proveedores (la última regla que se puede ver en la captura de pantalla).

    Si instala productos y componentes en el orden descrito en este tema, la versión más reciente de Web Deploy debe agregar automáticamente todas las reglas de delegación requerida para el servicio de administración Web. Si la página de delegación del servicio de administración no aparece ninguna regla, deberá crearlas usted mismo. Para obtener instrucciones sobre cómo hacerlo, consulte [configurar el controlador de implementación Web](https://go.microsoft.com/?linkid=9805124).
13. En el **conexiones** panel, haga clic en el nodo del servidor para volver a la configuración de nivel superior.

## <a name="create-and-configure-an-iis-website"></a>Crear y configurar un sitio Web de IIS

Antes de poder implementar el contenido web a su servidor, deberá crear y configurar un sitio Web de IIS para hospedar el contenido. Web Deploy solo puede implementar paquetes web en un sitio Web IIS existente; no puede crear el sitio Web por usted. También debe hacer una pequeña configuración adicional para permitir que su cuenta sin privilegios de administrador implementar el contenido de forma remota. En un nivel alto, necesita completar estas tareas:

- Cree una carpeta en el sistema de archivos para hospedar el contenido.
- Crear un sitio Web de IIS para servir el contenido y asociarla a la carpeta local.
- Conceder permisos de lectura para la identidad del grupo de aplicaciones en la carpeta local.
- Conceda los permisos de IIS necesarios para la cuenta de dominio que va a implementar la aplicación web.

Aunque no hay nada que le impida implementar contenido en el sitio Web predeterminado en IIS, no se recomienda este enfoque para algo distinto de los escenarios de prueba o demostración. Para simular un entorno de producción, debe crear un nuevo sitio Web IIS con una configuración que es específicas de los requisitos de la aplicación.

**Para crear un sitio Web de IIS**

1. En el sistema de archivos local, cree una carpeta para almacenar el contenido (por ejemplo, **C:\DemoSite**).
2. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Internet Information Services (IIS) Manager**.
3. En el Administrador de IIS, en el **conexiones** panel, expanda el nodo del servidor (por ejemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Haga clic en el **sitios** nodo y, a continuación, haga clic en **agregar sitio Web**.
5. En el **nombre del sitio** , escriba un nombre para el sitio Web de IIS (por ejemplo, **DemoSite**).
6. En el **ruta de acceso física** cuadro, escriba (o vaya a) la ruta de acceso a la carpeta local (por ejemplo, **C:\DemoSite**).
7. En el **puerto** , escriba el número de puerto en el que desea hospedar el sitio Web (por ejemplo, **85**).

    > [!NOTE]
    > Los números de puerto estándar son 80 para HTTP y 443 para HTTPS. Sin embargo, si hospeda este sitio Web en el puerto 80, deberá detener el sitio Web predeterminado antes de que puede tener acceso a su sitio.
8. Deje el **nombre de Host** cuadro en blanco, a menos que desee configurar un registro del sistema de nombres de dominio (DNS) para el sitio Web y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > En un entorno de producción, es probable que quiera hospedar su sitio Web en el puerto 80 y configurar un encabezado de host, junto con los registros DNS coincidentes. Para obtener más información sobre cómo configurar los encabezados de host en IIS 7, consulte [configurar un encabezado de Host para un sitio Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obtener más información sobre el rol de servidor DNS en Windows Server, vea [Introducción al servidor DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) y [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. En el **acciones** panel, en **editar sitio**, haga clic en **enlaces**.
10. En el **enlaces de sitios** cuadro de diálogo, haga clic en **agregar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. En el **Agregar enlace de sitio** cuadro de diálogo, establezca el **dirección IP** y **puerto** para que coincida con la configuración de sitio existente.
12. En el **nombre de Host** , escriba el nombre del servidor web (por ejemplo, **STAGEWEB1**) y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > El primer enlace del sitio le permite tener acceso al sitio localmente mediante la dirección IP y puerto o `http://localhost:85`. El segundo enlace del sitio le permite tener acceso al sitio desde otros equipos en el dominio con el nombre del equipo (por ejemplo, http://stageweb1:85).
13. En el **enlaces de sitios** cuadro de diálogo, haga clic en **cerrar**.
14. En el **conexiones** panel, haga clic en **grupos de aplicaciones**.
15. En el **grupos de aplicaciones** panel, haga clic en el nombre de su grupo de aplicaciones y, a continuación, haga clic en **configuración básica**. De forma predeterminada, el nombre de su grupo de aplicaciones coincidirá con el nombre de su sitio Web (por ejemplo, **DemoSite**).
16. En el **versión de .NET CLR** lista, seleccione **.NET CLR v4.0.30319**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

    > [!NOTE]
    > La solución de ejemplo requiere .NET Framework 4.0. Esto no es un requisito para Web Deploy en general.

En el orden de su sitio Web servir el contenido, la identidad del grupo de aplicaciones debe tener permisos de lectura en la carpeta local que almacena el contenido. En IIS 7.5, grupos de aplicaciones se ejecutan con una identidad del grupo de aplicación único de forma predeterminada (a diferencia de las versiones anteriores de IIS, donde los grupos de aplicaciones normalmente se ejecutaría con la cuenta de servicio de red). La identidad del grupo de aplicaciones no es una cuenta de usuario real y no se muestra en las listas de usuarios o grupos &#x2014; en su lugar, se crea dinámicamente cuando se inicia el grupo de aplicaciones. Se agrega cada identidad de grupo de aplicaciones local **IIS\_IUSRS** grupo de seguridad como un elemento oculto.

Para conceder permisos a una identidad de grupo de aplicaciones en un archivo o carpeta, que tiene dos opciones:

- Asignar permisos a la identidad del grupo de aplicación directamente, con el formato <strong>IIS AppPool\</strong ><em>[nombre de grupo de aplicaciones]</em>(por ejemplo, <strong>IIS AppPool\DemoSite</strong>).
- Asignar permisos a la **IIS\_IUSRS** grupo.

Es el enfoque más común asignar permisos a la variable local **IIS\_IUSRS** agrupar, dado que este enfoque le permite cambiar los grupos de aplicaciones sin volver a configurar los permisos de sistema de archivos. El siguiente procedimiento usa este enfoque basado en grupo.

> [!NOTE]
> Para obtener más información sobre las identidades del grupo de aplicaciones en IIS 7.5, vea [identidades del grupo de aplicación](https://go.microsoft.com/?linkid=9805123).


**Para configurar los permisos de carpeta para un sitio Web IIS**

1. En el Explorador de Windows, vaya a la ubicación de la carpeta local.
2. Haga clic en la carpeta y, a continuación, haga clic en **propiedades**.
3. En el **seguridad** , haga clic **editar**y, a continuación, haga clic en **agregar**.
4. Haga clic en **ubicaciones**. En el **ubicaciones** cuadro de diálogo, seleccione el servidor local y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. En el **Seleccionar usuarios o grupos** cuadro de diálogo, escriba **IIS\_IUSRS**, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar**.
6. En el <strong>permisos para</strong><em>[nombre de la carpeta]</em> cuadro de diálogo, tenga en cuenta que el nuevo grupo se ha asignado la <strong>lectura &amp; ejecutar</strong>, <strong>enumerar carpeta contenido</strong>, y <strong>lectura</strong> permisos de forma predeterminada. Déjelo sin cambios y haga clic en <strong>Aceptar</strong>.
7. Haga clic en <strong>Aceptar</strong> para cerrar el <em>[nombre de la carpeta]</em><strong>propiedades</strong> cuadro de diálogo.

Como una tarea final, debe conceder los permisos adecuados al usuario cuyas credenciales que usará para implementar el contenido que no sea de administrador. Este usuario requiere los permisos para implementar el contenido de forma remota a su sitio Web.

**Para configurar permisos del sitio Web de IIS para un usuario de dominio que no sea de administrador**

1. En el Administrador de IIS, en el **conexiones** panel, haga clic en el nodo de sitio Web (por ejemplo, **DemoSite**), seleccione **implementar**y, a continuación, haga clic en **configurar Web Publicación de la implementación**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. En el **configurar implementación de publicación en Web** cuadro de diálogo, a la derecha de la **seleccione un usuario para conceder permisos de publicación** lista, haga clic en el botón de puntos suspensivos.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. En el **permitir usuario** diálogo cuadro, escriba el nombre de usuario y dominio de la cuenta que desea usar para implementar contenido y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. En el **configurar implementación de publicación en Web** cuadro de diálogo, haga clic en **instalación**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Esta operación realiza dos funciones claves en un solo paso. En primer lugar, concede al usuario permiso para modificar el sitio Web de forma remota a través del servicio de administración Web, según las reglas de delegación que ha examinado en la sección anterior. En segundo lugar, conceda al usuario control total de la carpeta de origen para el sitio Web, que permite al usuario agregar, modificar y establecer permisos en el contenido del sitio Web.
5. En el **configurar implementación de publicación en Web** cuadro de diálogo, haga clic en **cerrar**.

## <a name="configure-firewall-exceptions"></a>Configurar excepciones de Firewall

De forma predeterminada, el servicio de administración Web de IIS escucha en el puerto TCP 8172. Si Firewall de Windows está habilitado en el servidor web, deberá crear una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172 (se permite todo el tráfico saliente en el Firewall de Windows de forma predeterminada). Si usa un firewall de terceros, deberá crear reglas para permitir el tráfico.

| Dirección | Desde el puerto | Al puerto | Tipo de puerto |
| --- | --- | --- | --- |
| Entrada | Cualquiera | 8172 | TCP |
| Saliente | 8172 | Cualquiera | TCP |
  

Para obtener más información sobre cómo configurar las reglas de Firewall de Windows, consulte [configurar reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Para los firewalls de terceros, consulte la documentación del producto.

## <a name="conclusion"></a>Conclusión

El servidor web ahora estará listo para aceptar implementaciones remotas al controlador de implementación Web a través del servicio de administración Web. Antes de intentar implementar una aplicación web en el servidor, es posible que desee comprobar estos puntos clave:

- ¿Ha habilitado la autenticación básica en el nivel de servidor en IIS?
- ¿Ha habilitado las conexiones remotas al servicio Web de administración?
- ¿Ha iniciado el servicio de administración Web?
- ¿Hay administración de reglas de delegación del servicio en su lugar?
- ¿La identidad del grupo de aplicaciones tiene acceso de lectura a la carpeta de origen para el sitio Web?
- ¿La cuenta de usuario sin privilegios de administrador tiene los permisos de nivel de sitio en IIS?
- ¿El firewall permite las conexiones entrantes en el servidor en el puerto TCP 8172?

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar los archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para implementar paquetes de web en el controlador de implementación Web, consulte [configurar propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Siguiente](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
