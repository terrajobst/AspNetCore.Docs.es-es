---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12 | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, realice un cambio de base de datos y los cambios de código relacionado, comprobar los cambios en Visual Studio, y luego implementar la actualización en entornos de prueba y producción.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección, agregará una columna de fecha de nacimiento a la `Person` la clase base para la `Student` y `Instructor` entidades. A continuación, actualice la página que muestra los datos de instructor para que se muestre la nueva columna.

En el *ContosoUniversity.DAL* proyecto abierto *Person.cs* y agregue la siguiente propiedad al final de la `Person` clase (debe haber dos sigue llaves de cierre):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

A continuación, actualizar el método de inicialización para que proporcione un valor para la nueva columna. Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye información de fecha de nacimiento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para mostrar la fecha de nacimiento. Agregarlo entre los de asignación de fecha y la oficina de contratación:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Si la sangría del código obtiene la sincronización, puede presionar CTRL-K y, a continuación, CTRL+D para volver a formatear automáticamente el archivo.)

Compile la solución y, a continuación, abra el **Package Manager Console** ventana. Asegúrese de que ContosoUniversity.DAL todavía está seleccionado como el **proyecto predeterminado**.

En el **Package Manager Console** ventana, seleccione **ContosoUniversity.DAL** como el **proyecto predeterminado**y, a continuación, escriba el comando siguiente:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` (clase) y en el `Up` método puede ver el código que crea la nueva columna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile la solución y, a continuación, escriba el siguiente comando en el **Package Manager Console** ventana (asegúrese de que todavía esté seleccionado el proyecto de ContosoUniversity.DAL):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Cuando finaliza el comando, ejecute la aplicación y seleccione la página de instructores. Cuando se carga la página, verá que tiene el nuevo campo de fecha de nacimiento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementación de la actualización de la base de datos en el entorno de prueba

En **el Explorador de soluciones** seleccione el proyecto ContosoUniversity.

En el **Web uno haga clic en publicar** barra de herramientas, seleccione la **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web**. (Si se deshabilita la barra de herramientas, seleccione el proyecto ContosoUniversity en **el Explorador de soluciones**.)

Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal. Ejecute la página de instructores para comprobar que la actualización se ha implementado correctamente. Cuando la aplicación intenta tener acceso a la base de datos de esta página, Code First actualiza el esquema de base de datos y se ejecuta el `Seed` método. Cuando se muestra la página, verá el esperado **la fecha de nacimiento** columna con fechas en ella.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementación de la actualización de la base de datos en el entorno de producción

Ahora puede implementar en producción. La única diferencia es que podrá usar *aplicación\_offline.htm* para impedir que los usuarios tienen acceso al sitio y lo que actualiza la base de datos mientras se va a implementar cambios. Para la implementación de producción, realice los pasos siguientes:

- Cargar la *aplicación\_offline.htm* archivo para el sitio de producción.
- En Visual Studio, elija el perfil de producción en el **Web uno haga clic en publicar** barra de herramientas y haga clic en **Publicar Web**.
- Eliminar el *aplicación\_offline.htm* archivo desde el sitio de producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción debe implementar un plan de copia de seguridad. Es decir, debe copiar periódicamente el *School-Prod.sdf* y *Prod.sdf aspnet* archivos de producción de sitio a una ubicación de almacenamiento seguro y debe mantener varias generaciones de estos copias de seguridad. Cuando se actualiza la base de datos, debe realizar una copia de seguridad de inmediatamente antes del cambio. A continuación, si comete un error y no detectar hasta después de haber implementado en producción, aún podrá recuperar la base de datos al estado que tenía antes de resultó dañado.


Cuando Visual Studio abre la URL de la página de inicio en el explorador, el *aplicación\_offline.htm* se muestra la página. Después de eliminar el *aplicación\_offline.htm* archivo, que puede ir a la página principal nuevo para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Ahora que ha implementado una actualización de la aplicación que incluye un cambio de base de datos en la prueba y producción. El siguiente tutorial muestra cómo migrar la base de datos de SQL Server Compact a SQL Server Express y SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
