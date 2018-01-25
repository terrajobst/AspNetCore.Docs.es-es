---
uid: webhooks/diagnostics/logging
title: Registro de Webhook de ASP.NET | Documentos de Microsoft
author: rick-anderson
description: "Cómo realizar el inicio de sesión ASP.NET Webhook."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="4534f-103">Registro de Webhook de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4534f-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="4534f-104">Microsoft ASP.NET WebHooks utiliza el registro como una manera de informar de problemas.</span><span class="sxs-lookup"><span data-stu-id="4534f-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="4534f-105">De forma predeterminada, los registros se escriben con [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) donde puede ser administradas usando [los agentes de escucha de seguimiento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como cualquier otra secuencia de registro.</span><span class="sxs-lookup"><span data-stu-id="4534f-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="4534f-106">Al implementar la aplicación Web como una aplicación Web de Azure, los registros se recogen automáticamente y pueden administrarse junto con cualquier otro [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registro.</span><span class="sxs-lookup"><span data-stu-id="4534f-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="4534f-107">Para obtener más información, vea [habilitar el registro de diagnósticos para aplicaciones web en el servicio de aplicaciones de Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="4534f-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="4534f-108">Además, se pueden obtener registros directamente desde dentro de Visual Studio como se describe en [solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="4534f-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="4534f-109">Redirección de registros</span><span class="sxs-lookup"><span data-stu-id="4534f-109">Redirecting Logs</span></span>

<span data-ttu-id="4534f-110">En lugar de escribir los registros para [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es posible proporcionar una implementación de registro alternativo que puede iniciar sesión directamente en un administrador de registro como [Log4Net](http://logging.apache.org/log4net/) y [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="4534f-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="4534f-111">Basta con proporcionar una implementación de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) y registrarlo con un motor de inyección de dependencia de su elección y se procesarán por Microsoft ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="4534f-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="4534f-112">Vea [inyección de dependencia en ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4534f-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
