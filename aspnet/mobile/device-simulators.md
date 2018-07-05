---
uid: mobile/device-simulators
title: Simular dispositivos móviles conocidos para realizar pruebas | Microsoft Docs
author: rick-anderson
description: Puede descargar emuladores para exploradores y dispositivos móviles más conocidos, siga estos vínculos
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: ''
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: b3dbb4dc83b602cd1aa374d3aa05c1b09a5dfe64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396089"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Simular dispositivos móviles conocidos para realizar pruebas
====================
> Puede descargar emuladores para exploradores y dispositivos móviles más conocidos, siga estos vínculos


| Dispositivo o explorador | Emulador o simulador |
| --- | --- |
| BrowserStack hospedado virtualización de explorador ![BrowserStack hospedado virtualización de explorador](device-simulators/_static/image1.png) | [Virtualización de BrowserStack hospedada en explorador](http://browserstack.com) probar su entorno local o de producción en cualquier explorador en cualquier plataforma. Puede crear un túnel entre su equipo y la red BrowserStack en su propia máquina virtual hospedada. Asegúrese de obtener el [extensión de Visual Studio BrowserStack](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) para una experiencia incluso más sencilla. |
| Windows Phone | [Descargas de Windows Phone SDK](https://dev.windowsphone.com/downloadsdk) Software Development Kit (SDK) de Windows Phone The incluye todas las herramientas que necesita para desarrollar aplicaciones y juegos para Windows Phone |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone y iPad simuladores para Windows, así como una capacidad de respuesta de herramienta de diseño. Puede integrar con la opción "Examinar con..." de VS 2012. |
| Android | [Página de inicio de Android SDK](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Las versiones más recientes: [Opera Developer Tools doméstica](http://www.opera.com/developer/tools/) Opera Mini 4.2: [simulador basado en Java en línea](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Kit de herramientas para desarrolladores de Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) tenga en cuenta que para conceder el acceso a la red de teléfono, también tiene el adaptador de red de VPC incluido en [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Para conectarse a Internet Explorer en el teléfono a su servidor de desarrollo de Visual Studio, consulte [entrada de blog de Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Imágenes del emulador de Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Tenga en cuenta que si desea ver la aplicación en un dispositivo móvil real (que es la única opción para realizar las pruebas completas iPhone o iPad, ya que no hay ningún verdadero emulador para Windows) necesitará hospedar su aplicación en IIS o IIS Express. No se puede usar fácilmente servidor web integrado de Visual Studio para ello, ya que no responde a solicitudes de otros equipos.
