---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 de 12 | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar sitios Web de Windows Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una actualización de la base de datos en una base de datos completa de SQL Server. Dado que migraciones de Code First realiza todo el trabajo de actualización de la base de datos, el proceso es casi idéntico a lo que hizo para SQL Server Compact en el [implementar una actualización de la base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección del tutorial realizará una base de datos cambiar y cambios de código correspondiente, a continuación, las pruebas en Visual Studio como preparación para su implementación en los entornos de prueba y producción. El cambio implica agregar un `OfficeHours` columna a la `Instructor` entidad y mostrar la nueva información en el **instructores** página web.

En el proyecto ContosoUniversity.DAL, abra *Instructor.cs* y agregue la siguiente propiedad entre el `HireDate` y `Courses` propiedades:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Actualizar la clase de inicializador para que propaga la nueva columna con datos de prueba. Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye la nueva columna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para el horario de oficina inmediatamente antes de cerrar `</Columns>` en la primera etiqueta `GridView` control:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compile la solución.

Abra la **Package Manager Console** ventana y seleccione ContosoUniversity.DAL como el **proyecto predeterminado**.

Escriba los siguientes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Ejecute la aplicación y seleccione la **instructores** página. La página tarda un poco más de lo habitual en cargarse, dado que Entity Framework vuelve a crear la base de datos y propaga con datos de prueba.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementación de la actualización de la base de datos en el entorno de prueba

Cuando usas migraciones de Code First, el método para implementar un cambio de base de datos en SQL Server es el mismo que para SQL Server Compact. Sin embargo, tendrá que cambiar la prueba de perfil de publicación porque todavía se configuró para migrar de SQL Server Compact a SQL Server.

El primer paso es quitar las transformaciones de cadena de conexión que creó en el tutorial anterior. Ya no son necesarios porque especificará las transformaciones de cadena de conexión en el perfil de publicación, como lo hizo antes de que configure la **Empaquetar/publicar SQL** ficha para la migración a SQL Server.

Abra la *Web.Test.config* de archivos y quitar el `connectionStrings` elemento. La transformación sola restante en el *Web.Test.config* archivo es para el `Environment` valor en el `appSettings` elemento.

Ahora puede actualizar el perfil de publicación y la publicación en el entorno de prueba.

Abra la **Publicar Web** asistente y, a continuación, cambie a la **perfil** ficha.

Seleccione el **prueba** perfil de publicación.

Seleccione el **configuración** ficha.

Haga clic en **habilitar la base de datos nueva publicación mejoras**.

En el cuadro de la cadena de conexión para **SchoolContext**, escriba el mismo valor que utilizó en el *Web.Test.config* archivo de transformación en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**. (En la versión de Visual Studio, la casilla de verificación podría etiquetarse **aplicar Code First Migrations**.)

En el cuadro de la cadena de conexión para **DefaultConnection**, escriba el mismo valor que utilizó en el *Web.Test.config* archivo de transformación en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deje **Actualizar base de datos** desactivada.

Haga clic en **Publicar**.

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página de inicio de la Universidad de Contoso.

Seleccione la página de instructores.

Cuando la aplicación ejecuta esta página, intenta obtener acceso a la base de datos. Migraciones de Code First comprueba si la base de datos es actual y encuentra que la migración más reciente no se ha aplicado todavía. Migraciones de Code First se aplica la migración más reciente, se ejecuta el `Seed` método y, a continuación, en la página se ejecuta con normalidad. Ver la nueva columna de horario de oficina con los datos a la inicialización.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementación de la actualización de la base de datos en el entorno de producción

Tendrá que cambiar también el perfil de publicación para el entorno de producción. En este caso deberá quitar el perfil existente y crear uno nuevo mediante la importación de un archivo .publishsettings actualizada. El archivo actualizado incluirá la cadena de conexión para la base de datos de SQL Server en Cytanium.

Como ha podido comprobar cuando implementa en el entorno de prueba, ya no necesita transformaciones de cadena de conexión en el *Web.Production.config* archivo de transformación. Abrir archivos y quitar el `connectionStrings` elemento. Las transformaciones restantes son para el `Environment` valor en el `appSettings` elemento y el `location` elemento que restringe el acceso a los informes de errores de Elmah.

Antes de crear un nuevo perfil de publicación para la producción, descargue un archivo .publishsettings actualizada del mismo modo que lo hizo anteriormente en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. (En el panel de control Cytanium, haga clic en **sitios Web**y, a continuación, haga clic en el **contosouniversity.com** sitio Web. Seleccione el **publicación Web** ficha y, a continuación, haga clic en **descargar perfil de publicación para este sitio web**.) La razón por la que están haciendo esto es recoger la cadena de conexión de base de datos en el archivo .publishsettings. La cadena de conexión no estaba disponible la primera vez que se descargó el archivo, ya que se sigue usando SQL Server Compact y no había creado la base de datos de SQL Server en Cytanium todavía.

Ahora puede actualizar el perfil de publicación y la publicación en el entorno de producción.

Abra la **Publicar Web** asistente y, a continuación, cambie a la **perfil** ficha.

Haga clic en **administrar perfiles**y, a continuación, elimine el perfil de producción.

Cerrar la **Publicar Web** Asistente para guardar los cambios.

Abra la **Publicar Web** Asistente nuevo y, a continuación, haga clic en **importación**.

En el **conexión** , modifique **dirección URL de destino** en el valor adecuado si usa una dirección URL temporal.

Haga clic en **Siguiente**.

En el **configuración** , haga clic en **habilitar la base de datos nueva publicación mejoras**.

En la lista de desplegable de cadena de conexión para **SchoolContext**, seleccione la cadena de conexión Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Seleccione **migraciones ejecutar Code First (se ejecuta al iniciarse la aplicación)**.

En la lista de desplegable de cadena de conexión para **DefaultConnection**, seleccione la cadena de conexión Cytanium.

Seleccione el **perfil** , haga clic en **administrar perfiles**y cambie el nombre del perfil de "contosouniversity.com - Web Deploy" a "Producción".

Cierre el perfil de publicación para guardar el cambio, a continuación, vuelva a abrirlo.

Haga clic en **Publicar**. (Para un sitio Web de producción real, se copiaría *aplicación\_offline.htm* a producción y put en la carpeta del proyecto antes de publicar, a continuación, quítela cuando se completa la implementación.)

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página de inicio de la Universidad de Contoso.

Seleccione la página de instructores.

Migraciones de Code First actualiza la base de datos de la misma manera que se hacía en el entorno de prueba. Ver la nueva columna de horario de oficina con los datos a la inicialización.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Ahora correctamente implementó una actualización de la aplicación que incluye un cambio de la base de datos, con una base de datos de SQL Server.

## <a name="more-information"></a>Más información

Con esto finaliza la siguiente serie de tutoriales sobre la implementación de una aplicación web ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información acerca de cualquiera de los temas tratados en estos tutoriales, vea el [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) en el sitio web MSDN.

## <a name="acknowledgements"></a>Reconocimientos

Me gustaría dar las gracias a las siguientes personas que realizado aportaciones importantes para el contenido de esta serie de tutoriales:

- [Alberto Poblacion, MVP &amp; MCT, España](https://mvp.support.microsoft.com/profile/Alberto)
- Estados Unidos de Jarod Ferguson, MVP de desarrollo de plataforma de datos,
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
