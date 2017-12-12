---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: "Implementación de Web ASP.NET con Visual Studio: establecer los permisos de carpeta | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 19bef5ff97fd5b79135df8ca9bd6bd316594cc5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implementación de Web ASP.NET con Visual Studio: establecer los permisos de carpeta
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

En este tutorial, establecer los permisos de carpeta para la *Elmah* carpeta en la web implementadas de sitio para que la aplicación puede crear archivos de registro de esa carpeta.

Cuando se prueba una aplicación web en Visual Studio usando el servidor de desarrollo de Visual Studio (Cassini) o IIS Express, la aplicación se ejecuta bajo la identidad. Es más probable es que un administrador en el equipo de desarrollo y tiene autoridad completa que hacer nada para cualquier archivo en cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta bajo la identidad definida para el grupo de aplicaciones que se asigna el sitio a. Por lo general, suele ser una cuenta definida por el sistema que tiene permisos limitados. De forma predeterminada ha leído y permisos de ejecución en los archivos y carpetas de la aplicación web, pero no tiene acceso de escritura.

Esto resulta un problema si la aplicación crea o actualiza los archivos, que es común que se necesitan en las aplicaciones web. En la aplicación de la Universidad de Contoso, Elmah crea archivos XML en el *Elmah* carpeta para guardar los detalles sobre los errores. Incluso si no usa algo parecido a Elmah, el sitio podría permitir a los usuarios cargar archivos o realizar otras tareas que escriben datos en una carpeta de su sitio.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Informes y registro de errores de prueba

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hiciera cuando se prueba en Visual Studio), puede producir un error que normalmente se deben registrar Elmah y, a continuación, abra el registro de errores de Elmah para ver los detalles. Si no puede crear un archivo XML y almacenar los detalles del error Elmah, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`, y, a continuación, solicitar una dirección URL no válida como *Studentsxxx.aspx*. Verá una página de error generados por el sistema en lugar de la *GenericErrorPage.aspx* página porque el `customErrors` configuración en el archivo Web.config es "RemoteOnly" y se ejecuta IIS localmente:

![Página de error 404 de HTTP](setting-folder-permissions/_static/image1.png)

Ahora ejecute *Elmah.axd* para ver el informe de errores. Después de iniciar sesión con las credenciales de cuenta de administrador (&quot;administración&quot; y &quot;devpwd&quot;), verá una página de registro de errores vacío porque Elmah no pudo crear un archivo XML en el *Elmah*carpeta:

![Error registro vacío](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Establecer permiso de escritura en la carpeta Elmah

Puede establecer permisos de las carpetas manualmente o puede hacer que una parte del proceso de implementación automática. Lo que automática requiere código complejo de MSBuild y, puesto que solo tiene que hacer esto la primera vez que implemente, los siguientes pasos de cómo hacerlo manualmente. (Para obtener información acerca de cómo realizar esta parte del proceso de implementación, consulte [establecer permisos de carpeta en Web publicar](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi.)

1. En **Explorador de archivos**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic en el *Elmah* carpeta, seleccione **propiedades**y, a continuación, seleccione la **seguridad** ficha.
2. Haga clic en **editar**.
3. En el **permisos para Elmah** cuadro de diálogo, seleccione **DefaultAppPool**y, a continuación, seleccione la **escribir** casilla de verificación en la **permitir** columna.

    ![Permisos de carpeta ELMAH](setting-folder-permissions/_static/image3.png)

    (Si no ve **DefaultAppPool** en el **nombres de grupo o usuario** lista, probablemente ha utilizado algún otro método que el especificado en este tutorial para configurar IIS y ASP.NET 4 en el equipo. En ese caso, averigüe qué identidad es usada por el grupo de aplicaciones asignado a la aplicación de la Universidad de Contoso y conceda el permiso de escritura para esa identidad. Consulte los vínculos acerca de las identidades de grupo de aplicaciones al final de este tutorial). Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Volver a probar los informes y registro de errores

Probar, produciendo un error nuevo de la misma manera (solicitud de una dirección URL incorrecta) y ejecute el **registro de errores** página. Esta vez el error aparece en la página.

![Página de registro de errores ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumen

Ahora ha completado todas las tareas necesarias obtener la Universidad de Contoso funciona correctamente en IIS en el equipo local. En el siguiente tutorial, realizará el sitio disponible públicamente mediante la implementación en Azure.

## <a name="more-information"></a>Más información

En este ejemplo, motivo por qué se ha podido guardar archivos de registro Elmah era bastante obvio. Puede utilizar el seguimiento de IIS en casos donde la causa del problema no es tan obvia; vea [solución de problemas de solicitudes utilizando seguimiento de error en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información acerca de cómo conceder permisos a las identidades del grupo de aplicaciones, consulte [identidades del grupo de aplicaciones](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de ACL del sistema archivo](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

>[!div class="step-by-step"]
[Anterior](deploying-to-iis.md)
[Siguiente](deploying-to-production.md)
