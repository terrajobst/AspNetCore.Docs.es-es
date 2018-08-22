---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Implementación Web de ASP.NET con Visual Studio: configuración de permisos de carpeta | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 930f46c0ddb0b77525098291393e526107a542d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837836"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implementación Web de ASP.NET con Visual Studio: configuración de permisos de carpeta
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

En este tutorial, se establecen permisos de carpeta para el *Elmah* carpeta en la web implementada de sitio para que la aplicación puede crear archivos de registro de esa carpeta.

Cuando se prueba una aplicación web en Visual Studio con el servidor de desarrollo de Visual Studio (Cassini) o IIS Express, la aplicación se ejecuta bajo su identidad. Es más probable es que un administrador en el equipo de desarrollo y tener plena autoridad para hacer nada para cualquier archivo en cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta bajo la identidad definida para el que el sitio se asigna al grupo de aplicaciones. Esto suele ser una cuenta de definido por el sistema que tiene permisos limitados. De forma predeterminada ha leído y permisos de ejecución en los archivos y carpetas de la aplicación web, pero no tiene acceso de escritura.

Esto se convierte en un problema si la aplicación crea o actualiza los archivos, que es común que se necesitan en las aplicaciones web. En la aplicación Contoso University, Elmah crea archivos XML en el *Elmah* carpeta con el fin de guardar los detalles sobre los errores. Incluso si no usa algo parecido a Elmah, su sitio podría permitir que los usuarios cargar los archivos o realizar otras tareas que escriben datos en una carpeta en su sitio.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Informes y registro de errores de prueba

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hizo al probar en Visual Studio), puede provocar un error que normalmente se registraría por Elmah y, a continuación, abra el registro de errores Elmah para ver los detalles. Si no pudo crear un archivo XML y almacenar los detalles del error Elmah, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`, y, a continuación, solicitar una dirección URL no válida como *Studentsxxx.aspx*. Verá una página de error generados por el sistema en lugar de la *GenericErrorPage.aspx* página porque el `customErrors` configuración en el archivo Web.config es "RemoteOnly" y se ejecuta IIS localmente:

![Página de error 404 de HTTP](setting-folder-permissions/_static/image1.png)

Ahora ejecute *Elmah.axd* para ver el informe de errores. Después de iniciar sesión con las credenciales de cuenta de administrador (&quot;admin&quot; y &quot;devpwd&quot;), verá una página de registro de errores vacío porque no pudo crear un archivo XML en Elmah el *Elmah*carpeta:

![Error registro vacío](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Establecer permiso de escritura en la carpeta Elmah

Puede establecer permisos de carpetas manualmente o puede hacer que una parte del proceso de implementación automática. Lo que automática requiere código complejo de MSBuild y puesto que solo tiene que hacer esto la primera vez que implemente, los siguientes pasos describen cómo hacerlo manualmente. (Para obtener información acerca de cómo hacer que esta parte del proceso de implementación, consulte [establecer permisos de carpeta en Web publicar](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi.)

1. En **Explorador de archivos**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic en el *Elmah* carpeta, seleccione **propiedades**y, a continuación, seleccione el **seguridad** ficha.
2. Haga clic en **Editar**.
3. En el **permisos para Elmah** cuadro de diálogo, seleccione **DefaultAppPool**y, a continuación, seleccione el **escribir** casilla de verificación en la **permitir** columna.

    ![Permisos de carpeta ELMAH](setting-folder-permissions/_static/image3.png)

    (Si no ve **DefaultAppPool** en el **los nombres de usuario o grupo** lista, probablemente ha utilizado algún otro método a la especificada en este tutorial para configurar IIS y ASP.NET 4 en el equipo. En ese caso, averigüe qué identidad se usa el grupo de aplicaciones asignado a la aplicación Contoso University y conceda el permiso de escritura a esa identidad. Consulte los vínculos que tratan las identidades del grupo de aplicaciones al final de este tutorial). Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Vuelva a probar los informes y registro de errores

Probar, produciendo un error de nuevo en la misma manera (solicitud de una dirección URL incorrecta) y ejecute el **registro de errores** página. Esta vez el error aparece en la página.

![Página de registro de Error ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumen

Ahora ha completado todas las tareas necesarias obtener Contoso University funciona correctamente en IIS en el equipo local. En el siguiente tutorial, se hará que el sitio disponible públicamente mediante su implementación en Azure.

## <a name="more-information"></a>Más información

En este ejemplo, el motivo de por qué no pudo guardar los archivos de registro Elmah era bastante obvio. Puede utilizar el seguimiento de IIS en casos donde no es tan obvia; la causa del problema consulte [solución de problemas no se pudo las solicitudes de uso de seguimiento en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información acerca de cómo conceder permisos a las identidades del grupo de aplicaciones, consulte [identidades del grupo de aplicación](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de ACL del sistema de archivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

> [!div class="step-by-step"]
> [Anterior](deploying-to-iis.md)
> [Siguiente](deploying-to-production.md)
