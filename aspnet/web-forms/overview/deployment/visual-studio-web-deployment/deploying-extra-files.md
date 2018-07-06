---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Implementación Web de ASP.NET con Visual Studio: implementar archivos adicionales | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820855"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implementación Web de ASP.NET con Visual Studio: implementar archivos adicionales
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo extender Visual Studio web publicar canalización para realizar una tarea adicional durante la implementación. La tarea consiste en copiar los archivos adicionales que no están en la carpeta del proyecto en el sitio web de destino.

En este tutorial se copiará un archivo adicional: *robots.txt*. Desea implementar este archivo en el almacenamiento provisional, pero no para producción. En [la implementación en producción](deploying-to-production.md) tutorial, ha agregado este archivo al proyecto y configura la producción perfil para excluirla de publicación. En este tutorial, verá un método alternativo para controlar esta situación, uno que sea útil para los archivos que desea implementar, pero no desea incluir en el proyecto.

## <a name="move-the-robotstxt-file"></a>Mueva el archivo robots.txt

Para prepararse para un método diferente de control de *robots.txt*, en esta sección del tutorial mueva el archivo a una carpeta que no se incluye en el proyecto y se elimina *robots.txt* desde el almacenamiento provisional entorno. Es necesario eliminar el archivo de almacenamiento provisional para que pueda comprobar que funciona correctamente al nuevo método de implementar el archivo en ese entorno.

1. En **el Explorador de soluciones**, haga clic en el *robots.txt* de archivo y haga clic en **excluir del proyecto**.
2. Mediante el Explorador de archivos de Windows, cree una nueva carpeta en la carpeta de soluciones y asígnele el nombre *ExtraFiles*.
3. Mover el *robots.txt* de archivos desde el *ContosoUniversity* carpeta de proyecto para el *ExtraFiles* carpeta.

    ![Carpeta ExtraFiles](deploying-extra-files/_static/image1.png)
4. Mediante la herramienta FTP, elimine el *robots.txt* archivo desde el sitio web de ensayo.

    Como alternativa, puede seleccionar **quitar archivos adicionales en destino** en **File Publish Options** en el **configuración** pestaña de perfil de publicación de ensayo, y Vuelva a publicar en el almacenamiento provisional.

## <a name="update-the-publish-profile-file"></a>Actualizar el archivo de perfil de publicación

Solo necesita *robots.txt* en almacenamiento provisional, por lo que es el perfil de publicación solo debe actualizar con el fin de implementarlo en prueba.

1. En Visual Studio, abra *Staging.pubxml*.
2. Al final del archivo, antes del cierre `</Project>` etiquetar, agregue el siguiente marcado:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Este código crea un nuevo *destino* que recopilará los archivos adicionales que se implementarán. Un destino se compone de uno o más tareas de MSBuild se ejecutará según las condiciones que especifique.

    El `Include` atributo especifica que la carpeta donde se encuentran los archivos es *ExtraFiles*, que se encuentra en el mismo nivel que la carpeta del proyecto. MSBuild recopilará todos los archivos de esa carpeta y todas las subcarpetas (el asterisco doble especifica recursiva subcarpetas) de forma recursiva. Con este código podría poner varios archivos y archivos en subcarpetas dentro de la *ExtraFiles* carpeta y todas se implementarán.

    El `DestinationRelativePath` elemento especifica que los archivos y carpetas deben copiarse a la carpeta raíz del sitio web de destino, en la misma estructura de archivos y carpetas que se encuentran en el *ExtraFiles* carpeta. Si desea copiar el *ExtraFiles* propia carpeta, el `DestinationRelativePath` valor sería *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Al final del archivo, antes del cierre `</Project>` etiquetar, agregue el siguiente marcado que especifica cuándo se debe ejecutar el destino de nuevo.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Este código hace que el nuevo `CustomCollectFiles` destino que se ejecutará cada vez que se ejecuta el destino que copia archivos en la carpeta de destino. Hay un destino independiente para publicar en comparación con la creación del paquete de implementación y el destino nuevo se inserta en ambos destinos en caso de que decida implementar mediante un paquete de implementación en lugar de la publicación.

    El *.pubxml* archivo ahora tiene un aspecto similar al ejemplo siguiente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Guarde y cierre el *Staging.pubxml* archivo.

## <a name="publish-to-staging"></a>Publicar en el almacenamiento provisional

Publicar con un solo clic o la línea de comandos, publicar la aplicación mediante el perfil de almacenamiento provisional.

Si usa un solo clic publicar, puede comprobar en el **Preview** ventana que *robots.txt* se va a copiar. En caso contrario, use la herramienta FTP para comprobar que la *robots.txt* archivo está en la carpeta raíz del sitio web después de la implementación.

## <a name="summary"></a>Resumen

Con esto finaliza esta serie de tutoriales sobre la implementación de una aplicación web ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información acerca de cualquiera de los temas tratados en estos tutoriales, vea el [mapa de contenido de implementación ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Más información

Si sabe cómo trabajar con archivos de MSBuild, puede automatizar muchas otras tareas de implementación escribiendo código en *.pubxml* archivos (para tareas específicas del perfil) o el proyecto *. wpp.targets* archivo (para las tareas que se aplican a todos los perfiles). Para obtener más información acerca de *.pubxml* y *. wpp.targets* archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil de publicación (.pubxml) y el. wpp.targets archivo en Web de Visual Studio Proyectos](https://msdn.microsoft.com/library/ff398069). Para obtener una introducción básica al código de MSBuild, vea **Anatomía de un archivo de proyecto** en [Enterprise Deployment Series: descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para obtener información sobre cómo trabajar con archivos de MSBuild para realizar tareas para sus propios escenarios, consulte este libro: [dentro de la Microsoft Build Engine: uso de MSBuild y Team Foundation Build](http://msbuildbook.com) por Sayed Ibraham Hashimi y William Bartholomew.

## <a name="acknowledgements"></a>Reconocimientos

Gustaría agradecer a las siguientes personas que realizan aportaciones significativas al contenido de esta serie de tutoriales:

- [Alberto Poblacion, MVP &amp; MCT, España](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP de desarrollo de plataforma de datos, España
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](command-line-deployment.md)
> [Siguiente](troubleshooting.md)
