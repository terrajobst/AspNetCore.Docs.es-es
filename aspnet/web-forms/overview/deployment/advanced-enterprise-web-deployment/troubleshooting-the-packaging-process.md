---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Solucionar problemas del proceso de empaquetado | Documentos de Microsoft
author: jrjlee
description: Este tema describe cómo puede recopilar información detallada sobre el proceso de empaquetado mediante la propiedad EnablePackageProcessLoggingAndAssert en el M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892691"
---
<a name="troubleshooting-the-packaging-process"></a>Solucionar problemas del proceso de empaquetado
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo recopilar información detallada sobre el proceso de empaquetado utilizando la **EnablePackageProcessLoggingAndAssert** propiedad en Microsoft Build Engine (MSBuild).
> 
> Al establecer el **EnablePackageProcessLoggingAndAssert** propiedad **true**, le de MSBuild:
> 
> - Agregar información adicional sobre el proceso de empaquetado en los registros de compilación.
> - Registrar errores en determinadas condiciones, por ejemplo, si se encuentran archivos duplicados en la lista de empaquetado.
> - Cree un directorio de registro en el *ProjectName*\_carpeta de paquete y usarlo para registrar información sobre los archivos que se va a empaquetar.
> 
> Si se producen errores en el proceso de empaquetado, o los paquetes de implementación web no contienen los archivos que se esperan, puede utilizar esta información para resolver problemas del proceso y el punto de anclaje donde va incorrecto todo.
> 
> > [!NOTE]
> > El **EnablePackageProcessLoggingAndAssert** propiedad solo funciona si compila el proyecto con el **depurar** configuración. La propiedad se omite en otras configuraciones.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Descripción de la propiedad EnablePackageProcessLoggingAndAssert

[Compilar y empaquetar proyectos de aplicación Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) se describe cómo la canalización de publicación de Web (WPP) proporciona un conjunto de destinos de MSBuild que amplían la funcionalidad de MSBuild y habilitarlo para integrarse con la Web de Internet Information Services (IIS) Herramienta de implementación (Web Deploy). Si empaqueta un proyecto de aplicación web, que se están invocando destinos WPP.

Muchos de estos destinos WPP incluyen lógica condicional que registra la información adicional cuando la **EnablePackageProcessLoggingAndAssert** propiedad está establecida en **true**. Por ejemplo, si examina la **paquete** destino, puede ver que crea un directorio de registro adicionales y se escribe una lista de archivos en un archivo de texto si **EnablePackageProcessLoggingAndAssert** es igual a **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Los destinos WPP se definen en el *Microsoft.Web.Publishing.targets* archivo en la carpeta de %\MSBuild\Microsoft\VisualStudio\v10.0\Web % PROGRAMFILES (x 86). Puede abrir este archivo y revisar los destinos en Visual Studio 2010 o en cualquier editor de XML. Procure no para modificar el contenido del archivo.


## <a name="enabling-the-additional-logging"></a>Habilitar el inicio de sesión adicional

Puede proporcionar un valor para el **EnablePackageProcessLoggingAndAssert** propiedad de varias maneras, dependiendo de cómo compilar el proyecto.

Si compila el proyecto desde la línea de comandos, puede proporcionar un valor para el **EnablePackageProcessLoggingAndAssert** propiedad como un argumento de línea de comandos:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Si usa un archivo de proyecto personalizado para generar los proyectos, puede incluir la **EnablePackageProcessLoggingAndAssert** valor en el **propiedades** atributo de la **MSBuild**tareas:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Si usa una definición de compilación de Team Foundation Server (TFS) para compilar los proyectos, puede proporcionar un valor para el **EnablePackageProcessLoggingAndAssert** propiedad en el **argumentos de MSBuild** fila:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Para obtener más información sobre cómo crear y configurar las definiciones de compilación, consulte [crear una compilación que admite la implementación de la definición](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Como alternativa, si desea incluir el paquete en cada compilación, puede modificar el archivo de proyecto para el proyecto de aplicación web establecer el **EnablePackageProcessLoggingAndAssert** propiedad **true**. Debe agregar la propiedad a la primera **PropertyGroup** elemento dentro del archivo .csproj o .vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Revisar los archivos de registro

Cuando se compila y empaqueta un proyecto de aplicación web con **EnablePackageProcessLoggingAndAssert** establecido en **true**, MSBuild crea una carpeta adicional con el nombre de registro en el *ProjectName* \_Carpeta del paquete. La carpeta de registro contiene varios archivos:

![](troubleshooting-the-packaging-process/_static/image2.png)

La lista de archivos que se ven varían según los elementos del proyecto y el proceso de compilación. Sin embargo, estos archivos se usan normalmente para registrar la lista de archivos que está recopilando el WPP para hacer el empaquetado en distintas fases del proceso:

- El *PreExcludePipelineCollectFilesPhaseFileList.txt* archivo incluyen los archivos que recopila de MSBuild para empaquetar antes de que se quitan los archivos que se especifican para su exclusión.
- El *AfterExcludeFilesFilesList.txt* archivo contiene la lista de archivos modificados después de quitan los archivos que se especifican para su exclusión.

    > [!NOTE]
    > Para obtener más información acerca de cómo excluir archivos y carpetas desde el proceso de empaquetado, vea [excluir archivos y carpetas de implementación](excluding-files-and-folders-from-deployment.md).
- El *AfterTransformWebConfig.txt* archivo enumera los archivos recopilados para hacer el empaquetado después de un *Web.config* transformaciones que se hayan realizado. En esta lista, cualquier específicas de la configuración *Web.config* transformar los archivos, como *Web.Debug.config* y *Web.Release.config*, se excluyen de la lista de archivos para empaquetado. Una sola transforman *Web.config* se incluye en su lugar.
- El *PostAutoParameterizationWebConfigConnectionStrings.txt* archivo contiene la lista de archivos después de las cadenas de conexión en el *Web.config* archivo con parámetros. Este es el proceso que le permite reemplazar las cadenas de conexión con la configuración correcta para su entorno de destino al implementar el paquete.
- El *Prepackage.txt* archivo contiene la lista anterior a la compilación finalizada de archivos que se van a incluirse en el paquete.

> [!NOTE]
> Los nombres de los archivos de registro adicionales normalmente corresponden a los destinos WPP. Puede revisar estos destinos mediante el examen de la *Microsoft.Web.Publishing.targets* archivo en la carpeta de %\MSBuild\Microsoft\VisualStudio\v10.0\Web % PROGRAMFILES (x 86).


Si el contenido del paquete web no es lo que esperaba, revise estos archivos puede ser una forma útil de identificar en qué punto en los aspectos del proceso produjo un error.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede utilizar el **EnablePackageProcessLoggingAndAssert** propiedad de MSBuild para solucionar problemas del proceso de empaquetado. Explican las distintas maneras en que puede proporcionar el valor de propiedad para el proceso de compilación y describe la información adicional que se registra cuando se establece la propiedad en **true**.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre la WPP y el modo en que administra el proceso de empaquetado, vea [edificio y proyectos de aplicación Web de empaquetado](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Para obtener instrucciones acerca de cómo excluir determinados archivos y carpetas de los paquetes de implementación web, consulte [excluir archivos y carpetas de implementación](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](running-windows-powershell-scripts-from-msbuild-project-files.md)
