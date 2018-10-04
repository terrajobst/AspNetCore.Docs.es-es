---
uid: webhooks/index
title: Información general sobre WebHooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Introducción a ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "48256497"
---
# <a name="aspnet-webhooks-overview"></a>Introducción a ASP.NET WebHooks

WebHooks es un patrón HTTP ligero que proporciona un modelo de pub/sub simple para el cableado juntos los servicios de SaaS y API Web. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.

Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios incluidos [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/)y mucho más. Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), o se ha confirmado un cambio de código en GitHub, o se ha iniciado un pago en [PayPal](http://www.paypal.com/), o se ha creado una tarjeta en [ Trello](http://www.trello.com/). Las posibilidades son infinitas.

Microsoft ASP.NET WebHooks hace más fácil de enviar y recibir WebHooks como parte de la aplicación ASP.NET:

* En el lado receptor, proporciona un modelo común para recibir y procesar WebHooks de cualquier número de proveedores de WebHook. Incluye de forma predetermina con compatibilidad para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) y [Zendesk](https://www.zendesk.com/) pero es fácil agregar compatibilidad para obtener más información.

* En el lado emisor proporciona soporte técnico para administrar y almacenar las suscripciones, así como para enviar notificaciones de eventos para el conjunto correcto de los suscriptores. Esto le permite definir su propio conjunto de eventos que pueden suscribirse a los suscriptores y se les notifiquen cuando las cosas se produce.

Las dos partes se pueden usar conjuntamente o diferencia según el escenario. Si solo tiene que recibir WebHooks de otros servicios, a continuación, puede usar solo la parte receptora; Si solo desea exponer WebHooks para que otros usuarios para consumir, puede hacer justamente eso.

El código tiene como destino ASP.NET Web API 2 y ASP.NET MVC 5 y está disponible como [OSS en GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Información general de WebHooks

WebHooks es un patrón que significa que varía de cómo se usa desde el servicio al servicio, pero la idea básica es la misma. Puede pensar WebHooks como un modelo de pub/sub simple donde un usuario puede suscribirse a eventos que ocurren en otros lugares. Se propagan las notificaciones de eventos como solicitudes HTTP POST que contiene información sobre el propio evento.

Normalmente, la solicitud HTTP POST contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook, incluida la información sobre el evento que provoca el WebHook para desencadenar. Por ejemplo, un ejemplo de un cuerpo de solicitud POST del WebHook de [GitHub](http://www.github.com/) tiene este aspecto como resultado un problema nuevo se abre en un repositorio determinado:

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

Para asegurarse de que el WebHook procede realmente del remitente, la solicitud POST se protegen de alguna manera y, a continuación, se comprobó el receptor. Por ejemplo, [WebHooks de GitHub](https://developer.github.com/webhooks/) incluye un *X-Hub-Signature* encabezado HTTP con un hash del cuerpo de la solicitud que se comprueba mediante la implementación del receptor para que no tenga que preocuparse.

Por lo general, el flujo de WebHook es algo como esto:

* El remitente WebHook expone eventos que se puede suscribir un cliente. Los eventos describen cambios observables en el sistema, por ejemplo que un nuevo elemento de datos se ha insertado, que se ha completado un proceso o alguna otra cosa.

* El receptor de WebHook se suscribe al registrar un WebHook que consta de cuatro elementos:

     1. Un URI de donde se debe registrar la notificación de eventos en forma de una solicitud HTTP POST;

     2. Un conjunto de filtros que describe los eventos concretos para el que se debe desencadenar el WebHook;

     3. Una clave secreta que se usa para firmar la solicitud HTTP POST;

     4. Datos adicionales que debe incluirse en la solicitud HTTP POST. Por ejemplo puede ser más campos de encabezado HTTP o las propiedades incluidas en el cuerpo de solicitud HTTP POST.

* Una vez que se produce un evento, se encuentran los registros coincidentes de WebHook y se envían las solicitudes HTTP POST. Normalmente, la generación de las solicitudes HTTP POST se reintentan varias veces si por alguna razón que el destinatario no responde o los resultados de la solicitud HTTP POST en una respuesta de error.

## <a name="webhooks-processing-pipeline"></a>Canalización de procesamiento de WebHooks

La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene este aspecto:

![Canalización de procesamiento de ASP.NET WebHooks](_static/WebHookReceivers.png)

Los conceptos clave dos son *receptores* y *controladores*:

* *Receptores* son responsables de para controlar el tipo de WebHook de un remitente determinado y para exigir comprobaciones de seguridad para asegurarse de que la solicitud de WebHook procede realmente del remitente.

* *Controladores* suelen ser donde ejecuta código de usuario, el WebHook determinado de procesamiento.

En los siguientes nodos estos conceptos se describen con más detalle.
