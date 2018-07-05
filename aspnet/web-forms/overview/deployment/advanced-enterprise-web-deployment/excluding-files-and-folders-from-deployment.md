---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Excluir archivos y carpetas de implementación | Microsoft Docs
author: jrjlee
description: Este tema describe cómo se pueden excluir archivos y carpetas de un paquete de implementación web al compilar y empaquetar un proyecto de aplicación web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c50352d423f41f84677dbf048e74088214340f3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382587"
---
<a name="excluding-files-and-folders-from-deployment"></a>Excluir archivos y carpetas de implementación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo se pueden excluir archivos y carpetas de un paquete de implementación web al compilar y empaquetar un proyecto de aplicación web.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="overview"></a>Información general

Cuando compila un proyecto de aplicación web en Visual Studio 2010, la canalización de publicación de Web (WPP) le permite ampliar este proceso de compilación al empaquetar la aplicación web compilada en un paquete de web que se puede implementar. A continuación, puede usar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para implementar este paquete web en un servidor web IIS remoto, o importar el paquete web manualmente mediante el Administrador de IIS. Este proceso de empaquetado se explica en [compilar y empaquetar proyectos de aplicación Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

¿Cómo controlar lo que se incluyen en el sitio web del paquete? La configuración del proyecto en Visual Studio a través del archivo de proyecto subyacente, proporciona un control suficiente para muchos escenarios. Sin embargo, en algunos casos es posible que desee adaptar el contenido del paquete web en entornos de destino específico. Por ejemplo, es posible que desea incluir una carpeta para los archivos de registro al implementar la aplicación en un entorno de prueba, pero excluir la carpeta al implementar la aplicación en un entorno de ensayo o producción. En este tema le mostrará cómo hacerlo.

## <a name="what-gets-included-by-default"></a>¿Qué Obtiene incluye de forma predeterminada?

Al configurar las propiedades del proyecto de aplicación web en Visual Studio, el **elementos para implementar** lista el **Empaquetar/Publicar Web** página le permite especificar qué desea incluir en la implementación web paquete. De forma predeterminada, se establece en **sólo los archivos necesarios para ejecutar esta aplicación**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Cuando se elige **sólo los archivos necesarios para ejecutar esta aplicación**, el WPP intentará determinar qué archivos se deben agregar al paquete de web. Esto incluye:

- Toda la compilación genera para el proyecto.
- Los archivos marcan con una acción de compilación **contenido**.

> [!NOTE]
> Se incluye la lógica que determina qué archivos se incluyen en este archivo:   
> *%ProgramFiles%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Excluir determinados archivos y carpetas

En algunos casos, deseará un mayor control sobre el que se implementan los archivos y carpetas. Si sabe qué archivos van a excluir delante de tiempo y la exclusión se aplica a todos los entornos de destino, simplemente puede establecer el **acción de compilación** de cada archivo **ninguno**.

**Para excluir archivos específicos de la implementación**

1. En el **el Explorador de soluciones** , haga clic en el archivo y, a continuación, haga clic en **propiedades**.
2. En el **propiedades** ventana, en el **acción de compilación** fila, seleccione **ninguno**.

Sin embargo, este enfoque no siempre resulta conveniente. Por ejemplo, desea variar qué archivos y carpetas se incluyen según el entorno de destino y desde fuera de Visual Studio. Por ejemplo, en la solución Contact Manager de ejemplo, eche un vistazo al contenido del proyecto ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- La carpeta interna contiene algunas secuencias de comandos SQL que el programador usa para crear, quitar y rellenar las bases de datos locales para fines de desarrollo. No hay nada en esta carpeta debe implementarse en un entorno de ensayo o producción.
- La carpeta Scripts contiene varios archivos de JavaScript. Muchos de estos archivos se incluyen únicamente para admitir la depuración o proporcionar IntelliSense en Visual Studio. Algunos de estos archivos no deben implementarse en entornos de ensayo o producción. Sin embargo, desea implementarlos en un entorno de prueba para desarrolladores para facilitar la solución de problemas.

Aunque podría manipular los archivos de proyecto para excluir determinados archivos y carpetas, hay una manera más fácil. El WPP incluye un mecanismo para excluir archivos y carpetas mediante la creación de listas de elementos denominadas **ExcludeFromPackageFolders** y **ExcludeFromPackageFiles**. Puede extender este mecanismo agregando sus propios elementos a estas listas. Para ello, deberá completar estos pasos generales:

1. Cree un archivo de proyecto personalizado denominado *.wpp.targets [nombre de proyecto]* en la misma carpeta que el archivo de proyecto.

    > [!NOTE]
    > El *. wpp.targets* archivo debe ir en la misma carpeta que el archivo de proyecto de aplicación web&#x2014;por ejemplo, *ContactManager.Mvc.csproj*&#x2014;en lugar de en la misma carpeta que cualquier personalizado archivos de proyecto que se usan para controlar el proceso de compilación e implementación.
2. En el *. wpp.targets* , agregue un **ItemGroup** elemento.
3. En el **ItemGroup** elemento, agregar **ExcludeFromPackageFolders** y **ExcludeFromPackageFiles** para excluir determinados archivos y carpetas según sea necesario.

Se trata de la estructura básica de este *. wpp.targets* archivo:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Tenga en cuenta que cada elemento incluye un elemento de metadatos de elemento denominado **FromTarget**. Este es un valor opcional que no afecten al proceso de compilación; sólo sirve para indicar por qué se omitieron los archivos o carpetas determinados si alguien revisa los registros de compilación.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Excluir archivos y carpetas de un paquete de Web

El procedimiento siguiente muestra cómo agregar un *. wpp.targets* archivo a un proyecto de aplicación web y cómo usar el archivo para excluir determinados archivos y carpetas desde el paquete web al compilar el proyecto.

**Para excluir archivos y carpetas de un paquete de implementación web**

1. Abra la solución en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, haga clic en el nodo del proyecto de aplicación web (por ejemplo, **ContactManager.Mvc**), seleccione **agregar**y, a continuación, haga clic en **Nuevo elemento**.
3. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **archivo XML** plantilla.
4. En el **nombre** , escriba *[nombre de proyecto] ***.wpp.targets** (por ejemplo, **ContactManager.Mvc.wpp.targets**) y, a continuación, haga clic en **agregar**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Si agrega un nuevo elemento en el nodo raíz de un proyecto, se crea el archivo en la misma carpeta que el archivo de proyecto. Puede comprobarlo abriendo la carpeta en el Explorador de Windows.
5. En el archivo, agregue un **proyecto** elemento y un **ItemGroup** elemento:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Si desea excluir carpetas del paquete web, agregue un **ExcludeFromPackageFolders** elemento a la **ItemGroup** elemento:

   1. En el **Include** atributo, proporcione una lista separada por comas de las carpetas que desea excluir.
   2. En el **FromTarget** elemento de metadatos, proporcione un valor significativo para indicar por qué las carpetas que se excluyen, como el nombre de la *. wpp.targets* archivo.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Si desea excluir archivos del paquete web, agregue un **ExcludeFromPackageFiles** elemento a la **ItemGroup** elemento:

   1. En el **Include** atributo, proporcione una lista separada por comas de los archivos que desea excluir.
   2. En el **FromTarget** elemento de metadatos, proporcione un valor significativo para indicar por qué los archivos que se excluyen, como el nombre de la *. wpp.targets* archivo.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. El *.wpp.targets [nombre de proyecto]* archivo ahora debería parecer a este:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Guarde y cierre el *.wpp.targets [nombre de proyecto]* archivo.

La próxima vez que creación y empaquetado el proyecto de aplicación web, el WPP detectará automáticamente el *. wpp.targets* archivo. Los archivos y carpetas que especificó no se incluirá en el paquete web.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo excluir archivos y carpetas específicos cuando se compila un paquete web, mediante la creación de un personalizado *. wpp.targets* archivo en la misma carpeta que el archivo de proyecto de aplicación web.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para controlar el proceso de implementación, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre el empaquetado y el proceso de implementación, consulte [compilar y empaquetar proyectos de aplicación Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurar parámetros para la implementación del paquete Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), y [ Implementar paquetes Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Anterior](deploying-membership-databases-to-enterprise-environments.md)
> [Siguiente](taking-web-applications-offline-with-web-deploy.md)
