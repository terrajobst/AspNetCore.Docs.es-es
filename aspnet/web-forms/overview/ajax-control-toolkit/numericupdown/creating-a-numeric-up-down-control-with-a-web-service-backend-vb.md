---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Crear un valor numérico arriba/abajo Control con un back-end de servicio de Web (VB) | Documentos de Microsoft
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla de verificación, numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar c como más...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 690fd89c552407ec5d77419aae2488e4832efe44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="83fea-103">Creación de un número hacia arriba o hacia abajo el Control con un back-end de servicio de Web (VB)</span><span class="sxs-lookup"><span data-stu-id="83fea-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="83fea-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83fea-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83fea-105">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="83fea-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="83fea-106">En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar como más cómodo.</span><span class="sxs-lookup"><span data-stu-id="83fea-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="83fea-107">De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.</span><span class="sxs-lookup"><span data-stu-id="83fea-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="83fea-108">Información general</span><span class="sxs-lookup"><span data-stu-id="83fea-108">Overview</span></span>

<span data-ttu-id="83fea-109">En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar como más cómodo.</span><span class="sxs-lookup"><span data-stu-id="83fea-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="83fea-110">De forma predeterminada, el `NumericUpDown` control siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.</span><span class="sxs-lookup"><span data-stu-id="83fea-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="83fea-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="83fea-111">Steps</span></span>

<span data-ttu-id="83fea-112">El Kit de herramientas de Control de AJAX de ASP.NET contiene el `NumericUpDown` extensor que agrega automáticamente dos botones a un cuadro de texto: uno para aumentar su valor, uno para disminuirlo.</span><span class="sxs-lookup"><span data-stu-id="83fea-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="83fea-113">Sin embargo, el control también admite una llamada al servicio web (o la llamada al método de página).</span><span class="sxs-lookup"><span data-stu-id="83fea-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="83fea-114">Cada vez que el hacia arriba o hacia abajo del botón se hace clic, el código de JavaScript código se conecta al servidor web y ejecuta un método no existe.</span><span class="sxs-lookup"><span data-stu-id="83fea-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="83fea-115">La firma del método es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="83fea-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="83fea-116">El `current` argumento es el valor actual en el cuadro de texto; el `tag` atributo es datos de contexto adicional que se pueden establecer como una propiedad de la `NumericUpDown` extender (pero no es necesario).</span><span class="sxs-lookup"><span data-stu-id="83fea-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="83fea-117">En este ejemplo, el valor numérico arriba/abajo control solo debe permitir valores que son potencias de dos: 1, 2, 4, 8, 16, 32, 64 y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="83fea-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="83fea-118">Por lo tanto, el método que se ejecuta cuando el usuario desea aumentar el valor debe duplicar el valor antiguo; el otro método debe dividir el valor por dos.</span><span class="sxs-lookup"><span data-stu-id="83fea-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="83fea-119">Por lo que este es el servicio web completa:</span><span class="sxs-lookup"><span data-stu-id="83fea-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="83fea-120">Finalmente, cree una nueva página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83fea-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="83fea-121">Como es habitual, necesita un `ScriptManager` (control), un `TextBox` control y un `NumericUpDownExtender` control.</span><span class="sxs-lookup"><span data-stu-id="83fea-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="83fea-122">Por último, tendrá que proporcionar la información del servicio web:</span><span class="sxs-lookup"><span data-stu-id="83fea-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="83fea-123">`ServiceDownMethod` nombre de la profundidad método web o página (método)</span><span class="sxs-lookup"><span data-stu-id="83fea-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="83fea-124">`ServiceDownPath` ruta de acceso al servicio web con el método de servicio hacia abajo; Omitir si está utilizando un método de página</span><span class="sxs-lookup"><span data-stu-id="83fea-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="83fea-125">`ServiceUpMethod` nombre de la seguridad método web o página (método)</span><span class="sxs-lookup"><span data-stu-id="83fea-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="83fea-126">`ServiceUpPath` ruta de acceso al servicio web con el método de servicio arriba; Omitir si está utilizando un método de página</span><span class="sxs-lookup"><span data-stu-id="83fea-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="83fea-127">Este es el marcado completo para la página:</span><span class="sxs-lookup"><span data-stu-id="83fea-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="83fea-128">Si ejecuta la página, observe cómo el valor en el cuadro de texto siempre duplica al hacer clic en el botón superior y está a la mitad al hacer clic en el botón inferior.</span><span class="sxs-lookup"><span data-stu-id="83fea-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="83fea-129">[![Aparecen solo los números que son una potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83fea-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="83fea-130">Aparecen solo los números que son una potencia de 2 ([haga clic aquí para ver la imagen a tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83fea-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="83fea-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="83fea-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
