---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurar un TFS el servidor de compilación para la implementación Web | Microsoft Docs
author: jrjlee
description: Este tema describe cómo preparar un servidor de compilación de Team Foundation Server (TFS) para compilar e implementar sus soluciones utilizando Team Build y la información de Internet...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7f57ad0392a068964bb910fbbaafea105fdbb3d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841346"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurar un servidor de compilación TFS para la implementación de Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo preparar un servidor de compilación de Team Foundation Server (TFS) para compilar e implementar sus soluciones utilizando la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) y Team Build.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

Para preparar un servidor de compilación para compilar e implementar sus soluciones, deberá:

- Instale y configure el servicio de compilación TFS.
- Instale Visual Studio 2010.
- Instale los productos o componentes que son necesarios para compilar la solución, como en las versiones de .NET Framework o ASP.NET MVC.
- Instalar Web Deploy 2.0 o posterior.

En este tema le mostrará cómo realizar estos procedimientos o señalen a otros recursos en caso de que existan. Las tareas y los tutoriales en este tema se suponen:

- Comenzamos con una compilación limpia del servidor ejecutando Windows Server 2008 R2 Service Pack 1.
- El servidor está unido al dominio con una dirección IP estática.
- Ha instalado la capa de aplicación de TFS en un servidor independiente, como se describe en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>¿Que lleva a cabo estos procedimientos?

En la mayoría de los casos, un administrador de TFS será responsable de configurar los servidores de compilación. En algunos casos, el equipo de desarrolladores puede tomar posesión de los servidores de compilación específica.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalar y configurar el servicio de compilación TFS

Al configurar un servidor de compilación, la primera tarea es instalar y configurar el servicio de compilación TFS. Como parte de este proceso, deberá:

- Instalar el servicio de compilación TFS y configurar una cuenta de servicio. Las tareas de compilación, incluida la implementación, se ejecutarán con la identidad de la cuenta de servicio de compilación.
- Crear un *controlador de compilación* y uno o varios *agentes de compilación*. Cada controlador de compilación administra un conjunto de agentes de compilación. Cuando se pone en cola una compilación, el controlador de compilación asigna la tarea de compilación a un agente de compilación disponibles. Cada colección de proyectos de equipo en TFS se asigna a un controlador de compilación único.
- Configure una carpeta de entrega para las salidas de compilación. Se trata de un recurso compartido de red. Cualquier compilación salidas, como paquetes de implementación web, se envían a la carpeta de entrega.

El [Administrar Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) capítulo en MSDN contiene todos los recursos que necesita para llevar a cabo estas tareas:

- Para obtener una introducción conceptual de Team Foundation Build, incluido el servicio de compilación, los controladores de compilación y agentes de compilación, véase [descripción de Team Foundation Build System](https://msdn.microsoft.com/library/dd793166.aspx).
- Para obtener información sobre cómo instalar y configurar el servicio de compilación, véase [configurar una máquina de compilación](https://msdn.microsoft.com/library/ms181712.aspx).
- Para obtener información sobre cómo crear controladores de compilación, véase [crear y trabajar con un controlador de compilación](https://msdn.microsoft.com/library/ee330987.aspx).
- Para obtener información sobre cómo crear agentes de compilación, véase [crear y trabajar con agentes de compilación](https://msdn.microsoft.com/library/bb399135.aspx).
- Para obtener información sobre cómo crear y configurar carpetas de entrega, vea [configurar carpetas de entrega](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalar los componentes y productos necesarios

Para habilitar el servidor de compilación crear soluciones de, debe instalar los productos, componentes o los ensamblados que requiere la solución. Antes de instalar los componentes de plataforma web, debe instalar Visual Studio 2010 (cualquier versión) en el servidor de compilación. Esto garantiza que los archivos de destino principales Microsoft Build Engine (MSBuild) y los archivos de destino de la canalización de publicación Web (WPP) están disponibles para el servicio de compilación. El instalador de Visual Studio también debe instalar Web Deploy, que es necesario si planea implementar paquetes web como parte del proceso de compilación.

La mejor manera de instalar los componentes de plataforma web común es usar el [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118). Esto garantiza que se va a instalar la versión más reciente de cada producto, y lo detecta e instala los requisitos previos de cada producto también automáticamente. En el caso de los [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) la solución, debe usar el instalador de plataforma Web para instalar estos productos y componentes:

- **.NET framework 4.0**. Esto es necesario para ejecutar aplicaciones creadas en esta versión de .NET Framework.
- **Herramienta de implementación 2.1 o posterior Web**. Web Deploy (y su archivo ejecutable subyacente, MSDeploy.exe) se instala en el servidor. Como parte de este proceso, se instala y se inicia el servicio del agente de implementación Web. Este servicio le permite implementar paquetes web desde un equipo remoto.
- **ASP.NET MVC 3**. Esto instala a los ensamblados que necesarios para ejecutar aplicaciones de ASP.NET MVC 3.

**Para instalar los componentes y productos necesarios**

1. Instale Visual Studio 2010. Cuando se le solicite seleccionar características para instalar, se debe incluir:

    1. Los lenguajes de programación que necesita para compilar.
    2. Visual Web Developer. Esto garantiza que los destinos WPP se agregan a su servidor de compilación.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Una vez completada la instalación de Visual Studio 2010, descargue e instale [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (si lo no ya está incluido en los medios de instalación).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 se resuelve un error que puede impedir que MSBuild de localizar el archivo ejecutable de MSDeploy.
3. Descargue e inicie la [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118).
4. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
5. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos**.
6. En el **Microsoft .NET Framework 4** fila, si ya no está instalada .NET Framework, haga clic en **agregar**.

    > [!NOTE]
    > Es posible que ya ha instalado .NET Framework 4.0 a través de Windows Update. Si ya está instalado un producto o componente, el instalador de plataforma Web indicará esto reemplazando el **agregar** botón con el texto **instalado**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. En el **ASP.NET MVC 3 (Visual Studio 2010)** la fila, haga clic en **agregar**.
8. En el panel de navegación, haga clic en **Server**.
9. En el **2.1 de herramienta de implementación Web** la fila, haga clic en **agregar**.
10. Haga clic en **Instalar**. El instalador de plataforma Web mostrará una lista de productos &#x2014; junto con las dependencias asociadas &#x2014; esté instalado y se le pedirá que acepte los términos de licencia.
11. Revise los términos de licencia y, si acepta los términos, haga clic en **acepto**.
12. Una vez completada la instalación, haga clic en **finalizar**y, a continuación, cierre el **instalador de plataforma Web 3.0** ventana.

> [!NOTE]
> Si el proceso de implementación incluye el uso de herramientas como VSDBCMD.exe o SQLCMD.exe, deberá asegurarse de que se instalan en el servidor de compilación. VSDBCMD.exe es una herramienta de Visual Studio y normalmente se agrega al servidor al instalar Team Foundation Build. SQLCMD.exe es una herramienta de SQL Server. Puede descargar una versión independiente de SQLCMD.exe desde el [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) página.


## <a name="conclusion"></a>Conclusión

En este momento, el servidor de compilación está listo para empezar a generar e implementar los proyectos de aplicación web. El tema siguiente, [crear una compilación que admite la implementación de la definición](creating-a-build-definition-that-supports-deployment.md), se describe cómo crear y configurar una definición de compilación para controlar cuándo y cómo se crea e implementa los proyectos.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones más generales sobre cómo trabajar con Team Build, consulte [Administrar Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Anterior](adding-content-to-source-control.md)
> [Siguiente](creating-a-build-definition-that-supports-deployment.md)
