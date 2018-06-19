---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implementación de Web ASP.NET con Visual Studio: preparación para la implementación de la base de datos | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 61392af322de454687da522055005a670b34f510
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889152"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implementación de Web ASP.NET con Visual Studio: preparación para la implementación de la base de datos
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo obtener el proyecto listo para la implementación de la base de datos. La estructura de base de datos y algunos (no todos) de los datos de dos de la aplicación las bases de datos deben implementarse en entornos de prueba, ensayo y producción.

Por lo general, al desarrollar una aplicación, escriba datos de prueba en una base de datos que no desea implementar en un sitio en vivo. Sin embargo, también puede tener algunos datos de producción que se va a implementar. En este tutorial podrá configurar el proyecto de la Universidad de Contoso y preparar las secuencias de comandos SQL para que los datos correctos se incluye cuando se implementa.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La aplicación de ejemplo usa SQL Server Express LocalDB. SQL Server Express es la edición gratuita de SQL Server. Normalmente se utiliza durante el desarrollo porque se basa en el mismo motor de base de datos como las versiones completas de SQL Server. Puede probar con SQL Server Express y estar seguro de que la aplicación comportará igual en producción, con unas pocas excepciones para las características que varían en las diferentes ediciones de SQL Server.

LocalDB es un modo especial de ejecución de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Normalmente, los archivos de base de datos de LocalDB se mantienen en la *aplicación\_datos* carpeta de un proyecto web. La característica de instancia de usuario en SQL Server Express también le permite trabajar con *.mdf* archivos, pero la característica de instancia de usuario está en desuso; por lo tanto, se recomienda LocalDB para trabajar con *.mdf* archivos.

Normalmente no se utiliza SQL Server Express para las aplicaciones web de producción. En particular LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para trabajar con IIS.

En Visual Studio 2012, LocalDB se instala de forma predeterminada con Visual Studio. En Visual Studio 2010 y versiones anteriores, SQL Server Express (sin LocalDB) se instala de forma predeterminada con Visual Studio; es decir ¿por qué se instala como uno de los requisitos previos en [el primer tutorial de esta serie](introduction.md).

Para obtener más información acerca de las ediciones de SQL Server, incluido LocalDB, vea los siguientes recursos [trabajar con bases de datos de SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework y proveedores universales

Acceso a la base de datos, la aplicación de la Universidad de Contoso requiere el siguiente software que debe implementarse con la aplicación porque no se incluye en .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que el sistema de pertenencia ASP.NET usar la base de datos de SQL Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Dado que este software se incluye en los paquetes de NuGet, el proyecto ya está configurado para que los ensamblados necesarios se implementan con el proyecto. (Los vínculos apuntan a las versiones actuales de estos paquetes, que podrían ser más recientes que lo que está instalado en el proyecto de inicio que has descargado para este tutorial.)

Si va a implementar en un proveedor de hospedaje de terceros en lugar de Azure, asegúrese de que utiliza Entity Framework 5.0 o posterior. Las versiones anteriores de migraciones de Code First requieren plena confianza y la mayoría de proveedores de hospedaje ejecutarán la aplicación en el nivel de confianza medio. Para obtener más información sobre el nivel de confianza medio, vea el [implementar a IIS como un entorno de prueba](deploying-to-iis.md) tutorial.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar migraciones de Code First para la implementación de base de datos de aplicación

La base de datos de aplicación de la Universidad de Contoso está administrado por Code First y va a implementar mediante el uso de migraciones de Code First. Para obtener información general de implementación de base de datos mediante el uso de migraciones de Code First, vea [el primer tutorial de esta serie](introduction.md).

Al implementar una base de datos de aplicación, por lo general no simplemente implemente la base de datos de desarrollo con todos los datos en ella en producción, porque muchos de los datos en ella está probablemente ahí solo con fines de prueba. Por ejemplo, los nombres de estudiante en una base de datos de prueba son ficticios. Por otro lado, a menudo no puede implementar la estructura de base de datos con ningún dato en ella en absoluto. Algunos de los datos en la base de datos de prueba podrían ser datos reales y deben existir cuando los usuarios pueden comenzar a usar la aplicación. Por ejemplo, la base de datos podría tener una tabla que contiene los valores de calificación válido o nombres de departamento real.

Para simular este escenario habitual, configurará un migraciones de Code First `Seed` método que inserta en la base de datos únicamente los datos que desea que estén ahí en producción. Esto `Seed` método no debe insertar los datos de prueba porque se ejecutará en producción después de Code First, se crea la base de datos de producción.

En versiones anteriores de Code First antes del lanzamiento de las migraciones, era habitual que `Seed` métodos para insertar datos de prueba también, porque con cada cambio de modelo durante el desarrollo de la base de datos no tiene que eliminar y volver a crear desde cero completamente. Con migraciones de Code First, prueba los datos se conservan después de realizar cambios de base de datos, por lo que incluso los datos de prueba de la `Seed` método no es necesario. El proyecto que ha descargado utiliza el método de inclusión de todos los datos en el `Seed` método de un inicializador de clase. En este tutorial podrá deshabilitar esa clase de inicializador y `enable Migrations. Then you'll update the `inicialización ' método en la configuración de migraciones de clase para que inserta solamente los datos que desea que se van a insertar en producción.

El siguiente diagrama muestra el esquema de la base de datos de aplicación:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Para estos tutoriales, supondremos que el `Student` y `Enrollment` tablas deben estar vacías cuando se implementó el sitio por primera vez. Las demás tablas contienen datos que tiene que cargarse previamente cuando la aplicación esté activa.

### <a name="disable-the-initializer"></a>Deshabilitar el inicializador

Puesto que va a usar migraciones de Code First, no es necesario usar el `DropCreateDatabaseIfModelChanges` inicializador Code First. El código para este inicializador está en el *SchoolInitializer.cs* archivo en el proyecto ContosoUniversity.DAL. Un valor en el `appSettings` elemento de la *Web.config* archivo hace que este inicializador para ejecutarse cada vez que la aplicación intenta tener acceso a la base de datos por primera vez:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra la aplicación *Web.config* de archivos y quite o marque como comentario el `add` elemento que especifica la clase de inicializador Code First. El `appSettings` elemento ahora el siguiente aspecto:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Otra manera de especificar un inicializador de clase es hacerlo mediante una llamada `Database.SetInitializer` en el `Application_Start` método en el *Global.asax* archivo. Si va a habilitar las migraciones en un proyecto que utiliza ese método para especificar al inicializador, quitar esa línea de código.


> [!NOTE]
> Si usa Visual Studio 2013, agregue los siguientes pasos entre los pasos 2 y 3: (a) PMC en Escriba "paquete de actualización entityframework-versión 6.1.1" para obtener la versión actual de EF. A continuación (b) generar el proyecto para obtener una lista de errores de compilación y corregirlos. Eliminar con instrucciones para los espacios de nombres que ya no existen, haga clic en y haga clic en resolver para agregar instrucciones using donde sea necesario y cambie las apariciones de System.Data.EntityState a System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Habilitar migraciones de Code First

1. Asegúrese de que el proyecto de ContosoUniversity (no ContosoUniversity.DAL) está configurado como proyecto de inicio. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y seleccione **establecer como proyecto de inicio**. Migraciones de Code First buscará en el proyecto de inicio para buscar la cadena de conexión de base de datos.
2. Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** (o **Administrador de paquetes de NuGet**) y, a continuación, **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. En la parte superior de la **Package Manager Console** ventana Seleccionar ContosoUniversity.DAL como el proyecto predeterminado y, a continuación, como el `PM>` símbolo del sistema escriba "enable-migrations".

    ![comando Enable-migrations](preparing-databases/_static/image4.png)

    (Si se produce un hablados error la *enable-migrations* no se reconoce el comando, escriba el comando *EntityFramework de paquete de actualización-reinstalar* e inténtelo de nuevo.)

    Este comando crea un *migraciones* carpeta en el proyecto ContosoUniversity.DAL y lo coloca en esa carpeta dos archivos: un *archivo Configuration.cs que* archivos que puede usar para configurar las migraciones y un *InitialCreate.cs* archivo para la primera migración que crea la base de datos.

    ![Carpeta de migraciones](preparing-databases/_static/image5.png)

    Ha seleccionado el proyecto DAL en el **proyecto predeterminado** la lista desplegable de la **Package Manager Console** porque el `enable-migrations` comando debe ejecutarse en el proyecto que contiene el primer código clase de contexto. Cuando esa clase está en un proyecto de biblioteca de clases, migraciones de Code First busca la cadena de conexión de base de datos en el proyecto de inicio para la solución. En la solución ContosoUniversity, el proyecto web se estableció como proyecto de inicio. Si no desea designar el proyecto que tiene la cadena de conexión como proyecto de inicio en Visual Studio, puede especificar el proyecto de inicio en el comando de PowerShell. Para ver la sintaxis del comando, escriba el comando `get-help enable-migrations`.

    El `enable-migrations` comando crea automáticamente la primera migración porque la base de datos ya existe. Una alternativa es que las migraciones de crear la base de datos. Para ello, utilice **Explorador de servidores** o **Explorador de objetos de SQL Server** para eliminar la base de datos de ContosoUniversity antes de habilitar las migraciones. Después de habilitar las migraciones, cree manualmente la primera migración escribiendo el comando "Agregar-migration InitialCreate". A continuación, puede crear la base de datos escribiendo el comando "update-database".

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

En este tutorial agregará datos fijas agregando código a la `Seed` método de las migraciones de Code First `Configuration` clase. El código llama a First Migrations el `Seed` método después de cada migración.

Puesto que la `Seed` método se ejecuta después de cada migración, hay datos en las tablas después de la primera migración. Para controlar esta situación usará el `AddOrUpdate` método para actualizar las filas que ya se han insertado o inserción si aún no existen. El `AddOrUpdate` método podría no ser la mejor elección para su escenario. Para obtener más información, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julie Lerman.

1. Abrir la *archivo Configuration.cs que* archivo y reemplace los comentarios en el `Seed` método por el código siguiente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Las referencias a `List` tiene líneas onduladas rojas debajo de ellos porque no tiene un `using` aún la instrucción para su espacio de nombres. Haga clic en una de las instancias de `List` y haga clic en **resolver**y, a continuación, haga clic en **using System.Collections.Generic**.

    ![Resolver con el uso de la instrucción](preparing-databases/_static/image6.png)

    Esta selección de menú agrega el código siguiente a la `using` instrucciones cerca de la parte superior del archivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Presione CTRL-MAYÚS + B para compilar el proyecto.

El proyecto ahora está listo para implementar la *ContosoUniversity* base de datos. Después de implementar la aplicación, la primera vez ejecutarlo y navegar a una página que tiene acceso a la base de datos Code First creará la base de datos y ejecutar este ejemplo `Seed` método.

> [!NOTE]
> Agregar código para el `Seed` método es una de las muchas maneras que puede insertar datos fijas en la base de datos. Una alternativa consiste en Agregar código a la `Up` y `Down` métodos de cada clase de migración. El `Up` y `Down` métodos contener código que implementa los cambios de la base de datos. Verá ejemplos de ellos en la [implementar una actualización de la base de datos](deploying-a-database-update.md) tutorial.
> 
> También puede escribir código que ejecuta instrucciones SQL mediante el `Sql` método. Por ejemplo, si se agrega una columna de presupuesto a la tabla de departamento y desea inicializar todos los presupuestos de departamento para 1.000,00 dólares como parte de una migración, puede agregar la siguiente línea de código para el `Up` método para que la migración:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Crear secuencias de comandos para la implementación de la base de datos de pertenencia

La aplicación de la Universidad de Contoso utiliza la autenticación de formularios o de sistema de pertenencia ASP.NET para autenticar y autorizar a los usuarios. El **actualización créditos** página solo es accesible para los usuarios que están en el rol de administrador.

Ejecute la aplicación y haga clic en **cursos**y, a continuación, haga clic en **actualización créditos**.

![Haga clic en créditos de actualización](preparing-databases/_static/image7.png)

El **sesión** página aparece porque la **créditos de actualización** página requiere privilegios administrativos.

Escriba *administración* como el nombre de usuario y *devpwd* como la contraseña y haga clic en **sesión**.

![Inicie sesión en la página](preparing-databases/_static/image8.png)

El **actualización créditos** aparecerá la página.

![Página de actualización de créditos](preparing-databases/_static/image9.png)

Información de usuario y el rol está en el *aspnet ContosoUniversity* base de datos especificada por el **DefaultConnection** cadena de conexión en el *Web.config* archivo.

Esta base de datos no está administrado por Entity Framework Code First, por lo que no puede usar migraciones para implementarlo. Se usará el proveedor dbDacFx para implementar el esquema de base de datos y a configurar el perfil de publicación para ejecutar un script que se insertará los datos iniciales en tablas de base de datos.

> [!NOTE]
> Un nuevo sistema de pertenencia ASP.NET (ahora denominado ASP.NET Identity) se introdujo con Visual Studio 2013. El nuevo sistema le permite mantener la aplicación y tablas de pertenencia en la misma base de datos, y puede usar migraciones de Code First para implementar ambos. La aplicación de ejemplo utiliza el sistema de pertenencia ASP.NET anterior, que no se pueden implementar mediante el uso de migraciones de Code First. Los procedimientos para la implementación de esta base de datos de pertenencia se aplican también a cualquier otro escenario en el que la aplicación necesita implementar una base de datos de SQL Server que no se crea por Entity Framework Code First.


Aquí también, normalmente no desea que los mismos datos en producción que tienen en el desarrollo. Cuando se implementa un sitio por primera vez, es común para excluir la mayoría o todas las cuentas de usuario creadas para las pruebas. Por lo tanto, el proyecto descargado tiene dos bases de datos de pertenencia: *aspnet ContosoUniversity.mdf* con usuarios de desarrollo y *aspnet-ContosoUniversity-Prod.mdf* con usuarios de producción. Para este tutorial, los nombres de usuario son los mismos en ambas bases de datos: *administración* y *nonadmin*. Ambos usuarios tienen la contraseña *devpwd* en la base de datos de desarrollo y *prodpwd* en la base de datos de producción.

Implementará los usuarios de desarrollo en el entorno de prueba y los usuarios de producción a ensayo y producción. Para hacer creará dos secuencias de comandos SQL en este tutorial, uno para el desarrollo y otro para producción y, en tutoriales posteriores configurará el proceso de publicación para ejecutarlos.

> [!NOTE]
> La base de datos de pertenencia almacena un valor hash de las contraseñas de cuentas. Para implementar las cuentas de un equipo a otro, debe asegurarse de que las rutinas de hash no generan hash diferentes en el servidor de destino que lo hacen en el equipo de origen. Generará el mismo hash cuando se usan ASP.NET Universal Providers, siempre y cuando no cambie el algoritmo predeterminado. El algoritmo predeterminado es HMACSHA256 y se especifica en el **validación** atributo de la **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elemento en el archivo Web.config.


Puede crear scripts de implementación de datos manualmente, mediante el uso de SQL Server Management Studio (SSMS), o mediante una herramienta de terceros. Este resto de este tutorial le mostrará cómo hacerlo en SSMS, pero si no desea instalar y usar SSMS puede obtener las secuencias de comandos de la versión completa del proyecto y vaya a la sección donde se almacenan en la carpeta de soluciones.

Para instalar SSMS, instálelo desde [centro de descarga: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) haciendo clic en [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Si elige uno incorrecto para el sistema no se podrá instalar y puede probar con los otros.

(Tenga en cuenta que esto es una descarga de 600 megabytes. Puede tardar mucho tiempo en instalar y requerirá un reinicio del equipo.)

En la primera página del centro de instalación de SQL Server, haga clic en **instalación independiente del nuevo servidor SQL Server o agregar características a una instalación existente**y siga las instrucciones, acepte las opciones predeterminadas.

### <a name="create-the-development-database-script"></a>Crear la secuencia de comandos de base de datos de desarrollo

1. Ejecute SSMS.
2. En el **conectar al servidor** diálogo cuadro, escriba *(localdb) \v11.0* como el **nombre del servidor**, deje **autenticación** establecido en **Autenticación de Windows**y, a continuación, haga clic en **conectar**.

    ![SSMS conectarse al servidor](preparing-databases/_static/image10.png)
3. En el **Explorador de objetos** ventana, expanda **bases de datos**, haga clic en **aspnet ContosoUniversity**, haga clic en **tareas**y, a continuación, haga clic en **Generar secuencias de comandos**.

    ![SSMS generar secuencias de comandos](preparing-databases/_static/image11.png)
4. En el **generar y publicar Scripts** cuadro de diálogo, haga clic en **establecer opciones de Scripting**.

    Puede omitir el **elegir objetos** paso porque el valor predeterminado es **secuencia de comandos de base de datos completa y todos los objetos de base de datos** y que es lo que desea.
5. Haga clic en **Avanzado**.

    ![Opciones de Scripting de SSMS](preparing-databases/_static/image12.png)
6. En el **opciones de Scripting avanzadas** cuadro de diálogo, desplácese hacia abajo hasta **tipos de datos a la secuencia de comandos**y haga clic en el **solo datos** opción en la lista desplegable.
7. Cambio **incluir USE DATABASE** a **False**. USAR instrucciones no son válidas para la base de datos de SQL Azure y no son necesarios para su implementación en SQL Server Express en el entorno de prueba.

    ![SSMS Script sólo los datos, ninguna instrucción USE](preparing-databases/_static/image13.png)
8. Haga clic en **Aceptar**.
9. En el **generar y publicar Scripts** cuadro de diálogo, el **nombre de archivo** cuadro especifica donde se creará la secuencia de comandos. Cambie la ruta de acceso a la carpeta de soluciones (la carpeta que tiene el archivo ContosoUniversity.sln) y el nombre de archivo para *dev.sql de datos de aspnet*.
10. Haga clic en **siguiente** para ir a la **resumen** ficha y, a continuación, haga clic en **siguiente** nuevo para crear la secuencia de comandos.

    ![Script SSMS creado](preparing-databases/_static/image14.png)
11. Haga clic en **Finalizar**.

### <a name="create-the-production-database-script"></a>Crear la secuencia de comandos de base de datos de producción

Dado que aún no ejecuta el proyecto con la base de datos de producción, no está conectado aún a la instancia de LocalDB. Por lo tanto, debe adjuntar la base de datos en primer lugar.

1. En SSMS **Explorador de objetos**, haga clic en **bases de datos** y haga clic en **adjuntar**.

    ![Adjuntar de SSMS](preparing-databases/_static/image15.png)
2. En el **adjuntar bases de datos** cuadro de diálogo, haga clic en **agregar** y, a continuación, navegue hasta la *aspnet-ContosoUniversity-Prod.mdf* un archivo en el *aplicación\_ Datos* carpeta.

     ![SSMS Agregar archivo .mdf para adjuntar](preparing-databases/_static/image16.png)
3. Haga clic en **Aceptar**.
4. Siga el mismo procedimiento que usó anteriormente para crear una secuencia de comandos para el archivo de producción. Asignar nombre al archivo de script *prod.sql de datos de aspnet*.

## <a name="summary"></a>Resumen

Ambas bases de datos están listos para implementarse y tiene dos secuencias de comandos de implementación de datos en la carpeta de soluciones.

![Scripts de implementación de datos](preparing-databases/_static/image17.png)

En el tutorial siguiente configurará las opciones de proyecto que afectan a la implementación y configurar automático *Web.config* archivo transformaciones para la configuración que debe ser diferente de la aplicación implementada.

## <a name="more-information"></a>Más información

Para obtener más información sobre NuGet, consulte [administrar bibliotecas de proyecto con NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) y [documentación de NuGet](http://docs.nuget.org/docs/start-here/overview). Si no desea usar NuGet, debe aprender cómo analizar un paquete de NuGet para determinar lo que hace cuando se instala. (Por ejemplo, puede configurar *Web.config* transformaciones, configurar secuencias de comandos de PowerShell para ejecutar en tiempo de compilación, etcetera.) Para más información acerca del funcionamiento de NuGet, consulte [crear y publicar un paquete](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) y [archivo de configuración y las transformaciones de código de origen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Siguiente](web-config-transformations.md)
