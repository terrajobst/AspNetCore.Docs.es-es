---
uid: webhooks/diagnostics/logging
title: Registro de WebHooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Cómo iniciar sesión en ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 65e4d49474034406be835eb31378c81ba0706da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828460"
---
# <a name="aspnet-webhooks-logging"></a>Registro de WebHooks de ASP.NET

Microsoft ASP.NET WebHooks utiliza el registro como una manera de informar de problemas. De forma predeterminada, los registros se escriben con [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) donde pueden ser administradas utilizando [los agentes de escucha de seguimiento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como cualquier otra secuencia de registro.

Al implementar la aplicación Web como aplicación Web de Azure, los registros se recogen automáticamente y pueden administrarse junto con cualquier otro [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registro. Para obtener más información, consulte [habilitar el registro de diagnóstico para aplicaciones web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Además, se pueden obtener los registros directamente desde dentro de Visual Studio como se describe en [solucionar problemas de una aplicación web en Azure App Service mediante Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirigir los registros

En lugar de escribir los registros a [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es posible proporcionar una implementación alternativa del registro que puede iniciar directamente a un administrador de registros como [Log4Net](http://logging.apache.org/log4net/) y [NLog ](http://nlog-project.org/). Basta con proporcionar una implementación de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) y registrarlo en un motor de inyección de dependencia de su elección y se elegirán por Microsoft ASP.NET WebHooks. Consulte [inserción de dependencias en ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obtener más información.
