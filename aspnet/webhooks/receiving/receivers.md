---
uid: webhooks/receiving/receivers
title: Receptores de ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: Receptores de Webhook de ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="d4da1-103">Receptores de Webhook de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4da1-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="d4da1-104">Recibir WebHooks depende de quién es el remitente.</span><span class="sxs-lookup"><span data-stu-id="d4da1-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="d4da1-105">A veces hay pasos adicionales registrando un WebHook de comprobar que realmente está escuchando el suscriptor.</span><span class="sxs-lookup"><span data-stu-id="d4da1-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="d4da1-106">Algunos WebHooks proporcionan un modelo de inserción y extracción donde la solicitud HTTP POST solo contiene una referencia a la información del evento que, a continuación, se usa para recuperarse de manera independiente.</span><span class="sxs-lookup"><span data-stu-id="d4da1-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="d4da1-107">A menudo el modelo de seguridad varía un poco.</span><span class="sxs-lookup"><span data-stu-id="d4da1-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="d4da1-108">El propósito de Microsoft ASP.NET WebHooks es para que sea más sencilla y más coherente para conectarlo su API sin dedicar mucho tiempo a pensar en cómo controlar cualquier variante particular de Webhook.</span><span class="sxs-lookup"><span data-stu-id="d4da1-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="d4da1-109">Un receptor de WebHook es responsable de Aceptar y verificar WebHooks de un remitente concreto.</span><span class="sxs-lookup"><span data-stu-id="d4da1-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="d4da1-110">Un receptor de WebHook puede tener cualquier número de Webhook, cada uno con su propia configuración.</span><span class="sxs-lookup"><span data-stu-id="d4da1-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="d4da1-111">Por ejemplo, el receptor de GitHub WebHook puede aceptar WebHooks de cualquier número de repositorios de GitHub.</span><span class="sxs-lookup"><span data-stu-id="d4da1-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="d4da1-112">URI de receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="d4da1-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="d4da1-113">Instalando Microsoft ASP.NET WebHooks obtener un controlador de WebHook general que acepta solicitudes de WebHook de un número de servicios abierto.</span><span class="sxs-lookup"><span data-stu-id="d4da1-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="d4da1-114">Cuando llega una solicitud, elige el receptor adecuado que haya instalado para el tratamiento de un remitente concreto de WebHook.</span><span class="sxs-lookup"><span data-stu-id="d4da1-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="d4da1-115">El URI de este controlador es el URI de WebHook que registrar con el servicio y tiene el formato:</span><span class="sxs-lookup"><span data-stu-id="d4da1-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="d4da1-116">Por motivos de seguridad, muchos receptores de WebHook requieren que el URI es un *https* URI y en algunos casos también debe contener un parámetro de consulta adicional que se utiliza para exigir que solo la parte interesada puede enviar WebHooks al URI anterior .</span><span class="sxs-lookup"><span data-stu-id="d4da1-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="d4da1-117">El <em> <receiver> </em> componente es el nombre del receptor, por ejemplo <em>github</em> o <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="d4da1-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="d4da1-118">El *{id}* es un identificador opcional que se puede usar para identificar una configuración específica de receptor de WebHook.</span><span class="sxs-lookup"><span data-stu-id="d4da1-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="d4da1-119">Esto se puede utilizar para registrar WebHooks N con un receptor concreto.</span><span class="sxs-lookup"><span data-stu-id="d4da1-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="d4da1-120">Por ejemplo, el URI de tres siguientes pueden utilizarse para registrar para tres WebHooks independientes:</span><span class="sxs-lookup"><span data-stu-id="d4da1-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="d4da1-121">Instalar a un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="d4da1-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="d4da1-122">Para recibir WebHooks mediante Microsoft ASP.NET WebHooks, primero instalar el paquete de Nuget para el proveedor de WebHook o el proveedor que desee recibir WebHooks desde.</span><span class="sxs-lookup"><span data-stu-id="d4da1-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="d4da1-123">Los paquetes de Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido.</span><span class="sxs-lookup"><span data-stu-id="d4da1-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="d4da1-124">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="d4da1-124">For example</span></span>

<span data-ttu-id="d4da1-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de Webhook de GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para la recepción de WebHooks generados por ASP. NET Webhook.</span><span class="sxs-lookup"><span data-stu-id="d4da1-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="d4da1-126">De fábrica encontrará soporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, demora, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="d4da1-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="d4da1-127">Configurar un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="d4da1-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="d4da1-128">Receptores de WebHook se configuran a través de la [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaz y las implementaciones determinadas de esa interfaz se pueden registrar con cualquier modelo de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="d4da1-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="d4da1-129">La implementación predeterminada usa la configuración de la aplicación que puede establecerse en el archivo Web.config, o bien, si usa aplicaciones Web de Azure, se puede establecer a través de la [Portal de Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d4da1-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configuración de la aplicación de Azure](_static/AzureAppSettings.png)

<span data-ttu-id="d4da1-131">El formato para las claves de configuración de la aplicación es como sigue:</span><span class="sxs-lookup"><span data-stu-id="d4da1-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="d4da1-132">El valor es una lista separada por comas de valores que coinciden con la *{id}* valores para el que se registraron WebHooks, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d4da1-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="d4da1-133">Inicializar un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="d4da1-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="d4da1-134">Receptores de WebHook se inicializan mediante el registro, normalmente en el *WebApiConfig* estático de la clase, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d4da1-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
