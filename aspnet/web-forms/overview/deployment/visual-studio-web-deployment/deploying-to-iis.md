---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Implementación Web de ASP.NET con Visual Studio: implementación de prueba | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 6bfd1399c9e627839005fa27086c90bc0cc049e5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826373"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implementación Web de ASP.NET con Visual Studio: implementación de prueba
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una aplicación web ASP.NET en IIS en el equipo local.

Al desarrollar una aplicación, por lo general probar ejecutándolo en Visual Studio. De forma predeterminada, los proyectos de aplicación web en Visual Studio 2012 usan IIS Express como el servidor web de desarrollo. IIS Express se comporta más como completa de IIS que Visual Studio Development Server (también conocido como Cassini), que usa Visual Studio 2010 de forma predeterminada. Pero ningún servidor de desarrollo web funciona exactamente igual que IIS. Como resultado, es posible que una aplicación se ejecute correctamente al probarlo en Visual Studio, pero producirá un error cuando se implementa en IIS.

Puede probar la aplicación de forma más confiable de las siguientes maneras:

1. Implementar la aplicación en IIS en el equipo de desarrollo utilizando el mismo proceso que usará más adelante para implementarlo en su entorno de producción. Puede configurar Visual Studio para que use IIS cuando se ejecuta un proyecto web, pero eso no probaría su proceso de implementación. Este método valida su proceso de implementación además de validar que la aplicación se ejecutará correctamente en IIS.
2. Implementar la aplicación en un entorno de prueba que es casi idéntico al entorno de producción. Puesto que el entorno de producción para estos tutoriales es Web Apps en Azure App Service, el entorno de prueba ideal es una aplicación web adicional creada en Azure App Service. Usaría esta segunda aplicación web solo para las pruebas, pero podría configurarse del mismo modo que la aplicación web de producción.

Opción 2 es la forma más confiable para probar y, si lo hace, no tiene necesariamente a la opción 1. Sin embargo, si va a implementar en una opción de proveedor de hospedaje de terceros 2 quizás no sea factible o podría ser costosa, por lo que esta serie de tutoriales muestra ambos métodos. Se proporciona orientación para la opción 2 en el [implementarla en el entorno de producción](deploying-to-production.md) tutorial.

Para obtener más información sobre el uso de servidores web en Visual Studio, consulte [servidores Web en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="install-iis"></a>Instalar IIS

Para implementar en IIS en el equipo de desarrollo, debe tener IIS y Web Deploy instalado. Web Deploy se instala con Visual Studio de forma predeterminada, pero IIS no está incluido en la configuración predeterminada de Windows 8 o Windows 7. Si ya ha instalado IIS y el grupo de aplicaciones predeterminada ya está establecido en .NET 4, vaya a [la siguiente sección](#sqlexpress).

1. Mediante el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx) es la mejor manera de instalar IIS y Web Deploy, porque el instalador de plataforma Web instala una configuración recomendada de IIS e instala automáticamente los requisitos previos para IIS y Web Implementar si es necesario.

    Para ejecutar el instalador de plataforma Web para instalar IIS y Web Deploy, use el siguiente vínculo. Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, el instalador de plataforma Web instala solo lo que falta.

   - [Instalar IIS y Web Deploy mediante WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Verá mensajes que indican que se instalará IIS 7. Lo vínculos funciona para IIS 8 en Windows 8, pero para Windows 8 Asegúrese de que ASP.NET 4.5 está instalado, siga estos pasos:

   - Abra **Panel de Control**, **programas y características**, **o desactivar las características de Windows Active**.
   - Expanda **Internet Information Services**, **servicios World Wide Web**, y **Application Development Features**.
   - Asegúrese de que **ASP.NET 4.5** está seleccionada.

      ![Seleccione ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Después de instalar IIS, ejecute **IIS Manager** para asegurarse de que la versión 4 de .NET Framework se asigna al grupo de aplicaciones de forma predeterminada.

1. Presione WINDOWS + R para abrir el **ejecutar** cuadro de diálogo.

    (O en Windows 8, escriba "run" el **iniciar** página, o bien en Windows 7 seleccione **ejecutar** desde el **iniciar** menú. Si **ejecutar** no se encuentra en la **iniciar** menú, haga clic en la barra de tareas, haga clic en **propiedades**, seleccione el **menú Inicio** pestaña, haga clic en **Personalizar**y seleccione **ejecutar comando**.)
2. Escriba "inetmgr" y, a continuación, haga clic en **Aceptar**.
3. En el **conexiones** panel, expanda el nodo del servidor y seleccione **grupos de aplicaciones**. En el **grupos de aplicaciones** panel, si **DefaultAppPool** está asignado a la versión de .NET framework 4 como se muestra en la ilustración siguiente, vaya a la sección siguiente.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Si ve solo dos grupos de aplicaciones y ambos se establecen en .NET Framework 2.0, deberá instalar el 4 de ASP.NET en IIS.

    Para Windows 8, consulte las instrucciones de la anterior sección para asegurarse de que ASP.NET 4.5 está instalado, o vea [en este artículo KB](https://support.microsoft.com/kb/2736284). Para Windows 7, abra una ventana del símbolo del sistema haciendo clic **símbolo** en el Windows **iniciar** menú y seleccionando **ejecutar como administrador**. A continuación, ejecute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar ASP.NET 4 en IIS, mediante los siguientes comandos. (En los sistemas de 32 bits, reemplace "Framework64" con "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Este comando crea nuevos grupos de aplicaciones para .NET Framework 4, pero aún así se establecerá el grupo de aplicaciones predeterminado a 2.0. Implementará una aplicación que tiene como destino .NET 4 para ese grupo de aplicaciones, por lo que debe cambiar el grupo de aplicaciones en .NET 4.
5. Si ha cerrado **IIS Manager**, ejecútelo de nuevo, expanda el nodo del servidor y haga clic en **grupos de aplicaciones** para mostrar el **grupos de aplicaciones** nuevo panel.
6. En el **grupos de aplicaciones** panel, haga clic en **DefaultAppPool**y, a continuación, en el **acciones** panel, haga clic **configuración básica**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. En el **Modificar grupo de aplicaciones** cuadro de diálogo, cambie **versión de .NET Framework** a **.NET Framework v4.0.30319** y haga clic en **Aceptar**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS ya está listo para publicar una aplicación web en él, pero antes de hacer se tiene que crear las bases de datos que va a utilizar en el entorno de prueba.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instale SQL Server Express

LocalDB no está diseñado para trabajar en IIS, por lo que para el entorno de prueba debe tener instalado SQL Server Express. Si usa Visual Studio 2010 SQL Server Express ya está instalado de forma predeterminada. Si utiliza Visual Studio 2012, deberá instalarlo.

Para instalar SQL Server Express, instálelo desde [centro de descarga: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) haciendo [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) o [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Si elige uno incorrecto para el sistema no se podrá instalar y el otro se puede probar.

En la primera página del centro de instalación de SQL Server, haga clic en **instalación independiente del nuevo servidor SQL Server o agregar características a una instalación existente**y siga las instrucciones y acepte las opciones predeterminadas. En el Asistente para instalación, acepte la configuración predeterminada. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server 2012 desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Crear bases de datos de SQL Server Express del entorno de prueba

La aplicación Contoso University tiene dos bases de datos: la base de datos de pertenencia y la base de datos de aplicación. Puede implementar estas bases de datos para dos bases de datos independientes o para una sola base de datos. Puede combinarlos con el fin de facilitar las combinaciones de base de datos entre la base de datos de aplicación y la base de datos de pertenencia. Si va a implementar en un proveedor de hospedaje de terceros, el plan de hospedaje también podría proporcionar un motivo para combinarlos. Por ejemplo, el proveedor de hospedaje puede cobrar más para varias bases de datos o podría incluso no permitir más de una base de datos.

En este tutorial, implementará dos bases de datos en el entorno de prueba y una base de datos en los entornos de ensayo y producción.

Desde el **vista** menú en Visual Studio, seleccione **Explorador de servidores** (**Database Explorer** en Visual Web Developer) y, a continuación, haga clic en **las conexiones de datos**  y seleccione **crear nueva base de datos de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

En el **crear nueva base de datos de SQL Server** diálogo cuadro, escriba ". \SQLExpress" en el **nombre del servidor** cuadro y "aspnet-ContosoUniversity" en el **nuevo nombre de base de datos** cuadro, a continuación, Haga clic en **Aceptar**.

![Crear aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Siga el mismo procedimiento para crear una nueva base de datos de la escuela de SQL Server Express denominada "ContosoUniversity".

**Explorador de servidores** ahora muestra las dos bases de datos.

![Nuevas bases de datos en el Explorador de servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Crear un script de concesión para las bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación tiene acceso a la base de datos mediante las credenciales del grupo de aplicaciones predeterminado. Sin embargo, de forma predeterminada, la identidad del grupo de aplicación no tiene permiso para abrir las bases de datos. Por lo tanto, tendrá que ejecutar un script para conceder ese permiso. En esta sección creará la secuencia de comandos que se va a ejecutar más adelante para asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

Haga clic en la solución (no de uno de los proyectos) y haga clic en **Agregar nuevo elemento**y, a continuación, cree un nuevo **archivo SQL** denominado *Grant.sql*. Copie los siguientes comandos SQL en el archivo y, a continuación, guarde y cierre el archivo:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Este script está diseñado para trabajar con SQL Server Express 2012 y con la configuración de IIS en Windows 8 o Windows 7, como se especifican en este tutorial. Si usa una versión diferente de SQL Server o de Windows, o si configura IIS en un equipo diferente, los cambios realizados en esta secuencia de comandos pueden ser necesarios. Para obtener más información acerca de los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota de seguridad** esta secuencia de comandos proporciona db\_permisos de propietario para el usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, puede especificar un usuario que tiene el esquema de base de datos completa, actualizar los permisos para la implementación y especificar para el tiempo de ejecución de un usuario diferente que tenga permisos solo para leer y escribir datos. Para obtener más información, consulte [revisar los cambios de Web.config automática para migraciones de Code First](#reviewingmigrations) más adelante en este tutorial.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Ejecute el script de concesión en la base de datos de aplicación

Puede configurar el perfil de publicación para ejecutar el script de concesión en la base de datos de pertenencia durante la implementación debido a que la implementación base de datos utiliza el proveedor dbDacFx. No se puede ejecutar scripts durante la implementación de migraciones de Code First, que es cómo va a implementar la base de datos de aplicación. Por lo tanto, tendrá que ejecutar manualmente el script antes de la implementación de la base de datos de aplicación.

1. En Visual Studio, abra el *Grant.sql* archivo que creó anteriormente.
2. Haga clic en **Conectar**. 

    ![Botón Conectar](deploying-to-iis/_static/image11.png)
3. En el **conectar al servidor** diálogo cuadro, escriba *. \SQLExpress* como el **nombre del servidor**y, a continuación, haga clic en **Connect**.
4. En la lista desplegable de base de datos seleccione **ContosoUniversity**y, a continuación, haga clic en **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

La identidad del grupo de aplicaciones predeterminado ahora tiene permisos suficientes en la base de datos de aplicación para migraciones de Code First crear las tablas de base de datos cuando se ejecuta la aplicación.

## <a name="publish-to-iis"></a>Publicar en IIS

Hay varias maneras que puede implementar en IIS con Visual Studio y Web Deploy:

- Publicar el uso de Visual Studio con un solo clic.
- Publicar desde la línea de comandos.
- Crear un *paquete de implementación* e instalarlo mediante la UI del Administrador IIS. El paquete de implementación consta de un *.zip* archivo que contiene todos los archivos y los metadatos necesarios para instalar un sitio en IIS.
- Crear un paquete de implementación e instalarlo mediante la línea de comandos.

El proceso que llevamos a cabo en los tutoriales anteriores para configurar Visual Studio para automatizar las tareas de implementación se aplica a todos estos métodos. En estos tutoriales usará los dos primeros de estos métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [implementar una aplicación web, cree e instale un paquete de implementación web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) en el mapa de contenido de implementación Web para Visual Studio y ASP.NET.

Antes de la publicación, asegúrese de que se está ejecutando Visual Studio en modo de administrador. Si no ve **(Administrador)** en la barra de título, cierre Visual Studio. En Windows 8 **iniciar** página o el de Windows 7 **iniciar** menú, haga clic en el icono de la versión de Visual Studio que está usando y seleccione **ejecutar como administrador**. Modo de administrador es necesario para publicar solo cuando va a publicar en IIS en el equipo local.

### <a name="create-the-publish-profile"></a>Crear el perfil de publicación

1. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity (no el proyecto ContosoUniversity.DAL) y seleccione **publicar**.

    El **publicación Web** aparece el asistente.

    ![Ficha perfil de Asistente de Web de publicación](deploying-to-iis/_static/image13.png)
2. En la lista desplegable, seleccione  **&lt;nuevo... &gt;**. (Con la actualización más reciente de Visual Studio instalada, no hay ninguna lista desplegable y el botón de hacer clic con el fin de crear un perfil nuevo desde cero es **personalizado**.)
3. En el **nuevo perfil** cuadro de diálogo, escriba "Test" y, a continuación, haga clic en **Aceptar**.

    El asistente avanza automáticamente a la **conexión** ficha.
4. En el **dirección URL del servicio** , escriba *localhost*.
5. En el **sitio o aplicación** , escriba *Default Web Site/ContosoUniversity*
6. En el **dirección URL de destino** , escriba `http://localhost/ContosoUniversity`

    El **dirección URL de destino** configuración no es necesaria. Cuando Visual Studio termine de implementar la aplicación, se abre automáticamente el explorador predeterminado para esta dirección URL. Si no desea que el explorador para abrir automáticamente después de la implementación, deje este cuadro en blanco.
7. Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

    Una marca de verificación verde comprueba que la conexión es correcta.

    ![Publicar la ficha de conexión del Asistente de Web](deploying-to-iis/_static/image14.png)
8. Haga clic en **siguiente** para avanzar a la **configuración** ficha.
9. El **configuración** cuadro desplegable especifica la configuración de compilación para implementar. Déjelo establecido en el valor predeterminado de la versión. No se puede implementar compilaciones de depuración en este tutorial.
10. Expanda **File Publish Options**y, a continuación, seleccione **excluir archivos de la aplicación\_carpeta de datos**.

    En el entorno de prueba de la aplicación tendrá acceso a las bases de datos que creó en la instancia local de SQL Server Express, no el archivo .mdf de archivos en el *aplicación\_datos* carpeta.
11. Deje el **precompilar durante la publicación** y **quitar archivos adicionales en destino** casillas desactivadas.

    ![Opciones de publicación del archivo en la pestaña Configuración](deploying-to-iis/_static/image15.png)

    Precompilar es una opción que es útil principalmente para sitios muy grandes; puede reducir el tiempo de inicio de la página de la primera vez que se solicita una página después de publicar el sitio.

    No es necesario quitar archivos adicionales, ya que se trata de la primera implementación y no habrá ningún archivo en la carpeta de destino aún.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Si selecciona **quitar archivos adicionales** para una implementación subsiguientes en el mismo sitio, asegúrese de usar la característica de vista previa para que ver con antelación qué archivos se eliminarán antes de implementar. El comportamiento esperado es que Web Deploy se eliminarán los archivos del servidor de destino que ha eliminado en el proyecto. Sin embargo, se compara la estructura de carpeta completa en las carpetas de origen y destino, y en algunos escenarios de Web Deploy podría eliminar archivos que no desea eliminar.
    > 
    > Por ejemplo, si tiene una aplicación web en una subcarpeta en el servidor al implementar un proyecto en la carpeta raíz, se eliminará la subcarpeta. Podría tener un proyecto para el sitio principal en contoso.com y otro proyecto para un blog en contoso.com/blog. La aplicación de blog está en una subcarpeta. Si selecciona Quitar archivos adicionales en el destino al implementar el sitio principal, se eliminará la aplicación de blog.
    > 
    > Para obtener otro ejemplo, la aplicación\_carpeta de datos deben eliminarse inesperadamente. Ciertas bases de datos como SQL Server Compact almacenan archivos de base de datos en la aplicación\_carpeta de datos. Tras la implementación inicial que no desea mantener copia de los archivos de base de datos en las implementaciones posteriores por lo que seleccione Excluir aplicación\_datos en la pestaña Empaquetar/Publicar Web. Una vez hecho esto, si tienes que quitar archivos adicionales en el destino seleccionado, los archivos de base de datos y la aplicación\_propia carpeta de datos se eliminarán la próxima vez que publique.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar la implementación de la base de datos de pertenencia

Los pasos siguientes se aplican a la **DefaultConnection** en la base de datos la **bases de datos** sección del cuadro de diálogo.

1. En el **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunta a la nueva base de pertenencia de SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **usar esta cadena de conexión en tiempo de ejecución** está seleccionada.

    También puede obtener la cadena de conexión de **Explorador de servidores**. En **Explorador de servidores**, expanda **conexiones de datos** y seleccione el  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** base de datos, a continuación, en el **propiedades** copia ventana el **cadena de conexión** valor. Cadena de conexión que tenga una configuración adicional que puede eliminar: `Pooling=False`.
2. Seleccione **Actualizar base de datos**.

    Esto hará que el esquema de base de datos que se creará en la base de datos de destino durante la implementación. Especifica las secuencias de comandos adicionales que necesita para ejecutar en los pasos siguientes: uno para conceder acceso de la base de datos en el grupo de aplicaciones predeterminado y otro para implementar los datos.
3. Haga clic en **configurar actualizaciones de base de datos**.
4. En el **configurar actualizaciones de base de datos** cuadro de diálogo, haga clic en **Agregar secuencia de comandos SQL** y, a continuación, navegue hasta la *Grant.sql* script que guardó anteriormente en la carpeta de soluciones.
5. Repita el proceso para agregar el *aspnet-data-dev.sql* secuencia de comandos.

    ![Configurar las actualizaciones de base de datos de base de datos de pertenencia](deploying-to-iis/_static/image16.png)
6. Haga clic en **Cerrar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar la implementación de la base de datos de aplicación

Cuando Visual Studio detecta un Entity Framework `DbContext` (clase), crea una entrada en el **bases de datos** sección que tiene un **ejecutar migraciones Code First** casilla de verificación en lugar de un  **Actualizar base de datos** casilla de verificación. En este tutorial usará esa casilla de verificación para especificar la implementación de migraciones de Code First.

En algunos escenarios, puede que esté usando un `DbContext` base de datos pero desea usar el proveedor dbDacFx en lugar de las migraciones para implementar la base de datos. En ese caso, consulte [cómo se puede implementar una base de datos Code First sin migraciones?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) en las P+F de implementación Web de ASP.NET en MSDN.

Los pasos siguientes se aplican a la **SchoolContext** en la base de datos la **bases de datos** sección del cuadro de diálogo.

1. En el **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunta a la nueva base de aplicación de SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **usar esta cadena de conexión en tiempo de ejecución** está seleccionada.

    También puede obtener la cadena de conexión de base de datos de aplicación de **Explorador de servidores** cadena de conexión de base de datos de la misma manera que obtuvo la pertenencia.
2. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**.

    Esta opción hace que el proceso de implementación configurar el archivo Web.config implementado para especificar el `MigrateDatabaseToLatestVersion` inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación tiene acceso a la base de datos por primera vez después de la implementación.

### <a name="configure-publish-profile-transforms"></a>Configurar transformaciones de perfil de publicación

1. Haga clic en **cerrar**y, a continuación, haga clic en **Sí** cuando se le pregunte si desea guardar los cambios.
2. En **el Explorador de soluciones**, expanda **propiedades**, expanda **PublishProfiles**.
3. Rright clic *Test.pubxml,* y, a continuación, haga clic en **Agregar transformación Config**.

    ![Agregar menú de transformación de configuración](deploying-to-iis/_static/image17.png)

    Visual Studio crea el *Web.Test.config* archivo de transformación y lo abre.
4. En el *Web.Test.config* transformar el archivo, inserte el código siguiente inmediatamente después de abrir etiqueta de configuración.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Cuando se utiliza la prueba de perfil de publicación, esta transformación establece el indicador de entorno a "Test". En el sitio implementado, verá "(prueba)" tras el encabezado H1 "Contoso University".
5. Guarde y cierre el archivo.
6. Haga clic en el *Web.Test.config* de archivo y haga clic en **Preview transformar** para asegurarse de que la transformación ha codificado genera los cambios previstos.

    El **Web.config Preview** ventana muestra el resultado de aplicar el *Web.Release.config* transforma y *Web.Test.config* transforma.

### <a name="preview-the-deployment-updates"></a>Vista previa de la implementación de actualizaciones

1. Abra el **publicación Web** nuevo asistente (haga clic en el proyecto ContosoUniversity y haga clic en **publicar**).
2. En el **Preview** pestaña, asegúrese de que el **prueba** perfil sea aún seleccionado y, a continuación, haga clic en **iniciar vista previa** para ver una lista de los archivos que se va a copiar.

    ![Botón de vista previa](deploying-to-iis/_static/image18.png)

    ![Vista previa de publicación](deploying-to-iis/_static/image19.png)

    También puede hacer clic en el **base de datos de vista previa** vínculo para ver las secuencias de comandos que se ejecutarán en la base de datos de pertenencia. (No hay scripts se ejecutan para la implementación de migraciones de Code First, por lo que no hay nada que obtener una vista previa de la base de datos de la aplicación.)
3. Haga clic en **Publicar**.

    Si Visual Studio no está en modo de administrador, puede aparecer un mensaje de error que indica un error de permisos. En ese caso, cierre Visual Studio, abrirlo en modo de administrador y, intente volver a publicar.

    Si Visual Studio está en modo de administrador, el **salida** informes correcta de la ventana de compilación y publicación.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Si escribió la dirección URL en el **dirección URL de destino** cuadro en el perfil de publicación **conexión** ficha, el explorador se abre automáticamente en la página principal de la Universidad de Contoso que se ejecutan en IIS en el equipo local.

## <a name="test-in-the-test-environment"></a>Probar en el entorno de prueba

Tenga en cuenta que el indicador de entorno muestra "(prueba)" en lugar de "(desarrollo)", que muestra que el *Web.config* transformación para el indicador de entorno fue correcta.

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos de un instructor. Al seleccionar esta página, puede tardar unos minutos en cargarse porque Code First crea la base de datos y, a continuación, se ejecuta el `Seed` método. (No hace cuando estaban en la página principal porque la aplicación no intenta tener acceso a la base de datos todavía.)

Haga clic en el **estudiantes** pestaña para comprobar que la base de datos implementada no tiene ningún estudiante.

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el alumno nuevo en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos .

Desde el **cursos** menú, seleccione **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **en el registro** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "devpwd"). El **actualización créditos** página se muestra, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Compruebe que un *Elmah* carpeta existe en el *c:\inetpub\wwwroot\ContosoUniversity* carpeta con el archivo de marcador de posición en él.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revise los cambios de Web.config automática para migraciones de Code First

Abra el *Web.config* archivo en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y puede ver que el proceso de implementación configurado migraciones de Code First para automáticamente actualizar la base de datos a la versión más reciente.

![](deploying-to-iis/_static/image21.png)

El proceso de implementación también crea una nueva cadena de conexión para migraciones de Code First exclusivamente actualizar el esquema de base de datos:

![Cadena de conexión Database_Publish](deploying-to-iis/_static/image22.png)

Esta cadena de conexión adicionales le permite especificar una cuenta de usuario para las actualizaciones del esquema de base de datos y una cuenta de usuario diferente para el acceso a datos de aplicación. Por ejemplo, podría asignar el **db\_propietario** rol para migraciones de Code First y **db\_datareader** y **db\_datawriter**roles a la aplicación. Se trata de un patrón común de defensa en profundidad que impide que el código potencialmente malintencionado en la aplicación modifiquen el esquema de base de datos. (Por ejemplo, esto puede suceder en un ataque de inyección SQL.) No se utiliza este patrón por estos tutoriales. Si desea implementar este patrón en su escenario, puede hacerlo mediante los pasos siguientes:

1. En el **configuración** pestaña de la **publicación Web** asistente, escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquema de base de datos completa y desactive el **usar esta cadena de conexión en tiempo de ejecución** casilla de verificación. En el archivo Web.config implementado, esto se convierte en el `DatabasePublish` cadena de conexión.
2. Crear una transformación del archivo Web.config para la cadena de conexión que desea que la aplicación para usar en tiempo de ejecución.

## <a name="summary"></a>Resumen

Ha implementado la aplicación en IIS en el equipo de desarrollo y haya probado.

![Página principal de la prueba](deploying-to-iis/_static/image23.png)

Esto comprueba que el proceso de implementación copia el contenido de la aplicación en la ubicación correcta (excepto los archivos que va a implementar), y también configurar ese Web Deploy a IIS correctamente durante la implementación. En el siguiente tutorial, va a ejecutar una prueba más que busca una tarea de implementación que aún no se han realizado: establecer permisos de carpeta en el *Elmah* carpeta.

## <a name="more-information"></a>Más información

Para obtener información sobre la ejecución de IIS o IIS Express en Visual Studio, consulte los siguientes recursos:

- [Introducción a IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) en el sitio de IIS.net.
- [Introducción a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Scott Guthrie.
- [Web de servidores en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio de ASP.NET.

Para obtener información acerca de los problemas que puede surgir cuando se ejecuta la aplicación en el nivel de confianza medio, consulte [de hospedaje de aplicaciones de ASP.NET en el nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los chicos del sitio Rolla 4.

> [!div class="step-by-step"]
> [Anterior](project-properties.md)
> [Siguiente](setting-folder-permissions.md)
