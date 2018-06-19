---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Instalación manual de paquetes de Web | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo importar manualmente un paquete de implementación web en Internet Information Services (IIS). La creación de tema y empaquetado Web Instaland...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890221"
---
<a name="manually-installing-web-packages"></a>Instalación manual de paquetes de Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo importar manualmente un paquete de implementación web en Internet Information Services (IIS).
> 
> El tema [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md) se describe cómo el IIS herramienta de implementación Web (Web Deploy), junto con Microsoft Build Engine (MSBuild) y la Web publicación canalización (WPP), permite empaquetar su proyectos de aplicación Web en un solo archivo zip. Este archivo, que se conoce normalmente como un paquete de implementación web (o simplemente un paquete de implementación), contiene toda la información de contenido y la configuración que necesita IIS para poder volver a crear la aplicación web en un servidor web.
> 
> Después de crear un paquete de implementación web, puede publicarlo en un servidor IIS de varias maneras. En muchos escenarios, es conveniente aprovechar las ventajas de los puntos de integración entre MSBuild, WPP y Web Deploy para crear e instalar paquetes de web de forma remota como parte de un proceso de compilación e implementación automatizado o paso a paso. Este proceso se describe en [implementar paquetes de Web](deploying-web-packages.md). Sin embargo, esto no siempre es posible. Imagine que desea implementar una aplicación web en un entorno de producción a través de Internet. Por motivos de seguridad, un entorno de producción de este tipo está en el muy menos probable que esté detrás de un firewall en una subred que es independiente del servidor de compilación, en una red perimetral (también conocida como DMZ, zona desmilitarizada y subred filtrada). En muchos casos, el entorno de producción será en un dominio independiente o en una red aislada físicamente.
> 
> En estos casos, puede ser la única opción transportar el paquete web en el servidor de destino e importarla manualmente en IIS. Aunque este enfoque impide la implementación automatizada, todavía es una técnica muy eficaz para publicar una aplicación web&#x2014;simplemente copiar un solo archivo zip en su servidor web y usar un asistente que le guiará a través del proceso de importación.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

Deberá completar estas tareas de alto nivel para importar un paquete de implementación web en IIS:

- Crear un paquete de implementación web mediante MSBuild o línea de comandos, Team Build, Visual Studio 2010.
- Copie el paquete de web en el servidor web de destino.
- Utilice al Asistente para paquetes de aplicación de importación en el Administrador de IIS para instalar el paquete de web y proporcionar los valores de variables como las cadenas de conexión y los extremos de servicio.

En este tema le mostrará cómo realizar estos procedimientos. Las tareas y los tutoriales en este tema se suponen que ya está familiarizado con los conceptos para paquetes de web, Web Deploy y el WPP. Para obtener más información, consulte [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).

> [!NOTE]
> En este tema se usa junto con mejor [configurar un servidor Web de publicación de implementar en Web (sin conexión implementación)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), que explica cómo instalar los componentes necesarios y preparar un sitio Web de IIS para la importación del paquete.


## <a name="create-a-web-deployment-package"></a>Crear un paquete de implementación Web

La primera tarea consiste en crear un paquete de implementación web para el proyecto de aplicación web que desea implementar. Puede crear paquetes de web en una variedad de formas.

**Enfoque 1: Crear un paquete como parte del proceso de compilación con Visual Studio**

Puede configurar el proyecto de aplicación web para crear un paquete de implementación web después de cada compilación a través de la **Empaquetar/Publicar Web** ficha en las páginas de propiedades de proyecto. Este proceso se describe en [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).

**Método 2: Crear un paquete como parte del proceso de compilación con MSBuild**

Si compila el proyecto de aplicación web mediante MSBuild directamente, ya sea a través de un archivo de proyecto de MSBuild personalizado o desde la línea de comandos, puede crear un paquete de implementación web como parte del proceso de compilación mediante la inclusión de la **DeployOnBuild = true** y **DeployTarget = paquete** propiedades en el comando. Este proceso se describe en [descripción del proceso de compilación](understanding-the-build-process.md).

**Método 3: Crear un paquete a petición en Visual Studio**

Puede crear un paquete de implementación web para un proyecto de aplicación web en cualquier momento en Visual Studio 2010. Para ello, en el **el Explorador de soluciones** ventana, haga clic en el proyecto de aplicación web y, a continuación, haga clic en **crear paquete de implementación**.

![](manually-installing-web-packages/_static/image1.png)

**Método 4: Crear un paquete a petición desde la línea de comandos**

Puede crear un paquete de implementación web desde la línea de comandos mediante la invocación de la **paquete** destino en el proyecto de aplicación web con MSBuild. El comando debe ser similar a este:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Lo que enfoque que usa, el resultado final es el mismo. El WPP crea un paquete de implementación web como un archivo zip, junto con varios recursos de soporte, en la carpeta de salida para el proyecto de aplicación web.

![](manually-installing-web-packages/_static/image2.png)

Cuando piensa importar manualmente el paquete web, necesita solamente el archivo zip. Copie este archivo en el servidor web de destino y puede empezar el proceso de importación.

## <a name="import-a-web-package-into-iis"></a>Importar un paquete Web en IIS

Puede usar el siguiente procedimiento para importar un paquete de implementación web desde el sistema de archivos local a un sitio Web de IIS. Antes de llevar a cabo este procedimiento, asegúrese de que dispone de:

- Copia el paquete de implementación web en el servidor web.
- Configurar un servidor web IIS para hospedar su aplicación.

Para obtener más información acerca de cómo configurar un servidor web IIS para admitir los paquetes de implementación web, consulte [configurar un servidor Web de publicación de implementar en Web (sin conexión implementación)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Para importar un paquete de implementación web mediante el Administrador de IIS**

1. En el Administrador de IIS, en la **conexiones** panel, haga clic en el sitio Web IIS, seleccione **implementar**y, a continuación, haga clic en **importar aplicación**.

    ![](manually-installing-web-packages/_static/image3.png)
2. En el Asistente para importar paquetes de aplicación, en la **seleccione el paquete** página, vaya a la ubicación de su paquete de implementación web y, a continuación, haga clic en **siguiente**.
3. En el **seleccionar el contenido del paquete** página, desactive cualquier contenido que no necesita y, a continuación, haga clic en **siguiente**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > En muchos casos, no puede importar todo el contenido que viene con un paquete de implementación web. Por ejemplo, no desea permitir que Web Deploy reemplazar la base de datos asociada.  
    > El **conceder permisos** entradas establecer permisos en el sistema de archivos de destino para asegurarse de que la identidad del grupo de aplicaciones puede tener acceso a la carpeta física que almacena el contenido del sitio Web. Además, el usuario de autenticación anónima está concedido permiso de lectura a la carpeta para permitir que la aplicación servir archivos de tipo Extensiones multipropósito de correo Internet (MIME). Si lo prefiere, puede quitar estas entradas y configurar los permisos manualmente.
4. En el **especificar información de paquete de aplicación** , proporcione la información solicitada.

    ![](manually-installing-web-packages/_static/image5.png)
5. Cuando se crea un paquete web, el WPP analiza el archivo de configuración de la aplicación y detecta las variables, como las cadenas de conexión y los extremos de servicio. En este caso:

    1. **Ruta de la aplicación** es la ruta de acceso IIS donde desea instalar la aplicación. Esta configuración es común a todos los paquetes de implementación que crea el WPP.
    2. **Dirección del extremo de servicio de ContactService** es la dirección que la aplicación debe utilizar para comunicarse con el servicio WCF implementado. Esta opción corresponde a una entrada en el *web.config* archivo.
    3. La primera **cadena de conexión** configuración es la cadena de conexión que Web Deploy debe usar para implementar la base de datos asociado a la aplicación (en este caso una pertenencia a base de datos ASP.NET). Esta opción corresponde a la configuración en el **Empaquetar/publicar SQL** pestaña en Visual Studio.
    4. El segundo **cadena de conexión** configuración es la cadena de conexión que se usará realmente la aplicación se comunique con la base de datos cuando está en funcionamiento. Esto corresponde a una entrada de cadena de conexión en el *web.config* archivo.

        > [!NOTE]
        > Para obtener más información acerca de dónde proceden estos parámetros, consulte [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md).
6. Haga clic en **Siguiente**.
7. Si esto no es la primera vez que se haya implementado la aplicación a este sitio Web, se le pedirá que especifique si desea eliminar todo el contenido existente antes de la instalación. Elija la opción adecuada para sus requisitos y, a continuación, haga clic en **siguiente**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Cuando IIS ha terminado de instalar el paquete, haga clic en **finalizar**.

    ![](manually-installing-web-packages/_static/image7.png)

En este momento, ha publicado correctamente la aplicación web en IIS.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo importar un paquete de implementación web a un sitio Web IIS mediante el Administrador de IIS. Este enfoque para la publicación de aplicaciones web es adecuada cuando las restricciones de seguridad o la infraestructura de implementación remota imposible o no deseados.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar un servidor web IIS para admitir manualmente importar un paquete web, consulte [configurar un servidor Web de publicación de implementar en Web (sin conexión implementación)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Para obtener instrucciones más general sobre la implementación de paquetes de web, consulte [Tutorial: implementar un proyecto de aplicación Web mediante un paquete de implementación Web (parte 1 de 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-and-running-a-deployment-command-file.md)
