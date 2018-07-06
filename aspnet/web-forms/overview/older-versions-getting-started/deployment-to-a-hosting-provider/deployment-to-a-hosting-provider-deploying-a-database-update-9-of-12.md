---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5edb3cae785d578674f47f1aa3d5be7dfa6a8791
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822100"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de la base de datos - 9 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, realiza un cambio de base de datos y los cambios de código relacionados, pruebe los cambios en Visual Studio y luego implementar la actualización en entornos de prueba y producción.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección, agregará una columna de fecha de nacimiento a la `Person` clase base para el `Student` y `Instructor` entidades. A continuación, actualice la página que muestra los datos de un instructor para que se muestre la nueva columna.

En el *ContosoUniversity.DAL* proyecto, abra *Person.cs* y agregue la siguiente propiedad al final de la `Person` clase (debe haber dos que hay a continuación de llaves de cierre):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

A continuación, actualice el método de inicialización para que proporcione un valor para la nueva columna. Abra *migrations\configuration* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye información de fecha de nacimiento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregue un nuevo campo de plantilla para mostrar la fecha de nacimiento. Agréguelo entre los que para la asignación de office y de fecha de contratación:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Si se queda sin sincronizarse sangría de código, puede presionar CTRL-K y, a continuación, CTRL+D para volver a formatear automáticamente el archivo.)

Compile la solución y, a continuación, abra el **Package Manager Console** ventana. Asegúrese de que ContosoUniversity.DAL todavía está seleccionado como el **proyecto predeterminado**.

En el **Package Manager Console** ventana, seleccione **ContosoUniversity.DAL** como el **proyecto predeterminado**y, a continuación, escriba el siguiente comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Cuando finalice este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` (clase) y en el `Up` método puede ver el código que crea la nueva columna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile la solución y, a continuación, escriba el siguiente comando en el **Package Manager Console** ventana (asegúrese de que el proyecto de ContosoUniversity.DAL todavía está seleccionado):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Cuando finaliza el comando, ejecute la aplicación y seleccione la página de instructores. Cuando se carga la página, verá que tiene el nuevo campo de fecha de nacimiento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementar la actualización de la base de datos en el entorno de prueba

En **el Explorador de soluciones** seleccione el proyecto ContosoUniversity.

En el **una publicación en Web clic** barra de herramientas, seleccione el **prueba** perfil de publicación y, a continuación, haga clic en **publicación Web**. (Si se deshabilita la barra de herramientas, seleccione el proyecto ContosoUniversity en **el Explorador de soluciones**.)

Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal. Ejecute la página de instructores para comprobar que la actualización se ha implementado correctamente. Cuando la aplicación intenta tener acceso a la base de datos para esta página, Code First actualiza el esquema de base de datos y ejecuta el `Seed` método. Cuando aparezca la página, vea el esperado **fecha de nacimiento** columna con fechas en ella.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementar la actualización de la base de datos en el entorno de producción

Ahora puede implementar en producción. La única diferencia es que podrá usar *aplicación\_offline.htm* impedir que los usuarios acceso al sitio y lo que actualiza la base de datos, mientras que va a implementar los cambios. Implementación de producción, realice los pasos siguientes:

- Cargar el *aplicación\_offline.htm* archivo al sitio de producción.
- En Visual Studio, elija el perfil de producción en el **una publicación en Web clic** barra de herramientas y haga clic en **publicación Web**.
- Eliminar el *aplicación\_offline.htm* archivo desde el sitio de producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción debe implementar un plan de copia de seguridad. Es decir, debe copiar periódicamente los *School-Prod.sdf* y *aspnet Prod.sdf* archivos desde la producción de sitio a una ubicación de almacenamiento seguro y debe mantener varias generaciones de estos copias de seguridad. Cuando se actualiza la base de datos, debe realizar una copia de seguridad desde inmediatamente antes del cambio. A continuación, si comete un error y no detectarlo hasta después de haber implementado en producción, aún podrá recuperar la base de datos al estado que tenía antes de resultó dañado.


Cuando Visual Studio abre la dirección URL de la página principal en el explorador, el *aplicación\_offline.htm* se muestra la página. Después de eliminar el *aplicación\_offline.htm* archivo, vaya a la página principal para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Ahora ha implementado una actualización de la aplicación que incluye un cambio de base de datos para pruebas y producción. El siguiente tutorial muestra cómo migrar la base de datos de SQL Server Compact a SQL Server Express y SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
