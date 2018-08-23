---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de propiedades del proyecto: 4 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: ecd169f70eee162a647f6ea827ba5649b6c22ffd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838212"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de propiedades del proyecto: 4 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Algunas opciones de implementación se configuran en las propiedades del proyecto que se almacenan en el archivo de proyecto (el *.csproj* o *.vbproj* archivo). En la mayoría de los casos, los valores predeterminados de estas opciones son los que desea, pero puede usar el **las propiedades del proyecto** la interfaz de usuario integradas en Visual Studio para trabajar con esta configuración si tiene que cambiarlos. En este tutorial revise la configuración de implementación en **las propiedades del proyecto**. También creará un archivo de marcador de posición que hace que una carpeta vacía para implementarse.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configuración de opciones de implementación en la ventana Propiedades del proyecto

Mayoría de las opciones que afectan a lo que sucede durante la implementación se incluye en el perfil de publicación, como verá en los tutoriales siguientes. Algunas opciones de configuración que debe ser consciente de que se encuentran en el **Empaquetar/publicar** pestañas de la **las propiedades del proyecto** ventana. Estas opciones se especifican para cada configuración de compilación, es decir, puede tener diferentes valores para una versión de lanzamiento que tiene para una compilación de depuración.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto, seleccione **propiedades**y, a continuación, seleccione el **Empaquetar/Publicar Web**ficha.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Cuando se muestre la ventana, el valor predeterminado es que muestra los valores para cualquier configuración de compilación está activo actualmente para la solución. Si el **configuración** cuadro no indica **activo (versión)**, seleccione **versión** con el fin de mostrar la configuración para la configuración de compilación de versión. Las compilaciones de versión va a implementar en entornos de la prueba y producción.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con **activo (versión)** o **versión** seleccionado, verá que los valores que están vigentes cuando se implementa mediante la configuración de compilación de versión:

- En el **elementos para implementar** cuadro, **sólo los archivos necesarios para ejecutar la aplicación** está seleccionada. Otras opciones son **todos los archivos de este proyecto** o **todos los archivos de esta carpeta de proyecto**. Al dejar la selección predeterminada sin cambios Evite implementar archivos de código fuente, por ejemplo. Esta configuración es la razón de por qué las carpetas que contienen los archivos binarios de SQL Server Compact tenían que se incluirán en el proyecto. Para obtener más información sobre esta configuración, consulte **¿por qué no todos los archivos en mi carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuración generado de exclusión** está seleccionada. Que no depurando cuando se usa esta configuración de compilación.
- **Excluir archivos de la aplicación\_carpeta datos** no está seleccionada. El archivo para la base de datos de pertenencia de SQL Server Compact se encuentra en esa carpeta y tendrá que implementarlo. Al implementar las actualizaciones que no incluyen los cambios de la base de datos, seleccione esta casilla de verificación.
- **Precompilar esta aplicación antes de publicar** no está seleccionada. En la mayoría de los escenarios, no hace falta para precompilar los proyectos de aplicación web. Para obtener más información acerca de esta opción, consulte [pestaña Empaquetar/Publicar Web, las propiedades del proyecto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) y [avanzada precompilar cuadro de diálogo Configuración](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Incluir todas las bases de datos configuradas en la pestaña Empaquetar/publicar SQL** está seleccionada, pero esta opción no tiene ningún efecto ahora debido a no está configurando el **Empaquetar/publicar SQL** ficha. Esta pestaña es para un método de implementación de la base de datos heredada que solía ser la única opción para implementar bases de datos de SQL Server. Deberá usar el **Empaquetar/publicar SQL** pestaña en el [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial.
- El **configuración del paquete de implementación Web** sección no es aplicable porque usa un solo clic publicar en estos tutoriales.

Cambiar el **configuración** cuadro desplegable de depuración para ver la configuración predeterminada para las compilaciones de depuración. Los valores son iguales, salvo **excluir símbolos de depuración generados** está desactivada de manera que pueda depurar al implementar una compilación de depuración.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Hacer que el seguro de que se implementa la carpeta Elmah

Como se vio en el tutorial anterior, el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona la funcionalidad de registro e informes de errores. En la aplicación Contoso University Elmah se ha configurado para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluir determinados archivos o carpetas de implementación es un requisito común; otro ejemplo sería una carpeta que los usuarios pueden cargar archivos a. No desea que los archivos de registro o cargar archivos que se crearon en el entorno de desarrollo para su implementación en producción. Y si va a implementar una actualización en producción, que no desea que el proceso de implementación para eliminar los archivos que existen en producción. (Dependiendo de cómo establecer una opción de implementación, si existe un archivo en el sitio de destino, pero no en el sitio de origen cuando se implementa, Web Deploy lo elimina del destino.)

Como se vio anteriormente en este tutorial, el **elementos para implementar** opción el **Empaquetar/Publicar Web** ficha se establece en **sólo los archivos necesarios para ejecutar esta aplicación**. Como resultado, los archivos de registro creados por Elmah en el desarrollo no se implementará, que es lo que desea que ocurra. (Para implementarse, tendría que se incluirán en el proyecto y sus **acción de compilación** propiedad tendría que establecerse en **contenido**. Para obtener más información, consulte **¿por qué no todos los archivos en mi carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiar en él. Por lo tanto, agregará un *.txt* archivo en la carpeta para que actúe como un marcador de posición para que se va a copiar la carpeta.

En **el Explorador de soluciones**, haga clic en el *Elmah* carpeta, seleccione **Agregar nuevo elemento**y cree un archivo denominado *marcador.txt*. Ponga el texto siguiente: "Es un archivo de marcador de posición para asegurarse de que se implementa la carpeta". y guarde el archivo. Eso es todo lo que debe hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta se encuentra, porque el **acción de compilación** propiedad de *.txt* archivos se establece en **contenido**de forma predeterminada.

Ahora ha completado todas las tareas de configuración de implementación. En el siguiente tutorial, deberá implementar el sitio de Contoso University en el entorno de prueba y probarla.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
