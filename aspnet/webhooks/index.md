---
uid: webhooks/index
title: Introducción a ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: Introducción a ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530054"
---
# <a name="aspnet-webhooks-overview"></a>Introducción a ASP.NET Webhook

Webhook es un patrón HTTP ligero proporcionando un modelo de pub/sub simple para el cableado juntos los servicios API Web y SaaS. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.

Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios que incluyen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/)y mucho más. Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), un cambio en el código se ha confirmado en GitHub o un pago se ha iniciado en [PayPal](http://www.paypal.com/), o se ha creado una tarjeta en [ Trello](http://www.trello.com/). ¡Las posibilidades son ilimitadas!

Microsoft ASP.NET WebHooks resulta más sencillo enviar y recibir WebHooks como parte de la aplicación de ASP.NET:

* En el lado receptor, proporciona un modelo común para recibir y procesar WebHooks de cualquier número de proveedores de WebHook. Se trata de fábrica con compatibilidad para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) y [Zendesk](https://www.zendesk.com/) pero resulta muy sencillo agregar soporte técnico para obtener más información.

* En el lado emisor proporciona la compatibilidad para administrar y almacenar las suscripciones, así como para enviar notificaciones de eventos para el conjunto adecuado de los suscriptores. Esto le permite definir su propio conjunto de eventos que los suscriptores pueden suscribirse a y les avise cuando ocurre cosas.

Las dos partes pueden usarse juntos o alejados según el escenario. Si solo necesita recibir WebHooks de otros servicios, a continuación, puede usar solo la parte receptora; Si solo desea exponer WebHooks para que otros usuarios para consumir, puede hacer exactamente eso.

El código destinado a ASP.NET Web API 2 y 5 de MVC de ASP.NET y está disponible como [OSS en GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Información general de Webhook

Webhook es un patrón que significa que varía cómo usarlo de servicio al servicio, pero la idea básica es la misma. Se puede considerar WebHooks como un modelo simple pub/sub donde un usuario puede suscribirse a eventos que sucede en otro lugar. Las notificaciones de eventos se propagan como solicitudes HTTP POST que contiene información sobre el evento.

Normalmente, la solicitud POST de HTTP contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook incluye información sobre el evento que provoca el WebHook para desencadenar. Por ejemplo, un ejemplo de un cuerpo de la solicitud POST de WebHook de [GitHub](http://www.github.com/) tiene este aspecto como resultado de un problema nuevo se abre en un repositorio determinado:

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

Para asegurarse de que el WebHook procede realmente del remitente, la solicitud POST se protegen de alguna manera y, a continuación, comprueba el receptor. Por ejemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) incluye un *X-concentrador-Signature* encabezado HTTP con un hash del cuerpo de la solicitud que se comprueba mediante la implementación del receptor para no tener que preocuparse sobre él.

Por lo general, el flujo de WebHook pasa algo parecido a esto:

* El remitente de WebHook expone eventos que se puede suscribir un cliente. Los eventos describan observables cambios en el sistema, por ejemplo un nuevo elemento de datos se ha insertado, que se ha completado un proceso u otra cosa.

* El receptor de WebHook se suscribe al registrar un WebHook que consta de cuatro aspectos:

     1. Un URI de dónde se debe registrar la notificación de eventos en forma de una solicitud HTTP POST;

     2. Un conjunto de filtros que describe los eventos particulares que debe activarse el WebHook;

     3. Una clave secreta que se utiliza para firmar la solicitud HTTP POST;

     4. Datos adicionales que debe incluirse en la solicitud HTTP POST. Por ejemplo, esto puede ser campos de encabezado HTTP adicionales o propiedades incluidas en el cuerpo de la solicitud HTTP POST.

* Una vez que se produce un evento, se encuentran los registros coincidentes de WebHook y se envían las solicitudes HTTP POST. Normalmente, la generación de las solicitudes HTTP POST se hizo varios reintentos si por alguna razón que el destinatario no responde o los resultados de la solicitud HTTP POST en una respuesta de error.

## <a name="webhooks-processing-pipeline"></a>Canalización de procesamiento de Webhook

La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene el siguiente aspecto:

![Canalización de procesamiento de Webhook de ASP.NET](_static/WebHookReceivers.png)

Los conceptos clave dos son *receptores* y *controladores*:

* *Receptores* es responsable de controlar el tipo concreto de WebHook de un remitente determinado y exigir comprobaciones de seguridad para asegurarse de que la solicitud de WebHook procede realmente del remitente.

* *Controladores* suelen ser donde ejecuta el código de usuario, procesar el WebHook determinado.

En los siguientes nodos estos conceptos se describen con más detalle.
