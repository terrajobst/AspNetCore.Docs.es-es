---
uid: webhooks/source
title: Código fuente de ASP.NET WebHooks y paquetes de NuGet | Microsoft Docs
author: rick-anderson
description: Vínculos a código fuente de ASP.NET WebHooks y paquetes de NuGet
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 1ee4adc2a28054ed1f856d7e4e991b34972a70e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802188"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código fuente de ASP.NET WebHooks y paquetes de NuGet

WebHooks de ASP.NET de Microsoft forma parte de la familia Microsoft ASP.NET de módulos y está hospedado como un [Open Source Project en GitHub](https://github.com/aspnet/WebHooks). Esto significa que se aceptan contribuciones, pero eche un vistazo a la [directrices de contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de incorporación de cambios.

Esta documentación en línea que se está leyendo ahora también está hospedada como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta contribuciones.

## <a name="nuget-packages"></a>Paquetes NuGet

El [paquetes de NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un paquete común que se comparte entre remitentes y receptores.

* [Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un conjunto de paquetes que admiten enviar sus propios WebHooks a otros usuarios. La funcionalidad para enviar los WebHooks se describe con más detalle en [WebHooks enviar](sending/index.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un conjunto de paquetes que admiten reciban WebHooks de otros usuarios. La funcionalidad para recibir los WebHooks se describe con más detalle en [WebHooks recibir](receiving/index.md).
