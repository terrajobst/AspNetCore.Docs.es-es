---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "Implementación de Web ASP.NET con Visual Studio: implementar archivos adicionales | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implementación de Web ASP.NET con Visual Studio: implementar archivos adicionales
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo extender Visual Studio publicación web de canalización para realizar una tarea adicional durante la implementación. La tarea consiste en copiar los archivos adicionales que no están en la carpeta del proyecto para el sitio web de destino.

En este tutorial se copiará un archivo adicional: *robots.txt*. Va a implementar este archivo a ensayo pero no a producción. En [la implementación en producción](deploying-to-production.md) tutorial, agrega este archivo al proyecto y configurar la producción de perfil para excluirla de publicación. En este tutorial, verá un método alternativo para controlar esta situación, uno que le serán útiles para los archivos que desea implementar, pero no desea incluir en el proyecto.

## <a name="move-the-robotstxt-file"></a>Mover el archivo robots.txt

Para prepararse para un método diferente de control de *robots.txt*, en esta sección del tutorial mover el archivo a una carpeta que no se incluye en el proyecto y se elimina *robots.txt* desde el almacenamiento provisional entorno. Es necesario eliminar el archivo de almacenamiento provisional para que pueda comprobar que el nuevo método de implementar el archivo en dicho entorno funciona correctamente.

1. En **el Explorador de soluciones**, haga clic en el *robots.txt* de archivo y haga clic en **excluir del proyecto**.
2. Mediante el Explorador de archivos de Windows, cree una carpeta nueva en la carpeta de soluciones y asígnele el nombre *ExtraFiles*.
3. Mover el *robots.txt* de archivos desde el *ContosoUniversity* carpeta de proyecto para la *ExtraFiles* carpeta.

    ![Carpeta ExtraFiles](deploying-extra-files/_static/image1.png)
4. Mediante la herramienta FTP, eliminar el *robots.txt* archivo desde el sitio web de ensayo.

    Como alternativa, puede seleccionar **quitar archivos adicionales en destino** en **File Publish Options** en el **configuración** ficha del perfil de publicación de ensayo, y volver a publicar para almacenamiento provisional.

## <a name="update-the-publish-profile-file"></a>Actualizar el archivo de perfil de publicación

Solo necesita *robots.txt* de ensayo, por lo que el perfil de publicación solo debe actualizar para poder implementarlo es de almacenamiento provisional.

1. En Visual Studio, abra *Staging.pubxml*.
2. Al final del archivo, antes del cierre `</Project>` etiqueta, agregue el siguiente marcado:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Este código crea un nuevo *destino* que recopilará los archivos adicionales que se implementen. Un destino se compone de uno o más tareas que va a ejecutar MSBuild según las condiciones que especifique.

    El `Include` atributo especifica que la carpeta en la que se va a buscar los archivos es *ExtraFiles*, que se encuentra en el mismo nivel que la carpeta del proyecto. MSBuild recopilar todos los archivos de esa carpeta y de todas las subcarpetas (el asterisco doble especifica recursiva subcarpetas) de forma recursiva. Con este código podría colocar varios archivos y archivos en subcarpetas de la *ExtraFiles* carpeta y todas se implementará.

    El `DestinationRelativePath` elemento especifica que los archivos y carpetas deben copiarse a la carpeta raíz del sitio web de destino, en la misma estructura de carpetas y archivos como que se encuentran en el *ExtraFiles* carpeta. Si desea copiar la *ExtraFiles* carpeta propiamente dicha, el `DestinationRelativePath` valor sería *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Al final del archivo, antes del cierre `</Project>` etiqueta, agregue el siguiente marcado que especifica cuándo se debe ejecutar el nuevo destino.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Este código hace que el nuevo `CustomCollectFiles` destino que se ejecuta cuando se ejecuta el destino que copia archivos en la carpeta de destino. Hay un destino independiente para publicar en comparación con la creación del paquete de implementación y el nuevo destino se inyecta en ambos destinos en caso de que decida implementar mediante un paquete de implementación en lugar de la publicación.

    El *.pubxml* archivo ahora tiene un aspecto similar al ejemplo siguiente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Guarde y cierre el *Staging.pubxml* archivo.

## <a name="publish-to-staging"></a>Publicar en almacenamiento provisional

Con un solo clic publicar o la línea de comandos, publicar la aplicación mediante el perfil de almacenamiento provisional.

Si usa un solo clic publicar, puede comprobar en la **vista previa** ventana que *robots.txt* se va a copiar. En caso contrario, use la herramienta FTP para comprobar que la *robots.txt* archivo se encuentra en la carpeta raíz del sitio web después de la implementación.

## <a name="summary"></a>Resumen

Con esto finaliza la siguiente serie de tutoriales sobre la implementación de una aplicación web ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información acerca de cualquiera de los temas tratados en estos tutoriales, vea el [mapa de contenido de implementación de ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Más información

Si sabe cómo trabajar con archivos de MSBuild, puede automatizar muchas otras tareas de implementación mediante la escritura de código *.pubxml* archivos (para tareas específicas del perfil) o en el proyecto *. wpp.targets* archivo (para las tareas que se aplica a todos los perfiles). Para obtener más información acerca de *.pubxml* y *. wpp.targets* archivos, vea [Cómo: modificar la configuración de implementación en los archivos de un perfil de publicación (.pubxml) y. wpp.targets archivo en Web de Visual Studio Proyectos](https://msdn.microsoft.com/en-us/library/ff398069). Para obtener una introducción básica a código de MSBuild, vea **Anatomía de un archivo de proyecto** en [serie de implementación empresarial: comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para obtener información sobre cómo trabajar con archivos de MSBuild para realizar tareas para sus propios escenarios, consulte este libro: [dentro de la Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi y William Bartholomew.

## <a name="acknowledgements"></a>Reconocimientos

Me gustaría dar las gracias a las siguientes personas que realizado aportaciones importantes para el contenido de esta serie de tutoriales:

- [Alberto Poblacion, MVP &amp; MCT, España](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- Estados Unidos de Jarod Ferguson, MVP de desarrollo de plataforma de datos,
- Duras Mittal, Microsoft
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

>[!div class="step-by-step"]
[Anterior](command-line-deployment.md)
[Siguiente](troubleshooting.md)
