---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Crear y ejecutar una implementación del comando archivo | Microsoft Docs
author: jrjlee
description: Este tema describe cómo crear un archivo de comandos que le permitirá ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un solo paso, re...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395378"
---
<a name="creating-and-running-a-deployment-command-file"></a>Crear y ejecutar un archivo de comandos de implementación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo crear un archivo de comandos que le permitirá ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un proceso paso a paso y repetible.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [Contact Manager](the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="process-overview"></a>Información general del proceso

En este tema, obtendrá información sobre cómo crear y ejecutar un archivo de comandos que usa estos archivos de proyecto para realizar una implementación repetible en su entorno de destino. En esencia, el archivo de comandos simplemente debe contener un comando de MSBuild que:

- Indica a MSBuild para ejecutar el entorno agnóstico *Publish.proj* archivo.
- Indica el *Publish.proj* archivo qué archivo contiene la configuración del proyecto específicas del entorno y dónde encontrarla.

## <a name="create-an-msbuild-command"></a>Crear un comando de MSBuild

Como se describe en [descripción del proceso de compilación](understanding-the-build-process.md), el archivo de proyecto específicos del entorno&#x2014;por ejemplo, *Env Dev.proj*&#x2014;está diseñado para importarse en el entorno agnóstico *Publish.proj* archivo en tiempo de compilación. Juntos, estos dos archivos proporcionan un conjunto completo de instrucciones que indican a MSBuild cómo compilar e implementar la solución.

El *Publish.proj* archivo usa un **importar** elemento que se va a importar el archivo de proyecto específicos del entorno.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Por lo tanto, si usa MSBuild.exe para compilar e implementar la solución Contact Manager, deberá:

- Ejecutar MSBuild.exe en el *Publish.proj* archivo.
- Especifica la ubicación del archivo de proyecto específicos del entorno al proporcionar un parámetro de línea de comandos denominado **TargetEnvPropsFile**.

Para ello, el comando MSBuild debe ser similar a esto:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Desde aquí, es un simple paso para mover a una implementación repetible y paso a paso. Todo lo que necesita hacer es agregar el comando de MSBuild en un archivo cmd. En la solución Contact Manager, la carpeta de publicación incluye un archivo denominado *publicar Dev.cmd* que hace exactamente esto.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> El **/fl** modificador indica a MSBuild que cree un archivo de registro denominado *msbuild.log* en el directorio de trabajo en el que se invocó MSBuild.exe.


Para implementar o volver a implementar la solución Contact Manager, todo lo que necesita hacer es ejecutar el *publicar Dev.cmd* archivo. Al ejecutar el archivo, MSBuild hará lo siguiente:

- Compilar todos los proyectos de la solución.
- Generar paquetes de web que se puede implementar para los proyectos de aplicación web.
- Generar archivos .dbschema y .deploymanifest para los proyectos de base de datos.
- Implementar los paquetes de web en el servidor web.
- Implementar la base de datos en el servidor de base de datos.

## <a name="run-the-deployment"></a>Ejecutar la implementación

Cuando haya creado un archivo de comandos para el entorno de destino, debe ser capaz de completar toda la implementación simplemente ejecutando el archivo.

**Para implementar la solución Contact Manager en el entorno de prueba**

1. En la estación de trabajo de desarrollador, abra el Explorador de Windows y, a continuación, vaya a la ubicación de la *publicar Dev.cmd* archivo.
2. Haga doble clic en el archivo para ejecutarlo.
3. Si un **abrir archivo-Advertencia de seguridad** aparece el cuadro de diálogo, haga clic en **ejecutar**.
4. Si los valores de configuración y los servidores de prueba se configuran correctamente, la ventana de símbolo del sistema mostrará un **compilación correcta** cuando MSBuild ha finalizado el procesamiento de los archivos de proyecto del mensaje.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Si se trata de la primera vez que se ha implementado la solución a este entorno, deberá agregar la cuenta de equipo de servidor web de prueba para el **db\_datawriter** y **db\_datareader**roles en el **ContactManager** base de datos. Este procedimiento se describe en [configurar un servidor de base de datos para publicación en Web implementar](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Sólo debe asignar estos permisos al crear la base de datos. De forma predeterminada, el proceso de compilación no volverá a la base de datos en cada implementación&#x2014;en su lugar, comparará la base de datos existente en el esquema más reciente y realizar solo los cambios necesarios. Como resultado, solo deberá asignar estos roles de base de datos la primera vez que implemente la solución.
6. Abra Internet Explorer y vaya a la dirección URL de la aplicación de Contact Manager (por ejemplo, `http://testweb1:85/ContactManager/`).
7. Compruebe que la aplicación funciona según lo previsto y podrá agregar contactos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusión

Creación de un archivo de comandos que contiene las instrucciones de MSBuild proporciona una manera rápida y sencilla de crear e implementar una solución de varios proyectos en un entorno de destino específico. Si tiene que implementar la solución repetidamente a varios entornos de destino, puede crear varios archivos de comandos. En cada archivo de comandos, el comando MSBuild compilará el mismo archivo de proyecto universal, pero especificará un archivo de proyecto específicos del entorno diferente. Por ejemplo, un archivo de comandos para publicar en un desarrollador o el entorno de prueba podría contener este comando de MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Un archivo de comandos para publicar en un entorno de ensayo puede contener este comando de MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


También puede personalizar el proceso de compilación para cada entorno reemplazar propiedades o estableciendo diversos otros modificadores en el comando de MSBuild. Para obtener más información, consulte [referencia de línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-projects.md)
> [Siguiente](manually-installing-web-packages.md)
