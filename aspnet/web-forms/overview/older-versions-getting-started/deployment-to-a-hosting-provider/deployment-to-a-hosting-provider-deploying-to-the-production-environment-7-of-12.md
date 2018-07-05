---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en el entorno de producción - 7 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383641"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en el entorno de producción - 7 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

En este tutorial, configurar una cuenta con un proveedor de hospedaje e implementar su ASP.NET característica de publicación de la aplicación web en el entorno de producción mediante el uso de Visual Studio con un solo clic.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Seleccionar un proveedor de hospedaje

Para la aplicación Contoso University y esta serie de tutoriales, necesita un proveedor que admite ASP.NET 4 y Web Deploy. Se ha elegido una empresa de hospedaje específica para que los tutoriales muestran la experiencia completa de la implementación en un sitio Web activo. Cada empresa de hospedaje proporciona características diferentes y la experiencia de implementación en sus servidores varía ligeramente. Sin embargo, el proceso descrito en este tutorial es típico para el proceso general. El proveedor de hospedaje utilizado para este tutorial, Cytanium.com, es uno de ellos que están disponibles y su uso en este tutorial no constituye una aprobación ni recomendación.

Cuando esté listo para seleccionar su propio proveedor de hospedaje, puede comparar las características y precios en la [Galería de proveedores](https://www.microsoft.com/web/hosting) en el sitio Microsoft.com/Web.

## <a name="creating-an-account"></a>Creación de una cuenta

Cree una cuenta en el proveedor seleccionado. Si la compatibilidad con una base de datos completa de SQL Server es un agregado adicional, no es necesario seleccionarlo para este tutorial, pero lo necesitará para el [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial más adelante en esta serie.

Estos tutoriales, no tienes que registrar un nuevo nombre de dominio. Puede probar para comprobar una implementación correcta mediante la dirección URL temporal asignada al sitio por el proveedor.

Una vez creada la cuenta, normalmente reciben un correo electrónico de bienvenida que contiene toda la información que necesita para implementar y administrar su sitio. La información que envía el proveedor de hospedaje será similar a lo que se muestra aquí. El correo de bienvenida Cytanium que se envía a los propietarios de cuenta nueva incluye la información siguiente:

- La dirección URL al sitio de panel de control del proveedor, donde puede administrar la configuración de su sitio. El identificador y la contraseña que especificó se incluyen en esta parte del correo electrónico de bienvenida para facilitar su consulta. (Ambos han cambiado en un valor de demostración para esta ilustración.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versión de .NET Framework de forma predeterminada y la información sobre cómo cambiarla. Muchos hospedaje sitios predeterminada a 2.0, que funciona con aplicaciones ASP.NET que tienen como destino .NET Framework 2.0, 3.0 o 3.5. Sin embargo Contoso University es una aplicación de .NET Framework 4, por lo que debe cambiar esta configuración. (Para una aplicación de ASP.NET 4.5 se usaría la configuración de .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- La dirección URL temporal que puede usar para tener acceso a su sitio web. Cuando se creó esta cuenta, se especificó "contosouniversity.com" como el nombre de dominio existente. Por lo tanto, es la dirección URL temporal `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Información acerca de cómo configurar las bases de datos y las cadenas de conexión que necesita para tener acceso a ellos:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Información sobre herramientas y configuración para la implementación de su sitio. (El correo electrónico desde Cytanium menciona también WebMatrix, que se omite aquí).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Configuración de la versión de .NET Framework

El correo electrónico de bienvenida Cytanium incluye un vínculo para obtener instrucciones sobre cómo cambiar la versión de .NET Framework. Estas instrucciones se explica que esto puede hacerse a través del panel de control Cytanium. Otros proveedores tienen sitios de panel de control que tienen un aspecto distintos, o puede indicar a hacerlo de manera diferente.

Vaya a la dirección URL de panel de control. Después de iniciar sesión con su nombre de usuario y contraseña, consulte el panel de control.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

En el **hospeda espacios** , mantenga el puntero sobre el icono de Web y seleccione **sitios Web** en el menú.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

En el **sitios Web** cuadro, haga clic en **contosouniversity.com** (el nombre del sitio que utilizó cuando creó la cuenta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

En el **propiedades del sitio Web** cuadro, seleccione el **extensiones** ficha.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Cambiar a ASP.NET desde **2.0 canalización integrada** a **4.0 (canalización integrada)** y, a continuación, haga clic en **actualización**.

## <a name="publishing-to-the-hosting-provider"></a>Publicación en el proveedor de hospedaje

Incluye el correo electrónico de bienvenida del proveedor de hospedaje de toda la configuración que necesita para publicar el proyecto, y puede escribir esa información manualmente en un perfil de publicación. Pero, usará una más fácil y menos propensas a errores método para configurar la implementación para el proveedor: se descargará un *.publishsettings* archivo e importarlo en un perfil de publicación.

En el explorador, vaya al panel de control Cytanium y seleccione **Web** y, a continuación, seleccione **sitios Web.**

![Panel de control seleccionando los sitios Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Seleccione el **contosouniversity.com** sitio web.

![Panel de control seleccionando contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Seleccione el **publicación Web** ficha.

![Pestaña publicación de Web del Panel de control](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Cree las credenciales para escribiendo un nombre de usuario y una contraseña de publicación en web. Puede escribir las mismas credenciales que usó para iniciar sesión en el panel de control. A continuación, haga clic en **habilitar**.

![Panel de control crear credenciales de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Haga clic en **descargar perfil de publicación para este sitio web**.

![Control Panel descargar perfil de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Cuando se le pida que abra o guarde el archivo, guárdelo.

![Guarde el archivo de perfil de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

En **el Explorador de soluciones** en Visual Studio, haga clic en el proyecto ContosoUniversity y seleccione **publicar**. El **publicación Web** abre el cuadro de diálogo en el **Preview** pestaña con la **prueba** perfil seleccionado, ya que es el último perfil que usó.

Seleccione el **perfil** pestaña y, a continuación, haga clic en **importación**.

![Botón de importación de Asistente de Web publicar](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

En el **importar la configuración de publicación** cuadro de diálogo, seleccione el *.publishsettings* archivo que descargó y haga clic en **abierto**. El asistente avanza a la ficha conexión con todos los campos cumplimentados.

![Publicar la ficha de conexión del Asistente de Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

El archivo .publishsettings coloca la URL permanente planeada para el sitio en el cuadro de dirección URL de destino, pero si aún no ha adquirido ese dominio, sustituya el valor por la dirección URL temporal. En este ejemplo, la dirección URL es  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Es el único propósito de este cuadro especificar qué dirección URL que el explorador abrirá automáticamente después de correctamente después de la implementación. Si se deja en blanco, la única consecuencia es que el explorador no se inicia automáticamente después de la implementación.

Haga clic en **validar conexión** para comprobar que la configuración es correcta y que puede conectarse al servidor. Como vimos anteriormente, una marca de verificación verde comprueba que la conexión es correcta.

Al hacer clic en validar conexión, puede aparecer un **Error de certificado** cuadro de diálogo. Si lo hace, compruebe que el nombre del servidor es el esperado. Si es así, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Accept**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de adquirir un certificado SSL para la dirección URL que se va a implementar en. Si desea establecer una conexión segura con un certificado válido, póngase en contacto con su proveedor de hospedaje.)

![Error de certificado](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Haga clic en **Siguiente**.

En el **bases de datos** sección de la **configuración** ficha, especifique el mismo perfil de publicación de los valores que especificó para la prueba. Encontrará las cadenas de conexión que necesita en las listas desplegables.

- En el cuadro cadena de conexión para **SchoolContext,** seleccione `Data Source=|DataDirectory|School-Prod.sdf`
- En **SchoolContext**, seleccione **aplicar Code First Migrations**.
- En el cuadro cadena de conexión para **DefaultConnection**, seleccione `Data Source=|DataDirectory|aspnet-Prod.sdf`
- En **DefaultConnection**, deje **Actualizar base de datos** desactivada.

![Pestaña de configuración del Asistente de Web publicar](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Haga clic en **Siguiente**.

En el **Preview** , haga clic **iniciar vista previa** para ver una lista de los archivos que se va a copiar. Verá la misma lista que vio anteriormente cuando se implementa en IIS en el equipo local.

Antes de publicar, cambie el nombre del perfil para que se aplicará el archivo de transformación Web.Production.config. Seleccione el **perfil** ficha y haga clic en **administrar perfiles**.

![Web Asistente para administrar perfiles de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

En el **Editar perfiles de publicación Web** diálogo cuadro, seleccione el perfil de producción, haga clic en **cambiar el nombre**y cambie el nombre del perfil a producción. A continuación, haga clic en **cerrar**.

![Editar cuadro de diálogo perfiles de publicación Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Haga clic en **Publicar**.

La aplicación se publica en el proveedor de hospedaje. El resultado se muestra en el **salida** ventana.

![Ventana de salida después de la implementación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

El explorador se abre automáticamente en la dirección URL que escribió en **dirección URL de destino** cuadro en el **conexión** pestaña de la **publicación Web** asistente. Verá la misma página de inicio como al ejecutar el sitio en Visual Studio, excepto que ahora no hay ninguna "(prueba)" o "(desarrollo)" indicador de entorno en la barra de título. Esto indica que el indicador de entorno *Web.config* transformación funcionó correctamente.

> [!NOTE]
> Si todavía ve "(prueba)" en el encabezado, elimine el *obj* carpeta del proyecto ContosoUniversity y volver a implementar. En las versiones preliminares del software, el archivo de transformación aplicada anteriormente (Web.Test.config) es posible que se aplicarán nuevo aunque use el perfil de producción.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Antes de ejecutar una página que hace que el acceso de la base de datos, asegúrese de que Elmah podrán registrar cualquier error que se produce.

## <a name="setting-folder-permissions-for-elmah"></a>Establecer permisos de carpeta para Elmah

Cuando tenga en cuenta en el tutorial anterior de esta serie, debe asegurarse de que la aplicación tiene permisos de escritura para la carpeta en la aplicación donde Elmah almacena archivos de registro de errores. Cuando implementa en IIS localmente en el equipo, configurar manualmente estos permisos. En esta sección, verá cómo establecer permisos en Cytanium. (Algunos proveedores de hospedaje es posible que no le permiten hacer esto, ofrecen una o varias carpetas predefinidas con permisos de escritura. En ese caso tendría que modificar la aplicación para usar las carpetas especificadas.)

Puede establecer los permisos de carpeta en el panel de control Cytanium. Vaya al control de dirección URL del panel y seleccione **administrador archivos**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

En el **administrador archivos** cuadro, seleccione **contosouniversity.com** y, a continuación, **wwwrooot** para ver la carpeta raíz de la aplicación. Haga clic en el icono de candado junto a **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

En el **archivo**/**permisos de carpeta** ventana, seleccione el **lectura** y **escribir** casillas de verificación de  **contosouniversity.com** y haga clic en **establecer permisos**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Asegúrese de que Elmah tiene acceso de escritura a la *Elmah* carpeta produciendo un error y, a continuación, mostrando el informe de errores Elmah. Solicitar una dirección URL no válida como *Studentsxxx.aspx*. Como antes, verá el *GenericErrorPage.aspx* página. Haga clic en el **cerrar sesión** vincular y, a continuación, ejecute *Elmah.axd*. Obtiene el **en el registro** en primer lugar, la página que valida que el *Web.config* transformación Elmah autorización agregó correctamente. Después de iniciar sesión, vea el informe que muestra el error que provocó el simplemente.

[![ELMAH.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Las pruebas en el entorno de producción

Ejecute el **estudiantes** página. La aplicación intenta tener acceso a la base de datos School por primera vez, lo que desencadena las migraciones de Code First para crear la base de datos. Cuando la página se muestra después de retraso de unos instantes, se muestra que no hay ningún estudiante.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Ejecute el **instructores** página para comprobar que los datos de inicialización insertan correctamente los datos de un instructor en la base de datos.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Al igual que en el entorno de prueba, desea comprobar que las actualizaciones de la base de datos funcionan en el entorno de producción, pero normalmente no desea escribir datos de prueba en la base de datos de producción. Para este tutorial, usará el mismo método que se hizo en la prueba. Pero en una aplicación real que se desea encontrar un método que valida esa base de datos, las actualizaciones tienen éxito sin introducir datos de prueba en la base de datos de producción. En algunas aplicaciones, puede que resulte práctico agregar algo y, a continuación, elimínelo.

Agregar un estudiante y, a continuación, ver los datos especificados en el **estudiantes** página para comprobar que puede actualizar datos en la base de datos.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Validar que las reglas de autorización funciona correctamente mediante la selección **actualización créditos** desde el **cursos** menú. El **en el registro** se muestra la página. Escriba sus credenciales de cuenta de administrador, haga clic en **en el registro**y el **actualización créditos** se muestra la página.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Si el inicio de sesión es correcta, el **actualización créditos** se muestra la página. Esto indica que se ha implementado correctamente la base de datos de pertenencia ASP.NET (con la cuenta de administrador solo).

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Ahora ha implementado y probado su sitio y está disponible públicamente a través de Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Creación de un entorno de prueba más confiable

Como se explica en el [implementarla en el entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial, el entorno de prueba más confiable sería una segunda cuenta en el proveedor de hospedaje que simplemente como la cuenta de producción. Esto podría resultar más costoso que el uso de IIS local como entorno de prueba, ya que tendría que suscribirse a una segunda cuenta de hospedaje. Pero si evita errores de sitio de producción o interrupciones, podría decidir que merece la pena el coste.

La mayor parte del proceso de creación e implementación de una cuenta de prueba es similar a lo que ha hecho ya para implementar en producción:

- Crear un *Web.config* archivo de transformación.
- Cree una cuenta en el proveedor de hospedaje.
- Crear un nuevo perfil de publicación e implementar en la cuenta de prueba.

### <a name="preventing-public-access-to-the-test-site"></a>Impedir el acceso público para el sitio de prueba

Una consideración importante para la cuenta de prueba es que estará activa en Internet, pero no desea que el público para usarlo. Para mantener la privacidad del sitio puede usar uno o varios de los métodos siguientes:

- Póngase en contacto con el proveedor de hospedaje para establecer las reglas de firewall que permitan el acceso al sitio de pruebas solo desde direcciones IP que usan para realizar pruebas.
- Ocultar la dirección URL para que no sea similar a la dirección URL del sitio público.
- Use un *robots.txt* archivo para asegurarse de que los motores de búsqueda no rastreará los vínculos de sitio y el informe de prueba a él en los resultados de búsqueda.

El primero de estos métodos es obviamente el más seguro, pero el procedimiento para el es específico de cada proveedor de hospedaje y no se tratarán en este tutorial. Si organizas con su proveedor de hospedaje para permitir solo la dirección IP ir a la dirección URL de cuenta de prueba, en teoría no tiene que preocuparse por los motores de búsqueda que se rastrean. Pero incluso en ese caso, implementar un *robots.txt* archivo es una buena idea como una copia de seguridad en caso de que esa regla de firewall accidentalmente alguna vez se ha desactivado.

El *robots.txt* archivo se incluye en la carpeta del proyecto y debe tener el siguiente texto en él:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

El `User-agent` línea indica los motores de búsqueda que se aplican las reglas en el archivo a todos los rastreadores de web de motor de búsqueda (robots), y el `Disallow` línea especifica que no se deben rastrear las páginas del sitio.

Probablemente deseará motores de búsqueda en el catálogo de su sitio de producción, por lo que necesita excluir este archivo de implementación de producción. Para ello, consulte **puedo excluir determinados archivos o carpetas de la implementación?** en [p+f sobre la implementación de ASP.NET Web Application Project](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Asegúrese de especificar la exclusión solo para el perfil de publicación de la producción.

Creación de una segunda cuenta de hospedaje es un enfoque para trabajar con un entorno de prueba que no es necesario, pero puede valer la pena el gasto adicional. En los tutoriales siguientes, aún utilizar IIS como entorno de prueba.

En el siguiente tutorial, deberá actualizar el código de la aplicación e implementar el cambio en los entornos de prueba y producción.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
