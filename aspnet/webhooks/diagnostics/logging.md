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
# <a name="aspnet-webhooks-logging"></a>Registro de Webhook de ASP.NET

Microsoft ASP.NET WebHooks utiliza el registro como una manera de informar de problemas. De forma predeterminada, los registros se escriben con [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) donde puede ser administradas usando [los agentes de escucha de seguimiento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como cualquier otra secuencia de registro.

Al implementar la aplicación Web como una aplicación Web de Azure, los registros se recogen automáticamente y pueden administrarse junto con cualquier otro [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registro. Para obtener más información, vea [habilitar el registro de diagnósticos para aplicaciones web en el servicio de aplicaciones de Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Además, se pueden obtener registros directamente desde dentro de Visual Studio como se describe en [solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirección de registros

En lugar de escribir los registros para [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es posible proporcionar una implementación de registro alternativo que puede iniciar sesión directamente en un administrador de registro como [Log4Net](http://logging.apache.org/log4net/) y [NLog ](http://nlog-project.org/). Basta con proporcionar una implementación de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) y registrarlo con un motor de inyección de dependencia de su elección y se procesarán por Microsoft ASP.NET WebHooks. Vea [inyección de dependencia en ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obtener más información.
