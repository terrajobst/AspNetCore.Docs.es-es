---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Archivo de comandos de creación y ejecución de una implementación | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo crear un archivo de comandos que le permiten ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un solo paso, re...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891183"
---
<a name="creating-and-running-a-deployment-command-file"></a>Crear y ejecutar un archivo de comandos de implementación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo crear un archivo de comandos que le permiten ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un proceso paso a paso y reproducible.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el Administrador de](the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="process-overview"></a>Información general del proceso

En este tema, aprenderá a crear y ejecutar un archivo de comandos que usa estos archivos de proyecto para realizar una implementación repetible al entorno de destino. En esencia, el archivo de comandos solo es necesario para que contenga un comando de MSBuild que:

- Indica a MSBuild que ejecute el independiente del entorno *Publish.proj* archivo.
- Indica el *Publish.proj* archivo el archivo que contiene la configuración del proyecto específicas del entorno y dónde encontrarla.

## <a name="create-an-msbuild-command"></a>Crear un comando de MSBuild

Como se describe en [descripción del proceso de compilación](understanding-the-build-process.md), el archivo de proyecto específicas del entorno&#x2014;por ejemplo, *Env Dev.proj*&#x2014;está diseñado para su importación en el entorno independiente *Publish.proj* archivo en tiempo de compilación. Juntos, estos dos archivos proporcionan un conjunto completo de instrucciones que indican cómo compilar e implementar la solución de MSBuild.

El *Publish.proj* archivo usa un **importar** elemento que se va a importar el archivo de proyecto específicos del entorno.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Por lo tanto, si usa MSBuild.exe para compilar e implementar la solución póngase en contacto con el administrador, debe:

- Ejecutar MSBuild.exe en la *Publish.proj* archivo.
- Especifica la ubicación del archivo del proyecto específicas del entorno al proporcionar un parámetro de línea de comandos denominado **TargetEnvPropsFile**.

Para ello, el comando MSBuild debe ser similar a este:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Desde aquí, es un paso simple para mover a una implementación repetible, paso a paso. Todo lo que necesita hacer es agregar el comando MSBuild a un archivo .cmd. En la solución póngase en contacto con el administrador, la carpeta de publicación incluye un archivo denominado *Dev.cmd publicar* que hace exactamente esto.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> El **/fl** modificador indica a MSBuild para crear un archivo de registro denominado *msbuild.log* en el directorio de trabajo en el que se invocó MSBuild.exe.


Para implementar o volver a implementar la solución póngase en contacto con el administrador, todo lo que necesita hacer es ejecutar el *Dev.cmd publicar* archivo. Al ejecutar el archivo, MSBuild hará lo siguiente:

- Compilar todos los proyectos de la solución.
- Generar paquetes de web que se pueden implementar para los proyectos de aplicación web.
- Generar archivos .dbschema y .deploymanifest para los proyectos de base de datos.
- Implementar los paquetes de web en el servidor web.
- Implementar la base de datos en el servidor de base de datos.

## <a name="run-the-deployment"></a>Ejecutar la implementación

Cuando haya creado un archivo de comandos para el entorno de destino, debe completar toda la implementación simplemente ejecutando el archivo.

**Para implementar la solución póngase en contacto con el administrador para su entorno de prueba**

1. En la estación de trabajo de desarrollador, abra el Explorador de Windows y, a continuación, vaya a la ubicación de la *Dev.cmd publicar* archivo.
2. Haga doble clic en el archivo para ejecutarlo.
3. Si un **abrir archivo-Advertencia de seguridad** aparece el cuadro de diálogo, haga clic en **ejecutar**.
4. Si los valores de configuración y los servidores de prueba están configurados correctamente, la ventana de símbolo del sistema mostrará un **compilación correcta** mensaje al MSBuild ha terminado de procesar los archivos de proyecto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Si se trata de la primera vez que se ha implementado la solución a este entorno, debe agregar la cuenta de equipo de servidor web de prueba para la **db\_datawriter** y **db\_datareader**roles en el **ContactManager** base de datos. Este procedimiento se describe en [configurar un servidor de base de datos para publicación en Web implementar](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Basta con estos permisos se asignan al crear la base de datos. De forma predeterminada, el proceso de compilación no volverá a crear la base de datos en cada implementación&#x2014;en su lugar, comparará la base de datos existente en el esquema más reciente y realizar solo los cambios necesarios. Como resultado, sólo necesitará asignar estos roles de base de datos la primera vez que implemente la solución.
6. Abra Internet Explorer y vaya a la dirección URL de la aplicación Administrador de contacto (por ejemplo, `http://testweb1:85/ContactManager/`).
7. Compruebe que la aplicación funciona según lo previsto y puede agregar contactos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusión

Crear un archivo de comandos que contiene las instrucciones de MSBuild proporciona una manera rápida y sencilla de crear e implementar una solución de varios proyecto en un entorno de destino específico. Si tiene que implementar varias veces la solución para varios entornos de destino, puede crear varios archivos de comandos. En cada archivo de comandos, el comando MSBuild compilará el mismo archivo de proyecto universal, pero especificará un archivo de proyecto específicos de un entorno diferente. Por ejemplo, un archivo de comandos para publicar un desarrollador o el entorno de prueba podría contener este comando MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Un archivo de comandos para publicar en un entorno de ensayo podría contener este comando MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicas del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


También puede personalizar el proceso de compilación para cada entorno reemplazar propiedades o estableciendo diversos otros modificadores en el comando MSBuild. Para obtener más información, consulte [referencia de línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-projects.md)
> [Siguiente](manually-installing-web-packages.md)
