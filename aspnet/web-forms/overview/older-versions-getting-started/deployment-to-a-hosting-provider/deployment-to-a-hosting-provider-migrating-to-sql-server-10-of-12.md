---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migración a SQL Server - 10 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: e47f9cb7df6cc119c69f0332df904183cef86405
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836135"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migración a SQL Server - 10 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo migrar de SQL Server Compact a SQL Server. Uno de los motivos es posible que desee hacerlo es aprovechar las características de SQL Server que SQL Server Compact no admite, como procedimientos almacenados, desencadenadores, vistas o replicación. Para obtener más información sobre las diferencias entre SQL Server Compact y SQL Server, consulte el [implementar SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express frente a SQL Server completo para el desarrollo

Una vez que ha decidido actualizar a SQL Server, es posible que desee usar SQL Server o SQL Server Express en los entornos de desarrollo y pruebas. Además de las diferencias en la compatibilidad con herramientas y en las características del motor de base de datos, hay diferencias en las implementaciones del proveedor SQL Server Compact y otras versiones de SQL Server. Estas diferencias pueden ocasionar el mismo código generar resultados diferentes. Por lo tanto, si decide mantener SQL Server Compact, como la base de datos de desarrollo, se debe probar exhaustivamente el sitio en SQL Server o SQL Server Express en un entorno de prueba antes de cada implementación en producción.

A diferencia de SQL Server Compact, SQL Server Express es esencialmente el mismo motor de base de datos y usa el mismo proveedor de .NET como completa de SQL Server. Cuando se prueba con SQL Server Express, puede estar seguro de obtener los mismos resultados igual que con SQL Server. Puede utilizar la mayoría de las mismas herramientas de base de datos con SQL Server que puede usar con SQL Server Express (una excepción notable que se va a [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), y es compatible con otras características de SQL Server como procedimientos almacenados, vistas, desencadenadores, y la replicación. (Normalmente, tendrá que usar completa de SQL Server en un sitio Web de producción, sin embargo. SQL Server Express se puede ejecutar en un entorno de hospedaje compartido, pero no se diseñó para, y muchos proveedores de hospedaje no la admiten.)

Si utiliza Visual Studio 2012, normalmente elegir SQL Server Express LocalDB para su entorno de desarrollo porque eso es lo que se instala de forma predeterminada con Visual Studio. Sin embargo, LocalDB no funciona en IIS, por lo que para el entorno de prueba debe usar SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinación de las bases de datos frente a mantenerlos independiente

La aplicación Contoso University tiene dos bases de datos de SQL Server Compact: la base de datos de pertenencia (*aspnet.sdf*) y la base de datos de aplicación (*School.sdf*). Cuando se migra, puede migrar estas bases de datos para dos bases de datos independientes o para una sola base de datos. Puede combinarlos con el fin de facilitar las combinaciones de base de datos entre la base de datos de aplicación y la base de datos de pertenencia. El plan de hospedaje también podría proporcionar un motivo para combinarlos. Por ejemplo, el proveedor de hospedaje puede cobrar más para varias bases de datos o podría incluso no permitir más de una base de datos. Que es el caso con Cytanium Lite hospeda la cuenta que se usa para este tutorial, lo que permite solo un único servidor de SQL database.

En este tutorial, deberá migrar las dos bases de datos de este modo:

- Migrar a dos bases de datos LocalDB en el entorno de desarrollo.
- Migrar a dos bases de datos de SQL Server Express en el entorno de prueba.
- Migrar a SQL Server completa combinado uno con una base de datos en el entorno de producción.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalación de SQL Server Express

SQL Server Express se instala automáticamente con Visual Studio 2010 de forma predeterminada, pero de forma predeterminada no se instala con Visual Studio 2012. Para instalar SQL Server 2012 Express, haga clic en el vínculo siguiente

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Elija *ENU/x64/SQLEXPR\_x64\_ENU.exe* o *ENU/x86/SQLEXPR\_x86\_ENU.exe*y en el Asistente para instalación, acepte el valor predeterminado Configuración. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server 2012 desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Creación de bases de datos SQL Server Express para el entorno de prueba

El siguiente paso es crear la pertenencia a ASP.NET y las bases de datos School.

Desde el **vista** menú, seleccione **Explorador de servidores** (**Database Explorer** en Visual Web Developer) y, a continuación, haga clic en **las conexiones de datos**y seleccione **crear nueva base de datos de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

En el **crear nueva base de datos de SQL Server** diálogo cuadro, escriba ". \SQLExpress" en el **nombre del servidor** cuadro y "aspnet-Test" en la **nuevo nombre de base de datos** cuadro y, después, haga clic en **Aceptar**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Siga el mismo procedimiento para crear una nueva base de datos de la escuela de SQL Server Express denominada "School-Test".

(Está anexando "Test" a estos nombres de base de datos porque más adelante creará una instancia adicional de cada base de datos para el entorno de desarrollo, y deberá ser capaz de diferenciar los dos conjuntos de bases de datos.)

**Explorador de servidores** ahora muestra las dos bases de datos.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Creación de un Script de concesión para las bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación tiene acceso a la base de datos mediante las credenciales del grupo de aplicaciones predeterminado. Sin embargo, de forma predeterminada, la identidad del grupo de aplicación no tiene permiso para abrir las bases de datos. Por lo tanto, tendrá que ejecutar un script para conceder ese permiso. En esta sección creará la secuencia de comandos que se va a ejecutar más adelante para asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

En la solución *SolutionFiles* carpeta que creó en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, cree un nuevo archivo SQL denominado *Grant.sql*. Copie los siguientes comandos SQL en el archivo y, a continuación, guarde y cierre el archivo:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Este script está diseñado para trabajar con SQL Server 2008 y con la configuración de IIS en Windows 7, como se especifican en este tutorial. Si usa una versión diferente de SQL Server o de Windows, o si configura IIS en un equipo diferente, los cambios realizados en esta secuencia de comandos pueden ser necesarios. Para obtener más información acerca de los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota de seguridad** esta secuencia de comandos proporciona db\_permisos de propietario para el usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, puede especificar un usuario que tiene el esquema de base de datos completa, actualizar los permisos para la implementación y especificar para el tiempo de ejecución de un usuario diferente que tenga permisos solo para leer y escribir datos. Para obtener más información, consulte **revisar los cambios de Web.config automática para migraciones de Code First** en [implementar en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Configuración de la implementación de la base de datos para el entorno de prueba

A continuación, se configurará Visual Studio para que realizará las tareas siguientes para cada base de datos:

- Generar un script SQL que crea la estructura de la base de datos del origen (tablas, columnas, restricciones, etc.) en la base de datos de destino.
- Generar un script SQL que inserta datos de la base de datos del origen en las tablas de la base de datos de destino.
- Ejecute los scripts generados y la secuencia de comandos de concesión que creó en la base de datos de destino.

Abra el **las propiedades del proyecto** ventana y seleccione el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **activo (versión)** o **versión** está seleccionado en el **configuración** lista desplegable.

Haga clic en **habilitar esta página**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

El **Empaquetar/publicar SQL** ficha normalmente está deshabilitada porque especifica un método de implementación heredada. Para la mayoría de los escenarios, debe configurar la implementación de base de datos en el **publicación Web** asistente. Migración de SQL Server Compact a SQL Server o SQL Server Express es un caso especial para el que este método es una buena elección.

Haga clic en **importar desde Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio busca cadenas de conexión en el *Web.config* archivo, busca uno para la base de datos de pertenencia y otro para la base de datos School y agrega una fila correspondiente a cada cadena de conexión en el **entradas de la base de datos**  tabla. Encuentra las cadenas de conexión son las bases de datos de SQL Server Compact existente y el siguiente paso será configurar cómo y dónde implementar estas bases de datos.

Especificar configuración de implementación de la base de datos en el **detalles de la base de datos de entrada** sección más adelante el **entradas de la base de datos** tabla. La configuración que se muestra en el **detalles de la base de datos de entrada** sección pertenecen a lo que la fila en la **entradas de la base de datos** tabla está seleccionada, como se muestra en la siguiente ilustración.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuración de las opciones de implementación para la base de datos de pertenencia

Seleccione el **DefaultConnection implementación** de fila en la **entradas de la base de datos** tabla con el fin de configurar las opciones que se aplican a la base de datos de pertenencia.

En **cadena de conexión de base de datos de destino**, escriba una cadena de conexión que apunta a la nueva base de pertenencia de SQL Server Express. Puede obtener la cadena de conexión que necesita de **Explorador de servidores**. En **Explorador de servidores**, expanda **conexiones de datos** y seleccione el **aspnetTest** base de datos, a continuación, en el **propiedades** copia ventana el **Cadena de conexión** valor.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

A continuación, se reproduce la misma cadena de conexión:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copie y pegue esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **extraer datos y/o esquema de una base de datos** está seleccionada. Esto es lo que genera scripts SQL que se genera automáticamente y se ejecutan en la base de datos de destino.

El **cadena de conexión para la base de datos de origen** se extrae el valor de la *Web.config* archivo y apunta a la base de datos de SQL Server Compact de desarrollo. Se trata de la base de datos de origen que se usará para generar los scripts que se ejecutarán más adelante en la base de datos de destino. Puesto que va a implementar la versión de producción de la base de datos, cambiar "aspnet-Dev.sdf" a "aspnet-Prod.sdf".

Cambio **opciones de scripting de base de datos** desde **solo esquema** a **esquema y los datos**, ya que van a copiar los datos (cuentas de usuario y roles), así como la estructura de base de datos.

Para configurar la implementación para ejecutar los scripts de concesión que creó anteriormente, tendrá que agregarlos a la **secuencias de comandos de base de datos** sección. Haga clic en **agregar Script**y en el **agregar secuencias de comandos SQL** cuadro de diálogo, navegue hasta la carpeta donde guardó el script de la concesión (Esto es la carpeta que contiene el archivo de solución). Seleccione el archivo denominado *Grant.sql*y haga clic en **abierto**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

La configuración de la **DefaultConnection implementación** la fila del **entradas de la base de datos** parecerse ahora a la siguiente ilustración:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de las opciones de implementación para la base de datos School

A continuación, seleccione el **SchoolContext implementación** de fila en la **entradas de la base de datos** tabla con el fin de configurar las opciones de implementación para la base de datos School.

Puede usar el mismo método que usó anteriormente para obtener la cadena de conexión para la nueva base de datos SQL Server Express. Copie esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Asegúrese de que **extraer datos y/o esquema de una base de datos** está seleccionada.

El **cadena de conexión para la base de datos de origen** se extrae el valor de la *Web.config* archivo y apunta a la base de datos de SQL Server Compact de desarrollo. Cambie "School-Dev.sdf" a "School-Prod.sdf" para implementar la versión de producción de la base de datos. (Nunca creó un archivo de la escuela Prod.sdf en la aplicación\_carpeta de datos, por lo que deberá copiar ese archivo desde el entorno de prueba a la aplicación\_carpeta de datos en la carpeta del proyecto ContosoUniversity más adelante.)

Cambio **opciones de scripting de base de datos** a **esquema y los datos**.

También puede ejecutar el script para conceder y permiso de escritura para esta base de datos a la identidad del grupo de aplicaciones, por tanto, agregue el *Grant.sql* como lo hizo para la base de datos de pertenencia a los archivos de scripts.

Cuando haya terminado, la configuración el **SchoolContext implementación** la fila del **entradas de la base de datos** ser similar a la siguiente ilustración:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Guarde los cambios en el **Empaquetar/publicar SQL** ficha.

Copia el *School-Prod.sdf* de archivos desde el *c:\inetpub\wwwroot\ContosoUniversity\App\_datos* carpeta a la *aplicación\_datos* carpeta en el proyecto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Especifica el modo de transacción para la secuencia de comandos de concesión

El proceso de implementación genera scripts que implementación el esquema de base de datos y los datos. De forma predeterminada, estos scripts se ejecutan en una transacción. Sin embargo, los scripts personalizados (por ejemplo, los scripts de concesión) de forma predeterminada no se ejecutan en una transacción. Si el proceso de implementación mezcla los modos de transacción, puede aparecer un error de tiempo de espera al ejecutarán los scripts durante la implementación. En esta sección, edita el archivo de proyecto con el fin de configurar los scripts personalizados se ejecuten en una transacción.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** del proyecto y seleccione **descargar el proyecto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

A continuación, haga clic en el proyecto nuevo y seleccione **editar ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

El editor de Visual Studio muestra el contenido XML del archivo del proyecto. Tenga en cuenta que hay varios `PropertyGroup` elementos. (En la imagen, el contenido de la `PropertyGroup` elementos se han omitido.)

![Ventana del editor de archivos de proyecto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

La primera de ellas, que no tiene ningún `Condition` atributo, es para los valores que se aplican con independencia de la configuración de compilación. Una `PropertyGroup` elemento solo se aplica a la configuración de compilación de depuración (tenga en cuenta el `Condition` atributo), uno solo se aplica a la configuración de compilación de lanzamiento y uno solo se aplica a la configuración de compilación de prueba. Dentro de la `PropertyGroup` elemento para la configuración de compilación de lanzamiento, verá un `PublishDatabaseSettings` elemento que contiene los valores que escribió en el **Empaquetar/publicar SQL** ficha. Hay un `Object` elemento que corresponde a cada una de las secuencias de comandos de concesión (tenga en cuenta las dos instancias especificadas de "Grant.sql"). De forma predeterminada, el `Transacted` atributo de la `Source` (elemento) para cada secuencia de comandos de concesión es `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Cambie el valor de la `Transacted` atributo de la `Source` elemento `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Guarde y cierre el archivo de proyecto y, a continuación, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **recargar el proyecto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurar transformaciones de Web.Config para las cadenas de conexión

Las cadenas de la conexión para el nuevo SQL Express bases de datos que se escribió el **Empaquetar/publicar SQL** ficha sirven únicamente para actualizar la base de datos de destino durante la implementación en Web Deploy. También tendrá que configurar *Web.config* transformaciones para que las cadenas de la conexión en implementado *Web.config* archivo, seleccione las bases de datos nueva de SQL Server Express. (Cuando se usa el **Empaquetar/publicar SQL** ficha, no se puede configurar las cadenas de conexión del perfil de publicación.)

Abra *Web.Test.config* y reemplace el `connectionStrings` elemento con el `connectionStrings` elemento en el ejemplo siguiente. (Asegúrese de que solo copiar el elemento connectionStrings, no en el código circundante que se muestra aquí para proporcionar contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Este código hace que el `connectionString` y `providerName` atributos de cada uno `add` elemento a sustituir en implementado *Web.config* archivo. Estas cadenas de conexión no son idénticas a los que se escribió en el **Empaquetar/publicar SQL** ficha. La configuración "MultipleActiveResultSets = True" se ha agregado a ellos porque es necesario para Entity Framework y los proveedores universales.

## <a name="installing-sql-server-compact"></a>Instalar SQL Server Compact

El paquete SqlServerCompact NuGet proporciona la base de datos de SQL Server Compact los ensamblados del motor de la aplicación Contoso University. Pero ahora no es la aplicación pero Web Deploy que debe ser capaz de leer las bases de datos de SQL Server Compact, con el fin de crear scripts para ejecutarlos en las bases de datos de SQL Server. Para habilitar Web Deploy leer las bases de datos de SQL Server Compact, instalar SQL Server Compact en el equipo de desarrollo con el siguiente vínculo: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Implementación en el entorno de prueba

Para publicar en el entorno de prueba, tiene que crear un perfil de publicación que está configurado para usar el **Empaquetar/publicar SQL** pestaña de la base de datos de publicación en lugar de la configuración de base de datos de perfil de publicación.

En primer lugar, elimine el perfil de prueba existente.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **administrar perfiles**.

Seleccione **prueba**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cerrar la **publicación Web** Asistente para guardar este cambio.

A continuación, cree un nuevo perfil de prueba y usarlo para publicar el proyecto.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Seleccione **&lt;nuevo... &gt;** en la lista desplegable lista y escriba "Test" como el nombre del perfil.

En el **dirección URL del servicio** , escriba *localhost*.

En el **sitio o aplicación** , escriba *Default Web Site/ContosoUniversity*.

En el **dirección URL de destino** , escriba `http://localhost/ContosoUniversity/`.

Haga clic en **Siguiente**.

El **configuración** ficha le advierte que el **Empaquetar/publicar SQL** ficha se ha configurado, y le brinda la oportunidad para reemplazar al hacer clic en habilitar la base de datos nuevas mejoras de publicación. Para esta implementación no desea reemplazar el **Empaquetar/publicar SQL** pestaña Configuración, así que haga clic en **siguiente**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un mensaje en el **Preview** ficha indica que **ninguna base de datos se han seleccionado para la publicación**, pero esto sólo significa que la publicación de la base de datos no está configurada en el perfil de publicación.

Haga clic en **Publicar**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio implementa la aplicación y abre el explorador a la página principal del sitio en el entorno de prueba. Ejecute la página de instructores para ver que muestra los mismos datos que vio anteriormente. Ejecute el **agregar estudiantes** página, agregue un nuevo alumno y, a continuación, ver el alumno nuevo en el **estudiantes** página. Esto comprueba que se puede actualizar la base de datos. Seleccione el **actualización créditos** página (deberá iniciar sesión) para comprobar que se implementó la base de datos de pertenencia y tiene acceso a él.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Creación de una base de datos SQL Server para el entorno de producción

Ahora que ha implementado en el entorno de prueba, está listo para configurar la implementación en producción. Comenzar como hizo en el entorno de prueba mediante la creación de una base de datos para implementar en. Como se indicó en la información general, el plan de hospedaje Cytanium Lite solo permite una sola base de datos de SQL Server, por lo que se va a configurar una base de datos, no dos. Todas las tablas y datos de la pertenencia y las bases de datos School SQL Server Compact se implementará en una base de datos de SQL Server en producción.

Vaya al panel de control en Cytanium [ http://panel.cytanium.com ](http://panel.cytanium.com). Mantenga el mouse sobre **bases de datos** y, a continuación, haga clic en **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

En el **SQL Server 2008** página, haga clic en **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nombre de la base de datos "School" y haga clic en **guardar**. (La página agrega automáticamente el prefijo "contosou", por lo que el nombre efectivo será "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

En la misma página, haga clic en **Create User**. En los servidores del Cytanium, en lugar de usar seguridad integrada de Windows y dejar que la identidad del grupo de aplicaciones de la base de datos, creará un usuario que tiene autoridad para abrir la base de datos. Agregue las credenciales del usuario a las cadenas de conexión que van en la producción *Web.config* archivo. En este paso creará esas credenciales.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Rellene los campos obligatorios en el **propiedades de usuario de SQL** página:

- Escriba el nombre "ContosoUniversityUser".
- Escriba una contraseña.
- Seleccione **contosouSchool** como base de datos predeterminada.
- Seleccione el **contosouSchool** casilla de verificación.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configuración de la implementación de la base de datos para el entorno de producción

Ahora está listo para configurar las opciones de implementación de base de datos en el **Empaquetar/publicar SQL** pestaña, como lo hizo anteriormente para el entorno de prueba.

Abra el **las propiedades del proyecto** ventana, seleccione el **Empaquetar/publicar SQL** pestaña y asegúrese de que **activo (versión)** o **versión** es seleccionado en el **configuración** lista desplegable.

Cuando configura las opciones de implementación para cada base de datos, la diferencia clave entre lo que hacer para entornos de producción y pruebas está en cómo configurar cadenas de conexión. El entorno de prueba que ha especificado las cadenas de conexión de base de datos de destino diferente, pero para el entorno de producción, la cadena de conexión de destino será el mismo para ambas bases de datos. Esto es porque se va a implementar ambas bases de datos en una base de datos en producción.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuración de las opciones de implementación para la base de datos de pertenencia

Para configurar los valores que se aplican a la base de datos de pertenencia, seleccione el **DefaultConnection implementación** de fila en la **entradas de la base de datos** tabla.

En **cadena de conexión de base de datos de destino**, escriba una cadena de conexión que apunta a la base de datos de SQL Server de producción nueva que acaba de crear. Puede obtener la cadena de conexión de su correo electrónico de bienvenida. La parte correspondiente del correo electrónico contiene la cadena de conexión de ejemplo siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Después de reemplazar las tres variables, la cadena de conexión que tiene este aspecto:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copie y pegue esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **extraer datos y/o esquema de una base de datos** es aún seleccionado y que **opciones de scripting de base de datos** sigue siendo **esquema y los datos**.

En el **secuencias de comandos de base de datos** cuadro, desactive la casilla de verificación situada junto a la secuencia de comandos Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de las opciones de implementación para la base de datos School

A continuación, seleccione el **SchoolContext implementación** de fila en la **entradas de la base de datos** tabla para configurar las opciones de base de datos School.

Copie la misma cadena de conexión en **cadena de conexión de base de datos de destino** que ha copiado en ese campo para la base de datos de pertenencia.

Asegúrese de que **extraer datos y/o esquema de una base de datos** es aún seleccionado y que **opciones de scripting de base de datos** sigue siendo **esquema y los datos**.

En el **secuencias de comandos de base de datos** cuadro, desactive la casilla de verificación situada junto a la secuencia de comandos Grant.sql.

Guarde los cambios en el **Empaquetar/publicar SQL** ficha.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Transformaciones de configuración de Web.Config para las cadenas de conexión a bases de datos de producción

A continuación, va a configurar *Web.config* transformaciones para que las cadenas de la conexión en implementado *Web.config* archivo para que apunte a la nueva base de datos de producción. La cadena de conexión que ha escrito en el **Empaquetar/publicar SQL** ficha para Web Deploy utilice es el mismo que el que la aplicación debe usar, excepto por la adición de la `MultipleResultSets` opción.

Abra *Web.Production.config* y reemplace el `connectionStrings` elemento con un `connectionStrings` elemento que es similar al ejemplo siguiente. (Solo copiar la `connectionStrings` elemento, no las etiquetas adyacentes que se proporcionan para mostrar el contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

En ocasiones verá consejos que le indica que siempre cifrar cadenas de conexión en el *Web.config* archivo. Esto podría ser adecuado si implementara a servidores de red de su propia compañía. Sin embargo, debe confiar en las prácticas de seguridad del proveedor de hospedaje cuando se implementa en un entorno de hospedaje compartido, y no es necesario ni práctico cifrar las cadenas de conexión.

## <a name="deploying-to-the-production-environment"></a>Implementación en el entorno de producción

Ahora está listo para implementarse en producción. Web Deploy leerá las bases de datos de SQL Server Compact en el proyecto *aplicación\_datos* carpeta y vuelva a crear todas sus tablas y datos en la base de datos de SQL Server de producción. Para poder publicar utilizando la **Empaquetar/Publicar Web** configuración de las tabulaciones, tendrá que crear un nuevo perfil de publicación para la producción.

En primer lugar, debe eliminar el perfil de producción existente como lo hizo anteriormente el perfil de prueba.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **administrar perfiles**.

Seleccione **producción**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cerrar la **publicación Web** Asistente para guardar este cambio.

A continuación, cree un nuevo perfil de producción y usarlo para publicar el proyecto.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **importación**y seleccione el archivo .publishsettings que descargó anteriormente.

En el **conexión** , modifique la **dirección URL de destino** a la URL correcta temporal, que en este ejemplo es http://contosouniversity.com.vserver01.cytanium.com.

Cambie el nombre del perfil de producción. (Seleccione la **perfil** ficha y haga clic en **administrar perfiles** hacerlo).

Cerrar la **publicación Web** Asistente guarde los cambios.

En una aplicación real en el que se actualizaba la base de datos en producción, podría hacer ahora dos pasos adicionales antes de publicar:

1. Cargar *aplicación\_offline.htm*, tal y como se muestra en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.
2. Use la **administrador archivos** característica del panel de control Cytanium para copiar el *aspnet Prod.sdf* y *School-Prod.sdf* archivos desde el sitio de producción a la *Aplicación\_datos* carpeta del proyecto ContosoUniversity. Esto garantiza que los datos que se va a implementar en la nueva base de datos de SQL Server incluyen las últimas actualizaciones realizadas por el sitio Web de producción.

En el **una publicación en Web clic** barra de herramientas, asegúrese de que el **producción** perfil está seleccionada y, a continuación, haga clic en **publicar**.

Si se cargan <em>aplicación\_offline.htm</em> antes de la publicación, tendrá que usar el <strong>Administrador de archivos</strong> utilidad en el panel de control Cytanium eliminar <em>aplicación\_sin conexión.</em> htm antes de probar. Puede también al mismo tiempo elimina el <em>.sdf</em> archivos desde el <em>aplicación\_datos</em> carpeta.

Ahora puede abrir un explorador y vaya a la dirección URL del sitio público para probar la aplicación de la misma manera que después de implementar en el entorno de prueba.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Cambio a SQL Server Express LocalDB en el desarrollo

Tal como se explicó en la información general, es suele ser preferible utilizar el mismo motor de base de datos en el desarrollo que usar en producción y prueba. (Recuerde que la ventaja de usar SQL Server Express en desarrollo es que la base de datos funcionan igual en sus entornos de desarrollo, prueba y producción). En esta sección podrá configurar el proyecto ContosoUniversity para usar SQL Server Express LocalDB cuando se ejecuta la aplicación desde Visual Studio.

La manera más sencilla de realizar esta migración es que Code First y el sistema de pertenencia cree ambas bases de datos de desarrollo nuevos para usted. Con este método para migrar requiere tres pasos:

1. Cambiar las cadenas de conexión para especificar nuevas bases de datos de SQL Express LocalDB.
2. Ejecute la herramienta de administración de sitios Web para crear un usuario administrador. Esto crea la base de datos de pertenencia.
3. Use el comando update-database migraciones de Code First para crear e inicializar la base de datos de aplicación.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Actualizando cadenas de conexión en el archivo Web.config

Abra el *Web.config* de archivo y reemplace el `connectionStrings` elemento con el código siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Creación de la base de datos de pertenencia

En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity y, a continuación, haga clic en **configuración de ASP.NET** en el **proyecto** menú.

Seleccione la ficha seguridad.

Haga clic en **crear o administrar Roles**y, a continuación, cree un **administrador** rol.

Vuelva a la ficha seguridad.

Haga clic en **crear usuario**y, a continuación, seleccione el **administrador** casilla de verificación y cree un usuario denominado administrador.

Cerrar la **herramienta Administración de sitios Web**.

### <a name="creating-the-school-database"></a>Creación de la base de datos School

Abra la ventana de consola de administrador de paquetes.

En el **proyecto predeterminado** lista desplegable, seleccione el proyecto ContosoUniversity.DAL.

Escriba el comando siguiente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migraciones de Code First se aplica a la migración inicial que crea la base de datos y, a continuación, se aplica a la migración AddBirthDate, entonces se ejecutará el método de inicialización.

Ejecute el sitio presionando F5 de Control. Al igual que para los entornos de prueba y producción, ejecute el **agregar estudiantes** página, agregue un nuevo alumno y, a continuación, ver el alumno nuevo en el **estudiantes** página. Esto comprueba que la base de datos School creado e inicializado y que ha leído y acceso de escritura a él.

Seleccione el **actualización créditos** página e inicie sesión para comprobar que se implementó la base de datos de pertenencia y que tiene acceso a él. Si no se ha migrado las cuentas de usuario, cree una cuenta de administrador y, a continuación, seleccione el **actualización créditos** página para comprobar que funciona.

## <a name="cleaning-up-sql-server-compact-files"></a>Limpiar los archivos SQL Server Compact

Ya no necesita los archivos y paquetes de NuGet que se incluyeron para admitir SQL Server Compact. Si desea (este paso no es necesario), puede limpiar los archivos innecesarios y las referencias.

En **el Explorador de soluciones**, elimine el *.sdf* archivos desde el *aplicación\_datos* carpeta y el *amd64* y *x86* carpetas desde el *bin* carpeta.

En **el Explorador de soluciones**, haga clic en la solución (no de uno de los proyectos) y, a continuación, haga clic en **administrar paquetes de NuGet para la solución**.

En el panel izquierdo de la **administrar paquetes de NuGet** cuadro de diálogo, seleccione **paquetes instalados**.

Seleccione el **EntityFramework.SqlServerCompact** del paquete y haga clic en **administrar**.

En el **seleccionar proyectos** cuadro de diálogo, se seleccionan ambos proyectos. Para desinstalar el paquete en ambos proyectos, desactive las casillas de verificación y, a continuación, haga clic en **Aceptar**.

En el cuadro de diálogo que le pregunta si desea desinstalar también los paquetes dependientes, haga clic en no. Uno de ellos es el paquete de Entity Framework que se debe mantener.

Siga el mismo procedimiento para desinstalar el **SqlServerCompact** paquete. (Los paquetes deben desinstalarse en este orden, porque el **EntityFramework.SqlServerCompact** paquete depende de la **SqlServerCompact** paquete.)

Ahora ha migrado correctamente a SQL Server Express y completa de SQL Server. En el siguiente tutorial, podrá realizar otro cambio de base de datos y verá cómo implementar los cambios de la base de datos cuando las bases de datos de prueba y producción utilizan SQL Server Express y completa de SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
