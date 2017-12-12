---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Parte 8: Páginas finales, control de excepciones y conclusión | Documentos de Microsoft"
author: JoeStagner
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y la excepción..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="9c767-104">Parte 8: Páginas finales, control de excepciones y conclusión</span><span class="sxs-lookup"><span data-stu-id="9c767-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="9c767-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9c767-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9c767-106">Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="9c767-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9c767-107">Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="9c767-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9c767-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9c767-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9c767-109">Parte 8 agrega una página de contacto, acerca de la página y el control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="9c767-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="9c767-110">Ésta es la conclusión de la serie.</span><span class="sxs-lookup"><span data-stu-id="9c767-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="9c767-111">Póngase en contacto con la página (envío de correo electrónico de ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="9c767-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="9c767-112">Crear una nueva página denominada ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="9c767-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="9c767-113">Mediante el diseñador, cree el siguiente formulario Anotar especial para incluir el ToolkitScriptManager y el control del Editor de la AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="9c767-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="9c767-114">.</span><span class="sxs-lookup"><span data-stu-id="9c767-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="9c767-115">Haga doble clic en el botón "Enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implemente un método para enviar la información de contacto como un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="9c767-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="9c767-116">Este código requiere que el archivo web.config contiene una entrada en la sección de configuración que especifica el servidor SMTP que se utilizará para enviar correo.</span><span class="sxs-lookup"><span data-stu-id="9c767-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="9c767-117">Acerca de la página</span><span class="sxs-lookup"><span data-stu-id="9c767-117">About Page</span></span>

<span data-ttu-id="9c767-118">Cree una página denominada AboutUs.aspx y agregue cualquier contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="9c767-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="9c767-119">Controlador de excepciones global</span><span class="sxs-lookup"><span data-stu-id="9c767-119">Global Exception Handler</span></span>

<span data-ttu-id="9c767-120">Por último, a lo largo de la aplicación hemos produce excepciones y circunstancias imprevistas inactivos que también hay excepciones no controlada de causa en nuestra aplicación web.</span><span class="sxs-lookup"><span data-stu-id="9c767-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="9c767-121">No queremos nunca una excepción no controlada que se mostrará a los visitantes del sitio web.</span><span class="sxs-lookup"><span data-stu-id="9c767-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="9c767-122">Aparte de ser una experiencia de usuario terribles las excepciones no controladas también pueden ser un problema de seguridad.</span><span class="sxs-lookup"><span data-stu-id="9c767-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="9c767-123">Para solucionar este problema implementamos un controlador de excepciones global.</span><span class="sxs-lookup"><span data-stu-id="9c767-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="9c767-124">Para ello, abra el archivo Global.asax y tenga en cuenta el siguiente controlador de eventos generados previamente.</span><span class="sxs-lookup"><span data-stu-id="9c767-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="9c767-125">Agregue código para implementar la aplicación\_controlador de errores como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="9c767-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="9c767-126">A continuación, agregue una página denominada Error.aspx a la solución y agregue este fragmento de código de marcado.</span><span class="sxs-lookup"><span data-stu-id="9c767-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="9c767-127">Ahora, en la página\_extracción de controlador de eventos mensajes de error de carga del objeto Request.</span><span class="sxs-lookup"><span data-stu-id="9c767-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="9c767-128">Conclusión</span><span class="sxs-lookup"><span data-stu-id="9c767-128">Conclusion</span></span>

<span data-ttu-id="9c767-129">Hemos visto que ese WebForms ASP.NET facilita el proceso para crear un sitio Web sofisticado con acceso de base de datos de pertenencia, AJAX, etcetera.</span><span class="sxs-lookup"><span data-stu-id="9c767-129">We've seen that that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="9c767-130">muy rápidamente.</span><span class="sxs-lookup"><span data-stu-id="9c767-130">pretty quickly.</span></span>

<span data-ttu-id="9c767-131">Espero que este tutorial le ha concedido las herramientas que necesita para empezar a compilar sus propios formularios Web Forms de ASP.NET en las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="9c767-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9c767-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="9c767-132">Previous</span></span>](tailspin-spyworks-part-7.md)
