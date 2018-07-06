---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Desconectar aplicaciones Web con Web implementarán | Microsoft Docs
author: jrjlee
description: Este tema describe cómo tomar una aplicación web sin conexión para la duración de una implementación automatizada mediante el Administrador de implementaciones de Web de Internet Information Services (IIS)...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: b8dc1ff26bbbe7dee1ee90fd929b2e98de5360db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829520"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Implementarán desconectar aplicaciones Web con Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo tomar una aplicación web sin conexión para la duración de una implementación automatizada mediante la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy). Los usuarios que vaya a la aplicación web se redirigen a un *aplicación\_offline.htm* archivo hasta que se complete la implementación.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

En muchos escenarios, desea desconectar una aplicación web mientras realiza cambios en los componentes relacionados, como las bases de datos o servicios web. Normalmente, en IIS y ASP.NET, para ello, coloque un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación web o sitio Web de IIS. El *aplicación\_offline.htm* archivo es un archivo HTML estándar y normalmente contendrá un simple mensaje advierte al usuario que el sitio está disponible temporalmente debido a mantenimiento. Mientras el *aplicación\_offline.htm* el archivo existe en la carpeta raíz del sitio Web, IIS le redireccionará automáticamente las solicitudes en el archivo. Cuando haya terminado de realizar actualizaciones, quita el *aplicación\_offline.htm* reanuda el archivo y el sitio Web que atienden solicitudes como de costumbre.

Al usar Web Deploy para realizar implementaciones automatizadas o paso a paso para un entorno de destino, es posible que desea incorporar agregando y quitando la *aplicación\_offline.htm* archivo en el proceso de implementación. Para ello, necesita completar estas tareas de alto nivel:

- En el archivo de proyecto de Microsoft Build Engine (MSBuild) que usa para controlar el proceso de implementación, cree un destino de MSBuild que copia un *aplicación\_offline.htm* archivo al servidor de destino antes de las tareas de implementación comenzar.
- Agregue otro destino de MSBuild que quita el *aplicación\_offline.htm* archivo desde el servidor de destino cuando se completan todas las tareas de implementación.
- En el proyecto de aplicación web, cree un *. wpp.targets* archivo que se asegura de que un *aplicación\_offline.htm* archivo se agrega al paquete de implementación cuando se invoca Web Deploy.

En este tema le mostrará cómo realizar estos procedimientos. Las tareas y los tutoriales en este tema se suponen que ya ha creado una solución que contenga al menos un proyecto de aplicación web, y que utilice un archivo de proyecto personalizadas para controlar el proceso de implementación como se describe en [implementación Web en el Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, puede usar el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución para seguir los ejemplos en el tema de ejemplo.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Agregar una aplicación\_archivos sin conexión a un proyecto de aplicación Web

La primera tarea es preciso completar consiste en Agregar un *aplicación\_sin conexión* archivo al proyecto de aplicación web:

- Para evitar que el archivo interfiera con el proceso de desarrollo (no desea la aplicación esté permanentemente sin conexión), debe llamarlo algo distinto *aplicación\_offline.htm*. Por ejemplo, podría denominar el archivo *aplicación\_sin conexión template.htm*.
- Para evitar que el archivo que se implementa como-es, debe establecer la acción de compilación en **ninguno**.

**Para agregar una aplicación\_archivos sin conexión a un proyecto de aplicación web**

1. Abra la solución en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, haga clic en el proyecto de aplicación web, elija **agregar**y, a continuación, haga clic en **nuevo elemento**.
3. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **página HTML**.
4. En el **nombre** , escriba **aplicación\_sin conexión template.htm**y, a continuación, haga clic en **agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Agregar algún HTML simple para informar a los usuarios que la aplicación no está disponible y, a continuación, guarde el archivo. No se incluyen las etiquetas de servidor (por ejemplo, las etiquetas que tienen el prefijo "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. En el **el Explorador de soluciones** , haga clic en el archivo nuevo y, a continuación, haga clic en **propiedades**.
7. En el **propiedades** ventana, en el **acción de compilación** fila, seleccione **ninguno**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Implementación y la eliminación de una aplicación\_archivos sin conexión

El paso siguiente es modificar la lógica de implementación para copiar el archivo en el servidor de destino al principio del proceso de implementación y quitarlo al final.

> [!NOTE]
> El siguiente procedimiento se da por supuesto que está usando un archivo de proyecto de MSBuild personalizado para controlar el proceso de implementación, como se describe en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Si va a implementar directamente desde Visual Studio, deberá usar un enfoque diferente. Sayed Ibrahim Hashimi describe uno de estos métodos en [cómo tomar su aplicación sin conexión durante la publicación en Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Para implementar un *aplicación\_sin conexión* archivo a un sitio Web IIS de destino, deberá invocar MSDeploy.exe mediante el [Web Deploy **contentPath** proveedor](https://technet.microsoft.com/library/dd569034(WS.10).aspx). El **contentPath** proveedor es compatible con las rutas de acceso del directorio físico y rutas de aplicación o sitio Web IIS, lo que facilita la elección ideal para la sincronización de un archivo de una carpeta de proyecto de Visual Studio y una aplicación web IIS. Para implementar el archivo, el comando MSDeploy debería parecerse a esto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Para quitar el archivo desde el sitio de destino al final del proceso de implementación, el comando MSDeploy debería parecerse a esto:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Para automatizar estos comandos como parte de un proceso de compilación e implementación, deberá integrarlos en el archivo de proyecto de MSBuild personalizado. El procedimiento siguiente describe cómo hacerlo.

**Para implementar y eliminar una aplicación\_archivos sin conexión**

1. En Visual Studio 2010, abra el archivo de proyecto de MSBuild que controla el proceso de implementación. En el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución de ejemplo, se trata la *Publish.proj* archivo.
2. En la raíz **proyecto** elemento, cree un nuevo **PropertyGroup** elemento para almacenar las variables para el *aplicación\_sin conexión* implementación:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. El **SourceRoot** propiedad está definida en otro lugar en el *Publish.proj* archivo. Indica la ubicación de la carpeta raíz para el contenido de origen en relación con la ruta de acceso actual&#x2014;en otras palabras, con respecto a la ubicación de la *Publish.proj* archivo.
4. El **contentPath** proveedor no aceptará las rutas de acceso relativa de archivo, por lo que necesita obtener una ruta de acceso absoluta al archivo de origen antes de implementarla. Puede usar el [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) tarea para hacer esto.
5. Agregue un nuevo **destino** elemento denominado **GetAppOfflineAbsolutePath**. Dentro de este destino, utilice el **ConvertToAbsolutePath** tarea para obtener una ruta de acceso absoluta a la *aplicación\_sin conexión de la plantilla* archivo en la carpeta del proyecto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Este destino toma la ruta de acceso relativa a la *aplicación\_sin conexión plantilla* archivo en la carpeta del proyecto y lo guarda en una nueva propiedad como una ruta de acceso absoluta del archivo. El **BeforeTargets** atributo especifica que desea que este destino se ejecute antes de la **DeployAppOffline** destino, que se creará en el paso siguiente.
7. Agregar un nuevo destino denominado **DeployAppOffline**. Dentro de este destino, invocar el comando MSDeploy.exe que implementa su *aplicación\_sin conexión* archivo al servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. En este ejemplo, el **ContactManagerIisPath** propiedad está definida en otro lugar en el archivo de proyecto. Esto es simplemente una ruta de aplicación IIS, en el formulario *[nombre del sitio Web de IIS] / [nombre de la aplicación]*. Incluye una condición de destino permite a los usuarios cambiar la *aplicación\_sin conexión* o desactivar la implementación cambiando un valor de propiedad o al proporcionar un parámetro de línea de comandos.
9. Agregar un nuevo destino denominado **DeleteAppOffline**. Dentro de este destino, invocar el comando MSDeploy.exe que quita su *aplicación\_sin conexión* archivo desde el servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. La tarea final consiste en invocar estos nuevos destinos en los lugares adecuados durante la ejecución del archivo del proyecto. Puede hacerlo de varias maneras. Por ejemplo, en el *Publish.proj* archivo, el **FullPublishDependsOn** propiedad especifica una lista de destinos que se deben ejecutar en pedido cuando la **FullPublish** predeterminada se invoca al destino.
11. Modificar el archivo de proyecto de MSBuild para invocar el **DeployAppOffline** y **DeleteAppOffline** destinos en los lugares adecuados del proceso de publicación.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Al ejecutar el archivo de proyecto de MSBuild personalizado, el *aplicación\_sin conexión* archivo se implementará en el servidor inmediatamente después de una compilación correcta. A continuación, se eliminará del servidor una vez que finalizan todas las tareas de implementación.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Agregar una aplicación\_archivos sin conexión para los paquetes de implementación

Según cómo configure la implementación, cualquier contenido existente en el destino de IIS de aplicaciones web&#x2014;como el *aplicación\_offline.htm* archivo&#x2014;pueden eliminarse automáticamente cuando se implementa un sitio web paquete para el destino. Para asegurarse de que el *aplicación\_offline.htm* archivo permanece vigente durante la implementación, deberá incluir el archivo dentro del propio paquete de implementación web además de implementar el archivo directamente en el inicio de el proceso de implementación.

- Si ha seguido las tareas anteriores de este tema, habrá agregado el *aplicación\_offline.htm* archivo al proyecto de aplicación web en un nombre de archivo diferente (hemos usado *aplicación\_ sin conexión template.htm*) y habrá establecido la acción de compilación como **ninguno**. Estos cambios son necesarios para evitar que el archivo de interferir con el desarrollo y depuración. Como resultado, debe personalizar el proceso de empaquetado para asegurarse de que el *aplicación\_offline.htm* archivo está incluido en el paquete de implementación web.

La canalización de publicación de Web (WPP) usa una lista de elementos denominada **FilesForPackagingFromProject** para generar una lista de archivos que deben incluirse en el paquete de implementación web. Puede personalizar el contenido de los paquetes de web agregando sus propios elementos a esta lista. Para ello, deberá completar estos pasos generales:

1. Cree un archivo de proyecto personalizado denominado *.wpp.targets [nombre de proyecto]* en la misma carpeta que el archivo de proyecto.

    > [!NOTE]
    > El *. wpp.targets* archivo debe ir en la misma carpeta que el archivo de proyecto de aplicación web&#x2014;por ejemplo, *ContactManager.Mvc.csproj*&#x2014;en lugar de en la misma carpeta que cualquier personalizado archivos de proyecto que se usan para controlar el proceso de compilación e implementación.
2. En el *. wpp.targets* , cree un nuevo destino de MSBuild que ejecuta *antes* el **CopyAllFilesToSingleFolderForPackage** destino. Éste es el objetivo WPP que genera una lista de aspectos que se deben incluir en el paquete.
3. En el nuevo destino, cree un **ItemGroup** elemento.
4. En el **ItemGroup** elemento, agregue un **FilesForPackagingFromProject** de elemento y especifique el *aplicación\_offline.htm* archivo.

El *. wpp.targets* archivo debe ser similar a esto:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Estos son los puntos clave de interés en este ejemplo:

- El **BeforeTargets** inserciones de atributos de este destino en el WPP especificando que se debe ejecutar inmediatamente antes de la **CopyAllFilesToSingleFolderForPackage** destino.
- El **FilesForPackagingFromProject** elemento utiliza la **DestinationRelativePath** valor de metadatos para cambiar el nombre del archivo de *aplicación\_sin conexión template.htm* para *aplicación\_offline.htm* cuando se agrega a la lista.

El procedimiento siguiente muestra cómo agregar esto *. wpp.targets* archivo a un proyecto de aplicación web.

**Para agregar un. archivos de destino.WPP en un paquete de implementación web**

1. Abra la solución en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, haga clic en el nodo del proyecto de aplicación web (por ejemplo, **ContactManager.Mvc**), seleccione **agregar**y, a continuación, haga clic en **Nuevo elemento**.
3. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **archivo XML** plantilla.
4. En el **nombre** , escriba *[nombre de proyecto] ***.wpp.targets** (por ejemplo, **ContactManager.Mvc.wpp.targets**) y, a continuación, haga clic en **agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Si agrega un nuevo elemento en el nodo raíz de un proyecto, se crea el archivo en la misma carpeta que el archivo de proyecto. Puede comprobarlo abriendo la carpeta en el Explorador de Windows.
5. En el archivo, agregue el marcado de MSBuild que se ha descrito anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Guarde y cierre el *.wpp.targets [nombre de proyecto]* archivo.

La próxima vez que creación y empaquetado el proyecto de aplicación web, el WPP detectará automáticamente el *. wpp.targets* archivo. El *aplicación\_sin conexión template.htm* archivo se incluirá en el paquete de implementación web resultante como *aplicación\_offline.htm*.

> [!NOTE]
> Si se produce un error en la implementación, el *aplicación\_offline.htm* archivo seguirá estando en su lugar y la aplicación permanecerá sin conexión. Esto suele ser el comportamiento deseado. Para conectar la aplicación en línea, puede eliminar el *aplicación\_offline.htm* archivo desde el servidor web. Como alternativa, si se corrija los errores y ejecuta una implementación correcta, el *aplicación\_offline.htm* archivo se quitará.


## <a name="conclusion"></a>Conclusión

En este tema se describe cómo tomar una aplicación web sin conexión para la duración de una implementación, mediante la publicación una *aplicación\_offline.htm* de archivos al servidor de destino al principio del proceso de implementación y eliminación en el fin. También se ha descrito cómo incluir un *aplicación\_offline.htm* archivo en un paquete de implementación web.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el empaquetado y el proceso de implementación, consulte [compilar y empaquetar proyectos de aplicación Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurar parámetros para la implementación del paquete Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), y [ Implementar paquetes Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Si publica sus aplicaciones web directamente desde Visual Studio, en lugar de con el enfoque de archivos de proyecto de MSBuild personalizado se describe en estos tutoriales, necesitará utilizar un enfoque ligeramente diferente para aprovechar la aplicación sin conexión durante la publicación proceso. Para obtener más información, consulte [cómo aprovechar la aplicación web sin conexión durante la publicación](https://go.microsoft.com/?linkid=9805135) (entrada de blog).

> [!div class="step-by-step"]
> [Anterior](excluding-files-and-folders-from-deployment.md)
> [Siguiente](running-windows-powershell-scripts-from-msbuild-project-files.md)
