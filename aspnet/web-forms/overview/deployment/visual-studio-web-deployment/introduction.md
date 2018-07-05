---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Implementación Web de ASP.NET con Visual Studio: Introducción | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar), ASP.NET web por aplicación en Azure App Service Web Apps o un proveedor de hospedaje de terceros con V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: ad06639db9c5de78fb9926c7ab00e348bcbb20cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382563"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Implementación Web de ASP.NET con Visual Studio: Introducción
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar), ASP.NET web por aplicación en Azure App Service Web Apps o un proveedor de hospedaje de terceros, mediante Visual Studio 2012 con el SDK de Azure para. NET. La mayoría de los procedimientos es similar para Visual Studio 2013.
> 
> Desarrollar una aplicación web con el fin de que esté disponible para las personas a través de Internet. Pero tutoriales de programación web normalmente se detiene justo después de que ha mostrado cómo obtener algo trabajando en el equipo de desarrollo. Esta serie de tutoriales comienza donde los demás omita: ha creado una aplicación web, probó, y está listo para ir. Pasos adicionales Estos tutoriales muestra cómo implementar primero a IIS en el equipo de desarrollo local para pruebas y, a continuación, en Azure o un proveedor de hospedaje de terceros para ensayo y producción. La aplicación de ejemplo que se va a implementar es un proyecto de aplicación web que usa Entity Framework, SQL Server y el sistema de pertenencia ASP.NET. La aplicación de ejemplo usa ASP.NET Web Forms, pero los procedimientos mostrados se aplican también a ASP.NET MVC y Web API.
> 
> Estos tutoriales se suponen que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, es un buen lugar para comenzar un [Tutorial básico de formularios de ASP.NET Web](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o un [Tutorial básico de ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de implementación de ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) o [StackOverflow](http://stackoverflow.com).
> 
> Este contenido también está disponible como un libro electrónico gratuito en [la Galería de TechNet de E-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Información general

Estos tutoriales le ayudarán a implementar una aplicación web ASP.NET que incluye las bases de datos de SQL Server. En primer lugar a implementar en IIS en el equipo de desarrollo local para pruebas y, a continuación, en Web Apps en Azure App Service y Azure SQL Database para ensayo y producción. Verá cómo implementar con Visual Studio publicar con un solo clic, y verá cómo se implementa mediante la línea de comandos.

La serie de tutoriales es posible que facilitan el proceso de implementación parece desalentadora. De hecho, los procedimientos básicos son simples. Sin embargo, en situaciones del mundo real, a menudo necesitará realizar tareas de implementación adicionales, por ejemplo, establecer permisos de carpeta en el servidor de destino. Nos hemos mostrado algunas de estas tareas adicionales, con la esperanza de que no omita los tutoriales de información que podría impedirle implementar correctamente una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia, y cada parte se basa en la parte anterior. Puede omitir las partes que no son pertinentes para su situación, pero, a continuación, es posible que deba ajustar los procedimientos en los tutoriales posteriores.

## <a name="intended-audience"></a>Público destinatario

Los tutoriales están dirigidos a los desarrolladores de ASP.NET que trabajan en entornos donde:

- El entorno de producción es un proveedor de hospedaje de terceros o de Azure App Service Web Apps.
- Implementación no se limita a un proceso de integración continua, pero puede realizarse directamente desde Visual Studio.

Implementación de [control de código fuente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) mediante un [la entrega continua](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) proceso no se trata en estos tutoriales, excepto un tutorial que muestra cómo implementar la línea de comandos. Para obtener información sobre la entrega continua, consulte los siguientes recursos:

- [La integración continua y entrega continua (crear aplicaciones de nube del mundo Real con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Implementar una aplicación web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Implementación de aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un conjunto anterior de tutoriales escrito para Visual Studio 2010, que aún tiene información útil para los entornos empresariales).

## <a name="using-a-third-party-hosting-provider"></a>Uso de un proveedor de hospedaje de terceros

Los tutoriales le llevan por el proceso de configuración de una cuenta de Azure e implementar la aplicación en Web Apps en Azure App Service para ensayo y producción. Sin embargo, puede usar los mismos procedimientos básicos para la implementación en un proveedor de hospedaje de terceros de su elección. Donde los tutoriales van por procesos exclusivos en Azure, se explica y dar a conocer a qué diferencias pueden esperar en un proveedor de hospedaje de terceros.

## <a name="deploying-web-app-projects"></a>Implementación de proyectos de aplicación web

La aplicación de ejemplo que descargar e implementar estos tutoriales es un proyecto de aplicación web de Visual Studio. Sin embargo, si instala la versión más reciente [actualización de publicación Web para Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), puede usar los mismos métodos de implementación y herramientas para proyectos de aplicación web.

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

La aplicación de ejemplo es un proyecto de formularios Web Forms de ASP.NET, pero todo lo que aprenda a realizar también es aplicable a ASP.NET MVC. Un proyecto de MVC de Visual Studio es simplemente otra forma de proyecto de aplicación web. La única diferencia es que si va a implementar en un proveedor de hospedaje que no es compatible con ASP.NET MVC o la versión de destino del mismo, debe asegurarse de que ha instalado adecuado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) o [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) paquete de NuGet en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo usa C#, pero los tutoriales no requieren conocimientos de C# y las técnicas de implementación que se muestra en los tutoriales no son específicas del idioma.

## <a name="database-deployment-methods"></a>Métodos de implementación de la base de datos

Hay tres formas que puede implementar una base de datos de SQL Server junto con la implementación web en Visual Studio:

- Migraciones de Entity Framework Code First
- El proveedor de Web Deploy dbDacFx
- El proveedor de Web Deploy dbFullSql

En este tutorial usará los dos primeros de estos métodos. El proveedor de Web Deploy dbFullSql es un método heredado que ya no se recomienda excepto para algunos escenarios específicos como la migración de SQL Server Compact a SQL Server.

Los métodos que se muestra en este tutorial son para bases de datos de SQL Server, no SQL Server Compact. Para obtener información sobre cómo implementar una base de datos de SQL Server Compact, vea [Visual Studio Web Deployment con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Los métodos que se muestran en este tutorial requieren el uso de la Web Deploy método de publicación. Si prefiere publicar otro método, como FTP, sistema de archivos o FPSE, consulte [implementar una base de datos por separado de la implementación de aplicaciones web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) en el mapa de contenido de implementación Web para Visual Studio y ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migraciones de Entity Framework Code First

En la versión 4.3 de Entity Framework, Microsoft presentó migraciones de Code First. Migraciones de Code First automatiza el proceso de realización de cambios incrementales en un modelo de datos y propagar los cambios a la base de datos. En versiones anteriores de Code First, normalmente permiten Entity Framework quite y vuelva a crear la base de datos cada vez que cambie el modelo de datos. Esto no es un problema en el desarrollo porque están volver a crear fácilmente datos de prueba, pero en producción normalmente desea actualizar el esquema de base de datos sin quitar la base de datos. Permite que la característica migraciones de Code First actualizar la base de datos sin quitar y volver a crearla. Puede permitir que Code First decidir automáticamente cómo realizar los cambios necesarios en el esquema, o puede escribir código que personaliza los cambios. Para obtener una introducción a las migraciones de Code First, consulte [migraciones de Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Al implementar un proyecto web, Visual Studio puede automatizar el proceso de implementación de una base de datos que se administra mediante migraciones de Code First. Cuando se crea el perfil de publicación, seleccione una casilla de verificación con la etiqueta ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación). Esta configuración hace que el proceso de implementación configurar automáticamente el archivo Web.config de aplicación en el servidor de destino para que utilice Code First la `MigrateDatabaseToLatestVersion` clase de inicializador.

Visual Studio no hace nada con la base de datos durante el proceso de implementación. Cuando la aplicación implementada se accede a la base de datos por primera vez después de la implementación, Code First automáticamente la base de datos se crea o actualiza el esquema de base de datos a la versión más reciente. Si la aplicación implementa un método de inicialización de las migraciones, el método se ejecuta después de la base de datos se crea o actualiza el esquema.

En este tutorial, usará migraciones de Code First para implementar la base de datos de aplicación.

### <a name="the-dbdacfx-web-deploy-provider"></a>El proveedor de Web Deploy dbDacFx

Para una base de datos de SQL Server no está administrado por Entity Framework Code First, puede seleccionar una casilla de verificación con la etiqueta Actualizar base de datos cuando se configura el perfil de publicación. Durante la implementación inicial, el proveedor dbDacFx crea las tablas y otros objetos de base de datos en la base de datos de destino para que coincida con la base de datos de origen. En las implementaciones posteriores, el proveedor determina cuál es la diferencia entre las bases de datos de origen y destino y actualiza el esquema de la base de datos de destino para que coincida con la base de datos de origen. De forma predeterminada, el proveedor no realice los cambios que provocan la pérdida de datos, por ejemplo, cuando se quita una tabla o columna.

Este método no automatiza la implementación de datos de tablas de base de datos, pero puede crear secuencias de comandos para hacer eso y configurar Visual Studio para ejecutarlas durante la implementación. Otro motivo para ejecutar scripts durante la implementación es para realizar cambios de esquema que no se realizara automáticamente porque podría causar pérdida de datos.

En este tutorial, usará el proveedor dbDacFx para implementar la base de datos de pertenencia ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución obvia. Para ayudarle con algunos escenarios problemáticos comunes, un [página de referencia de la solución de problemas](troubleshooting.md) está disponible. Si recibe un mensaje de error o algo no funciona al avanzar por los tutoriales, asegúrese de comprobar la página de solución de problemas.

## <a name="comments-welcome"></a>Le damos la bienvenida de comentarios

Comentarios en los tutoriales son bienvenidos y, cuando se actualiza el tutorial se realizarán todos los esfuerzos para tener en cuenta correcciones o sugerencias de mejoras que se proporcionan en los comentarios de tutorial.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se escribió para los siguientes productos:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio 2012 Express para Web con [la actualización más reciente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Puede seguir el tutorial con Visual Studio 2010 SP1 o Visual Studio 2013, pero algunas capturas de pantalla serán diferentes y algunas características serán diferentes.

Si usa Visual Studio 2013, instale [Azure SDK para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Si usa Visual Studio 2010 SP1, instale el software siguiente:

- [Azure SDK para Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [Herramientas de datos SQL Server](https://msdn.microsoft.com/library/hh500335.aspx).

Dependiendo de cuántas de las dependencias SDK ya tiene en su equipo, la instalación del SDK de Azure podría tardar mucho tiempo, desde unos minutos a media hora o más. Necesita el SDK de Azure incluso si va a publicar en un proveedor de hospedaje de terceros en lugar de en Azure, las características de publicación porque el SDK incluye las actualizaciones más recientes en la web de Visual Studio.

> [!NOTE]
> En este tutorial se escribió con la versión 1.8.1 del SDK de Azure. Desde entonces se han publicado las versiones más recientes con características adicionales. Los tutoriales se actualizaron para mencionar a estas características y un vínculo a los recursos que tienen más información sobre ellos.


Las instrucciones y capturas de pantalla se basan en Windows 8, pero los tutoriales explican las diferencias para Windows 7.

Algunos otros programas de software es necesario para completar el tutorial, pero no debe tenerlo instalado aún. El tutorial le guiará por los pasos para instalarlo cuando lo necesite.

## <a name="download-the-sample-application"></a>Descargue la aplicación de ejemplo

La aplicación que va a implementar se denomina Contoso University y ya se ha creado para usted. Es una versión simplificada de un sitio web de la universidad, según se describe en la aplicación Contoso University libremente el [tutoriales de Entity Framework en el sitio ASP.NET](https://asp.net/entity-framework/tutorials).

Una vez instalados los requisitos previos, descargue el [aplicación web Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). El *.zip* archivo contiene varias versiones del proyecto. Para trabajar con los pasos del tutorial, comience con el proyecto que se encuentra en la carpeta de C#. Para ver el aspecto que tiene el proyecto al final de los tutoriales, abra el proyecto en la carpeta ContosoUniversity-End.

Para preparar el proyecto para trabajar con los pasos del tutorial, realice los pasos siguientes:

1. Guarde los archivos de solución ContosoUniversity desde la carpeta de C# en una carpeta denominada ContosoUniversity en cualquier carpeta que se puede usar para trabajar con proyectos de Visual Studio.

    De forma predeterminada, esta es la carpeta siguiente para Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Para las capturas de pantalla en este tutorial, la carpeta del proyecto se encuentra en el directorio raíz en el `C`: unidad.)
2. Inicie Visual Studio y abra el proyecto.
3. En **el Explorador de soluciones**, haga clic en la solución y haga clic en **EnableNuGet la restauración del paquete**.
4. Compile la solución.
5. Si recibe errores de compilación, restaurar manualmente los paquetes de NuGet:

    1. En **el Explorador de soluciones**, haga clic en la solución y, a continuación, haga clic en **administrar paquetes de NuGet para la solución**.
    2. En la parte superior de la **administrar paquetes de NuGet** , verá el cuadro de diálogo **faltan paquetes de NuGet algunos en esta solución. Haga clic para restaurar.** Haga clic en el **restaurar** botón.
    3. Recompilar la solución.
6. Presione CTRL-F5 para ejecutar la aplicación.

    La aplicación se abre en la página principal de Contoso University.

    ![Desarrollo de la página principal](introduction/_static/image1.png)

    (Podría haber un tiempo de espera mientras Visual Studio se inicia la instancia de SQL Server Express LocalDB y podría producirse un error de tiempo de espera si proceso tarda demasiado tiempo. En ese caso simplemente inicie el proyecto nuevo.)

Las páginas del sitio Web son accesibles desde la barra de menús y le permiten realizar las siguientes funciones:

- Mostrar las estadísticas de alumno (la página About).
- Mostrar, editar, eliminar y agregar a los alumnos.
- Mostrar y editar los cursos.
- Visualización y edición de instructores.
- Mostrar y editar los departamentos de TI.

A continuación se muestran capturas de pantalla de unas pocas páginas representativas.

![Desarrollo de la página de alumnos](introduction/_static/image2.png)

![Agregar el desarrollo de la página de alumnos](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Revise las características de la aplicación que afectan a la implementación

Las siguientes características de la aplicación afecta a cómo implementarlo o lo que tiene que hacer para implementarlo. Cada uno de ellos se explica con más detalle en los siguientes tutoriales de la serie.

- Contoso University se usa una base de datos de SQL Server para almacenar datos de la aplicación, como nombres de student e instructor. La base de datos contiene una combinación de datos de prueba y datos de producción y, cuando se implementa en producción debe excluir los datos de prueba.
- La aplicación utiliza el sistema de pertenencia ASP.NET, que almacena información de cuenta de usuario en una base de datos de SQL Server. La aplicación define un usuario de administrador que tenga acceso a información restringida. Deberá implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- La aplicación utiliza un error de terceros, registros e informes de utilidad. Esta utilidad se proporciona en un ensamblado que debe implementarse con la aplicación.
- La utilidad de registro de errores escribe información de error en los archivos XML en una carpeta de archivos. Tiene que asegurarse de que la cuenta que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta, y debe excluir de esta carpeta de implementación. (En caso contrario, los datos de registro de error desde el entorno de prueba podrían estar implementados en producción o es posible que se puede eliminar archivos de registro de errores de producción).
- La aplicación incluye algunas opciones de configuración que se deben cambiar en implementado *Web.config* archivo según el entorno de destino (pruebas, ensayo o producción) y otras configuraciones que deben cambiarse según la compilación configuración (Debug o Release).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Se debe implementar únicamente el ensamblado que genera este proyecto, no el propio proyecto.

## <a name="summary"></a>Resumen

En este primer tutorial de la serie, ha descargado el proyecto de Visual Studio de ejemplo y revisar las características de sitio que afectan a cómo implementar la aplicación. En los tutoriales siguientes, te preparas para la implementación mediante la configuración de algunas de estas cosas se administran automáticamente. Otros que se encarga de forma manual.

> [!div class="step-by-step"]
> [Siguiente](preparing-databases.md)
