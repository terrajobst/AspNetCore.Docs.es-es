---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción - 1 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7449f16367b1eddd7ed66544792129d2a3bc9627
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806075"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción - 1 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web.
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Estos tutoriales le ayudarán a implementar en primer lugar a IIS en el equipo de desarrollo local para pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que se va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia ASP.NET. Para comenzar con SQL Server Compact y la implementación en SQL Server Compact y tutoriales posteriores muestran cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.
> 
> Los tutoriales se supone que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, es un buen lugar para comenzar un [Tutorial básico de formularios de ASP.NET Web](../tailspin-spyworks/tailspin-spyworks-part-1.md) o un [Tutorial básico de ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de implementación de ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Información general

Estos tutoriales le ayudarán a implementar en primer lugar a IIS en el equipo de desarrollo local para pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que se va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia ASP.NET. Para comenzar con SQL Server Compact y la implementación en SQL Server Compact y tutoriales posteriores muestran cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.

El número de tutoriales – 11 en todos los más de una página de solución de problemas podría provocar que el proceso de implementación parece desalentadora. De hecho, los procedimientos básicos para implementar un sitio constituyen una parte relativamente pequeña del conjunto de tutoriales. Sin embargo, en situaciones del mundo real, a menudo necesita información sobre algún aspecto extra pequeña pero importante de la implementación, por ejemplo, establecer permisos de carpeta en el servidor de destino. Hemos incluido muchas de estas técnicas adicionales en los tutoriales, con la esperanza de que no omita los tutoriales de información que podría impedirle implementar correctamente una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia, y cada parte se basa en la parte anterior. Sin embargo, puede omitir las partes que no son pertinentes para su situación. (Omitir partes podría requerir que ajustar los procedimientos en los tutoriales posteriores.)

## <a name="intended-audience"></a>Público destinatario

Los tutoriales están dirigidos a los desarrolladores de ASP.NET que trabajan en organizaciones pequeñas u otros entornos donde:

- No se utiliza un proceso de integración continua (compilaciones automatizadas y la implementación).
- El entorno de producción es un proveedor de hospedaje de terceros.
- Normalmente, una persona asume varios roles (la misma persona desarrolla, prueba e implementa).

En entornos empresariales, es más habitual para implementar procesos de integración continua y el entorno de producción normalmente se hospeda los servidores de la empresa. Distintas personas también suelen realizan distintos roles. Para obtener información acerca de la implementación empresarial, consulte [implementar aplicaciones Web en escenarios empresariales](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Las organizaciones de todos los tamaños también pueden implementar aplicaciones web en Azure y la mayoría de los procedimientos que se muestra en estos tutoriales se aplica también a los servicios de aplicación de Azure Web Apps. Para obtener una introducción a Azure, consulte [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>El proveedor de hospedaje que se muestra en los tutoriales

Los tutoriales le llevan por el proceso de configuración de una cuenta con una empresa de hospedaje e implementación de la aplicación en ese proveedor de hospedaje. Se ha elegido una empresa de hospedaje específica para que los tutoriales muestran la experiencia completa de la implementación en un sitio Web activo. Cada empresa de hospedaje proporciona características diferentes y la experiencia de implementación en sus servidores varía ligeramente; Sin embargo, el proceso descrito en este tutorial es típico para el proceso general.

El proveedor de hospedaje utilizado para este tutorial, Cytanium.com, es uno de ellos que están disponibles y su uso en este tutorial no constituye una aprobación ni recomendación.

## <a name="deploying-web-site-projects"></a>Implementación de proyectos de sitio Web

Contoso University es un proyecto de aplicación web de Visual Studio. La mayoría de los métodos de implementación y herramientas que se muestra en este tutorial no se aplica a [proyectos de sitios Web](https://msdn.microsoft.com/library/dd547590.aspx). Para obtener información sobre cómo implementar proyectos de sitio web, consulte [mapa de contenido de implementación ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

En este tutorial implementa un proyecto de formularios Web Forms de ASP.NET, pero todo lo que aprenda a realizar también es aplicable a ASP.NET MVC. Un proyecto de MVC de Visual Studio es simplemente otra forma de proyecto de aplicación web. La única diferencia es que si va a implementar en un proveedor de hospedaje que no es compatible con ASP.NET MVC o la versión de destino del mismo, debe asegurarse de que ha instalado adecuado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) Paquete de NuGet en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo usa C#, pero los tutoriales no requieren conocimientos de C# y las técnicas de implementación que se muestra en los tutoriales no son específicas del idioma.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este Tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución. Para ayudarle con algunos escenarios problemáticos comunes, un [página de referencia de la solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) está disponible. Si recibe un mensaje de error o algo no funciona al avanzar por los tutoriales, asegúrese de comprobar la página de solución de problemas.

## <a name="comments-welcome"></a>Bienvenida de comentarios

Comentarios en los tutoriales son bienvenidos y, cuando se actualiza el tutorial se realizarán todos los esfuerzos para tener en cuenta correcciones o sugerencias de mejoras que se proporcionan en los comentarios de tutorial.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene Windows 7 o posterior y uno de los siguientes productos instalados en el equipo:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Si tiene Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, instale también los siguientes productos:

- [Azure SDK para .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (incluye la actualización de publicación en Web)
- [Microsoft Visual Studio 2010 SP1 Tools para SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Algunos otros programas de software es necesario para completar el tutorial, pero no tienes que tener que cargado aún. El tutorial le guiará por los pasos para instalarlo cuando lo necesite.

## <a name="downloading-the-sample-application"></a>Descargar la aplicación de ejemplo

La aplicación que se va a implementar se denomina Contoso University y ya se ha creado para usted. Es una versión simplificada de un sitio web de la universidad, según se describe en la aplicación Contoso University libremente el [tutoriales de Entity Framework en el sitio ASP.NET](https://asp.net/entity-framework/tutorials).

Una vez instalados los requisitos previos, descargue el [aplicación web Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). El *.zip* archivo contiene varias versiones del proyecto y un archivo PDF que contiene todos los tutoriales de 12. Para trabajar con los pasos del tutorial, comience con ContosoUniversity Begin. Para ver el aspecto que tiene el proyecto al final de los tutoriales, abra ContosoUniversity-End. Para ver qué aspecto tiene el proyecto antes de la migración a SQL Server completo en el tutorial de 10, abra ContosoUniversity AfterTutorial09.

Para prepararse para trabajar a través de los pasos del tutorial, guarde a ContosoUniversity Begin en cualquier carpeta que utilice para trabajar con proyectos de Visual Studio. De forma predeterminada, esta es la carpeta siguiente:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Para las capturas de pantalla en este tutorial, la carpeta del proyecto se encuentra en el directorio raíz en el `C`: unidad.)

Inicie Visual Studio, abra el proyecto y presione CTRL-F5 para ejecutarla.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Las páginas del sitio Web son accesibles desde la barra de menús y le permiten realizar las siguientes funciones:

- Mostrar las estadísticas de alumno (la página About).
- Mostrar, editar, eliminar y agregar a los alumnos.
- Mostrar y editar los cursos.
- Visualización y edición de instructores.
- Mostrar y editar los departamentos de TI.

A continuación se muestran capturas de pantalla de unas pocas páginas representativas.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisión de las características de la aplicación que afectan a la implementación

Las siguientes características de la aplicación afecta a cómo implementarlo o lo que tiene que hacer para implementarlo. Cada uno de ellos se explica con más detalle en los siguientes tutoriales de la serie.

- Contoso University se usa una base de datos de SQL Server Compact para almacenar datos de la aplicación, como nombres de student e instructor. La base de datos contiene una combinación de datos de prueba y datos de producción y, cuando se implementa en producción debe excluir los datos de prueba. Más adelante en la serie de tutoriales a migrar desde SQL Server Compact a SQL Server.
- La aplicación utiliza el sistema de pertenencia ASP.NET, que almacena información de cuenta de usuario en una base de datos de SQL Server Compact. La aplicación define un usuario de administrador que tenga acceso a información restringida. Deberá implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- Dado que la base de datos de aplicación y la base de datos de pertenencia a usan SQL Server Compact como el motor de base de datos, tendrá que implementar el motor de base de datos en el proveedor de hospedaje, así como las bases de datos.
- La aplicación usa proveedores de pertenencia universales de ASP.NET para que el sistema de pertenencia puede almacenar sus datos en una base de datos de SQL Server Compact. El ensamblado que contiene los proveedores de pertenencia universal debe implementarse con la aplicación.
- La aplicación usa Entity Framework 5.0 para tener acceso a datos en la base de datos de aplicación. El ensamblado que contiene el Entity Framework 5.0 debe implementarse con la aplicación.
- La aplicación utiliza un error de terceros, registros e informes de utilidad. Esta utilidad se proporciona en un ensamblado que debe implementarse con la aplicación.
- La utilidad de registro de errores escribe información de error en los archivos XML en una carpeta de archivos. Tiene que asegurarse de que la cuenta que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta, y debe excluir de esta carpeta de implementación. (En caso contrario, los datos de registro de error desde el entorno de prueba podrían estar implementados en producción o es posible que se puede eliminar archivos de registro de errores de producción).
- La aplicación incluye algunas opciones de configuración que se deben cambiar en implementado *Web.config* archivo según el entorno de destino (ensayo o producción) y otras configuraciones que deben cambiarse según la compilación configuración (Debug o Release).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Se debe implementar únicamente el ensamblado que genera este proyecto, no el propio proyecto.

En este primer tutorial de la serie, ha descargado el proyecto de Visual Studio de ejemplo y revisar las características de sitio que afectan a cómo implementar la aplicación. En los tutoriales siguientes, te preparas para la implementación mediante la configuración de algunas de estas cosas se administran automáticamente. Otros que se encarga de forma manual.

> [!div class="step-by-step"]
> [Siguiente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
