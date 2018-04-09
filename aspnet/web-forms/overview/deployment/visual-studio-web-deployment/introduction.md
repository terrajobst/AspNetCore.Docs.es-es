---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Implementación de Web ASP.NET con Visual Studio: Introducción | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web por aplicación para aplicaciones de Web del servicio de aplicación de Azure o un proveedor de hospedaje de terceros mediante V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Implementación de Web ASP.NET con Visual Studio: Introducción
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET por aplicación para aplicaciones de Web del servicio de aplicación de Azure o un proveedor de hospedaje de terceros, web mediante Visual Studio 2012 con el SDK de Azure para. NET. La mayoría de los procedimientos es similar para Visual Studio 2013.
> 
> Desarrollar una aplicación web para que esté disponible para las personas a través de Internet. Pero tutoriales de programación web normalmente se detiene después de que ha mostrado cómo obtener algo a funcionar en el equipo de desarrollo. Esta serie de tutoriales comienza donde los demás dejan: se ha creado una aplicación web, probado, y está listo para funcionar. Pasos adicionales Estos tutoriales muestra cómo implementar primero a IIS en el equipo de desarrollo local para las pruebas y, a continuación, en Azure o en un proveedor de hospedaje de terceros para ensayo y producción. La aplicación de ejemplo que se va a implementar es un proyecto de aplicación web que utiliza Entity Framework, SQL Server y el sistema de pertenencia ASP.NET. La aplicación de ejemplo usa ASP.NET Web Forms, pero los procedimientos mostrados se aplican también a ASP.NET MVC y API Web.
> 
> Estos tutoriales se supone que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, es un buen lugar para comenzar una [Tutorial básico de formularios de ASP.NET Web](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o un [Tutorial básico de MVC de ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de implementación de ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) o [StackOverflow](http://stackoverflow.com).
> 
> Este contenido también está disponible como un libro electrónico gratuito en [la Galería de TechNet de libros electrónicos](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Información general

Estos tutoriales lo guían a través de implementar una aplicación web ASP.NET que incluye las bases de datos de SQL Server. Va a implementar en primer lugar a IIS en el equipo de desarrollo local para las pruebas y, a continuación, para las aplicaciones Web en el servicio de aplicaciones de Azure y base de datos de SQL de Azure para ensayo y producción. Podrá ver cómo implementar con Visual Studio publicar un solo clic, y verá cómo implementar mediante la línea de comandos.

El número de tutoriales podría provocar que el proceso de implementación parece desalentadora. De hecho, los procedimientos básicos son simples. Sin embargo, en situaciones del mundo real, a menudo necesitará realizar tareas de implementación adicionales, por ejemplo, establecer los permisos de carpetas en el servidor de destino. Nos hemos mostrado algunas de estas tareas adicionales, con la esperanza de que los tutoriales no deje información que podría impedirle que implementar correctamente una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia, y cada parte se basa en la parte anterior. Puede omitir los elementos que no sean relevantes para su situación, pero, a continuación, es posible que deba ajustar los procedimientos en tutoriales posteriores.

## <a name="intended-audience"></a>Destinatarios

Los tutoriales están dirigidos a los desarrolladores ASP.NET que trabajan en entornos donde:

- El entorno de producción es un proveedor de hospedaje de terceros o de aplicaciones de Web del servicio de aplicación de Azure.
- Implementación no se limita a un proceso de integración continua, pero se efectuará directamente desde Visual Studio.

Implementación de [control de código fuente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) con un [la entrega continua](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) proceso no se aborda en estos tutoriales excepto un tutorial que muestra cómo implementar la línea de comandos. Para obtener información sobre la entrega continua, vea los siguientes recursos:

- [Integración continua y la entrega continua (creación de aplicaciones de nube reales con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Implementar una aplicación web en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un conjunto anterior de tutoriales escritos para Visual Studio 2010, que todavía tiene información útil para entornos empresariales).

## <a name="using-a-third-party-hosting-provider"></a>Uso de un proveedor de hospedaje de terceros

Los tutoriales le conducen por el proceso de configurar una cuenta de Azure e implementar la aplicación para las aplicaciones Web en el servicio de aplicación de Azure para ensayo y producción. Sin embargo, puede usar los mismos procedimientos básicos para la implementación en un proveedor de hospedaje de terceros de su elección. Donde los tutoriales sobrepasar procesos únicos en Azure, se explica y aparezca un mensaje indicando qué diferencias que se pueden esperar en un proveedor de hospedaje de terceros.

## <a name="deploying-web-app-projects"></a>Implementación de proyectos de aplicación web

La aplicación de ejemplo que descargar e implementar para estos tutoriales es un proyecto de aplicación web de Visual Studio. Sin embargo, si instala la versión más reciente [la actualización de publicación Web para Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), puede usar los mismos métodos de implementación y herramientas para los proyectos de aplicación web.

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

La aplicación de ejemplo es un proyecto de formularios Web Forms de ASP.NET, pero todo lo que obtenga información acerca de cómo llevar a cabo también es aplicable a ASP.NET MVC. Un proyecto de MVC de Visual Studio es simplemente otro tipo de proyecto de aplicación web. La única diferencia es que si va a implementar en un proveedor de hospedaje que no es compatible con ASP.NET MVC o la versión de destino del mismo, debe asegurarse de que ha instalado la correspondiente ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) o [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) paquete de NuGet en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo usa C#, pero los tutoriales no requieren conocimientos de C# y las técnicas de implementación que se muestra en los tutoriales no son específicos del idioma.

## <a name="database-deployment-methods"></a>Métodos de implementación de base de datos

Hay tres maneras de implementar una base de datos de SQL Server junto con la implementación de web en Visual Studio:

- Migraciones de Entity Framework Code First
- El proveedor dbDacFx Web Deploy
- El proveedor de Web Deploy dbFullSql

En este tutorial utilizará las dos primeras de estos métodos. El proveedor de Web Deploy dbFullSql es un método heredado que ya no se recomienda excepto para algunos escenarios como la migración de SQL Server Compact a SQL Server.

Los métodos que se muestra en este tutorial son bases de datos en SQL Server, no SQL Server Compact. Para obtener información sobre cómo implementar una base de datos de SQL Server Compact, vea [Visual Studio Web implementación con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Los métodos que se muestra en este tutorial requieren el uso de la Web Deploy método de publicación. Si prefiere publicar otro método, como FTP, sistema de archivos o FPSE, consulte [implementar una base de datos por separado de la implementación de aplicaciones web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) en el mapa de contenido de implementación Web para Visual Studio y ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migraciones de Entity Framework Code First

En la versión 4.3 de Entity Framework, Microsoft introdujo migraciones de Code First. Migraciones de Code First automatiza el proceso de realizar los cambios incrementales en un modelo de datos y propagar los cambios a la base de datos. En versiones anteriores de Code First, normalmente permiten Entity Framework quitar y volver a crear la base de datos cada vez que cambie el modelo de datos. Esto no es un problema en el desarrollo porque están volver a crear fácilmente datos de prueba, pero en producción normalmente es conveniente actualizar el esquema de base de datos sin quitar la base de datos. La característica de migraciones permite Code First actualizar la base de datos sin quitar y volver a crearla. Puede dejar que Code First decidir automáticamente cómo realizar los cambios necesarios en el esquema, o puede escribir código que personaliza los cambios. Para obtener una introducción a migraciones de Code First, vea [migraciones de Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Al implementar un proyecto web, Visual Studio puede automatizar el proceso de implementación de una base de datos que se administra mediante migraciones de Code First. Cuando se crea el perfil de publicación, seleccione una casilla de verificación con la etiqueta ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación). Esta configuración hace que el proceso de implementación configurar automáticamente el archivo Web.config de aplicación en el servidor de destino para que utilice Code First la `MigrateDatabaseToLatestVersion` clase inicializador.

Visual Studio no hace nada con la base de datos durante el proceso de implementación. Cuando la aplicación implementada tiene acceso a la base de datos por primera vez después de la implementación, Code First creará automáticamente la base de datos o actualiza el esquema de base de datos a la versión más reciente. Si la aplicación implementa un método de inicialización de las migraciones, el método se ejecuta después de la base de datos se crea o se actualiza el esquema.

En este tutorial, deberá usar migraciones de Code First para implementar la base de datos de aplicación.

### <a name="the-dbdacfx-web-deploy-provider"></a>El proveedor dbDacFx Web Deploy

Para una base de datos de SQL Server no está administrado por Entity Framework Code First, puede seleccionar una casilla de verificación con la etiqueta Actualizar base de datos cuando se configura el perfil de publicación. Durante la implementación inicial, el proveedor dbDacFx crea tablas y otros objetos de base de datos en la base de datos de destino para que coincida con la base de datos de origen. En las implementaciones posteriores, el proveedor determina lo que es diferente entre las bases de datos de origen y de destino y actualiza el esquema de la base de datos de destino para que coincida con la base de datos de origen. De forma predeterminada, el proveedor no realice los cambios que provocan la pérdida de datos, por ejemplo, cuando se quita una tabla o columna.

Este método automatizar la implementación de datos de tablas de base de datos, pero puede crear scripts para hacerlo y configurar Visual Studio para ejecutarlas durante la implementación. Es otra razón para ejecutar scripts durante la implementación realizar cambios de esquema que no se puede realizar automáticamente porque podría causar pérdida de datos.

En este tutorial, usará el proveedor dbDacFx para implementar la base de datos de pertenencia ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución obvia. Para ayudarle con algunos escenarios comunes de problema, un [página de referencia de la solución de problemas](troubleshooting.md) está disponible. Si aparece un mensaje de error o algo no funciona tal y como se vaya a través de los tutoriales, asegúrese de comprobar la página de solución de problemas.

## <a name="comments-welcome"></a>Página principal de comentarios

Comentarios en los tutoriales son bienvenidos y, cuando se actualiza el tutorial se realizará todo lo posible para tener en cuenta correcciones o sugerencias para mejoras en el que se proporcionan en los comentarios de tutorial.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Este tutorial se escribió para los siguientes productos:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio 2012 Express for Web con [la última actualización](https://go.microsoft.com/fwlink/?LinkId=272486).
- [SDK de Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Puede seguir el tutorial mediante Visual Studio 2010 SP1 o Visual Studio 2013, pero algunas capturas de pantalla son diferentes y algunas características será diferentes.

Si está usando Visual Studio 2013, instale [Azure SDK para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Si usa Visual Studio 2010 SP1, instale el software siguiente:

- [SDK de Azure para Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [Herramientas de datos SQL Server](https://msdn.microsoft.com/library/hh500335.aspx).

Dependiendo de cuántas de las dependencias del SDK ya tiene en su equipo, instalar el SDK de Azure puede tardar mucho tiempo, desde unos minutos a una media hora o más. Se necesita el SDK de Azure incluso si tiene previsto publicar en un proveedor de hospedaje de terceros en lugar de en Azure, dado que el SDK incluye las actualizaciones más recientes en web de Visual Studio publicar características.

> [!NOTE]
> Este tutorial se escribió con la versión 1.8.1 del SDK de Azure. Desde entonces se han publicado las versiones más recientes con características adicionales. Los tutoriales se han actualizado para que se mencionan estas características y un vínculo a los recursos que tienen más información sobre ellos.


Las instrucciones y las capturas de pantalla se basan en Windows 8, pero los tutoriales explican las diferencias para Windows 7.

Otro software es necesario para completar el tutorial, pero no es necesario tenerlo instalado aún. El tutorial le guiará por los pasos para instalarlo cuando lo necesite.

## <a name="download-the-sample-application"></a>Descargar la aplicación de ejemplo

La aplicación que va a implementar se denomina Contoso universidad y ya se ha creado para usted. Es una versión simplificada de un sitio web de la universidad, flexible basado en la aplicación de la Universidad de Contoso se describe en el [tutoriales de Entity Framework en el sitio ASP.NET](https://asp.net/entity-framework/tutorials).

Si tiene los requisitos previos instalados, descargue el [aplicación web de Contoso Universidad](https://go.microsoft.com/fwlink/p/?LinkId=282627). El *.zip* archivo contiene varias versiones del proyecto. Para trabajar con los pasos del tutorial, comience con el proyecto que se encuentra en la carpeta de C#. Para ver el aspecto que tiene el proyecto al final de los tutoriales, abra el proyecto en la carpeta ContosoUniversity-End.

Para preparar el proyecto para trabajar a través de los pasos de tutorial, realice los pasos siguientes:

1. Guarde los archivos de solución de ContosoUniversity desde la carpeta de C# en una carpeta denominada ContosoUniversity en cualquier carpeta que se puede usar para trabajar con proyectos de Visual Studio.

    De forma predeterminada, ésta es la carpeta siguiente para Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Para las capturas de pantalla en este tutorial, la carpeta del proyecto se encuentra en el directorio raíz en el `C`: unidad.)
2. Inicie Visual Studio y abra el proyecto.
3. En **el Explorador de soluciones**, haga clic en la solución y haga clic en **EnableNuGet la restauración del paquete**.
4. Compile la solución.
5. Si se producen errores de compilación, restaure manualmente los paquetes de NuGet:

    1. En **el Explorador de soluciones**, haga clic en la solución y, a continuación, haga clic en **administrar paquetes de NuGet para la solución**.
    2. En la parte superior de la **administrar paquetes de NuGet** , verá el cuadro de diálogo **faltan paquetes de NuGet algunas en esta solución. Haga clic en restaurar.** Haga clic en el **restaurar** botón.
    3. Recompilar la solución.
6. Presione CTRL-F5 para ejecutar la aplicación.

    La aplicación se abre en la página de inicio de la Universidad de Contoso.

    ![Desarrollo de la página principal](introduction/_static/image1.png)

    (Puede haber un tiempo de espera mientras Visual Studio inicia la instancia de SQL Server Express LocalDB y podría producirse un error de tiempo de espera si proceso tarda demasiado tiempo. En ese caso simplemente inicie el proyecto nuevo.)

Páginas Web sean accesibles desde la barra de menús y le permiten realizar las siguientes funciones:

- Mostrar las estadísticas de estudiante (la página acerca de).
- Mostrar, editar, eliminar y agregar los alumnos.
- Mostrar y editar cursos.
- Mostrar y editar instructores.
- Mostrar y editar los departamentos.

Siguientes son capturas de pantalla de unas pocas páginas representativas.

![Desarrollo de la página de estudiantes](introduction/_static/image2.png)

![Agregar el desarrollo de la página de estudiantes](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Revise las características de la aplicación que afectan a la implementación

Las siguientes características de la aplicación afecta a cómo implementarlo o lo que tendrá que hacer para implementarlo. Cada uno de ellos se explica con más detalle en los siguientes tutoriales de la serie.

- Universidad de Contoso usa una base de datos de SQL Server para almacenar datos de aplicación, como nombres de student y instructor. La base de datos contiene una mezcla de datos de prueba y los datos de producción y, cuando se implementa en producción que precise excluir los datos de prueba.
- La aplicación utiliza el sistema de pertenencia ASP.NET, que almacena información de la cuenta de usuario en una base de datos de SQL Server. La aplicación define un usuario de administrador que tenga acceso a información restringida. Debe implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- La aplicación utiliza un error de terceros, registros e informes de utilidad. Esta utilidad se proporciona en un ensamblado que debe implementarse con la aplicación.
- La utilidad de registro de error escribe información de error en archivos XML en una carpeta de archivos. Tiene que asegurarse de que la cuenta que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta, y se debe excluir esta carpeta de implementación. (En caso contrario, los datos de registro de error desde el entorno de prueba podrían implementarse en producción o se podrían eliminar los archivos de registro de errores de producción).
- La aplicación incluye algunos valores de configuración que se deben cambiar en implementado *Web.config* archivo según el entorno de destino (prueba, ensayo o producción) y otras opciones que se deben cambiar dependiendo de la compilación configuración (Debug o Release).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Se debe implementar únicamente el ensamblado que genera este proyecto, no el propio proyecto.

## <a name="summary"></a>Resumen

En este primer tutorial de la serie, ha descargado el proyecto de Visual Studio de ejemplo y revisar las características de sitio que afectan a cómo implementar la aplicación. En los siguientes tutoriales para preparar para la implementación mediante la configuración de algunas de estas cosas administrará automáticamente. Otros que se ocupa de forma manual.

> [!div class="step-by-step"]
> [Siguiente](preparing-databases.md)
