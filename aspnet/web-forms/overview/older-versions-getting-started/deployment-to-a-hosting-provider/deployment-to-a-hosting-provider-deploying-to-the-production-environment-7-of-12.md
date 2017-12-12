---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementarla en el entorno de producción - 7 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ad44968975b7929f5b0f70334deabc7238797402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementarla en el entorno de producción - 7 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar en aplicaciones de Web del servicio de aplicación de Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, se configure una cuenta con un proveedor de hospedaje e implementar su ASP.NET característica de publicación de aplicación web en el entorno de producción mediante el uso de Visual Studio con un solo clic.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Al seleccionar un proveedor de hospedaje

Para la aplicación de la Universidad de Contoso y esta serie de tutoriales, necesita un proveedor que admita ASP.NET 4 y Web Deploy. Una empresa de hospedaje específica se ha elegido para que los tutoriales muestran la experiencia completa de la implementación en un sitio Web. Cada empresa de hospedaje proporciona diversas características y la experiencia de la implementación de sus servidores varía ligeramente. Sin embargo, el proceso descrito en este tutorial es habitual para el proceso general. El proveedor de hospedaje que se utiliza para este tutorial, Cytanium.com, es uno de muchos de los que están disponibles y su uso en este tutorial no constituye una aprobación ni recomendación.

Cuando esté listo para seleccionar su propio proveedor de hospedaje, puede comparar características y precios que figuran en la [Galería de proveedores de](https://www.microsoft.com/web/hosting) en el sitio Web de Microsoft.com.

## <a name="creating-an-account"></a>Crear una cuenta

Cree una cuenta en el proveedor seleccionado. Si la compatibilidad con una base de datos completa de SQL Server es un agregado adicional, no es necesario seleccionar para este tutorial, pero necesitará para la [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial más adelante en esta serie.

Para estos tutoriales, no tienes que registrar un nombre de dominio nuevo. Puede probar para comprobar una implementación correcta mediante la dirección URL temporal asignada al sitio por el proveedor.

Una vez creada la cuenta, normalmente recibirá un correo electrónico de bienvenida que contiene toda la información que necesita para implementar y administrar el sitio. La información que envía el proveedor de hospedaje será similar a lo que se muestra aquí. El correo de bienvenida Cytanium que se envía a los propietarios de cuenta nuevo incluye la información siguiente:

- La dirección URL al sitio de panel de control del proveedor, donde puede administrar la configuración para el sitio. El identificador y la contraseña que especificó se incluyen en esta parte del correo electrónico de bienvenida para facilitar su consulta. (Ambos se han cambiado para un valor de demostración de esta ilustración.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versión de .NET Framework de forma predeterminada y obtener información sobre cómo cambiarla. Muchos hospedaje sitios predeterminada a 2.0, que funciona con aplicaciones de ASP.NET que tienen como destino .NET Framework 2.0, 3.0 o 3.5. Sin embargo Universidad de Contoso es una aplicación de .NET Framework 4, por lo que debe cambiar esta configuración. (Para una aplicación de ASP.NET 4.5 se usa la configuración de .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- La dirección URL temporal que puede usar para tener acceso a su sitio web. Cuando se creó esta cuenta, se escribió "contosouniversity.com" como el nombre de dominio existente. Por lo tanto, es la dirección URL temporal `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Obtener información sobre cómo configurar las bases de datos y las cadenas de conexión que necesita para tener acceso a ellos:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Información sobre herramientas y configuración de implementación del sitio. (El correo electrónico desde Cytanium también menciona WebMatrix, que se omite aquí).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Configuración de la versión de .NET Framework

El correo electrónico de bienvenida Cytanium incluye un vínculo para obtener instrucciones sobre cómo cambiar la versión de .NET Framework. Estas instrucciones se explica que esto puede hacerse a través del panel de control Cytanium. Otros proveedores tienen sitios de panel de control que tienen un aspecto distintos, o puede indicar a hacerlo de forma diferente.

Vaya a la dirección URL de panel de control. Después de iniciar sesión con su nombre de usuario y contraseña, consulte el panel de control.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

En el **hospeda espacios** , mantenga el puntero sobre el icono de Web y seleccione **sitios Web** en el menú.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

En el **sitios Web** cuadro, haga clic en **contosouniversity.com** (el nombre del sitio que utilizó cuando creó la cuenta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

En el **propiedades de sitio Web** cuadro, seleccione la **extensiones** ficha.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Cambiar ASP.NET desde **2.0 canalización integrada** a **4.0 (canalización integrada)**y, a continuación, haga clic en **actualización**.

## <a name="publishing-to-the-hosting-provider"></a>Publicación en el proveedor de hospedaje

El correo electrónico de bienvenida del proveedor de hospedaje incluye todos los valores que necesarios para publicar el proyecto, y puede escribir manualmente esa información en un perfil de publicación. Pero usará una más fácil y menos propenso a errores método para configurar la implementación para el proveedor: se descargará un *.publishsettings* archivo e importarla a un perfil de publicación.

En el explorador, vaya al panel de control Cytanium y seleccione **Web** y, a continuación, seleccione **sitios Web.**

![Panel de control seleccionando los sitios Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Seleccione el **contosouniversity.com** sitio web.

![Seleccionar contosouniversity.com el Panel de control](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Seleccione el **publicación Web** ficha.

![Pestaña de publicación de Web del Panel de control](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Crear las credenciales para escribiendo un nombre de usuario y una contraseña de publicación en web. Puede escribir las mismas credenciales que usas para iniciar sesión en el panel de control. A continuación, haga clic en **habilitar**.

![El Panel de control crear credenciales de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Haga clic en **descargar perfil de publicación para este sitio web**.

![Perfil de publicación de descarga del Panel de control](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Cuando se le pida que abra o guarde el archivo, guárdelo.

![Guarde el archivo del perfil de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

En **el Explorador de soluciones** en Visual Studio, haga clic en el proyecto ContosoUniversity y seleccione **publicar**. El **Publicar Web** abre el cuadro de diálogo en el **vista previa** pestaña con el **prueba** perfil seleccionado, ya que es el último perfil usa.

Seleccione el **perfil** ficha y, a continuación, haga clic en **importación**.

![Web Asistente para importación botón Publicar](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

En el **importar la configuración de publicación** cuadro de diálogo, seleccione la *.publishsettings* archivo que descargó y haga clic en **abiertos**. El asistente avanza a la ficha conexión con todos los campos que se rellena.

![Publicar la ficha de conexión del Asistente de Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

El archivo .publishsettings coloca la dirección de URL permanente planeada para el sitio en el cuadro de dirección URL de destino, pero si aún no ha adquirido ese dominio, reemplace el valor con la dirección URL temporal. En este ejemplo, la dirección URL es  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Es el único propósito de este cuadro especificar qué dirección URL del explorador abrirá automáticamente después de correctamente después de la implementación. Si se deja en blanco, la única consecuencia es que el explorador no se inicia automáticamente después de la implementación.

Haga clic en **validar conexión** para comprobar que la configuración es correcta y que puede conectarse al servidor. Como se vio anteriormente, una marca de verificación verde comprueba que la conexión es correcta.

Al hacer clic en validar conexión, puede aparecer un **Error de certificado** cuadro de diálogo. Si lo hace, compruebe que el nombre del servidor es el esperado. Si es así, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Accept**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de adquirir un certificado SSL para la dirección URL que va a implementar. Si desea establecer una conexión segura mediante el uso de un certificado válido, póngase en contacto con su proveedor de hospedaje.)

![Error de certificado](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Haga clic en **Siguiente**.

En el **bases de datos** sección de la **configuración** ficha, especifique el mismo perfil de publicación de los valores que ha especificado para la prueba. Encontrará las cadenas de conexión que necesita en las listas desplegables.

- En el cuadro de la cadena de conexión para **SchoolContext,** seleccionar`Data Source=|DataDirectory|School-Prod.sdf`
- En **SchoolContext**, seleccione **aplicar Code First Migrations**.
- En el cuadro de la cadena de conexión para **DefaultConnection**, seleccione`Data Source=|DataDirectory|aspnet-Prod.sdf`
- En **DefaultConnection**, deje **Actualizar base de datos** desactivada.

![Publicar la pestaña de configuración del Asistente de Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Haga clic en **Siguiente**.

En el **vista previa** , haga clic en **iniciar Preview** para ver una lista de los archivos que se va a copiar. Verá la misma lista que vio anteriormente cuando implementa en IIS en el equipo local.

Antes de publicar, cambie el nombre del perfil para que se aplicará el archivo de transformación Web.Production.config. Seleccione el **perfil** ficha y haga clic en **administrar perfiles**.

![Web Asistente para administrar perfiles de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

En el **Editar perfiles de publicación de Web** diálogo cuadro, seleccione el perfil de producción, haga clic en **cambiar el nombre de**y cambie el nombre del perfil a producción. A continuación, haga clic en **cerrar**.

![Editar cuadro de diálogo perfiles de publicación de Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Haga clic en **Publicar**.

La aplicación se publica en el proveedor de hospedaje. El resultado se muestra en el **salida** ventana.

![Ventana de salida después de la implementación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

El explorador se abre automáticamente en la dirección URL que escribió en **dirección URL de destino** cuadro en el **conexión** pestaña de la **Publicar Web** asistente. Verá la misma página de inicio que cuando ejecute el sitio Web en Visual Studio, excepto que ahora no hay ninguna "(Test)" o "(desarrollo)" entorno indicador en la barra de título. Esto indica que el indicador de entorno *Web.config* transformación funcionó correctamente.

> [!NOTE]
> Si todavía aparece "(Test)" en el encabezado, elimine la *obj* carpeta desde el proyecto ContosoUniversity y volver a implementar. En las versiones preliminares del software, el archivo de transformación aplicada anteriormente (Web.Test.config) puede obtener aplicarse de nuevo aunque use el perfil de producción.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Antes de ejecutar una página que hace que el acceso de base de datos, asegúrese de que Elmah podrá registrar los errores que se producen.

## <a name="setting-folder-permissions-for-elmah"></a>Establecer los permisos de carpeta para Elmah

Cuando tenga en cuenta en el tutorial anterior de esta serie, debe asegurarse de que la aplicación tiene permisos de escritura para la carpeta en la aplicación donde Elmah almacena archivos de registro de errores. Cuando implementa en IIS localmente en el equipo, se establecen los permisos manualmente. En esta sección, verá cómo establecer permisos en Cytanium. (Algunos proveedores de hospedaje pueden no le permiten hacer esto, es posible que ofrezcan una o varias carpetas predefinidas con permisos de escritura. En ese caso tendría que modificar la aplicación para usar las carpetas especificadas.)

Puede establecer los permisos de carpeta en el panel de control de Cytanium. Ir al control panel dirección URL y seleccione **el Administrador de archivos**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

En el **el Administrador de archivos** cuadro, seleccione **contosouniversity.com** y, a continuación, **wwwrooot** para ver la carpeta raíz de la aplicación. Haga clic en el icono de candado junto a **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

En el **archivo**/**permisos de la carpeta** ventana, seleccione la **lectura** y **escribir** casillas de verificación de  **contosouniversity.com** y haga clic en **establecer permisos**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Asegúrese de que Elmah tiene acceso de escritura a la *Elmah* carpeta, produciendo un error y, a continuación, mostrar el informe de errores de Elmah. Solicitar una dirección URL no válida como *Studentsxxx.aspx*. Como antes, verá el *GenericErrorPage.aspx* página. Haga clic en el **Log Out** vincular y, a continuación, ejecute *Elmah.axd*. Obtiene el **inicio de sesión** en primer lugar, página que valida que la *Web.config* transformación Elmah autorización agregó correctamente. Tras iniciar sesión, se mostrará el informe que muestra el error que provocó el solo.

[![ELMAH.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Realización de pruebas en el entorno de producción

Ejecute el **estudiantes** página. La aplicación intenta tener acceso a la base de datos School por primera vez, lo que desencadena migraciones de Code First para crear la base de datos. Cuando se muestra la página después de retraso de unos instantes, muestra que no hay ningún alumno.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Ejecute el **instructores** página para comprobar que los datos de inicialización se ha insertado correctamente datos instructor en la base de datos.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Como hizo en el entorno de prueba, desea comprobar que las actualizaciones de base de datos funcionen en el entorno de producción, pero normalmente no desea escribir datos de prueba en la base de datos de producción. Para este tutorial, usará el mismo método que hizo en prueba. Pero en una aplicación real que se desea buscar un método que valida esa base de datos, las actualizaciones son correctas sin la introducción de datos de prueba en la base de datos de producción. En algunas aplicaciones, puede resultar práctico agregar algo y, a continuación, elimínelo.

Agregar un estudiante y, a continuación, ver los datos especificados en el **estudiantes** página para comprobar que se pueden actualizar datos en la base de datos.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Validar que las reglas de autorización funciona correctamente mediante la selección **actualización créditos** desde el **cursos** menú. El **inicio de sesión** se muestra la página. Escriba sus credenciales de cuenta de administrador, haga clic en **inicio de sesión**y el **actualización créditos** se muestra la página.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Si el inicio de sesión es correcto, el **actualización créditos** se muestra la página. Esto indica que la base de datos de pertenencia ASP.NET (con la cuenta de administrador de inicio único) se ha implementado correctamente.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Ahora ha implementado y probado el sitio y está disponible públicamente a través de Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Crear un entorno de prueba más confiable

Como se explica en la [implementarla en el entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial, el entorno de prueba más fiable sería una segunda cuenta en el proveedor de hospedaje que simplemente como la cuenta de producción. Esto podría resultar más costoso que el uso de IIS local como el entorno de prueba, ya que tendría que suscribirse a una segunda cuenta de hospedaje. Pero si se evita que los errores del sitio de producción o las interrupciones, podría decidir que merece la pena el coste.

La mayor parte del proceso de creación e implementación de una cuenta de prueba es similar a lo que ya ha hecho para implementarse en producción:

- Crear un *Web.config* archivo de transformación.
- Cree una cuenta en el proveedor de hospedaje.
- Crear un nuevo perfil de publicación e implementar en la cuenta de prueba.

### <a name="preventing-public-access-to-the-test-site"></a>Impedir el acceso público para el sitio de prueba

Una consideración importante para la cuenta de prueba es que resultará en vivo en Internet, pero no desea que el público para usarlo. Para mantener la privacidad del sitio puede usar uno o varios de los métodos siguientes:

- Póngase en contacto con el proveedor de hospedaje para establecer las reglas de firewall que permitan el acceso al sitio de prueba solo desde direcciones IP que se use para pruebas.
- Ocultar la dirección URL para que no sea similar a la dirección URL del sitio público.
- Use un *robots.txt* archivo para asegurarse de que los motores de búsqueda no rastreará los vínculos de sitio y el informe de prueba a él en resultados de búsqueda.

El primero de estos métodos es claramente la más segura, pero el procedimiento para el es específico de cada proveedor de hospedaje y no se tratarán en este tutorial. Si organiza con su proveedor de hospedaje para permitir solo la dirección IP ir a la URL de la cuenta de prueba, en teoría no tiene que preocuparse de los motores de búsqueda se rastrean. Pero incluso en ese caso, implementar un *robots.txt* archivo es una buena idea como una copia de seguridad en caso de que dicha regla de firewall accidentalmente alguna vez se ha desactivado.

El *robots.txt* archivo va en la carpeta del proyecto y debe tener el siguiente texto en él:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

El `User-agent` línea indica los motores de búsqueda que se aplican las reglas en el archivo para todos los rastreadores de web de motor de búsqueda (robots), y la `Disallow` línea especifica que no va a rastrear ninguna página en el sitio.

Probablemente le convenga motores de búsqueda en el catálogo del sitio de producción, por lo que necesita excluir este archivo de implementación de producción. Para ello, consulte **¿se puede excluir determinados archivos o carpetas de implementación?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Asegúrese de especificar la exclusión sólo para el perfil de publicación de la producción.

Crear una segunda cuenta de hospedaje es una forma de trabajar con un entorno de prueba que no es necesario, pero puede merecer la pena el gasto adicional. En los siguientes tutoriales aún utilizar IIS como el entorno de prueba.

En el siguiente tutorial, podrá actualizar código de aplicación e implementar el cambio a los entornos de prueba y producción.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
