---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[¿Cómo lo hago?:] Utilice la propiedad Reponse.Filter para reemplazar el HTML en una página ASP.NET | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo usar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página. En primer lugar, se crea una página de ejemplo w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 0e8ae8b62bbb4ac1fc7e942cf3dd0438383cb510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829377"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[¿Cómo lo hago?:] Utilice la propiedad Reponse.Filter para reemplazar el HTML en una página ASP.NET
====================
por [Chris Pels](https://twitter.com/chrispels)

En este vídeo Chris Pels muestra cómo usar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página. En primer lugar, se crea una página de ejemplo con algún texto simple. A continuación, se crea una clase de Stream personalizada que actúa como la secuencia de reemplazo de la secuencia actual que se envían al explorador del usuario. En esa clase de secuencia personalizada se recupera el contenido de la página de la secuencia, modificar y, a continuación, escribe en la secuencia de respuesta. En esta clase Stream personalizada se personaliza el método Write para reemplazar el código HTML en la secuencia de respuesta base, modificar, por tanto, lo que se envía al explorador del usuario. Por último, la nueva clase de secuencia se asigna a la propiedad Response.Filter en la página\_cargar eventos, por lo tanto, lo que proporciona el mecanismo para modificar el contenido de la página.

[&#9654;Vea el vídeo (13 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
