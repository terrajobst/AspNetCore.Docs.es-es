---
uid: web-pages/readme/beta3
title: Web Matrix y Léame de versión Beta 3 de ASP.NET Web Pages (Razor) | Documentos de Microsoft
author: rick-anderson
description: Matriz de Web y Léame de versión Beta 3 de ASP.NET Web Pages (Razor)
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892886"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Matriz de Web y Léame de versión Beta 3 de ASP.NET Web Pages (Razor)
====================
> Matriz de Web y Léame de versión Beta 3 de ASP.NET Web Pages (Razor)

9 de noviembre de 2010

## <a name="contents"></a>Contenido

- [Información general](#Overview)
- [Instalación](#Installation_Notes)
- [Nuevas características, los cambios y los problemas conocidos de la versión Beta 3](#Known_Issues)

    - [Problemas de instalación de WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalación de aplicaciones](#Known_Issues_Installing_Applications)
    - [Publicación de aplicaciones](#Known_Issues_Publishing_Applications)
    - [Otros problemas](#Known_Issues_Other_Issues)
- [Para obtener más información](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Información general

> Microsoft WebMatrix Beta es una pila de desarrollo web gratuita que se instala en pocos minutos. Se integra un servidor web con la base de datos y marcos de trabajo para crear una única experiencia integrada de programación. Puede usar WebMatrix Beta para agilizar la manera de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o puede usar WebMatrix Beta para iniciar un nuevo sitio Web mediante aplicaciones populares de código abierto como DotNetNuke, Umbraco, WordPress o Joomla. Versión WebMatrix Beta usa el mismo servidor web eficaz, el motor de base de datos y el entorno de marcos de trabajo que se ejecutará el sitio Web en internet, lo que hace la transición de desarrollo a producción ágil y sin inconvenientes.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalación

> Para instalar la versión Beta 3 de WebMatrix, use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Después de instalar al instalador de plataforma Web, se puede usar para instalar la versión Beta 3 de WebMatrix.
> 
> Si tiene problemas durante la instalación, consulte [solución de problemas con el instalador de plataforma Web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instrucciones para publicar aplicaciones

> Vea [instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nuevas características, cambios, andKnown problemas

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalación de WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: la versión Beta 3 de WebMatrix solo está disponible en plataformas compatibles con Microsoft .NET Framework 4

> La versión 4 de .NET Framework es necesaria para la versión WebMatrix Beta. En algunos casos, el programa de instalación de la versión WebMatrix Beta le permitirá intente instalar en una plataforma que no forma parte del conjunto de configuración admitidos. En concreto, Windows Vista sin la actualización de SP1 le permiten comenzar la instalación de la versión WebMatrix Beta, pero producirá un error en el componente de .NET Framework 4 y bloquear la instalación.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: No se puede instalar WebMatrix Beta 3 si se instala Microsoft Visual Studio 2008 sin Microsoft Visual Studio 2008 SP1

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

Esta sección del documento describe nuevas características, los cambios y los problemas conocidos con la versión Beta 3 de ASP.NET Web Pages con sintaxis Razor.

- [Nuevas características](#NewFeatures)
- [Cambios](#Changes)
- [Problemas](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nuevas características en la versión Beta 3 para ASP.NET Web Pages con sintaxis Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nuevo: Método de "Html.Raw" representa el marcado sin codificar

> El nuevo `Html.Raw` método le permite presentar HTML como marcado en lugar de representar la salida codificada. (De forma predeterminada, ASP.NET Razor codifica las cadenas antes de procesarlas.) La sintaxis es la siguiente:
> 
> `Html.Raw(value)`
> 
> En el siguiente ejemplo se muestra cómo usar `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Cambios en la versión Beta 3 para ASP.NET Web Pages con sintaxis Razor

#### <a name="change-hrefattribute-method-removed"></a>Cambio: Método "Atributo href" quitar

> El `HrefAttribute` método de la `WebPage` clase se ha quitado. Esta aplicación auxiliar se usó para codificar los caracteres no seguros en las direcciones URL. Ya no es necesario porque ASP.NET Razor automáticamente codifica las cadenas. (Use la nueva `Html.Raw` método para representar las cadenas sin codificar.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Cambio: Sintaxis declarativa "@helper" aplicaciones auxiliares cambiadas

> En la versión Beta 3, ASP.NET cambia cómo analizar aplicaciones auxiliares que se crean mediante el `@helper` sintaxis. En esencia, el `@helper` sintaxis ahora se analiza como un bloque de código en lugar de como un bloque de marcado que puede incluir código. Por lo tanto, no es necesario incluirse en el código dentro de la aplicación auxiliar `@{ }` bloques. Por el contrario, marcado dentro de la aplicación auxiliar debe incluirse explícitamente en los elementos HTML o ASP.NET Razor `<text></text>` etiquetas.
> 
> Por ejemplo, la siguiente `@helper` sintaxis funciona en la versión Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> En la versión Beta 3, esta aplicación auxiliar debe modificarse para que sea similar al ejemplo siguiente:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Tenga en cuenta que el `@{ }` ya no se utiliza caracteres alrededor del código inicial en la aplicación auxiliar. Esto es porque el contenido de las aplicaciones auxiliares se trata como un bloque de código de forma predeterminada. La aplicación auxiliar representa el marcado, que comienza con la apertura `<a>` etiqueta. Si la aplicación auxiliar debe representar texto sin formato o las etiquetas que no incluyen una etiqueta de cierre (por ejemplo, `<meta>` etiquetas), debe ser el contenido que se representará en `<text></text>` etiquetas.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Cambio: Quita "WebPageContext.HttpContext"

> El `WebPageContext.HttpContext` se ha quitado la propiedad. Utilice `HttpContext.Current` en su lugar. (El `WebPageContext.HttpContext` propiedad simplemente ajustado esto.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Cambio: Movido al nuevo paquete de aplicación auxiliar de "Facebook"

> El `Facebook` auxiliar se ha movido a la *Facebook.Helper* library, que incluye el `Facebook` auxiliar y funciones adicionales. Debe instalar esta biblioteca como un paquete independiente, como se describe en "Instalar aplicaciones auxiliares con Administrador de paquetes" en el tutorial [Introducción a las páginas ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Cambio: Pertenencia, el rol y seguridad de tipos se mueve al nuevo ensamblado

> Los tipos siguientes se han movido a la `WebMatrix.WebData` ensamblado:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Cambio: Clase "TagBuilder" movido al ensamblado System.Web.WebPages.dll

> La `TagBuilde` clase de r se ha movido al ensamblado System.Web.WebPages.dll. Anteriormente, esto estaba en un ensamblado que formaba parte de ASP.NET MVC. Este cambio significa que no es necesario instalar ASP.NET MVC para poder usar la `TagBuilder` clase.
> 
> Sin embargo, la clase aún está en el `System.Web.Mvc` espacio de nombres. Para poder usar el `TagBuilder` clase (por ejemplo, en una ASP.NET Razor aplicación auxiliar personalizada), debe hacer referencia el espacio de nombres (por ejemplo, agregando `@using System.Web.Mvc` en el código).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Cambio: Ha cambiado; de la sintaxis de validación de solicitud Clase de "Validación" quitar

> En la versión Beta 3, para deshabilitar la validación para un campo individual o un conjunto de campos, puede llamar a la `Validation.Exclude` método, pasando el nombre o nombres de los campos que se van a excluir de la validación. Una nueva sintaxis está disponible en la versión Beta 3 para omitir la validación. El `Validation` se ha quitado el método usado en la versión Beta 3.
> 
> > [!NOTE]
> > Si no deshabilita la validación de solicitudes, si los usuarios intentan cargar el marcado HTML (por ejemplo, mediante un editor de texto enriquecido en una página), el sitio Web notificará un error como *se ha detectado un valor de Request.Form potencialmente peligroso desde el cliente*y no se acepta la entrada del usuario. Si deshabilita la validación de solicitudes, debe comprobar manualmente proporcionados por el usuario para asegurarse de que no contienen marcado potencialmente peligroso o secuencias de comandos con algo parecido a la [V4.0 de biblioteca de secuencias de comandos de Microsoft Anti-Cross sitio](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Para deshabilitar la validación de solicitudes automática, llame a la `Request.Unvalidated` método y pásele el nombre del campo o de otro objeto de entrada que desea omitir la validación de solicitud para. Puede usar este método para omitir la validación de los elementos en el `Form`, `QueryString`, `Cookies`, y `ServerVariables` las colecciones. Los ejemplos siguientes muestran cómo utilizar el `Unvalidated` método:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemas conocidos de ASP.NET Web Pages con sintaxis Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamiento inesperado cuando se usa una tabla de usuario personalizada para la suscripción

> Para inicializar el proveedor de pertenencia para un sitio Web de ASP.NET Razor, se llama a la `WebSecurity.InitializeDatabaseConnection` método. (En WebMatrix, la plantilla de sitio de inicio incluye una llamada a este método en el  *\_AppStart.cshtml* archivo.) Si el `autoCreateTables` parámetro de este método se establece en true (de forma predeterminada, se establece en true en la plantilla de sitio de inicio), y si se pasa un nombre de tabla no reconocida para el método (el segundo parámetro), el método no produce un error. En su lugar, crea automáticamente en la tabla.
> 
> Esto puede ser un problema si va a utilizar una tabla de usuario personalizada para la pertenencia a pero pase el nombre de tabla incorrecto para el `WebSecurity.InitializeDatabaseConnection` método. Dado que el método no predeterminada genera un error si no existe en la tabla que especifique, y porque en su lugar, crea una nueva tabla, la aplicación puede parecer que va a trabajar. Sin embargo, código de aplicación que se basa en una tabla de usuario personalizada (y en los campos de él) puede producir un error con errores inesperados.
> 
> **Solución alternativa**  
> Asegúrese de que el nombre se pasa en el `InitializeDatabaseConnection` coincidencias de método, el perfil de usuario de la tabla en la base de datos de pertenencia, o asegúrese de que el `autoCreateTables` parámetro se establece en false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "error no se pudo generar una instancia de usuario de SQL Server"

> Si una aplicación WebMatrix Web usa SQL Server Express y ejecuta IIS 7.5 en Windows 7 o Windows Server 2008 R2, verá un error que indica que SQL Server no se puede recuperar la ruta de acceso de aplicación local del usuario en tiempo de ejecución.
> 
> **Solución alternativa** Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente el servicio de red) tiene permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas como *aplicación\_datos*. Hay información más detallada en el artículo de Knowledge Base [problemas con instancias de usuario de SQL Server Express y proyectos de aplicación Web de ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: En Visual Studio, los espacios de nombres para los ensamblados personalizados (DLL) no se importan automáticamente

> Si utiliza ensamblados personalizados en un proyecto de Visual Studio, los espacios de nombres declarados en los ensamblados no se importan automáticamente en tiempo de diseño. Como resultado, las referencias a tipos personalizados no se reconozcan en tiempo de diseño y se marcan como no reconocido en Visual Studio (con un "subrayado ondulado"). Este problema se produce solo en tiempo de diseño en Visual Studio; la propia aplicación se ejecuta correctamente.
> 
> **Solución alternativa**  
> Incluir un `using` instrucción (`imports` en Visual Basic) que hace referencia a las entidades que no se reconocen en tiempo de diseño.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense y proyecto de plantillas disponibles solo en ASP.NET MVC versión 3

> Instalar ASP.NET Web Pages no instalar herramientas para Visual Studio como IntelliSense y proyecto de plantillas para aplicaciones de ASP.NET Web Pages.
> 
> **Solución alternativa** para usar las plantillas de proyecto y de IntelliSense para las aplicaciones de ASP.NET Web Pages en Visual Studio, instale ASP.NET MVC 3 RC ya sea mediante el instalador de plataforma Web o la [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: "&lt;auxiliar&gt; no se pudo encontrar la clase" error

> Después de actualizar a la versión Beta 3, podría aparecer un error que una clase auxiliar (por ejemplo, el `Facebook` clase) no se encuentra. A partir de la versión Beta 2 y continuar en la versión Beta 3, las aplicaciones auxiliares que se han movido a los paquetes que deben instalar explícitamente. Los sitios existentes no se actualizan para incluir estos paquetes; Esto incluye los sitios en la *\My Documents\IISExpress* o *\My Documents\My sitios Web* carpetas. En concreto, verá este error si usa el sitio predeterminado en *Mis sitios* (WebSite1), que incluye una referencia a la `Twitter` auxiliar.
> 
> **Solución alternativa**  
> Comenta llamadas a las aplicaciones auxiliares en el sitio, ejecute el  *\_administración* página e instale el paquete o paquetes que incluyen las aplicaciones auxiliares que desea usar. Después de instalar el paquete, puede quite el comentario de las líneas que hacen referencia a las aplicaciones auxiliares.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: Implementar ensamblados de Beta 3 ASP.NET Razor en la carpeta Bin podría no funcionar en sitios que hospeda

> Si implementa un sitio Web de ASP.NET Web Pages en un sitio de hospedaje, y si implementa los ensamblados de la versión Beta 3 de ASP.NET Razor en el sitio *Bin* carpeta, es posible que experimente errores, incluidos los siguientes:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Esto puede ocurrir si el proveedor de hospedaje ha instalado a los ensamblados de ASP.NET Web Pages Beta 1 en la caché del servidor aplicación global (GAC). Ensamblados en la GAC obtienen prioridad sobre los ensamblados instalados localmente en el *Bin* carpeta.
> 
> **Solución alternativa** póngase en contacto con su proveedor de hospedaje para confirmar que los errores que se está viendo debido a un conflicto entre las versiones del proveedor de los ensamblados y el suyo. Si es así, solicite que el proveedor de hospedaje actualizar los ensamblados en la GAC del servidor.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Leer fuentes u otros datos externos a través de un servidor proxy

> Si el servidor que ejecuta el sitio está detrás de un servidor proxy, tendrá que configurar la información de proxy en el *Web.config* archivo para poder leer la información que procede de fuera de su sitio. Por ejemplo, si usa el `ReCaptcha` auxiliar, el Ayudante se comunica con el servicio de reCAPTCHA, pero puede estar bloqueado por el servidor proxy. De forma similar, las fuentes que se utilizan en ASP.NET Web Pages, como la fuente utilizada por el Administrador de paquetes, podrían requerir configuración de proxy.
> 
> Si tiene problemas de trabajar con un servicio externo o para trabajar con el paquete de fuente, coloque los elementos siguientes en la raíz de la aplicación *Web.config* archivo:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Para obtener más información acerca de cómo configurar un servidor proxy, consulte [ &lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio Web de MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: Error de "No se puede cargar Microsoft.Web.Infrastructure.dll"

> Si previamente instalado la versión Beta 1 de ASP.NET Web Pages con sintaxis Razor y, a continuación, instalar la versión Beta 3, todos los ensamblados correspondientes se instalan en la GAC excepto *Microsoft.Web.Infrastructure.dll*. En consecuencia, al ejecutar páginas ASP.NET Razor, verá un error que indica que *Microsoft.Web.Infrastructure.dll* no se pudo cargar.
> 
> Este problema no se produce si se carga la versión Beta 3 en un equipo limpio.
> 
> **Solución alternativa**  
> En el Panel de Control, desinstale ASP.NET Web Pages. A continuación, vuelva a instalar la versión Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Desinstalar .NET Framework versión 4 deshabilita ASP.NET Web Pages con sintaxis Razor

> Si desinstala la versión 4 de .NET Framework y, a continuación, vuelva a instalarlo, se deshabilita ASP.NET Web Pages con sintaxis Razor. Páginas con la *.cshtml* extensión no se ejecutan correctamente. Las páginas Web ASP.NET registra un ensamblado en la raíz de la máquina *Web.config* archivo y quitar .NET Framework se elimina el archivo. Volver a instalar .NET Framework instala una nueva versión del archivo de configuración, pero no agrega la referencia del ensamblado de ASP.NET Web Pages.
> 
> **Solución alternativa** después de reinstalar .NET Framework, vuelva a instalar ASP.NET Web Pages con sintaxis Razor. Esto agrega el siguiente elemento a la *Web.config* archivo en la raíz de la máquina, que se encuentra normalmente en la siguiente ubicación:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: Las aplicaciones que implementó previamente con ensamblados ASP.NET en la carpeta Bin experimentan errores

> Durante la implementación, copias de los ensamblados de ASP.NET Web Pages (por ejemplo, *Microsoft.WebPages.dll*) a la *Bin* carpeta del sitio Web en el servidor. (Esto podría deberse a automáticamente durante la implementación o porque el programador copiará los ensamblados de forma explícita.) Sin embargo, cuando se instala la versión Beta 3, los errores se produce, como los errores que no se encuentra determinados tipos. Esto ocurre porque un número de tipos de páginas Web de ASP.NET se han movido en diferentes espacios de nombres para la versión Beta 3.
> 
> **Solución alternativa**   
> Desactive el *Bin* carpeta de la aplicación implementada, copie los nuevos ensamblados en la carpeta (o volver a implementar la aplicación) y, a continuación, reinicie la aplicación.


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
> - Si no tiene control sobre el equipo del servidor (por ejemplo, va a implementar en un sitio Web de hospedaje), agregue lo siguiente del sitio Web *Web.config* archivo:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: Uso de proyecto de aplicación Web o páginas Web de ASP.NET y MVC de ASP.NET en la misma aplicación

> Si utiliza ASP.NET Web Pages en un proyecto de aplicación Web o una aplicación de ASP.NET MVC, puede aparecer un error que *WebPageHttpApplication* no se encuentra.
> 
> **Solución alternativa**  
> Si recibe este error, cambie la clase base del que se deriva la aplicación. En el *Global.asax* archivo, cambie la línea siguiente:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> por esta otra:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Se invierte en vigor un cambio que se introdujo en la versión Beta 1 de ASP.NET Web Pages con sintaxis Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Implementar una aplicación en un equipo que no tiene SQL Server Compact instalado

> Las aplicaciones que incluyen bases de datos de SQL Server Compact se pueden ejecutar en un equipo donde SQL Server Compact no está instalado. Microsoft WebMatrix Beta 3 automáticamente copia estos archivos binarios para usted y realiza la correspondiente *Web.config* transformaciones de archivos.
> 
> **Solución alternativa** si necesita copiar estos archivos y realizar la *Web.config* cambios en los archivos manualmente, haga lo siguiente:
> 
> 1. Copie los ensamblados de motor de base de datos a la *Bin* carpeta (y sus subcarpetas) de la aplicación en el equipo de destino: 
> 
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **a** *\Bin*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **a** *\Bin\x86*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **a** *\Bin\amd64*
> 2. En la carpeta raíz del sitio Web, cree o abra un *Web.config* archivo. (En versión Beta 3 de WebMatrix, está disponible si hace clic en este tipo de archivo **todos los** en el **elegir un tipo de archivo** cuadro de diálogo.)
> 3. Agregue el siguiente elemento como elemento secundario de la **&lt;configuración&gt;** elemento (no en el **&lt;system.web&gt;** elemento):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: Aplicaciones auxiliares de base de datos y WebGrid no funcionan en el nivel de confianza medio en Visual Basic

> Si está utilizando Visual Basic (crear *vbhtml* archivos), el `Database` y `WebGrid` aplicaciones auxiliares no funcionará si la aplicación está configurada para usar el nivel de confianza medio.
> 
> **Solución alternativa**  
> Establecer temporalmente la aplicación para utilizar plena confianza.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: No se reconoce la propiedad "Encrypt"

> SQL Server Compact 4.0 no reconoce el `Encrypt` propiedad de la `SqlCeConnection` clase. Esta propiedad no se debe usar para cifrar los archivos de base de datos. El `Encrypt` propiedad ha quedado obsoleta en la versión SQL Server Compact 3.5 y se conservan solo por compatibilidad con versiones anteriores. 
> 
> **Solución alternativa**  
> Use la `Encryption Mode` propiedad de la `SqlCeConnection` clase para cifrar los archivos de base de datos de SQL Server Compact 4.0. En el ejemplo siguiente se muestra cómo crear una base de datos de SQL Server Compact 4.0 cifrados mediante la `Encryption Mode` propiedad:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Para cambiar el modo de cifrado de una base de datos de SQL Server Compact 4.0 existente, haga lo siguiente:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Para cifrar una base de datos de SQL Server Compact 4.0 sin cifrar, haga lo siguiente:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: Las bibliotecas de tiempo de ejecución de Microsoft Visual C++ 2008 son necesarias

> El nativo archivos DLL de SQL Server Compact 4.0 necesita los Microsoft Visual C++ 2008 en tiempo de ejecución las bibliotecas (x 86, IA64 y x 64), Service Pack 1.
> 
> **Solución alternativa**  
> Instalar .NET Framework 3.5 SP1. Esto también instala Visual C++ 2008 en tiempo de ejecución bibliotecas SP1. Puede descargar las bibliotecas desde la ubicación siguiente:   
>   
> [Actualización de seguridad ATL de paquete redistribuible de Microsoft Visual C++ 2008 Service Pack 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Tenga en cuenta que la instalación de .NET Framework 2.0, 3.0, o no 4 *no* instalar SP1 de las bibliotecas en tiempo de ejecución de Visual C++ 2008.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: Si está instalado SQL Server Compact antes de instalar .NET Framework en el equipo, su nombre invariable del proveedor no está registrado en el archivo machine.config de .NET Framework

> SQL Server Compact se puede instalar en un equipo que no tiene .NET Framework instalado porque SQL Server Compact requieren .NET framework. Si está instalada la versión 3.5 de .NET Framework ni 4 antes de instalar SQL Server Compact, el programa de instalación de SQL Server Compact no registra su nombre invariable del proveedor en el *machine.config* archivo. Cualquier aplicación que se basa en la entrada de SQL Server Compact en el *machine.config* se producirá un error de archivo. La entrada de registro del nombre invariable en *machine.config* parece que el ejemplo siguiente:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solución alternativa**  
> Desinstalar SQL Server Compact 4.0 CTP1. Descargue e instale las versiones completas de .NET Framework en la siguiente ubicación:
> 
> [Microsoft .NET Framework 3.5 con Service Pack1 (paquete completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versión de Microsoft .NET Framework 4.0 (paquete completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> A continuación, vuelva a instalar [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalación de aplicaciones

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Instalar una aplicación puede tardar mucho tiempo si se redirige la carpeta Mis documentos del usuario a un recurso compartido de red

> **Solución alternativa**  
> Ninguno. La aplicación puede tardar bastante tiempo en instalar, pero se instalará correctamente.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publicación de aplicaciones

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Otros problemas

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: / Filtro de búsqueda no funciona en los informes para agrupar por: tipo de problema

> Al ejecutar un informe para un sitio, si escribe texto en el *filtrar por dirección URL* y haga clic en *búsqueda*, no ocurre nada. Esto es porque este control no es funcional mientras se encuentre el *Group By* estado del informe se establece en *tipo de problema*, que es el valor predeterminado.
> 
> **Solución alternativa** en el *Group By* ficha de cinta de opciones, haga clic en *URL* para agrupar las entradas por su dirección URL de origen. El cuadro de texto y el botón para filtrar las entradas son funcionales mientras se encuentre en este estado.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: Las aplicaciones WCF no se ejecuta con IIS Express

> Ir a una aplicación de WCF produce un error similar al siguiente:
> 
> *No se pudo cargar el archivo o ensamblado ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una de sus dependencias. El sistema no puede encontrar el archivo especificado.*
> 
> Esto ocurre porque la versión Beta de IIS Express no es compatible con WCF de forma predeterminada.
> 
> **Solución alternativa** utilizar cualquiera de las siguientes soluciones alternativas (solución #2 requiere Microsoft Windows Vista o posterior):
> 
> 
> 1. Copia la *Microsoft.Web.dll* y *Microsoft.Web.Administration.dll* ensamblados de la ubicación de instalación de WebMatrix para la *bin* directorio de WCF aplicación. De forma predeterminada, WebMatrix está instalado en el *Microsoft WebMatrix* subcarpeta en el sistema *archivos de programa* carpeta.
> 2. En Microsoft Windows Vista o versiones posteriores, cree un vínculo simbólico a los ensamblados en el *bin* directorio mediante los siguientes comandos. (Este enfoque tiene la ventaja de que no crea una copia de los ensamblados).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Instale a los dos ensamblados en la GAC. Desde un símbolo del sistema con privilegios elevados, ejecute los siguientes comandos:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: la versión Beta 3 de WebMatrix es no se puede realizar ciertas tareas que requieren elevación

> WebMatrix Beta 3 es no se puede realizar ciertas tareas que requieren elevación, tales como instalar componentes adicionales en las situaciones siguientes:
> 
> - En Windows Vista o Windows 7, está iniciando sesión con una cuenta que no tiene privilegios administrativos y Control de cuentas de usuario (UAC) está deshabilitado.
> - Está utilizando Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Solución alternativa**  
> Mayoría de las tareas en la versión Beta 3 de WebMatrix no requiere permisos administrativos. Para aquellos que lo hacen, puede realizar la operación porque un administrador o siga estos pasos:
> 
> - En Windows Vista o Windows 7, habilitar UAC.
> - En Windows XP, agregue el usuario al grupo de seguridad Administradores.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: El "Sitio de la galería Web" está deshabilitada

> El **sitio desde galería Web** opción está deshabilitada si el instalador de plataforma Web 3.0 no está instalado.
> 
> **Solución alternativa**  
> Instalar el [instalador de plataforma Web de Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: En Windows Server 2003, IIS Express no se inicia para un usuario sin derechos administrativos

> En Windows Server 2003, al iniciar una página o iniciar IIS Express, IIS Express no se inicia. Para las páginas Web, se muestra un error que indica que la aplicación se ha iniciado por un usuario sin derechos administrativos.
> 
> **Solución alternativa**  
> Inicie la versión Beta 3 de WebMatrix como usuario administrativo. Para obtener más información, vea el artículo de Knowledge Base siguiente:  
>   
> [Una aplicación que se haya iniciado por un usuario sin derechos administrativos no puede escuchar el tráfico HTTP del equipo en el que se ejecuta la aplicación en Windows Vista, Windows Server 2003 o Windows XP.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: El botón de "Relaciones" está deshabilitado

> El **relaciones** situado bajo el **tabla** pestaña en el **bases de datos** área de trabajo está deshabilitado para las bases de datos de SQL Server Compact.
> 
> **Solución alternativa**  
> Ninguno. SQL Server Compact no admite las relaciones entre tablas.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: Las consultas SQL parametrizadas producen excepciones

> En SQL Server Compact 4.0, si no especifica un tipo de datos como `SqlDbType` o `DbType` para los parámetros en consultas con parámetros, se produce una excepción cuando se ejecuta la consulta.
> 
> **Solución alternativa**  
> Definir explícitamente el tipo de datos para los parámetros como `SqlDbType` o `DbType`. Esto es fundamental en el caso de los tipos de datos BLOB (`image` y `ntext`). Utilice código similar al siguiente:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obtener más información

Para obtener más información acerca de la versión Beta 3 de WebMatrix, consulte los siguientes sitios Web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Reservados todos los derechos. [Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).
