---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización Code-Only - 8 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: a07ac968482866ba9a4eca25722d99fd641080e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización Code-Only - 8 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Tras la implementación inicial, continúa el trabajo de mantenimiento y desarrollo de su sitio web y, en poco tiempo desea implementar una actualización. Este tutorial le guiará por el proceso de implementar una actualización en el código de aplicación. Esta actualización no implica un cambio de la base de datos; podrá ver lo que es diferente sobre la implementación de un cambio de base de datos en el siguiente tutorial.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Realizar un cambio de código

Como ejemplo sencillo de una actualización a la aplicación, agregará a la **instructores** página una lista de cursos que imparte el instructor seleccionado.

Si ejecuta el **instructores** página, observará que no hay **seleccione** vínculos en la cuadrícula, pero estos no hagan algo distinto de marca la fila segundo plano se volverán grises.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Ahora agregará código que se ejecuta cuando el **seleccione** vínculo se hace clic en y muestra una lista de cursos que imparte el instructor seleccionado.

En *Instructors.aspx*, agregue el siguiente marcado inmediatamente después de la **ErrorMessageLabel** `Label` control:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Ejecute la página y seleccione un instructor. Ver una lista de cursos imparten por ese instructor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implementación de la actualización de código en el entorno de prueba

Implementación en el entorno de prueba es cuestión de ejecución con un solo clic, vuelva a publicar. Para facilitar este proceso más rápido, puede usar el **Web uno haga clic en publicar** barra de herramientas.

En el **vista** menú, elija **las barras de herramientas** y, a continuación, seleccione **Web uno haga clic en publicar**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity.

el **Web uno haga clic en publicar** barra de herramientas, elija la **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web** (el icono con flechas que apuntan a izquierda y derecha).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio implementa la aplicación actualizada, y el explorador se abre automáticamente en la página principal. Ejecute la página de instructores y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Lo haría normalmente también hacer pruebas de regresión (es decir, probar el resto del sitio para asegurarse de que el nuevo cambio no interrumpe cualquier funcionalidad existente). Pero en este tutorial podrá omitir este paso y continuar para implementar la actualización en producción.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedir volver a implementar el estado de la base de datos inicial a producción

En una aplicación real, los usuarios interactúan con el sitio de producción después de la implementación inicial y las bases de datos se rellenan con datos activos. Por lo tanto, no desea volver a implementar la base de datos de pertenencia en su estado inicial, que se borran todos los datos en directo. Puesto que las bases de datos de SQL Server Compact son archivos en el *aplicación\_datos* carpeta, tiene que evitar que esto cambiando la configuración de implementación para que los archivos en el *aplicación\_datos* carpeta no se implementan.

Abra la **propiedades del proyecto** ventana para el proyecto ContosoUniversity y seleccione la **Empaquetar/Publicar Web** ficha. Asegúrese de que el **configuración** cuadro desplegable ha **activo (versión)** o **versión** seleccionada, seleccione **excluir archivos de la aplicación\_Carpeta de datos**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

En caso de que se decide implementar una compilación de depuración en el futuro, es una buena idea para realizar el mismo cambio de la configuración de compilación de depuración: cambiar **configuración** a **depurar** y, a continuación, seleccione **excluir archivos de la aplicación\_carpeta de datos**.

Guarde y cierre el **Empaquetar/Publicar Web** ficha.

> [!NOTE] 
> 
> [!IMPORTANT]
> Asegúrese de que no tiene **quitar archivos adicionales en destino** seleccionado en los perfiles de publicación. Si selecciona esta opción, el proceso de implementación eliminará las bases de datos que haya en la aplicación\_datos en el sitio implementado y eliminarán la aplicación\_carpeta de datos propiamente dicha.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedir el acceso de usuario para el sitio de producción durante la actualización

El cambio que se va a implementar es ahora un cambio simple en una sola página. Pero a veces implementar cambios mayores y, en ese caso el sitio puede comportarse de manera extraña aunque si un usuario solicita una página antes de que finalice la implementación. Para evitar esto, puede utilizar un *aplicación\_offline.htm* archivo. Cuando coloca un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo que para evitar el acceso durante la implementación, coloca *aplicación\_offline.htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quitar *aplicación\_offline.htm*.

En **el Explorador de soluciones**, haga clic en la solución (no de uno de los proyectos) y seleccione **nueva carpeta de soluciones**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nombre de la carpeta *SolutionFiles*.

En la nueva carpeta, cree una página HTML con el nombre *aplicación\_offline.htm*. Reemplace el contenido existente por el siguiente marcado:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Puede copiar la *aplicación\_offline.htm* archivo al sitio mediante el uso de una conexión FTP o **el Administrador de archivos** utilidad en el panel de control del proveedor de hospedaje. Para este tutorial, usará el **el Administrador de archivos**.

Abra el panel de control y seleccione **el Administrador de archivos** como hizo en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. Seleccione **contosouniversity.com** y, a continuación, **wwwroot** para llegar a la carpeta raíz de la aplicación y, a continuación, haga clic en **cargar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

En el **cargar archivo** cuadro de diálogo, seleccione la *aplicación\_offline.htm* de archivos y, a continuación, haga clic en **cargar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Ir a dirección URL de su sitio. Verá que la *aplicación\_offline.htm* página aparece ahora en lugar de la página principal.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Ahora está listo para implementarse en producción.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implementación de la actualización de código en el entorno de producción

En el **Web uno haga clic en publicar** barra de herramientas, elija la **producción** perfil de publicación y, a continuación, haga clic en **Publicar Web**.

Visual Studio implementa la aplicación actualizada y abre el explorador a la página principal del sitio. El *aplicación\_offline.htm* se muestra el archivo. Para poder probar para comprobar una implementación correcta, debe quitar la *aplicación\_offline.htm* archivo.

Vuelva a la **el Administrador de archivos** aplicación en el panel de control. Seleccione **contosouniversity.com** y **wwwroot**, seleccione **aplicación\_offline.htm**y, a continuación, haga clic en **eliminar**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

En el explorador, abra la página de instructores en el sitio público y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Ahora que ha implementado una actualización de la aplicación que no incluía un cambio de base de datos. El siguiente tutorial muestra cómo implementar un cambio de base de datos.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
