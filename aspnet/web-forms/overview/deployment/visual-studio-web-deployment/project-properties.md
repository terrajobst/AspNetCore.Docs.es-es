---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Implementación de Web ASP.NET con Visual Studio: propiedades del proyecto | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "30879886"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implementación de Web ASP.NET con Visual Studio: propiedades del proyecto
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Algunas opciones de implementación están configuradas en las propiedades de proyecto que se almacenan en el archivo de proyecto (la *.csproj* o *.vbproj* archivo). En la mayoría de los casos, los valores predeterminados de estas opciones son las que desea, pero puede usar el **propiedades del proyecto** interfaz de usuario integradas en Visual Studio para trabajar con esta configuración si tiene que cambiarlos. En este tutorial revise las opciones de implementación en **propiedades del proyecto**. También creará un archivo de marcador de posición que hace que una carpeta vacía para implementarse.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurar opciones de implementación en la ventana de propiedades de proyecto

Mayoría de las configuraciones que afectan a lo que sucede durante la implementación se incluye en el perfil de publicación, como verá en los tutoriales siguientes. Algunas opciones de configuración que debe ser consciente de que se encuentran en el **Empaquetar/publicar** pestañas de la **propiedades del proyecto** ventana. Estas opciones se especifican para cada configuración de compilación, es decir, puede tener diferentes valores para una versión de lanzamiento que tiene para una compilación de depuración.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto, seleccione **propiedades**y, a continuación, seleccione el **Empaquetar/Publicar Web**ficha.

![Pestaña Empaquetar/publicar web](project-properties/_static/image1.png)

Cuando se muestra la ventana, el valor predeterminado es que muestra la configuración para cualquier configuración de compilación está activa actualmente para la solución. Si el **configuración** cuadro no indica **activo (versión)**, seleccione **versión** para mostrar valores para la configuración de compilación de versión. Va a implementar versiones de lanzamiento a los entornos de la prueba y producción.

![Seleccionar configuración de compilación de lanzamiento](project-properties/_static/image2.png)

Con **activo (versión)** o **versión** seleccionado, verá los valores que están vigentes cuando se implementa mediante la configuración de compilación de lanzamiento:

- En el **elementos que se va a implementar** cuadro, **sólo los archivos necesarios para ejecutar la aplicación** está seleccionada. Otras opciones son **todos los archivos de este proyecto** o **todos los archivos en esta carpeta de proyecto**. Al dejar la selección predeterminada sin cambios no implementar archivos de código fuente, por ejemplo. Este valor es el motivo por qué las carpetas que contienen los archivos binarios de SQL Server Compact se tenían que se incluye en el proyecto. Para obtener más información sobre esta configuración, consulte **¿por qué no todos los archivos en la carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuración de exclusión genera** está seleccionada. Que no depurando cuando se usa esta configuración de compilación.
- **Incluir todas las bases de datos configuradas en la pestaña Empaquetar/publicar SQL** está seleccionada. Especifica si Visual Studio va a implementar las bases de datos, así como archivos. Aunque la casilla de verificación etiquetar solo menciona el **Empaquetar/publicar SQL** ficha, si se desactiva esta casilla también deshabilitaría implementación de base de datos que está configurado en el perfil de publicación. Se va a hacer una versión posterior, por lo que debe permanecer seleccionada la casilla de verificación. El **Empaquetar/publicar SQL** ficha se utiliza para una base de datos heredado método que no va a usar en estos tutoriales de publicación.
- El **configuración del paquete de implementación Web** sección no es aplicable porque lo está utilizando un solo clic publicar en estos tutoriales.

Cambiar el **configuración** cuadro de lista desplegable para la depuración para ver la configuración predeterminada para las compilaciones de depuración. Los valores son iguales, salvo **excluir símbolos de depuración generados** está desactivada para que puedan depurar cuando se implementa una compilación de depuración.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Asegúrese de que se implemente la carpeta Elmah

Como vimos en el tutorial anterior, el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona la funcionalidad de registro e informes de errores. En la aplicación de la Universidad de Contoso Elmah se ha configurado para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta ELMAH](project-properties/_static/image3.png)

Excluir determinados archivos o carpetas de implementación es un requisito común; otro ejemplo sería una carpeta que los usuarios pueden cargar archivos a. No desea que los archivos de registro o cargar archivos que se crearon en el entorno de desarrollo para su implementación en producción. Y si va a implementar una actualización en producción que no desea que el proceso de implementación para eliminar archivos que existen en producción. (Dependiendo de cómo se establece una opción de implementación, si existe un archivo en el sitio de destino pero no en el sitio de origen cuando se implementa, Web Deploy se elimina de destino.)

Como vimos anteriormente en este tutorial, el **elementos que se va a implementar** opción en el **Empaquetar/Publicar Web** ficha está establecida en **sólo los archivos necesarios para ejecutar esta aplicación**. Como resultado, los archivos de registro creados por Elmah en desarrollo no se implementará, que es lo que desea que ocurra. (Para implementarse, tendría que se incluirá en el proyecto y sus **acción de compilación** propiedad tendría que estar establecido en **contenido**. Para obtener más información, consulte **¿por qué no todos los archivos en la carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiar en ella. Por lo tanto, agregará un *.txt* archivo en la carpeta para que actúe como un marcador de posición para que se va a copiar la carpeta.

En **el Explorador de soluciones**, haga clic en el *Elmah* carpeta, seleccione **Agregar nuevo elemento**y cree un archivo de texto denominado *marcador.txt*. Incluir el texto siguiente en él: "Es un archivo de marcador de posición para asegurarse de que se implemente la carpeta". Y guarde el archivo. Eso es todo lo que tiene que hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta se encuentra en, porque el **acción de compilación** propiedad de *.txt* archivos se establece en **contenido**de forma predeterminada.

## <a name="summary"></a>Resumen

Ahora ha completado todas las tareas de configuración de implementación. En el siguiente tutorial, podrá implementar el sitio de la Universidad de Contoso en el entorno de prueba y probarla.

> [!div class="step-by-step"]
> [Anterior](web-config-transformations.md)
> [Siguiente](deploying-to-iis.md)
