---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: "[¿Cómo I:] ¿Elegir entre métodos de AJAX página actualizaciones? | Microsoft Docs"
author: JoeStagner
description: "En este vídeo Joe Stagner compara los dos métodos principales de realización de actualizaciones de la página de estilo AJAX en una aplicación ASP.NET. El primer método consiste en usar un Upd..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: cc3721c24c9fed0cb028d755330a5c6189b613b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[¿Cómo I:] ¿Elegir entre métodos de AJAX página actualizaciones?
====================
por [Joe Stagner](https://github.com/JoeStagner)

En este vídeo Joe Stagner compara los dos métodos principales de realización de actualizaciones de la página de estilo AJAX en una aplicación ASP.NET. El primer método consiste en usar un control UpdatePanel, donde es necesario ningún código adicional para escribirse en el lado del cliente o en el lado del servidor. La ventaja de usar el control UpdatePanel es que todo funciona automáticamente. La penalización es en el cliente requiere una gran cantidad de datos que se incluirá en la solicitud de AJAX y respuesta y en el servidor requiere un ciclo de vida de la página completa que se ejecute. El segundo método consiste en usar devoluciones de llamada de red, donde se necesita código adicional se escriban en el lado del cliente y el lado del servidor. La ventaja de utilizar las devoluciones de llamada de red es que en el cliente requiere que los datos muy poco que se incluirá en la solicitud de AJAX y respuesta, y en el servidor requiere solo el método de llamada de servicio que se ejecute. El penality es el tiempo y el esfuerzo necesario para escribir el código necesario. Juan finaliza el vídeo con una descripción de lo debe considerar al elegir entre los dos métodos principales de las actualizaciones de página de estilo AJAX. (Este vídeo usa el código de la [¿cómo comenzar con ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) vídeo y [¿cómo puedo realizar cliente red las devoluciones de llamada con ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) vídeo.)

[&#9654; Vea el vídeo (11 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

>[!div class="step-by-step"]
[Anterior](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Siguiente](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
