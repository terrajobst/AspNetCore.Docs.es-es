---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implementación Web de ASP.NET con Visual Studio: preparación para la implementación de la base de datos | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388546"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implementación Web de ASP.NET con Visual Studio: preparación para la implementación de la base de datos
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo obtener el proyecto listo para la implementación de la base de datos. La estructura de base de datos y algunos (no todos) de los datos en dos de la aplicación se deben implementar las bases de datos en entornos de prueba, ensayo y producción.

Normalmente, al desarrollar una aplicación, escriba datos de prueba en una base de datos que no desea implementar en un sitio activo. Sin embargo, también podría tener algunos datos de producción que desea implementar. En este tutorial podrá configurar el proyecto de Contoso University y preparar scripts SQL para que los datos correctos se incluye cuando se implementa.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La aplicación de ejemplo usa SQL Server Express LocalDB. SQL Server Express es la edición gratuita de SQL Server. Normalmente se usa durante el desarrollo porque se basa en el mismo motor de base de datos como versiones completas de SQL Server. Puede probar con SQL Server Express y estar seguro de que la aplicación comportará igual en producción, con algunas excepciones para las características que varían entre las ediciones de SQL Server.

LocalDB es un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Normalmente, los archivos de base de datos de LocalDB se mantienen en el *aplicación\_datos* carpeta de un proyecto web. La característica de instancia de usuario en SQL Server Express también le permite trabajar con *.mdf* archivos, pero la característica de instancia de usuario está en desuso; por lo tanto, se recomienda LocalDB para trabajar con *.mdf* archivos.

Normalmente, SQL Server Express no se utiliza para las aplicaciones web de producción. En particular LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para trabajar con IIS.

En Visual Studio 2012, LocalDB está instalado de forma predeterminada con Visual Studio. En Visual Studio 2010 y versiones anteriores, SQL Server Express (sin LocalDB) se instala de forma predeterminada con Visual Studio; que es ¿por qué ha instalado como uno de los requisitos previos en [el primer tutorial de esta serie](introduction.md).

Para obtener más información acerca de las ediciones de SQL Server, incluido LocalDB, vea los siguientes recursos [trabajar con bases de datos de SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework y proveedores universales

Obtener acceso a la base de datos, la aplicación Contoso University requiere el siguiente software debe implementarse con la aplicación porque no está incluido en .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que el sistema de pertenencia ASP.NET usar Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Dado que este software se incluye en los paquetes de NuGet, el proyecto ya está configurado para que los ensamblados necesarios se implementan con el proyecto. (Los vínculos apuntan a las versiones actuales de estos paquetes, que podrían ser más recientes que está instalado en el proyecto de inicio que descargó para este tutorial).

Si va a implementar en un proveedor de hospedaje de terceros en lugar de Azure, asegúrese de que use Entity Framework 5.0 o posterior. Las versiones anteriores de migraciones de Code First requieren plena confianza y la mayoría de los proveedores de hospedaje ejecutarán la aplicación en Medium Trust. Para obtener más información sobre el nivel de confianza medio, consulte el [implementar en IIS como un entorno de prueba](deploying-to-iis.md) tutorial.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar las migraciones de Code First para la implementación de la base de datos de aplicación

La base de datos de la aplicación Contoso University se administra mediante Code First y va a implementar mediante el uso de migraciones de Code First. Para obtener información general de implementación de la base de datos mediante el uso de migraciones de Code First, consulte [el primer tutorial de esta serie](introduction.md).

Al implementar una base de datos de aplicación, normalmente no basta con implementar la base de datos de desarrollo con todos los datos en ella a producción, porque muchos de los datos en él es probablemente existe solo para fines de prueba. Por ejemplo, los nombres de estudiante de una base de datos de prueba son ficticios. Por otro lado, a menudo no se puede implementar la estructura de base de datos con ningún dato en él en absoluto. Algunos de los datos en la base de datos de prueba podrían ser datos reales y deben existir cuando los usuarios pueden comenzar a usar la aplicación. Por ejemplo, la base de datos podría tener una tabla que contiene los valores de calificación válido o nombres de departamento real.

Para simular este escenario habitual, configurará un migraciones de Code First `Seed` método que se inserta en la base de datos solo los datos que se desean estar allí en producción. Esto `Seed` método no debe insertar datos de prueba porque se ejecutarán en producción después de Code First crea la base de datos en producción.

En versiones anteriores de Code First antes de que se publicó las migraciones, era habitual para `Seed` métodos para insertar datos de prueba también, porque con cada cambio de modelo durante el desarrollo de la base de datos tenía que eliminar y volver a crear desde cero por completo. Con migraciones de Code First, prueba los datos se conservan después de realizar cambios de la base de datos, por lo que incluso los datos de prueba de la `Seed` método no es necesario. El proyecto que ha descargado usa el método de inclusión de todos los datos en el `Seed` método de una clase de inicializador. En este tutorial podrá deshabilitar esa clase de inicializador y `enable Migrations. Then you'll update the `inicialización ' método en la configuración de las migraciones de clase para que inserte solo datos que se van a insertarse en producción.

El siguiente diagrama muestra el esquema de la base de datos de aplicación:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Estos tutoriales, supondremos que el `Student` y `Enrollment` tablas deben estar vacías cuando el sitio se implementó por primera vez. Las demás tablas contienen datos que tiene que cargarse previamente cuando la aplicación esté activa.

### <a name="disable-the-initializer"></a>Deshabilitar el inicializador

Puesto que va a usar migraciones de Code First, no es necesario usar el `DropCreateDatabaseIfModelChanges` inicializador Code First. El código para este inicializador está en el *SchoolInitializer.cs* archivo en el proyecto ContosoUniversity.DAL. Un valor en el `appSettings` elemento de la *Web.config* Este inicializador para ejecutarse cada vez que la aplicación intenta obtener acceso a la base de datos por primera vez hace que el archivo:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra la aplicación *Web.config* de archivos y quitar o marque como comentario el `add` elemento que especifica la clase de inicializador Code First. El `appSettings` elemento ahora este aspecto:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Otra manera de especificar una clase de inicializador es hacerlo mediante una llamada a `Database.SetInitializer` en el `Application_Start` método en el *Global.asax* archivo. Si va a habilitar las migraciones en un proyecto que usa dicho método para especificar al inicializador, quitar esa línea de código.


> [!NOTE]
> Si usa Visual Studio 2013, agregue los siguientes pasos entre los pasos 2 y 3: (a) en la PMC escriba "update-package entityframework-versión 6.1.1" para obtener la versión actual de EF. A continuación (b) generar el proyecto para obtener una lista de errores de compilación y corregirlos. Eliminar instrucciones using para espacios de nombres que ya no existen, haga clic en y haga clic en resolver para agregar instrucciones using donde son necesarios y cambie las apariciones de System.Data.EntityState a System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Habilitar migraciones de Code First

1. Asegúrese de que el proyecto ContosoUniversity (no ContosoUniversity.DAL) esté establecido como proyecto de inicio. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y seleccione **establecer como proyecto de inicio**. Migraciones de Code First buscará en el proyecto de inicio para buscar la cadena de conexión de base de datos.
2. Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** (o **Administrador de paquetes de NuGet**) y, a continuación, **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. En la parte superior de la **Package Manager Console** ventana Seleccione ContosoUniversity.DAL como proyecto predeterminado y, a continuación, at el `PM>` símbolo del sistema escriba "enable-migrations".

    ![comando Enable-migrations](preparing-databases/_static/image4.png)

    (Si se produce un error que dice que el *enable-migrations* no se reconoce el comando, escriba el comando *update-package EntityFramework-reinstalar* e inténtelo de nuevo.)

    Este comando crea un *migraciones* carpeta del proyecto ContosoUniversity.DAL, y lo coloca en esa carpeta dos archivos: un *Configuration.cs* archivo que puede usar para configurar las migraciones y un *InitialCreate.cs* archivo para la primera migración que crea la base de datos.

    ![Carpeta de migraciones](preparing-databases/_static/image5.png)

    Ha seleccionado el proyecto DAL en el **proyecto predeterminado** la lista desplegable de la **Package Manager Console** porque el `enable-migrations` comando debe ejecutarse en el proyecto que contiene el primer código clase de contexto. Cuando esa clase está en un proyecto de biblioteca de clases, migraciones de Code First busca la cadena de conexión de base de datos en el proyecto de inicio para la solución. En la solución ContosoUniversity, el proyecto web se ha establecido como proyecto de inicio. Si no desea designar el proyecto que tiene la cadena de conexión como proyecto de inicio en Visual Studio, puede especificar el proyecto de inicio del comando de PowerShell. Para ver la sintaxis del comando, escriba el comando `get-help enable-migrations`.

    El `enable-migrations` comando crea automáticamente la primera migración porque ya existe la base de datos. Una alternativa es que las migraciones de crear la base de datos. Para ello, utilice **Explorador de servidores** o **Explorador de objetos de SQL Server** para eliminar la base de datos ContosoUniversity antes de habilitar las migraciones. Después de habilitar las migraciones, cree manualmente la primera migración escribiendo el comando "add-migration InitialCreate". A continuación, puede crear la base de datos escribiendo el comando "update-database".

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

En este tutorial agregará datos fijos agregando código a la `Seed` método de las migraciones de Code First `Configuration` clase. El código llama a migraciones de First el `Seed` método después de cada migración.

Puesto que el `Seed` método se ejecuta después de cada migración, los datos ya existe en las tablas después de la primera migración. Para controlar esta situación usará el `AddOrUpdate` método para actualizar las filas que ya se han insertado o insertan en el caso de que no existan. El `AddOrUpdate` método podría no ser la mejor opción para su escenario. Para obtener más información, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julie.

1. Abra el *Configuration.cs* de archivo y reemplace los comentarios en el `Seed` método con el código siguiente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Las referencias a `List` tiene líneas onduladas rojas debajo de ellos porque no tiene un `using` instrucción aún para su espacio de nombres. Haga clic en uno de las instancias de `List` y haga clic en **resolver**y, a continuación, haga clic en **using System.Collections.Generic**.

    ![Resolver con la instrucción using](preparing-databases/_static/image6.png)

    Esta selección de menú agrega el código siguiente a la `using` las instrucciones en la parte superior del archivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Presione CTRL-MAYÚS-B para compilar el proyecto.

Ahora el proyecto está listo para implementar el *ContosoUniversity* base de datos. Después de implementar la aplicación, la primera vez, ejecutarlo y navegar a una página que tiene acceso a la base de datos, Code First creará la base de datos y ejecutar este ejemplo `Seed` método.

> [!NOTE]
> Agregar código para el `Seed` método es una de las muchas maneras que puede insertar datos fijas en la base de datos. Una alternativa consiste en Agregar código a la `Up` y `Down` métodos de cada clase de migración. El `Up` y `Down` métodos contienen código que implementa los cambios de la base de datos. Verá ejemplos de ellos en el [implementar una actualización de la base de datos](deploying-a-database-update.md) tutorial.
> 
> También puede escribir código que se ejecuta instrucciones SQL mediante el uso de la `Sql` método. Por ejemplo, si fuese a agregar una columna de presupuesto a la tabla de departamento y quiere inicializar todos los presupuestos de departamento para los 1.000,00 dólares como parte de una migración, puede agregar la siguiente línea de código para el `Up` método para que la migración:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Crear secuencias de comandos para la implementación de la base de datos de pertenencia

La aplicación Contoso University usa la autenticación de formularios y el sistema de pertenencia a ASP.NET para autenticar y autorizar a los usuarios. El **actualización créditos** página solo es accesible para los usuarios que están en el rol de administrador.

Ejecute la aplicación y haga clic en **cursos**y, a continuación, haga clic en **actualización créditos**.

![Haga clic en créditos de actualización](preparing-databases/_static/image7.png)

El **inicie sesión** página aparece porque la **actualización créditos** página requiere privilegios administrativos.

Escriba *admin* como el nombre de usuario y *devpwd* como la contraseña y haga clic en **inicie sesión**.

![Inicie sesión en la página](preparing-databases/_static/image8.png)

El **actualización créditos** aparece la página.

![Página de actualización de créditos](preparing-databases/_static/image9.png)

Información de usuario y el rol está en el *aspnet ContosoUniversity* base de datos especificada por el **DefaultConnection** cadena de conexión en el *Web.config* archivo.

Esta base de datos no está administrado por Entity Framework Code First, por lo que no puede usar las migraciones para implementarlo. Deberá usar el proveedor dbDacFx para implementar el esquema de base de datos y a configurar el perfil de publicación para ejecutar un script que insertará datos iniciales en las tablas de base de datos.

> [!NOTE]
> Se introdujo un nuevo sistema de pertenencia ASP.NET (ahora denominado ASP.NET Identity) con Visual Studio 2013. El nuevo sistema le permite mantener la aplicación y tablas de pertenencia en la misma base de datos, y puede usar migraciones de Code First para implementar ambos. La aplicación de ejemplo usa el sistema de pertenencia ASP.NET anterior, que no se puede implementar mediante el uso de migraciones de Code First. Los procedimientos para la implementación de esta base de datos de pertenencia se aplican también a cualquier otro escenario en el que la aplicación necesita para implementar una base de datos de SQL Server que no se crea por Entity Framework Code First.


Aquí también, normalmente no desea que los mismos datos en producción que tiene en el desarrollo. Al implementar un sitio por primera vez, es habitual excluir la mayoría o todas las cuentas de usuario creadas para las pruebas. Por lo tanto, el proyecto descargado tiene dos bases de datos de pertenencia: *aspnet ContosoUniversity.mdf* con usuarios de desarrollo y *ContosoUniversity-aspnet-Prod.mdf* con los usuarios de producción. Para este tutorial, los nombres de usuario son los mismos en ambas bases de datos: *admin* y *nonadmin*. Ambos usuarios tienen la contraseña *devpwd* en la base de datos de desarrollo y *prodpwd* en la base de datos de producción.

Los usuarios de desarrollo va a implementar en el entorno de prueba y los usuarios de producción, ensayo y producción. Hacer creará dos secuencias de comandos SQL en este tutorial, uno para el desarrollo y otra para producción y en los tutoriales posteriores configurará el proceso de publicación para ejecutarlos.

> [!NOTE]
> La base de datos de pertenencia almacena un hash de contraseñas de cuentas. Para implementar las cuentas de un equipo a otro, debe asegurarse de que las rutinas de hash no generan hash distintos del servidor de destino de como lo hacen en el equipo de origen. Generará el mismos hash cuando se usa ASP.NET Universal Providers, siempre y cuando no cambie el algoritmo predeterminado. El algoritmo predeterminado es HMACSHA256 y se especifica en el **validación** atributo de la **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elemento en el archivo Web.config.


Puede crear scripts de implementación de datos manualmente, mediante el uso de SQL Server Management Studio (SSMS) o mediante una herramienta de terceros. En el resto de este tutorial le mostrará cómo hacerlo en SSMS, pero si prefiere no instalar y usar SSMS puede obtener las secuencias de comandos de la versión completa del proyecto y vaya a la sección donde se almacenan en la carpeta de soluciones.

Para instalar SSMS, instálelo desde [centro de descarga: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) haciendo [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Si elige uno incorrecto para el sistema no se podrá instalar y el otro se puede probar.

(Tenga en cuenta que esto es una descarga de 600 megabytes. Puede tardar mucho tiempo en instalar y requerirá un reinicio del equipo.)

En la primera página del centro de instalación de SQL Server, haga clic en **instalación independiente del nuevo servidor SQL Server o agregar características a una instalación existente**y siga las instrucciones y acepte las opciones predeterminadas.

### <a name="create-the-development-database-script"></a>Crear la secuencia de comandos de base de datos de desarrollo

1. Ejecute SSMS.
2. En el **conectar al servidor** diálogo cuadro, escriba *(localdb) \v11.0* como el **nombre del servidor**, deje **autenticación** establecido en **Autenticación de Windows**y, a continuación, haga clic en **Connect**.

    ![SSMS conectarse al servidor](preparing-databases/_static/image10.png)
3. En el **Explorador de objetos** ventana, expanda **bases de datos**, haga clic en **aspnet ContosoUniversity**, haga clic en **tareas**y, a continuación, haga clic en **Generar secuencias de comandos**.

    ![SSMS generar secuencias de comandos](preparing-databases/_static/image11.png)
4. En el **generar y publicar Scripts** cuadro de diálogo, haga clic en **establecer opciones de Scripting**.

    Puede omitir el **elegir objetos** paso porque el valor predeterminado es **secuencia de comandos de base de datos completa y todos los objetos de base de datos** y eso es lo que quiere.
5. Haga clic en **Avanzado**.

    ![Opciones de Scripting de SSMS](preparing-databases/_static/image12.png)
6. En el **opciones de Scripting avanzadas** cuadro de diálogo, desplácese hacia abajo hasta **tipos de datos en el script**y haga clic en el **solo datos** opción en la lista desplegable.
7. Cambio **incluir USE DATABASE** a **False**. USAR instrucciones no son válidas para Azure SQL Database y no son necesarios para su implementación en SQL Server Express en el entorno de prueba.

    ![SSMS Script sólo los datos, ninguna instrucción USE](preparing-databases/_static/image13.png)
8. Haga clic en **Aceptar**.
9. En el **generar y publicar Scripts** cuadro de diálogo, el **nombre de archivo** cuadro especifica donde se creará el script. Cambie la ruta de acceso a la carpeta de solución (la carpeta que tiene el archivo ContosoUniversity.sln) y el nombre de archivo a *aspnet-data-dev.sql*.
10. Haga clic en **siguiente** para ir a la **resumen** pestaña y, a continuación, haga clic en **siguiente** nuevo para crear la secuencia de comandos.

    ![Crea el Script de SSMS](preparing-databases/_static/image14.png)
11. Haga clic en **Finalizar**.

### <a name="create-the-production-database-script"></a>Crear la secuencia de comandos de base de datos de producción

Dado que aún no ejecuta el proyecto con la base de datos de producción, no está conectado aún a la instancia de LocalDB. Por lo tanto, debe adjuntar la base de datos en primer lugar.

1. En SSMS **Explorador de objetos**, haga clic en **bases de datos** y haga clic en **adjuntar**.

    ![Adjuntar de SSMS](preparing-databases/_static/image15.png)
2. En el **adjuntar bases de datos** cuadro de diálogo, haga clic en **agregar** y, a continuación, navegue hasta la *ContosoUniversity-aspnet-Prod.mdf* de archivos en el *aplicación\_ Datos* carpeta.

     ![SSMS Agregar archivo .mdf para adjuntar](preparing-databases/_static/image16.png)
3. Haga clic en **Aceptar**.
4. Siga el mismo procedimiento que usó anteriormente para crear un script para el archivo de producción. Nombre del archivo de script *aspnet-data-prod.sql*.

## <a name="summary"></a>Resumen

Ambas bases de datos ya están listos para implementarse y tendrá dos scripts de implementación de datos en la carpeta de soluciones.

![Scripts de implementación de datos](preparing-databases/_static/image17.png)

En el siguiente tutorial configurará las opciones de proyecto que afectan a la implementación y configurar automatic *Web.config* archivo transformaciones de configuración que debe ser diferente en la aplicación implementada.

## <a name="more-information"></a>Más información

Para obtener más información sobre NuGet, consulte [administrar bibliotecas de proyectos con NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) y [documentación de NuGet](http://docs.nuget.org/docs/start-here/overview). Si no desea usar NuGet, deberá saber cómo se analizan para determinar lo que hace cuando se instala un paquete de NuGet. (Por ejemplo, puede configurar *Web.config* transformaciones, configurar scripts de PowerShell para ejecutar en tiempo de compilación, etcetera.) Para más información acerca del funcionamiento de NuGet, consulte [crear y publicar un paquete](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) y [archivo de configuración y las transformaciones de código fuente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Siguiente](web-config-transformations.md)
