---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816637"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo implementar en sitios Web de Windows Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una actualización de la base de datos en una base de datos completa de SQL Server. Como migraciones de Code First hace todo el trabajo de actualización de la base de datos, el proceso es casi idéntico a lo que hizo para SQL Server Compact en el [implementar una actualización de la base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección del tutorial, hará cambiar una base de datos y los cambios de código correspondientes, a continuación, las pruebas en Visual Studio como preparación para su implementación en los entornos de prueba y producción. El cambio implica agregar un `OfficeHours` columna a la `Instructor` entidad y muestra la información nueva en el **instructores** página web.

En el proyecto ContosoUniversity.DAL, abra *Instructor.cs* y agregue la siguiente propiedad entre la `HireDate` y `Courses` propiedades:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Actualice la clase de inicializador para que inicializa la nueva columna con datos de prueba. Abra *migrations\configuration* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye la nueva columna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregue un nuevo campo de plantilla para el horario de oficina justo antes de cerrar `</Columns>` en la primera etiqueta `GridView` control:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compile la solución.

Abra el **Package Manager Console** ventana y seleccione ContosoUniversity.DAL como el **proyecto predeterminado**.

Escriba los siguientes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Ejecute la aplicación y seleccione el **instructores** página. La página tarda un poco más de lo habitual en cargarse, dado que Entity Framework vuelve a crear la base de datos y la inicializa con datos de prueba.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementar la actualización de la base de datos en el entorno de prueba

Cuando se usa migraciones de Code First, el método para implementar un cambio de base de datos en SQL Server es el mismo que para SQL Server Compact. Sin embargo, tiene que cambiar la prueba de perfil de publicación porque todavía está establecida para migrar de SQL Server Compact a SQL Server.

El primer paso es quitar las transformaciones de cadena de conexión que creó en el tutorial anterior. Ya no son necesarias porque deberá especificar las transformaciones de cadena de conexión del perfil de publicación, como hizo antes de configurar el **Empaquetar/publicar SQL** ficha para la migración a SQL Server.

Abra el *Web.Test.config* de archivos y quitar el `connectionStrings` elemento. La única transformación restante en el *Web.Test.config* archivo es para el `Environment` valor en el `appSettings` elemento.

Ahora puede actualizar el perfil de publicación y la publicación para el entorno de prueba.

Abra el **publicación Web** asistente y, a continuación, cambie a la **perfil** ficha.

Seleccione el **prueba** perfil de publicación.

Seleccione el **configuración** ficha.

Haga clic en **habilitar la base de datos nuevas mejoras de publicación**.

En el cuadro cadena de conexión para **SchoolContext**, escriba el mismo valor que usó en el *Web.Test.config* archivo de transformación en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**. (En su versión de Visual Studio, la casilla de verificación podría etiquetarse **aplicar Code First Migrations**.)

En el cuadro cadena de conexión para **DefaultConnection**, escriba el mismo valor que usó en el *Web.Test.config* archivo de transformación en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deje **Actualizar base de datos** desactivada.

Haga clic en **Publicar**.

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página principal de Contoso University.

Seleccione la página de instructores.

Cuando la aplicación ejecuta esta página, intenta tener acceso a la base de datos. Migraciones de Code First comprueba si la base de datos actual y busca la última migración no se han aplicado todavía. Migraciones de Code First se aplica la última migración, se ejecuta el `Seed` método y, a continuación, en la página se ejecuta con normalidad. Ver la nueva columna de horario de oficina con los datos inicializados.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementar la actualización de la base de datos en el entorno de producción

Debe cambiar también el perfil de publicación para el entorno de producción. En este caso deberá quitar el perfil existente y crear uno nuevo importando un archivo .publishsettings actualizada. El archivo actualizado incluirá la cadena de conexión para la base de datos de SQL Server Cytanium.

Como se vio cuando implementa en el entorno de prueba, ya no necesita transformaciones de cadena de conexión en el *Web.Production.config* archivo de transformación. Abrir archivo y quitar el `connectionStrings` elemento. Las transformaciones restantes son para el `Environment` valor en el `appSettings` elemento y el `location` elemento que restringe el acceso a los informes de errores de Elmah.

Antes de crear un nuevo perfil de publicación para la producción, descargar un archivo .publishsettings actualizado del mismo modo que hicimos antes en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. (En el panel de control Cytanium, haga clic en **sitios Web**y, a continuación, haga clic en el **contosouniversity.com** sitio Web. Seleccione el **publicación Web** pestaña y, a continuación, haga clic en **descargar perfil de publicación para este sitio web**.) Es el motivo por el que está realizando la operación recoger la cadena de conexión de base de datos en el archivo .publishsettings. La cadena de conexión no estaba disponible la primera vez que se descargó el archivo, porque todavía estaba utilizando SQL Server Compact y no se habían creado la base de datos de SQL Server en Cytanium aún.

Ahora puede actualizar el perfil de publicación y la publicación en el entorno de producción.

Abra el **publicación Web** asistente y, a continuación, cambie a la **perfil** ficha.

Haga clic en **administrar perfiles**y, a continuación, elimine el perfil de producción.

Cerrar la **publicación Web** Asistente para guardar este cambio.

Abra el **publicación Web** nuevo asistente y, a continuación, haga clic en **importación**.

En el **conexión** , modifique **dirección URL de destino** con el valor correcto si está utilizando una URL temporal.

Haga clic en **Siguiente**.

En el **configuración** , haga clic **habilitar la base de datos nuevas mejoras de publicación**.

En la lista de desplegable cadena de conexión para **SchoolContext**, seleccione la cadena de conexión Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Seleccione **migraciones ejecutar Code First (se ejecuta al iniciar la aplicación)**.

En la lista de desplegable cadena de conexión para **DefaultConnection**, seleccione la cadena de conexión Cytanium.

Seleccione el **perfil** , haga clic **administrar perfiles**y cambie el nombre del perfil de "contosouniversity.com - Web Deploy" a "Producción".

Cierre el perfil de publicación para guardar el cambio y después vuelva a abrirlo.

Haga clic en **Publicar**. (Para un sitio Web de producción real, se copiaría *aplicación\_offline.htm* a producción y put en la carpeta del proyecto antes de publicar, a continuación, quítela cuando se completa la implementación.)

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página principal de Contoso University.

Seleccione la página de instructores.

Migraciones de Code First actualiza la base de datos de la misma manera que lo hacía en el entorno de prueba. Ver la nueva columna de horario de oficina con los datos inicializados.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Se ha implementado correctamente la actualización de una aplicación que incluye un cambio de base de datos, con una base de datos de SQL Server.

## <a name="more-information"></a>Más información

Con esto finaliza esta serie de tutoriales sobre la implementación de una aplicación web ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información acerca de cualquiera de los temas tratados en estos tutoriales, vea el [mapa de contenido de implementación ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) en el sitio web MSDN.

## <a name="acknowledgements"></a>Reconocimientos

Gustaría agradecer a las siguientes personas que realizan aportaciones significativas al contenido de esta serie de tutoriales:

- [Alberto Poblacion, MVP &amp; MCT, España](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de desarrollo de plataforma de datos, España
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
