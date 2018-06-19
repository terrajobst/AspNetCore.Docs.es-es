---
uid: web-pages/readme/overview
title: Archivo Léame de WebMatrix | Documentos de Microsoft
author: rick-anderson
description: WebMatrix y ASP.NET Web Pages (Razor) versión 1.0 Léame
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30898975"
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
- [Problemas y cambios](#ChangesAndIssues)

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

> Microsoft WebMatrix 1.0 es una pila de desarrollo web gratuita que se instala en pocos minutos. Se integra un servidor web con la base de datos y marcos de trabajo para crear una única experiencia integrada de programación. Puede usar WebMatrix para agilizar la manera de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o puede usar WebMatrix para iniciar un nuevo sitio Web mediante aplicaciones populares de código abierto como DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix usa el mismo servidor web eficaz, el motor de base de datos y el entorno de marcos de trabajo que se ejecutará el sitio Web en internet, lo que hace la transición de desarrollo a producción ágil y sin inconvenientes.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalación

> Para instalar WebMatrix 1.0, primero debe instalar la [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Después de instalar al instalador de plataforma Web, se puede utilizar para instalar WebMatrix.
> 
> Si tiene problemas durante la instalación, consulte [solución de problemas con el instalador de plataforma Web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Cómo publicar aplicaciones

> Vea [instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Problemas y cambios

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemas de instalación de 1.0 de WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1.0 está disponible solo en plataformas compatibles con Microsoft .NET Framework 4

> La versión 4 de .NET Framework es necesaria para WebMatrix. En algunos casos, el programa de instalación de 1.0 de WebMatrix le permitirá intente instalar en una plataforma que no forma parte del conjunto de configuración admitidos. En concreto, Windows Vista sin la actualización de SP1 le permiten comenzar la instalación de WebMatrix, pero producirá un error en el componente de .NET Framework 4 y bloquear la instalación.
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

> Los ensamblados administrados para SQL Server Compact 4.0 no se colocan en la caché de ensamblados global (GAC) al instalar SQL Server Compact 4.0 en un equipo de 64 bits y el equipo tiene solo el .NET Framework 3.5 SP1 Client Profile instalado. Son los ensamblados administrados que no están instalados en la GAC:
> 
> - *System.Data.SqlServerCe.dll* (proveedor de ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Solución alternativa**  
> Desinstalar SQL Server Compact 4.0. Descargue e instale la versión completa de .NET Framework 3.5 SP1 desde la siguiente ubicación:  
>   
> [Microsoft .NET Framework 3.5 con Service Pack1 (paquete completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> A continuación, vuelva a instalar SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: No se puede desinstalar SQL Server Compact usando la línea de comandos

> Desinstalación de SQL Server Compact usando opciones de línea de comandos no funciona en esta versión.
> 
> **Solución alternativa**  
> Use *programas y características* en el Panel de Control de Windows para desinstalar Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Esta sección del documento describe nuevas características, los cambios y los problemas conocidos con la 1.0 versión de ASP.NET Web Pages con sintaxis Razor.

- [Nuevas características](#NewFeatures)
- [Cambios](#Changes)
- [Problemas](#Issues)

#### <a id="NewFeatures"></a>  Nuevas características

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nuevo: Agrega la opción de configuración para deshabilitar al administrador de paquetes

> Un nuevo `asp:AdminManagerEnabled` clave está disponible para el `<appSettings>` elemento en el *web.config* archivo, que le permite deshabilitar por completo el Administrador de paquetes. El valor predeterminado de este elemento es true, lo que significa que si no se incluye en el *web.config* archivo, el Administrador de paquetes está habilitado. Para deshabilitar el Administrador de paquetes, agregue el siguiente elemento en el *web.config* archivo en la raíz del sitio Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Cambios

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Cambiar: la clave de "webPages:AdminFolderVirtualPath" cambiada a "asp: AdminFolderVirtualPath"

> El `webPages:AdminFolderVirtualPath` clave que puede agregarse a la *web.config* archivo para especificar la ubicación del Administrador de paquetes se cambió para usar el `asp:` espacio de nombres en lugar de la `webPages` espacio de nombres. Si ha usado este elemento, debe cambiar el nombre del archivo de configuración.


#### <a id="Issues"></a>  Problemas conocidos

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: Contraseñas para los usuarios de pertenencia que ya no se reconoce

> El algoritmo para crear y almacenar contraseñas de pertenencia (inicio de sesión) se cambió para que sea más seguro. Como resultado, no se reconocerá las contraseñas almacenadas para los miembros (usuarios) creados en las versiones Beta de ASP.NET Razor. 
> 
> **Solución alternativa** si el sitio aún no se han colocado en producción, quite los registros de usuario de la base de datos de pertenencia. Si activa la base de datos, volver a generar mediante programación las contraseñas existentes en la base de datos de pertenencia.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamiento inesperado cuando se usa una tabla de usuario personalizada para la suscripción

> Para inicializar el proveedor de pertenencia para un sitio Web de ASP.NET Razor, se llama a la `WebSecurity.InitializeDatabaseConnection` método. (En WebMatrix, la plantilla de sitio de inicio incluye una llamada a este método en el  *\_AppStart.cshtml* archivo.) Si el `autoCreateTables` parámetro de este método se establece en true (de forma predeterminada, se establece en true en la plantilla de sitio de inicio), y si se pasa un nombre de tabla no reconocida para el método (el segundo parámetro), el método no produce un error. En su lugar, crea automáticamente en la tabla.
> 
> Esto puede ser un problema si va a utilizar una tabla de usuario personalizada para la pertenencia a pero pase el nombre de tabla incorrecto para el `WebSecurity.InitializeDatabaseConnection` método. Dado que el método no predeterminada genera un error si no existe en la tabla que especifique, y porque en su lugar, crea una nueva tabla, la aplicación puede parecer que va a trabajar. Sin embargo, código de aplicación que se basa en una tabla de usuario personalizada (y en los campos de él) puede producir un error con errores inesperados.
> 
> **Solución alternativa**  
> Asegúrese de que el nombre se pasa en el `InitializeDatabaseConnection` coincidencias de método, el perfil de usuario de la tabla en la base de datos de pertenencia, o asegúrese de que el `autoCreateTables` parámetro se establece en false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problema: Mensaje de Error "el módulo de administración requiere acceso a ~/App\_datos"

> En algunas circunstancias, intenta crear usuarios o en caso contrario, trabajar con el sistema de pertenencia ASP.NET puede dar lugar a la página para mostrar el error *el módulo de administración requiere acceso a ~/App\_datos*. Esto ocurre si la cuenta que se está ejecutando IIS o IIS Express no tiene permisos para crear y escribir en el *aplicación\_datos* carpeta bajo la raíz del sitio Web. 
> 
> **Solución alternativa** crear manualmente un *aplicación\_datos* carpeta para el sitio Web. Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente el servicio de red) tiene permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas, como aplicación\_datos. Hay información más detallada en el artículo de Knowledge Base [problemas con instancias de usuario de SQL Server Express y proyectos de aplicación Web de ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "error no se pudo generar una instancia de usuario de SQL Server"

> Si una aplicación WebMatrix Web usa SQL Server Express y ejecuta IIS 7.5 en Windows 7 o Windows Server 2008 R2, verá un error que indica que SQL Server no se puede recuperar la ruta de acceso de aplicación local del usuario en tiempo de ejecución.
> 
> **Solución alternativa** Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente el servicio de red) tiene permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas como *aplicación\_datos*. Hay información más detallada en el artículo de Knowledge Base [problemas con instancias de usuario de SQL Server Express y proyectos de aplicación Web de ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: Los archivos que contiene recursos del Administrador de paquetes o las contraseñas de administrador de paquetes son servable en IIS 6.0 y versiones anteriores

> Si implementa una aplicación de ASP.NET Web Pages (Razor) que se compiló con la versión de RC2, y si la aplicación contiene un *password.txt* o *packagesources.txt* de archivos en   */App\_ Datos/admin*, IIS 6.0 se usará el archivo si se solicita, puede exponer las contraseñas para la instancia del Administrador de paquetes. 
> 
> **Solución alternativa** cambiar el nombre de la *password.txt* o *packagesources.txt* del archivo a *password.config* o *packagesources.config*. De forma predeterminada, IIS 6.0 no sirve a archivos que tienen la *.config* extensión. (En IIS 7, no hay archivos en el *aplicación\_datos* carpeta se atienden, por lo que no es necesario cambiar el nombre de los archivos.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: Desinstalación de paquetes instalados con la versión Beta 3 no quitar completamente los componentes de paquete

> Si instaló un paquete mediante el Administrador de paquetes en la versión Beta 3 y, a continuación, intentar desinstalar mediante la versión actual, el paquete no se desinstaló totalmente. Mediante el Administrador de paquetes **desinstalar** botón quita algunos componentes, pero deje el código de biblioteca del paquete y no actualiza el *package.config* archivo.
> 
> **Solución alternativa**   
> Siga estos pasos:  
> 1. Eliminar el *aplicación\_Data\packages* carpeta. Esto quita todos los paquetes.   
> 2. Eliminar el *packages.config* archivo en la raíz del sitio Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: En Visual Studio, invocar el Administrador de paquetes basados en web requiere la aplicación sin conexión

> Si está trabajando en Visual Studio (no WebMatrix) y use la  *\_administración* funcionalidad para iniciar el Administrador de paquetes, Visual Studio deja sin conexión la aplicación y registra el *aplicación\_ offline.htm* en la raíz del sitio Web, que interrumpen su capacidad para usar el Administrador de paquetes.
> 
> [!NOTE]
> Aunque normalmente verá este comportamiento cuando se usa la interfaz del Administrador de paquetes basada en web, el mismo comportamiento se produce si agregar, quitar o modificar los archivos de la *aplicación\_datos* carpeta.
> 
> **Solución alternativa**   
> Para trabajar con paquetes en Visual Studio, utilice la extensión de NuGet en lugar del Administrador de paquetes basados en web. Para obtener información, consulte el [documentación de NuGet](https://docs.microsoft.com/nuget/). Si está trabajando con otros archivos en el *aplicación\_datos* carpeta, considere la posibilidad de mantener los archivos en otro lugar para evitar este problema. Si esto no es práctico, elimine la *aplicación\_offline.htm* archivo manualmente o espere a que el sitio vuelve a conectarse automáticamente (de forma predeterminada, después de 30 segundos).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense y proyecto de plantillas disponibles solo en ASP.NET MVC versión 3

> Instalar ASP.NET Web Pages no instalar herramientas para Visual Studio como IntelliSense y proyecto de plantillas para aplicaciones de ASP.NET Web Pages.
> 
> **Solución alternativa** para usar las plantillas de proyecto y de IntelliSense para las aplicaciones de ASP.NET Web Pages en Visual Studio, instale ASP.NET MVC 3 RC ya sea mediante el instalador de plataforma Web o la [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Leer fuentes u otros datos externos a través de un servidor proxy

> Si el servidor que ejecuta el sitio está detrás de un servidor proxy, tendrá que configurar la información de proxy en el *web.config* archivo para poder leer la información que procede de fuera de su sitio. Por ejemplo, si usa el `ReCaptcha` auxiliar, el Ayudante se comunica con el servicio de reCAPTCHA, pero puede estar bloqueado por el servidor proxy. De forma similar, las fuentes que se utilizan en ASP.NET Web Pages, como la fuente utilizada por el Administrador de paquetes, podrían requerir configuración de proxy.
> 
> Si tiene problemas de trabajar con un servicio externo o para trabajar con el paquete de fuente, coloque los elementos siguientes en la raíz de la aplicación *web.config* archivo:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Para obtener más información acerca de cómo configurar un servidor proxy, consulte [ &lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio Web de MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Desinstalar .NET Framework versión 4 deshabilita ASP.NET Web Pages con sintaxis Razor

> Si desinstala la versión 4 de .NET Framework y, a continuación, vuelva a instalarlo, se deshabilita ASP.NET Web Pages con sintaxis Razor. Páginas con la *.cshtml* extensión no se ejecutan correctamente. Las páginas Web ASP.NET registra un ensamblado en la raíz de la máquina *web.config* archivo y quitar .NET Framework se elimina el archivo. Volver a instalar .NET Framework instala una nueva versión del archivo de configuración, pero no agrega la referencia del ensamblado de ASP.NET Web Pages.
> 
> **Solución alternativa** después de reinstalar .NET Framework, vuelva a instalar ASP.NET Web Pages con sintaxis Razor. Esto agrega el siguiente elemento a la *web.config* archivo en la raíz de la máquina, que se encuentra normalmente en la siguiente ubicación:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: Direcciones URL sin extensión no encuentran archivos.cshtml/.vbhtml en IIS 7 o IIS 7.5

> En IIS 7 o IIS 7.5, las solicitudes con una dirección URL similar a la siguiente no están posible encontrar las páginas que tienen la *.cshtml* o *.vbhtml* extensión:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> El problema se produce porque la reescritura de direcciones URL no está habilitada de forma predeterminada para IIS 7 o IIS 7.5. El escenario más probable es que no se ve el problema al probar localmente mediante IIS Express, pero experimenta al implementar el sitio Web en un sitio Web de hospedaje.
> 
> **Solución alternativa**
> 
> - Si tiene control sobre el equipo del servidor, en el equipo servidor instale la actualización que se describe en [hay una actualización disponible que permite determinados controladores de IIS 7.0 o IIS 7.5 para controlar las solicitudes de direcciones URL no termina con un período](https://support.microsoft.com/kb/980368).
> - Si no tiene control sobre el equipo del servidor (por ejemplo, va a implementar en un sitio Web de hospedaje), agregue lo siguiente del sitio Web *web.config* archivo: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implementar una aplicación en un equipo que no tiene SQL Server Compact instalado

> Las aplicaciones que incluyen bases de datos de SQL Server Compact se pueden ejecutar en un equipo donde SQL Server Compact no está instalado. Microsoft WebMatrix 1.0 automáticamente copia estos archivos binarios para usted y realiza la correspondiente *web.config* transformaciones de archivos.
> 
> **Solución alternativa** si necesita copiar estos archivos y realizar la *web.config* cambios en los archivos manualmente, haga lo siguiente:
> 
> 1. Copie los ensamblados de motor de base de datos a la *Bin* carpeta (y sus subcarpetas) de la aplicación en el equipo de destino:  
> 
>    - Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>a</em></strong>\Bin\x86*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>a</strong><em>\Bin\amd64</em>
> 
> 2. En la carpeta raíz del sitio Web, cree o abra un *web.config* archivo. (En WebMatrix 1.0, está disponible si hace clic en este tipo de archivo **todos los** en el **elegir un tipo de archivo** cuadro de diálogo.)
> 3. Agregue el siguiente elemento como elemento secundario de la `<configuration>` elemento (no en el `<system.web>` elemento):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: "Base de datos" y "WebGrid" aplicaciones auxiliares no funcionan en el nivel de confianza medio en Visual Basic

> Si está utilizando Visual Basic (crear *vbhtml* archivos), el `Database` y `WebGrid` aplicaciones auxiliares no funcionará si la aplicación está configurada para usar el nivel de confianza medio.
> 
> **Solución alternativa**  
> Si usa Visual Studio 2010, puede resolver este problema mediante la instalación de la versión de Service Pack 1. Hasta que esté disponible la versión final de la versión de SP1, puede descargar la versión Beta de SP1 desde el [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) página en Microsoft Download Center.   
>   
> Si esto no es práctico, o si no usa Visual Studio 2010, puede establece la aplicación para utilizar plena confianza.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: "ApplicationPart" recursos son accesibles desde el exterior

> Si un ensamblado contiene objetos que se deriva de la `ApplicationPart` de la clase, que se exponen los recursos del ensamblado por el `ResourceRouteHandler` clase. Pongamos como ejemplo esta URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Esta solicitud de descarga todas las cadenas de recursos en el *System.Web.WebPages.Administration.dll* ensamblado. Se descargan todos los recursos incrustados (incluso aquellos que no están diseñados para ser sirve como contenido estático). Si los recursos incrustados contienen información confidencial, esto puede representar un riesgo de seguridad. 
> 
> **Solución alternativa**   
> Si crea un **ApplicationPart** de objetos, asegúrese de que los recursos incrustados asociados a ese **ApplicationPart** ensamblado del objeto no contienen información confidencial.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Para obtener información acerca de los problemas de instalación de WebMatrix, consulte [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.


Esta sección del documento describen los problemas conocidos para el entorno de desarrollo de WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: No se reflejan los cambios en el nombre de usuario o la contraseña de una cadena de conexión de base de datos en un archivo web.config en el área de trabajo de las bases de datos

> **Solución alternativa**  
> 
> 1. En el *web.config* de archivo, cambie el nombre de la base de datos en la cadena de conexión (por ejemplo, agregar "1" a él).
> 2. Guardar el *web.config* archivo.
> 3. Haga clic en **bases de datos** y actualizar.
> 4. Cambiar el nombre de la base de datos en la cadena de conexión en el *web.config* archivo por el nombre de base de datos original.
> 5. Guardar el *web.config* archivo.
> 6. Haga clic en **bases de datos** y actualizar.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: No se puede eliminar carpetas que creó WebMatrix

> Si WebMatrix se ejecuta con permisos elevados (es decir, ha empezado a usar WebMatrix el **ejecutar como administrador** opción en Windows), no se puede eliminar las carpetas que se crean mediante WebMatrix mediante el Explorador de Windows.
> 
> **Solución alternativa**  
> Ejecute el Explorador de Windows con permisos elevados. Siga estos pasos:  
> 
> 1. En Windows, haga clic en **iniciar**.
> 2. Escriba "Explorador de Windows" y haga clic en la entrada de **el Explorador de Windows**.
> 3. Haga clic en **ejecutar como administrador**. A continuación, puede eliminar las carpetas.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1.0 es no se puede realizar ciertas tareas que requieren elevación

> WebMatrix 1.0 es no se puede realizar ciertas tareas que requieren elevación, tales como instalar componentes adicionales en las situaciones siguientes:
> 
> - En Windows Vista o Windows 7, está iniciando sesión con una cuenta que no tiene privilegios administrativos y Control de cuentas de usuario (UAC) está deshabilitado.
> - Está utilizando Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Solución alternativa**  
> Mayoría de las tareas en WebMatrix 1.0 no requiere permisos administrativos. Para aquellos que lo hacen, puede realizar la operación porque un administrador o siga estos pasos:
> 
> - En Windows Vista o Windows 7, habilitar UAC.
> - En Windows XP, agregue el usuario al grupo de seguridad Administradores.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: El "Sitio de la galería Web" está deshabilitada

> El **sitio desde galería Web** opción está deshabilitada si el instalador de plataforma Web 3.0 no está instalado.
> 
> **Solución alternativa**  
> Instalar el [instalador de plataforma Web de Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome no está disponible como una opción de ejecución

> Google Chrome no aparece en la lista de exploradores en **ejecutar** en el **inicio** ficha.
> 
> **Solución alternativa**  
> Algunas versiones de Google Chrome no se registran correctamente con la característica de programas predeterminados de Windows. Como alternativa, inicie Google Chrome, haga clic en el *control Google Chrome y personalizar* menú, haga clic en *opciones*y, a continuación, haga clic en *Asegúrese de Google Chrome mi explorador predeterminado*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: El cuadro de diálogo "Foreign Key" no permite especificar una clave principal

> El **Foreign Key** cuadro de diálogo no permite especificar el nombre de clave principal de la tabla de clave principal.
> 
> **Solución alternativa**  
> Esto tiene su porqué. No es necesario que escriba el nombre de la clave principal de la tabla de clave principal.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense no está disponible en WebMatrix para Razor sintaxis, C# o Visual Basic

> IntelliSense se admite en WebMatrix para HTML y CSS. Sin embargo, no está disponible para otros idiomas. 
> 
> **Solución alternativa**   
> Ninguno.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense para HTML y CSS sugiere elementos que no son adecuados como

> IntelliSense para el marcado en WebMatrix es compatible con HTML utilizando el [esquema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) y el uso de CSS el [esquema CSS 2.1](http://www.w3.org/TR/CSS2/). Dado que IntelliSense se basa en estos esquemas específicos, determinadas etiquetas, atributos o propiedades podrían sugiere que no son adecuadas para la definición de estilo o la página actual. Para HTML, también se pueden producir en sugerencias inesperados en el contenido que se puede interpretar como XHTML con formato incorrecto (por ejemplo, cuando no se cierran las etiquetas). Este problema podría ser más evidente si el punto de inserción está dentro de una etiqueta incompleta; en ese caso, IntelliSense puede sugerir nuevas etiquetas de apertura u ofrecen otras sugerencias incorrectas. 
> 
> **Solución alternativa**   
> Para HTML, asegúrese de que está trabajando en una página XHTML con formato correcto, completa. CSS, no hay ninguna solución alternativa.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: No se invoca IntelliSense mientras se escribe

> En ocasiones, IntelliSense no puede invocar como HTML o CSS que se va a se escribe en el editor. En concreto, esto puede ocurrir cuando el punto de inserción está directamente al lado de otro elemento o al final de un archivo. 
> 
> **Solución alternativa**   
> Asegúrese de que hay espacio en blanco alrededor del punto de inserción y que el punto de inserción no está al final de un archivo. También puede invocar IntelliSense manualmente presionando Ctrl + barra espaciadora.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: Ninguna interfaz de usuario está disponible para deshabilitar IntelliSense

> WebMatrix 1.0 se proporciona ninguna interfaz de usuario o de gestos para deshabilitar IntelliSense. 
> 
> **Solución alternativa**   
> Iniciar WebMatrix mediante el comando siguiente, que incluye un modificador que deshabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express tiene su propio archivo Léame, que está disponible en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact tiene su propio archivo Léame, que está disponible en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Para obtener información acerca de los problemas que implican la instalación de SQL Server Compact como parte de WebMatrix, consulte [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.

### <a id="Known_Issues_Installing_Applications"></a>  Instalación de aplicaciones

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Instalar una aplicación puede tardar mucho tiempo si se redirige la carpeta Mis documentos del usuario a un recurso compartido de red

> **Solución alternativa**  
> Ninguno. La aplicación puede tardar bastante tiempo en instalar, pero se instalará correctamente.


### <a id="Known_Issues_Publishing_Applications"></a>  Publicación de aplicaciones

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "requerido no se puede obtener permisos" error al publicar una base de datos de SQL Compact

> WebMatrix no admite totalmente la implementación auxiliares de los archivos binarios de SQL Server Compact a un servidor que está ejecutando la versión 3.5 de .NET Framework con una configuración de nivel de confianza medio.
> 
> **Solución alternativa**  
> Es la solución preferida instalar .NET Framework 4 en el servidor. Como alternativa, haga lo siguiente:
> 
> 1. Agregue los siguientes elementos para la `SecurityClasses` sección *Web\_MediumTrust.config* archivo:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Crear un nuevo conjunto de permisos en el *Web\_MediumTrust.config* archivo con los permisos siguientes:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Aplicar el permiso establecido en SQL Server Compact colocando los siguientes elementos el *Web\_MediumTrust.config* archivo:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: Las aplicaciones web de galería y PhpBB mostrarán un error "El servicio no está disponible" después de publicar

> En algunas circunstancias, al publicar una aplicación produce un error "el servicio no está disponible".
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal inversa (\) al final del nombre del servidor en el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: Vínculos y el diseño del sitio Web de Moodle se interrumpen después de publicar

> Después de publicar una aplicación Moodle, la aplicación no funcione correctamente.
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal (/) al final de la **nombre del sitio** campo el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: Publicación nopCommerce se produce un error de base de datos

> Publicación nopCommerce se produce un error y notifica un error de base de datos como "insertar en la instrucción nop\_tabla del registro de error."
> 
> **Solución alternativa**  
> 
> 1. En WebMatrix, haga clic en **ejecutar** para iniciar nopCommerce localmente.
> 2. Inicie sesión en la página de administración.
> 3. Haga clic en el **System** menú.
> 4. Haga clic en el **registro** opción.
> 5. Haga clic en el **Vaciar registro** botón.
> 6. Vuelva a publicar nopCommerce.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: Silverstripe CMS muestra un "Error FCGI PHP HTTP 500" al descargar un sitio publicado

> **Solución alternativa**  
> Tras hacer clic en **descarga publicado sitio**, omitir `silverstripe-cache/manifest_main` en **vista previa de publicación**. Este archivo se usa para el almacenamiento en caché y es específico de cada equipo.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: Subtext muestra "Error de servidor en la aplicación '/'" cuando se descarga un sitio publicado

> **Solución alternativa**  
> Abra el sitio *web.config* archivo y reemplace el Id. de usuario y la contraseña en la cadena de conexión de base de datos con las credenciales de administrador de SQL Server (las credenciales "sa").
> 
> Como alternativa, siga estos pasos para conceder a la cuenta de usuario que ha iniciado sesión con `db_owner` permisos:
> 
> 1. Instalar SQL Server Management Studio con el instalador de plataforma Web.
> 2. Conéctese a la instancia local de SQL Server Express (de forma predeterminada, `.\SQLEXPRESS`).
> 3. Haga clic en **bases de datos** &gt; *[localSubtextDatabase]* &gt; **seguridad** &gt; **usuarios** &gt; *[localSubtextUser*] (valor predeterminado es `subtextuser`], menú contextual y haga clic en **propiedades**.
> 4. Seleccione **db\_propietario** en la sección de la pertenencia al rol.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: Sitio no funcionen después de publicar si el campo "Dirección URL de destino" no es el prefijo http:// o https://

> En el **configuración de publicación** cuadro de diálogo, si la dirección URL de destino no comienza con `http://` o `https://`, el sitio no funcionen después de la implementación.
> 
> **Solución alternativa**  
> Asegúrese de que antes de publicar un sitio, la dirección URL de destino en la **configuración de publicación** inicia el cuadro de diálogo con `http://` o `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: Publicar una base de datos de MySQL produce el error "no se pudo publicar la base de datos. Esto puede ocurrir si la base de datos remota no puede ejecutar la secuencia de comandos."

> El error puede producirse por una serie de motivos. Un motivo que puede ver este error es si la secuencia de comandos de base de datos contiene un carácter de comillas simples (') y juego de caracteres de destino MySQL la base de datos predeterminada no es UTF-8.
> 
> **Solución alternativa**  
> Establecer el carácter predeterminado establecido para la base de datos MySQL remota en UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: Algunos vínculos no están visibles en DotNetNuke tras publicar o descargando el sitio

> Si se publica o descarga un sitio de DotNetNuke, tendrá que borrar la memoria caché para obtener los nuevos vínculos que aparecerá en el sitio.
> 
> **Solución alternativa**
> 
> 1. Inicie sesión como "Host".
> 2. Vaya al menú de host y seleccione **configuración de Host**.
> 3. Desplácese hacia abajo y en **configuración avanzada**, expanda **configuración de rendimiento**.
> 4. Haga clic en el **Borrar caché** vínculo para las páginas.
> 5. Vaya a la parte inferior de la página y reiniciar la aplicación.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: Algunos vínculos en AtomSite se interrumpen después de descargar un sitio publicado

> **Solución alternativa**  
> En el *service.config* archivo *users.config* archivo y todos los *.xml* archivos, reemplace la cadena de dirección URL (por ejemplo, `http://myhost.com/atomsite`) con el local (por ejemplo, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: Aplicaciones basadas en MySQL como WordPress no pueden publicar e informar de un error de base de datos

> De manera predeterminada, WebMatrix instala MySQL con el juego de caracteres UTF-8. Si instala MySQL en su propio y el juego de caracteres no es UTF-8 (por ejemplo, es Latin1), podría producir un error en el proceso de publicación para las bases de datos.
> 
> **Solución alternativa**
> 
> 1. Cambiar el juego de caracteres para MySQL a UTF-8. (Para obtener más información, consulte [Server del juego de caracteres y la intercalación](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) en el sitio Web de MySQL.)
> 2. Vuelva a instalar la aplicación.
> 3. Volver a publicar la aplicación.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "Sitio de descarga publicado" se produce un error para las aplicaciones que tienen el programa de instalación basada en explorador

> Algunas aplicaciones (por ejemplo, Kentico CMS) requieren que se inicie en el explorador para llevar a cabo el programa de instalación posteriores a la instalación como la creación de una base de datos. Si publica una aplicación similar al siguiente sin completar la instalación basada en explorador, intenta descargar el mismo sitio desde un servidor remoto generará un error.
> 
> **Solución alternativa**  
> Finalizar el programa de instalación basada en explorador antes de publicar el sitio.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "Sitio de descarga publicado" produce un error de base de datos de DotNetNuke y Kooboo CMS

> Si intenta descargar una aplicación de un servidor y tener credenciales de administrador en la cadena de conexión de base de datos en el **configuración de publicación** cuadro de diálogo, puede aparecer el siguiente error en el registro de la publicación:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solución alternativa**  
> Si es factible, vuelva a publicar el sitio (o publicarlo) con credenciales sin privilegios de administrador para la base de datos.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obtener más información

Para obtener más información acerca de WebMatrix 1.0, vea los siguientes sitios Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Reservados todos los derechos. [Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).
