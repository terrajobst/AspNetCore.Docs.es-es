---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[¿Cómo lo hago?:] ¿Elegir entre métodos de AJAX de actualización de páginas? | Microsoft Docs'
author: JoeStagner
description: En este vídeo Joe Stagner compara los dos métodos principales de realización de actualizaciones de la página de estilo AJAX en una aplicación ASP.NET. El primer método consiste en usar un Upd...
ms.author: aspnetcontent
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 226fb0423ea05ad9034c909037358331918f2892
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838444"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[¿Cómo lo hago?:] ¿Elegir entre métodos de AJAX de actualización de páginas?
====================
por [Joe Stagner](https://github.com/JoeStagner)

En este vídeo Joe Stagner compara los dos métodos principales de realización de actualizaciones de la página de estilo AJAX en una aplicación ASP.NET. El primer método consiste en usar un control UpdatePanel, donde no debe escribirse en el lado del cliente o en el lado del servidor ningún código adicional. La ventaja de usar el control UpdatePanel es que todo funciona automáticamente. La penalización es en el cliente requiere una gran cantidad de datos que se incluirán en la solicitud de AJAX y la respuesta y en el servidor requiere un ciclo de vida de página completa que se ejecutará. El segundo método es usar devoluciones de llamada de red, donde se necesita código adicional se escriban en el lado del cliente y el lado del servidor. La ventaja de utilizar las devoluciones de llamada de red es que en el cliente requiere muy pocos datos para incluirse en la solicitud de AJAX y la respuesta, y en el servidor requiere solo el método de llamada de servicio que se ejecutará. El penality es el tiempo y esfuerzo necesario para escribir el código necesario. Joe concluye el vídeo con una descripción de lo debe considerar al elegir entre los dos métodos principales de las actualizaciones de página de estilo AJAX. (Este vídeo usa el código de la [cómo ¿empezar a usar ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) vídeo y la [¿cómo puedo realizar devoluciones de red del lado cliente con ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) vídeo.)

[&#9654;Vea el vídeo (11 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Anterior](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Siguiente](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
