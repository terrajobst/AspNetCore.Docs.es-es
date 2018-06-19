---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET deniega el acceso a directorios IIS | Documentos de Microsoft
author: rick-anderson
description: Estas notas del producto describen lo que debe hacer si una solicitud para la aplicación ASP.NET devuelve el error "acceso denegado al directorio DirectoryName. No se pudo s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070776"
---
<a name="aspnet-denied-access-to-iis-directories"></a>Denegado el acceso a directorios IIS de ASP.NET
====================
> Estas notas del producto describen lo que debe hacer si una solicitud para la aplicación ASP.NET devuelve el error "acceso denegado a *DirectoryName* directory. No se pudo iniciar la supervisión de cambios en el directorio."
> 
> Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.


RTM de ASP.NET V1 ahora se ejecuta con un menor con privilegios de cuenta de windows - registrado como la cuenta "ASPNET" en un equipo local.

En algunos sistemas bloqueados, esta cuenta puede no predeterminada tener lectura acceso de seguridad para los directorios de contenido de un sitio Web, el directorio raíz de la aplicación o el directorio raíz del sitio web. En este caso le notificará el siguiente error cuando se solicitan páginas desde una aplicación web determinada:

![](denied-access-to-iis-directories/_static/image1.jpg)

Para solucionar este problema debe cambiar los permisos de seguridad en los directorios adecuados.

En concreto, ASP.NET requiere leer, ejecutar y lista de acceso de la cuenta ASPNET para la raíz del sitio web (por ejemplo: c:\inetpub\wwwroot o en cualquier directorio de sitio alternativo que se ha configurado en IIS), el directorio de contenido y el directorio raíz de la aplicación con el fin de supervisar los cambios del archivo de configuración. La raíz de la aplicación se corresponde con la ruta de acceso de la carpeta asociada con el directorio virtual de la aplicación en la herramienta de administración de IIS (inetmgr).

Por ejemplo, considere la siguiente jerarquía de la aplicación en la carpeta wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

En este ejemplo, la cuenta ASPNET necesita los permisos de lectura definidos anteriormente para el contenido en el myapp y el directorio wwwroot. Una única ACL heredada en la carpeta raíz también opcionalmente sirve para ambos directorios si está anidados.

Para agregar permisos para un directorio, realice los pasos siguientes:

- Mediante el Explorador de Windows, navegue hasta el directorio
- Haga clic con el botón secundario en la carpeta del directorio y elija "Propiedades"
- Vaya a la pestaña "Seguridad" en el cuadro de diálogo de propiedades
- Haga clic en el botón "Agregar" y escriba el nombre del equipo seguido del nombre de la cuenta ASPNET. Por ejemplo, en un equipo denominado "webdev", ¿escriba webdev\ASPNET y haga clic en "Aceptar".
- Asegúrese de que la cuenta ASPNET tiene la "lectura &amp; Execute", "Mostrar el contenido de la carpeta" y "Lectura" casillas de verificación activadas.
- Haga clic en Aceptar para cerrar el cuadro de diálogo y guardar los cambios.

![](denied-access-to-iis-directories/_static/image2.jpg)

Si lo desea, estos cambios se pueden automatizar mediante secuencias de comandos o la herramienta "cacls.exe" que se incluye con Windows. Para obtener más información sobre la cuenta ASPNET, consulte el [documento de preguntas más frecuentes sobre](https://go.microsoft.com/fwlink/?LinkId=5828).

Si una determinada aplicación web se basa en tener escritura o modificar permisos para un archivo o carpeta determinado, esto se puede conceder el mismo procedimiento y comprobando las casillas de verificación de "Escritura" o "Modificar".

En equipos que permiten a todos los usuarios o el grupo a los usuarios acceso de lectura a estos directorios (que es la configuración predeterminada), no se encuentre ningún problema y los pasos anteriores no será necesarios.
