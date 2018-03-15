---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: "Implementarán aplicaciones sin conexión de toma de Web con Web | Documentos de Microsoft"
author: jrjlee
description: "Este tema describe cómo hacer que una aplicación web sin conexión durante el tiempo que dure una implementación automatizada mediante el Administrador de implementaciones de Web de Internet Information Services (IIS)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1c262ec7b834107524a18c6552b171f731452c91
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Implementarán aplicaciones sin conexión de toma de Web con Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo hacer que una aplicación web sin conexión durante el tiempo que dure una implementación automatizada con la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy). Los usuarios que vaya a la aplicación web se redirigen a un *aplicación\_offline.htm* archivo hasta que se complete la implementación.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En muchos escenarios, es conveniente desconectar una aplicación web mientras realiza cambios en los componentes relacionados, como las bases de datos o servicios web. Por lo general, en IIS y ASP.NET, para ello, colocando un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación web o sitio Web de IIS. El *aplicación\_offline.htm* archivo es un archivo HTML estándar y normalmente contiene un simple mensaje advierte al usuario que el sitio no está disponible temporalmente debido a mantenimiento. Mientras el *aplicación\_offline.htm* el archivo existe en la carpeta raíz del sitio Web, IIS le redirigirá automáticamente las solicitudes para el archivo. Cuando haya terminado de realizar actualizaciones, quite el *aplicación\_offline.htm* archivo y el sitio Web se reanuda servir solicitudes como de costumbre.

Al utilizar Web Deploy para realizar implementaciones automatizadas o paso a paso para un entorno de destino, puede que desee incorporar agregando y quitando el *aplicación\_offline.htm* archivo en su proceso de implementación. Para ello, debe completar estas tareas de alto nivel:

- En el archivo de proyecto de Microsoft Build Engine (MSBuild) que usa para controlar el proceso de implementación, cree un destino de MSBuild que copia un *aplicación\_offline.htm* archivo al servidor de destino antes de que las tareas de implementación comenzar.
- Agregar otro destino de MSBuild que quita el *aplicación\_offline.htm* archivo desde el servidor de destino cuando complete todas las tareas de implementación.
- En el proyecto de aplicación web, cree una *. wpp.targets* archivo que garantiza que un *aplicación\_offline.htm* archivo se agrega al paquete de implementación cuando se llama a Web Deploy.

En este tema le mostrará cómo realizar estos procedimientos. Las tareas y los tutoriales en este tema se suponen que ya ha creado una solución que contenga al menos un proyecto de aplicación web, y que usa un archivo de proyecto personalizado para controlar el proceso de implementación, como se describe en [implementación Web en el Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, puede usar el [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución para seguir los ejemplos en el tema de ejemplo.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Adición de una aplicación\_archivos sin conexión a un proyecto de aplicación Web

La primera tarea que necesite completar consiste en Agregar un *aplicación\_sin conexión* archivo al proyecto de aplicación web:

- Para evitar que el archivo interfiera con el proceso de desarrollo (no quiere que la aplicación sea permanentemente sin conexión), debe llamarlo algo distinto de *aplicación\_offline.htm*. Por ejemplo, podría denominar el archivo *aplicación\_sin conexión template.htm*.
- Para impedir que el archivo se ha implementado como-es, debe establecer la acción de compilación en **ninguno**.

**Para agregar una aplicación\_archivos sin conexión a un proyecto de aplicación web**

1. Abra la solución en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, haga clic en el proyecto de aplicación web, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.
3. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **página HTML**.
4. En el **nombre** , escriba **aplicación\_sin conexión template.htm**y, a continuación, haga clic en **agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Agregar algún HTML simple para informar a los usuarios que la aplicación no está disponible y, a continuación, guarde el archivo. No incluya etiquetas de servidor (por ejemplo, las etiquetas que tienen el prefijo "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. En el **el Explorador de soluciones** ventana, haga clic en el nuevo archivo y, a continuación, haga clic en **propiedades**.
7. En el **propiedades** ventana, en la **acción de compilación** fila, seleccione **ninguno**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Implementación y eliminación de una aplicación\_archivos sin conexión

El paso siguiente es modificar la lógica de implementación para copiar el archivo en el servidor de destino al principio del proceso de implementación y quitar al final.

> [!NOTE]
> El siguiente procedimiento se asume que está usando un archivo de proyecto de MSBuild personalizado para controlar el proceso de implementación, como se describe en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Si va a implementar directamente desde Visual Studio, debe usar un enfoque diferente. Sayed Ibrahim Hashimi describe uno de estos métodos en [cómo tomar la aplicación sin conexión durante la publicación en Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Para implementar un *aplicación\_sin conexión* archivo a un sitio Web IIS de destino, debe invocar MSDeploy.exe utilizando la [Web Deploy **contentPath** proveedor](https://technet.microsoft.com/library/dd569034(WS.10).aspx). El **contentPath** proveedor admite las rutas de acceso del directorio físico y rutas de sitio Web o aplicación de IIS, lo que facilita la elección ideal para la sincronización de un archivo entre una carpeta de proyecto de Visual Studio y la aplicación web de IIS. Para implementar el archivo, el comando MSDeploy debe ser similar a este:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Para quitar el archivo desde el sitio de destino al final del proceso de implementación, el comando MSDeploy debe ser similar a este:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Para automatizar estos comandos como parte de un proceso de compilación e implementación, debe integrarlos en el archivo de proyecto de MSBuild personalizado. El procedimiento siguiente describe cómo hacerlo.

**Para implementar y eliminar una aplicación\_archivos sin conexión**

1. En Visual Studio 2010, abra el archivo de proyecto de MSBuild que controla el proceso de implementación. En el [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución de ejemplo, se trata de la *Publish.proj* archivo.
2. En la raíz **proyecto** elemento, cree un nuevo **PropertyGroup** elemento que se va a almacenar las variables para el *aplicación\_sin conexión* implementación:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. El **SourceRoot** propiedad se define en otra parte en el *Publish.proj* archivo. Indica la ubicación de la carpeta raíz para el contenido de origen en relación con la ruta de acceso actual & #x 2014; es decir, relativa a la ubicación de la *Publish.proj* archivo.
4. El **contentPath** proveedor no aceptará las rutas de acceso de archivo relativa, por lo que necesita obtener una ruta de acceso absoluta al archivo de origen para que puede implementar. Puede usar el [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) tarea para hacer esto.
5. Agregue un nuevo **destino** elemento denominado **GetAppOfflineAbsolutePath**. En este destino, use la **ConvertToAbsolutePath** tarea para obtener una ruta de acceso absoluta a la *aplicación\_plantillas sin conexión* archivo en la carpeta del proyecto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Este destino toma la ruta de acceso relativa a la *aplicación\_sin conexión plantilla* archivo en la carpeta del proyecto y lo guarda en una nueva propiedad como una ruta de acceso absoluta del archivo. El **BeforeTargets** atributo especifica que desea que este destino para ejecutar antes de la **DeployAppOffline** destino, que creará en el paso siguiente.
7. Agregar un nuevo destino denominado **DeployAppOffline**. En este destino, invocar el comando MSDeploy.exe que implementa el *aplicación\_sin conexión* archivos al servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. En este ejemplo, el **ContactManagerIisPath** propiedad se define en otra parte en el archivo de proyecto. Esto es simplemente una ruta de aplicación IIS, en el formulario *[nombre del sitio Web de IIS] / [nombre de aplicación]*. Incluye una condición en el destino permite a los usuarios cambiar el *aplicación\_sin conexión* implementación o desactivar si cambia un valor de propiedad o proporcionar un parámetro de línea de comandos.
9. Agregar un nuevo destino denominado **DeleteAppOffline**. En este destino, invocar el comando MSDeploy.exe que quita la *aplicación\_sin conexión* archivo desde el servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. La tarea final consiste en invocar estos nuevos destinos en los lugares adecuados durante la ejecución del archivo del proyecto. Puede hacerlo de varias maneras. Por ejemplo, en la *Publish.proj* archivo, el **FullPublishDependsOn** propiedad especifica una lista de destinos que se deben ejecutar en orden cuando la **FullPublish** predeterminado se invoca al destino.
11. Modificar el archivo de proyecto de MSBuild para invocar la **DeployAppOffline** y **DeleteAppOffline** destinos en los lugares adecuados en el proceso de publicación.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Al ejecutar el archivo de proyecto de MSBuild personalizado, el *aplicación\_sin conexión* archivo se implementará en el servidor inmediatamente después de una compilación correcta. A continuación, se eliminarán del servidor después de completar todas las tareas de implementación.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Adición de una aplicación\_archivos sin conexión a los paquetes de implementación

Dependiendo de cómo configurar la implementación, cualquier contenido existente en la aplicación web IIS de destino & #x 2014; como el *aplicación\_offline.htm* archivo & #x 2014; podrían eliminarse automáticamente cuando se implementa un sitio web paquete para el destino. Para asegurarse de que el *aplicación\_offline.htm* archivo permanece en su lugar para la duración de la implementación, debe incluir el archivo dentro del propio paquete de implementación web además implementar el archivo directamente en el inicio de el proceso de implementación.

- Si ha seguido las tareas anteriores de este tema, se habrán agregado el *aplicación\_offline.htm* archivo al proyecto de aplicación web en un nombre de archivo diferente (utilizáramos *aplicación\_ sin conexión template.htm*) y habrá establece la acción de compilación en **ninguno**. Estos cambios son necesarios para evitar que el archivo desde interfiera con el desarrollo y depuración. Como resultado, debe personalizar el proceso de empaquetado para asegurarse de que el *aplicación\_offline.htm* archivo se incluye en el paquete de implementación web.

La canalización de publicación de Web (WPP) utiliza una lista de elementos con el nombre **FilesForPackagingFromProject** para generar una lista de archivos que deben incluirse en el paquete de implementación web. Puede personalizar el contenido de los paquetes de web agregando sus propios elementos a esta lista. Para ello, debe completar estos pasos de alto nivel:

1. Cree un archivo de proyecto personalizado denominado *.wpp.targets [nombre del proyecto]* en la misma carpeta que el archivo de proyecto.

    > [!NOTE]
    > El *. wpp.targets* archivo debe estar en la misma carpeta que el archivo de proyecto de aplicación web & #x 2014; por ejemplo, *ContactManager.Mvc.csproj*& #x 2014; en lugar de en la misma carpeta cualquiera archivos de proyecto personalizadas que se usan al control de la compilación y proceso de implementación.
2. En el *. wpp.targets* , cree un nuevo destino de MSBuild que ejecuta *antes de* el **CopyAllFilesToSingleFolderForPackage** destino. Este es el destino WPP que genera una lista de aspectos que se deben incluir en el paquete.
3. En el nuevo destino, cree un **ItemGroup** elemento.
4. En el **ItemGroup** elemento, agregue un **FilesForPackagingFromProject** de elemento y especifique el *aplicación\_offline.htm* archivo.

El *. wpp.targets* archivo debe ser similar a este:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Estos son los puntos clave de interés en este ejemplo:

- El **BeforeTargets** atributo inserta este destino en el WPP especificando que se debe ejecutar inmediatamente antes de la **CopyAllFilesToSingleFolderForPackage** destino.
- El **FilesForPackagingFromProject** elemento utiliza el **DestinationRelativePath** valor de metadatos para cambiar el nombre del archivo de *aplicación\_sin conexión template.htm* para *aplicación\_offline.htm* ya que se agrega a la lista.

El procedimiento siguiente muestra cómo agregar esto *. wpp.targets* archivo a un proyecto de aplicación web.

**Para agregar un. archivo wpp.targets a un paquete de implementación web**

1. Abra la solución en Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, haga clic en el nodo del proyecto de aplicación web (por ejemplo, **ContactManager.Mvc**), seleccione **agregar**y, a continuación, haga clic en **Nuevo elemento**.
3. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la **archivo XML** plantilla.
4. En el **nombre** , escriba *[nombre del proyecto] ***.wpp.targets** (por ejemplo, **ContactManager.Mvc.wpp.targets**) y, a continuación, haga clic en **agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Si agrega un nuevo elemento al nodo raíz de un proyecto, el archivo se crea en la misma carpeta que el archivo de proyecto. Para comprobarlo, abra la carpeta en el Explorador de Windows.
5. En el archivo, agregue el marcado de MSBuild que se ha descrito anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Guarde y cierre el *.wpp.targets [nombre del proyecto]* archivo.

La próxima vez que compilación y el paquete del proyecto de aplicación web, el WPP detectará automáticamente la *. wpp.targets* archivo. El *aplicación\_sin conexión template.htm* archivo se incluirán en el paquete de implementación web resultante como *aplicación\_offline.htm*.

> [!NOTE]
> Si se produce un error en la implementación, el *aplicación\_offline.htm* archivo permanecerá en su lugar y la aplicación permanecerá sin conexión. Por lo general, suele ser el comportamiento deseado. Para poner la aplicación en línea, puede eliminar el *aplicación\_offline.htm* archivo desde el servidor web. Como alternativa, si corrija los errores y ejecutar una implementación correcta, el *aplicación\_offline.htm* archivo que se va a quitar.


## <a name="conclusion"></a>Conclusión

En este tema se describe cómo tomar una aplicación web sin conexión para la duración de una implementación, al publicar un *aplicación\_offline.htm* archivos al servidor de destino al principio del proceso de implementación y quitar en el final. También describe cómo incluir un *aplicación\_offline.htm* archivo en un paquete de implementación web.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el empaquetado y el proceso de implementación, consulte [edificio y proyectos de aplicación Web de empaquetado](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurar parámetros para la implementación de paquete de Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), y [ Implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Si publicar sus aplicaciones web directamente desde Visual Studio, en lugar de usar el enfoque de archivo de proyecto de MSBuild personalizado descrito en estos tutoriales, necesitará utilizar un enfoque ligeramente diferente para hacer que su aplicación sin conexión durante la publicación proceso. Para obtener más información, consulte [cómo aprovechar la aplicación web sin conexión durante la publicación](https://go.microsoft.com/?linkid=9805135) (entrada de blog).

>[!div class="step-by-step"]
[Anterior](excluding-files-and-folders-from-deployment.md)
[Siguiente](running-windows-powershell-scripts-from-msbuild-project-files.md)
