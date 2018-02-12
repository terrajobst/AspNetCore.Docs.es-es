---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Implementación de Web ASP.NET con Visual Studio: implementar una actualización de la base de datos | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Implementación de Web ASP.NET con Visual Studio: implementar una actualización de la base de datos
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

En este tutorial, realice un cambio de base de datos y los cambios de código relacionado, comprobar los cambios en Visual Studio, y luego implementar la actualización en los entornos de prueba, ensayo y producción.

En primer lugar, el tutorial muestra cómo actualizar una base de datos que se administra mediante migraciones de Code First y, a continuación, más adelante muestra cómo actualizar una base de datos mediante el proveedor dbDacFx.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Implementar una actualización de la base de datos mediante el uso de migraciones de Code First

En esta sección, agregará una columna de fecha de nacimiento a la `Person` la clase base para la `Student` y `Instructor` entidades. A continuación, actualice la página que muestra los datos de instructor para que se muestre la nueva columna. Por último, implementa los cambios en la prueba, ensayo y producción.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Agregar una columna a una tabla en la base de datos de aplicación

1. En el *ContosoUniversity.DAL* proyecto abierto *Person.cs* y agregue la siguiente propiedad al final de la `Person` clase (debe haber dos sigue llaves de cierre):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    A continuación, actualice el `Seed` método por lo que proporciona un valor para la nueva columna. Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye información de fecha de nacimiento:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compile la solución y, a continuación, abra el **Package Manager Console** ventana. Asegúrese de que ContosoUniversity.DAL todavía está seleccionado como el **proyecto predeterminado**.
3. En el **Package Manager Console** ventana, seleccione **ContosoUniversity.DAL** como el **proyecto predeterminado**y, a continuación, escriba el comando siguiente:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` (clase) y en el `Up` método puede ver el código que crea la nueva columna. El `Up` método crea la columna de la hora de implementar el cambio y el `Down` método elimina la columna cuando se están Revirtiendo el cambio.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compile la solución y, a continuación, escriba el siguiente comando en el **Package Manager Console** ventana (asegúrese de que todavía esté seleccionado el proyecto de ContosoUniversity.DAL):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework se ejecuta el `Up` método y, a continuación, se ejecuta el `Seed` método.

### <a name="display-the-new-column-in-the-instructors-page"></a>Mostrar la nueva columna en la página de instructores

1. En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para mostrar la fecha de nacimiento. Agregarlo entre los de asignación de fecha y la oficina de contratación:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Si la sangría del código obtiene la sincronización, puede presionar CTRL-K y, a continuación, CTRL+D para volver a formatear automáticamente el archivo.)
2. Ejecute la aplicación y haga clic en el **instructores** vínculo.

    Cuando se carga la página, verá que tiene el nuevo campo de fecha de nacimiento.

    ![Página de instructores con fecha de nacimiento](deploying-a-database-update/_static/image2.png)
3. Cierre el explorador.

### <a name="deploy-the-database-update"></a>Implementar la actualización de la base de datos

1. En **el Explorador de soluciones** seleccione el proyecto ContosoUniversity.
2. En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web**. (Si se deshabilita la barra de herramientas, seleccione el proyecto ContosoUniversity en **el Explorador de soluciones**.)

    Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal.
3. Ejecute el **instructores** página para comprobar que la actualización se ha implementado correctamente.

    Cuando la aplicación intenta tener acceso a la base de datos de esta página, Code First actualiza el esquema de base de datos y se ejecuta el `Seed` método. Cuando se muestra la página, verá el esperado **la fecha de nacimiento** columna con fechas en ella.
4. En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **ensayo** perfil de publicación y, a continuación, haga clic en **Publicar Web**.
5. Ejecute el **instructores** página de almacenamiento provisional para comprobar que la actualización se ha implementado correctamente.
6. En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **producción** perfil de publicación y, a continuación, haga clic en **Publicar Web**.
7. Ejecute el **instructores** página en producción para comprobar que la actualización se ha implementado correctamente.

    Para una actualización de la aplicación de producción real que incluye un cambio de base de datos normalmente, también se podría pasar la aplicación sin conexión durante la implementación mediante *aplicación\_offline.htm*, tal y como se vio en el tutorial anterior.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Implementar una actualización de la base de datos mediante el proveedor dbDacFx

En esta sección, agregará un *comentarios* columna a la *usuario* de la tabla en la base de datos de pertenencia y cree una página que permite mostrar y editar comentarios para cada usuario. A continuación, implementar los cambios a la prueba, ensayo y producción.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Agregar una columna a una tabla en la base de datos de pertenencia

1. En Visual Studio, abra **Explorador de objetos de SQL Server**.
2. Expanda **(localdb) \v11.0**, expanda **bases de datos**, expanda **aspnet ContosoUniversity** (no **aspnet-ContosoUniversity-Prod**) y, a continuación, expanda **tablas**.

    Si no ve **(localdb) \v11.0** en el **SQL Server** nodo, haga clic en el **SQL Server** nodo y haga clic en **agregar SQL Server**. En el **conectar al servidor** cuadro de diálogo Escriba *(localdb) \v11.0* como el **nombre del servidor**y, a continuación, haga clic en **conectar**.

    Si no ve **aspnet ContosoUniversity**, ejecute el proyecto e inicie sesión con la *administración* credenciales (contraseña es *devpwd*) y, a continuación, actualice el  **Explorador de objetos de SQL Server** ventana.
3. Haga clic en el **usuarios** de tabla y, a continuación, haga clic en **Diseñador de vistas**.

    ![Diseñador de vistas SSOX](deploying-a-database-update/_static/image3.png)
4. En el diseñador, agregue un *comentarios* columna y hacerla *nvarchar (128)* y acepta valores NULL y, a continuación, haga clic en **actualización**.

    ![Agregar columna de comentarios](deploying-a-database-update/_static/image4.png)
5. En el **actualizaciones de base de datos de vista previa** cuadro, haga clic en **Actualizar base de datos**.

    ![Vista previa de actualizaciones de base de datos](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Crear una página para mostrar y editar la nueva columna

1. En **el Explorador de soluciones**, haga clic en el **cuenta** carpeta en el proyecto ContosoUniversity, haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento** .
2. Crear un nuevo **formulario de Web usar la página maestra** y asígnele el nombre *UserInfo.aspx*. Acepte el valor predeterminado *Site.Master* archivo como la página maestra.
3. Copie el siguiente marcado en el `MainContent` `Content` elemento (la última de las 3 `Content` elementos):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Haga clic en el *UserInfo.aspx* página y haga clic en **ver en el explorador**.
5. Inicie sesión con su *administración* las credenciales de usuario (contraseña es *devpwd*) y agregar algunos comentarios a un usuario para comprobar que la página funciona correctamente.

    ![Página de información de usuario](deploying-a-database-update/_static/image6.png)
6. Cierre el explorador.

## <a name="deploy-the-database-update"></a>Implementar la actualización de la base de datos

Para implementar mediante el proveedor dbDacFx, basta con que seleccione el **Actualizar base de datos** opción en el perfil de publicación. Sin embargo, para la implementación inicial cuando se usa esta opción también configuró algunos scripts SQL adicionales para ejecutar: los que todavía están en el perfil y deberá volver a impedir que se ejecute de nuevo.

1. Abra la **Publicar Web** Asistente haciendo clic en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Seleccione el **prueba** perfil.
3. Haga clic en el **configuración** ficha.
4. En **DefaultConnection**, seleccione **Actualizar base de datos**.
5. Deshabilitar las secuencias de comandos adicionales que se configuró para ejecutarse para la implementación inicial:

    1. Haga clic en **configurar actualizaciones de base de datos**.
    2. En el **configurar actualizaciones de base de datos** cuadro de diálogo, desactive las casillas de verificación junto a *Grant.sql* y *dev.sql de datos de aspnet*.
    3. Haga clic en **Cerrar**.
6. Haga clic en el **vista previa** ficha.
7. En **bases de datos** y a la derecha del **DefaultConnection**, haga clic en el **base de datos de vista previa** vínculo.

    ![Vista previa de la base de datos](deploying-a-database-update/_static/image7.png)

    La ventana de vista previa muestra la secuencia de comandos que se ejecutará en la base de datos de destino para realizar ese esquema de base de datos coincide con el esquema de la base de datos de origen. El script incluye un comando ALTER TABLE que agrega la nueva columna.
8. Cerrar la **vista previa de la base de datos** cuadro de diálogo y, a continuación, haga clic en **publicar**.

    Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal.
9. Ejecute la página de información de usuario (agregar *Account/UserInfo.aspx* a la dirección URL de la página de inicio) para comprobar que la actualización se ha implementado correctamente. Deberá volver a iniciar sesión escribiendo *administración* y *devpwd*.

    Datos de tablas no se implementan de forma predeterminada y no ha configurado un script de implementación de datos que se ejecuta, por lo que no encontrará el comentario que ha agregado en el desarrollo. Ahora puede agregar un nuevo comentario de ensayo para comprobar que el cambio se implementó en la base de datos y la página funciona correctamente.
10. Siga el mismo procedimiento para implementar en ensayo y producción.

    No olvide deshabilitar las secuencias de comandos adicionales. La única diferencia en comparación con el perfil de prueba es que se deshabilitará solo un script en el almacenamiento provisional y perfiles de producción porque se configuraron para ejecutar solo *aspnet-prod-data.sql*.

    Las credenciales de ensayo y producción son admin y prodpwd.

    Para una actualización de la aplicación de producción real que incluye un cambio de base de datos se tardaría normalmente también la aplicación sin conexión durante la implementación mediante la carga de *aplicación\_offline.htm* antes de la publicación y la eliminación a continuación, tal y como se vio en [el tutorial anterior](deploying-a-code-update.md).

## <a name="summary"></a>Resumen

Ahora que ha implementado una actualización de la aplicación que incluye un cambio de base de datos mediante el proveedor dbDacFx y migraciones de Code First.

![Página de instructores con fecha de nacimiento](deploying-a-database-update/_static/image8.png)

![Página de información de usuario](deploying-a-database-update/_static/image9.png)

El siguiente tutorial muestra cómo ejecutar las implementaciones mediante el uso de la línea de comandos.

>[!div class="step-by-step"]
[Anterior](deploying-a-code-update.md)
[Siguiente](command-line-deployment.md)
