---
uid: webhooks/receiving/receivers
title: Receptores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Receptores de WebHooks de ASP.NET
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: b4b995d5d781576b2b22db51d78e0e303bfdccc4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812150"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="ccd6e-103">Receptores de WebHooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ccd6e-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="ccd6e-104">Recibir WebHooks depende de quién es el remitente.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="ccd6e-105">A veces hay pasos adicionales que registrar un WebHook comprobando que el suscriptor está escuchando realmente.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="ccd6e-106">Algunos WebHooks proporcionan un modelo de inserción a extracción donde la solicitud HTTP POST solo contiene una referencia a la información de evento que se va a recuperar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="ccd6e-107">A menudo el modelo de seguridad varía significativamente.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="ccd6e-108">El propósito de WebHooks de ASP.NET de Microsoft es que resulte más sencillo y más coherente para conectar la API sin necesidad de dedicar mucho tiempo a pensar cómo controlar cualquier variante concreta de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="ccd6e-109">Un receptor de WebHook es responsable de Aceptar y comprobar los WebHooks de un remitente determinado.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="ccd6e-110">Un receptor de WebHook puede admitir cualquier número de WebHooks, cada uno con su propia configuración.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="ccd6e-111">Por ejemplo, el receptor de WebHook de GitHub puede aceptar WebHooks de cualquier número de repositorios de GitHub.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="ccd6e-112">URI del receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="ccd6e-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="ccd6e-113">Instalando Microsoft ASP.NET WebHooks obtener un controlador de WebHook general que acepta solicitudes de WebHook de un número abierto de servicios.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="ccd6e-114">Cuando llega una solicitud, elige el receptor adecuado que haya instalado para el tratamiento de un remitente particular de WebHook.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="ccd6e-115">El URI de este controlador es el URI de WebHook que se registra con el servicio y tiene el formato:</span><span class="sxs-lookup"><span data-stu-id="ccd6e-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="ccd6e-116">Por motivos de seguridad, muchos de los destinatarios de WebHook requieren que el URI es un *https* URI y en algunos casos también debe contener un parámetro de consulta adicionales que se utiliza para exigir que solo la parte interesada puede enviar WebHooks al URI anterior .</span><span class="sxs-lookup"><span data-stu-id="ccd6e-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="ccd6e-117">El <em> <receiver> </em> componente es el nombre del receptor, por ejemplo <em>github</em> o <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="ccd6e-118">El *{id}* es un identificador opcional que puede utilizarse para identificar una configuración determinada de receptor de WebHook.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="ccd6e-119">Esto se puede usar para registrar N WebHooks con un receptor en particular.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="ccd6e-120">Por ejemplo, los siguientes tres URI pueden utilizarse para registrarse en tres WebHooks independientes:</span><span class="sxs-lookup"><span data-stu-id="ccd6e-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="ccd6e-121">Instalación de un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="ccd6e-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="ccd6e-122">Para recibir WebHooks con Microsoft ASP.NET WebHooks, primero instalar el paquete Nuget para el proveedor de WebHook o proveedores que desea recibir WebHooks desde.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="ccd6e-123">Los paquetes de Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="ccd6e-124">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="ccd6e-124">For example</span></span>

<span data-ttu-id="ccd6e-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de WebHooks de GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para la recepción de WebHooks generados por ASP. WebHooks de NET.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="ccd6e-126">De fábrica puede obtener soporte técnico para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="ccd6e-127">Configuración de un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="ccd6e-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="ccd6e-128">Receptores de WebHook se configuran mediante el [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaz y ciertas implementaciones de dicha interfaz se pueden registrar con cualquier modelo de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="ccd6e-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="ccd6e-129">La implementación predeterminada usa la configuración de la aplicación que puede establecerse en el archivo Web.config, o bien, si usa Azure Web Apps, se puede establecer a través de la [Portal de Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ccd6e-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configuración de la aplicación de Azure](_static/AzureAppSettings.png)

<span data-ttu-id="ccd6e-131">El formato para las claves de configuración de la aplicación es como sigue:</span><span class="sxs-lookup"><span data-stu-id="ccd6e-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="ccd6e-132">El valor es una lista separada por comas de valores que coinciden con el *{id}* valores para el que los WebHooks se han registrado, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ccd6e-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="ccd6e-133">Inicializar un receptor de WebHook</span><span class="sxs-lookup"><span data-stu-id="ccd6e-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="ccd6e-134">Receptores de WebHook se inicializan registrándolos, normalmente en el *WebApiConfig* clase estática, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ccd6e-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
