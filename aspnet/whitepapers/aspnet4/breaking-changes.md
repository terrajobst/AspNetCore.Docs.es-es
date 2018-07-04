---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 importantes cambios | Microsoft Docs
author: rick-anderson
description: Este documento describe los cambios que se han realizado para la versión de .NET Framework versión 4 que puede afectar a las aplicaciones que se crearon con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: bf9968c49e904c064338d7123405cb6578a915f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378655"
---
<a name="aspnet-4-breaking-changes"></a>Cambios de 4 de ASP.NET
====================
> Este documento describe los cambios que se han realizado para la versión de .NET Framework versión 4 que puede afectar a las aplicaciones que se crearon con versiones anteriores, incluidas las versiones de ASP.NET 4 Beta 1 y Beta 2.
> 
> [Descargue estas notas del producto](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Contenido

[Valor de ControlRenderingCompatibilityVersion en el archivo Web.config](#0.1__Toc256770141 "_Toc256770141")  
[Cambios de ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode y UrlEncode ahora codifican las comillas simples](#0.1__Toc256770143 "_Toc256770143")  
[Página de ASP.NET (.aspx) analizador es Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Actualiza los archivos de definición de explorador](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll quitó el archivo de configuración de raíz Web](#0.1__Toc256770146 "_Toc256770146")  
[Validación de solicitudes ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Algoritmo hash predeterminado es ahora HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Errores de configuración relacionados con la nueva configuración de ASP.NET 4 Root](#0.1__Toc256770149 "_Toc256770149")  
[Las aplicaciones de ASP.NET 4 secundarios producirá un error para que se inicie en ASP.NET 2.0 o ASP.NET 3.5 aplicaciones](#0.1__Toc256770150 "_Toc256770150")  
[Producirá un error de sitios Web ASP.NET 4 iniciar en equipos donde está instalado SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[La propiedad HttpRequest.FilePath ya No incluye los valores de PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 aplicaciones podrían generar errores HttpException que hacen referencia a eurl.axd](#0.1__Toc256770153 "_Toc256770153")  
[No se podrían provocar no controladores de eventos en un documento predeterminado en IIS 7 o IIS 7.5 en modo integrado de](#0.1__Toc256770154 "_Toc256770154")  
[Cambia a la implementación de seguridad (CA) de acceso de código ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Se han movido MembershipUser y otros tipos en el Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Almacenamiento en caché cambia para variar el resultado \* encabezado HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Tipos de System.Web.Security Passport son obsoleto](#0.1__Toc256770158 "_Toc256770158")  
[Se produce un error en la propiedad MenuItem.PopOutImageUrl representar una imagen en ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl y Menu.DynamicPopOutImageUrl no representar imágenes cuando las rutas de acceso contienen barras diagonales inversas](#0.1__Toc256770160 "_Toc256770160")  
[Declinación de responsabilidades](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Valor de ControlRenderingCompatibilityVersion en el archivo Web.config

Se han modificado los controles de ASP.NET en .NET Framework versión 4 con el fin de permitirle especificar con mayor precisión cómo representar marcado. En versiones anteriores de .NET Framework, algunos controles emitían un marcado que no tenía ninguna manera de deshabilitar. De forma predeterminada, este tipo de marcado de ASP.NET 4 ya no se genera.

Si usa Visual Studio 2010 para actualizar la aplicación desde ASP.NET 2.0 o ASP.NET 3.5, la herramienta agrega automáticamente una configuración para el `Web.config` archivo que conserva la representación heredada. Sin embargo, si actualiza una aplicación cambiando el grupo de aplicaciones en IIS para elegir como destino .NET Framework 4, ASP.NET usa el nuevo modo de representación de forma predeterminada. Para deshabilitar el nuevo modo de representación, agregue la siguiente configuración en el `Web.config` archivo:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Los cambios más importantes de representación que aporta el nuevo comportamiento son los siguientes:

- El **imagen** y **ImageButton** controles ya no representan un `border="0"` atributo.
- El **BaseValidator** clase y controles de validación que derivan de ella ya no representan texto rojo de forma predeterminada.
- El **HtmlForm** control no representa un **nombre** atributo.
- El **tabla** controlar ya no representa un `border="0"` atributo.
- Los controles que no están diseñados para la entrada de usuario (por ejemplo, el **etiqueta** control) ya no representan la `disabled="disabled"` atributo si sus **habilitado** propiedad está establecida en **false**(o si heredan esta configuración de un control contenedor).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode cambios

El **ClientIDMode** configuración en ASP.NET 4 permite especificar cómo genera ASP.NET el **id** atributo para los elementos HTML. En versiones anteriores de ASP.NET, el comportamiento predeterminado era equivalente a la **AutoID** de **ClientIDMode**. Sin embargo, el valor predeterminado es ahora **Predictable**.

Si usa Visual Studio 2010 para actualizar la aplicación desde ASP.NET 2.0 o ASP.NET 3.5, la herramienta agrega automáticamente una configuración para el `Web.config` archivo que conserva el comportamiento de versiones anteriores de .NET Framework. Sin embargo, si actualiza una aplicación cambiando el grupo de aplicaciones en IIS para elegir como destino .NET Framework 4, ASP.NET usará el nuevo modo de forma predeterminada. Para deshabilitar el nuevo modo de identificador de cliente, agregue la siguiente configuración en el `Web.config` archivo:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode y UrlEncode ahora codifican las comillas simples

En ASP.NET 4, el **HtmlEncode** y **UrlEncode** métodos de la **HttpUtility** y **HttpServerUtility** clases se han actualizado a codificar el carácter de comilla simple (') como sigue:

- El **HtmlEncode** método codifica las instancias de la comilla simple como.
- El **UrlEncode** método codifica las instancias de la comilla simple como 27%.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Página de ASP.NET (.aspx) analizador es Stricter

El analizador de páginas para las páginas ASP.NET (`.aspx` archivos) y controles de usuario (`.ascx` archivos) es más estricto en ASP.NET 4 y rechazará más instancias de marcado no válido. Por ejemplo, los siguientes dos fragmentos podrían analizar correctamente en versiones anteriores de ASP.NET, pero ahora, producirán errores del analizador en ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Tenga en cuenta el punto y coma no válido al final de la **HiddenField** etiqueta.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Tenga en cuenta la sin cerrar **estilo** atributo que se ejecuta en el **CssClass** atributo.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Actualiza los archivos de definición de explorador

Los archivos de definición de explorador se han actualizado para que incluyan información sobre exploradores y dispositivos nuevos y actualizados. Se han quitado exploradores y dispositivos anteriores, como Netscape Navigator, y se han agregado exploradores y dispositivos más recientes, como Google Chrome y Apple iPhone.

Si la aplicación contiene definiciones de explorador personalizadas que heredan de una de las definiciones de explorador que se han quitado, aparecerá un error. Por ejemplo, si la `App_Browsers` carpeta contiene una definición de explorador que se hereda de la definición de explorador IE2, recibirá el siguiente mensaje de error de configuración:

- No se encuentra el elemento de explorador o puerta de enlace con el Id. 'IE2'.

> [!NOTE]
> El **HttpBrowserCapabilities** objeto (que se expone mediante la página **Request.Browser** propiedad) se controla mediante los archivos de definiciones del explorador. Por lo tanto, la información devuelta por el acceso a una propiedad de este objeto en ASP.NET 4 podría ser diferente de la información devuelta en una versión anterior de ASP.NET.


Puede volver a los archivos de definición de explorador antiguo copiando los archivos de definición de explorador de la siguiente carpeta:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copie los archivos en la correspondiente `\CONFIG\Browsers` carpeta para ASP.NET 4. Después de copiar los archivos, ejecute Aspnet\_herramienta de línea de comandos regbrowsers.exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll quitado del archivo de configuración de raíz Web

En versiones anteriores de ASP.NET, una referencia al ensamblado System.Web.Mobile.dll se incluyó en la raíz `Web.config` de archivos en el **ensamblados** sección. Con el fin de mejorar el rendimiento, se ha quitado la referencia a este ensamblado.

El ensamblado System.Web.Mobile.dll está incluido en ASP.NET 4, pero está en desuso. Si desea usar los tipos del ensamblado System.Web.Mobile.dll, agregue una referencia a este ensamblado en la raíz de cualquier `Web.config` archivo o a una aplicación `Web.config` archivo. Por ejemplo, si desea utilizar cualquiera de los controles ASP.NET mobile (en desuso), debe agregar una referencia al ensamblado System.Web.Mobile.dll en el `Web.config` archivo.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Validación de solicitudes de ASP.NET

La característica de validación de solicitudes en ASP.NET proporciona un cierto nivel de protección predeterminada frente a ataques de scripting entre sitios (XSS). En versiones anteriores de ASP.NET, la validación de solicitudes se habilitó de forma predeterminada. Sin embargo, aplica solo a las páginas ASP.NET (`.aspx` archivos y sus archivos de clase) y solo cuando se estaban ejecutando dichas páginas.

En ASP.NET 4, de forma predeterminada, la validación de solicitudes está habilitada para todas las solicitudes, ya que está habilitado antes de la **BeginRequest** fase de una solicitud HTTP. Como resultado, la validación de solicitudes se aplica a las solicitudes de todos los recursos ASP.NET, no solo las solicitudes de página .aspx. Esto incluye las solicitudes, como llamadas a servicios Web y controladores HTTP personalizados. Validación de solicitudes también está activa cuando los módulos HTTP personalizados estén leyendo el contenido de una solicitud HTTP.

Como resultado, ahora pueden producir errores de validación de solicitud para las solicitudes que anteriormente no se desencadenan errores. Para revertir al comportamiento de la característica de validación de solicitudes de ASP.NET 2.0, agregue la siguiente configuración en el `Web.config` archivo:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Sin embargo, se recomienda que analizar los errores de validación de solicitud para determinar si los controladores existentes, módulos u otro código personalizado, tiene acceso a potencialmente entradas HTTP no seguras que podrían ser XSS vectores de ataque.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Algoritmo hash predeterminado es ahora HMACSHA256

ASP.NET usa cifrado y algoritmos hash para ayudar a proteger los datos, como las cookies de autenticación de formularios y el estado de vista. De forma predeterminada, ASP.NET 4 ahora usa el algoritmo HMACSHA256 para las operaciones de hash de las cookies y el estado de vista. Las versiones anteriores de ASP.NET utilizan el algoritmo HMACSHA1 antiguo.

Las aplicaciones pueden verse afectadas si ejecuta mixto ASP.NET 2.0/ASP.NET 4 entornos donde los datos, como las cookies de autenticación de formularios deben trabajar across.NET versiones de Framework. Para configurar una aplicación Web de ASP.NET 4 para usar el algoritmo HMACSHA1 anterior, agregue la siguiente configuración en el `Web.config` archivo:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Errores de configuración relacionados con la nueva configuración de ASP.NET 4 Root

Los archivos de configuración raíz (el `machine.config` archivo y la raíz `Web.config` archivo) para el .NET Framework 4 (y, por tanto, ASP.NET 4) se han actualizado para incluir la mayor parte de la información de configuración reutilizable que, en ASP.NET 3.5, se encontró en el aplicación `Web.config` archivos. Dada la complejidad de los sistemas de configuración de IIS 7 e IIS 7.5 administrados, ejecutar aplicaciones de ASP.NET 3.5 en ASP.NET 4 y en IIS 7 e IIS 7.5 puede producir en ASP.NET o IIS errores de configuración.

Se recomienda que actualice las aplicaciones de ASP.NET 3.5 a ASP.NET 4 mediante el uso de las herramientas de actualización de proyecto en Visual Studio 2010, si es posible. Visual Studio 2010 modifica automáticamente la aplicación de ASP.NET 3.5 `Web.config` archivo contenga la configuración adecuada para ASP.NET 4.

Sin embargo, es un escenario admitido para ejecutar aplicaciones de ASP.NET 3.5 con .NET Framework 4 sin volver a compilar. En ese caso, es posible que deba modificar manualmente la aplicación `Web.config` archivo antes de ejecutar la aplicación en .NET Framework 4 y en IIS 7 o IIS 7.5.

Las dos secciones siguientes describen los cambios que necesite para realizar diferentes combinaciones de software.

**Windows Vista SP1 o Windows Server 2008 SP1, donde están instalados ni revisión KB958854 ni SP2.** En esta configuración, el sistema de configuración de IIS 7 combina incorrectamente configuración administrada de una aplicación mediante la comparación de nivel de la aplicación `Web.config` archivo a la versión 2.0 de ASP.NET `machine.config` archivos. Debido a esto, nivel de aplicación `Web.config` archivos de .NET Framework 3.5 o posterior deben tener un **system.web.extensions** definición de la sección de configuración (el elemento) para no provocar un error de validación de IIS 7.

Sin embargo, puede modificar manualmente nivel de aplicación `Web.config` entradas del archivo que no coinciden exactamente con las definiciones de sección de configuración reutilizable original que se introdujeron con Visual Studio 2008, producirán errores de configuración de ASP.NET. (Las entradas de configuración predeterminado que se generan mediante Visual Studio 2008 funcionen correctamente.) Un problema común es que puede modificar manualmente `Web.config` archivos omita el **allowDefinition** y **requirePermission** atributos de configuración que se encuentran en la sección de configuración distintos definiciones. Esto produce una incoherencia entre la sección de configuración abreviado en el nivel de aplicación `Web.config` archivos y la definición completa en ASP.NET 4 `machine.config` archivo. Como resultado, en tiempo de ejecución, el sistema de configuración de ASP.NET 4 produce un error de configuración.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 y también en Windows Vista SP1 y Windows Server 2008 SP1 está instalado revisión KB958854.**

En este escenario, el sistema de configuración nativa de IIS 7 e IIS 7.5 devuelve un error de configuración ya que realiza una comparación de texto en el **tipo** atributo que se define para un controlador de la sección de configuración administradas. Dado que todos los `Web.config` los archivos generados por Visual Studio 2008 y Visual Studio 2008 SP1 tienen "3.5" en la cadena de tipo para el **system.web.extensions** (y otros relacionados) controladores de sección de configuración y porque ASP.NET 4 `machine.config` archivo tiene "4.0" en el **tipo** atributo para los mismos controladores de sección de configuración, las aplicaciones que se generan en Visual Studio 2008 SP1 o de Visual Studio 2008 siempre producirá un error de validación de la configuración en IIS 7 y IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Resolver estos problemas.

La solución alternativa para el primer escenario consiste en actualizar el nivel de aplicación `Web.config` archivo incluyendo el texto de la configuración de modelo desde un `Web.config` archivo generado automáticamente por Visual Studio 2008.

Una solución alternativa para el primer escenario consiste en instalar Service Pack 2 para la Vista o Windows Server 2008 en el equipo o instale la revisión KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) para corregir el comportamiento de combinación de configuración incorrecta de la Sistema de configuración de IIS. Sin embargo, después de realizar cualquiera de estas acciones, la aplicación probablemente se encontrará con un error de configuración debido al problema descrito para el segundo escenario.

La solución alternativa para el segundo escenario es elimine o comente todo el **system.web.extensions** definiciones desde el nivel de aplicación de grupo de definiciones de la sección de configuración y la sección de configuración `Web.config` archivo. Estas definiciones son normalmente en la parte superior del nivel de aplicación `Web.config` de archivos y puede identificarse por el **configSections** elemento y sus elementos secundarios.

En ambos casos, se recomienda eliminar manualmente el **system.codedom** sección, aunque esto no es necesario.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Las aplicaciones de ASP.NET 4 secundarios producirá un error para que se inicie en ASP.NET 2.0 o ASP.NET 3.5 aplicaciones

Las aplicaciones de ASP.NET 4 configuradas como elementos secundarios de aplicaciones que ejecutan versiones anteriores de ASP.NET podrían no iniciarse debido a errores de configuración o de compilación. El ejemplo siguiente muestra una estructura de directorios de una aplicación afectada.

`/parentwebapp` (configurado para usar ASP.NET 2.0 o ASP.NET 3.5)  
`/childwebapp` (configurado para usar ASP.NET 4)

La aplicación en el `childwebapp` carpeta producirá un error iniciar en IIS 7 o IIS 7.5 y notificará un error de configuración. El texto de error incluirá un mensaje similar al siguiente:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

En IIS 6, la aplicación en el `childwebapp` carpeta también se producirá un error al iniciar, pero se notificará un error diferente. Por ejemplo, el texto del error podría indicar lo siguiente:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Estos escenarios se producen porque la información de configuración de la aplicación principal en el `parentwebapp` carpeta forma parte de la jerarquía de información de configuración que determina los valores de configuración combinada final que se usan por la web secundarios aplicación en el `childwebapp` carpeta. Dependiendo de si la aplicación Web de ASP.NET 4 se ejecuta en IIS 7 (o IIS 7.5) o en IIS 6, el sistema de configuración de IIS o el sistema de compilación de ASP.NET 4 se devolverá un error.

Los pasos que debe seguir para resolver este problema y obtener al elemento secundario que funcione la aplicación de ASP.NET 4 dependen de si ejecuta la aplicación de ASP.NET 4 en IIS 6 o en IIS 7 (o IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Paso 1 (IIS 7 o IIS 7.5)

Este paso sólo es necesario en sistemas operativos que ejecutan IIS 7 o IIS 7.5, que incluye Windows Vista, Windows Server 2008, Windows 7 y Windows Server 2008 R2.

Mover el **configSections** definición en el `Web.config` archivo de la aplicación primaria (la aplicación que se ejecuta ASP.NET 2.0 o ASP.NET 3.5) en la raíz `Web.config` archivo para.NET Framework 2.0. El sistema de configuración nativa de IIS 7 e IIS 7.5 examina el **configSections** elemento cuando combina con la jerarquía de archivos de configuración. Mover el **configSections** definición de la aplicación Web del padre `Web.config` archivo a la raíz `Web.config` archivo oculta de manera eficaz el elemento desde el proceso de mezcla de configuración que se produce el elemento secundario de ASP.NET 4 aplicación.

En un sistema operativo de 32 bits o para grupos de aplicaciones de 32 bits, la raíz `Web.config` archivo para ASP.NET 2.0 y ASP.NET 3.5 normalmente se encuentra en la siguiente carpeta:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

En un sistema operativo de 64 bits o para grupos de aplicaciones de 64 bits, la raíz `Web.config` archivo para ASP.NET 2.0 y ASP.NET 3.5 normalmente se encuentra en la siguiente carpeta:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Si ejecuta aplicaciones Web de 32 bits y 64 bits en un equipo de 64 bits, debe mover el **configSections** elemento copia en la raíz `Web.config` archivos de 32 bits y los sistemas de 64 bits.

Cuando coloca el **configSections** elemento en la raíz `Web.config` archivo, pegue la sección inmediatamente después de la **configuración** elemento. El ejemplo siguiente muestra lo que la parte superior de la raíz `Web.config` archivo aspecto cuando haya terminado de mover los elementos.

> [!NOTE]
> En el ejemplo siguiente, las líneas se han ajustado para mejorar la legibilidad.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Paso 2 (todas las versiones de IIS)

Este paso es necesario si la aplicación Web de ASP.NET 4 secundarios se está ejecutando en IIS 6 o en IIS 7 (o IIS 7.5).

En el `Web.config` Agregar archivo de la aplicación Web que se está ejecutando ASP.NET 2 o ASP.NET 3.5, principal un **ubicación** etiqueta que especifique explícitamente (para sistemas de configuración de IIS y ASP.NET) que solo las entradas de configuración se aplican a la aplicación Web principal. El ejemplo siguiente muestra la sintaxis de la **ubicación** elemento va a agregar:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

El ejemplo siguiente se muestra cómo el **ubicación** etiqueta a la que se utiliza para ajustar todas las secciones de configuración a partir de la **appSettings** sección y terminando con **system.webServer**sección.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Cuando haya completado los pasos 1 y 2, las aplicaciones Web de ASP.NET 4 secundarios se iniciarán sin errores.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Producirá un error de sitios Web ASP.NET 4 iniciar en equipos donde está instalado SharePoint

Servidores Web que ejecutan SharePoint tienen un `Web.config` archivo que se implementa en la raíz de un sitio Web de SharePoint (por ejemplo, `c:\inetpub\wwwroot\web.config` para el sitio Web predeterminado). En este `Web.config` archivo, SharePoint establece una confianza parcial personalizado nivel denominada WSS\_mínima.

Si intenta ejecutar un sitio Web de ASP.NET 4 que se implementa como un elemento secundario de este tipo de sitio Web de SharePoint, verá el siguiente error:

`Could not find permission set named 'ASP.NET'.`

Este error se produce porque la infraestructura de seguridad (CAS) de acceso de código de ASP.NET 4 busca un conjunto de permisos denominado ASP.NET. Sin embargo, la parcial confiar en el archivo de configuración que se hace referencia por WSS\_mínima no contiene ningún conjunto de permisos con ese nombre.

Actualmente no hay una versión de SharePoint que sea compatible con ASP.NET. Como resultado, no debe intentar ejecutar un sitio Web de ASP.NET 4 como un sitio secundario debajo de Web de SharePoint de sitios.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La propiedad HttpRequest.FilePath ya No incluye los valores de PathInfo

Las versiones anteriores de ASP.NET incluidas un **PathInfo** valor en el valor devuelto de diversas archivos relacionados con la ruta de acceso propiedades, incluyendo **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, y **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 ya no incluye el **PathInfo** valor en los valores devueltos de estas propiedades. En su lugar, el **PathInfo** información está disponible en **HttpRequest.PathInfo**. Por ejemplo, veamos el siguiente fragmento de dirección URL:

`/testapp/Action.mvc/SomeAction`

En versiones anteriores de ASP.NET, **HttpRequest** propiedades tienen los siguientes valores:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (vacío)

En ASP.NET 4, **HttpRequest** en su lugar, las propiedades tienen los siguientes valores:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 podrían generar errores HttpException que hacen referencia a eurl.axd a aplicaciones

Después de que se ha habilitado ASP.NET 4 en IIS 6, las aplicaciones de ASP.NET 2.0 que se ejecutan en IIS 6 (en Windows Server 2003 o Windows Server 2003 R2) podrían generar errores como la siguiente:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Este error se produce porque cuando ASP.NET detecta que un sitio Web está configurado para usar ASP.NET 4, un componente nativo de ASP.NET 4 pasa una dirección URL sin extensión a la parte administrada de ASP.NET para su posterior procesamiento. Sin embargo, si los directorios virtuales que están por debajo de un sitio Web de ASP.NET 4 se configuran para usar ASP.NET 2.0, procesamiento de la dirección URL sin extensión en los resultados de esta manera en una dirección URL modificada que contiene la cadena "eurl.axd". Esta dirección URL modificada, a continuación, se envía a la aplicación de ASP.NET 2.0. ASP.NET 2.0 no reconoce el formato de "eurl.axd". Por lo tanto, ASP.NET 2.0 intenta encontrar un archivo denominado `eurl.axd` y ejecútelo. Dado que no existe ese archivo, se produce un error en la solicitud con un **HttpException** excepción.

Puede solucionar este problema mediante una de las opciones siguientes.

### <a name="option-1"></a>Opción 1

Si no es necesario para ejecutar el sitio Web de ASP.NET 4, vuelva a asignar el sitio para usar ASP.NET 2.0 en su lugar.

### <a name="option-2"></a>Opción 2

Si se requiere ASP.NET 4 para ejecutar el sitio Web, mueva los directorios virtuales de ASP.NET 2.0 de secundario a otro sitio Web que está asignado a ASP.NET 2.0.

### <a name="option-3"></a>Opción 3

Si no resulta práctico volver a asignar el sitio Web para ASP.NET 2.0 o para cambiar la ubicación de un directorio virtual, deshabilite explícitamente la dirección URL sin extensión de procesamiento en ASP.NET 4. Utilice el procedimiento siguiente:

1. En el registro de Windows, abra el nodo siguiente:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Cree un nuevo **DWORD** valor denominado **EnableExtensionlessUrls**.
2. Establecer **EnableExtensionlessUrls** en 0. Esto deshabilita el comportamiento de dirección URL sin extensión.
3. Guarde el valor del registro y cierre el editor del registro.
4. Ejecute el **iisreset** herramienta de línea de comandos, lo que hace que IIS leer el nuevo valor del registro.

> [!NOTE]
> Establecer **EnableExtensionlessUrls** 1 habilita el comportamiento de dirección URL sin extensión. Se trata la configuración predeterminada si se especifica ningún valor.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>No se podrían provocar no controladores de eventos en un documento predeterminado en IIS 7 o IIS 7.5 en modo integrado

ASP.NET 4 incluye las modificaciones que cambiarán el modo **acción** atributo del código HTML **formulario** elemento se representa cuando se resuelve una dirección URL sin extensión en un documento predeterminado. Un ejemplo de una dirección URL sin extensión resolver a un documento predeterminado sería [ http://contoso.com/ ](http://contoso.com/), lo que provocará una solicitud para [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

Ahora, ASP.NET 4 representa el código HTML **formulario** del elemento **acción** valor de atributo como una cadena vacía cuando se realiza una solicitud a una dirección URL sin extensión que tiene un documento predeterminado asignado a él. Por ejemplo, en las versiones anteriores de ASP.NET, una solicitud para [ http://contoso.com ](http://contoso.com) daría lugar a una solicitud para `Default.aspx`. En ese documento, la apertura **formulario** etiqueta se representaría como en el ejemplo siguiente:

`<form action="Default.aspx" />`

En ASP.NET 4, una solicitud para [ http://contoso.com ](http://contoso.com) también tiene como resultado una solicitud para `Default.aspx`. Sin embargo, ASP.NET ahora representa el HTML de apertura **formulario** etiqueta como en el ejemplo siguiente:

`<form action="" />`

Esta diferencia en el modo **acción** atributo se representa para hacer cambios sutiles en cómo se procesa un formulario post por IIS y ASP.NET. Cuando el **acción** atributo es una cadena vacía, IIS **DefaultDocumentModule** objeto creará una solicitud secundaria a `Default.aspx`. En la mayoría de las condiciones, esta solicitud secundaria es transparente para el código de aplicación y la `Default.aspx` página se ejecuta con normalidad.

A pesar de ello, una interacción potencial entre el código administrado y el modo integrado de IIS 7 o IIS 7.5 puede hacer que las páginas .aspx administradas dejen de funcionar correctamente durante la solicitud secundaria. Si dan las siguientes condiciones, la solicitud secundaria a un `Default.aspx` documento dará como resultado un error o un comportamiento inesperado:

1. Una página .aspx se envía al explorador con el **formulario** del elemento **acción** atributo establecido en "".
2. El formulario se publica a ASP.NET.
3. Un módulo HTTP administrado lee una parte del cuerpo de la entidad. Por ejemplo, un módulo lee **Request.Form** o **Request.Params**. Esto hace que el cuerpo de la entidad de la solicitud POST se lea en la memoria administrada. Como resultado, el cuerpo de la entidad ya no está disponible para los módulos de código nativo que se ejecutan en el modo integrado de IIS 7 o IIS 7.5.
4. IIS **DefaultDocumentModule** objeto finalmente se ejecuta y crea una solicitud secundaria a la `Default.aspx` documento. Sin embargo, dado que un fragmento del código administrado ya ha leído el cuerpo de la entidad, no hay ningún cuerpo de entidad disponible para enviarlo a la solicitud secundaria.
5. Cuando se ejecuta la canalización HTTP para la solicitud secundaria, el controlador para `.aspx` files se ejecuta durante la fase de ejecución de controlador.
6. Porque no hay ningún cuerpo de entidad, hay ninguna variable de formulario y ningún estado de vista y, por lo tanto, no está disponible para el controlador de página .aspx determinar qué eventos (si existe) se supone que se generará ninguna información. Como resultado, no se ejecuta ninguno de los controladores de eventos postback para la página .aspx afectada.

Puede evitar este comportamiento de las maneras siguientes:

- Identifique el módulo HTTP que tiene acceso el cuerpo de la entidad de la solicitud durante las solicitudes de documento predeterminada y determinar si se puede configurar para ejecutar solo para las solicitudes administradas. En el modo integrado de IIS 7 e IIS 7.5, los módulos HTTP se pueden marcar para ejecutarse solo para las solicitudes administradas agregando el siguiente atributo para el módulo **System.webServer/Modules** entrada:

- `precondition="managedHandler"`

- Este deshabilita la configuración del módulo para las solicitudes que IIS 7 e IIS 7.5 determinan como que no administra las solicitudes. Para las solicitudes de documentos de forma predeterminada, la primera solicitud es una dirección URL sin extensión. Por lo tanto, IIS no ejecuta los módulos administrados que están marcados con una condición previa de controlador administrado durante el procesamiento de la solicitud inicial. Como resultado, los módulos administrados no leerá accidentalmente el cuerpo de entidad y, por tanto, el cuerpo de la entidad sigue estando disponible y se pasa a la solicitud secundaria y el documento predeterminado.

- Si los módulos HTTP problemáticos tienen que ejecutar para todas las solicitudes (para los archivos estáticos, para las direcciones URL sin extensión que se resuelven en el **DefaultDocumentModule** objeto administradas solicitudes, etc.), modifique las páginas .aspx afectada por de forma explícita establecer el **acción** propiedad de la página **System.Web.UI.HtmlControls.HtmlForm** control en una cadena no vacía. Por ejemplo, si el documento predeterminado es `Default.aspx`, modifique el código de la página para establecer explícitamente el **HtmlForm** del control **acción** propiedad a "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Cambios realizados en la implementación de seguridad (CA) de acceso de código de ASP.NET

ASP.NET 2.0, y por extensión las características de ASP.NET que se agregaron en 3.5, utilice el modelo de security (CAS) de acceso de código 2.0 y .NET Framework 1.1. Aun así, la implementación de CAS en ASP.NET 4 se ha mejorado considerablemente. Como resultado, las aplicaciones de ASP.NET de confianza parcial que se basan en el código de confianza que se ejecuta en la caché de ensamblados global (GAC) pueden producir un error con varias excepciones de seguridad. Aplicaciones de confianza parcial que se basan en modificaciones extensas de la directiva CAS del equipo también pueden producir un error con excepciones de seguridad.

Puede revertir las aplicaciones de ASP.NET 4 de confianza parcial al comportamiento de ASP.NET 1.1 y 2.0 con el nuevo **legacyCasModel** atributo en el **confianza** elemento de configuración, tal como se muestra en el ejemplo siguiente :

`<trust level= "Medium" legacyCasModel="true" />`

Cuando se revierte al modelo CAS heredado, se habilitan los siguientes comportamientos de las entidades de certificación anteriores:

- Respeta la directiva de entidades emisoras de certificados de equipo.
- Se permiten varios conjuntos de permisos diferentes en un único dominio de aplicación.
- Las aserciones de permiso explícito no son necesarias para los ensamblados en la GAC que se invocan cuando solo ASP.NET u otro código de .NET Framework está en la pila.

No se puede revertir un escenario en .NET Framework 4: aplicaciones de confianza parcial no Web ya no pueden llamar a determinadas API en System.Web.dll y System.Web.Extensions.dll. En versiones anteriores de .NET Framework, era posible para las aplicaciones de Web que no son de confianza parcial que se deben conceder explícitamente <strong>AspNetHostingPermission</strong> permisos. Estas aplicaciones, a continuación, podrían usar <strong>System.Web.HttpUtility</strong>, los tipos en el <strong>System.Web.ClientServices.\< /strong > * espacios de nombres y tipos relacionados con pertenencia a roles y perfiles. Ya no se admite llamar a estos tipos de aplicaciones Web que no son de confianza parcial en .NET Framework 4.

> [!NOTE]
> El **HtmlEncode** y **HtmlDecode** funcionalidad de la **System.Web.HttpUtility** clase se ha movido a la nueva versión de .NET Framework 4  **System.Net.WebUtility** clase. Si esta fue la única funcionalidad ASP.NET que se estaba usando, modifique el código de la aplicación para usar la nueva **WebUtility** clase en su lugar.


Este es un resumen general de los cambios a la implementación de entidades emisoras de certificados predeterminada en ASP.NET 4:

- Dominios de aplicación de ASP.NET ahora son dominios de aplicación homogénea. Conjuntos de concesión de plena confianza y de confianza parcial solo están disponibles en un dominio de aplicación.
- Conjuntos de concesión de confianza parcial ASP.NET son independientes de cualquier directiva de CAS de nivel empresarial, nivel de equipo o nivel de usuario.
- Los ensamblados ASP.NET que se incluye en 3.5 y 3.5 SP1 se han convertido para usar el modelo de transparencia de .NET Framework 4.
- Uso de ASP.NET **AspNetHostingPermission** atributo se ha reducido considerablemente. La mayoría de las instancias de este atributo se quitaron las APIs ASP.NET públicas.
- Los ensamblados compilados dinámicamente creados por los proveedores de compilación ASP.NET se actualizaron para marcar explícitamente los ensamblados como transparente.
- Ahora se marcan todos los ensamblados ASP.NET de manera que se respeta el atributo APTCA solo en entornos de hospedaje Web. Entornos de hospedaje Web que no son de confianza parcial, como ClickOnce no podrá llamar a los ensamblados ASP.NET.

Para obtener más información sobre el nuevo modelo de seguridad de acceso de código de ASP.NET 4, consulte [Using Code Access Security en aplicaciones ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) en el sitio Web de MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Se han movido MembershipUser y otros tipos en el Namespace System.Web.Security

Algunos tipos que se usan en la pertenencia ASP.NET se han movido desde `System.Web.dll` al nuevo ensamblado System.Web.ApplicationServices.dll. Estos tipos se han movido para resolver las dependencias de capas de arquitectura entre los tipos del cliente y de las SKU de .NET Framework extendidas.

Proyectos de sitio Web no tienen problemas como resultado de mover estos tipos, porque System.Web.ApplicationServices.dll se agregó a la lista de ensamblados de referencia que se usa de forma predeterminada por el sistema de compilación de ASP.NET. Si actualiza un proyecto de sitio Web que se creó con una versión anterior de ASP.NET a ASP.NET 4, ábralo en Visual Studio 2010, el proyecto se compilará sin errores.

De forma similar, si actualiza un proyecto de aplicación Web que se creó en una versión anterior de ASP.NET a ASP.NET 4, ábralo en Visual Studio 2010, el proceso de actualización agrega una referencia a System.Web.ApplicationServices.dll al proyecto. Por lo tanto, puede actualizar proyectos de aplicación también se compilan sin errores de Web.

Archivos (binarios) compilados que se crearon con versiones anteriores de ASP.NET también se ejecutarán sin errores en ASP.NET 4, aunque los tipos de pertenencia se movieron a un ensamblado diferente. Reenviar la información de tipo se ha agregado a la versión de ASP.NET 4 de `System.Web.dll` que enruta automáticamente a la nueva ubicación para los tipos de referencias en tiempo de ejecución para estos tipos.

Sin embargo, se producirá un error de las bibliotecas de clases que usan tipos de suscripción específica y que se han actualizado desde versiones anteriores de ASP.NET compilar cuando se utiliza en un proyecto de ASP.NET 4. Por ejemplo, puede producir un error de un proyecto de biblioteca de clases compilar y notificar un error como el siguiente:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Puede solucionar este problema agregando una referencia a System.Web.ApplicationServices.dll en el proyecto de biblioteca de clases.

La siguiente lista se muestran los *System.Web.Security* tipos que se movieron desde `System.Web.dll` a System.Web.ApplicationServices.dll:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Almacenamiento en caché cambia para variar el resultado \* encabezado HTTP

En ASP.NET 1.0, un error provocado en caché las páginas que especifican `Location="ServerAndClient"` como configuración de la caché de resultados para emitir un `Vary:*` encabezado HTTP en la respuesta. Como consecuencia, se indicaba a los exploradores de cliente que nunca almacenasen en caché la página localmente.

En ASP.NET 1.1, el **System.Web.HttpCachePolicy.SetOmitVaryStar** se agregó el método, se podría llamar para suprimir el `Vary:*` encabezado. Este método se ha elegido porque cambiando el encabezado HTTP emitido se consideran potencialmente importantes cambios en el momento. Sin embargo, los desarrolladores confundidos por el comportamiento de ASP.NET e informes de errores sugieren que los desarrolladores son conscientes de las existentes **SetOmitVaryStar** comportamiento.

En ASP.NET 4, la Decisión se realizó para solucionar el problema de raíz. El `Vary:*` encabezado HTTP ya no se emite desde las respuestas que especifican la directiva siguiente:

`<%@OutputCache Location="ServerAndClient" %>`

Como resultado, **SetOmitVaryStar** deja de ser necesaria con el fin de suprimir la `Vary:*` encabezado.

En las aplicaciones que especifican `Location="ServerAndClient"` en el **@ OutputCache** la directiva en una página, podrá ver el comportamiento implícito en el nombre de la **ubicación** valor del atributo, que es, serán páginas puede almacenar en caché en el explorador sin necesidad de que se llama el **SetOmitVaryStar** método.

Si las páginas de la aplicación deben emitir `Vary:*`, llame a la **AppendHeader** método, como en el ejemplo siguiente:

`HttpResponse.AppendHeader("Vary","*");`

Como alternativa, puede cambiar el valor de la caché de resultados **ubicación** atributo "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Tipos System.Web.Security Passport están obsoleto

La compatibilidad con Passport integrada en ASP.NET 2.0 ha sido obsoleto y no admitida durante unos años debido a cambios en la cuenta de Passport (ahora Live ID). Como resultado, los cinco tipos relacionados con Passport en **System.Web.Security** ahora se marcan con el **ObsoleteAttribute** atributo.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Se produce un error en la propiedad MenuItem.PopOutImageUrl representar una imagen en ASP.NET 4

En ASP.NET 3.5, el *MenuItem.PopOutImageUrl* propiedad le permite especificar la dirección URL de una imagen que se muestra en un elemento de menú para indicar que el elemento de menú tiene un submenú dinámico. El ejemplo siguiente muestra cómo especificar esta propiedad en el marcado en ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Como resultado un cambio de diseño en ASP.NET 4, no se representa ningún resultado para el *PopOutImageUrl* si la propiedad está establecida para el *MenuItem* clase. En su lugar, debe especificar una dirección URL de imagen directamente en el *menú* controlar mediante la *StaticPopOutImageUrl* propiedad o el *DynamicPopOutImageUrl* propiedad. Cuando se trabaja con un menú estático, el *Menu.StaticPopOutImageUrl* propiedad especifica la dirección URL de una imagen que se muestra con el fin de indicar que el elemento de menú estático tiene un submenú, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Si está trabajando con un menú dinámico, usa el *Menu.DynamicPopOutImageUrl* propiedad para especificar la dirección URL de una imagen que indica que un elemento de menú dinámico tiene un submenú. El ejemplo siguiente es similar al anterior, pero se muestra cómo establecer el *DynamicPopOutImageUrl* propiedad para un menú dinámico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Si el *Menu.DynamicPopOutImageUrl* propiedad no está establecida y el *Menu.DynamicEnableDefaultPopOutImage* propiedad está establecida en *false*, no se muestra ninguna imagen. De forma similar, si la *StaticPopOutImageUrl* propiedad no está establecida y el *StaticEnableDefaultPopOutImage* propiedad está establecida en *false*, no se muestra ninguna imagen.

Al establecer las rutas de acceso para estas propiedades, use una barra diagonal (/) en lugar de una barra diagonal inversa (\). Para obtener más información, consulte [Menu.StaticPopOutImageUrl y Menu.DynamicPopOutImageUrl no representar imágenes cuando las rutas de acceso contienen barras diagonales inversas](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") en otra parte de este documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>No se pueden representar imágenes cuando las rutas de acceso contienen barras diagonales inversas Menu.StaticPopOutImageUrl y Menu.DynamicPopOutImageUrl

En ASP.NET 4, las imágenes que especifican mediante el *Menu.StaticPopOutImageUrl* y *Menu.DynamicPopOutImageUrl* propiedades no se representarán si la ruta de acceso contiene backlashes (\). Se trata de un cambio con respecto a versiones anteriores de ASP.NET.

El siguiente ejemplo de *menú* controlar marcado muestra el *StaticPopOutImageUrl* propiedad establecida mediante una ruta de acceso que contiene una barra diagonal inversa. En ASP.NET 4, no se representará la imagen especificada en la propiedad.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Para corregir este problema, cambie los valores de ruta de acceso que se especifican en el *StaticPopOutImageUrl* y *DynamicPopOutImageUrl* propiedades para usar barras diagonales (/). El ejemplo siguiente muestra este cambio:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Tenga en cuenta que las aplicaciones que se han migrado desde versiones anteriores de ASP.NET a ASP.NET 4 también podrían verse afectado, porque el *MenuItem.PopOutImageUrl* propiedad ha cambiado. Para obtener más información, consulte [The MenuItem.PopOutImageUrl propiedad no se puede representar una imagen en ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") en otra parte de este documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos mencionados son ficticio y ninguna asociación con ninguna empresa real, organización, producto, nombre de dominio, correo electrónico dirección, logotipo, persona, lugar o evento se pretende indicar ni debe deducirse.

© 2010 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
