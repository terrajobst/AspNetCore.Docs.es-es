---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Code First Migrations e implementación con Entity Framework en una aplicación ASP.NET MVC | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio...
ms.author: riande
ms.date: 10/08/2018
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5c926c61cec5c7de43e2c3f0e377023b8ee799d0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913026"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Código de la primera implementación con Entity Framework en una aplicación ASP.NET MVC y migraciones
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Hasta ahora la aplicación se ejecutaba localmente en IIS Express en el equipo de desarrollo. Para que una aplicación real disponible para otras personas a través de Internet, tendrá que implementarlo en un proveedor de hospedaje web. En este tutorial, implementará la aplicación Contoso University en la nube en Azure.

El tutorial contiene las siguientes secciones:

- Habilitar migraciones de Code First. La característica de migraciones le permite cambiar el modelo de datos e implementar los cambios en producción mediante la actualización del esquema de base de datos sin tener que quitar y volver a crear la base de datos.
- Implementar en Azure. Este paso es opcional; se puede continuar con el resto de los tutoriales sin necesidad de implementar el proyecto.

Se recomienda que utilice un proceso de integración continua con control de código fuente para la implementación, pero en este tutorial no se trata esos temas. Para obtener más información, consulte el [control de código fuente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) y [integración continua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) capítulos de [Building Real-World Cloud Apps with Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

## <a name="enable-code-first-migrations"></a>Habilitación de migraciones de Code First

Al desarrollar una aplicación nueva, el modelo de datos cambia con frecuencia y, cada vez que lo hace, se deja de sincronizar con la base de datos. Ha configurado el Entity Framework para quitar y volver a crear la base de datos cada vez que cambie el modelo de datos automáticamente. Al agregar, quitar, o cambiar las clases de entidad o cambiar su `DbContext` clase, la próxima vez que ejecute la aplicación se elimina la base de datos automáticamente, se crea uno nuevo que coincide con el modelo y la inicializa con datos de prueba.

Este método para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción. Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que desea mantener, y no desea perder todo lo que cada vez que realice un cambio, como agregar una nueva columna. El [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) característica soluciona este problema habilitando el Code First actualizar el esquema de base de datos en lugar de quitar y volver a crear la base de datos. En este tutorial, va a implementar la aplicación y preparar para la deberá habilitar migraciones.

1. Deshabilitar el inicializador que configuró anteriormente al marcar como comentario o eliminar el `contexts` elemento que ha agregado al archivo Web.config de la aplicación.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. También en la aplicación *Web.config* de archivo, cambie el nombre de la base de datos en la cadena de conexión a ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Este cambio configura el proyecto para que la primera migración cree una nueva base de datos. Esto no es obligatorio, pero más adelante verá por qué es una buena idea.
3. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

1. En el `PM>` símbolo del sistema escriba los siguientes comandos:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    ![comando Enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    El `enable-migrations` comando crea un *migraciones* carpeta del proyecto ContosoUniversity, y lo coloca en esa carpeta un *Configuration.cs* archivo que se puede editar para configurar las migraciones.

    (Si se perdió el paso anterior que le indica que cambie el nombre de la base de datos, las migraciones se encuentra la base de datos existente y realizar automáticamente la `add-migration` comando. Eso está bien, simplemente significa que no ejecuta una prueba del código de migraciones antes de implementar la base de datos. Más adelante al ejecutar el `update-database` comando no sucederá nada porque ya existe la base de datos.)

    ![Carpeta de migraciones](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Al igual que la clase de inicializador que vio anteriormente, el `Configuration` clase incluye un `Seed` método.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    El propósito de la [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método es para que pueda insertar o actualizar los datos de prueba después de que Code First crea o actualiza la base de datos. Se llama al método cuando se crea la base de datos y cada vez que se actualiza el esquema de base de datos después de cambiar de un modelo de datos.

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

Al quitar y volver a crear la base de datos para cada cambio de modelo de datos, utilice la clase de inicializador `Seed` método para insertar datos de prueba, porque después de cada cambio de modelo se quita la base de datos y todos los datos de prueba se pierde. Con migraciones de Code First, prueba los datos se conservan después de realizar cambios de la base de datos, por lo que incluso los datos de prueba de la [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método normalmente no es necesario. De hecho, no desea que el `Seed` método para insertar datos de prueba si va a usar las migraciones para implementar la base de datos en producción, porque el `Seed` método se ejecutará en producción. En ese caso, desea que el `Seed` método para insertar en la base de datos solo los datos que necesita en producción. Por ejemplo, es posible que desee incluir nombres de departamento real en la base de datos la `Department` tabla cuando la aplicación esté disponible en producción.

Para este tutorial, usará las migraciones para la implementación, pero su `Seed` método insertará datos de prueba de todos modos para que resulte más fácil ver cómo funciona la funcionalidad de la aplicación sin tener que insertar manualmente una gran cantidad de datos.

1. Reemplace el contenido de la *Configuration.cs* archivo con el código siguiente, que carga los datos de prueba en la nueva base de datos.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    El [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método toma el objeto de contexto de base de datos como un parámetro de entrada y el código en el método utiliza ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, los agrega a la correspondiente [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) propiedad y, a continuación, guarda los cambios en la base de datos. No es necesario llamar a la [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método después de cada grupo de entidades, como se hizo aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras se está escribiendo el código en la base de datos.

    Algunas de las instrucciones que insertan datos utilizan el [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método para realizar una operación "upsert". Dado que el `Seed` método se ejecuta cada vez que ejecute la `update-database` comando, normalmente después de cada migración, simplemente no se puede insertar datos, porque las filas que se está intentando agregar ya estarán allí después de la primera migración que crea la base de datos. La operación "upsert" evita los errores que sucedería si se intenta insertar una fila que ya existe, pero ***invalida*** cualquier cambio en los datos que ha realizado durante las pruebas de la aplicación. Con datos de prueba en algunas tablas puede no ser conveniente que esto ocurra: en algunos casos cuando cambien los datos durante las pruebas que desee los cambios tras las actualizaciones de la base de datos. En ese caso en el que desea realizar una operación de inserción condicional: insertar una fila solo si ya no existe. El método de inicialización usa ambos enfoques.

    El primer parámetro pasado a la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método especifica la propiedad que se utilizará para comprobar si ya existe una fila. Para los datos de estudiante de prueba que se va a proporcionar, el `LastName` propiedad puede usarse para este propósito, puesto que cada apellido en la lista es único:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Este código supone que los apellidos son únicos. Si agrega manualmente un alumno con un nombre último duplicado, obtendrá la próxima vez que se realiza una migración a la siguiente excepción:

    **La secuencia contiene más de un elemento**

    Para obtener información sobre cómo controlar los datos redundantes, como dos estudiantes denominados "Alexander Carson", consulte [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) en el blog de Rick Anderson. Para obtener más información sobre la `AddOrUpdate` método, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julie.

    El código que crea `Enrollment` entidades se supone que tiene el `ID` valor en las entidades en el `students` colección, aunque no establece esta propiedad en el código que crea la colección.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Puede usar el `ID` propiedad aquí porque el `ID` valor se establece cuando se llama a `SaveChanges` para el `students` colección. EF obtiene automáticamente el valor de clave principal cuando inserta una entidad en la base de datos, y actualiza el `ID` propiedad de la entidad en la memoria.

    El código que agrega cada `Enrollment` entidad a la `Enrollments` no usa el conjunto de entidades la `AddOrUpdate` método. Comprueba si una entidad ya existe y que inserta la entidad si no existe. Este enfoque, conservará los cambios que realice en un nivel de inscripción mediante el uso de la interfaz de usuario de la aplicación. El código establece bucles en cada miembro de la `Enrollment` [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) y si la inscripción no se encuentra en la base de datos, agrega la inscripción a la base de datos. La primera vez que actualice la base de datos, la base de datos estará vacío, por lo que se va a agregar cada inscripción.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Compile el proyecto.

### <a name="execute-the-first-migration"></a>Ejecutar la primera migración

Cuando ejecutó el `add-migration` de comandos, las migraciones generan el código que desea crear la base de datos desde el principio. Este código también está disponible en el *migraciones* carpeta, en el archivo denominado  *&lt;timestamp&gt;\_InitialCreate.cs*. El `Up` método de la `InitialCreate` clase crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos, y el `Down` método elimina.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.

Se trata de la migración inicial que se creó cuando se escribió el `add-migration InitialCreate` comando. El parámetro (`InitialCreate` en el ejemplo) se usa para el archivo de nombre y puede ser lo que quiera; normalmente, elegir una palabra o frase que resuma lo que se hace en la migración. Por ejemplo, podría denominar una migración posterior &quot;AddDepartmentTable&quot;.

Si creó la migración inicial cuando la base de datos ya existía, se genera el código de creación de la base de datos pero no es necesario ejecutarlo porque la base de datos ya coincide con el modelo de datos. Al implementar la aplicación en otro entorno donde la base de datos todavía no existe, se ejecutará este código para crear la base de datos, por lo que es recomendable probarlo primero. Por eso ha cambiado el nombre de la base de datos en la cadena de conexión anteriormente&mdash;para que las migraciones puedan crear uno nuevo desde cero.

1. En el **Package Manager Console** ventana, escriba el siguiente comando:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    El `update-database` comando ejecuta el `Up` método para crear la base de datos y, a continuación, se ejecuta el `Seed` método para rellenar la base de datos. El mismo proceso se ejecutará automáticamente en producción después de implementar la aplicación, como verá en la sección siguiente.
2. Use **Explorador de servidores** para inspeccionar la base de datos como hizo en el primer tutorial y ejecutar la aplicación para comprobar que todo funciona igual que antes.

## <a name="deploy-to-azure"></a>Implementar en Azure

Hasta ahora la aplicación se ejecutaba localmente en IIS Express en el equipo de desarrollo. Para que esté disponible para otras personas a través de Internet, tendrá que implementarlo en un proveedor de hospedaje web. En esta sección del tutorial, va a implementar en Azure. Esta sección es opcional; puede omitir este paso y continuar con el tutorial siguiente, o puede adaptar las instrucciones de esta sección para otro proveedor de hospedaje de su elección.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Usar migraciones de Code First para implementar la base de datos

Para implementar la base de datos, deberá usar migraciones de Code First. Cuando se crea el perfil de publicación que usar para configurar las opciones de implementación desde Visual Studio, seleccionará una casilla denominada **Actualizar base de datos**. Esta configuración hace que el proceso de implementación configurar automáticamente la aplicación *Web.config* de archivos en el servidor de destino para que utilice Code First la `MigrateDatabaseToLatestVersion` clase de inicializador.

Visual Studio no hace nada con la base de datos durante el proceso de implementación mientras se está copiando el proyecto en el servidor de destino. Cuando se ejecuta la aplicación implementada y tiene acceso a la base de datos por primera vez después de la implementación, Code First comprueba si la base de datos coincide con el modelo de datos. Si se produce un error de coincidencia, Code First crea automáticamente la base de datos (si aún no existe) o actualiza el esquema de base de datos a la versión más reciente (si existe una base de datos, pero no coincide con el modelo). Si la aplicación implementa un migraciones `Seed` método, se ejecuta el método después de la base de datos se crea o actualiza el esquema.

Las migraciones `Seed` método inserta datos de prueba. Si se implementa en un entorno de producción, tendría que cambiar el `Seed` método por lo que TI sólo inserta los datos que se van a insertarse en la base de datos de producción. Por ejemplo, en el modelo de datos actual es posible que desee tener cursos real pero estudiantes ficticios en la base de datos de desarrollo. Puede escribir un `Seed` método para cargar ambos en el desarrollo y, a continuación, marque como comentario los alumnos ficticios antes de implementar en producción. También se puede escribir un `Seed` método para cargar solo los cursos y escribir manualmente los alumnos ficticios en la base de datos de prueba mediante el uso de la interfaz de usuario de la aplicación.

### <a name="get-an-azure-account"></a>Obtener una cuenta de Azure

Necesitará una cuenta de Azure. Si aún no tiene uno, pero tiene una suscripción de Visual Studio, puede [activar los beneficios de suscripción](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). En caso contrario, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Crear un sitio web y una base de datos SQL en Azure

La aplicación web en Azure se ejecutará en un entorno de hospedaje compartido, lo que significa que se ejecuta en máquinas virtuales (VM) que se comparten con otros clientes de Azure. Un entorno de hospedaje compartido es una manera de bajo costo para empezar a trabajar en la nube. Más adelante, si aumenta el tráfico web, la aplicación puede escalar para satisfacer las necesidades de forma que se ejecutan en máquinas virtuales dedicadas. Para obtener más información sobre opciones de precios para Azure App Service, lea [precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).

Implementará la base de datos a Azure SQL database. SQL database es un servicio de base de datos relacional basado en la nube que se basa en tecnologías de SQL Server. Herramientas y aplicaciones que funcionan con SQL Server también funcionan con SQL database.

1. En el [Portal de administración de Azure](https://portal.azure.com), elija **crear un recurso** en la izquierda ficha y, a continuación, elija **ver todo** en el **New** panel (o *hoja*) para ver todos los recursos disponibles. Elija **aplicación Web + SQL** en el **Web** sección de la **todo** hoja. Por último, elija **crear**.

    ![Crear un recurso en Azure portal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   El formulario para crear un nuevo **nueva aplicación Web + SQL** abre recursos.

2. Escriba una cadena en el **nombre de la aplicación** cuadro que se usará como la dirección URL única para la aplicación. La dirección URL completa consistirá en lo que escriba aquí además el dominio predeterminado de Azure App Services (. azurewebsites.net). Si el **nombre de la aplicación** ya está en uso, el asistente le proporcionará una roja *el nombre de la aplicación no está disponible* mensaje. Si el **nombre de la aplicación** está disponible, verá una marca de verificación verde.

    ![Creación de vínculo de la base de datos en el Portal de administración](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-web-app-sql-resource.png)

3. En el **suscripción** , seleccione la suscripción de Azure en el que desea que el **App Service** residir.

4. En el **grupo de recursos** cuadro de texto, elija un grupo de recursos o cree uno nuevo. Esta configuración especifica el sitio web se ejecutará en el centro de datos. Para obtener más información acerca de los grupos de recursos, consulte [grupos de recursos](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Cree un nuevo **Plan de App Service** haciendo clic en el *sección App Service*, **crear nuevo**y rellene **plan de App Service** (puede ser mismo nombre que App Service), **ubicación**, y **tarifa** (hay una opción gratuita).

6. Haga clic en **base de datos SQL**y, a continuación, elija **crear una nueva base de datos** o seleccione una base de datos existente.

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/new-sql-database.png)

7. En el **nombre** , escriba un nombre para la base de datos.
8. Haga clic en el **TargetServer** cuadro y, a continuación, seleccione **crear un nuevo servidor**. Como alternativa, si ha creado anteriormente un servidor, puede seleccionar ese servidor en la lista de servidores disponibles.
9. Elija **tarifa** sección, elija *gratis*. Si se necesitan recursos adicionales, la base de datos se puede escalar verticalmente en cualquier momento. Para más información sobre los precios de SQL Azure, consulte [precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modificar [intercalación](/sql/relational-databases/collations/collation-and-unicode-support) según sea necesario.
11. Especifique un administrador **SQL Admin Username** y **contraseña de administrador de SQL**.

   - Si seleccionó **el servidor de base de datos SQL nueva**, defina un nuevo nombre y una contraseña que usará más adelante cuando acceda a la base de datos.
   - Si ha seleccionado un servidor que creó anteriormente, escriba las credenciales para ese servidor.

12. Recopilación de telemetría puede habilitarse para App Service con Application Insights. Con muy poca configuración, Application Insights recopila eventos valiosa, excepción, dependencia, solicitud y la información de seguimiento. Para obtener más información acerca de Application Insights, consulte [Azure Monitor](https://azure.microsoft.com/services/monitor/).
13. Haga clic en **crear** en la parte inferior para indicar que haya terminado.

    El Portal de administración que se devuelve a la página panel y el **notificaciones** área situada en la parte superior de la página de muestra que se está creando el sitio. Después de un tiempo (normalmente menos de un minuto), hay una notificación de que la implementación se realizó correctamente. En la barra de navegación a la izquierda, el nuevo servicio de aplicación aparece en el **App Services** sección y la nueva base de datos SQL aparece en la **bases de datos SQL** sección.

### <a name="deploy-the-app-to-azure"></a>Implementación de la aplicación en Azure

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.

    ![Publicar el elemento de menú en el Explorador de soluciones](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

2. En el **elegir un destino de publicación** página, elija **App Service** y, a continuación, **seleccionar existente**y, a continuación, elija **publicar**.

    ![Seleccionar una página de destino de publicación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Si anteriormente no ha agregado la suscripción de Azure en Visual Studio, realice los pasos en la pantalla. Estos pasos habilitan Visual Studio para conectarse a su suscripción de Azure hasta que la lista de **App Services** incluirá su sitio web.

4. En el **App Service** página, seleccione el **suscripción** agrega el servicio de aplicación. En **vista**, seleccione **grupo de recursos**. Expanda el grupo de recursos que agregó el servicio de aplicación y, a continuación, seleccione el servicio de aplicación. Elija **Aceptar** para publicar la aplicación.

    ![Seleccione el servicio de aplicación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/app-service-page.png)

5. El **salida** ventana muestra qué acciones de implementación se realizaron e informa la correcta finalización de la implementación.

    ![Ventana de salida de informes de implementación correcta](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-output.png)

6. Tras una implementación correcta, el explorador predeterminado se abre automáticamente en la dirección URL del sitio web implementado.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    La aplicación se ejecuta ahora en la nube.

En este momento, el *SchoolContext* base de datos se ha creado en la base de datos SQL de Azure porque seleccionó **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**. El *Web.config* en el sitio web implementado, ha sido modificado para que la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador ejecuta la primera vez que el código lee o escribe datos en la base de datos (lo que sucedió Si seleccionó la **estudiantes** pestaña):

![Fragmento del archivo Web.config](https://asp.net/media/4367421/mig.png)

El proceso de implementación también crea una nueva cadena de conexión *(SchoolContext\_DatabasePublish*) para migraciones de Code First que se usará para actualizar el esquema de base de datos y la propagación de la base de datos.

![Cadena de conexión en el archivo Web.config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Puede encontrar la versión implementada del archivo Web.config en su propio equipo en *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Puede acceder a implementado *Web.config* propio archivo mediante FTP. Para obtener instrucciones, consulte [implementación Web de ASP.NET con Visual Studio: implementar una actualización de código](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Siga las instrucciones que comienzan con "para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña."

> [!NOTE]
> La aplicación web no implementa la seguridad, por lo que cualquier persona que encuentre la dirección URL puede cambiar los datos. Para obtener instrucciones sobre cómo proteger el sitio web, consulte [implementar una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL database en Azure](/aspnet/core/security/authorization/secure-data). Puede impedir que otras personas mediante el sitio, detenga el servicio mediante el Portal de administración de Azure o **Explorador de servidores** en Visual Studio.

![Detener el elemento de menú de servicio de aplicación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Escenarios de migración avanzada

Si implementa una base de datos mediante la ejecución de migraciones automáticamente como se muestra en este tutorial, y va a implementar en un sitio web que se ejecuta en varios servidores, podría obtener varios servidores al intentar ejecutar migraciones al mismo tiempo. Las migraciones son atómicas, por lo que si dos servidores intentan ejecutar la migración del mismo, uno se realizará correctamente y el otro se producirá un error (suponiendo que las operaciones no se puede realizar dos veces). En ese escenario si desea evitar estos problemas, puede llamar manualmente a las migraciones y configurar su propio código para que solo sucede una vez. Para obtener más información, consulte [ejecución y las migraciones de secuencias de comandos desde el código](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) en el blog de Rowan Miller y [Migrate.exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (para ejecutar las migraciones desde la línea de comandos).

Para obtener información sobre otros escenarios de migración, consulte [migraciones Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inicializadores de primer código

En la sección de implementación, vio la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador que se va a usar. Código primero también proporciona otros inicializadores, incluidos [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valor predeterminado), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que usó anteriormente) y [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). El `DropCreateAlways` inicializador puede ser útil para configurar las condiciones para las pruebas unitarias. También puede escribir a sus propios inicializadores, y se puede llamar a un inicializador explícitamente si no desea esperar hasta que la aplicación lee o escribe en la base de datos.

Para obtener más información sobre los inicializadores, vea [descripción inicializadores de base de datos en Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) y el capítulo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) , Julie Lerman y Rowan Miller.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a habilitar migraciones e implementar la aplicación. En el siguiente, comenzará examinando temas más avanzados expandiendo el modelo de datos.

Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar.

Pueden encontrar vínculos a otros recursos de Entity Framework en [acceso a datos de ASP.NET - recursos recomendados](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Anterior](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Siguiente](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
