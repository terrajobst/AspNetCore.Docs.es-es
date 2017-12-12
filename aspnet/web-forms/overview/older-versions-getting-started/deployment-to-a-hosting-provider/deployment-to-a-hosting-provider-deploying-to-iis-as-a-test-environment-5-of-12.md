---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación en IIS como un entorno de prueba - 5 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5538744dfaff76f28c5f17d8f5d782ef3f6c118
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación en IIS como un entorno de prueba - 5 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una aplicación web ASP.NET a IIS en el equipo local.

Al desarrollar una aplicación, por lo general probar mediante la ejecución en Visual Studio. De forma predeterminada, esto significa que está usando el servidor de desarrollo de Visual Studio (también conocido como Cassini). El servidor de desarrollo de Visual Studio facilita la prueba durante el desarrollo en Visual Studio, pero no funciona exactamente igual que IIS. Como resultado, es posible que una aplicación se ejecutará correctamente cuando se prueba en Visual Studio, pero producirá un error cuando se implementa en IIS en un entorno de hospedaje.

Puede probar la aplicación de forma más confiable de las siguientes maneras:

1. Usar IIS Express o completa de IIS en lugar del servidor de desarrollo de Visual Studio cuando se prueba en Visual Studio durante el desarrollo. Este método suele emula con más precisión cómo se ejecutará el sitio en IIS. Sin embargo, este método prueba el proceso de implementación o validar que el resultado del proceso de implementación se ejecutará correctamente.
2. Implementar la aplicación en IIS en el equipo de desarrollo utilizando el mismo proceso que va a utilizar más adelante para implementar en el entorno de producción. Este método valida su proceso de implementación además de validar que la aplicación se ejecutará correctamente en IIS.
3. Implementar la aplicación en un entorno de prueba que sea lo más cercano posible al entorno de producción. Puesto que el entorno de producción para estos tutoriales es un proveedor de hospedaje de terceros, el entorno de prueba ideal sería una segunda cuenta con el proveedor de hospedaje. Usaría esta segunda cuenta sólo para las pruebas, pero debe configurar de la misma manera que la cuenta de producción.

Este tutorial muestra los pasos para la opción 2. Se proporciona orientación para la opción 3 al final de la [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, y al final de este tutorial se incluyen vínculos a recursos para la opción 1.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configuración de la aplicación en ejecución en el nivel de confianza medio

Antes de instalar IIS e implementarlos en él, va a cambiar una configuración del archivo Web.config para que el sitio de ejecución más como lo hará en un entorno de hospedaje compartido típico.

Normalmente, los proveedores de hospedaje ejecutan su sitio web en *confianza Media*, lo que significa que hay algunas cosas que no se permite hacer. Por ejemplo, el código de aplicación no se puede tener acceso al registro de Windows y no se lectura o escritura archivos que se encuentran fuera de la jerarquía de carpetas de la aplicación. De forma predeterminada, la aplicación se ejecuta *alto nivel de confianza* en el equipo local, lo que significa que la aplicación pueda hacer cosas que produciría un error cuando se implementa en producción. Por lo tanto, para hacer que el entorno de prueba que reflejar con más precisión el entorno de producción, configurará la aplicación se ejecute en el nivel de confianza medio.

En el archivo Web.config de aplicación, agregue un **confianza** elemento en el **system.web** elemento, tal como se muestra en este ejemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

La aplicación se ejecutará ahora en el nivel de confianza medio en IIS, incluso en el equipo local. Esta configuración le permite detectar tan pronto como sea posible cualquier intento por código de aplicación para hacer algo que produciría un error en producción.

> [!NOTE]
> Si usa Entity Framework Code First Migrations, asegúrese de que tiene la versión 5.0 o posterior instalado. En Entity Framework versión 4.3, migraciones requiere plena confianza para poder actualizar el esquema de base de datos.


## <a name="installing-iis-and-web-deploy"></a>Instalar IIS y Web Deploy

Para implementar en IIS en el equipo de desarrollo, debe tener IIS y Web Deploy instalado. No se incluyen en la configuración predeterminada de Windows 7. Si ya ha instalado IIS y Web Deploy, vaya a la sección siguiente.

Mediante el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx) es la mejor manera de instalar IIS y Web Deploy, porque el instalador de plataforma Web instala una configuración recomendada de IIS e instala automáticamente los requisitos previos para IIS y Web Implementar si es necesario.

Para ejecutar el instalador de plataforma Web para instalar IIS y Web Deploy, utilice el siguiente vínculo. Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, el instalador de plataforma Web instala solo lo que faltan.

- [Instalar IIS y Web Deploy mediante WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Si se establece el grupo de aplicaciones predeterminado en .NET 4

Después de instalar IIS, ejecute **el Administrador de IIS** para asegurarse de que la versión 4 de .NET Framework se asigna al grupo de aplicaciones predeterminado.

Desde las ventanas **iniciar** menú, seleccione **ejecutar**, escriba "inetmgr" y, a continuación, haga clic en **Aceptar**. (Si la **ejecutar** comando no está en su **iniciar** menú, puede presionar la tecla Windows y R para abrirlo. O haga clic en la barra de tareas, haga clic en **propiedades**, seleccione la **menú Inicio** , haga clic en **personalizar**y seleccione **ejecutar comando**.)

En el **conexiones** panel, expanda el nodo del servidor y seleccione **grupos de aplicaciones**. En el **grupos de aplicaciones** panel, si **DefaultAppPool** está asignado a .NET framework versión 4, como se muestra en la siguiente ilustración, vaya a la sección siguiente.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Si ve solo dos grupos de aplicaciones y ambos se establecen en la versión 2.0 de .NET Framework, tendrá que instalar 4 de ASP.NET en IIS:

- Abra una ventana del símbolo del sistema haciendo clic en **símbolo** en las ventanas de **iniciar** menú y seleccionando **ejecutar como administrador**. A continuación, ejecute [aspnet\_regiis.exe](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx) para instalar ASP.NET 4 en IIS, utilizando los comandos siguientes. (En sistemas de 64 bits, reemplace "Framework" con "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando crea nuevos grupos de aplicaciones para .NET Framework 4, pero aun así se establecerá el grupo de aplicaciones predeterminado a 2.0. Implementará una aplicación que tiene como destino .NET 4 para ese grupo de aplicaciones, por lo que debe cambiar el grupo de aplicaciones en .NET 4.

Si lo ha cerrado **el Administrador de IIS**, vuelva a ejecutarlo, expanda el nodo del servidor y haga clic en **grupos de aplicaciones** para mostrar la **grupos de aplicaciones** nuevo panel.

En el **grupos de aplicaciones** panel, haga clic en **DefaultAppPool**y, a continuación, en la **acciones** panel, haga clic **configuración básica**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

En el **Modificar grupo de aplicaciones** cuadro de diálogo, cambie **versión de .NET Framework** a **.NET Framework v4.0.30319** y haga clic en **Aceptar**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Ahora está listo para publicar en IIS.

## <a name="publishing-to-iis"></a>Publicar en IIS

Existen varias formas que se pueden implementar mediante Visual Studio 2010 y Web Deploy:

- Usar Visual Studio publicar un solo clic.
- Crear un *paquete de implementación* e instalarlo usando la UI del Administrador de IIS. El paquete de implementación está formada por un *.zip* archivo que contiene todos los archivos y los metadatos necesarios para instalar un sitio en IIS.
- Crear un paquete de implementación e instalarlo mediante la línea de comandos.

El proceso que llevamos a cabo en los tutoriales anteriores para configurar Visual Studio para automatizar tareas de implementación se aplica a todos estos tres métodos. En estos tutoriales usará el primero de estos métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [mapa de contenido de implementación de ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx).

Antes de publicar, asegúrese de que está ejecutando Visual Studio en modo de administrador. (En Windows 7 **iniciar** menú, haga clic en el icono de la versión de Visual Studio que se use y seleccione **ejecutar como administrador**.) Modo de administrador es necesario para publicar solo cuando se publican en IIS en el equipo local.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity (no en el proyecto ContosoUniversity.DAL) y seleccione **publicar**.

El **Publicar Web** aparece el Asistente para.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

En la lista desplegable, seleccione  **&lt;nuevo... &gt;**.

En el **nuevo perfil** cuadro de diálogo, escriba "Test" y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo Nuevo perfil](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Este nombre es que el mismo que el nodo central de la Web.Test.config transformar el archivo que creó anteriormente. Esta correspondencia es lo que hace que las transformaciones de Web.Test.config que se aplicará cuando se publica utilizando este perfil.

El asistente avanza automáticamente a la **conexión** ficha.

En el **dirección URL del servicio** cuadro, escriba *localhost*.

En el **sitio o aplicación** cuadro, escriba *sitio Web predeterminado/ContosoUniversity*.

En el **dirección URL de destino** cuadro, escriba `http://localhost/ContosoUniversity`.

El **dirección URL de destino** configuración no es necesaria. Cuando Visual Studio finaliza la implementación de la aplicación, automáticamente se abre el explorador predeterminado para esta dirección URL. Si no desea que el explorador para abrir automáticamente después de la implementación, deje este cuadro en blanco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

Una marca de verificación verde comprueba que la conexión es correcta.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Haga clic en **siguiente** para avanzar a la **configuración** ficha.

El **configuración** cuadro desplegable especifica la configuración de compilación para implementar. El valor predeterminado es la versión, lo que es lo que desea.

Deje el **quitar archivos adicionales en destino** casilla está desactivada. Puesto que es la primera implementación, no haber ningún archivo en la carpeta de destino aún.

En el **bases de datos** sección, escriba el siguiente valor en el cuadro de la cadena de conexión para **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **utilizar esta cadena de conexión en tiempo de ejecución** está seleccionada.

También en **SchoolContext**, seleccione **aplicar Code First Migrations**. Esta opción hace que el proceso de implementación configurar el archivo Web.config implementado para especificar el `MigrateDatabaseToLatestVersion` inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación tiene acceso a la base de datos por primera vez después de la implementación.

En el cuadro de la cadena de conexión para **DefaultConnection**, escriba el siguiente valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deje **Actualizar base de datos** desactivada. La base de datos de pertenencia que se va a implementar copiando el archivo .sdf en aplicación\_datos y no desea que el proceso de implementación que hacer nada más con esta base de datos.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Haga clic en **siguiente** para avanzar a la **vista previa** ficha.

En el **vista previa** , haga clic en **iniciar Preview** para ver una lista de los archivos que se va a copiar.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Haga clic en **Publicar**.

Si Visual Studio no está en modo de administrador, podría obtener un mensaje de error que indica un error de permisos. En ese caso, cierre Visual Studio, abra en modo de administrador y, intente volver a publicar.

Si Visual Studio está en modo de administrador, el **salida** informes de la ventana correcta generación y publicación.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

El explorador se abre automáticamente en la página principal de universidad de Contoso que se ejecutan en IIS en el equipo local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Realización de pruebas en el entorno de prueba

Tenga en cuenta que el indicador de entorno mostrará "(Test)" en lugar de "(desarrollo)", que muestra que la *Web.config* transformación para el indicador de entorno fue correcta.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Ejecute el **estudiantes** página para comprobar que la base de datos implementada no tiene ningún alumno. Cuando se selecciona esta página puede tardar unos minutos en cargarse porque crea la base de datos de Code First y, a continuación, se ejecuta el `Seed` método. (No hacen cuando estaban en la página principal porque la aplicación no intenta obtener acceso a la base de datos todavía).

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos instructor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el nuevo alumno en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Desde el **cursos** menú, seleccione **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **inicio de sesión** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "Pa$ w0rd"). El **actualización créditos** aparece la página, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Compruebe que un *Elmah* carpeta existe con el archivo de marcador de posición en él.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar los cambios de Web.config automática para migraciones de Code First

Abra la *Web.config* archivo en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y verá que el proceso de implementación configurado migraciones de Code First para automáticamente actualizar la base de datos a la versión más reciente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

El proceso de implementación también crea una nueva cadena de conexión para migraciones de Code First utilizar exclusivamente para actualizar el esquema de base de datos:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Esta cadena de conexión adicionales le permite especificar una cuenta de usuario para las actualizaciones del esquema de base de datos y una cuenta de usuario diferente para el acceso de datos de aplicación. Por ejemplo, puede asignar la base de datos\_rol de propietario para migraciones de Code First y base de datos\_datareader y db\_datawriter roles a la aplicación. Se trata de un patrón común de defensa en profundidad que evita que un código potencialmente malintencionado en la aplicación de cambiar el esquema de base de datos. (Por ejemplo, esto podría suceder en un ataque de inyección de SQL). Este patrón no se utiliza por estos tutoriales. No se aplica a SQL Server Compact y no se aplica al migrar a SQL Server en un tutorial posterior de esta serie. El sitio de Cytanium ofrece una sola cuenta de usuario para tener acceso a la base de datos de SQL Server que usted crea en Cytanium. Si es posible implementar este patrón en el escenario, puede hacerlo mediante la realización de los pasos siguientes:

1. En el **configuración** pestaña de la **Publicar Web** asistente, escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquema de base de datos completa y desactive el **usar esta cadena de conexión en tiempo de ejecución** casilla de verificación. En el archivo Web.config implementado, se convierte en el `DatabasePublish` cadena de conexión.
2. Crear una transformación del archivo Web.config para la cadena de conexión que desea que la aplicación para utilizar en tiempo de ejecución.

Ahora ha implementado la aplicación en IIS en el equipo de desarrollo y probado no existe. Esto comprueba que el proceso de implementación copia el contenido de la aplicación en la ubicación correcta (excepto los archivos que va a implementar), y también configurar ese Web Deploy a IIS correctamente durante la implementación. En el siguiente tutorial, va a ejecutar una prueba más que busca una tarea de implementación que no se ha realizado aún: establecer permisos para carpetas en el *Elmah* carpeta.

## <a name="more-information"></a>Más información

Para obtener información sobre la ejecución de IIS o IIS Express en Visual Studio, vea los siguientes recursos:

- [Introducción a IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) en el sitio de IIS.net.
- [Introducción a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Guthrie.
- [Cómo: especificar el servidor Web para proyectos Web de Visual Studio](https://msdn.microsoft.com/en-us/library/ms178108.aspx).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio ASP.NET.
- [Probar sus MVC de ASP.NET o una aplicación de formularios Web en IIS 7 en 30 segundos](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) en el blog de Rick Anderson. Esta entrada proporciona ejemplos de por qué no es tan confiable como prueba en IIS Express prueba con el servidor de desarrollo de Visual Studio (Cassini) y por qué pruebas en IIS Express no serán tan confiable como prueba en IIS.

Para obtener información acerca de los problemas que puede surgir cuando la aplicación se ejecuta con confianza media, consulte [hospedaje de aplicaciones de ASP.NET en el nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los 4 encargados de Rolla sitio.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
