---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta - 6 de 12 | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887270"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta - 6 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, establecer los permisos de carpeta para la *Elmah* carpeta en la web implementadas de sitio para que la aplicación puede crear archivos de registro de esa carpeta.

Cuando se prueba una aplicación web en Visual Studio usando el servidor de desarrollo de Visual Studio (Cassini), la aplicación se ejecuta bajo la identidad. Es más probable es que un administrador en el equipo de desarrollo y tiene autoridad completa que hacer nada para cualquier archivo en cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta bajo la identidad definida para el grupo de aplicaciones que se asigna el sitio a. Por lo general, suele ser una cuenta definida por el sistema que tiene permisos limitados. De forma predeterminada ha leído y permisos de ejecución en los archivos y carpetas de la aplicación web, pero no tiene acceso de escritura.

Esto resulta un problema si la aplicación crea o actualiza los archivos, que es común que se necesitan en las aplicaciones web. En la aplicación de la Universidad de Contoso, Elmah crea archivos XML en el *Elmah* carpeta para guardar los detalles sobre los errores. Incluso si no usa algo parecido a Elmah, el sitio podría permitir a los usuarios cargar archivos o realizar otras tareas que escriben datos en una carpeta de su sitio.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Registro e informes de errores de pruebas

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hiciera cuando se prueba en Visual Studio), puede producir un error que normalmente se deben registrar Elmah y, a continuación, abra el registro de errores de Elmah para ver los detalles. Si no puede crear un archivo XML y almacenar los detalles del error Elmah, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`, y, a continuación, solicitar una dirección URL no válida como *Studentsxxx.aspx*. Verá una página de error generados por el sistema en lugar de la *GenericErrorPage.aspx* página porque el `customErrors` configuración en el archivo Web.config es "RemoteOnly" y se ejecuta IIS localmente:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Ahora ejecute *Elmah.axd* para ver el informe de errores. Vea una página de registro de errores vacío porque Elmah no pudo crear un archivo XML en el *Elmah* carpeta:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Configuración de permiso de escritura en la carpeta Elmah

Puede establecer permisos de las carpetas manualmente o puede hacer que una parte del proceso de implementación automática. Lo que automática requiere código de MSBuild complejo y puesto que solo tiene que hacer esto la primera vez que se implementa, este tutorial solo muestra cómo hacerlo manualmente. (Para obtener información acerca de cómo realizar esta parte del proceso de implementación, consulte [establecer permisos de carpeta en Web publicar](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi.)

En **el Explorador de Windows**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic en el *Elmah* carpeta, seleccione **propiedades**y, a continuación, seleccione la **seguridad** ficha.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Si no ve **DefaultAppPool** en el **nombres de grupo o usuario** lista, probablemente ha utilizado algún otro método que el especificado en este tutorial para configurar IIS y ASP.NET 4 en el equipo. En ese caso, averigüe qué identidad es usada por el grupo de aplicaciones asignado a la aplicación de la Universidad de Contoso y conceda el permiso de escritura para esa identidad. Consulte los vínculos acerca de las identidades de grupo de aplicaciones al final de este tutorial).

Haga clic en **Editar**. En el **permisos para Elmah** cuadro de diálogo, seleccione **DefaultAppPool**y, a continuación, seleccione la **escribir** casilla de verificación en la **permitir** columna.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retesting-error-logging-and-reporting"></a>Al volver a examinar el registro e informes de errores

Probar, produciendo un error nuevo de la misma manera (solicitud de una dirección URL incorrecta) y ejecute el **registro de errores** página. Esta vez el error aparece en la página.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

También necesita permiso de escritura en el *aplicación\_datos* carpeta ya tiene archivos de base de datos de SQL Server Compact en esa carpeta y desea poder actualizar los datos de esas bases de datos. En ese caso, sin embargo, no tiene que hacer nada más, dado que el proceso de implementación establece automáticamente el permiso de escritura en el *aplicación\_datos* carpeta.

Ahora ha completado todas las tareas necesarias obtener la Universidad de Contoso funciona correctamente en IIS en el equipo local. En el siguiente tutorial, realizará el sitio disponible públicamente mediante la implementación en un proveedor de hospedaje.

## <a name="more-information"></a>Más información

En este ejemplo, motivo por qué se ha podido guardar archivos de registro Elmah era bastante obvio. Puede utilizar el seguimiento de IIS en casos donde la causa del problema no es tan obvia; vea [solución de problemas de solicitudes utilizando seguimiento de error en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información acerca de cómo conceder permisos a las identidades del grupo de aplicaciones, consulte [identidades del grupo de aplicaciones](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de ACL del sistema archivo](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
