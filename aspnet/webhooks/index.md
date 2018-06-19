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
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="b84ea-103">Introducción a ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="b84ea-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="b84ea-104">Webhook es un patrón HTTP ligero proporcionando un modelo de pub/sub simple para el cableado juntos los servicios API Web y SaaS.</span><span class="sxs-lookup"><span data-stu-id="b84ea-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="b84ea-105">Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados.</span><span class="sxs-lookup"><span data-stu-id="b84ea-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="b84ea-106">La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="b84ea-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="b84ea-107">Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios que incluyen [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/)y mucho más.</span><span class="sxs-lookup"><span data-stu-id="b84ea-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="b84ea-108">Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), un cambio en el código se ha confirmado en GitHub o un pago se ha iniciado en [PayPal](http://www.paypal.com/), o se ha creado una tarjeta en [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="b84ea-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="b84ea-109">¡Las posibilidades son ilimitadas!</span><span class="sxs-lookup"><span data-stu-id="b84ea-109">The possibilities are endless!</span></span>

<span data-ttu-id="b84ea-110">Microsoft ASP.NET WebHooks resulta más sencillo enviar y recibir WebHooks como parte de la aplicación de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b84ea-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="b84ea-111">En el lado receptor, proporciona un modelo común para recibir y procesar WebHooks de cualquier número de proveedores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="b84ea-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="b84ea-112">Se trata de fábrica con compatibilidad para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) y [Zendesk](https://www.zendesk.com/) pero resulta muy sencillo agregar soporte técnico para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b84ea-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="b84ea-113">En el lado emisor proporciona la compatibilidad para administrar y almacenar las suscripciones, así como para enviar notificaciones de eventos para el conjunto adecuado de los suscriptores.</span><span class="sxs-lookup"><span data-stu-id="b84ea-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="b84ea-114">Esto le permite definir su propio conjunto de eventos que los suscriptores pueden suscribirse a y les avise cuando ocurre cosas.</span><span class="sxs-lookup"><span data-stu-id="b84ea-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="b84ea-115">Las dos partes pueden usarse juntos o alejados según el escenario.</span><span class="sxs-lookup"><span data-stu-id="b84ea-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="b84ea-116">Si solo necesita recibir WebHooks de otros servicios, a continuación, puede usar solo la parte receptora; Si solo desea exponer WebHooks para que otros usuarios para consumir, puede hacer exactamente eso.</span><span class="sxs-lookup"><span data-stu-id="b84ea-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="b84ea-117">El código destinado a ASP.NET Web API 2 y 5 de MVC de ASP.NET y está disponible como [OSS en GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="b84ea-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="b84ea-118">Información general de Webhook</span><span class="sxs-lookup"><span data-stu-id="b84ea-118">WebHooks Overview</span></span>

<span data-ttu-id="b84ea-119">Webhook es un patrón que significa que varía cómo usarlo de servicio al servicio, pero la idea básica es la misma.</span><span class="sxs-lookup"><span data-stu-id="b84ea-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="b84ea-120">Se puede considerar WebHooks como un modelo simple pub/sub donde un usuario puede suscribirse a eventos que sucede en otro lugar.</span><span class="sxs-lookup"><span data-stu-id="b84ea-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="b84ea-121">Las notificaciones de eventos se propagan como solicitudes HTTP POST que contiene información sobre el evento.</span><span class="sxs-lookup"><span data-stu-id="b84ea-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="b84ea-122">Normalmente, la solicitud POST de HTTP contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook incluye información sobre el evento que provoca el WebHook para desencadenar.</span><span class="sxs-lookup"><span data-stu-id="b84ea-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="b84ea-123">Por ejemplo, un ejemplo de un cuerpo de la solicitud POST de WebHook de [GitHub](http://www.github.com/) tiene este aspecto como resultado de un problema nuevo se abre en un repositorio determinado:</span><span class="sxs-lookup"><span data-stu-id="b84ea-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="b84ea-124">Para asegurarse de que el WebHook procede realmente del remitente, la solicitud POST se protegen de alguna manera y, a continuación, comprueba el receptor.</span><span class="sxs-lookup"><span data-stu-id="b84ea-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="b84ea-125">Por ejemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) incluye un *X-concentrador-Signature* encabezado HTTP con un hash del cuerpo de la solicitud que se comprueba mediante la implementación del receptor para no tener que preocuparse sobre él.</span><span class="sxs-lookup"><span data-stu-id="b84ea-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="b84ea-126">Por lo general, el flujo de WebHook pasa algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="b84ea-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="b84ea-127">El remitente de WebHook expone eventos que se puede suscribir un cliente.</span><span class="sxs-lookup"><span data-stu-id="b84ea-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="b84ea-128">Los eventos describan observables cambios en el sistema, por ejemplo un nuevo elemento de datos se ha insertado, que se ha completado un proceso u otra cosa.</span><span class="sxs-lookup"><span data-stu-id="b84ea-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="b84ea-129">El receptor de WebHook se suscribe al registrar un WebHook que consta de cuatro aspectos:</span><span class="sxs-lookup"><span data-stu-id="b84ea-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="b84ea-130">Un URI de dónde se debe registrar la notificación de eventos en forma de una solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="b84ea-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="b84ea-131">Un conjunto de filtros que describe los eventos particulares que debe activarse el WebHook;</span><span class="sxs-lookup"><span data-stu-id="b84ea-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="b84ea-132">Una clave secreta que se utiliza para firmar la solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="b84ea-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="b84ea-133">Datos adicionales que debe incluirse en la solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b84ea-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="b84ea-134">Por ejemplo, esto puede ser campos de encabezado HTTP adicionales o propiedades incluidas en el cuerpo de la solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b84ea-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="b84ea-135">Una vez que se produce un evento, se encuentran los registros coincidentes de WebHook y se envían las solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b84ea-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="b84ea-136">Normalmente, la generación de las solicitudes HTTP POST se hizo varios reintentos si por alguna razón que el destinatario no responde o los resultados de la solicitud HTTP POST en una respuesta de error.</span><span class="sxs-lookup"><span data-stu-id="b84ea-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="b84ea-137">Canalización de procesamiento de Webhook</span><span class="sxs-lookup"><span data-stu-id="b84ea-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="b84ea-138">La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="b84ea-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Canalización de procesamiento de Webhook de ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="b84ea-140">Los conceptos clave dos son *receptores* y *controladores*:</span><span class="sxs-lookup"><span data-stu-id="b84ea-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="b84ea-141">*Receptores* es responsable de controlar el tipo concreto de WebHook de un remitente determinado y exigir comprobaciones de seguridad para asegurarse de que la solicitud de WebHook procede realmente del remitente.</span><span class="sxs-lookup"><span data-stu-id="b84ea-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="b84ea-142">*Controladores* suelen ser donde ejecuta el código de usuario, procesar el WebHook determinado.</span><span class="sxs-lookup"><span data-stu-id="b84ea-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="b84ea-143">En los siguientes nodos estos conceptos se describen con más detalle.</span><span class="sxs-lookup"><span data-stu-id="b84ea-143">In the following nodes these concepts are described in more details.</span></span>
