---
uid: web-pages/readme/overview
title: Archivo Léame de WebMatrix | Microsoft Docs
author: rick-anderson
description: WebMatrix y ASP.NET Web Pages (Razor) versión 1.0 Léame
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837671"
---
<a name="webmatrix-readme"></a>Archivo Léame de WebMatrix
====================
13 de enero de 2011

## <a name="contents"></a>Contenido

> [!NOTE]
> Este archivo Léame se aplica a la 1.0 versión de WebMatrix.


- [Información general](#Overview)
- [Instalación](#Installation_Notes)
- [Cómo publicar aplicaciones](#InstructionsForPublishingApplications)
- [Los cambios y problemas](#ChangesAndIssues)

    - [Instalación de WebMatrix 1.0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalación de aplicaciones](#Known_Issues_Installing_Applications)
    - [Publicación de aplicaciones](#Known_Issues_Publishing_Applications)
- [Para obtener más información](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Información general

> Microsoft WebMatrix 1.0 es una pila de desarrollo web gratuito que se instala en pocos minutos. Se integra un servidor web con la base de datos y marcos para crear una única experiencia integrada de programación. Puede usar WebMatrix para agilizar la manera de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o puede usar WebMatrix para iniciar un nuevo sitio Web con aplicaciones de código abierto populares como DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix usa el mismo servidor web eficaz, el motor de base de datos y el entorno de marcos que se ejecutará el sitio Web en internet, lo que hace la transición de desarrollo a producción homogénea y sin problemas.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalación

> Para instalar WebMatrix 1.0, primero debe instalar el [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Después de instalar al instalador de plataforma Web, puede usarlo para instalar WebMatrix.
> 
> Si tiene problemas durante la instalación, consulte [solución de problemas con el instalador de plataforma Web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Cómo publicar aplicaciones

> Consulte [instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Los cambios y problemas

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemas de instalación de 1.0 de WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1.0 solo está disponible en las plataformas que admiten Microsoft .NET Framework 4

> La versión 4 de .NET Framework es necesaria para WebMatrix. En algunos casos, el programa de instalación de 1.0 WebMatrix le permitirá intenta instalar en una plataforma que no forma parte del conjunto de configuración admitidos. En concreto, Windows Vista sin la actualización SP1 le permiten comenzar la instalación de WebMatrix, pero producirá un error en el componente de .NET Framework 4 y bloquear la instalación.
> 
> **Solución alternativa**  
> Instalar en una plataforma compatible, que incluye:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 o posterior
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: No se puede instalar WebMatrix 1.0 si se instala Microsoft Visual Studio 2008 sin Microsoft Visual Studio 2008 SP1

> **Solución alternativa**  
> Instalar [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) desde el centro de descarga de Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: Algunos ensamblados de SQL Server Compact 4.0 no están instalados en la GAC

> Los ensamblados administrados para SQL Server Compact 4.0 no se colocan en la caché de ensamblados global (GAC) al instalar SQL Server Compact 4.0 en un equipo de 64 bits y el equipo tiene solo .NET Framework 3.5 SP1 Client Profile instalado. Son los ensamblados administrados que no están instalados en la GAC:
> 
> - *System.Data.SqlServerCe.dll* (proveedor de ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Solución alternativa**  
> Desinstalar SQL Server Compact 4.0. Descargue e instale la versión completa de .NET Framework 3.5 SP1 desde la ubicación siguiente:  
>   
> [Paquete de Microsoft .NET Framework 3.5 con Service 1 (paquete completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> A continuación, vuelva a instalar SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: No se puede desinstalar SQL Server Compact usando la línea de comandos

> Desinstalación de SQL Server Compact usando las opciones de línea de comandos no funciona en esta versión.
> 
> **Solución alternativa**  
> Use *programas y características* en el Panel de Control de Windows para desinstalar Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Esta sección del documento describen las nuevas características, cambios y problemas conocidos con la 1.0 versión de ASP.NET Web Pages con sintaxis Razor.

- [Características nuevas](#NewFeatures)
- [Cambios](#Changes)
- [Problemas](#Issues)

#### <a id="NewFeatures"></a>  Nuevas características

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nuevo: Agrega la opción de configuración para deshabilitar al administrador de paquetes

> Un nuevo `asp:AdminManagerEnabled` clave está disponible para el `<appSettings>` elemento en el *web.config* archivo, que le permite deshabilitar completamente el Administrador de paquetes. El valor predeterminado para este elemento es true, lo que significa que si no se incluye en el *web.config* archivo, el Administrador de paquetes está habilitado. Para deshabilitar el Administrador de paquetes, agregue el siguiente elemento a la *web.config* archivo en la raíz del sitio Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Cambios

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Cambio: clave "webPages:AdminFolderVirtualPath" ha cambiado a "asp: AdminFolderVirtualPath"

> El `webPages:AdminFolderVirtualPath` clave que se puede agregar a la *web.config* archivo para especificar la ubicación del Administrador de paquetes se cambió para usar el `asp:` espacio de nombres en lugar de la `webPages` espacio de nombres. Si ha usado este elemento, debe cambiar el nombre del archivo de configuración.


#### <a id="Issues"></a>  Problemas conocidos

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: Contraseñas para usuarios de pertenencia que ya no se reconoce

> El algoritmo para crear y almacenar las contraseñas de pertenencia (inicio de sesión) se ha cambiado para ser más seguro. Como resultado, no se reconocerá las contraseñas almacenadas para los miembros (usuarios) creados en las versiones Beta de ASP.NET Razor. 
> 
> **Solución alternativa** si el sitio aún no se haya puesto en producción, quitar los registros de usuario de la base de datos de pertenencia. Si la base de datos está activa, volver a generar mediante programación las contraseñas existentes en la base de datos de pertenencia.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamiento inesperado cuando se usa una tabla de usuario personalizada para la suscripción

> Para inicializar el proveedor de pertenencia para un sitio Web de ASP.NET Razor, llame a la `WebSecurity.InitializeDatabaseConnection` método. (En WebMatrix, la plantilla de sitio inicial incluye una llamada a este método en el  *\_AppStart.cshtml* archivo.) Si el `autoCreateTables` parámetro de este método está establecido en true (de forma predeterminada, se establece en true en la plantilla de sitio de inicio), y si un nombre de tabla no reconocida se pasa al método (el segundo parámetro), el método no produce un error. En su lugar, crea automáticamente la tabla.
> 
> Esto puede ser un problema si va a usar una tabla de usuario personalizada para la suscripción pero pase el nombre de tabla incorrecto para el `WebSecurity.InitializeDatabaseConnection` método. Dado que el método no predeterminada genera un error si no existe la tabla que se especifique, y porque crea una nueva tabla en su lugar, la aplicación puede parecer a trabajar. Sin embargo, código de aplicación que se basa en la tabla de usuario personalizada (y en los campos de él) puede producir un error con errores inesperados.
> 
> **Solución alternativa**  
> Asegúrese de que pasa el nombre de la `InitializeDatabaseConnection` coincidencias de método, el perfil de usuario de la tabla en la base de datos de pertenencia o asegúrese de que el `autoCreateTables` parámetro se establece en false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problema: Mensaje de Error "el módulo de administración requiere acceso a ~/App\_datos"

> En algunas circunstancias, intentando crear usuarios o trabajar con el sistema de pertenencia ASP.NET puede hacer que la página Mostrar el error *el módulo de administración requiere acceso a ~/App\_datos*. Esto se produce si la cuenta que se está ejecutando en IIS o IIS Express no tiene permisos para crear y escribir en el *aplicación\_datos* carpeta bajo la raíz del sitio Web. 
> 
> **Solución alternativa** crear manualmente un *aplicación\_datos* carpeta para el sitio Web. Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente, el servicio de red) tiene los permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas, como aplicación\_datos. Información más detallada está disponible en el artículo de Knowledge Base [problemas con las instancias de usuario de SQL Server Express y proyectos de aplicación Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "error no se pudo generar una instancia de usuario de SQL Server"

> Si una aplicación WebMatrix Web usa SQL Server Express y se está ejecutando IIS 7.5 en Windows 7 o Windows Server 2008 R2, puede aparecer un error que indica que SQL Server no se puede recuperar la ruta de acceso de aplicación local del usuario en tiempo de ejecución.
> 
> **Solución alternativa** Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente, el servicio de red) tiene los permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas como *aplicación\_datos*. Información más detallada está disponible en el artículo de Knowledge Base [problemas con las instancias de usuario de SQL Server Express y proyectos de aplicación Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: Los archivos que contiene los recursos del Administrador de paquetes o las contraseñas de administrador de paquetes son que se pueden proporcionar en IIS 6.0 y versiones anteriores

> Si implementa una aplicación de ASP.NET Web Pages (Razor) que se generó con la versión RC2, y si la aplicación contiene un *password.txt* o *packagesources.txt* archivo */App\_ Administrador dedatos/*, IIS 6.0 servirá el archivo si se solicita, puede exponer las contraseñas para la instancia del Administrador de paquetes. 
> 
> **Solución alternativa** cambiar el nombre de la *password.txt* o *packagesources.txt* archivo *password.config* o *packagesources.config*. De forma predeterminada, IIS 6.0 no suministrará los archivos que tienen la *.config* extensión. (En IIS 7, no hay archivos en el *aplicación\_datos* se sirven carpeta, por lo que no es necesario cambiar el nombre de los archivos.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: Los paquetes instalados con la versión Beta 3 de desinstalación no quitar completamente los componentes del paquete

> Si instaló un paquete mediante el Administrador de paquetes en la versión Beta 3 y, a continuación, intente desinstalar mediante la versión actual, el paquete no se desinstala completamente. Mediante el Administrador de paquetes **desinstalar** botón quita algunos componentes, pero deje el código de biblioteca del paquete y no actualiza el *package.config* archivo.
> 
> **Solución alternativa**   
> Siga estos pasos:  
> 1. Eliminar el *aplicación\_Data\packages* carpeta. Esto quita todos los paquetes.   
> 2. Eliminar el *packages.config* archivo en la raíz del sitio Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: En Visual Studio, invocar el Administrador de paquetes basada en web toma la aplicación sin conexión

> Si está trabajando en Visual Studio (no WebMatrix) y use la  *\_admin* funcionalidad para iniciar el Administrador de paquetes, Visual Studio se desconecta de la aplicación y registra el *aplicación\_ offline.htm* en la raíz del sitio Web, se interrumpirá su capacidad para usar el Administrador de paquetes.
> 
> [!NOTE]
> Aunque normalmente vería este comportamiento cuando se usa la interfaz del Administrador de paquetes basado en web, se produce el mismo comportamiento si agregar, quitar o modificar los archivos en el *aplicación\_datos* carpeta.
> 
> **Solución alternativa**   
> Para trabajar con paquetes en Visual Studio, utilice la extensión de NuGet en lugar del Administrador de paquetes basada en web. Para obtener información, consulte el [documentación de NuGet](https://docs.microsoft.com/nuget/). Si está trabajando con otros archivos en el *aplicación\_datos* carpeta, considere la posibilidad de mantener los archivos en otra parte, para evitar este problema. Si no es práctico, elimine el *aplicación\_offline.htm* archivo manualmente o espere a que el sitio vuelve a conectarse automáticamente (de forma predeterminada, después de 30 segundos).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense y plantillas de proyectos disponibles sólo en ASP.NET MVC versión 3

> Instalar ASP.NET Web Pages no también instalar tools para Visual Studio, como IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages.
> 
> **Solución alternativa** para usar IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages en Visual Studio, instalar RC de ASP.NET MVC 3 mediante el instalador de plataforma Web o el [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Leer fuentes u otros datos externos a través de un servidor proxy

> Si el servidor que ejecuta el sitio está detrás de un servidor proxy, deberá configurar la información de proxy en el *web.config* archivo para poder leer la información que proviene de fuera de su sitio. Por ejemplo, si usa el `ReCaptcha` auxiliar, la aplicación auxiliar se comunica con el servicio de reCAPTCHA, pero podría estar bloqueada por el servidor proxy. De forma similar, las fuentes que se usan en ASP.NET Web Pages, como la fuente utilizada por el Administrador de paquetes, pueden requerir la configuración de proxy.
> 
> Si experimenta problemas en trabajar con un servicio externo o trabajar con la fuente del paquete, colocar los elementos siguientes en la raíz de la aplicación *web.config* archivo:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Para obtener más información acerca de cómo configurar un servidor proxy, consulte [ &lt;proxy&gt; (configuración de red) del elemento](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio Web de MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Al desinstalar la versión 4 de .NET Framework deshabilita ASP.NET Web Pages con sintaxis Razor

> Si desinstala la versión 4 de .NET Framework y, a continuación, vuelva a instalarlo, se deshabilita ASP.NET Web Pages con sintaxis Razor. Las páginas con la *.cshtml* extensión no se ejecutan correctamente. Páginas Web de ASP.NET, se registra un ensamblado en la raíz de la máquina *web.config* archivo y quitar .NET Framework se elimina el archivo. Volver a instalar .NET Framework instala una nueva versión del archivo de configuración, pero no agrega la referencia del ensamblado de ASP.NET Web Pages.
> 
> **Solución alternativa** después de volver a instalar .NET Framework, reinstale ASP.NET Web Pages con sintaxis Razor. Esto agrega el siguiente elemento a la *web.config* archivo en la raíz de la máquina, que se encuentra normalmente en la siguiente ubicación:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: Direcciones URL sin extensión no encuentra los archivos de.cshtml/.vbhtml en IIS 7 o IIS 7.5

> En IIS 7 o IIS 7.5, las solicitudes con una dirección URL similar a la siguiente no están posible encontrar las páginas que tienen la *.cshtml* o *.vbhtml* extensión:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> El problema surge porque la reescritura de URL no está habilitada de forma predeterminada para IIS 7 o IIS 7.5. El escenario más probable es que no ve el problema al realizar pruebas localmente mediante IIS Express, pero la experiencia al implementar su sitio Web en un sitio Web de hospedaje.
> 
> **Solución alternativa**
> 
> - Si tiene control sobre el equipo del servidor, en el equipo servidor instalar la actualización que se describe en [hay una actualización disponible que habilita ciertos controladores de IIS 7.0 o IIS 7.5 para controlar las solicitudes cuyas direcciones URL no termina con un punto](https://support.microsoft.com/kb/980368).
> - Si no tiene control sobre el equipo del servidor (por ejemplo, va a implementar en un sitio de hospedaje Web), agregue lo siguiente del sitio Web *web.config* archivo: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implementar una aplicación en un equipo que no tiene SQL Server Compact instalado

> Las aplicaciones que incluyen bases de datos de SQL Server Compact pueden ejecutar en un equipo donde SQL Server Compact no está instalado. Microsoft WebMatrix 1.0 automáticamente copia estos archivos binarios para usted y realiza adecuado *web.config* transformaciones de archivos.
> 
> **Solución alternativa** si tiene que copiar estos archivos y realizar el *web.config* cambios en los archivos manualmente, haga lo siguiente:
> 
> 1. Copie los ensamblados del motor de base de datos a la *Bin* carpeta (y las subcarpetas) de la aplicación en el equipo de destino:  
> 
>    - Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>a</em></strong>\Bin\x86*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>a</strong><em>\Bin\amd64</em>
> 
> 2. En la carpeta raíz del sitio Web, cree o abra un *web.config* archivo. (En WebMatrix 1.0, está disponible si hace clic en este tipo de archivo **todas** en el **elegir un tipo de archivo** cuadro de diálogo.)
> 3. Agregue el siguiente elemento como elemento secundario de la `<configuration>` elemento (no está dentro de la `<system.web>` elemento):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: "Database" y "WebGrid" aplicaciones auxiliares no funcionan en el nivel de confianza medio en Visual Basic

> Si está utilizando Visual Basic (crear *.vbhtml* archivos), el `Database` y `WebGrid` aplicaciones auxiliares no funcionará si la aplicación está configurada para usar el nivel de confianza medio.
> 
> **Solución alternativa**  
> Si usa Visual Studio 2010, puede resolver este problema mediante la instalación de la versión del Service Pack 1. Hasta que esté disponible la versión final de la versión SP1, puede descargar la versión Beta de SP1 desde el [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) página en Microsoft Download Center.   
>   
> Si esto no es práctico, o si no usa Visual Studio 2010, temporalmente puede establece la aplicación para usar la plena confianza.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: "ApplicationPart" recursos son accesibles desde el exterior

> Si un ensamblado contiene objetos que se deriva el `ApplicationPart` clase, que se exponen los recursos del ensamblado por el `ResourceRouteHandler` clase. Pongamos como ejemplo esta URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Esta solicitud de descarga todas las cadenas de recursos en el *System.Web.WebPages.Administration.dll* ensamblado. Se descargan todos los recursos incrustados (incluso aquellos que no están diseñados para ser servido como contenido estático). Si los recursos incrustados contienen información confidencial, esto puede representar un riesgo de seguridad. 
> 
> **Solución alternativa**   
> Si creas un **ApplicationPart** , asegúrese de que los recursos incrustados asociada con la que **ApplicationPart** ensamblado del objeto no contienen información confidencial.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Para obtener información acerca de problemas de instalación de WebMatrix, vea [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.


Esta sección del documento describen problemas conocidos para el entorno de desarrollo de WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: No se reflejan los cambios en el nombre de usuario o contraseña de una cadena de conexión de base de datos en un archivo web.config en el área de trabajo de bases de datos

> **Solución alternativa**  
> 
> 1. En el *web.config* de archivo, cambie el nombre de la base de datos en la cadena de conexión (por ejemplo, agregue "1" a él).
> 2. Guardar el *web.config* archivo.
> 3. Haga clic en **bases de datos** y actualizar.
> 4. Cambiar el nombre de la base de datos en la cadena de conexión en el *web.config* archivo por el nombre de base de datos original.
> 5. Guardar el *web.config* archivo.
> 6. Haga clic en **bases de datos** y actualizar.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: No se puede eliminar las carpetas creadas mediante WebMatrix

> Si WebMatrix se ejecuta con permisos elevados (es decir, Introducción al uso de WebMatrix el **ejecutar como administrador** opción en Windows), no se puede eliminar carpetas que se crean mediante WebMatrix mediante el Explorador de Windows.
> 
> **Solución alternativa**  
> Ejecute el Explorador de Windows con permisos elevados. Siga estos pasos:  
> 
> 1. En Windows, haga clic en **iniciar**.
> 2. Escriba "Explorador de Windows" y haga clic en la entrada de **Windows Explorer**.
> 3. Haga clic en **ejecutar como administrador**. A continuación, puede eliminar las carpetas.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1.0 no puede realizar ciertas tareas que requieren la elevación

> WebMatrix 1.0 no puede realizar ciertas tareas que requieren la elevación, como la instalación de componentes adicionales en las situaciones siguientes:
> 
> - En Windows Vista o Windows 7, que ha iniciado sesión con una cuenta que no tiene privilegios administrativos y Control de cuentas de usuario (UAC) está deshabilitada.
> - Está utilizando Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Solución alternativa**  
> Mayoría de las tareas en WebMatrix 1.0 no requiere permisos administrativos. Para aquellos que lo hacen, puede realizar la operación porque un administrador o siga estos pasos:
> 
> - En Windows Vista o Windows 7, habilitar UAC.
> - En Windows XP, agregue el usuario al grupo de seguridad Administradores.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Sitio de la galería Web" está deshabilitada

> El **sitio desde galería Web** opción está deshabilitada si el instalador de plataforma Web 3.0 no está instalado.
> 
> **Solución alternativa**  
> Instalar el [instalador de plataforma Web de Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome no está disponible como una opción de ejecución

> Google Chrome no se muestra en la lista de exploradores en **ejecutar** en el **inicio** ficha.
> 
> **Solución alternativa**  
> Algunas versiones de Google Chrome no se registran correctamente con la característica de programas predeterminados en Windows. Como alternativa, inicie Google Chrome, haga clic en el *control Google Chrome y personalizar* menú, haga clic en *opciones*y, a continuación, haga clic en *Make Google Chrome mi explorador predeterminado*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: El cuadro de diálogo "Foreign Key" no permite escribir una clave principal

> El **Foreign Key** cuadro de diálogo no permite especificar el nombre de clave principal de la tabla de clave principal.
> 
> **Solución alternativa**  
> Esto tiene su porqué. No es necesario que escriba el nombre de la clave principal de la tabla de clave principal.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense no está disponible en WebMatrix para Razor sintaxis, C# o Visual Basic

> IntelliSense se admite en WebMatrix para HTML y CSS. Sin embargo, no está disponible para otros lenguajes. 
> 
> **Solución alternativa**   
> Ninguno.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense para HTML y CSS sugiere los elementos que no son adecuados contextualmente

> IntelliSense para el marcado en WebMatrix es compatible con HTML mediante la [esquema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) y el uso de CSS el [esquema CSS 2.1](http://www.w3.org/TR/CSS2/). Dado que IntelliSense se basa en estos esquemas específicos, determinadas etiquetas, atributos o propiedades podrían sugiere que no son adecuadas para la definición de estilo o la página actual. Para HTML, también puede conducir a sugerencias inesperadas en el contenido que podría interpretarse como XHTML con formato incorrecto (por ejemplo, cuando no se cierran las etiquetas). Este problema podría ser más evidente si el punto de inserción está dentro de una etiqueta incompleta; en ese caso, IntelliSense puede sugerir nuevas etiquetas de apertura u ofrecen otras sugerencias incorrectos. 
> 
> **Solución alternativa**   
> Para HTML, asegúrese de que está trabajando dentro de una página XHTML completa tiene el formato correcto. No hay ninguna solución alternativa para CSS.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: No se invoca IntelliSense mientras escribe

> En ocasiones, no se puede invocar IntelliSense como HTML o CSS se está entrando en el editor. En concreto, esto puede ocurrir cuando el punto de inserción está directamente al lado de otro elemento o al final de un archivo. 
> 
> **Solución alternativa**   
> Asegúrese de que hay espacio en blanco alrededor del punto de inserción y que no es el punto de inserción al final de un archivo. También puede invocar IntelliSense manualmente presionando CTRL+BARRA ESPACIADORA.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: no hay ninguna interfaz de usuario para deshabilitar IntelliSense

> WebMatrix 1.0 se proporciona ninguna interfaz de usuario o de gestos para deshabilitar IntelliSense. 
> 
> **Solución alternativa**   
> Inicie WebMatrix mediante el comando siguiente, que incluye un modificador que deshabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express tiene su propio archivo Léame, que está disponible en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0 x 409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact tiene su propio archivo Léame, que está disponible en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Para obtener información acerca de los problemas que implican la instalación de SQL Server Compact como parte de WebMatrix, consulte [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.

### <a id="Known_Issues_Installing_Applications"></a>  Instalación de aplicaciones

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Instalar una aplicación puede tardar mucho tiempo si la carpeta Mis documentos del usuario se redirige a un recurso compartido de red

> **Solución alternativa**  
> Ninguno. La aplicación puede tardar un rato para instalar, pero se instalará correctamente.


### <a id="Known_Issues_Publishing_Applications"></a>  Publicación de aplicaciones

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "requerido no se puede obtener permisos" error al publicar una base de datos SQL

> WebMatrix admite totalmente la implementación archivos binarios compatibles para SQL Server Compact a un servidor que se está ejecutando la versión 3.5 de .NET Framework con una configuración de nivel de confianza medio.
> 
> **Solución alternativa**  
> Es la solución preferida instalar .NET Framework 4 en el servidor. Como alternativa, haga lo siguiente:
> 
> 1. Agregue los siguientes elementos para el `SecurityClasses` sección *Web\_MediumTrust.config* archivo:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Crear un nuevo conjunto de permisos en el *Web\_MediumTrust.config* archivo con los siguientes permisos necesarios:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Se aplica el permiso establecido en SQL Server Compact colocando los siguientes elementos el *Web\_MediumTrust.config* archivo:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: Las aplicaciones web de galería y PhpBB muestran un error "Servicio no está disponible" después de la publicación

> En algunas circunstancias, publicar una aplicación, produce un error "servicio no está disponible".
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal inversa (\) al final del nombre del servidor en el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: Vínculos y el diseño del sitio Web de Moodle se interrumpen después de la publicación

> Después de publicar una aplicación de Moodle, la aplicación no funciona correctamente.
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal (/) al final de la **nombre del sitio** campo el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: Publicación de nopCommerce se produce un error de base de datos

> Publicación nopCommerce se produce un error y notifica un error de base de datos, como "insertar en la instrucción nop\_tabla del registro de error."
> 
> **Solución alternativa**  
> 
> 1. En WebMatrix, haga clic en **ejecutar** para iniciar nopCommerce localmente.
> 2. Inicie sesión en la página de administración.
> 3. Haga clic en el **sistema** menú.
> 4. Haga clic en el **registro** opción.
> 5. Haga clic en el **Vaciar registro** botón.
> 6. Vuelva a publicar nopCommerce.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: Silverstripe CMS muestra un "Error FCGI PHP HTTP 500" al descargar un sitio publicado

> **Solución alternativa**  
> Tras hacer clic en **Descargar sitio publicado**, omitir `silverstripe-cache/manifest_main` en **vista previa de publicación**. Este archivo se usa para fines de almacenamiento en caché y es específico de cada equipo.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: Subtext muestra "Error de servidor en la aplicación '/'" cuando se descarga un sitio publicado

> **Solución alternativa**  
> Abra la carpeta del sitio *web.config* de archivo y reemplace el identificador de usuario y contraseña en la cadena de conexión de base de datos con las credenciales de administrador de SQL Server (las credenciales "sa").
> 
> Como alternativa, siga estos pasos para conceder a la cuenta de usuario que ha iniciado sesión con `db_owner` permisos:
> 
> 1. Instalar SQL Server Management Studio mediante el instalador de plataforma Web.
> 2. Conéctese a la instancia local de SQL Server Express (de forma predeterminada, `.\SQLEXPRESS`).
> 3. Haga clic en **bases de datos** &gt; *[localSubtextDatabase]* &gt; **seguridad** &gt; **usuarios** &gt; *[localSubtextUser*] (valor predeterminado es `subtextuser`], con el botón secundario y haga clic en **propiedades**.
> 4. Seleccione **db\_propietario** en la sección de la pertenencia al rol.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: Sitio podría no funcionar después de publicar si el campo "Dirección URL de destino" no es el prefijo http:// o https://

> En el **configuración de publicación** cuadro de diálogo, si la dirección URL de destino no comienza con `http://` o `https://`, el sitio no funcionen después de la implementación.
> 
> **Solución alternativa**  
> Asegúrese de que antes de publicar un sitio, la dirección URL de destino en el **configuración de publicación** inicia el cuadro de diálogo con `http://` o `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: Publicar una base de datos MySQL produce el error "no se pudo publicar la base de datos. Esto puede ocurrir si la base de datos remoto no puede ejecutar la secuencia de comandos."

> El error puede producirse por una serie de motivos. Una razón que ve este error es si la secuencia de comandos de base de datos contiene un carácter de comillas simples (') y el juego de caracteres de destino MySQL la base de datos predeterminado no es UTF-8.
> 
> **Solución alternativa**  
> Establece el carácter predeterminado establecido para la base de datos MySQL remota en UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: Algunos vínculos no están visibles en DotNetNuke tras publicar o descargar del sitio

> Si publicar o descargar un sitio de DotNetNuke, es posible que deba borrar la memoria caché para obtener los nuevos vínculos que aparecen en el sitio.
> 
> **Solución alternativa**
> 
> 1. Inicie sesión como "Host".
> 2. Vaya al menú de host y seleccione **configuración de Host**.
> 3. Desplácese hacia abajo y en **configuración avanzada**, expanda **configuración de rendimiento**.
> 4. Haga clic en el **Borrar caché** vínculo para las páginas.
> 5. Vaya a la parte inferior de la página y reiniciar la aplicación.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: Algunos vínculos de AtomSite se interrumpen después de descargar un sitio publicado

> **Solución alternativa**  
> En el *service.config* archivo, *users.config* archivo y todos los *.xml* archivos, reemplace la cadena de dirección URL (por ejemplo, `http://myhost.com/atomsite`) con locales (por ejemplo, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: Aplicaciones basadas en MySQL como WordPress no se pueden publicar y notificar un error de base de datos

> De forma predeterminada, WebMatrix instala MySQL con el juego de caracteres UTF-8. Si instala MySQL en su propio y el juego de caracteres no es UTF-8 (por ejemplo, es Latin1), podría producir un error en el proceso de publicación para las bases de datos.
> 
> **Solución alternativa**
> 
> 1. Cambiar el juego de caracteres de MySQL a UTF-8. (Para obtener más información, consulte [intercalación y juego de caracteres de servidor](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) en el sitio Web de MySQL.)
> 2. Vuelva a instalar la aplicación.
> 3. Vuelva a publicar la aplicación.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "Sitio publicado de descarga" se produce un error para las aplicaciones que tienen el programa de instalación basada en explorador

> Algunas aplicaciones (por ejemplo, Kentico CMS) requieren que se inicie en el explorador con el fin de realizar la configuración posterior a la instalación como la creación de una base de datos. Si publica una aplicación similar al siguiente sin completar la instalación basada en explorador, intenta descargar el mismo sitio desde un servidor remoto generará un error.
> 
> **Solución alternativa**  
> Finalizar la instalación basada en explorador antes de publicar el sitio.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "Sitio publicado de descarga" se produce un error de base de datos de DotNetNuke y Kooboo CMS

> Si se intenta descargar una aplicación de un servidor y tener credenciales de administrador en la cadena de conexión de base de datos en el **configuración de publicación** cuadro de diálogo, es posible que vea el siguiente error en el registro de la publicación:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solución alternativa**  
> Si es posible, vuelva a publicar el sitio (o publicarlo) con las credenciales sin privilegios de administrador para la base de datos.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obtener más información

Para obtener más información acerca de WebMatrix 1.0, consulte los siguientes sitios Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Reservados todos los derechos. [Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).
