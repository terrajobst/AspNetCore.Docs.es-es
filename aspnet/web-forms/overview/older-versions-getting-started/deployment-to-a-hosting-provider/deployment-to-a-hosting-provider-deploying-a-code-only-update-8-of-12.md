---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización Code-Only - 8 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: cc994f989734153592ce78af5f4be57953b24458
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837344"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización Code-Only - 8 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Tras la implementación inicial, continúa el trabajo de mantenimiento y desarrollo de su sitio web y, en poco tiempo desea implementar una actualización. Este tutorial le guiará por el proceso de implementación de una actualización en el código de aplicación. Esta actualización no implica un cambio de base de datos; podrá ver cuál es la diferencia acerca de cómo implementar un cambio de base de datos en el siguiente tutorial.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Realizar un cambio de código

Como ejemplo sencillo de una actualización a la aplicación, agregará el **instructores** página una lista de cursos impartidos por el instructor seleccionado.

Si ejecuta el **instructores** página, observará que hay **seleccione** vínculos en la cuadrícula, pero no es algo distinto de make el color gris a su vez de fila en segundo plano.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Ahora agregará código que se ejecuta cuando el **seleccione** vínculo está seleccionado y muestra una lista de cursos impartidos por el instructor seleccionado.

En *Instructors.aspx*, agregue el marcado siguiente inmediatamente después de la **ErrorMessageLabel** `Label` control:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Ejecute la página y seleccione un instructor. Vea una lista de cursos impartidos por ese instructor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implementación de la actualización del código en el entorno de prueba

Implementación en el entorno de prueba es solo cuestión de ejecución con un solo clic, vuelva a publicar. Para agilizar este proceso, puede usar el **una publicación en Web clic** barra de herramientas.

En el **vista** menú, elija **las barras de herramientas** y, a continuación, seleccione **una publicación en Web clic**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity.

el **una publicación en Web clic** barra de herramientas, elija la **prueba** perfil de publicación y, a continuación, haga clic en **publicación Web** (el icono con flechas que señalan al left y right).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio implementa la aplicación actualizada, y el explorador se abre automáticamente en la página principal. Ejecute la página de instructores y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Lo haría normalmente también hacer las pruebas de regresión (es decir, el resto del sitio para asegurarse de que el nuevo cambio no interrumpa ninguna funcionalidad existente de prueba). Pero para este tutorial podrá omitir ese paso y continuar para implementar la actualización en producción.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedir volver a implementar el estado de la base de datos inicial a producción

En una aplicación real, los usuarios interactúan con el sitio de producción después de la implementación inicial y las bases de datos se rellenan con datos activos. Por lo tanto, no desea volver a implementar la base de datos de pertenencia en su estado inicial, que se borran todos los datos en directo. Puesto que las bases de datos de SQL Server Compact son archivos en el *aplicación\_datos* carpeta, tendrá que evitar que esto cambiando la configuración de implementación para que los archivos en el *aplicación\_datos* carpeta no se implementa.

Abra el **las propiedades del proyecto** ventana para el proyecto ContosoUniversity y seleccione el **Empaquetar/Publicar Web** ficha. Asegúrese de que el **configuración** cuadro desplegable ha **activo (versión)** o **versión** seleccionado, seleccione **excluir archivos de la aplicación\_Carpeta de datos**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

En caso de que decida implementar una compilación de depuración en el futuro, es una buena idea para realizar el mismo cambio para la configuración de compilación de depuración: cambiar **configuración** a **depurar** y, a continuación, seleccione **excluir los archivos de la aplicación\_carpeta de datos**.

Guarde y cierre el **Empaquetar/Publicar Web** ficha.

> [!NOTE] 
> 
> [!IMPORTANT]
> Asegúrese de que no tiene **quitar archivos adicionales en destino** seleccionado en los perfiles de publicación. Si selecciona esta opción, el proceso de implementación eliminará las bases de datos que tiene en la aplicación\_datos en el sitio implementado y se eliminará la aplicación\_propia carpeta de datos.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedir el acceso de usuario al sitio de producción durante la actualización

El cambio que va a implementar ahora es un simple cambio en una sola página. Pero a veces se implementan cambios mayores y, en ese caso el sitio puede comportarse de forma extraña si un usuario solicita una página antes de que finalice la implementación. Para evitar esto, puede usar un *aplicación\_offline.htm* archivo. Cuando coloca un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo que colocar para evitar el acceso durante la implementación, *aplicación\_offline.htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *aplicación\_offline.htm*.

En **el Explorador de soluciones**, haga clic en la solución (no de uno de los proyectos) y seleccione **nueva carpeta de soluciones**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nombre de la carpeta *SolutionFiles*.

En la nueva carpeta, cree una página HTML llamada *aplicación\_offline.htm*. Reemplace el contenido existente por el siguiente marcado:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Puede copiar el *aplicación\_offline.htm* archivo en el sitio mediante el uso de una conexión FTP o la **administrador archivos** utilidad en el panel de control del proveedor de hospedaje. Para este tutorial, usará el **administrador archivos**.

Abra el panel de control y seleccione **administrador archivos** como hizo en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. Seleccione **contosouniversity.com** y, a continuación, **wwwroot** para llegar a la carpeta raíz de la aplicación y, a continuación, haga clic en **cargar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

En el **cargar archivo** cuadro de diálogo, seleccione el *aplicación\_offline.htm* de archivo y, a continuación, haga clic en **cargar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Vaya a la dirección URL del sitio. Verá que el *aplicación\_offline.htm* página aparece ahora en lugar de la página principal.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Ahora está listo para implementarse en producción.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implementación de la actualización del código en el entorno de producción

En el **una publicación en Web clic** barra de herramientas, elija la **producción** perfil de publicación y, a continuación, haga clic en **publicación Web**.

Visual Studio implementa la aplicación actualizada y abre el explorador a la página principal del sitio. El *aplicación\_offline.htm* se muestra el archivo. Antes de poder probar para comprobar la implementación correcta, debe quitar la *aplicación\_offline.htm* archivo.

Vuelva a la **administrador archivos** aplicación en el panel de control. Seleccione **contosouniversity.com** y **wwwroot**, seleccione **aplicación\_offline.htm**y, a continuación, haga clic en **eliminar**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

En el explorador, abra la página de instructores en el sitio público y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Ahora ha implementado una actualización de la aplicación que no incluía un cambio de base de datos. El siguiente tutorial muestra cómo implementar un cambio de base de datos.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
