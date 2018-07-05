---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fbfbc667b4031c67827dc158ffca4d1b4dbfb23e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394299"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. La aplicación de ejemplo es un sitio Web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.
> 
> El tutorial muestra ejemplos en C#. El [ejemplo descargable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contiene código en C# y Visual Basic.
> 
> ## <a name="database-first"></a>En primer lugar la base de datos
> 
> Hay tres maneras de trabajar con datos en Entity Framework: *Database First*, *Model First*, y *Code First*. Este tutorial es para la primera base de datos. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir el mejor método para su escenario, consulte [flujos de trabajo de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>formularios Web Forms
> 
> Esta serie de tutoriales usa el modelo de formularios Web Forms de ASP.NET y se supone que sabe cómo trabajar con formularios Web Forms de ASP.NET en Visual Studio. Si no, ve [Introducción a ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si prefiere trabajar con el marco de MVC de ASP.NET, vea [Introducción a Entity Framework con ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. El tutorial no se ha probado con las versiones posteriores de Visual Studio. Existen muchas diferencias en las selecciones de menú, cuadros de diálogo y plantillas. |
> | .NET 4 | .NET 4.5 es compatible con .NET 4, pero el tutorial no se ha probado con .NET 4.5. |
> | Entity Framework 4 | El tutorial no se ha probado con las versiones posteriores de Entity Framework. A partir de Entity Framework 5, EF usa de forma predeterminada el `DbContext API` que se introdujo con EF 4.1. El control EntityDataSource se diseñó para usar el `ObjectContext` API. Para obtener información sobre cómo usar EntityDataSource control con el `DbContext` API, consulte [esta entrada de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework y LINQ al foro de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

La aplicación que se va a compilar en estos tutoriales es un sitio Web de la Universidad simple.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. A continuación, se muestran algunas de las pantallas que se va a crear.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Creación de la aplicación Web

Para iniciar el tutorial, abra Visual Studio y, a continuación, cree un nuevo proyecto de aplicación Web ASP.NET con la **aplicación Web ASP.NET** plantilla:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Esta plantilla crea un proyecto de aplicación web que ya incluye una hoja de estilos y páginas maestras:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Abra el *Site.Master* de archivos y cambiar "My ASP.NET Application" a "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Buscar el *menú* control denominado `NavigationMenu` y reemplácelo por el marcado siguiente, que agrega los elementos de menú para las páginas que va a crear.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Abra el *Default.aspx* página y cambie el `Content` control denominado `BodyContent` a este:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Ahora tiene una página principal simple con vínculos a las diversas páginas que se creará:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Creación de la base de datos

Estos tutoriales, usará el Diseñador de modelos de datos de Entity Framework para crear automáticamente el modelo de datos basado en una base de datos existente (a menudo denominada el *database first* enfoque). Una alternativa que no se trata en esta serie de tutoriales consiste en crear el modelo de datos manualmente y, a continuación, tener diseñador generar los scripts que crean la base de datos (el *model first* enfoque).

El método centrado en la base de datos utilizados en este tutorial, el siguiente paso es agregar una base de datos al sitio. La manera más fácil es descargar el proyecto que acompaña a este tutorial. A continuación, haga clic en el *aplicación\_datos* carpeta, seleccione **Agregar elemento existente**y seleccione el *School.mdf* archivo de base de datos desde el proyecto descargado.

Una alternativa es seguir las instrucciones de [crear la base de datos de ejemplo School](https://msdn.microsoft.com/library/bb399731.aspx). Si descarga la base de datos o crearla, copie el *School.mdf* archivo desde la carpeta siguiente a la aplicación *aplicación\_datos* carpeta:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Esta ubicación de la *.mdf* archivo supone que está utilizando SQL Server 2008 Express.)

Si crea la base de datos desde una secuencia de comandos, realice los pasos siguientes para crear un diagrama de base de datos:

1. En **Explorador de servidores**, expanda **conexiones de datos**, expanda *School.mdf*, haga clic en **diagramas de base de datos**y seleccione **Agregar nuevo diagrama**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Seleccione todas las tablas y, a continuación, haga clic en **agregar**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crea un diagrama de base de datos que se muestra las tablas, columnas de las tablas y relaciones entre las tablas. Puede mover las tablas para organizarlos como desee.
3. Guarde el diagrama como "SchoolDiagram" y ciérrelo.

Si descargó el *School.mdf* archivo que acompaña a este tutorial, puede ver el diagrama de base de datos haciendo doble clic en **SchoolDiagram** en **diagramas de base de datos** en **Explorador de servidores**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

El diagrama es algo parecido a esto (las tablas deben estar en distintas ubicaciones de los que se muestran aquí):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Crear el modelo de datos de Entity Framework

Ahora puede crear un modelo de datos de Entity Framework desde esta base de datos. Se pudo crear el modelo de datos en la carpeta raíz de la aplicación, pero para este tutorial, podrá colocarlo en una carpeta denominada *DAL* (para la capa de acceso a datos).

En **el Explorador de soluciones**, agregue una carpeta de proyecto denominada *DAL* (asegúrese de que se encuentra en el proyecto, no en la solución).

Haga clic en el *DAL* carpeta y, a continuación, seleccione **agregar** y **nuevo elemento**. En **plantillas instaladas**, seleccione **datos**, seleccione el **ADO.NET Entity Data Model** plantilla, asígnele el nombre *SchoolModel.edmx*, y a continuación, haga clic en **agregar**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Esto inicia al Asistente para Entity Data Model. En el primer paso del asistente, la **generar desde la base de datos** está seleccionada de forma predeterminada. Haga clic en **Siguiente**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

En el **elegir la conexión de datos** paso, deje los valores predeterminados y haga clic en **siguiente**. La base de datos School está activada de forma predeterminada y la configuración de conexión se guarda en el *Web.config* archivo como **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

En el **elija los objetos de base de datos** paso del asistente, seleccione todas las tablas, excepto `sysdiagrams` (que se creó para el diagrama que se generó anteriormente) y, a continuación, haga clic en **finalizar**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Una vez que ha terminado de crear el modelo, Visual Studio muestra una representación gráfica de los objetos de Entity Framework (entidades) que corresponden a las tablas de base de datos. (Al igual que con el diagrama de base de datos, la ubicación de los elementos individuales podría ser diferente de lo que ve en esta ilustración. Puede arrastrar los elementos aproximadamente para que coincida con la ilustración, si desea.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Explorar el modelo de datos de Entity Framework

Puede ver que el diagrama de la entidad es muy similar al diagrama de base de datos, con un par de diferencias. Una diferencia es la adición de símbolos al final de cada asociación que indican el tipo de asociación (relaciones entre tablas se llaman las asociaciones de entidad en el modelo de datos):

- Una asociación uno a cero o uno está representada por "1" y "de 0.. 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    En este caso, un `Person` entidad puede o no se puede asociar un `OfficeAssignment` entidad. Un `OfficeAssignment` entidad debe estar asociada con un `Person` entidad. En otras palabras, un instructor puede o no se puede asignar a una oficina, y se puede asignar cualquier oficina a solo un instructor.
- Una asociación uno a varios está representada por "1" y "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    En este caso, un `Person` entidad puede o no puede tener asociada `StudentGrade` entidades. Un `StudentGrade` entidad debe estar asociada con uno `Person` entidad. `StudentGrade` las entidades representan realmente cursos inscritos en esta base de datos; Si un alumno está inscrito en un curso y todavía, no hay ningún nivel la `Grade` propiedad es null. En otras palabras, un alumno no estar inscritos en ningún curso aún, puede inscribirse en un curso o se puede inscribir en varios cursos. Cada curso en un curso inscrito se aplica a uno solo.
- Una asociación varios a varios es representada por "\*"y"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    En este caso, un `Person` entidad puede o no puede tener asociada `Course` entidades y viceversa, también es cierto: una `Course` entidad puede o no puede tener asociada `Person` entidades. En otras palabras, un instructor puede impartir varios cursos y un curso puede ser impartido por varios instructores. (En esta base de datos, esta relación se aplica solo a los instructores; no se vinculan a los alumnos a cursos. Los estudiantes están vinculados a cursos por la tabla StudentGrades.)

Otra diferencia entre el diagrama de base de datos y el modelo de datos es la más **las propiedades de navegación** sección para cada entidad. Una propiedad de navegación de una entidad hace referencia a las entidades relacionadas. Por ejemplo, el `Courses` propiedad en un `Person` entidad contiene una colección de todos los `Course` entidades que están relacionadas con esa `Person` entidad.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Sin embargo, otra diferencia entre el modelo de datos y la base de datos es la ausencia de la `CourseInstructor` tabla de asociación que se utiliza en la base de datos para vincular el `Person` y `Course` tablas en una relación de varios a varios. Las propiedades de navegación permiten obtener relacionados con `Course` entidades de la `Person` entidad relacionado `Person` entidades desde el `Course` entidad, por lo que no es necesario para representar la tabla de asociación del modelo de datos.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Para fines de este tutorial, supongamos que el `FirstName` columna de la `Person` tabla realmente contiene tanto una persona nombre y apellido. Para cambiar el nombre del campo para reflejar esto, pero el Administrador de base de datos (DBA) no es posible que desee cambiar la base de datos. Puede cambiar el nombre de la `FirstName` sin cambios de propiedad en el modelo de datos, dejando su base de datos equivalente.

En el diseñador, haga clic en **FirstName** en el `Person` entidad y, a continuación, seleccione **cambiar el nombre**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Escriba el nuevo nombre de "FirstMidName". Esto cambia la manera de que hacer referencia a la columna en el código sin cambiar la base de datos.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

El Explorador de modelos proporciona otra manera de ver la estructura de base de datos, la estructura del modelo de datos y la asignación entre ellos. Para verlo, haga clic en un área en blanco en entity designer y, a continuación, haga clic en **Explorador de modelos**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

El **Explorador de modelos** panel muestra una vista de árbol. (El **Explorador de modelos** panel se puede acoplar a la **el Explorador de soluciones** panel.) El **SchoolModel** nodo representa la estructura del modelo de datos y el **SchoolModel.Store** nodo representa la estructura de base de datos.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Expanda **SchoolModel.Store** para ver las tablas, expanda **tablas / vistas** para ver las tablas y, a continuación, expanda **curso** para ver las columnas dentro de una tabla.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Expanda **SchoolModel**, expanda **tipos de entidad**y, a continuación, expanda el **curso** nodo para ver las entidades y las propiedades dentro de las entidades.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

En el Diseñador de cualquiera o la **Explorador de modelos** panel puede ver cómo Entity Framework se relaciona con los objetos de los dos modelos. Haga clic en el `Person` entidad y seleccione **asignación de tabla**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Se abrirá el **detalles de Mapping** ventana. Tenga en cuenta que esta ventana le permite ver que la columna de base de datos `FirstName` se asigna a `FirstMidName`, que es lo que cambió de nombre para el modelo de datos.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework usa XML para almacenar información acerca de la base de datos, el modelo de datos y las asignaciones entre ellos. El *SchoolModel.edmx* archivo es realmente un archivo XML que contiene esta información. El diseñador presenta la información en un formato gráfico, pero también puede ver el archivo como XML con el botón secundario el *.edmx* archivo **el Explorador de soluciones**, haga clic en **abrir con**y seleccionando **Editor XML (texto)**. (El Diseñador de modelos de datos y un editor XML son sólo dos maneras diferentes de abrir y trabajar con el mismo archivo, por lo que no puede tener el Diseñador de abrir y abra el archivo en un editor XML al mismo tiempo).

Ahora ha creado un sitio Web, una base de datos y un modelo de datos. En el siguiente tutorial podrá empezar a trabajar con datos mediante el modelo de datos y el ASP.NET `EntityDataSource` control.

> [!div class="step-by-step"]
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-2.md)
