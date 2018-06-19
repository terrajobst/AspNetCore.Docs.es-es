---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Compilar y empaquetar proyectos de aplicación Web | Documentos de Microsoft
author: jrjlee
description: Cuando desea implementar un proyecto de aplicación web en un entorno de servidor remoto, la primera tarea es compilar el proyecto y generar una implementación de web paquetes?...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892314"
---
<a name="building-and-packaging-web-application-projects"></a>Compilar y empaquetar proyectos de aplicación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cuando desea implementar un proyecto de aplicación web en un entorno de servidor remoto, la primera tarea consiste en compilar el proyecto y generar un paquete de implementación web. Este tema describe cómo funciona el proceso de compilación para los proyectos de aplicación web. En concreto, se explica:
> 
> - Cómo la canalización de publicación de Web (WPP) extiende el proceso de compilación para incluir la funcionalidad de implementación.
> - Cómo la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) activa la aplicación web en un paquete de implementación.
> - Cómo la compilación y empaquetado del proceso y qué archivos se crean.


En Visual Studio 2010, el proceso de compilación e implementación de proyectos de aplicación web es compatible con la WPP. El WPP proporciona un conjunto de destinos de Microsoft Build Engine (MSBuild) que amplían la funcionalidad de MSBuild y habilitarlo para integrar con Web Deploy. En Visual Studio, puede ver esta funcionalidad ampliada en las páginas de propiedades para el proyecto de aplicación web. El **Empaquetar/Publicar Web** página, junto con el **Empaquetar/publicar SQL** le permite configurar cómo se empaqueta el proyecto de aplicación web para la implementación cuando se completa el proceso de compilación de la página.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>¿Cómo funciona el WPP?

Si Eche un vistazo en el archivo de proyecto de C#: proyecto de aplicación basada en web, puede ver que importa archivos .targets de dos.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


La primera **importación** instrucción es común a todos los proyectos de Visual C#. Este archivo, *Microsoft.CSharp.targets*, contiene los destinos y tareas que son específicas de Visual C#. Por ejemplo, el compilador de C# (**Csc**) a continuación invoca la tarea. El *Microsoft.CSharp.targets* archivos a su vez importa el *Microsoft.Common.targets* archivo. Esto define los destinos que son comunes a todos los proyectos, como **generar**, **volver a generar**, **ejecutar**, **compilar**, y **limpiar** . El segundo **importación** instrucción es específica de proyectos de aplicación web. El *Microsoft.WebApplication.targets* archivos a su vez importa el *Microsoft.Web.Publishing.targets* archivo. El *Microsoft.Web.Publishing.targets* archivo básicamente *es* el WPP. Definen como destinos, **paquete** y **MSDeployPublish**, que invocar Web Deploy para realizar varias tareas de implementación.

Para entender cómo se utilizan estos destinos adicionales, en la solución de ejemplo póngase en contacto con el administrador, abra el *Publish.proj* de archivos y eche un vistazo a la **BuildProjects** destino.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Este destino utiliza la **MSBuild** tareas para compilar varios proyectos. Observe el **DeployOnBuild** y **DeployTarget** propiedades:

- El **DeployOnBuild = true** propiedad esencialmente significa "Quiero ejecutar un destino adicional cuando se complete correctamente la compilación".
- El **DeployTarget** propiedad identifica el nombre del destino que desea ejecutar cuando el **DeployOnBuild** propiedad es igual a **true**. En este caso, debe especificar que desea que MSBuild para ejecutar el **paquete** destino después de compilar el proyecto.

El **paquete** destino se define en el *Microsoft.Web.Publishing.targets* archivo. En esencia, este destino toma el resultado de compilación del proyecto de aplicación web y la convierte en un paquete de implementación web que se pueden publicar en un servidor web IIS.

> [!NOTE]
> Para ver un archivo de proyecto (por ejemplo, <em>ContactManager.Mvc.csproj</em>) en Visual Studio 2010, primero debe descargar el proyecto de la solución. En el <strong>el Explorador de soluciones</strong> ventana, haga clic en el nodo del proyecto y, a continuación, haga clic en <strong>descargar el proyecto</strong>. Haga clic en el nodo del proyecto nuevo y, a continuación, haga clic en <strong>editar</strong><em>[archivo de proyecto]</em>). El archivo de proyecto se abrirá en formato XML. No olvide volver a cargar el proyecto cuando haya terminado.  
> Para obtener más información sobre los destinos de MSBuild, tareas, y <strong>importación</strong> , vea [comprender el archivo de proyecto](understanding-the-project-file.md). Para obtener una introducción más detallada a los archivos de proyecto y el WPP, consulte [dentro de la Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>¿Qué es un paquete de implementación Web?

Al compilar e implementar un proyecto de aplicación web, mediante el uso de Visual Studio 2010 o directamente mediante MSBuild, el resultado final es normalmente un *paquete de implementación web*. El paquete de implementación web es un archivo zip. Contiene todo lo que IIS y Web Deploy necesario para volver a crear la aplicación web, incluidos:

- El resultado compilado de la aplicación web, incluido el contenido, los archivos de recursos, archivos de configuración, JavaScript y en cascada recursos (CSS) de hojas de estilo y así sucesivamente.
- Hace referencia a ensamblados para el proyecto de aplicación web y para las proyectos dentro de la solución.
- Secuencias de comandos SQL para generar las bases de datos que va a implementar con la aplicación web.

Una vez que se ha generado el paquete de implementación web, puede publicarlo en un servidor web IIS de varias maneras. Por ejemplo, puede implementar de forma remota estableciendo como destino el servicio del agente remoto de implementación Web o el controlador de implementación Web en el servidor web de destino, o puede usar el Administrador de IIS para importar manualmente el paquete en el servidor web de destino. Para obtener más información sobre estos enfoques de implementación, consulte [la elección del enfoque de derecha a la implementación de Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>¿Cómo funciona el proceso de compilación?

Esto muestra lo que sucede cuando se compila y empaqueta un proyecto de aplicación web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Cuando compila un proyecto de aplicación web, el proceso de compilación genera un archivo denominado *[nombre del proyecto]. SourceManifest.xml*. Junto con el archivo de proyecto y los resultados de la compilación, esto *. SourceManifest.xml* archivo indica a Web Deploy lo que necesita para incluir en el paquete de implementación web. Con estas entradas, Web Deploy genera un paquete de implementación web denominado *.zip [nombre del proyecto]*.

Junto con el paquete de implementación web, el proceso de compilación genera dos archivos que le permitirá usar el paquete:

- El *. deploy.cmd* archivo incluye un conjunto de comandos de Web Deploy (MSDeploy.exe) con parámetros que publicar el paquete de implementación web en un servidor web IIS remoto. Ejecuta el *. deploy.cmd* archivo, con los parámetros adecuados, normalmente proporciona un rápido y más fácil alternativo para construir manualmente el MSDeploy.exe comandos usted mismo.
- El *SetParameters.xml* archivo proporciona un conjunto de valores de parámetro para el comando MSDeploy.exe. Estos valores incluyen propiedades como el nombre de la aplicación web IIS a la que va a implementar el paquete, los valores de los puntos de conexión de servicio y las cadenas de conexión definidas en el *web.config* archivo y cualquier propiedad de implementación valores definidos en las páginas de propiedades del proyecto.

El *SetParameters.xml* archivo es fundamental para administrar el proceso de implementación. Este archivo se genera dinámicamente según el contenido de su proyecto de aplicación web. Por ejemplo, si agrega una cadena de conexión para su *web.config* archivo, el proceso de compilación automáticamente detectará la cadena de conexión, la implementación se parametrizan en consecuencia y crear una entrada en el  *SetParameters.xml* archivo para que pueda modificar la cadena de conexión como parte del proceso de implementación. El siguiente tema, [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md), se explica el rol de este archivo con más detalle y describe las distintas formas en que se puede modificar durante la compilación e implementación.

> [!NOTE]
> En Visual Studio 2010, el WPP no admite precompilar las páginas en una aplicación web antes de empaquetado. La próxima versión de Visual Studio y la WPP incluirá la capacidad para precompilar una aplicación web como una opción de empaquetado.


## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general sobre la compilación y el proceso de empaquetado para los proyectos de aplicación web en Visual Studio 2010. Describe cómo el WPP permite invocar comandos de Web Deploy de MSBuild y explica cómo la compilación y empaquetado del proceso.

Después de crear un paquete de implementación web, el siguiente paso es implementarla. Para obtener más información sobre esto, consulte [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md) y [implementar paquetes de Web](deploying-web-packages.md).

## <a name="further-reading"></a>Información adicional

Los temas siguientes en este tutorial, [configurar parámetros para la implementación de paquete de Web](configuring-parameters-for-web-package-deployment.md) y [implementar paquetes de Web](deploying-web-packages.md), ofrecen orientación sobre cómo usar el paquete de web que ha creado. El tutorial final de esta serie, [implementación avanzada de Web de empresa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), proporciona instrucciones sobre cómo personalizar y solucionar problemas del proceso de empaquetado.

Para obtener una introducción más detallada a los archivos de proyecto y el WPP, consulte [dentro de la Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-build-process.md)
> [Siguiente](configuring-parameters-for-web-package-deployment.md)
