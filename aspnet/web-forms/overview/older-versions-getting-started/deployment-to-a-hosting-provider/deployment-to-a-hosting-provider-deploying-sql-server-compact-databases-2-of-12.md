---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación de SQL Server Compact bases de datos - 2 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: d0b76c06495c51df3ed0f61cd318507a05240392
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación de SQL Server Compact bases de datos - 2 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo configurar dos bases de datos de SQL Server Compact y el motor de base de datos para la implementación.

Acceso a la base de datos, la aplicación de la Universidad de Contoso requiere el siguiente software que debe implementarse con la aplicación porque no se incluye en .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (el motor de base de datos).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (que permiten al sistema de pertenencia ASP.NET que utilice SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/en-us/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First con migraciones).

La estructura de base de datos y algunos (no todos) de los datos de dos de la aplicación también se deben implementar las bases de datos. Por lo general, al desarrollar una aplicación, escriba datos de prueba en una base de datos que no desea implementar en un sitio en vivo. Sin embargo, también puede escribir algunos datos de producción que se va a implementar. En este tutorial configurará el proyecto de la Universidad de Contoso para que el software necesario y los datos correctos se incluyen cuando se implementa.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>En comparación con SQL Server Express de SQL Server Compact

La aplicación de ejemplo usa SQL Server Compact 4.0. Este motor de base de datos es una opción relativamente nueva para los sitios Web; las versiones anteriores de SQL Server Compact no funcionan en un entorno de hospedaje web. SQL Server Compact proporciona algunas ventajas en comparación con el escenario más común de desarrollo con SQL Server Express y la implementación completa de SQL Server. Dependiendo del proveedor de hospedaje que elija, SQL Server Compact pueden ser más económico implementar, porque algunos proveedores cobran adicional para admitir una base de datos completa de SQL Server. No hay ningún cargo adicional para SQL Server Compact dado que puede implementar el motor de base de datos como parte de la aplicación web.

Sin embargo, también debe conocer sus limitaciones. SQL Server Compact no admite procedimientos almacenados, desencadenadores, vistas o replicación. (Para obtener una lista completa de características de SQL Server que no son compatibles con SQL Server Compact, vea [diferencias entre SQL Server Compact y SQL Server](https://msdn.microsoft.com/en-us/library/bb896140.aspx).) Además, algunas de las herramientas que puede usar para manipular esquemas y datos de SQL Server Express y las bases de datos de SQL Server no funcionan con SQL Server Compact. Por ejemplo, no puede usar SQL Server Management Studio o SQL Server Data Tools en Visual Studio con bases de datos de SQL Server Compact. Tiene otras opciones para trabajar con bases de datos de SQL Server Compact:

- Puede usar el Explorador de servidores en Visual Studio, que ofrece una funcionalidad de manipulación de base de datos limitado para SQL Server Compact.
- Puede utilizar la función de manipulación de base de datos de [WebMatrix](https://www.microsoft.com/web/webmatrix/), que tiene más características que el Explorador de servidores.
- Puede usar terceros relativamente completos o abrir herramientas de código fuente, como la [cuadro de herramientas de SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) y [utilidad de script de esquema y los datos de SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Puede escribir y ejecutar sus propias secuencias de comandos DDL (lenguaje de definición de datos) para manipular el esquema de base de datos.

Puede empezar con SQL Server Compact y, a continuación, actualizar más adelante a medida que evolucionan sus necesidades. Más adelante tutoriales de esta serie muestra cómo migrar de SQL Server Compact a SQL Server Express y SQL Server. Sin embargo, si va a crear una nueva aplicación y esperar que necesita SQL Server en un futuro próximo, es probablemente sea mejor empezar con SQL Server o SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configurar el motor de base de datos compacta de SQL Server para la implementación

El software necesario para el acceso a datos en la aplicación de la Universidad de Contoso se agregó mediante la instalación de los siguientes paquetes de NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (proveedores universales de ASP.NET)
- [Entity Framework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Los vínculos apuntan a las versiones actuales de estos paquetes, que podrían ser más recientes que lo que está instalado en el proyecto de inicio que has descargado para este tutorial. Para la implementación de un proveedor de hospedaje, asegúrese de que usa Entity Framework 5.0 o posterior. Las versiones anteriores de migraciones de Code First requieren plena confianza y muchos proveedores de hospedaje de su aplicación se ejecutará en el nivel de confianza medio. Para obtener más información sobre el nivel de confianza medio, vea el [implementación en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

Instalación del paquete de NuGet generalmente se encarga de todo lo que necesita para implementar este software con la aplicación. En algunos casos, esto implica tareas como cambiar el archivo Web.config y agregar scripts de PowerShell que se ejecutan cada vez que compile la solución. **Si desea agregar compatibilidad para cualquiera de estas características (por ejemplo, SQL Server Compact y Entity Framework) sin usar NuGet, asegúrese de que sabe lo que hace la instalación de paquete de NuGet para que pueda hacer el mismo trabajo manualmente.**

Hay una excepción donde NuGet no toma las precauciones de todo lo que tiene que hacer para garantizar una implementación correcta. El paquete SqlServerCompact NuGet agrega un script posterior a la compilación para el proyecto que copie los ensamblados nativos a *x86* y *amd64* subcarpetas bajo el proyecto *bin* carpeta, pero el script no incluye las carpetas en el proyecto. Como resultado, Web Deploy no se copiarlos en el sitio web de destino a menos que incluya manualmente en el proyecto. (Este comportamiento resultante de la configuración de la implementación predeterminada; es otra opción, que no usan en estos tutoriales cambiar la configuración que controla este comportamiento. La configuración que se puede cambiar es **sólo los archivos necesarios para ejecutar la aplicación** en **elementos que se va a implementar** en el **Empaquetar/Publicar Web** pestaña de la **proyecto Propiedades** ventana. Si cambia esta configuración no se recomienda porque puede producir en la implementación de muchos archivos más al entorno de producción que se necesitan no existe. Para obtener más información acerca de las alternativas, consulte la [configurar propiedades del proyecto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) tutorial.)

Compile el proyecto y, a continuación, en **el Explorador de soluciones** haga clic en **mostrar todos los archivos** si aún no lo ha hecho. También tendrá que hacer clic en **actualizar**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Expanda el **bin** carpeta para ver el **amd64** y **x86** carpetas y, a continuación, seleccione las carpetas, menú contextual y seleccione **incluir en el proyecto**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Cambiarán los iconos de carpeta para mostrar que la carpeta se ha incluido en el proyecto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configuración de migraciones de Code First para la implementación de base de datos de aplicación

Al implementar una base de datos de aplicación, por lo general no simplemente implemente la base de datos de desarrollo con todos los datos en ella en producción, porque muchos de los datos en ella está probablemente ahí solo con fines de prueba. Por ejemplo, los nombres de estudiante en una base de datos de prueba son ficticios. Por otro lado, a menudo no puede implementar la estructura de base de datos con ningún dato en ella en absoluto. Algunos de los datos en la base de datos de prueba podrían ser datos reales y deben existir cuando los usuarios pueden comenzar a usar la aplicación. Por ejemplo, la base de datos podría tener una tabla que contiene los valores de calificación válido o nombres de departamento real.

Para simular este escenario habitual, configurará un método de inicialización de migraciones primer código que inserta en la base de datos únicamente los datos que desea que estén ahí en producción. Este método de inicialización no inserta los datos de prueba porque se ejecutará en producción después de Code First, se crea la base de datos de producción.

En versiones anteriores de Code First antes del lanzamiento de las migraciones, era común para los métodos de inicialización insertar datos de prueba también, porque con cada cambio de modelo durante el desarrollo de la base de datos no tiene que eliminar y volver a crear desde cero completamente. Con migraciones de Code First, datos de prueba se conservan después de realizar cambios de base de datos, por lo que no es necesario incluir datos de prueba en el método de inicialización. El proyecto descargado utiliza el método de migraciones anteriores de incluir todos los datos en el método de inicialización de una clase de inicializador. En este tutorial podrá deshabilitar la clase de inicializador y habilitará las migraciones. A continuación, actualizaremos el método de inicialización de la clase de configuración de migraciones para que inserte solo los datos que se van a insertar en producción.

El siguiente diagrama muestra el esquema de la base de datos de aplicación:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Para estos tutoriales, supondremos que el `Student` y `Enrollment` tablas deben estar vacías cuando se implementó el sitio por primera vez. Las demás tablas contienen datos que tiene que cargarse previamente cuando la aplicación esté activa.

Puesto que va a usar migraciones de Code First, ya no debe utilizar el **DropCreateDatabaseIfModelChanges** inicializador Code First. El código para este inicializador está en el archivo SchoolInitializer.cs en el proyecto ContosoUniversity.DAL. Un valor en el **appSettings** Este inicializador para ejecutarse cada vez que la aplicación intenta tener acceso a la base de datos por primera vez hace que el elemento del archivo Web.config:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Abra el archivo Web.config de la aplicación y quitar el elemento que especifica la clase de inicializador Code First desde el elemento appSettings. El elemento appSettings parece similar al siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Otra manera de especificar un inicializador de clase es hacerlo mediante una llamada `Database.SetInitializer` en el `Application_Start` método en el *Global.asax* archivo. Si va a habilitar las migraciones en un proyecto que utiliza ese método para especificar al inicializador, quitar esa línea de código.


A continuación, habilitar migraciones de Code First.

El primer paso es asegurarse de que el proyecto de ContosoUniversity está establecido como proyecto de inicio. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y seleccione **establecer como proyecto de inicio**. Migraciones de Code First buscará en el proyecto de inicio para buscar la cadena de conexión de base de datos.

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

En la parte superior de la **Package Manager Console** ventana Seleccionar ContosoUniversity.DAL como el proyecto predeterminado y, a continuación, como el `PM>` símbolo del sistema escriba "enable-migrations".

![Habilitar migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Este comando crea un *archivo Configuration.cs que* archivo en una nueva *migraciones* carpeta en el proyecto ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Ha seleccionado el proyecto de la capa DAL porque se debe ejecutar el comando "enable-migrations" en el proyecto que contiene la clase de contexto de Code First. Cuando esa clase está en un proyecto de biblioteca de clases, migraciones de Code First busca la cadena de conexión de base de datos en el proyecto de inicio para la solución. En la solución ContosoUniversity, el proyecto web se estableció como proyecto de inicio. (Si desea designar el proyecto que tiene la cadena de conexión como proyecto de inicio en Visual Studio, puede especificar el proyecto de inicio en el comando de PowerShell. Para ver la sintaxis del comando para el comando enable-migrations, puede escribir el comando "get-help enable-migrations".)

Abra el archivo del archivo Configuration.cs que y reemplace los comentarios en el `Seed` método por el código siguiente:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Las referencias a `List` tiene líneas onduladas rojas debajo de ellos porque no tiene un `using` aún la instrucción para su espacio de nombres. Haga clic en una de las instancias de `List` y haga clic en **resolver**y, a continuación, haga clic en **using System.Collections.Generic**.

![Resolver con el uso de la instrucción](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Esta selección de menú agrega el código siguiente a la `using` instrucciones cerca de la parte superior del archivo.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Agregar código para el `Seed` método es una de las muchas maneras que puede insertar datos fijas en la base de datos. Una alternativa consiste en Agregar código a la `Up` y `Down` métodos de cada clase de migración. El `Up` y `Down` métodos contener código que implementa los cambios de la base de datos. Verá ejemplos de ellos en la [implementar una actualización de la base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.
> 
> También puede escribir código que ejecuta instrucciones SQL mediante el `Sql` método. Por ejemplo, si se agrega una columna de presupuesto a la tabla de departamento y desea inicializar todos los presupuestos de departamento para 1.000,00 dólares como parte de una migración, puede agregar la siguiente línea de código para el `Up` método para que la migración:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> En este ejemplo se muestra para este tutorial se usa el `AddOrUpdate` método en el `Seed` método de las migraciones de Code First `Configuration` clase. El código llama a First Migrations el `Seed` después de cada migración, este método y actualiza las filas que ya se han insertado o insertan si aún no existen. El `AddOrUpdate` método podría no ser la mejor elección para su escenario. Para obtener más información, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julie Lerman.


Presione CTRL-MAYÚS + B para compilar el proyecto.

El paso siguiente consiste en crear un `DbMigration` clase para la migración inicial. Desea que esta migración para crear una nueva base de datos, por lo que debe eliminar la base de datos que ya existe. Las bases de datos de SQL Server Compact se encuentran en *.sdf* archivos en el *aplicación\_datos* carpeta. En **el Explorador de soluciones**, expanda *aplicación\_datos* en el proyecto ContosoUniversity para ver las dos bases de datos de SQL Server Compact, que se representan mediante *.sdf*archivos.

Haga clic en el *School.sdf* de archivo y haga clic en **eliminar**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

En el **Package Manager Console** ventana, escriba el comando "Agregar-migración inicial" para crear la migración inicial y asígnele el nombre "Inicial".

![migration_command agregar](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta y esta clase contiene código que crea el esquema de base de datos.

En el **Package Manager Console**, escriba el comando "update-database" para crear la base de datos y ejecutar el **inicialización** método.

![actualización database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. In that Case, eliminar el *School.sdf* archivo nuevo y vuelva a intentar la `update-database` comando.)

Ejecute la aplicación. Ahora la página de los alumnos está vacía pero la página de instructores contiene instructores. Esto es lo que obtendrá en producción después de implementar la aplicación.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

El proyecto ahora está listo para implementar la *School* base de datos.

## <a name="creating-a-membership-database-for-deployment"></a>Crear una base de datos de pertenencia para la implementación

La aplicación de la Universidad de Contoso utiliza la autenticación de formularios o de sistema de pertenencia ASP.NET para autenticar y autorizar a los usuarios. Uno de sus páginas es accesible únicamente a los administradores. Para ver esta página, ejecute la aplicación y seleccione **actualización créditos** desde el menú desplegable bajo **cursos**. La aplicación muestra el **inicio de sesión** página, ya que sólo los administradores autorizados para usar el **actualización créditos** página.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Inicie sesión como "admin" con la contraseña "Pa$ w0rd" (tenga en cuenta el número cero en lugar de la letra "o" en "w0rd"). Tras iniciar sesión, el **actualización créditos** se muestra la página.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Cuando se implementa un sitio por primera vez, es común para excluir la mayoría o todas las cuentas de usuario creadas para las pruebas. En este caso, va a implementar una cuenta de administrador y no hay ninguna cuenta de usuario. En lugar de eliminar manualmente las cuentas de prueba, creará una nueva base de datos de pertenencia que tiene solo la cuenta de usuario de un administrador que necesite en producción.

> [!NOTE]
> La base de datos de pertenencia almacena un valor hash de las contraseñas de cuentas. Para implementar las cuentas de un equipo a otro, debe asegurarse de que las rutinas de hash no generan hash diferentes en el servidor de destino que lo hacen en el equipo de origen. Generará el mismo hash cuando se usan ASP.NET Universal Providers, siempre y cuando no cambie el algoritmo predeterminado. El algoritmo predeterminado es HMACSHA256 y se especifica en el **validación** atributo de la  **[machineKey](https://msdn.microsoft.com/en-us/library/w8h3skw9.aspx)**  elemento en el archivo Web.config.


Migraciones de Code First no mantiene la base de datos de pertenencia y no hay ningún inicializador automática que propaga la base de datos con las cuentas de prueba (como sucede con la base de datos School). Por lo tanto, para mantener los datos de prueba disponibles realizará una copia de la base de datos de prueba antes de crear uno nuevo.

En **el Explorador de soluciones**, cambiar el nombre de la *aspnet.sdf* un archivo en el *aplicación\_datos* carpeta a *Dev.sdf aspnet*. (No hacer una copia, simplemente cambie su nombre, se creará una nueva base de datos en un momento.)

En **el Explorador de soluciones**, asegúrese de que está seleccionado el proyecto web (ContosoUniversity, no ContosoUniversity.DAL). A continuación, en la **proyecto** menú, seleccione **de configuración de ASP.NET** para ejecutar el **herramienta de administración de sitios Web**(WAT).

Seleccione el **seguridad** ficha.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Haga clic en **crear o administrar funciones** y agregue un **administrador** rol.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Vuelva a la **seguridad** , haga clic en **Create User**, y agregue el usuario "admin" como administrador. Antes de hacer clic en el **Create User** situado en la **Create User** página, asegúrese de que selecciona el **administrador** casilla de verificación. La contraseña utilizada en este tutorial es "Pa$ w0rd", y puede escribir cualquier dirección de correo electrónico.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Cierre el explorador. En **el Explorador de soluciones**, haga clic en el botón Actualizar para ver la nueva *aspnet.sdf* archivo.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Haga clic en **aspnet.sdf** y seleccione **incluir en el proyecto**.

## <a name="distinguishing-development-from-production-databases"></a>Distinción de desarrollo de bases de datos de producción

En esta sección, podrá cambiar el nombre de las bases de datos para que las versiones de desarrollo son Dev.sdf School y Dev.sdf aspnet y las versiones de producción son Prod.sdf School y Prod.sdf de aspnet. Esto no es necesario, pero si lo hace por lo que le ayudará a impedir la obtención de versiones de prueba y producción de las bases de datos confundirse.

En **el Explorador de soluciones**, haga clic en **actualizar** y expanda la aplicación\_carpeta de datos para ver la base de datos School que creó anteriormente; haga clic en él y seleccione **incluir en el proyecto** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Cambiar el nombre de *aspnet.sdf* a *Prod.sdf aspnet*.

Cambiar el nombre de *School.sdf* a *School-Dev.sdf*.

Al ejecutar la aplicación en Visual Studio que no desea utilizar el *-Prod* versiones de los archivos de base de datos, que desea usar *- Dev* versiones. Por lo tanto, tendrá que cambiar las cadenas de conexión en el archivo Web.config para que apunten a la *- Dev* versiones de las bases de datos. (No ha creado un archivo Prod.sdf School, pero eso está bien porque Code First van a crear esa base de datos en el tiempo de producción de la primera que se ejecuta una aplicación no existe).

Abra el archivo Web.config de la aplicación y busque las cadenas de conexión:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Cambie "aspnet.sdf" a "aspnet-Dev.sdf" y cambiar "School.sdf" a "Escuela Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

El motor de base de datos de SQL Server Compact y ambas bases de datos están listos para implementarse. En el tutorial siguiente configura el automático *Web.config* archivo transformaciones para la configuración que debe ser diferente en los entornos de desarrollo, prueba y producción. (Entre los valores que se deben cambiar se encuentran las cadenas de conexión, pero se configuran los cambios más tarde cuando se crea un perfil de publicación).

## <a name="more-information"></a>Más información

Para obtener más información sobre NuGet, consulte [administrar bibliotecas de proyecto con NuGet](https://msdn.microsoft.com/en-us/magazine/hh547106.aspx) y [documentación de NuGet](http://docs.nuget.org/docs/start-here/overview). Si no desea usar NuGet, debe aprender cómo analizar un paquete de NuGet para determinar lo que hace cuando se instala. (Por ejemplo, puede configurar *Web.config* transformaciones, configurar secuencias de comandos de PowerShell para ejecutar en tiempo de compilación, etcetera.) Para más información acerca del funcionamiento de NuGet, consulte especialmente [crear y publicar un paquete](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) y [archivo de configuración y las transformaciones de código de origen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
