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
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="91abc-103">Introducción a ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="91abc-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="91abc-104">WebHooks es un patrón HTTP ligero que proporciona un modelo de pub/sub simple para el cableado juntos los servicios de SaaS y API Web.</span><span class="sxs-lookup"><span data-stu-id="91abc-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="91abc-105">Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados.</span><span class="sxs-lookup"><span data-stu-id="91abc-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="91abc-106">La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="91abc-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="91abc-107">Debido a su simplicidad, WebHooks ya están expuestos por un gran número de servicios incluidos [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/)y mucho más.</span><span class="sxs-lookup"><span data-stu-id="91abc-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="91abc-108">Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en [Dropbox](http://dropbox.com/), o se ha confirmado un cambio de código en GitHub, o se ha iniciado un pago en [PayPal](http://www.paypal.com/), o se ha creado una tarjeta en [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="91abc-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="91abc-109">Las posibilidades son infinitas.</span><span class="sxs-lookup"><span data-stu-id="91abc-109">The possibilities are endless!</span></span>

<span data-ttu-id="91abc-110">Microsoft ASP.NET WebHooks hace más fácil de enviar y recibir WebHooks como parte de la aplicación ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="91abc-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="91abc-111">En el lado receptor, proporciona un modelo común para recibir y procesar WebHooks de cualquier número de proveedores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="91abc-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="91abc-112">Incluye de forma predetermina con compatibilidad para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [bandas](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) y [Zendesk](https://www.zendesk.com/) pero es fácil agregar compatibilidad para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="91abc-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="91abc-113">En el lado emisor proporciona soporte técnico para administrar y almacenar las suscripciones, así como para enviar notificaciones de eventos para el conjunto correcto de los suscriptores.</span><span class="sxs-lookup"><span data-stu-id="91abc-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="91abc-114">Esto le permite definir su propio conjunto de eventos que pueden suscribirse a los suscriptores y se les notifiquen cuando las cosas se produce.</span><span class="sxs-lookup"><span data-stu-id="91abc-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="91abc-115">Las dos partes se pueden usar conjuntamente o diferencia según el escenario.</span><span class="sxs-lookup"><span data-stu-id="91abc-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="91abc-116">Si solo tiene que recibir WebHooks de otros servicios, a continuación, puede usar solo la parte receptora; Si solo desea exponer WebHooks para que otros usuarios para consumir, puede hacer justamente eso.</span><span class="sxs-lookup"><span data-stu-id="91abc-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="91abc-117">El código tiene como destino ASP.NET Web API 2 y ASP.NET MVC 5 y está disponible como [OSS en GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="91abc-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="91abc-118">Información general de WebHooks</span><span class="sxs-lookup"><span data-stu-id="91abc-118">WebHooks Overview</span></span>

<span data-ttu-id="91abc-119">WebHooks es un patrón que significa que varía de cómo se usa desde el servicio al servicio, pero la idea básica es la misma.</span><span class="sxs-lookup"><span data-stu-id="91abc-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="91abc-120">Puede pensar WebHooks como un modelo de pub/sub simple donde un usuario puede suscribirse a eventos que ocurren en otros lugares.</span><span class="sxs-lookup"><span data-stu-id="91abc-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="91abc-121">Se propagan las notificaciones de eventos como solicitudes HTTP POST que contiene información sobre el propio evento.</span><span class="sxs-lookup"><span data-stu-id="91abc-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="91abc-122">Normalmente, la solicitud HTTP POST contiene un objeto JSON o datos de formulario HTML determinados por el remitente de WebHook, incluida la información sobre el evento que provoca el WebHook para desencadenar.</span><span class="sxs-lookup"><span data-stu-id="91abc-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="91abc-123">Por ejemplo, un ejemplo de un cuerpo de solicitud POST del WebHook de [GitHub](http://www.github.com/) tiene este aspecto como resultado un problema nuevo se abre en un repositorio determinado:</span><span class="sxs-lookup"><span data-stu-id="91abc-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="91abc-124">Para asegurarse de que el WebHook procede realmente del remitente, la solicitud POST se protegen de alguna manera y, a continuación, se comprobó el receptor.</span><span class="sxs-lookup"><span data-stu-id="91abc-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="91abc-125">Por ejemplo, [WebHooks de GitHub](https://developer.github.com/webhooks/) incluye un *X-Hub-Signature* encabezado HTTP con un hash del cuerpo de la solicitud que se comprueba mediante la implementación del receptor para que no tenga que preocuparse.</span><span class="sxs-lookup"><span data-stu-id="91abc-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="91abc-126">Por lo general, el flujo de WebHook es algo como esto:</span><span class="sxs-lookup"><span data-stu-id="91abc-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="91abc-127">El remitente WebHook expone eventos que se puede suscribir un cliente.</span><span class="sxs-lookup"><span data-stu-id="91abc-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="91abc-128">Los eventos describen cambios observables en el sistema, por ejemplo que un nuevo elemento de datos se ha insertado, que se ha completado un proceso o alguna otra cosa.</span><span class="sxs-lookup"><span data-stu-id="91abc-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="91abc-129">El receptor de WebHook se suscribe al registrar un WebHook que consta de cuatro elementos:</span><span class="sxs-lookup"><span data-stu-id="91abc-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="91abc-130">Un URI de donde se debe registrar la notificación de eventos en forma de una solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="91abc-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="91abc-131">Un conjunto de filtros que describe los eventos concretos para el que se debe desencadenar el WebHook;</span><span class="sxs-lookup"><span data-stu-id="91abc-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="91abc-132">Una clave secreta que se usa para firmar la solicitud HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="91abc-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="91abc-133">Datos adicionales que debe incluirse en la solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="91abc-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="91abc-134">Por ejemplo puede ser más campos de encabezado HTTP o las propiedades incluidas en el cuerpo de solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="91abc-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="91abc-135">Una vez que se produce un evento, se encuentran los registros coincidentes de WebHook y se envían las solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="91abc-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="91abc-136">Normalmente, la generación de las solicitudes HTTP POST se reintentan varias veces si por alguna razón que el destinatario no responde o los resultados de la solicitud HTTP POST en una respuesta de error.</span><span class="sxs-lookup"><span data-stu-id="91abc-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="91abc-137">Canalización de procesamiento de WebHooks</span><span class="sxs-lookup"><span data-stu-id="91abc-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="91abc-138">La canalización de procesamiento de Microsoft ASP.NET WebHooks para WebHooks entrantes tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="91abc-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Canalización de procesamiento de ASP.NET WebHooks](_static/WebHookReceivers.png)

<span data-ttu-id="91abc-140">Los conceptos clave dos son *receptores* y *controladores*:</span><span class="sxs-lookup"><span data-stu-id="91abc-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="91abc-141">*Receptores* son responsables de para controlar el tipo de WebHook de un remitente determinado y para exigir comprobaciones de seguridad para asegurarse de que la solicitud de WebHook procede realmente del remitente.</span><span class="sxs-lookup"><span data-stu-id="91abc-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="91abc-142">*Controladores* suelen ser donde ejecuta código de usuario, el WebHook determinado de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="91abc-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="91abc-143">En los siguientes nodos estos conceptos se describen con más detalle.</span><span class="sxs-lookup"><span data-stu-id="91abc-143">In the following nodes these concepts are described in more details.</span></span>
