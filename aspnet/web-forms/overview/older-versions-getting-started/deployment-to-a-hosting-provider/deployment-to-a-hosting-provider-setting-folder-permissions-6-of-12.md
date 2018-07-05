---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta - 6 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 17910952e78418ef9e5b80efb04b5fe1e70c11bf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385316"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta - 6 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, se establecen permisos de carpeta para el *Elmah* carpeta en la web implementada de sitio para que la aplicación puede crear archivos de registro de esa carpeta.

Cuando se prueba una aplicación web en Visual Studio con el servidor de desarrollo de Visual Studio (Cassini), la aplicación se ejecuta bajo su identidad. Es más probable es que un administrador en el equipo de desarrollo y tener plena autoridad para hacer nada para cualquier archivo en cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta bajo la identidad definida para el que el sitio se asigna al grupo de aplicaciones. Esto suele ser una cuenta de definido por el sistema que tiene permisos limitados. De forma predeterminada ha leído y permisos de ejecución en los archivos y carpetas de la aplicación web, pero no tiene acceso de escritura.

Esto se convierte en un problema si la aplicación crea o actualiza los archivos, que es común que se necesitan en las aplicaciones web. En la aplicación Contoso University, Elmah crea archivos XML en el *Elmah* carpeta con el fin de guardar los detalles sobre los errores. Incluso si no usa algo parecido a Elmah, su sitio podría permitir que los usuarios cargar los archivos o realizar otras tareas que escriben datos en una carpeta en su sitio.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Registro e informes de errores de pruebas

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hizo al probar en Visual Studio), puede provocar un error que normalmente se registraría por Elmah y, a continuación, abra el registro de errores Elmah para ver los detalles. Si no pudo crear un archivo XML y almacenar los detalles del error Elmah, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`, y, a continuación, solicitar una dirección URL no válida como *Studentsxxx.aspx*. Verá una página de error generados por el sistema en lugar de la *GenericErrorPage.aspx* página porque el `customErrors` configuración en el archivo Web.config es "RemoteOnly" y se ejecuta IIS localmente:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Ahora ejecute *Elmah.axd* para ver el informe de errores. Vea una página de registro de errores vacío porque no pudo crear un archivo XML en Elmah el *Elmah* carpeta:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Configuración de permiso de escritura en la carpeta Elmah

Puede establecer permisos de carpetas manualmente o puede hacer que una parte del proceso de implementación automática. Lo que automática requiere código complejo de MSBuild y puesto que solo tiene que hacer esto la primera vez que implemente, este tutorial solo muestra cómo hacerlo manualmente. (Para obtener información acerca de cómo hacer que esta parte del proceso de implementación, consulte [establecer permisos de carpeta en Web publicar](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi.)

En **Windows Explorer**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic en el *Elmah* carpeta, seleccione **propiedades**y, a continuación, seleccione el **seguridad** ficha.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Si no ve **DefaultAppPool** en el **los nombres de usuario o grupo** lista, probablemente ha utilizado algún otro método a la especificada en este tutorial para configurar IIS y ASP.NET 4 en el equipo. En ese caso, averigüe qué identidad se usa el grupo de aplicaciones asignado a la aplicación Contoso University y conceda el permiso de escritura a esa identidad. Consulte los vínculos que tratan las identidades del grupo de aplicaciones al final de este tutorial).

Haga clic en **Editar**. En el **permisos para Elmah** cuadro de diálogo, seleccione **DefaultAppPool**y, a continuación, seleccione el **escribir** casilla de verificación en la **permitir** columna.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retesting-error-logging-and-reporting"></a>Al volver a examinar el registro e informes de errores

Probar, produciendo un error de nuevo en la misma manera (solicitud de una dirección URL incorrecta) y ejecute el **registro de errores** página. Esta vez el error aparece en la página.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

También necesita permiso de escritura en el *aplicación\_datos* carpeta porque tiene archivos de base de datos de SQL Server Compact en esa carpeta y desea poder actualizar los datos de esas bases de datos. En ese caso, sin embargo, no tiene que hacer nada más, como el proceso de implementación configura automáticamente el permiso de escritura en el *aplicación\_datos* carpeta.

Ahora ha completado todas las tareas necesarias obtener Contoso University funciona correctamente en IIS en el equipo local. En el siguiente tutorial, se hará que el sitio disponible públicamente mediante su implementación en un proveedor de hospedaje.

## <a name="more-information"></a>Más información

En este ejemplo, el motivo de por qué no pudo guardar los archivos de registro Elmah era bastante obvio. Puede utilizar el seguimiento de IIS en casos donde no es tan obvia; la causa del problema consulte [solución de problemas no se pudo las solicitudes de uso de seguimiento en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información acerca de cómo conceder permisos a las identidades del grupo de aplicaciones, consulte [identidades del grupo de aplicación](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de ACL del sistema de archivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
