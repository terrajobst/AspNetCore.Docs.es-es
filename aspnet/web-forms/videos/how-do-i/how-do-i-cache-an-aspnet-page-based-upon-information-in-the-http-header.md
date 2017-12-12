---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[¿Cómo I:]  Caché de una página ASP.NET basada en la información en el encabezado HTTP | Documentos de Microsoft"
author: rick-anderson
description: "En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET basada en información de encabezado HTTP de la página. En primer lugar, el potencial i HTTP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: fcb4b5e3b60bbcf3a0e5a95ff294bf41c7553a9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[¿Cómo I:]  Caché de una página ASP.NET basada en la información en el encabezado HTTP
====================
por [Chris Pels](https://twitter.com/chrispels)

En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET basada en información de encabezado HTTP de la página. En primer lugar, se revisan los posibles valores del encabezado HTTP. A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "accept-language", un encabezado HTTP, para controlar el almacenamiento en caché según el idioma del explorador del usuario. La página de ejemplos se ve en Internet Explorer que está establecida en inglés y, a continuación, en FireFox que está configurado para usar el francés. Por último, se explica la opción para mover la definición de la memoria caché para un CacheProfile en el archivo web.config.

[&#9654; Vea el vídeo (12 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
