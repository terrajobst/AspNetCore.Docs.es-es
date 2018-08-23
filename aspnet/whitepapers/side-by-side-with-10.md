---
uid: whitepapers/side-by-side-with-10
title: La ejecución de .NET Framework 1.0 y 1.1 de ASP.NET Side-by-Side | Microsoft Docs
author: rick-anderson
description: Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en el equipo, lo que permite a una aplicación Web ASP.NET para ejecutarse en cualquier versión de los fotogramas...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: b5457a62e127ba555674fbe3b9f75552cad041c7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836541"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Ejecución de .NET Framework 1.0 y 1.1 de ASP.NET Side-by-Side
====================
> Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en el equipo, lo que permite a una aplicación Web ASP.NET para ejecutarse en cualquier versión de framework.
> 
> Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.


En ASP.NET, se dice que las aplicaciones se ejecutan en paralelo cuando se instalan en el mismo equipo, pero usar diferentes versiones de .NET Framework. El siguiente tema describe cómo configurar las aplicaciones de ASP.NET para la ejecución en paralelo y proporciona pasos detallados para:

- [Mantener la asignación de la aplicación Web a la versión 1.0 de .NET Framework durante la instalación](#1)
- [Asignar una aplicación Web a una versión específica de .NET Framework](#2)
- [Busque la versión de .NET Framework que está utilizando un sitio Web](#3)

Tradicionalmente, cuando se actualiza un componente o aplicación en un equipo, la versión anterior está eliminada y reemplazada por la versión más reciente. Si la nueva versión no es compatible con la versión anterior, esto suele afectar a otras aplicaciones que usan el componente o aplicación. .NET Framework proporciona compatibilidad para la ejecución en paralelo, lo que permite varias versiones de un ensamblado o la aplicación se instale en el mismo equipo al mismo tiempo. Dado que varias versiones pueden instalarse al mismo tiempo, las aplicaciones administradas pueden seleccionar qué versión debe utilizar sin que afecte a las aplicaciones que usan una versión diferente.

De forma predeterminada, durante la instalación de .NET Framework versión 1.1, todas las aplicaciones ASP.NET existentes se vuelven a configurar automáticamente para utilizar la versión más reciente de .NET Framework. Si no desea que sus aplicaciones ASP.NET para .NET Framework 1.1 de forma predeterminada, haga clic en [aquí](#1) para saber cómo evitarlo durante la instalación.

Si actualiza su servidor Web a .NET Framework 1.1 y desea que una o varias aplicaciones Web para ejecutar .NET Framework 1.0, deberá actualizar la asignación de Script de Internet Information Services (IIS). La asignación de script es el mecanismo para asignar la extensión de archivo .aspx para una aplicación Web específica a una versión de .NET Framework. Haga clic en [aquí](#2) para obtener información sobre cómo asignar una aplicación Web a una versión específica de .NET Framework.

Puede usar el Administrador de información de Internet o la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe) para buscar la versión de .NET Framework se está ejecutando una aplicación Web concreta. Haga clic en [aquí](#3) para obtener información sobre cómo buscar la versión de .NET Framework que está utilizando un sitio Web.

Una consideración de importación al migrar a .NET Framework 1.1 es que cada versión de .NET Framework usa su propio archivo Machine.config. Como resultado, si un administrador Web ha realizado cambios en el archivo Machine.config, esos cambios se deben migrar al archivo Machine.config de .NET Framework 1.1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Mantener la asignación de la aplicación Web a .NET Framework 1.0 durante la instalación

De forma predeterminada, todas las aplicaciones ASP.NET existentes se vuelven a configurar automáticamente durante la instalación para usar la versión más reciente de .NET Framework. Con la versión más reciente de .NET Framework, las aplicaciones pueden aprovechar al máximo las mejoras y nuevas características incluidas en la nueva versión. Al mismo tiempo, el Administrador de Web, que es posible que desee un control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones ASP.NET existentes durante la instalación de .NET Framework.

Para evitar la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de .NET Framework, el administrador Web puede usar la opción de línea de comandos /noaspupgrade con el programa de instalación Dotnetfx.exe.

**Para evitar la reasignación del total de la aplicación ASP.NET a la versión más reciente**

1. Vaya a **iniciar**.
2. Haga clic en **ejecutar**.
3. Escriba **cmd**.
4. Haga clic en **Aceptar**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Desde el símbolo del sistema, escriba la línea siguiente para iniciar la instalación de .NET Framework: **/c: Dotnetfx.exe "instalar /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Haga clic en **Sí** en el programa de instalación de Microsoft .NET Framework 1.1. Esta acción iniciará el proceso de instalación de .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Asignar una aplicación Web a una versión específica de .NET Framework

Cada versión de .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe). Esta herramienta permite a los administradores especificar que una aplicación Web se ejecute en una versión concreta de .NET Framework. Esto se conoce como asignación de una aplicación Web a una versión de .NET Framework. Los administradores deben seleccionar Aspnet\_regiis.exe que corresponde a la versión de .NET Framework que se asociará con la aplicación Web. Por ejemplo, un administrador que desea especificar que un sitio Web usa .NET Framework 1.1 debe usar Aspnet\_regiis.exe que viene con .NET Framework 1.1.

Aspnet\_regiis.exe para la versión 1.0 se encuentra en:

- C:\Windows\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe versión 1,1 se encuentra en:

- C:\Windows\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe proporciona dos opciones para la asignación de una aplicación Web de la secuencia de comandos:

- **-s** establece la asignación de script en la ruta de acceso y en su elemento secundario directorios.
- **-sn** establece la asignación de script en la ruta de acceso solo.

La ruta de acceso define la ruta de los metadatos de Web application IIS, que se define en el formulario de W3SVC/ROOT / {WebSiteNumber} / {aplicación\_nombre}. Por ejemplo, para una aplicación Web denominada Portal ubicado en el sitio Web predeterminado, la ruta de acceso de metabase es W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Tenga en cuenta que también puede usar una herramienta denominada Editor de la Metabase para obtener la ruta de acceso de metabase. Esta herramienta se puede descargar desde el sitio de Microsoft Support en [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Ejecute Aspnet\_mapa y su subapplication de secuencias de comandos regiis.exe -s W3SVC/1/ROOT/el Portal para actualizar el portal de IIS.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Ejecute Aspnet\_regiis.exe -sn W3SVC/1/ROOT/el Portal para actualizar la secuencia de comandos IIS portal se asignan, sin que afecte a las aplicaciones en el portal? subdirectorios s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Busque la versión de .NET Framework que está utilizando una aplicación Web

Un administrador puede usar el Administrador de servicios de Internet para encontrar la versión de .NET Framework ejecuta un sitio Web. Versiones diferentes del sistema operativo inicie el Administrador de servicios de Internet de forma diferente. Para iniciar el Administrador de servicios, siga los pasos indicados a continuación.

**Para iniciar el Administrador de servicios Internet**

1. Vaya a **iniciar**.
2. Haga clic en **ejecutar**.
3. Tipo **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Desde el Administrador de servicios de Internet, seleccione la aplicación Web cuya versión de .NET Framework que desea saber.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Haga doble clic en la aplicación Web y haga clic en **propiedades.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. En la ventana de propiedades, seleccione **configuración.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. En la tabla de asignación de aplicaciones, seleccione **.aspx**y haga clic en **editar**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Desde el **ejecutable** cuadro de texto, examine el directorio de versión mediante el desplazamiento. Si el directorio de versión es v.1.1.4322, la aplicación se asigna a la versión 1.1 de .NET Framework. Por el contrario, si el directorio de versión es v1.0.3705, la aplicación se asigna a .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
