---
uid: whitepapers/side-by-side-with-10
title: "Ejecución de ASP.NET en paralelo de .NET Framework 1.0 y 1.1 | Documentos de Microsoft"
author: rick-anderson
description: "Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en su equipo, lo que permite una aplicación Web ASP.NET para ejecutarse en cualquiera de las versiones de los fotogramas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Ejecución de ASP.NET en paralelo de .NET Framework 1.0 y 1.1
====================
> Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en su equipo, lo que permite una aplicación Web ASP.NET para ejecutarse en cualquiera de las versiones de framework.
> 
> Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.


En ASP.NET, las aplicaciones se dice que se ejecutan en paralelo cuando se instalan en el mismo equipo, pero utilizan distintas versiones de .NET Framework. El siguiente tema describe cómo configurar aplicaciones ASP.NET para la ejecución en paralelo y proporciona los pasos detallados para:

- [Mantener la asignación de la aplicación Web a la versión 1.0 de .NET Framework durante la instalación](#1)
- [Asignar una aplicación Web a una versión concreta de .NET Framework](#2)
- [Buscar la versión de .NET Framework que está utilizando un sitio Web](#3)

Tradicionalmente, cuando un componente o aplicación se actualiza en un equipo, la versión anterior es eliminada y reemplazada por la versión más reciente. Si la nueva versión no es compatible con la versión anterior, esto normalmente interrumpe otras aplicaciones que utilizan el componente o aplicación. .NET Framework proporciona compatibilidad para la ejecución en paralelo, lo que permite varias versiones de un ensamblado o la aplicación se instale en el mismo equipo al mismo tiempo. Dado que se pueden instalar varias versiones simultáneamente, las aplicaciones administradas pueden seleccionar qué versión debe utilizar sin que ello afecte a las aplicaciones que utilizan una versión diferente.

De forma predeterminada, durante la instalación de .NET Framework versión 1.1, todas las aplicaciones de ASP.NET existentes se vuelven a configurar automáticamente para utilizar la versión más reciente de .NET Framework. Si no desea que las aplicaciones de ASP.NET en la configuración predeterminada para .NET Framework 1.1, haga clic en [aquí](#1) para obtener información sobre cómo evitar que esto durante la instalación.

Si actualiza el servidor Web a la versión 1.1 de .NET Framework y desea que una o varias aplicaciones Web para ejecutar .NET Framework 1.0, debe actualizar la asignación de Script de Internet Information Services (IIS). La asignación de script es el mecanismo para asignar la extensión de archivo .aspx para una aplicación Web concreta a una versión de .NET Framework. Haga clic en [aquí](#2) para obtener información sobre cómo asignar una aplicación Web a una versión concreta de .NET Framework.

Puede usar el Administrador de información de Internet o la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe) para buscar qué versión de .NET Framework se ejecuta una aplicación Web concreta. Haga clic en [aquí](#3) para obtener información sobre cómo buscar la versión de .NET Framework que está utilizando un sitio Web.

Una consideración de importación al migrar a .NET Framework 1.1 es que cada versión de .NET Framework usa su propio archivo Machine.config. Como resultado, si un administrador Web ha realizado cambios en el archivo Machine.config, esos cambios se deben migrar al archivo Machine.config de .NET Framework 1.1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Mantener la asignación de la aplicación Web para .NET Framework 1.0 durante la instalación

De forma predeterminada, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente durante la instalación para usar la versión más reciente de .NET Framework. Con la versión más reciente de .NET Framework, las aplicaciones pueden aprovechar al máximo de mejoras y nuevas características incluidas en la nueva versión. Al mismo tiempo, el Administrador de Web, que podría desear el control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones de ASP.NET existentes durante la instalación de .NET Framework.

Para impedir la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de .NET Framework, el administrador Web puede usar la opción de línea de comandos /noaspupgrade con el programa de instalación Dotnetfx.exe.

**Para evitar la reasignación del total de la aplicación ASP.NET a la versión más reciente**

1. Vaya a **iniciar**.
2. Haga clic en **ejecutar**.
3. Escriba **cmd**.
4. Haga clic en **Aceptar**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Desde el símbolo del sistema, escriba la línea siguiente para iniciar la instalación de .NET Framework: **/c: Dotnetfx.exe "instalar /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Haga clic en **Sí** en el programa de instalación de Microsoft .NET Framework 1.1. Se iniciará el proceso de instalación de .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Asignar una aplicación Web a una versión concreta de .NET Framework

Cada versión de .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe). Esta herramienta permite a los administradores especificar que una aplicación Web se ejecute en una versión concreta de .NET Framework. Esto se conoce como asignación de una aplicación Web a una versión de .NET Framework. Los administradores deben seleccionar Aspnet\_regiis.exe que corresponde a la versión de .NET Framework que se asociará con la aplicación Web. Por ejemplo, un administrador que desea especificar que un sitio Web use .NET Framework 1.1 debe utilizar Aspnet\_regiis.exe que se incluye con .NET Framework 1.1.

Aspnet\_regiis.exe para la versión 1.0 se encuentra en:

- C:\Windows\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis

Aspnet\_regiis.exe para la versión 1,1 se encuentra en:

- C:\Windows\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe proporciona dos opciones para asignar una aplicación Web de la secuencia de comandos:

- **-s** establece la asignación de script en la ruta de acceso y su elemento secundario directorios.
- **-sn** establece la asignación de script en la ruta de acceso solo.

La ruta de acceso define la ruta de metadatos de Web aplicación IIS, que se define en el formulario de W3SVC/ROOT / {WebSiteNumber} / {aplicación\_nombre}. Por ejemplo, para una aplicación Web que se le denominada Portal ubicado en el sitio Web predeterminado, la ruta de acceso de metabase es W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Tenga en cuenta que también puede utilizar una herramienta denominada Editor de Metabase para obtener la ruta de acceso de metabase. Puede descargar esta herramienta desde el sitio de Microsoft Support en [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Ejecutar Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal para actualizar el portal de IIS script mapa y su subapplication.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Ejecutar Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal para actualizar la secuencia de comandos IIS portal se asignan, sin que ello afecte a las aplicaciones en el portal? subdirectorios s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Buscar la versión de .NET Framework que está utilizando una aplicación Web

Un administrador puede usar el Administrador de servicios de Internet para buscar la versión de .NET Framework ejecuta un sitio Web. Versiones diferentes del sistema operativo inicie el Administrador de servicios de Internet de manera diferente. Para iniciar el Administrador de servicios, siga los pasos indicados a continuación.

**Para iniciar el Administrador de servicios de Internet**

1. Vaya a **iniciar**.
2. Haga clic en **ejecutar**.
3. Tipo de **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Desde el Administrador de servicios de Internet, seleccione la aplicación Web cuya versión de .NET Framework que desea conocer.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Haga doble clic en la aplicación Web y haga clic en **propiedades.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. En la ventana Propiedades, seleccione **configuración.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. En la tabla de asignación de aplicación, seleccione **.aspx**y haga clic en **editar**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Desde el **ejecutable** cuadro de texto, busque en el directorio de la versión mediante el desplazamiento. Si el directorio de la versión es v.1.1.4322, la aplicación se asigna a la versión 1.1 de .NET Framework. Por el contrario, si el directorio de la versión es v1.0.3705, la aplicación se asigna a .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
