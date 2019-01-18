---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Implementación Web de ASP.NET con Visual Studio: Implementación de prueba | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396342"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implementación Web de ASP.NET con Visual Studio: Implementación de prueba
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales muestra cómo implementar (publicar) de ASP.NET en Azure App Service Web Apps o a un proveedor de hospedaje de terceros con Visual Studio 2017 de aplicación web. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general

En este tutorial, implementará una aplicación web ASP.NET en Internet Information Server (IIS) en el equipo local.

Por lo general al desarrollar una aplicación, ejecutarlo y probarlo en Visual Studio. De forma predeterminada, los proyectos de aplicación web en Visual Studio 2017 usan IIS Express como el servidor web de desarrollo. IIS Express se comporta más como completa de IIS que Visual Studio Development Server (también conocido como Cassini), que Visual Studio 2017 usa de forma predeterminada. Pero ningún servidor de desarrollo web funciona exactamente igual que IIS. Por lo tanto, una aplicación podría ejecutar y probar correctamente en Visual Studio pero producirá un error cuando se implementa en IIS.

Confiable puede probar la aplicación de dos maneras:

1. Implementar la aplicación en IIS en el equipo de desarrollo con el mismo proceso que usará más adelante para implementarlo en su entorno de producción.

   Puede configurar Visual Studio para que use IIS cuando se ejecuta un proyecto web, pero que no sería probar el proceso de implementación. Este método valida su proceso de implementación y que la aplicación se ejecuta correctamente en IIS.

2. Implementar la aplicación en un entorno de prueba similar al entorno de producción. 
 
   El entorno de producción para estos tutoriales es Web Apps en Azure App Service. El entorno de prueba ideal es una aplicación web adicional creada en el servicio de Azure. Aunque se podría configurar la misma manera que una aplicación web de producción, solo se podría usar para realizar pruebas.

Opción 2 es la manera más confiable para probar. Si usa la opción 2, necesariamente no es necesario usar la opción 1. Sin embargo si va a implementar a un tercero, proveedor de hospedaje opción2 quizás no sea factible o podría ser costosa, por lo que esta serie de tutoriales muestra ambos métodos. Se proporciona orientación para la opción 2 en el [implementarla en el entorno de producción](deploying-to-production.md) tutorial.

Para obtener más información sobre el uso de servidores web en Visual Studio, consulte [servidores Web en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Aviso: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Descargar el proyecto de inicio de Contoso University

Descargue e instale la solución de inicio de Contoso University Visual Studio y el proyecto. Esta solución contiene el tutorial completado. 

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Instalar IIS

Para implementar en IIS en el equipo de desarrollo, confirme que están instalado IIS y Web Deploy. De forma predeterminada, Visual Studio instala Web Deploy, pero IIS no está incluida en la configuración predeterminada de Windows 10, Windows 8 o Windows 7. Si ya ha instalado IIS y el grupo de aplicaciones predeterminada ya está establecido en .NET 4, vaya a [la siguiente sección](#sqlexpress).

1. Se recomienda que use el [instalador de plataforma Web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) para instalar IIS y Web Deploy. WPI instala una configuración recomendada de IIS que incluye los requisitos previos de IIS y Web Deploy si es necesario.

   Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, la WPI instala solo lo que falta.

   * Use el instalador de plataforma Web para instalar IIS y Web Deploy:
    
     ![Instale IIS siguiendo WPI](deploying-to-iis/_static/image30.png)

     ![Instalar Web Deploy mediante WPI](deploying-to-iis/_static/image31.png)

     Verá mensajes que indican que se instalará IIS 7. El vínculo funciona para IIS 8 en Windows 8; pero para Windows 8 y versiones posteriores, siga los pasos siguientes para asegurarse de que está instalado ASP.NET 4.7:

   * Abra **Panel de Control** > **programas** > **programas y características** > **o desactivar las características de Windows activa** .

   * Expanda **Internet Information Services**, **servicios World Wide Web**, y **Application Development Features**.
   
   * Confirme que **ASP.NET 4.7** está seleccionada.

     ![Seleccione ASP.NET 4.7](deploying-to-iis/_static/image1a.png)

   * Confirme que **servicios World Wide Web** y **consola de administración IIS** está seleccionada. Esto instala IIS y el Administrador de IIS.
    
     ![Seleccione Servicios World Wide Web](deploying-to-iis/_static/image24.png)    
  
   * Seleccione **Aceptar**. Aparecen mensajes de cuadro de diálogo que indica está realizando la instalación.

Después de instalar IIS, ejecute **IIS Manager** para asegurarse de que la versión 4 de .NET Framework se asigna al grupo de aplicaciones de forma predeterminada.

1. Presione WINDOWS + R para abrir el **ejecutar** cuadro de diálogo.

   (En Windows 8 o posterior, escriba "run" el **iniciar** página. En Windows 7, seleccione **ejecutar** desde el **iniciar** menú. Si **ejecutar** no se encuentra en la **iniciar** menú, haga clic en la barra de tareas, seleccione **propiedades**, seleccione el **menú Inicio** pestaña, seleccione **Personalizar**y seleccione **ejecutar comando**.)

2. Escriba "inetmgr" y seleccione **Aceptar**.

3. En el **conexiones** panel, expanda el nodo del servidor y seleccione **grupos de aplicaciones**. En el **grupos de aplicaciones** panel si **DefaultAppPool** está asignado a la versión de .NET framework 4 como se muestra en la ilustración siguiente, vaya a la sección siguiente.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Si ve solo dos grupos de aplicaciones y ambos se establecen en .NET Framework 2.0, instale ASP.NET 4 en IIS.

   Para Windows 8 o posterior, consulte las instrucciones de la sección anterior para asegurarse de que ese ASP.NET 4.7 está instalado o [cómo instalar ASP.NET 4.5 en Windows 8 y Windows Server 2012](https://support.microsoft.com/kb/2736284). Para Windows 7, abra una ventana del símbolo del sistema haciendo clic **símbolo** en el Windows **iniciar** menú y seleccionando **ejecutar como administrador**. Ejecute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar ASP.NET 4 en IIS mediante los siguientes comandos. (En los sistemas de 32 bits, reemplace "Framework64" con "Framework").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Este comando crea la nueva aplicación que seguirá siendo grupos para .NET Framework 4, pero el grupo de aplicaciones predeterminada establecida en 2.0. Va a implementar una aplicación que los destinos de .NET 4 para ese grupo de aplicaciones, por lo que cambiar el grupo de aplicaciones en .NET 4.

5. Si ha cerrado **IIS Manager**, ejecútelo de nuevo, expanda el nodo del servidor y seleccione **grupos de aplicaciones**.

6. En el **grupos de aplicaciones** panel, seleccione **DefaultAppPool**. En el **acciones** panel, seleccione **configuración básica**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. En el **Modificar grupo de aplicaciones** cuadro de diálogo, cambie el **versión de .NET CLR** a **.NET CLR v4.0.30319**. Seleccione **Aceptar**.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Ahora está listo para publicar una aplicación web en IIS. En primer lugar, sin embargo, crear bases de datos para las pruebas.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instale SQL Server Express

LocalDB no está diseñado para funcionar en IIS, por lo que el entorno de prueba debe tener instalado SQL Server Express. Si usa Visual Studio 2010 SQL Server Express, ya está instalado de forma predeterminada. Si usa Visual Studio 2012 o posterior, instale SQL Server Express.

Para instalar SQL Server Express, descargue e instale desde [centro de descarga: Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

En la primera página del centro de instalación de SQL Server, seleccione **instalación independiente del nuevo servidor SQL Server o agregar características a una instalación existente** y siga las instrucciones que acepte las opciones predeterminadas. En el Asistente para la instalación, acepte la configuración predeterminada. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Crear bases de datos de SQL Server Express del entorno de prueba

La aplicación Contoso University tiene dos bases de datos: 

1. Base de datos de pertenencia 
2. base de datos de aplicación 

Puede implementar estas bases de datos para dos bases de datos independientes o para una sola base de datos. Combinarlas hace que las combinaciones de base de datos entre ellos sea más fácil. 

Si va a implementar en un proveedor de hospedaje de terceros, el plan de hospedaje también podría proporcionar un motivo para combinarlos. Por ejemplo, el proveedor podría cargar más de varias bases de datos o podría incluso no permitir más de una base de datos.

En este tutorial, implementará dos bases de datos en el entorno de prueba y una base de datos en los entornos de ensayo y producción.

Desde el **vista** menú en Visual Studio, seleccione **Explorador de servidores** (**Database Explorer** en Visual Web Developer). Haga clic en **conexiones de datos** y seleccione **crear nueva base de datos de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

En el **crear nueva base de datos de SQL Server** diálogo cuadro, escriba ". \SQLExpress" en el **nombre del servidor** cuadro y "aspnet-ContosoUniversity" en el **nuevo nombre de base de datos** cuadro. Seleccione **Aceptar**.

![Crear aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Siga el mismo procedimiento para crear una nueva base de datos de la escuela de SQL Server Express denominada `ContosoUniversity`.

**Explorador de servidores** muestra las dos bases de datos.

![Nuevas bases de datos en el Explorador de servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Crear un script de concesión para las bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación utiliza las credenciales del grupo de aplicaciones predeterminado para tener acceso a la base de datos. Sin embargo, de forma predeterminada, el grupo de aplicaciones no tiene permiso para abrir las bases de datos. Esto significa que necesita ejecutar un script para conceder ese permiso. En esta sección, podrá crear ese script y ejecutarlo más adelante para asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

En un editor de texto, copie los siguientes comandos SQL en un nuevo archivo y guárdelo como *Grant.sql*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

En Visual Studio, abra la solución de Contoso University. Haga clic en la solución (no de uno de los proyectos) y seleccione **agregar**. Seleccione **elemento existente**, vaya a *Grant.sql*y ábralo.

> [!NOTE]
> Este script está diseñado para trabajar con SQL Server Express 2012 o posterior y con la configuración de IIS en Windows 10, Windows 8 o Windows 7 tal como se especifican en este tutorial. Si usa una versión diferente de SQL Server o Windows, o si configura IIS en un equipo diferente, los cambios realizados en esta secuencia de comandos pueden ser necesarios. Para obtener más información acerca de los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Nota de seguridad** ofrece esta secuencia de comandos `db_owner` permisos al usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, es posible que desea especificar un usuario que tenga el esquema de base de datos completa actualizar los permisos para la implementación y especifique para el tiempo de ejecución de un usuario diferente que tenga permisos solo para leer y escribir datos. Para obtener más información, consulte [revisar los cambios de Web.config automática para migraciones de Code First](#reviewingmigrations) más adelante en este tutorial.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Ejecute el script de concesión en la base de datos de aplicación

Puede configurar el perfil de publicación para ejecutar el script de concesión en la base de datos de pertenencia durante la implementación debido a que la implementación base de datos utiliza el [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) proveedor. No se puede ejecutar scripts durante la implementación de migraciones de Code First, que es cómo va a implementar la base de datos de aplicación. Esto significa que tiene que ejecutar manualmente el script antes de la implementación de la base de datos de aplicación.

1. En Visual Studio, abra el *Grant.sql* archivo que creó anteriormente.

2. Seleccione **conectar**. 

    ![Botón Conectar](deploying-to-iis/_static/image11.png)

3. En el **conectar al servidor** diálogo cuadro, escriba *. \SQLExpress* como el **nombre del servidor**. Seleccione **conectar**.

4. En la lista desplegable de base de datos seleccione **ContosoUniversity**. Seleccione **ejecutar**. 

   ![](deploying-to-iis/_static/image12.png)

La identidad del grupo de aplicaciones predeterminado ahora tiene permisos suficientes en la base de datos de aplicación para migraciones de Code First crear las tablas de base de datos cuando se ejecuta la aplicación.

## <a name="publish-to-iis"></a>Publicación en IIS

Hay varias maneras que puede implementar en IIS con Visual Studio y Web Deploy:

* Publicar el uso de Visual Studio con un solo clic.
* Publicar desde la línea de comandos.
* Crear un paquete de implementación e instalarlo mediante el Administrador de IIS. El paquete tiene un archivo ZIP con todos los archivos y los metadatos necesarios para instalar un sitio en IIS.
* Crear un paquete de implementación e instalarlo mediante la línea de comandos.

El proceso que llevamos a cabo en los tutoriales anteriores para configurar Visual Studio para automatizar las tareas de implementación se aplica a todos estos métodos. En estos tutoriales, usará los dos primeros métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [implementar una aplicación web, cree e instale un paquete de implementación web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) en el mapa de contenido de implementación Web para Visual Studio y ASP.NET.

Antes de la publicación, asegúrese de que está ejecutando Visual Studio en modo de administrador. Si no ve **(Administrador)** en la barra de título, cierre Visual Studio. En Windows 8 (o posterior) **iniciar** página o el de Windows 7 **iniciar** menú, haga clic en el icono de Visual Studio y seleccione **ejecutar como administrador**. Modo de administrador sólo es necesario para la publicación cuando se va a publicar en IIS en el equipo local.

### <a name="create-the-publish-profile"></a>Crear el perfil de publicación

1. En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto (no la **ContosoUniversity.DAL** proyecto). Seleccione **Publicar**. El **publicar** aparece la página.

2. Seleccione **nuevo perfil**. El **elegir un destino de publicación** aparece el cuadro de diálogo.

3. Seleccione **IIS, FTP, etcetera**. Seleccione **Crear perfil**. El **publicar** aparece el asistente.

   ![Ficha perfil de Asistente de Web de publicación](deploying-to-iis/_static/image26.png)

4. Desde el **método Publish** menú desplegable, seleccione **Web Deploy**.

5. Para **Server**, escriba *localhost*.

6. Para **nombre del sitio**, escriba *Default Web Site/ContosoUniversity*.

7. Para **dirección URL de destino**, escriba *http://localhost/ContosoUniversity*.

   El **dirección URL de destino** configuración no es necesaria. Cuando Visual Studio termine de implementar la aplicación, se abre automáticamente el explorador predeterminado para esta dirección URL. Si no desea que el explorador para abrir automáticamente después de la implementación, deje este cuadro en blanco.

8. Seleccione **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

   Una marca de verificación verde comprueba que la conexión es correcta.

   ![Publicar la ficha de conexión del Asistente de Web](deploying-to-iis/_static/image27.png)

9. Seleccione **siguiente** para avanzar a la **configuración** ficha.

10. El **configuración** cuadro desplegable especifica la configuración de compilación para implementar. Déjelo establecido en el valor predeterminado de **versión**. No se puede implementar compilaciones de depuración en este tutorial.

11. Expanda **opciones de publicación de archivos**. Seleccione **excluir archivos de la aplicación\_carpeta de datos**.

    En el entorno de prueba, la aplicación tiene acceso a las bases de datos que creó en la instancia local de SQL Server Express, no los archivos .mdf en la *aplicación\_datos* carpeta.

12. Deje el **precompilar durante la publicación** y **quitar archivos adicionales en destino** casillas desactivadas.

    ![Opciones de publicación del archivo en la pestaña Configuración](deploying-to-iis/_static/image15a.png)

    Precompilar es una opción que es útil principalmente para sitios de gran tamaño. Puede reducir el tiempo de inicio de la primera vez que se solicita una página después de publicar el sitio.

    No es necesario quitar archivos adicionales, ya que se trata de la primera implementación y no habrá ningún archivo en la carpeta de destino aún.

    > [!NOTE] 
    > Si selecciona **quitar archivos adicionales en destino** para una implementación subsiguientes en el mismo sitio, asegúrese de usar la característica de vista previa para que ver con antelación qué archivos se eliminarán antes de implementar. El comportamiento esperado es que Web Deploy se eliminarán los archivos del servidor de destino que ha eliminado en el proyecto. Sin embargo, se compara la estructura de carpeta completa en las carpetas de origen y destino; y, en algunos escenarios, Web Deploy podría eliminar archivos que no desea eliminar.
    > 
    > Por ejemplo si tiene una aplicación web en una subcarpeta en el servidor al implementar un proyecto en la carpeta raíz, se eliminará la subcarpeta. Podría tener un proyecto para el sitio principal en contoso.com y otro proyecto para un blog en contoso.com/blog. La aplicación de blog está en una subcarpeta. Si selecciona **quitar archivos adicionales en destino** al implementar el sitio principal, se eliminará la aplicación de blog.
    > 
    > Para obtener otro ejemplo, la aplicación\_carpeta de datos deben eliminarse inesperadamente. Ciertas bases de datos como SQL Server Compact almacenan archivos de base de datos en la aplicación\_carpeta de datos. Tras la implementación inicial, que no desea mantener copiar los archivos de base de datos en las implementaciones posteriores, así que seleccionamos **Excluir aplicación\_datos** en la pestaña Empaquetar/Publicar Web. Después de hacer esa if tiene **quitar archivos adicionales en destino** seleccionada, los archivos de base de datos y la aplicación\_propia carpeta de datos se eliminarán la próxima vez que publique.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar la implementación de la base de datos de pertenencia

Los pasos siguientes se aplican a la **DefaultConnection** base de datos en el cuadro de diálogo **bases de datos** sección.

1. En el **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunta a la nueva base de pertenencia de SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   El proceso de implementación pone esta cadena de conexión en el archivo Web.config implementado porque **usar esta cadena de conexión en tiempo de ejecución** está seleccionada.

    También puede obtener la cadena de conexión de **Explorador de servidores**. En **Explorador de servidores**, expanda **conexiones de datos** y seleccione el  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** base de datos, a continuación, en el **propiedades** copia ventana el **cadena de conexión** valor. Cadena de conexión que tenga una configuración adicional que puede eliminar: `Pooling=False`.

2. Seleccione **Actualizar base de datos**.

   Esto hace que el esquema de base de datos que se creará en la base de datos de destino durante la implementación. En los pasos siguientes, especifique las secuencias de comandos adicionales que necesite ejecutar: uno para conceder acceso de la base de datos en el grupo de aplicaciones predeterminado y otro para implementar los datos.

3. Seleccione **configurar actualizaciones de base de datos**.
 
4. En el **configurar actualizaciones de base de datos** cuadro de diálogo, seleccione **Agregar secuencia de comandos SQL**. Navegue hasta la *Grant.sql* script que guardó anteriormente en la carpeta de soluciones.

5. Repita el proceso para agregar el *aspnet-data-dev.sql* secuencia de comandos.

   ![Configurar las actualizaciones de base de datos de base de datos de pertenencia](deploying-to-iis/_static/image16.png)

6. Seleccione **cerrar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar la implementación de la base de datos de aplicación

Cuando Visual Studio detecta un Entity Framework `DbContext` (clase), crea una entrada en el **bases de datos** sección que tiene un **ejecutar migraciones Code First** casilla de verificación en lugar de un  **Actualizar base de datos** casilla de verificación. Para este tutorial, usará esa casilla de verificación para especificar la implementación de migraciones de Code First.

En algunos escenarios, puede que esté usando un `DbContext` base de datos pero desea usar el proveedor dbDacFx en lugar de las migraciones para implementar la base de datos. En ese caso, consulte [cómo se puede implementar una base de datos Code First sin migraciones?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) en las P+F de implementación Web de ASP.NET en MSDN.

Los pasos siguientes se aplican a la **SchoolContext** base de datos en el cuadro de diálogo **bases de datos** sección.

1. En el **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunta a la nueva base de aplicación de SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   El proceso de implementación pone esta cadena de conexión en el archivo Web.config implementado porque **usar esta cadena de conexión en tiempo de ejecución** está seleccionada.

   También puede obtener la cadena de conexión de base de datos de aplicación de **Explorador de servidores** de la misma manera que obtuvo la cadena de conexión de base de datos de pertenencia.

2. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**.

   Esta opción hace que el proceso de implementación configurar el archivo Web.config implementado para especificar el `MigrateDatabaseToLatestVersion` inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación tiene acceso a la base de datos por primera vez después de la implementación.

### <a name="configure-publish-profile-transforms"></a>Configurar transformaciones de perfil de publicación

1. Seleccione **cerrar**. Seleccione **Sí** cuando se le pregunte si desea guardar los cambios.

2. En **el Explorador de soluciones**, expanda **propiedades**, expanda **PublishProfiles**.

3. Haga clic en *CustomProfile.pubxml* y cámbiele el nombre *Test.pubxml*.

4. Haga clic en *Test.pubxml*. Seleccione **Agregar transformación de configuración**.

   ![Agregar menú de transformación de configuración](deploying-to-iis/_static/image17.png)

   Visual Studio crea el *Web.Test.config* archivo de transformación y lo abre.

5. En el *Web.Test.config* transformar el archivo, inserte el código siguiente inmediatamente después de abrir etiqueta de configuración.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Cuando se utiliza la prueba de perfil de publicación, esta transformación establece el indicador de entorno a "Test". En el sitio implementado, verá "(prueba)" después del encabezado H1 "Contoso University".

6. Guarde y cierre el archivo.

7. Haga clic en el *Web.Test.config* de archivo y seleccione **Preview transformar** para asegurarse de que la transformación ha codificado genera los cambios previstos.

    El **Web.config Preview** ventana muestra el resultado de aplicar el *Web.Release.config* transforma y *Web.Test.config* transforma.

### <a name="preview-the-deployment-updates"></a>Vista previa de la implementación de actualizaciones

1. Abra el **publicación Web** nuevo asistente (haga clic en el proyecto ContosoUniversity, seleccione **publicar**, a continuación, **Preview**).

2. En el **Preview** cuadro de diálogo, seleccione **iniciar vista previa** para ver una lista de los archivos que se va a copiar. 

   ![Vista previa de publicación](deploying-to-iis/_static/image29.png)

   También puede seleccionar la **base de datos de vista previa** vínculo para ver las secuencias de comandos que se ejecutarán en la base de datos de pertenencia. (No hay scripts se ejecutan para la implementación de migraciones de Code First, por lo que no hay nada que obtener una vista previa de la base de datos de la aplicación.)

3. Seleccione **Publicar**.

   Si Visual Studio no está en modo de administrador, puede aparecer un mensaje de error de permisos. En ese caso, cierre Visual Studio, abrirlo en modo de administrador y, intente volver a publicar.

   Si Visual Studio está en modo de administrador, el **salida** informes correcta de la ventana de compilación y publicación.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Si escribió la dirección URL en el **dirección URL de destino** cuadro en el perfil de publicación **conexión** ficha, el explorador se abre automáticamente en la página principal de la Universidad de Contoso que se ejecutan en IIS en el equipo.

## <a name="test-in-the-test-environment"></a>Probar en el entorno de prueba

Tenga en cuenta que el indicador de entorno muestra "(prueba)" en lugar de "(desarrollo)", que muestra que el *Web.config* transformación para el indicador de entorno fue correcta.

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos de un instructor. Al seleccionar esta página, puede tardar unos minutos en cargarse porque Code First crea la base de datos y, a continuación, se ejecuta el `Seed` método. (No hace cuando estaban en la página principal porque la aplicación no intenta tener acceso a la base de datos todavía.)

Seleccione el **estudiantes** pestaña para comprobar que la base de datos implementada no tiene ningún estudiante.

Seleccione **agregar estudiantes** desde el **estudiantes** menú. Agregar un estudiante y, a continuación, ver el alumno nuevo en el **estudiantes** página. Esto comprueba que puede escribir correctamente en la base de datos.

Desde el **cursos** menú, seleccione **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **en el registro** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "devpwd"). El **actualización créditos** se muestra la página. Esto comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Compruebe que un *ELMAH* carpeta existe en el *c:\inetpub\wwwroot\ContosoUniversity* carpeta con el archivo de marcador de posición en él.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revise los cambios de Web.config automática para migraciones de Code First

Abra el *Web.config* archivo en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y puede ver que el proceso de implementación configurado migraciones de Code First para automáticamente actualizar la base de datos a la versión más reciente.

![](deploying-to-iis/_static/image21.png)

El proceso de implementación también crea una nueva cadena de conexión para migraciones de Code First exclusivamente actualizar el esquema de base de datos:

![Cadena de conexión Database_Publish](deploying-to-iis/_static/image22.png)

Esta cadena de conexión adicionales le permite especificar una cuenta de usuario para las actualizaciones del esquema de base de datos y una cuenta de usuario diferente para el acceso a datos de aplicación. Por ejemplo, podría asignar el **db\_propietario** rol para migraciones de Code First y **db\_datareader** con **db\_datawriter**roles a la aplicación. Se trata de un patrón común de defensa en profundidad que impide que el código potencialmente malintencionado en la aplicación modifiquen el esquema de base de datos. (Por ejemplo, esto puede suceder en un ataque de inyección SQL.) Estos tutoriales no usan este patrón. Para implementar este patrón en su escenario, siga estos pasos:

1. En el **publicación Web** Asistente bajo el **configuración** , escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquema de base de datos completa. Desactive el **usar esta cadena de conexión en tiempo de ejecución** casilla de verificación. En el archivo Web.config implementado, esto se convierte en el `DatabasePublish` cadena de conexión.

2. Crear una transformación del archivo Web.config para la cadena de conexión que desea que la aplicación para usar en tiempo de ejecución.

## <a name="summary"></a>Resumen

Ahora ha implementado la aplicación en IIS en el equipo de desarrollo y probado no existe.

![Página principal de la prueba](deploying-to-iis/_static/image23.png)

Esto comprueba que el proceso de implementación copia el contenido de la aplicación en la ubicación correcta (excepto los archivos que va a implementar) y también configurar ese Web Deploy a IIS correctamente durante la implementación. En el siguiente tutorial, va a ejecutar una prueba más que busca una tarea de implementación que aún no se han realizado: establecer permisos de carpeta en el *Elm ah* carpeta.

## <a name="more-information"></a>Más información

Para obtener información sobre la ejecución de IIS o IIS Express en Visual Studio, consulte los siguientes recursos:

- [Introducción a IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) en el sitio de IIS.net.
- [Introducción a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Scott Guthrie.
- [Web de servidores en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio de ASP.NET.

Para obtener información acerca de los problemas que puede surgir cuando se ejecuta la aplicación en el nivel de confianza medio, consulte [de hospedaje de aplicaciones de ASP.NET en el nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los cuatro chicos del sitio Rolla.

> [!div class="step-by-step"]
> [Anterior](project-properties.md)
> [Siguiente](setting-folder-permissions.md)
