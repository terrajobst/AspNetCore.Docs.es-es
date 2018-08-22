---
uid: webhooks/receiving/receivers
title: Receptores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Receptores de WebHooks de ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828131"
---
# <a name="aspnet-webhooks-receivers"></a>Receptores de WebHooks de ASP.NET

Recibir WebHooks depende de quién es el remitente. A veces hay pasos adicionales que registrar un WebHook comprobando que el suscriptor está escuchando realmente. Algunos WebHooks proporcionan un modelo de inserción a extracción donde la solicitud HTTP POST solo contiene una referencia a la información de evento que se va a recuperar de forma independiente. A menudo el modelo de seguridad varía significativamente.

El propósito de WebHooks de ASP.NET de Microsoft es que resulte más sencillo y más coherente para conectar la API sin necesidad de dedicar mucho tiempo a pensar cómo controlar cualquier variante concreta de WebHooks.

Un receptor de WebHook es responsable de Aceptar y comprobar los WebHooks de un remitente determinado. Un receptor de WebHook puede admitir cualquier número de WebHooks, cada uno con su propia configuración. Por ejemplo, el receptor de WebHook de GitHub puede aceptar WebHooks de cualquier número de repositorios de GitHub.

## <a name="webhook-receiver-uris"></a>URI del receptor de WebHook

Instalando Microsoft ASP.NET WebHooks obtener un controlador de WebHook general que acepta solicitudes de WebHook de un número abierto de servicios. Cuando llega una solicitud, elige el receptor adecuado que haya instalado para el tratamiento de un remitente particular de WebHook.

El URI de este controlador es el URI de WebHook que se registra con el servicio y tiene el formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de seguridad, muchos de los destinatarios de WebHook requieren que el URI es un *https* URI y en algunos casos también debe contener un parámetro de consulta adicionales que se utiliza para exigir que solo la parte interesada puede enviar WebHooks al URI anterior .

El <em> <receiver> </em> componente es el nombre del receptor, por ejemplo <em>github</em> o <em>slack</em>.

El *{id}* es un identificador opcional que puede utilizarse para identificar una configuración determinada de receptor de WebHook. Esto se puede usar para registrar N WebHooks con un receptor en particular. Por ejemplo, los siguientes tres URI pueden utilizarse para registrarse en tres WebHooks independientes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalación de un receptor de WebHook

Para recibir WebHooks con Microsoft ASP.NET WebHooks, primero instalar el paquete Nuget para el proveedor de WebHook o proveedores que desea recibir WebHooks desde. Los paquetes de Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido. Por ejemplo

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de WebHooks de GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para la recepción de WebHooks generados por ASP. WebHooks de NET.

De fábrica puede obtener soporte técnico para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.

## <a name="configuring-a-webhook-receiver"></a>Configuración de un receptor de WebHook

Receptores de WebHook se configuran mediante el [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaz y ciertas implementaciones de dicha interfaz se pueden registrar con cualquier modelo de inyección de dependencia. La implementación predeterminada usa la configuración de la aplicación que puede establecerse en el archivo Web.config, o bien, si usa Azure Web Apps, se puede establecer a través de la [Portal de Azure](https://portal.azure.com/).

![Configuración de la aplicación de Azure](_static/AzureAppSettings.png)

El formato para las claves de configuración de la aplicación es como sigue:

```
MS_WebHookReceiverSecret_<receiver>
```

El valor es una lista separada por comas de valores que coinciden con el *{id}* valores para el que los WebHooks se han registrado, por ejemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializar un receptor de WebHook

Receptores de WebHook se inicializan registrándolos, normalmente en el *WebApiConfig* clase estática, por ejemplo:

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
