---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[¿Cómo lo hago?:]  Caché de una página ASP.NET en función de la información en el encabezado HTTP | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET en función de la información de encabezado HTTP de la página. En primer lugar, el potencial encabe HTTP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 3987e6ea1e5ea5575813bdf5598d0499ba37db20
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395258"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[¿Cómo lo hago?:]  Caché de una página ASP.NET en función de la información en el encabezado HTTP
====================
por [Chris Pels](https://twitter.com/chrispels)

En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET en función de la información de encabezado HTTP de la página. En primer lugar, se revisan los posibles valores del encabezado HTTP. A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "accept-language", un encabezado HTTP, para controlar el almacenamiento en caché según el idioma del explorador del usuario. La página de ejemplo se ve en Internet Explorer que se establece en inglés y, a continuación, en FireFox que está configurado para utilizar a francés. Por último, se describe la opción para mover la definición de la memoria caché para un CacheProfile en el archivo web.config.

[&#9654;Vea el vídeo (12 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
