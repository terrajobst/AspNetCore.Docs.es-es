---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción - 1 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: a0f38c83bd9231dbd37d3d505c90316af521b336
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción - 1 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web.
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Estos tutoriales lo guían a través de implementar primero a IIS en el equipo de desarrollo local para las pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia ASP.NET. Comenzar a usar SQL Server Compact e implementan en SQL Server Compact y los tutoriales posteriores muestra cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.
> 
> Los tutoriales se supone que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, es un buen lugar para comenzar una [Tutorial básico de formularios de ASP.NET Web](../tailspin-spyworks/tailspin-spyworks-part-1.md) o un [Tutorial básico de MVC de ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de implementación de ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Información general

Estos tutoriales lo guían a través de implementar primero a IIS en el equipo de desarrollo local para las pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia ASP.NET. Comenzar a usar SQL Server Compact e implementan en SQL Server Compact y los tutoriales posteriores muestra cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.

El número de tutoriales: 11 en todas las más de una página de solución de problemas: podría provocar que el proceso de implementación parece desalentadora. De hecho, los procedimientos básicos para implementar un sitio constituyen una parte relativamente pequeña del conjunto de tutorial. Sin embargo, en situaciones del mundo real, a menudo necesita información sobre algún aspecto extra pequeña pero importante de la implementación, por ejemplo, establecer los permisos de carpetas en el servidor de destino. Hemos incluido muchas de estas técnicas adicionales en los tutoriales, con la esperanza de que los tutoriales no deje información que podría impedirle que implementar correctamente una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia, y cada parte se basa en la parte anterior. Sin embargo, puede omitir los elementos que no sean relevantes para su situación. (Omitir partes puede requerir ajustar los procedimientos en tutoriales posteriores.)

## <a name="intended-audience"></a>Destinatarios

Los tutoriales están dirigidos a los desarrolladores ASP.NET que trabajan en organizaciones pequeñas o en otros entornos donde:

- No se utiliza un proceso de integración continua (compilaciones automatizadas e implementación).
- El entorno de producción es un proveedor de hospedaje de terceros.
- Normalmente, una persona asume varios roles (la misma persona desarrolla, prueba e implementa).

En entornos empresariales, es más habitual para implementar los procesos de integración continua y el entorno de producción normalmente está hospedado en los servidores de la compañía. Cada persona normalmente, también ejecutan diferentes roles. Para obtener información acerca de la implementación empresarial, consulte [implementar aplicaciones Web en escenarios empresariales](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Las organizaciones de todos los tamaños también pueden implementar aplicaciones web en Azure y la mayoría de los procedimientos que se muestran en estos tutoriales se aplica también a aplicaciones de Web de servicios de aplicación de Azure. Para obtener una introducción a Azure, consulte [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>El proveedor de hospedaje que se muestra en los tutoriales

Los tutoriales le conducen por el proceso de configurar una cuenta con una empresa de hospedaje e implementar la aplicación en ese proveedor de hospedaje. Una empresa de hospedaje específica se ha elegido para que los tutoriales muestran la experiencia completa de la implementación en un sitio Web. Cada empresa de hospedaje proporciona diversas características y la experiencia de la implementación de sus servidores varía ligeramente; Sin embargo, el proceso descrito en este tutorial es habitual para el proceso general.

El proveedor de hospedaje que se utiliza para este tutorial, Cytanium.com, es uno de muchos de los que están disponibles y su uso en este tutorial no constituye una aprobación ni recomendación.

## <a name="deploying-web-site-projects"></a>Implementación de proyectos de sitios Web

Universidad de Contoso es un proyecto de aplicación web de Visual Studio. La mayoría de los métodos de implementación y las herramientas que se muestran en este tutorial no se aplica a [proyectos de sitios Web](https://msdn.microsoft.com/library/dd547590.aspx). Para obtener información sobre cómo implementar proyectos de sitios web, consulte [mapa de contenido de implementación de ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

Para este tutorial implementa un proyecto de formularios Web Forms de ASP.NET, pero todo lo que obtenga información acerca de cómo llevar a cabo también es aplicable a ASP.NET MVC. Un proyecto de MVC de Visual Studio es simplemente otro tipo de proyecto de aplicación web. La única diferencia es que si va a implementar en un proveedor de hospedaje que no es compatible con ASP.NET MVC o la versión de destino del mismo, debe asegurarse de que ha instalado la correspondiente ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) Paquete de NuGet en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo usa C#, pero los tutoriales no requieren conocimientos de C# y las técnicas de implementación que se muestra en los tutoriales no son específicos del idioma.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este Tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución. Para ayudarle con algunos escenarios comunes de problema, un [página de referencia de la solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) está disponible. Si aparece un mensaje de error o algo no funciona tal y como se vaya a través de los tutoriales, asegúrese de comprobar la página de solución de problemas.

## <a name="comments-welcome"></a>Bienvenida de comentarios

Comentarios en los tutoriales son bienvenidos y, cuando se actualiza el tutorial se realizará todo lo posible para tener en cuenta correcciones o sugerencias para mejoras en el que se proporcionan en los comentarios de tutorial.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene Windows 7 o posterior y uno de los siguientes productos instalados en el equipo:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Si tiene Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, instale también los siguientes productos:

- [Azure SDK para .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (incluye la actualización de publicación de Web)
- [Microsoft Visual Studio 2010 SP1 Tools para SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Otro software es necesario para completar el tutorial, pero no tienes que tener que cargado todavía. El tutorial le guiará por los pasos para instalarlo cuando lo necesite.

## <a name="downloading-the-sample-application"></a>Descargar la aplicación de ejemplo

La aplicación que se va a implementar se denomina Contoso universidad y ya se ha creado para usted. Es una versión simplificada de un sitio web de la universidad, flexible basado en la aplicación de la Universidad de Contoso se describe en el [tutoriales de Entity Framework en el sitio ASP.NET](https://asp.net/entity-framework/tutorials).

Si tiene los requisitos previos instalados, descargue el [aplicación web de Contoso Universidad](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). El *.zip* archivo contiene varias versiones del proyecto y un archivo PDF que contiene todos los tutoriales de 12. Para trabajar con los pasos del tutorial, comience con Begin ContosoUniversity. Para ver el aspecto que tiene el proyecto al final de los tutoriales, abra ContosoUniversity-End. Para ver el aspecto que tiene el proyecto antes de la migración completa de SQL Server en el tutorial 10, abra ContosoUniversity AfterTutorial09.

Para prepararse para trabajar con los pasos de tutorial, guarde a Begin ContosoUniversity en cualquier carpeta que se puede usar para trabajar con proyectos de Visual Studio. De forma predeterminada, ésta es la carpeta siguiente:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Para las capturas de pantalla en este tutorial, la carpeta del proyecto se encuentra en el directorio raíz en el `C`: unidad.)

Inicie Visual Studio, abra el proyecto y presione CTRL-F5 para ejecutarlo.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Páginas Web sean accesibles desde la barra de menús y le permiten realizar las siguientes funciones:

- Mostrar las estadísticas de estudiante (la página acerca de).
- Mostrar, editar, eliminar y agregar los alumnos.
- Mostrar y editar cursos.
- Mostrar y editar instructores.
- Mostrar y editar los departamentos.

Siguientes son capturas de pantalla de unas pocas páginas representativas.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisión de características que afectan a la implementación de la aplicación

Las siguientes características de la aplicación afecta a cómo implementarlo o lo que tendrá que hacer para implementarlo. Cada uno de ellos se explica con más detalle en los siguientes tutoriales de la serie.

- Universidad de Contoso usa una base de datos de SQL Server Compact para almacenar datos de aplicación, como nombres de student y instructor. La base de datos contiene una mezcla de datos de prueba y los datos de producción y, cuando se implementa en producción que precise excluir los datos de prueba. Más adelante en la serie de tutoriales podrá migrar de SQL Server Compact a SQL Server.
- La aplicación utiliza el sistema de pertenencia ASP.NET, que almacena información de la cuenta de usuario en una base de datos de SQL Server Compact. La aplicación define un usuario de administrador que tenga acceso a información restringida. Debe implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- Dado que la base de datos de aplicación y la base de datos de pertenencia a usan SQL Server Compact como el motor de base de datos, tendrá que implementar el motor de base de datos para el proveedor de hospedaje, así como las bases de datos.
- La aplicación utiliza los proveedores de pertenencia universales de ASP.NET para que el sistema de pertenencia puede almacenar sus datos en una base de datos de SQL Server Compact. El ensamblado que contiene los proveedores de pertenencia universal debe implementarse con la aplicación.
- La aplicación usa Entity Framework 5.0 para tener acceso a datos en la base de datos de aplicación. El ensamblado que contiene el Entity Framework 5.0 debe implementarse con la aplicación.
- La aplicación utiliza un error de terceros, registros e informes de utilidad. Esta utilidad se proporciona en un ensamblado que debe implementarse con la aplicación.
- La utilidad de registro de error escribe información de error en archivos XML en una carpeta de archivos. Tiene que asegurarse de que la cuenta que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta, y se debe excluir esta carpeta de implementación. (En caso contrario, los datos de registro de error desde el entorno de prueba podrían implementarse en producción o se podrían eliminar los archivos de registro de errores de producción).
- La aplicación incluye algunos valores de configuración que se deben cambiar en implementado *Web.config* archivo según el entorno de destino (prueba o producción) y otras opciones que se deben cambiar dependiendo de la compilación configuración (Debug o Release).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Se debe implementar únicamente el ensamblado que genera este proyecto, no el propio proyecto.

En este primer tutorial de la serie, ha descargado el proyecto de Visual Studio de ejemplo y revisar las características de sitio que afectan a cómo implementar la aplicación. En los siguientes tutoriales para preparar para la implementación mediante la configuración de algunas de estas cosas administrará automáticamente. Otros que se ocupa de forma manual.

>[!div class="step-by-step"]
[Siguiente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
