---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finales, control de excepciones y conclusión | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y la excepción...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828988"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="4bc94-104">Parte 8: Páginas finales, control de excepciones y conclusión</span><span class="sxs-lookup"><span data-stu-id="4bc94-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="4bc94-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4bc94-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4bc94-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="4bc94-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4bc94-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="4bc94-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4bc94-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="4bc94-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4bc94-109">Parte 8 agrega una página de contacto, acerca de la página y control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="4bc94-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="4bc94-110">Ésta es la conclusión de la serie.</span><span class="sxs-lookup"><span data-stu-id="4bc94-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="4bc94-111">Póngase en contacto con la página (envío de correo electrónico desde ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="4bc94-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="4bc94-112">Crear una nueva página denominada ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="4bc94-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="4bc94-113">Con el diseñador, cree el siguiente formulario toma nota especial para incluir el ToolkitScriptManager y el control del Editor de la AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="4bc94-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="4bc94-114">.</span><span class="sxs-lookup"><span data-stu-id="4bc94-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="4bc94-115">Haga doble clic en el botón "Enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implementar un método para enviar la información de contacto como un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4bc94-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="4bc94-116">Este código requiere que el archivo web.config contiene una entrada en la sección de configuración que especifica el servidor SMTP para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4bc94-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="4bc94-117">Acerca de la página</span><span class="sxs-lookup"><span data-stu-id="4bc94-117">About Page</span></span>

<span data-ttu-id="4bc94-118">Cree una página denominada AboutUs.aspx y agregue cualquier contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="4bc94-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="4bc94-119">Controlador de excepciones global</span><span class="sxs-lookup"><span data-stu-id="4bc94-119">Global Exception Handler</span></span>

<span data-ttu-id="4bc94-120">Por último, en toda la aplicación se ha iniciado la excepción y hay una circunstancia imprevista en frío es también las excepciones no controlada de causa en nuestra aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4bc94-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="4bc94-121">No queremos nunca una excepción no controlada que se mostrará para un visitante del sitio web.</span><span class="sxs-lookup"><span data-stu-id="4bc94-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="4bc94-122">Aparte de ser una experiencia de usuario terrible las excepciones no controladas también pueden ser un problema de seguridad.</span><span class="sxs-lookup"><span data-stu-id="4bc94-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="4bc94-123">Para solucionar este problema implementamos un controlador de excepciones global.</span><span class="sxs-lookup"><span data-stu-id="4bc94-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="4bc94-124">Para ello, abra el archivo Global.asax y tenga en cuenta el siguiente controlador de eventos generados previamente.</span><span class="sxs-lookup"><span data-stu-id="4bc94-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="4bc94-125">Agregue código para implementar la aplicación\_controlador de errores como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="4bc94-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="4bc94-126">A continuación, agregue una página denominada Error.aspx a la solución y agregue este fragmento de código de marcado.</span><span class="sxs-lookup"><span data-stu-id="4bc94-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="4bc94-127">Ahora en la página\_extracción del controlador de eventos los mensajes de error de carga del objeto Request.</span><span class="sxs-lookup"><span data-stu-id="4bc94-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="4bc94-128">Conclusión</span><span class="sxs-lookup"><span data-stu-id="4bc94-128">Conclusion</span></span>

<span data-ttu-id="4bc94-129">Hemos visto que ASP.NET WebForms facilita la tarea para crear un sitio Web sofisticado con acceso de base de datos, suscripciones, AJAX, etcetera.</span><span class="sxs-lookup"><span data-stu-id="4bc94-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="4bc94-130">muy rápidamente.</span><span class="sxs-lookup"><span data-stu-id="4bc94-130">pretty quickly.</span></span>

<span data-ttu-id="4bc94-131">Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear su propio ASP.NET WebForms aplicaciones!</span><span class="sxs-lookup"><span data-stu-id="4bc94-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4bc94-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="4bc94-132">Previous</span></span>](tailspin-spyworks-part-7.md)
