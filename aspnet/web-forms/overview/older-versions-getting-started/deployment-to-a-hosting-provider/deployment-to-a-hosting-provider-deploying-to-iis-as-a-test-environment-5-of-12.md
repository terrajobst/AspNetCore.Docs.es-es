---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en IIS como entorno de prueba - 5 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828846"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en IIS como entorno de prueba - 5 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo implementar una aplicación web ASP.NET en IIS en el equipo local.

Al desarrollar una aplicación, por lo general probar ejecutándolo en Visual Studio. De forma predeterminada, esto significa que está usando el servidor de desarrollo de Visual Studio (también conocido como Cassini). El servidor de desarrollo de Visual Studio facilita la prueba durante el desarrollo en Visual Studio, pero no funciona exactamente igual que IIS. Como resultado, es posible que una aplicación se ejecute correctamente al probarlo en Visual Studio, pero producirá un error cuando se implementa en un entorno de hospedaje en IIS.

Puede probar la aplicación de forma más confiable de las siguientes maneras:

1. Utilice completa de IIS o IIS Express en lugar del servidor de desarrollo de Visual Studio cuando se prueba en Visual Studio durante el desarrollo. Este método generalmente emula con más precisión cómo se ejecutará el sitio en IIS. Sin embargo, este método no prueba el proceso de implementación ni validar que el resultado del proceso de implementación se ejecutará correctamente.
2. Implementar la aplicación en IIS en el equipo de desarrollo utilizando el mismo proceso que usará más adelante para implementarlo en su entorno de producción. Este método valida su proceso de implementación además de validar que la aplicación se ejecutará correctamente en IIS.
3. Implementar la aplicación en un entorno de prueba que sea lo más parecido posible al entorno de producción. Puesto que el entorno de producción para estos tutoriales es un proveedor de hospedaje de terceros, el entorno de prueba ideal sería una segunda cuenta con el proveedor de hospedaje. Usaría esta segunda cuenta solo para las pruebas, pero podría configurarse del mismo modo que la cuenta de producción.

Este tutorial muestra los pasos para la opción 2. Se proporciona orientación para la opción 3 al final de la [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, y al final de este tutorial se incluyen vínculos a recursos para la opción 1.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configuración de la aplicación para ejecutarse en el nivel de confianza medio

Antes de instalar IIS y la implementación en él, va a cambiar una configuración del archivo Web.config con el fin de hacer que el sitio ejecute más como lo hará en un entorno de hospedaje compartido típico.

Los proveedores de hospedaje, normalmente ejecutan su sitio web *confianza Media*, lo que significa que hay algunas cosas que no se permite hacer. Por ejemplo, el código de aplicación no puede tener acceso al registro de Windows y no se puede leer o escribir archivos que se encuentran fuera de la jerarquía de carpetas de la aplicación. De forma predeterminada, la aplicación se ejecuta *confianza alta* en el equipo local, lo que significa que la aplicación podría ser capaz de hacer las cosas que produciría un error al implementarla en producción. Por lo tanto, para hacer que el entorno de prueba que más reflejan con precisión el entorno de producción, configurará la aplicación se ejecute en el nivel de confianza medio.

En el archivo Web.config de aplicación, agregue un **confianza** elemento en el **system.web** elemento, como se muestra en este ejemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

La aplicación se ejecutará ahora en el nivel de confianza medio en IIS, incluso en el equipo local. Esta configuración le permite detectar cualquier intento de tan pronto como sea posible por código de aplicación para hacer algo que produciría un error en producción.

> [!NOTE]
> Si usa migraciones de Entity Framework Code First, asegúrese de que tiene la versión 5.0 o posterior instalado. En la versión 4.3 de Entity Framework, las migraciones requiere plena confianza con el fin de actualizar el esquema de base de datos.


## <a name="installing-iis-and-web-deploy"></a>Implementación de instalación de IIS y Web

Para implementar en IIS en el equipo de desarrollo, debe tener IIS y Web Deploy instalado. No se han incluido en la configuración predeterminada de Windows 7. Si ya ha instalado IIS y Web Deploy, vaya a la sección siguiente.

Mediante el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx) es la mejor manera de instalar IIS y Web Deploy, porque el instalador de plataforma Web instala una configuración recomendada de IIS e instala automáticamente los requisitos previos para IIS y Web Implementar si es necesario.

Para ejecutar el instalador de plataforma Web para instalar IIS y Web Deploy, use el siguiente vínculo. Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, el instalador de plataforma Web instala solo lo que falta.

- [Instalar IIS y Web Deploy mediante WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Si se establece el grupo de aplicaciones predeterminado en .NET 4

Después de instalar IIS, ejecute **IIS Manager** para asegurarse de que la versión 4 de .NET Framework se asigna al grupo de aplicaciones de forma predeterminada.

Desde el Windows **iniciar** menú, seleccione **ejecutar**, escriba "inetmgr" y, a continuación, haga clic en **Aceptar**. (Si la **ejecutar** comando no está en su **iniciar** menú, puede presionar la tecla Windows y R para abrirlo. O haga clic en la barra de tareas, haga clic en **propiedades**, seleccione el **menú Inicio** , haga clic **personalizar**y seleccione **ejecutar comando**.)

En el **conexiones** panel, expanda el nodo del servidor y seleccione **grupos de aplicaciones**. En el **grupos de aplicaciones** panel, si **DefaultAppPool** está asignado a la versión de .NET framework 4 como se muestra en la ilustración siguiente, vaya a la sección siguiente.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Si ve solo dos grupos de aplicaciones y ambos se establecen en .NET Framework 2.0, debe instalar el 4 de ASP.NET en IIS:

- Abra una ventana del símbolo del sistema con el botón secundario **símbolo** en el Windows **iniciar** menú y seleccionando **ejecutar como administrador**. A continuación, ejecute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar ASP.NET 4 en IIS, mediante los siguientes comandos. (En sistemas de 64 bits, reemplace "Framework" con "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando crea nuevos grupos de aplicaciones para .NET Framework 4, pero aún así se establecerá el grupo de aplicaciones predeterminado a 2.0. Implementará una aplicación que tiene como destino .NET 4 para ese grupo de aplicaciones, por lo que debe cambiar el grupo de aplicaciones en .NET 4.

Si ha cerrado **IIS Manager**, ejecútelo de nuevo, expanda el nodo del servidor y haga clic en **grupos de aplicaciones** para mostrar el **grupos de aplicaciones** nuevo panel.

En el **grupos de aplicaciones** panel, haga clic en **DefaultAppPool**y, a continuación, en el **acciones** panel, haga clic **configuración básica**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

En el **Modificar grupo de aplicaciones** cuadro de diálogo, cambie **versión de .NET Framework** a **.NET Framework v4.0.30319** y haga clic en **Aceptar**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Ahora está listo para publicar en IIS.

## <a name="publishing-to-iis"></a>Publicar en IIS

Hay varias maneras puede implementar mediante Visual Studio 2010 y Web Deploy:

- Publicar el uso de Visual Studio con un solo clic.
- Crear un *paquete de implementación* e instalarlo mediante la UI del Administrador IIS. El paquete de implementación consta de un *.zip* archivo que contiene todos los archivos y los metadatos necesarios para instalar un sitio en IIS.
- Crear un paquete de implementación e instalarlo mediante la línea de comandos.

El proceso que llevamos a cabo en los tutoriales anteriores para configurar Visual Studio para automatizar las tareas de implementación se aplica a todos estos tres métodos. En estos tutoriales se usará el primero de estos métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [mapa de contenido de implementación ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Antes de la publicación, asegúrese de que se está ejecutando Visual Studio en modo de administrador. (En el de Windows 7 **iniciar** menú, haga clic en el icono de la versión de Visual Studio que está usando y seleccione **ejecutar como administrador**.) Modo de administrador es necesario para publicar solo cuando va a publicar en IIS en el equipo local.

En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity (no el proyecto ContosoUniversity.DAL) y seleccione **publicar**.

El **publicación Web** aparece el asistente.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

En la lista desplegable, seleccione  **&lt;nuevo... &gt;**.

En el **nuevo perfil** cuadro de diálogo, escriba "Test" y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo Nuevo perfil](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Este nombre es que el mismo que el nodo central de la Web.Test.config transformar el archivo que creó anteriormente. Esta correspondencia es lo que hace que las transformaciones Web.Test.config aplicarse cuando se publica mediante este perfil.

El asistente avanza automáticamente a la **conexión** ficha.

En el **dirección URL del servicio** , escriba *localhost*.

En el **sitio o aplicación** , escriba *Default Web Site/ContosoUniversity*.

En el **dirección URL de destino** , escriba `http://localhost/ContosoUniversity`.

El **dirección URL de destino** configuración no es necesaria. Cuando Visual Studio termine de implementar la aplicación, se abre automáticamente el explorador predeterminado para esta dirección URL. Si no desea que el explorador para abrir automáticamente después de la implementación, deje este cuadro en blanco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

Una marca de verificación verde comprueba que la conexión es correcta.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Haga clic en **siguiente** para avanzar a la **configuración** ficha.

El **configuración** cuadro desplegable especifica la configuración de compilación para implementar. El valor predeterminado es la versión, que es lo que desea.

Deje el **quitar archivos adicionales en destino** casilla desactivada. Puesto que es la primera implementación, no habrá ningún archivo en la carpeta de destino aún.

En el **bases de datos** sección, escriba el siguiente valor en el cuadro cadena de conexión para **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

El proceso de implementación, se colocará esta cadena de conexión en el archivo Web.config implementado porque **usar esta cadena de conexión en tiempo de ejecución** está seleccionada.

También en **SchoolContext**, seleccione **aplicar Code First Migrations**. Esta opción hace que el proceso de implementación configurar el archivo Web.config implementado para especificar el `MigrateDatabaseToLatestVersion` inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación tiene acceso a la base de datos por primera vez después de la implementación.

En el cuadro cadena de conexión para **DefaultConnection**, escriba el siguiente valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deje **Actualizar base de datos** desactivada. La base de datos de pertenencia se va a implementar copiando el archivo .sdf en aplicación\_datos y no desea que el proceso de implementación que hacer nada más con esta base de datos.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Haga clic en **siguiente** para avanzar a la **Preview** ficha.

En el **Preview** , haga clic **iniciar vista previa** para ver una lista de los archivos que se va a copiar.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Haga clic en **Publicar**.

Si Visual Studio no está en modo de administrador, puede aparecer un mensaje de error que indica un error de permisos. En ese caso, cierre Visual Studio, abrirlo en modo de administrador y, intente volver a publicar.

Si Visual Studio está en modo de administrador, el **salida** informes correcta de la ventana de compilación y publicación.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

El explorador se abre automáticamente en la página principal de la Universidad de Contoso que se ejecutan en IIS en el equipo local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Las pruebas en el entorno de prueba

Tenga en cuenta que el indicador de entorno muestra "(prueba)" en lugar de "(desarrollo)", que muestra que el *Web.config* transformación para el indicador de entorno fue correcta.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Ejecute el **estudiantes** página para comprobar que la base de datos implementada no tiene ningún estudiante. Cuando se selecciona esta página puede tardar unos minutos en cargarse porque Code First crea la base de datos y, a continuación, se ejecuta el `Seed` método. (No hace cuando estaban en la página principal porque la aplicación no intenta tener acceso a la base de datos todavía.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos de un instructor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el alumno nuevo en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Desde el **cursos** menú, seleccione **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **en el registro** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "Pa$ w0rd"). El **actualización créditos** página se muestra, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Compruebe que un *Elmah* carpeta existe con el archivo de marcador de posición en él.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar los cambios de Web.config automática para migraciones de Code First

Abra el *Web.config* archivo en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y puede ver que el proceso de implementación configurado migraciones de Code First para automáticamente actualizar la base de datos a la versión más reciente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

El proceso de implementación también crea una nueva cadena de conexión para migraciones de Code First exclusivamente actualizar el esquema de base de datos:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Esta cadena de conexión adicionales le permite especificar una cuenta de usuario para las actualizaciones del esquema de base de datos y una cuenta de usuario diferente para el acceso a datos de aplicación. Por ejemplo, podría asignar la base de datos\_rol de propietario para migraciones de Code First y db\_datareader y db\_datawriter roles a la aplicación. Se trata de un patrón común de defensa en profundidad que impide que el código potencialmente malintencionado en la aplicación modifiquen el esquema de base de datos. (Por ejemplo, esto puede suceder en un ataque de inyección SQL.) No se utiliza este patrón por estos tutoriales. No se aplica a SQL Server Compact y no se aplica al migrar a SQL Server en un tutorial más adelante en esta serie. El sitio Cytanium ofrece una sola cuenta de usuario para tener acceso a la base de datos de SQL Server que se crea en Cytanium. Si es capaz de implementar este patrón en su escenario, puede hacerlo mediante los pasos siguientes:

1. En el **configuración** pestaña de la **publicación Web** asistente, escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquema de base de datos completa y desactive el **usar esta cadena de conexión en tiempo de ejecución** casilla de verificación. En el archivo Web.config implementado, esto se convierte en el `DatabasePublish` cadena de conexión.
2. Crear una transformación del archivo Web.config para la cadena de conexión que desea que la aplicación para usar en tiempo de ejecución.

Ha implementado la aplicación en IIS en el equipo de desarrollo y haya probado. Esto comprueba que el proceso de implementación copia el contenido de la aplicación en la ubicación correcta (excepto los archivos que va a implementar), y también configurar ese Web Deploy a IIS correctamente durante la implementación. En el siguiente tutorial, va a ejecutar una prueba más que busca una tarea de implementación que aún no se han realizado: establecer permisos de carpeta en el *Elmah* carpeta.

## <a name="more-information"></a>Más información

Para obtener información sobre la ejecución de IIS o IIS Express en Visual Studio, consulte los siguientes recursos:

- [Introducción a IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) en el sitio de IIS.net.
- [Introducción a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Scott Guthrie.
- [Cómo: especificar el servidor Web para proyectos Web en Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio de ASP.NET.
- [Probar su aplicación de formularios de Web en IIS 7 o ASP.NET MVC en 30 segundos](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) en el blog de Rick Anderson. Esta entrada proporciona ejemplos de por qué las pruebas con el servidor de desarrollo de Visual Studio (Cassini) no es tan confiable como las pruebas en IIS Express, y por qué las pruebas en IIS Express no es tan confiable como las pruebas en IIS.

Para obtener información acerca de los problemas que puede surgir cuando se ejecuta la aplicación en el nivel de confianza medio, consulte [de hospedaje de aplicaciones de ASP.NET en el nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los chicos del sitio Rolla 4.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
