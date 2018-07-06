---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Implementación Web de ASP.NET con Visual Studio: las propiedades del proyecto | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 86e4e3ef5f126a5bf8abc91c2d5ce3d14b1e1c6c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837670"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implementación Web de ASP.NET con Visual Studio: las propiedades del proyecto
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Algunas opciones de implementación se configuran en las propiedades del proyecto que se almacenan en el archivo de proyecto (el *.csproj* o *.vbproj* archivo). En la mayoría de los casos, los valores predeterminados de estas opciones son los que desea, pero puede usar el **las propiedades del proyecto** la interfaz de usuario integradas en Visual Studio para trabajar con esta configuración si tiene que cambiarlos. En este tutorial revise la configuración de implementación en **las propiedades del proyecto**. También creará un archivo de marcador de posición que hace que una carpeta vacía para implementarse.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurar opciones de implementación en la ventana Propiedades del proyecto

Mayoría de las opciones que afectan a lo que sucede durante la implementación se incluye en el perfil de publicación, como verá en los tutoriales siguientes. Algunas opciones de configuración que debe ser consciente de que se encuentran en el **Empaquetar/publicar** pestañas de la **las propiedades del proyecto** ventana. Estas opciones se especifican para cada configuración de compilación, es decir, puede tener diferentes valores para una versión de lanzamiento que tiene para una compilación de depuración.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto, seleccione **propiedades**y, a continuación, seleccione el **Empaquetar/Publicar Web**ficha.

![Pestaña Empaquetar/publicar web](project-properties/_static/image1.png)

Cuando se muestre la ventana, el valor predeterminado es que muestra los valores para cualquier configuración de compilación está activo actualmente para la solución. Si el **configuración** cuadro no indica **activo (versión)**, seleccione **versión** con el fin de mostrar la configuración para la configuración de compilación de versión. Las compilaciones de versión va a implementar en entornos de la prueba y producción.

![Seleccionar configuración de compilación de versión](project-properties/_static/image2.png)

Con **activo (versión)** o **versión** seleccionado, verá que los valores que están vigentes cuando se implementa mediante la configuración de compilación de versión:

- En el **elementos para implementar** cuadro, **sólo los archivos necesarios para ejecutar la aplicación** está seleccionada. Otras opciones son **todos los archivos de este proyecto** o **todos los archivos de esta carpeta de proyecto**. Al dejar la selección predeterminada sin cambios Evite implementar archivos de código fuente, por ejemplo. Esta configuración es la razón de por qué las carpetas que contienen los archivos binarios de SQL Server Compact tenían que se incluirán en el proyecto. Para obtener más información sobre esta configuración, consulte **¿por qué no todos los archivos en mi carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuración generado de exclusión** está seleccionada. Que no depurando cuando se usa esta configuración de compilación.
- **Incluir todas las bases de datos configuradas en la pestaña Empaquetar/publicar SQL** está seleccionada. Especifica si Visual Studio implementará las bases de datos, así como archivos. Aunque la casilla de verificación de etiqueta únicas menciones el **Empaquetar/publicar SQL** ficha, si desactiva esta casilla de verificación podría también deshabilitar la implementación de la base de datos que está configurado en el perfil de publicación. Va a realizar más adelante, por lo que la casilla de verificación debe mantenerse seleccionada. El **Empaquetar/publicar SQL** pestaña sirve para una base de datos heredado, el método que no va a usar en estos tutoriales de publicación.
- El **configuración del paquete de implementación Web** sección no es aplicable porque usa un solo clic publicar en estos tutoriales.

Cambiar el **configuración** cuadro desplegable de depuración para ver la configuración predeterminada para las compilaciones de depuración. Los valores son iguales, salvo **excluir símbolos de depuración generados** está desactivada de manera que pueda depurar al implementar una compilación de depuración.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Asegúrese de que la carpeta Elmah obtiene implementada

Como se vio en el tutorial anterior, el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona la funcionalidad de registro e informes de errores. En la aplicación Contoso University Elmah se ha configurado para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta ELMAH](project-properties/_static/image3.png)

Excluir determinados archivos o carpetas de implementación es un requisito común; otro ejemplo sería una carpeta que los usuarios pueden cargar archivos a. No desea que los archivos de registro o cargar archivos que se crearon en el entorno de desarrollo para su implementación en producción. Y si va a implementar una actualización en producción, que no desea que el proceso de implementación para eliminar los archivos que existen en producción. (Dependiendo de cómo establecer una opción de implementación, si existe un archivo en el sitio de destino, pero no en el sitio de origen cuando se implementa, Web Deploy lo elimina del destino.)

Como se vio anteriormente en este tutorial, el **elementos para implementar** opción el **Empaquetar/Publicar Web** ficha se establece en **sólo los archivos necesarios para ejecutar esta aplicación**. Como resultado, los archivos de registro creados por Elmah en el desarrollo no se implementará, que es lo que desea que ocurra. (Para implementarse, tendría que se incluirán en el proyecto y sus **acción de compilación** propiedad tendría que establecerse en **contenido**. Para obtener más información, consulte **¿por qué no todos los archivos en mi carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiar en él. Por lo tanto, agregará un *.txt* archivo en la carpeta para que actúe como un marcador de posición para que se va a copiar la carpeta.

En **el Explorador de soluciones**, haga clic en el *Elmah* carpeta, seleccione **Agregar nuevo elemento**y cree un archivo de texto denominado *marcador.txt*. Ponga el texto siguiente: "Es un archivo de marcador de posición para asegurarse de que se implementa la carpeta". y guarde el archivo. Eso es todo lo que debe hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta se encuentra, porque el **acción de compilación** propiedad de *.txt* archivos se establece en **contenido**de forma predeterminada.

## <a name="summary"></a>Resumen

Ahora ha completado todas las tareas de configuración de implementación. En el siguiente tutorial, deberá implementar el sitio de Contoso University en el entorno de prueba y probarla.

> [!div class="step-by-step"]
> [Anterior](web-config-transformations.md)
> [Siguiente](deploying-to-iis.md)
