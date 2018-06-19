---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[¿Cómo I:] Utilice la propiedad Reponse.Filter para reemplazar el código HTML en una página ASP.NET | Documentos de Microsoft'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo utilizar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página. En primer lugar, se crea una página de ejemplo w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525534"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[¿Cómo I:] Use la propiedad Reponse.Filter para reemplazar el código HTML en una página ASP.NET
====================
por [Chris Pels](https://twitter.com/chrispels)

En este vídeo Chris Pels muestra cómo utilizar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página. En primer lugar, se crea una página de ejemplo con algún texto simple. A continuación, se crea una clase de secuencia personalizada que actúa como la secuencia de reemplazo de la secuencia actual que se envían al explorador del usuario. En esa clase de secuencia personalizada se recupera el contenido de la página de la secuencia, modifica y, a continuación, escribe en la secuencia de respuesta. En esta clase de secuencia personalizada al método de escritura se ha personalizado para reemplazar el código HTML en la secuencia de respuesta base, con lo que modificar lo que se envía al explorador del usuario. Por último, la nueva clase de secuencia se asigna a la propiedad en la página\_cargar eventos, por lo tanto, proporciona el mecanismo para modificar el contenido de la página.

[&#9654; Vea el vídeo (13 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
