---
uid: web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
title: '[¿Cómo I:] Implementar el control de errores al enviar correo electrónico con ASP.NET | Documentos de Microsoft'
author: rick-anderson
description: Chris Pels muestra cómo implementar el control de errores al enviar un correo electrónico con ASP.NET. Crea una página web ASP.NET para enviar correo electrónico, se muestra cómo configurar & lt....
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/06/2008
ms.topic: article
ms.assetid: c02ffd50-aa19-4cdc-b1bf-760989979a61
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
msc.type: video
ms.openlocfilehash: a860eb958956bcac1682e8be7d4e70a9d44b2c4d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26526434"
---
<a name="how-do-i-implement-error-handling-when-sending-email-with-aspnet"></a><span data-ttu-id="6faeb-104">[¿Cómo I:] Implementar el control de errores al enviar correo electrónico con ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6faeb-104">[How Do I:] Implement Error Handling when Sending Email with ASP.NET</span></span>
====================
<span data-ttu-id="6faeb-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="6faeb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="6faeb-106">Chris Pels muestra cómo implementar el control de errores al enviar un correo electrónico con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6faeb-106">Chris Pels shows how to implement error handling when sending an email with ASP.NET.</span></span> <span data-ttu-id="6faeb-107">Se crea una página web ASP.NET para enviar correo electrónico, se muestra cómo configurar &lt;mailSettings&gt; en el archivo web.config, se describe la clase System.Net.Mail y cómo se utiliza para crear y enviar mensajes de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="6faeb-107">He creates an ASP.NET web page to send email, shows how to configure &lt;mailSettings&gt; in the web.config file, describes the System.Net.Mail class and how it's used to create and send email messages.</span></span> <span data-ttu-id="6faeb-108">A continuación, agrega el control de errores mediante las clases de excepción System.Net.Mail, que proporcionan información sobre errores que pueden producirse al enviar correo electrónico y revisan la enumeración SmtpStatusCode, que proporciona una lista de resultados posibles al enviar un correo electrónico con el SmtpClient.</span><span class="sxs-lookup"><span data-stu-id="6faeb-108">He then adds error handling using System.Net.Mail exception classes, which provide information about errors that can occur when sending email, and reviews the SmtpStatusCode enumeration, which provides a list of possible outcomes when sending an email with the SmtpClient.</span></span> <span data-ttu-id="6faeb-109">Por último, y lo envía un correo electrónico de prueba que genera una excepción y revisa la información en el depurador de Visual Studio de control de errores.</span><span class="sxs-lookup"><span data-stu-id="6faeb-109">Finally, he sends a test email that raises an exception and reviews the error handling information in the Visual Studio debugger.</span></span>

[<span data-ttu-id="6faeb-110">&#9654; Vea el vídeo (24 minutos)</span><span class="sxs-lookup"><span data-stu-id="6faeb-110">&#9654; Watch video (24 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-error-handling-when-sending-email-with-aspnet)
