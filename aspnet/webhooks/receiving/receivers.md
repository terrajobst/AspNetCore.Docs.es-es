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
ms.locfileid: "30896301"
---
# <a name="aspnet-webhooks-receivers"></a>Receptores de Webhook de ASP.NET

Recibir WebHooks depende de quién es el remitente. A veces hay pasos adicionales registrando un WebHook de comprobar que realmente está escuchando el suscriptor. Algunos WebHooks proporcionan un modelo de inserción y extracción donde la solicitud HTTP POST solo contiene una referencia a la información del evento que, a continuación, se usa para recuperarse de manera independiente. A menudo el modelo de seguridad varía un poco.

El propósito de Microsoft ASP.NET WebHooks es para que sea más sencilla y más coherente para conectarlo su API sin dedicar mucho tiempo a pensar en cómo controlar cualquier variante particular de Webhook.

Un receptor de WebHook es responsable de Aceptar y verificar WebHooks de un remitente concreto. Un receptor de WebHook puede tener cualquier número de Webhook, cada uno con su propia configuración. Por ejemplo, el receptor de GitHub WebHook puede aceptar WebHooks de cualquier número de repositorios de GitHub.

## <a name="webhook-receiver-uris"></a>URI de receptor de WebHook

Instalando Microsoft ASP.NET WebHooks obtener un controlador de WebHook general que acepta solicitudes de WebHook de un número de servicios abierto. Cuando llega una solicitud, elige el receptor adecuado que haya instalado para el tratamiento de un remitente concreto de WebHook.

El URI de este controlador es el URI de WebHook que registrar con el servicio y tiene el formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de seguridad, muchos receptores de WebHook requieren que el URI es un *https* URI y en algunos casos también debe contener un parámetro de consulta adicional que se utiliza para exigir que solo la parte interesada puede enviar WebHooks al URI anterior .

El <em> <receiver> </em> componente es el nombre del receptor, por ejemplo <em>github</em> o <em>slack</em>.

El *{id}* es un identificador opcional que se puede usar para identificar una configuración específica de receptor de WebHook. Esto se puede utilizar para registrar WebHooks N con un receptor concreto. Por ejemplo, el URI de tres siguientes pueden utilizarse para registrar para tres WebHooks independientes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalar a un receptor de WebHook

Para recibir WebHooks mediante Microsoft ASP.NET WebHooks, primero instalar el paquete de Nuget para el proveedor de WebHook o el proveedor que desee recibir WebHooks desde. Los paquetes de Nuget se denominan [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) donde la última parte indica el servicio admitido. Por ejemplo

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) proporciona compatibilidad para la recepción de Webhook de GitHub y [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) proporciona compatibilidad para la recepción de WebHooks generados por ASP. NET Webhook.

De fábrica encontrará soporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, demora, Stripe, Trello y WordPress, pero es posible admitir cualquier número de otros proveedores.

## <a name="configuring-a-webhook-receiver"></a>Configurar un receptor de WebHook

Receptores de WebHook se configuran a través de la [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfaz y las implementaciones determinadas de esa interfaz se pueden registrar con cualquier modelo de inyección de dependencia. La implementación predeterminada usa la configuración de la aplicación que puede establecerse en el archivo Web.config, o bien, si usa aplicaciones Web de Azure, se puede establecer a través de la [Portal de Azure](https://portal.azure.com/).

![Configuración de la aplicación de Azure](_static/AzureAppSettings.png)

El formato para las claves de configuración de la aplicación es como sigue:

```
MS_WebHookReceiverSecret_<receiver>
```

El valor es una lista separada por comas de valores que coinciden con la *{id}* valores para el que se registraron WebHooks, por ejemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializar un receptor de WebHook

Receptores de WebHook se inicializan mediante el registro, normalmente en el *WebApiConfig* estático de la clase, por ejemplo:

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
