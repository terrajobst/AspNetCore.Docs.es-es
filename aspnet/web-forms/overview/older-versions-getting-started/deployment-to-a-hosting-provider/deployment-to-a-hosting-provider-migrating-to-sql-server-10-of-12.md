---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrar a SQL Server - 10 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 31d83a11488212ab0ff83494d5e896ffcbeaa8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrar a SQL Server - 10 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo migrar de SQL Server Compact a SQL Server. Puede hacer que una de las razones es aprovechar las características de SQL Server que SQL Server Compact no admite, como procedimientos almacenados, desencadenadores, vistas o replicación. Para obtener más información acerca de las diferencias entre SQL Server Compact y SQL Server, consulte el [implementar SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express frente a completa de SQL Server para el desarrollo

Una vez que haya decidido actualizar a SQL Server, puede usar SQL Server o SQL Server Express en sus entornos de desarrollo y pruebas. Además de las diferencias en la compatibilidad con las herramientas y características del motor de base de datos, existen diferencias en las implementaciones del proveedor entre SQL Server Compact y otras versiones de SQL Server. Estas diferencias pueden ocasionar el mismo código generar resultados diferentes. Por lo tanto, si decide mantener SQL Server Compact como la base de datos de desarrollo, debe probar exhaustivamente su sitio en SQL Server o SQL Server Express en un entorno de prueba antes de cada implementación en producción.

A diferencia de SQL Server Compact, SQL Server Express es básicamente el mismo motor de base de datos y utiliza el mismo proveedor de .NET como completa de SQL Server. Cuando se prueba con SQL Server Express, puede estar seguro de obtener los mismos resultados tal y como aparecerá con SQL Server. Puede utilizar la mayoría de las mismas herramientas de base de datos con SQL Server Express que pueden usar con SQL Server (una excepción notable que se va a [SQL Server Profiler](https://msdn.microsoft.com/en-us/library/ms181091.aspx)), y es compatible con otras características de SQL Server como procedimientos almacenados, vistas, desencadenadores, y la replicación. (Normalmente tendrá que usar completa de SQL Server en un sitio Web de producción, sin embargo. SQL Server Express se puede ejecutar en un entorno de hospedaje compartido, pero no se diseñó para, y muchos proveedores de hospedaje no la admiten.)

Si usas Visual Studio 2012, normalmente elegir SQL Server Express LocalDB para su entorno de desarrollo ya que es lo que está instalado de forma predeterminada con Visual Studio. Sin embargo, LocalDB no funciona en IIS, por lo que para el entorno de prueba deba usar SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinar las bases de datos frente a mantenerlos independiente

La aplicación de la Universidad de Contoso tiene dos bases de datos de SQL Server Compact: la base de datos de pertenencia (*aspnet.sdf*) y la base de datos de aplicación (*School.sdf*). Cuando se migra, puede migrar las bases de datos a dos bases de datos o a una sola base de datos. Desea combinarlas con el fin de facilitar las combinaciones de base de datos entre la base de datos de aplicación y la base de datos de pertenencia. El plan de hospedaje también puede proporcionar una razón para combinarlos. Por ejemplo, el proveedor de hospedaje puede cobrar otras cuestiones para varias bases de datos o incluso podría no permitir a más de una base de datos. Que es el caso con Cytanium Lite hospeda la cuenta que se usa para este tutorial, lo que permite una sola base de SQL Server.

En este tutorial, deberá migrar las dos bases de datos de esta forma:

- Migrar a dos bases de datos de LocalDB en el entorno de desarrollo.
- Migrar a dos bases de datos de SQL Server Express en el entorno de prueba.
- Migrar a SQL Server completa combinada uno con una base de datos en el entorno de producción.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalar SQL Server Express

SQL Server Express se instala automáticamente con Visual Studio 2010 de forma predeterminada, pero de forma predeterminada no se instala con Visual Studio 2012. Para instalar SQL Server 2012 Express, haga clic en el vínculo siguiente

- [SQL Server Express 2012](https://www.microsoft.com/en-us/download/details.aspx?id=29062)

Elija *ENU/x64/SQLEXPR\_x64\_ENU.exe* o *ENU/x86/SQLEXPR\_x86\_ENU.exe*y en el Asistente para la instalación, acepte el valor predeterminado Configuración. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server 2012 desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/en-us/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Creación de bases de datos SQL Server Express para el entorno de prueba

El siguiente paso es crear la pertenencia a ASP.NET y las bases de datos School.

Desde el **vista** menú, seleccione **Explorador de servidores** (**el Explorador de base de datos** en Visual Web Developer) y, a continuación, haga clic en **las conexiones de datos**y seleccione **crear nueva base de datos de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

En el **crear nueva base de datos de SQL Server** diálogo cuadro, escriba ". \SQLExpress" en la **nombre del servidor** cuadro y "aspnet-Test" en la **nuevo nombre de base de datos** cuadro, a continuación, haga clic en **Aceptar**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Siga el mismo procedimiento para crear una nueva base de datos de la escuela de SQL Server Express denominada "Escuela-Test".

(Se anexa "Test" a estos nombres de base de datos porque más adelante creará una instancia adicional de cada base de datos para el entorno de desarrollo, y debe ser capaz de distinguir los dos conjuntos de bases de datos.)

**Explorador de servidores** muestra ahora las dos bases de datos.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Crear una secuencia de comandos de concesión para las bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación obtiene acceso a la base de datos utilizando las credenciales del grupo de aplicaciones predeterminado. Sin embargo, de forma predeterminada, la identidad del grupo de aplicación no tiene permiso para abrir las bases de datos. Por lo que tendrá que ejecutar un script para conceder ese permiso. En esta sección creará el script que se va a ejecutar más adelante si desea asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

En la solución *SolutionFiles* carpeta que creó en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, cree un nuevo archivo SQL denominado *Grant.sql*. Copie los siguientes comandos SQL en el archivo y, a continuación, guarde y cierre el archivo:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Esta secuencia de comandos está diseñado para trabajar con SQL Server 2008 y con la configuración de IIS en Windows 7, tal y como se especifican en este tutorial. Si usa una versión diferente de SQL Server o de Windows, o si ha configurado IIS en el equipo diferente, los cambios en esta secuencia de comandos podrían ser necesarios. Para obtener más información acerca de los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota de seguridad** esta secuencia de comandos proporciona db\_permisos de propietario para el usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, puede especificar un usuario que tiene el esquema de base de datos completa actualizar los permisos para la implementación, y especificar para el tiempo de ejecución un usuario diferente que tenga permisos sólo para leer y escribir datos. Para obtener más información, consulte **revisar los cambios de Web.config automática para migraciones de Code First** en [implementación en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>La configuración de implementación de base de datos para el entorno de prueba

A continuación, podrá configurar Visual Studio para que realizará las tareas siguientes para cada base de datos:

- Generar un script SQL que crea la estructura de la base de datos del origen (tablas, columnas, restricciones, etc.) en la base de datos de destino.
- Generar un script SQL que inserta datos de la base de datos del origen en las tablas de la base de datos de destino.
- Ejecutar los scripts generados y la secuencia de comandos de concesión que ha creado, en la base de datos de destino.

Abra la **propiedades del proyecto** ventana y seleccione el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **activo (versión)** o **versión** está seleccionado en el **configuración** lista desplegable.

Haga clic en **habilitar esta página**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

El **Empaquetar/publicar SQL** ficha normalmente está deshabilitada porque especifica un método de implementación heredada. Para la mayoría de los escenarios, debe configurar la implementación de la base de datos en el **Publicar Web** asistente. Migración de SQL Server Compact a SQL Server o SQL Server Express es un caso especial para el que este método es una buena elección.

Haga clic en **importar desde Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio busca cadenas de conexión en el *Web.config* archivo, busca una para la base de datos de pertenencia y otra para la base de datos School y agrega una fila correspondiente a cada cadena de conexión en el **las entradas de la base de datos**  tabla. Las cadenas de conexión que se encuentra son para las bases de datos de SQL Server Compact existentes, y será el siguiente paso configurar cómo y dónde implementar estas bases de datos.

Especificar configuración de implementación de base de datos en el **detalles de la entrada de base de datos** sección más adelante el **las entradas de la base de datos** tabla. La configuración se muestra en el **detalles de la entrada de base de datos** sección pertenecen a lo que la fila en la **entradas de la base de datos** tabla está seleccionada, tal como se muestra en la siguiente ilustración.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuración de la implementación de la base de datos de pertenencia

Seleccione el **DefaultConnection implementación** de fila en la **entradas de la base de datos** tabla con el fin de configurar las opciones que se aplican a la base de datos de pertenencia.

En **cadena de conexión de base de datos de destino**, escriba una cadena de conexión que apunta a la nueva base de pertenencia de SQL Server Express. Puede obtener la cadena de conexión que necesita de **Explorador de servidores**. En **Explorador de servidores**, expanda **las conexiones de datos** y seleccione la **aspnetTest** base de datos, a continuación, desde el **propiedades** copia de la ventana el **Cadena de conexión** valor.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

La misma cadena de conexión se reproduce aquí:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copie y pegue esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **extraer datos y/o esquema de una base de datos existente** está seleccionada. Esto es lo que hace que las secuencias de comandos SQL se generan automáticamente y se ejecutan en la base de datos de destino.

El **cadena de conexión para la base de datos de origen** se extrae el valor de la *Web.config* archivo y apunta a la base de datos de SQL Server Compact de desarrollo. Se trata de la base de datos de origen que se usará para generar los scripts que se ejecutarán más adelante en la base de datos de destino. Puesto que desea implementar la versión de producción de la base de datos, cambie "aspnet-Dev.sdf" a "aspnet-Prod.sdf".

Cambio **opciones de scripting de base de datos** de **solo esquema** a **esquema y los datos**, ya que van a copiar los datos (cuentas de usuario y roles), así como la estructura de base de datos.

Para configurar la implementación para ejecutar las secuencias de comandos de concesión que creó anteriormente, tendrá que agregarlos a la **secuencias de comandos de base de datos** sección. Haga clic en **agregar Script**y en el **agregar secuencias de comandos SQL** cuadro de diálogo, navegue hasta la carpeta donde guardó la secuencia de comandos de concesión (ésta es la carpeta que contiene el archivo de solución). Seleccione el archivo denominado *Grant.sql*y haga clic en **abiertos**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

La configuración de la **DefaultConnection implementación** de fila en **entradas de la base de datos** parecerse a la siguiente ilustración:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de la implementación de la base de datos School

A continuación, seleccione la **SchoolContext implementación** de fila en la **entradas de la base de datos** tabla para configurar opciones de implementación para la base de datos School.

Puede usar el mismo método que usó previamente para obtener la cadena de conexión para la base de datos nueva de SQL Server Express. Copie esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Asegúrese de que **extraer datos y/o esquema de una base de datos existente** está seleccionada.

El **cadena de conexión para la base de datos de origen** se extrae el valor de la *Web.config* archivo y apunta a la base de datos de SQL Server Compact de desarrollo. Cambie "Escuela Dev.sdf" a "Escuela-Prod.sdf" para implementar la versión de producción de la base de datos. (Nunca se creó un archivo Prod.sdf School en la aplicación\_carpeta de datos, por lo que deberá copiar dicho archivo desde el entorno de prueba en la aplicación\_carpeta de datos en la carpeta del proyecto ContosoUniversity más adelante.)

Cambio **opciones de scripting de base de datos** a **esquema y los datos**.

También puede ejecutar el script para conceder la lectura y permisos de escritura en esta base de datos a la identidad del grupo de aplicaciones, así que agregue el *Grant.sql* que escribió para la base de datos de pertenencia a los archivos de scripts.

Cuando haya terminado, la configuración la **SchoolContext implementación** de fila en **entradas de la base de datos** el aspecto de la siguiente ilustración:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Guardar los cambios a la **Empaquetar/publicar SQL** ficha.

Copia la *School-Prod.sdf* de archivos desde el *c:\inetpub\wwwroot\ContosoUniversity\App\_datos* carpeta a la *aplicación\_datos* carpeta en el proyecto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Especificar el modo de transacción para la secuencia de comandos de concesión

El proceso de implementación genera scripts que implementan el esquema de base de datos y los datos. De forma predeterminada, estas secuencias de comandos se ejecutan en una transacción. Sin embargo, los scripts personalizados (por ejemplo, las secuencias de comandos de concesión) de forma predeterminada no se ejecutan en una transacción. Si el proceso de implementación mezcla los modos de transacción, podría obtener un error de tiempo de espera cuando se ejecutan los scripts durante la implementación. En esta sección, edita el archivo de proyecto con el fin de configurar los scripts personalizados para que se ejecute en una transacción.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** de proyecto y seleccione **descargar el proyecto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

A continuación, haga clic en el proyecto nuevo y seleccione **ContosoUniversity.csproj editar**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

El editor de Visual Studio muestra el contenido XML del archivo del proyecto. Observe que hay varias `PropertyGroup` elementos. (En la imagen, el contenido de la `PropertyGroup` elementos se han omitido.)

![Ventana del editor de archivos de proyecto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

La primera de ellas, que no tiene ningún `Condition` atributo, se hace para valores que se aplican con independencia de la configuración de compilación. Una `PropertyGroup` elemento solo se aplica a la configuración de compilación de depuración (tenga en cuenta el `Condition` atributo), uno solo se aplica a la configuración de compilación de lanzamiento, y uno solo se aplica a la configuración de compilación de prueba. En el `PropertyGroup` elemento para la configuración de compilación de lanzamiento, verá un `PublishDatabaseSettings` elemento que contiene los valores que especificó en el **Empaquetar/publicar SQL** ficha. Hay un `Object` elemento que corresponde a cada una de las secuencias de comandos de concesión (Observe las dos instancias especificadas de "Grant.sql"). De forma predeterminada, el `Transacted` atributo de la `Source` elemento por cada secuencia de comandos de concesión es `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Cambie el valor de la `Transacted` atributo de la `Source` elemento `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Guarde y cierre el archivo de proyecto y, a continuación, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **recargar el proyecto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configuración de las transformaciones de Web.Config para las cadenas de conexión

Las cadenas de la conexión para el nuevo SQL Express bases de datos que se escriben en el **Empaquetar/publicar SQL** pestaña solo se usan Web Deploy para actualizar la base de datos de destino durante la implementación. También tendrá que configurar *Web.config* transformaciones para que las cadenas de la conexión en implementado *Web.config* archivo, seleccione las nuevas bases de datos SQL Server Express. (Cuando se usa el **Empaquetar/publicar SQL** ficha, no se puede configurar cadenas de conexión en el perfil de publicación.)

Abra *Web.Test.config* y reemplace el `connectionStrings` elemento con la `connectionStrings` elemento en el ejemplo siguiente. (Asegúrese de que solo copiar el elemento connectionStrings, no en el código adyacente que se muestra aquí para proporcionar un contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Este código hace que la `connectionString` y `providerName` atributos de cada `add` elemento se reemplacen en implementado *Web.config* archivo. Estas cadenas de conexión no son idénticas a los que escribió en el **Empaquetar/publicar SQL** ficha. El valor "MultipleActiveResultSets = True" se ha agregado a ellas porque es necesario para Entity Framework y los proveedores universales.

## <a name="installing-sql-server-compact"></a>Instalar SQL Server Compact

El paquete de SqlServerCompact NuGet proporciona la base de datos de SQL Server Compact ensamblados de motor para la aplicación de la Universidad de Contoso. Pero ahora no es la aplicación pero Web Deploy que debe tener permiso para leer las bases de datos de SQL Server Compact, con el fin de crear scripts para que se ejecute en las bases de datos de SQL Server. Para habilitar Web Deploy leer las bases de datos de SQL Server Compact, instalar SQL Server Compact en el equipo de desarrollo con el siguiente vínculo: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Implementación en el entorno de prueba

Para publicar en el entorno de prueba, tendrá que crear un perfil de publicación que está configurado para usar el **Empaquetar/publicar SQL** ficha para base de datos de publicación en lugar de la configuración de base de datos de perfil de publicación.

En primer lugar, elimine el perfil de prueba existente.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **administrar perfiles de**.

Seleccione **prueba**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cerrar la **Publicar Web** Asistente para guardar los cambios.

A continuación, crear un nuevo perfil de prueba y usarlo para publicar el proyecto.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Seleccione  **&lt;nuevo... &gt;**  en la lista desplegable lista y, escriba "Test" como el nombre del perfil.

En el **dirección URL del servicio** cuadro, escriba *localhost*.

En el **sitio o aplicación** cuadro, escriba *sitio Web predeterminado/ContosoUniversity*.

En el **dirección URL de destino** cuadro, escriba `http://localhost/ContosoUniversity/`.

Haga clic en **Siguiente**.

El **configuración** ficha le advierte que el **Empaquetar/publicar SQL** ficha se ha configurado, y le da la oportunidad de invalidación, haga clic en habilitar la base de datos nuevas mejoras de publicación. Para esta implementación que no desea reemplazar el **Empaquetar/publicar SQL** pestaña Configuración, por lo que basta con hacer clic **siguiente**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un mensaje en el **vista previa** ficha indica que **ninguna base de datos se han seleccionado para la publicación**, pero esto sólo significa que la publicación de la base de datos no está configurada en el perfil de publicación.

Haga clic en **Publicar**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio implementa la aplicación y abre el explorador a la página principal del sitio en el entorno de prueba. Ejecute la página de instructores para ver que muestra los mismos datos que vio anteriormente. Ejecute el **agregar estudiantes** página, agregue un nuevo alumno y, a continuación, ver el nuevo alumno en el **estudiantes** página. Esto comprueba que el se puede actualizar la base de datos. Seleccione el **actualización créditos** página (que necesite iniciar sesión) para comprobar que se implementó la base de datos de pertenencia y tiene acceso a él.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Crear una base de datos SQL Server para el entorno de producción

Ahora que se han implementado en el entorno de prueba, está listo para configurar la implementación de producción. Comenzar como hizo en el entorno de prueba mediante la creación de una base de datos para implementar en. Tal y como recuerda la información general, el plan de hospedaje Cytanium Lite solo permite una sola base de datos de SQL Server, por lo que vamos a configurar una base de datos, no dos. Todas las tablas y los datos de la pertenencia y las bases de datos School SQL Server Compact se implementará en una base de datos de SQL Server en producción.

Vaya al panel de control Cytanium en [http://panel.cytanium.com](http://panel.cytanium.com). Mantenga el mouse sobre **bases de datos** y, a continuación, haga clic en **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

En el **SQL Server 2008** página, haga clic en **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nombre de la base de datos "Escuela" y haga clic en **guardar**. (La página automáticamente agrega el prefijo "contosou", por lo que el nombre efectivo será "contosouSchool").

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

En la misma página, haga clic en **Create User**. En servidores de Cytanium, en lugar de usar seguridad integrada de Windows y dejar que la identidad del grupo de aplicaciones a abrir la base de datos, creará un usuario que tenga autoridad para abrir la base de datos. Va a agregar las credenciales del usuario a las cadenas de conexión que van en la producción *Web.config* archivo. En este paso creará esas credenciales.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Rellene los campos obligatorios en el **propiedades de usuario de SQL** página:

- Escriba "ContosoUniversityUser" como el nombre.
- Escriba una contraseña.
- Seleccione **contosouSchool** como la base de datos de forma predeterminada.
- Seleccione el **contosouSchool** casilla de verificación.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>La configuración de implementación de base de datos para el entorno de producción

Ahora está listo para configurar la configuración de implementación de base de datos en el **Empaquetar/publicar SQL** ficha, como lo hizo anteriormente para el entorno de prueba.

Abra la **propiedades del proyecto** ventana, seleccione la **Empaquetar/publicar SQL** pestaña y asegúrese de que **activo (versión)** o **versión** es seleccionado en el **configuración** lista desplegable.

Al configurar valores de implementación para cada base de datos, la principal diferencia entre lo que hacer para entornos de producción y de prueba está en cómo configurar cadenas de conexión. El entorno de prueba que ha especificado las cadenas de conexión de base de datos de destino diferente, pero para el entorno de producción, la cadena de conexión de destino será el mismo para ambas bases de datos. Esto es porque va a implementar ambas bases de datos en una base de datos de producción.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuración de la implementación de la base de datos de pertenencia

Para configurar los valores que se aplican a la base de datos de pertenencia, seleccione la **DefaultConnection implementación** de fila en la **entradas de la base de datos** tabla.

En **cadena de conexión de base de datos de destino**, escriba una cadena de conexión que apunta a la base de datos de SQL Server de producción nueva que acaba de crear. Puede obtener la cadena de conexión desde el correo electrónico de bienvenida. La parte correspondiente del correo electrónico contiene la cadena de conexión de ejemplo siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Después de reemplazar las tres variables, la cadena de conexión que necesita es similar en este ejemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copie y pegue esta cadena de conexión en **cadena de conexión de base de datos de destino** en el **Empaquetar/publicar SQL** ficha.

Asegúrese de que **extraer datos y/o esquema de una base de datos** es aún seleccionado y que **opciones de scripting de base de datos** sigue siendo **esquema y los datos**.

En el **secuencias de comandos de base de datos** cuadro, desactive la casilla de verificación situada junto a la secuencia de comandos Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de la implementación de la base de datos School

A continuación, seleccione la **SchoolContext implementación** de fila en la **entradas de la base de datos** tabla para configurar las opciones de base de datos School.

Copie la misma cadena de conexión en **cadena de conexión de base de datos de destino** que ha copiado en ese campo de la base de datos de pertenencia.

Asegúrese de que **extraer datos y/o esquema de una base de datos** es aún seleccionado y que **opciones de scripting de base de datos** sigue siendo **esquema y los datos**.

En el **secuencias de comandos de base de datos** cuadro, desactive la casilla de verificación situada junto a la secuencia de comandos Grant.sql.

Guardar los cambios a la **Empaquetar/publicar SQL** ficha.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Configuración de Web.Config se transforma para las cadenas de conexión a bases de datos de producción

A continuación, configure las *Web.config* transformaciones para que las cadenas de la conexión en implementado *Web.config* archivo para que apunte a la nueva base de datos de producción. La cadena de conexión que ha especificado en el **Empaquetar/publicar SQL** ficha de Web Deploy usar es el mismo que la aplicación debe usar, excepto por la adición de la `MultipleResultSets` opción.

Abra *Web.Production.config* y reemplace el `connectionStrings` elemento con un `connectionStrings` elemento que es similar al ejemplo siguiente. (Copiar solo la `connectionStrings` elemento, no las etiquetas adyacentes que se proporcionan para mostrar el contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

A veces puede ver consejos que indica si debe siempre cifrar cadenas de conexión en el *Web.config* archivo. Esto podría ser adecuado si estuviera implementando a servidores de red de su empresa. Cuando va a implementar en un entorno de hospedaje compartido, sin embargo, está confiando en las prácticas de seguridad del proveedor de hospedaje, y no es necesario ni práctico cifrar las cadenas de conexión.

## <a name="deploying-to-the-production-environment"></a>Implementación en el entorno de producción

Ahora está listo para implementarse en producción. Web Deploy leerá las bases de datos de SQL Server Compact en el proyecto *aplicación\_datos* carpeta y vuelva a crear todas sus tablas y datos en la base de datos de SQL Server de producción. Para poder publicar mediante la **Empaquetar/Publicar Web** configuración de las tabulaciones, tendrá que crear un nuevo perfil de publicación para la producción.

En primer lugar, debe eliminar el perfil de producción existente como lo hizo anteriormente el perfil de prueba.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **administrar perfiles de**.

Seleccione **producción**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cerrar la **Publicar Web** Asistente para guardar los cambios.

A continuación, crear un nuevo perfil de producción y usarla para publicar el proyecto.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione el **perfil** ficha.

Haga clic en **importación**y seleccione el archivo .publishsettings que descargó anteriormente.

En el **conexión** , modifique la **dirección URL de destino** a la URL correcta temporal, que en este ejemplo es http://contosouniversity.com.vserver01.cytanium.com.

Cambie el nombre del perfil de producción. (Seleccione la **perfil** ficha y haga clic en **administrar perfiles** hacerlo).

Cerrar la **Publicar Web** Asistente para guardar los cambios.

En una aplicación real en el que se está actualizando la base de datos en producción, haría lo ahora dos pasos adicionales antes de publicar:

1. Cargar *aplicación\_offline.htm*, tal y como se muestra en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.
2. Use la **el Administrador de archivos** característica del panel de control Cytanium para copiar el *aspnet Prod.sdf* y *School-Prod.sdf* archivos desde el sitio de producción a la *Aplicación\_datos* carpeta del proyecto ContosoUniversity. Esto garantiza que los datos que se va a implementar la nueva base de datos de SQL Server incluyen las actualizaciones más recientes realizadas por el sitio Web de producción.

En el **Web uno haga clic en publicar** barra de herramientas, asegúrese de que el **producción** perfil está seleccionada y, a continuación, haga clic en **publicar**.

Si cargó *aplicación\_offline.htm* antes de la publicación, deberá utilizar el **el Administrador de archivos** utilidad en el panel de control de Cytanium eliminar *aplicación\_sin conexión.* htm antes de probar. También puede al mismo tiempo eliminar el *.sdf* archivos desde el *aplicación\_datos* carpeta.

Ahora puede abrir un explorador y vaya a la dirección URL de su sitio público para probar la aplicación de la misma manera que después de implementar en el entorno de prueba.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Cambiar a SQL Server Express LocalDB en el desarrollo

Tal y como se ha explicado en la información general, es suele ser preferible utilizar el mismo motor de base de datos en desarrollo que use en producción y pruebas. (Recuerde que la ventaja de utilizar SQL Server Express en el desarrollo es que la base de datos funcionan igual en los entornos de desarrollo, prueba y producción). En esta sección podrá configurar el proyecto ContosoUniversity para usar SQL Server Express LocalDB cuando se ejecuta la aplicación desde Visual Studio.

La manera más sencilla de realizar esta migración es dejar que Code First y el sistema de pertenencia a crear las nuevas bases de datos de desarrollo para usted. Con este método para migrar requiere tres pasos:

1. Cambiar las cadenas de conexión para especificar nuevas bases de datos de SQL Express LocalDB.
2. Ejecute la herramienta de administración de sitios Web para crear un usuario de administrador. Esto crea la base de datos de pertenencia.
3. Use el comando de actualización de base de datos de migraciones de Code First para crear e inicializar la base de datos de aplicación.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Actualizar las cadenas de conexión en el archivo Web.config

Abra la *Web.config* archivo y reemplazar el `connectionStrings` elemento con el código siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Crear la base de datos de pertenencia

En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity y, a continuación, haga clic en **de configuración de ASP.NET** en el **proyecto** menú.

Seleccione la ficha seguridad.

Haga clic en **crear o administrar funciones**y, a continuación, cree un **administrador** rol.

Volver a la ficha seguridad.

Haga clic en **crear usuario**y, a continuación, seleccione la **administrador** casilla de verificación y crea un usuario denominado administrador.

Cerrar la **herramienta de administración de sitios Web**.

### <a name="creating-the-school-database"></a>Crear la base de datos School

Abra la ventana de la consola de administrador de paquetes.

En el **proyecto predeterminado** lista desplegable, seleccione el proyecto ContosoUniversity.DAL.

Escriba el comando siguiente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migraciones de Code First se aplica a la migración inicial que crea la base de datos y, a continuación, se aplica la migración de AddBirthDate, a continuación, se ejecuta el método de inicialización.

Ejecute el sitio Web presionando F5 de Control. Igual que para los entornos de prueba y producción, ejecute el **agregar estudiantes** página, agregue un nuevo alumno y, a continuación, ver el nuevo alumno en el **estudiantes** página. Esto comprueba que la base de datos School creado e inicializado y que ha leído y acceso de escritura a él.

Seleccione el **actualización créditos** página e inicie sesión para comprobar que se implementó la base de datos de pertenencia y que tiene acceso a él. Si no se ha migrado las cuentas de usuario, cree una cuenta de administrador y, a continuación, seleccione la **actualización créditos** página para comprobar que funciona.

## <a name="cleaning-up-sql-server-compact-files"></a>Limpiar los archivos SQL Server Compact

Ya no necesita los archivos y los paquetes de NuGet que se incluyeron para admitir SQL Server Compact. Si desea (este paso no es necesario), puede limpiar los archivos innecesarios y referencias.

En **el Explorador de soluciones**, eliminar el *.sdf* archivos desde el *aplicación\_datos* carpeta y el *amd64* y *x86* carpetas desde la *bin* carpeta.

En **el Explorador de soluciones**, haga clic en la solución (no de uno de los proyectos) y, a continuación, haga clic en **administrar paquetes de NuGet para la solución**.

En el panel izquierdo de la **administrar paquetes de NuGet** cuadro de diálogo, seleccione **paquetes instalados**.

Seleccione el **EntityFramework.SqlServerCompact** del paquete y haga clic en **administrar**.

En el **seleccionar proyectos** cuadro de diálogo, se seleccionan ambos proyectos. Para desinstalar el paquete en los dos proyectos, desactive las casillas de verificación y, a continuación, haga clic en **Aceptar**.

En el cuadro de diálogo que le pregunta si desea desinstalar los paquetes dependientes también, haga clic en no. Uno de ellos es el paquete de Entity Framework que se debe mantener.

Siga el mismo procedimiento para desinstalar el **SqlServerCompact** paquete. (Los paquetes deben desinstalarse en este orden porque la **EntityFramework.SqlServerCompact** paquete depende de la **SqlServerCompact** paquete.)

Ahora ha migrado correctamente a SQL Server Express y completa de SQL Server. En el siguiente tutorial realizará otro cambio de base de datos y aprenderá a implementar los cambios de la base de datos cuando las bases de datos de prueba y producción usan SQL Server Express y completa de SQL Server.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
