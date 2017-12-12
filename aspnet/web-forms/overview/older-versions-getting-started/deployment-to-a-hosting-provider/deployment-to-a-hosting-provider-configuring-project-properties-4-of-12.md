---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de propiedades del proyecto: 4 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2ba202a1a0d0ba752576e8906b739cc9e83fde2a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de propiedades del proyecto: 4 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Algunas opciones de implementación están configuradas en las propiedades de proyecto que se almacenan en el archivo de proyecto (la *.csproj* o *.vbproj* archivo). En la mayoría de los casos, los valores predeterminados de estas opciones son las que desea, pero puede usar el **propiedades del proyecto** interfaz de usuario integradas en Visual Studio para trabajar con esta configuración si tiene que cambiarlos. En este tutorial revise las opciones de implementación en **propiedades del proyecto**. También creará un archivo de marcador de posición que hace que una carpeta vacía para implementarse.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configuración de la implementación en la ventana de propiedades de proyecto

Mayoría de las configuraciones que afectan a lo que sucede durante la implementación se incluye en el perfil de publicación, como verá en los tutoriales siguientes. Algunas opciones de configuración que debe ser consciente de que se encuentran en el **Empaquetar/publicar** pestañas de la **propiedades del proyecto** ventana. Estas opciones se especifican para cada configuración de compilación, es decir, puede tener diferentes valores para una versión de lanzamiento que tiene para una compilación de depuración.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto, seleccione **propiedades**y, a continuación, seleccione el **Empaquetar/Publicar Web**ficha.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Cuando se muestra la ventana, el valor predeterminado es que muestra la configuración para cualquier configuración de compilación está activa actualmente para la solución. Si el **configuración** cuadro no indica **activo (versión)**, seleccione **versión** para mostrar valores para la configuración de compilación de versión. Va a implementar versiones de lanzamiento a los entornos de la prueba y producción.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con **activo (versión)** o **versión** seleccionado, verá los valores que están vigentes cuando se implementa mediante la configuración de compilación de lanzamiento:

- En el **elementos que se va a implementar** cuadro, **sólo los archivos necesarios para ejecutar la aplicación** está seleccionada. Otras opciones son **todos los archivos de este proyecto** o **todos los archivos en esta carpeta de proyecto**. Al dejar la selección predeterminada sin cambios no implementar archivos de código fuente, por ejemplo. Este valor es el motivo por qué las carpetas que contienen los archivos binarios de SQL Server Compact se tenían que se incluye en el proyecto. Para obtener más información sobre esta configuración, consulte **¿por qué no todos los archivos en la carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/en-us/library/ee942158.aspx).
- **Símbolos de depuración de exclusión genera** está seleccionada. Que no depurando cuando se usa esta configuración de compilación.
- **Excluir archivos de la aplicación\_carpeta de datos** no está seleccionada. El archivo de SQL Server Compact para la base de datos de pertenencia está en esa carpeta y tiene que implementarlo. Al implementar las actualizaciones que no incluyen cambios de base de datos, seleccionará esta casilla de verificación.
- **Precompilar esta aplicación antes de la publicación** no está seleccionada. En la mayoría de los escenarios, no hay ninguna necesidad para precompilar los proyectos de aplicación web. Para obtener más información acerca de esta opción, vea [pestaña Empaquetar/Publicar Web, propiedades del proyecto](https://msdn.microsoft.com/en-us/library/dd410108(v=vs.110).aspx) y [avanzada precompilar cuadro de diálogo Configuración](https://msdn.microsoft.com/en-us/library/hh475319(v=vs.110).aspx).
- **Incluir todas las bases de datos configuradas en la pestaña Empaquetar/publicar SQL** está seleccionada, pero esta opción no tiene ningún efecto ahora debido a que no estén configurando la **Empaquetar/publicar SQL** ficha. Esa pestaña es un método de implementación de bases de datos heredadas que solía ser la única opción para implementar bases de datos de SQL Server. Usará el **Empaquetar/publicar SQL** pestaña en el [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial.
- El **configuración del paquete de implementación Web** sección no es aplicable porque lo está utilizando un solo clic publicar en estos tutoriales.

Cambiar el **configuración** cuadro de lista desplegable para la depuración para ver la configuración predeterminada para las compilaciones de depuración. Los valores son iguales, salvo **excluir símbolos de depuración generados** está desactivada para que puedan depurar cuando se implementa una compilación de depuración.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Realizar seguro que se implemente la carpeta Elmah

Como vimos en el tutorial anterior, el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona la funcionalidad de registro e informes de errores. En la aplicación de la Universidad de Contoso Elmah se ha configurado para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluir determinados archivos o carpetas de implementación es un requisito común; otro ejemplo sería una carpeta que los usuarios pueden cargar archivos a. No desea que los archivos de registro o cargar archivos que se crearon en el entorno de desarrollo para su implementación en producción. Y si va a implementar una actualización en producción que no desea que el proceso de implementación para eliminar archivos que existen en producción. (Dependiendo de cómo se establece una opción de implementación, si existe un archivo en el sitio de destino pero no en el sitio de origen cuando se implementa, Web Deploy se elimina de destino.)

Como vimos anteriormente en este tutorial, el **elementos que se va a implementar** opción en el **Empaquetar/Publicar Web** ficha está establecida en **sólo los archivos necesarios para ejecutar esta aplicación**. Como resultado, los archivos de registro creados por Elmah en desarrollo no se implementará, que es lo que desea que ocurra. (Para implementarse, tendría que se incluirá en el proyecto y sus **acción de compilación** propiedad tendría que estar establecido en **contenido**. Para obtener más información, consulte **¿por qué no todos los archivos en la carpeta de proyecto se implementa?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/en-us/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiar en ella. Por lo tanto, agregará un *.txt* archivo en la carpeta para que actúe como un marcador de posición para que se va a copiar la carpeta.

En **el Explorador de soluciones**, haga clic en el *Elmah* carpeta, seleccione **Agregar nuevo elemento**y cree un archivo denominado *marcador.txt*. Incluir el texto siguiente en él: "Es un archivo de marcador de posición para asegurarse de que se implemente la carpeta". Y guarde el archivo. Eso es todo lo que tiene que hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta se encuentra en, porque el **acción de compilación** propiedad de *.txt* archivos se establece en **contenido**de forma predeterminada.

Ahora ha completado todas las tareas de configuración de implementación. En el siguiente tutorial, podrá implementar el sitio de la Universidad de Contoso en el entorno de prueba y probarla.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
