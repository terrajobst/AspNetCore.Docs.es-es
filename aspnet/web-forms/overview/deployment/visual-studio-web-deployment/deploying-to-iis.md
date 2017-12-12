---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: "Implementación de Web ASP.NET con Visual Studio: implementación de prueba | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 97910940f9de26ca71b111b945581d2de6650b02
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implementación de Web ASP.NET con Visual Studio: implementación de prueba
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una aplicación web ASP.NET a IIS en el equipo local.

Al desarrollar una aplicación, por lo general probar mediante la ejecución en Visual Studio. De forma predeterminada, los proyectos de aplicación web en Visual Studio 2012 usan IIS Express como el servidor de desarrollo de web. IIS Express se comporta más como completa de IIS que Visual Studio Development Server (también conocido como Cassini), que usa Visual Studio 2010 de forma predeterminada. Pero ningún servidor de desarrollo de web funciona exactamente igual que IIS. Como resultado, es posible que una aplicación se ejecutará correctamente cuando se prueba en Visual Studio, pero producirá un error cuando se implementa en IIS.

Puede probar la aplicación de forma más confiable de las siguientes maneras:

1. Implementar la aplicación en IIS en el equipo de desarrollo utilizando el mismo proceso que va a utilizar más adelante para implementar en el entorno de producción. Puede configurar Visual Studio para que utilice IIS cuando se ejecuta un proyecto web, pero eso no probaría su proceso de implementación. Este método valida su proceso de implementación además de validar que la aplicación se ejecutará correctamente en IIS.
2. Implementar la aplicación en un entorno de prueba que es prácticamente idéntico al entorno de producción. Puesto que el entorno de producción para estos tutoriales es aplicaciones Web en el servicio de aplicaciones de Azure, el entorno de prueba ideal es una aplicación web adicional creada en el servicio de aplicación de Azure. Usaría esta segunda aplicación web sólo para las pruebas, pero se establecería de la misma manera que la aplicación web de producción.

Opción 2 es la manera más confiable de probar y, si lo hace, no tiene necesariamente a la opción 1. Sin embargo, si va a implementar en una opción de proveedor de hospedaje terceros 2 quizás no sea factible o podría ser costosa, por lo que esta serie de tutoriales muestra ambos métodos. Se proporciona orientación para la opción 2 en el [implementarla en el entorno de producción](deploying-to-production.md) tutorial.

Para obtener más información sobre el uso de servidores web en Visual Studio, vea [servidores Web en Visual Studio para proyectos Web ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx).

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="install-iis"></a>Instalar IIS

Para implementar en IIS en el equipo de desarrollo, debe tener IIS y Web Deploy instalado. Web Deploy se instala de forma predeterminada con Visual Studio, pero IIS no está incluido en la configuración predeterminada de Windows 8 o Windows 7. Si ya ha instalado IIS y el grupo de aplicaciones predeterminado ya está establecido en .NET 4, vaya a [la siguiente sección](#sqlexpress).

1. Mediante el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx) es la mejor manera de instalar IIS y Web Deploy, porque el instalador de plataforma Web instala una configuración recomendada de IIS e instala automáticamente los requisitos previos para IIS y Web Implementar si es necesario.

    Para ejecutar el instalador de plataforma Web para instalar IIS y Web Deploy, utilice el siguiente vínculo. Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, el instalador de plataforma Web instala solo lo que faltan.

    - [Instalar IIS y Web Deploy mediante WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    Verá mensajes que indican que se instalará IIS 7. Lo vínculos funciona para IIS 8 en Windows 8, pero para Windows 8 Asegúrese de que ASP.NET 4.5 se instala mediante la realización de los pasos siguientes:

    1. Abra **el Panel de Control**, **programas y características**, **o desactivar las características de Windows Active**.
    2. Expanda **servicios de Internet Information Server**, **servicios World Wide Web**, y **características de desarrollo de aplicaciones**.
    3. Asegúrese de que **ASP.NET 4.5** está seleccionada.

        ![Seleccionar ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Después de instalar IIS, ejecute **el Administrador de IIS** para asegurarse de que la versión 4 de .NET Framework se asigna al grupo de aplicaciones predeterminado.

1. Presione WINDOWS + R para abrir el **ejecutar** cuadro de diálogo.

    (O en Windows 8, escriba "run" el **iniciar** página, o bien en Windows 7 seleccione **ejecutar** desde el **iniciar** menú. Si **ejecutar** no se encuentra en la **iniciar** menú, haga clic en la barra de tareas, haga clic en **propiedades**, seleccione la **menú Inicio** , haga clic en **Personalizar**y seleccione **ejecutar comando**.)
2. Escriba "inetmgr" y, a continuación, haga clic en **Aceptar**.
3. En el **conexiones** panel, expanda el nodo del servidor y seleccione **grupos de aplicaciones**. En el **grupos de aplicaciones** panel, si **DefaultAppPool** está asignado a .NET framework versión 4, como se muestra en la siguiente ilustración, vaya a la sección siguiente.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Si ve solo dos grupos de aplicaciones y ambos se establecen en la versión 2.0 de .NET Framework, tendrá que instalar 4 de ASP.NET en IIS.

    Para Windows 8, vea las instrucciones que aparecen en la versión anterior sección para asegurarse de que ASP.NET 4.5 está instalado, o [este artículo de KB](https://support.microsoft.com/kb/2736284). Para Windows 7, abra una ventana de símbolo del sistema haciendo clic en **símbolo** en las ventanas de **iniciar** menú y seleccionando **ejecutar como administrador**. A continuación, ejecute [aspnet\_regiis.exe](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx) para instalar ASP.NET 4 en IIS, utilizando los comandos siguientes. (En los sistemas de 32 bits, reemplace "Framework64" con "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Este comando crea nuevos grupos de aplicaciones para .NET Framework 4, pero aun así se establecerá el grupo de aplicaciones predeterminado a 2.0. Implementará una aplicación que tiene como destino .NET 4 para ese grupo de aplicaciones, por lo que debe cambiar el grupo de aplicaciones en .NET 4.
5. Si lo ha cerrado **el Administrador de IIS**, vuelva a ejecutarlo, expanda el nodo del servidor y haga clic en **grupos de aplicaciones** para mostrar la **grupos de aplicaciones** nuevo panel.
6. En el **grupos de aplicaciones** panel, haga clic en **DefaultAppPool**y, a continuación, en la **acciones** panel, haga clic **configuración básica**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. En el **Modificar grupo de aplicaciones** cuadro de diálogo, cambie **versión de .NET Framework** a **.NET Framework v4.0.30319** y haga clic en **Aceptar**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS ya está listo para publicar una aplicación web a él, pero antes de que puede hacer tendrá que crear las bases de datos que va a utilizar en el entorno de prueba.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instalar SQL Server Express

LocalDB no está diseñado para trabajar en IIS, por lo que para el entorno de prueba debe tener instalado SQL Server Express. Si usa Visual Studio 2010 SQL Server Express ya está instalado de forma predeterminada. Si usas Visual Studio 2012, tendrá que instalarlo.

Para instalar SQL Server Express, instálelo desde [centro de descarga: Microsoft SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=29062) haciendo clic en [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) o [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Si elige uno incorrecto para el sistema no se podrá instalar y puede probar con los otros.

En la primera página del centro de instalación de SQL Server, haga clic en **instalación independiente del nuevo servidor SQL Server o agregar características a una instalación existente**y siga las instrucciones, acepte las opciones predeterminadas. En el Asistente para la instalación, acepte la configuración predeterminada. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server 2012 desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/en-us/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Crear bases de datos de SQL Server Express para el entorno de prueba

La aplicación de la Universidad de Contoso tiene dos bases de datos: la base de datos de pertenencia y la base de datos de aplicación. Puede implementar estas bases de datos en dos bases de datos o en una sola base de datos. Desea combinarlas con el fin de facilitar las combinaciones de base de datos entre la base de datos de aplicación y la base de datos de pertenencia. Si va a implementar en un proveedor de hospedaje de terceros, el plan de hospedaje también puede proporcionar una razón para combinarlos. Por ejemplo, el proveedor de hospedaje puede cobrar otras cuestiones para varias bases de datos o incluso podría no permitir a más de una base de datos.

En este tutorial, implementará dos bases de datos en el entorno de prueba y una base de datos en los entornos de ensayo y producción.

Desde el **vista** menú en, seleccione Visual Studio **Explorador de servidores** (**el Explorador de base de datos** en Visual Web Developer) y, a continuación, haga clic en **las conexiones de datos**  y seleccione **crear nueva base de datos de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

En el **crear nueva base de datos de SQL Server** diálogo cuadro, escriba ". \SQLExpress" en la **nombre del servidor** cuadro y "aspnet-ContosoUniversity" en la **nuevo nombre de base de datos** cuadro, a continuación, Haga clic en **Aceptar**.

![Crear aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Siga el mismo procedimiento para crear una nueva base de datos de la escuela de SQL Server Express denominada "ContosoUniversity".

**Explorador de servidores** muestra ahora las dos bases de datos.

![Nuevas bases de datos en el Explorador de servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Crear una secuencia de comandos de concesión para las bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación obtiene acceso a la base de datos utilizando las credenciales del grupo de aplicaciones predeterminado. Sin embargo, de forma predeterminada, la identidad del grupo de aplicación no tiene permiso para abrir las bases de datos. Por lo que tendrá que ejecutar un script para conceder ese permiso. En esta sección creará el script que se va a ejecutar más adelante si desea asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

Haga clic en la solución (no de uno de los proyectos) y haga clic en **Agregar nuevo elemento**y, a continuación, cree un nuevo **archivo SQL** denominado *Grant.sql*. Copie los siguientes comandos SQL en el archivo y, a continuación, guarde y cierre el archivo:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Esta secuencia de comandos está diseñado para trabajar con SQL Server Express 2012 y con la configuración de IIS en Windows 8 o Windows 7, tal y como se especifican en este tutorial. Si usa una versión diferente de SQL Server o de Windows, o si ha configurado IIS en el equipo diferente, los cambios en esta secuencia de comandos podrían ser necesarios. Para obtener más información acerca de los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota de seguridad** esta secuencia de comandos proporciona db\_permisos de propietario para el usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, puede especificar un usuario que tiene el esquema de base de datos completa actualizar los permisos para la implementación, y especificar para el tiempo de ejecución un usuario diferente que tenga permisos sólo para leer y escribir datos. Para obtener más información, consulte [revisar los cambios de Web.config automática para migraciones de Code First](#reviewingmigrations) más adelante en este tutorial.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Ejecute la secuencia de comandos de concesión en la base de datos de aplicación

Puede configurar el perfil de publicación para ejecutar el script de concesión en la base de datos de pertenencia durante la implementación porque esa implementación de base de datos utiliza el proveedor dbDacFx. No se puede ejecutar scripts durante la implementación de migraciones de Code First, que es cómo va a implementar la base de datos de aplicación. Por tanto, tendrá que ejecutar manualmente la secuencia de comandos antes de la implementación de la base de datos de aplicación.

1. En Visual Studio, abra el *Grant.sql* archivo que creó anteriormente.
2. Haga clic en **conectar**. 

    ![Botón Conectar](deploying-to-iis/_static/image11.png)
3. En el **conectar al servidor** diálogo cuadro, escriba *. \SQLExpress* como el **nombre del servidor**y, a continuación, haga clic en **conectar**.
4. En la lista desplegable de base de datos seleccione **ContosoUniversity**y, a continuación, haga clic en **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

La identidad del grupo de aplicaciones predeterminado ahora tiene los permisos necesarios en la base de datos de aplicación para migraciones de Code First crear las tablas de base de datos cuando se ejecuta la aplicación.

## <a name="publish-to-iis"></a>Publicar en IIS

Hay varias maneras que puede implementar en IIS con Visual Studio y Web Deploy:

- Usar Visual Studio publicar un solo clic.
- Publicar desde la línea de comandos.
- Crear un *paquete de implementación* e instalarlo usando la UI del Administrador de IIS. El paquete de implementación está formada por un *.zip* archivo que contiene todos los archivos y los metadatos necesarios para instalar un sitio en IIS.
- Crear un paquete de implementación e instalarlo mediante la línea de comandos.

El proceso que llevamos a cabo en los tutoriales anteriores para configurar Visual Studio para automatizar tareas de implementación se aplica a todos estos métodos. En estos tutoriales usará las dos primeras de estos métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [implementar una aplicación web, cree e instale un paquete de implementación web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) en el mapa de contenido de implementación Web para Visual Studio y ASP.NET.

Antes de publicar, asegúrese de que está ejecutando Visual Studio en modo de administrador. Si no ve **(Administrador)** en la barra de título, cierre Visual Studio. En el sistema operativo Windows 8 **iniciar** página o Windows 7 **iniciar** menú, haga clic en el icono de la versión de Visual Studio que se use y seleccione **ejecutar como administrador**. Modo de administrador es necesario para publicar solo cuando se publican en IIS en el equipo local.

### <a name="create-the-publish-profile"></a>Crear el perfil de publicación

1. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity (no en el proyecto ContosoUniversity.DAL) y seleccione **publicar**.

    El **Publicar Web** aparece el Asistente para.

    ![Publicar la pestaña de perfil de Asistente de Web](deploying-to-iis/_static/image13.png)
2. En la lista desplegable, seleccione  **&lt;nuevo... &gt;**. (Con la actualización más reciente de Visual Studio instalada, no hay ninguna lista desplegable, y el botón para hacer clic en para crear un nuevo perfil desde el principio es **personalizado**.)
3. En el **nuevo perfil** cuadro de diálogo, escriba "Test" y, a continuación, haga clic en **Aceptar**.

    El asistente avanza automáticamente a la **conexión** ficha.
4. En el **dirección URL del servicio** cuadro, escriba *localhost*.
5. En el **sitio o aplicación** cuadro, escriba *sitio Web predeterminado/ContosoUniversity*
6. En el **dirección URL de destino** cuadro, escriba`http://localhost/ContosoUniversity`

    El **dirección URL de destino** configuración no es necesaria. Cuando Visual Studio finaliza la implementación de la aplicación, automáticamente se abre el explorador predeterminado para esta dirección URL. Si no desea que el explorador para abrir automáticamente después de la implementación, deje este cuadro en blanco.
7. Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

    Una marca de verificación verde comprueba que la conexión es correcta.

    ![Publicar la ficha de conexión del Asistente de Web](deploying-to-iis/_static/image14.png)
8. Haga clic en **siguiente** para avanzar a la **configuración** ficha.
9. El **configuración** cuadro desplegable especifica la configuración de compilación para implementar. Deje el valor predeterminado de la versión. No puede implementar compilaciones de depuración en este tutorial.
10. Expanda **File Publish Options**y, a continuación, seleccione **excluir archivos de la aplicación\_carpeta de datos**.

    En el entorno de prueba de la aplicación tendrá acceso a las bases de datos que creó en la instancia local de SQL Server Express, no el archivo .mdf archivos en el *aplicación\_datos* carpeta.
11. Deje el **Precompile durante la publicación** y **quitar archivos adicionales en destino** casillas desactivadas.

    ![Opciones de publicación del archivo en la ficha Configuración](deploying-to-iis/_static/image15.png)

    Precompilar es una opción que es útil principalmente para sitios muy grandes; puede reducir el tiempo de inicio de la página de la primera vez que se solicita una página una vez publicado el sitio.

    No es necesario quitar archivos adicionales, ya que se trata de la primera implementación y no habrá ningún archivo en la carpeta de destino aún.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Si selecciona **quitar archivos adicionales** para una implementación subsiguientes en el mismo sitio, asegúrese de usar la característica de vista previa para que se vea de antemano qué archivos se eliminarán antes de implementar. El comportamiento esperado es que Web Deploy eliminará archivos en el servidor de destino que ha eliminado en el proyecto. Sin embargo, se compara la estructura de carpetas completo en las carpetas de origen y destino, y en algunos escenarios de Web Deploy es posible que elimine archivos que no desea eliminar.
    > 
    > Por ejemplo, si tiene una aplicación web en una subcarpeta en el servidor cuando se implementa un proyecto en la carpeta raíz, se eliminará la subcarpeta. Puede que tenga un proyecto para el sitio principal en contoso.com y otro proyecto para un blog en contoso.com/blog. La aplicación de blog está en una subcarpeta. Si selecciona para quitar archivos adicionales en el destino al implementar el sitio principal, se eliminarán la aplicación de blog.
    > 
    > Para obtener otro ejemplo, la aplicación\_carpeta de datos puede obtener eliminada inesperadamente. Ciertas bases de datos como SQL Server Compact almacenan archivos de base de datos en la aplicación\_carpeta de datos. Tras la implementación inicial que no desea mantener copiar los archivos de base de datos en las implementaciones posteriores para seleccionar Excluir aplicación\_datos en la pestaña Empaquetar/Publicar Web. Después de hacerlo, si tienes que quitar archivos adicionales en destino seleccionado, los archivos de base de datos y la aplicación\_carpeta de datos propiamente dicha se eliminará la próxima vez que publique.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar la implementación de la base de datos de pertenencia

Los pasos siguientes se aplican a la **DefaultConnection** en la base de datos la **bases de datos** sección del cuadro de diálogo.

1. En el **cadena de conexión remota** cuadro, escriba la siguiente cadena de conexión que apunta a la nueva base de pertenencia de SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **utilizar esta cadena de conexión en tiempo de ejecución** está seleccionada.

    También puede obtener la cadena de conexión de **Explorador de servidores**. En **Explorador de servidores**, expanda **las conexiones de datos** y seleccione la  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** base de datos, a continuación, desde el **propiedades** copia ventana el **cadena de conexión** valor. Cadena de conexión que tenga una configuración adicional que puede eliminar: `Pooling=False`.
2. Seleccione **Actualizar base de datos**.

    Esto hará que el esquema de base de datos que se creen en la base de datos de destino durante la implementación. En los pasos siguientes especifican las secuencias de comandos adicionales que necesite ejecutar: uno para conceder acceso de base de datos para el grupo de aplicaciones predeterminado y otro para la implementación de datos.
3. Haga clic en **configurar actualizaciones de base de datos**.
4. En el **configurar actualizaciones de base de datos** cuadro de diálogo, haga clic en **Agregar secuencia de comandos SQL** y, a continuación, navegue hasta la *Grant.sql* secuencia de comandos que guardó anteriormente en la carpeta de soluciones.
5. Repita el proceso para agregar el *dev.sql de datos de aspnet* secuencia de comandos.

    ![Configurar actualizaciones de base de datos de base de datos de pertenencia](deploying-to-iis/_static/image16.png)
6. Haga clic en **Cerrar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar la implementación de la base de datos de aplicación

Cuando Visual Studio detecta un Entity Framework `DbContext` (clase), crea una entrada en el **bases de datos** sección que tiene un **ejecutar Code First Migrations** casilla de verificación en lugar de un  **Actualizar base de datos** casilla de verificación. En este tutorial usará esa casilla de verificación para especificar la implementación de migraciones de Code First.

En algunos escenarios, puede que esté usando un `DbContext` base de datos pero desea usar el proveedor dbDacFx en lugar de las migraciones para implementar la base de datos. En ese caso, consulte [cómo se puede implementar una base de datos Code First sin migraciones?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) en las P+F de implementación Web de ASP.NET en MSDN.

Los pasos siguientes se aplican a la **SchoolContext** en la base de datos la **bases de datos** sección del cuadro de diálogo.

1. En el **cadena de conexión remota** cuadro, escriba la siguiente cadena de conexión que apunta a la nueva base de aplicación de SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **utilizar esta cadena de conexión en tiempo de ejecución** está seleccionada.

    También puede obtener la cadena de conexión de base de datos de aplicación de **Explorador de servidores** del mismo modo que obtuvo la pertenencia a la cadena de conexión de base de datos.
2. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**.

    Esta opción hace que el proceso de implementación configurar el archivo Web.config implementado para especificar el `MigrateDatabaseToLatestVersion` inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación tiene acceso a la base de datos por primera vez después de la implementación.

### <a name="configure-publish-profile-transforms"></a>Configurar transformaciones de perfil de publicación

1. Haga clic en **cerrar**y, a continuación, haga clic en **Sí** cuando se le pregunte si desea guardar los cambios.
2. En **el Explorador de soluciones**, expanda **propiedades**, expanda **PublishProfiles**.
3. Clic Rright *Test.pubxml,* y, a continuación, haga clic en **Agregar configuración transformar**.

    ![Agregar menús de transformación de configuración](deploying-to-iis/_static/image17.png)

    Visual Studio crea el *Web.Test.config* archivo de transformación y lo abre.
4. En el *Web.Test.config* transformar el archivo, inserte el código siguiente inmediatamente después de la apertura etiqueta de configuración.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Cuando se utiliza la prueba de perfil de publicación, esta transformación establece el indicador de entorno a "Test". En el sitio implementado verá "(Test)" después del encabezado H1 "Universidad de Contoso".
5. Guarde y cierre el archivo.
6. Haga clic en el *Web.Test.config* de archivo y haga clic en **transformar de vista previa** para asegurarse de que la transformación se codificó produce cambios esperados.

    El **vista previa de Web.config** ventana muestra el resultado de aplicar el *Web.Release.config* transforma y *Web.Test.config* transforma.

### <a name="preview-the-deployment-updates"></a>Obtener una vista previa de las actualizaciones de implementación

1. Abra la **Publicar Web** Asistente para nuevo (haga clic en el proyecto ContosoUniversity y haga clic en **publicar**).
2. En el **vista previa** ficha, asegúrese de que el **prueba** perfil es aún seleccionado y, a continuación, haga clic en **iniciar Preview** para ver una lista de los archivos que se va a copiar.

    ![Botón de vista previa](deploying-to-iis/_static/image18.png)

    ![Vista previa de publicación](deploying-to-iis/_static/image19.png)

    También puede hacer clic en el **base de datos de vista previa** vínculo para ver las secuencias de comandos que se ejecutarán en la base de datos de pertenencia. (No hay secuencias de comandos se ejecutan para la implementación de migraciones de Code First, por lo que no hay nada para obtener una vista previa de la base de datos de aplicación.)
3. Haga clic en **Publicar**.

    Si Visual Studio no está en modo de administrador, podría obtener un mensaje de error que indica un error de permisos. En ese caso, cierre Visual Studio, abra en modo de administrador y, intente volver a publicar.

    Si Visual Studio está en modo de administrador, el **salida** informes de la ventana correcta generación y publicación.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Si escribió la dirección URL en el **dirección URL de destino** cuadro en el perfil de publicación **conexión** ficha, el explorador se abre automáticamente en la página principal de universidad de Contoso que se ejecutan en IIS en el equipo local.

## <a name="test-in-the-test-environment"></a>Probar en el entorno de prueba

Tenga en cuenta que el indicador de entorno mostrará "(Test)" en lugar de "(desarrollo)", que muestra que la *Web.config* transformación para el indicador de entorno fue correcta.

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos de instructor. Cuando se selecciona esta página, puede tardar unos minutos en cargarse porque crea la base de datos de Code First y, a continuación, se ejecuta el `Seed` método. (No hacen cuando estaban en la página principal porque la aplicación no intenta obtener acceso a la base de datos todavía).

Haga clic en el **estudiantes** pestaña para comprobar que la base de datos implementada no tiene ningún alumno.

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el nuevo alumno en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos .

Desde el **cursos** menú, seleccione **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **inicio de sesión** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "devpwd"). El **actualización créditos** aparece la página, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Compruebe que un *Elmah* carpeta existe en el *c:\inetpub\wwwroot\ContosoUniversity* carpeta con el archivo de marcador de posición en él.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar los cambios de Web.config automática para migraciones de Code First

Abra la *Web.config* archivo en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y verá que el proceso de implementación configurado migraciones de Code First para automáticamente actualizar la base de datos a la versión más reciente.

![](deploying-to-iis/_static/image21.png)

El proceso de implementación también crea una nueva cadena de conexión para migraciones de Code First utilizar exclusivamente para actualizar el esquema de base de datos:

![Cadena de conexión de Database_Publish](deploying-to-iis/_static/image22.png)

Esta cadena de conexión adicionales le permite especificar una cuenta de usuario para las actualizaciones del esquema de base de datos y una cuenta de usuario diferente para el acceso de datos de aplicación. Por ejemplo, puede asignar la **db\_propietario** rol para migraciones de Code First y **db\_datareader** y **db\_datawriter**roles a la aplicación. Se trata de un patrón común de defensa en profundidad que evita que un código potencialmente malintencionado en la aplicación de cambiar el esquema de base de datos. (Por ejemplo, esto podría suceder en un ataque de inyección de SQL). Este patrón no se utiliza por estos tutoriales. Si desea implementar este patrón en el escenario, puede hacerlo mediante la realización de los pasos siguientes:

1. En el **configuración** pestaña de la **Publicar Web** asistente, escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquema de base de datos completa y desactive el **usar esta cadena de conexión en tiempo de ejecución** casilla de verificación. En el archivo Web.config implementado, se convierte en el `DatabasePublish` cadena de conexión.
2. Crear una transformación del archivo Web.config para la cadena de conexión que desea que la aplicación para utilizar en tiempo de ejecución.

## <a name="summary"></a>Resumen

Ahora ha implementado la aplicación en IIS en el equipo de desarrollo y probado no existe.

![Página principal de prueba](deploying-to-iis/_static/image23.png)

Esto comprueba que el proceso de implementación copia el contenido de la aplicación en la ubicación correcta (excepto los archivos que va a implementar), y también configurar ese Web Deploy a IIS correctamente durante la implementación. En el siguiente tutorial, va a ejecutar una prueba más que busca una tarea de implementación que no se ha realizado aún: establecer permisos para carpetas en el *Elmah* carpeta.

## <a name="more-information"></a>Más información

Para obtener información sobre la ejecución de IIS o IIS Express en Visual Studio, vea los siguientes recursos:

- [Introducción a IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) en el sitio de IIS.net.
- [Introducción a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Guthrie.
- [Servidores en Visual Studio Web para los proyectos Web ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio ASP.NET.

Para obtener información acerca de los problemas que puede surgir cuando la aplicación se ejecuta con confianza media, consulte [hospedaje de aplicaciones de ASP.NET en el nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los 4 encargados de Rolla sitio.

>[!div class="step-by-step"]
[Anterior](project-properties.md)
[Siguiente](setting-folder-permissions.md)
