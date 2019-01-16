---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción a ASP.NET 4.7 Web Forms y Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante Microsoft Visual Studio y ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341558"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="6f4e4-103">Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6f4e4-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="6f4e4-104">[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f4e4-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="6f4e4-105">Esta serie de tutoriales muestra cómo crear una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="6f4e4-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="6f4e4-106">Introduction</span></span>

<span data-ttu-id="6f4e4-107">Esta serie de tutoriales le guiará en el proceso de creación de una aplicación de formularios Web Forms de ASP.NET mediante Visual Studio 2017 y ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="6f4e4-108">Se creará una aplicación denominada **Wingtip Toys** : un sitio web escaparate simplificada de venta de artículos en línea.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="6f4e4-109">Durante la serie, se resaltan las nuevas características de ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="6f4e4-110">Audiencia de destino</span><span class="sxs-lookup"><span data-stu-id="6f4e4-110">Target audience</span></span>

<span data-ttu-id="6f4e4-111">Los programadores nuevos en ASP.NET Web Forms son la audiencia objetivo de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="6f4e4-112">Debe tener algunos conocimientos en las áreas siguientes:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="6f4e4-113">Programación orientada a objetos (OOP) e idiomas</span><span class="sxs-lookup"><span data-stu-id="6f4e4-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="6f4e4-114">Desarrollo Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="6f4e4-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="6f4e4-115">bases de datos relacionales</span><span class="sxs-lookup"><span data-stu-id="6f4e4-115">Relational databases</span></span>
- <span data-ttu-id="6f4e4-116">Arquitectura de N niveles</span><span class="sxs-lookup"><span data-stu-id="6f4e4-116">N-tier architecture</span></span>

<span data-ttu-id="6f4e4-117">Para revisar estas áreas, considere la posibilidad de estudiar el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="6f4e4-118">Introducción a Visual C#</span><span class="sxs-lookup"><span data-stu-id="6f4e4-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="6f4e4-119">[Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="6f4e4-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="6f4e4-120">Base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="6f4e4-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="6f4e4-121">Arquitectura de varios niveles</span><span class="sxs-lookup"><span data-stu-id="6f4e4-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="6f4e4-122">Características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6f4e4-122">Application features</span></span>

<span data-ttu-id="6f4e4-123">Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="6f4e4-124">El proyecto de aplicación Web (no el proyecto de sitio Web)</span><span class="sxs-lookup"><span data-stu-id="6f4e4-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="6f4e4-125">Formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="6f4e4-125">Web Forms</span></span>
- <span data-ttu-id="6f4e4-126">Páginas maestras, configuración</span><span class="sxs-lookup"><span data-stu-id="6f4e4-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="6f4e4-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="6f4e4-127">Bootstrap</span></span>
- <span data-ttu-id="6f4e4-128">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="6f4e4-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="6f4e4-129">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="6f4e4-129">Request Validation</span></span>
- <span data-ttu-id="6f4e4-130">Controles de datos fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="6f4e4-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="6f4e4-131">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-131">Model Binding</span></span>
- <span data-ttu-id="6f4e4-132">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-132">Data Annotations</span></span>
- <span data-ttu-id="6f4e4-133">Proveedores de valor</span><span class="sxs-lookup"><span data-stu-id="6f4e4-133">Value Providers</span></span>
- <span data-ttu-id="6f4e4-134">SSL y OAuth</span><span class="sxs-lookup"><span data-stu-id="6f4e4-134">SSL and OAuth</span></span>
- <span data-ttu-id="6f4e4-135">ASP.NET Identity, configuración y la autorización</span><span class="sxs-lookup"><span data-stu-id="6f4e4-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="6f4e4-136">Validación discreta</span><span class="sxs-lookup"><span data-stu-id="6f4e4-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="6f4e4-137">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="6f4e4-137">Routing</span></span>
- <span data-ttu-id="6f4e4-138">Control de errores de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6f4e4-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="6f4e4-139">Tareas y escenarios de aplicación</span><span class="sxs-lookup"><span data-stu-id="6f4e4-139">Application scenarios and tasks</span></span>

<span data-ttu-id="6f4e4-140">Las tareas de la serie de tutoriales incluyen:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="6f4e4-141">Crear, revisar y ejecutar un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="6f4e4-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="6f4e4-142">Creación de una estructura de base de datos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-142">Creating a database structure</span></span>
- <span data-ttu-id="6f4e4-143">Inicializar y la propagación de una base de datos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="6f4e4-144">Personalizar la interfaz de usuario con los estilos, gráficos y una página maestra</span><span class="sxs-lookup"><span data-stu-id="6f4e4-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="6f4e4-145">Agregar páginas y navegación</span><span class="sxs-lookup"><span data-stu-id="6f4e4-145">Adding pages and navigation</span></span>
- <span data-ttu-id="6f4e4-146">Mostrar detalles de menú y los datos de productos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="6f4e4-147">Creación de un carro de la compra</span><span class="sxs-lookup"><span data-stu-id="6f4e4-147">Creating a shopping cart</span></span>
- <span data-ttu-id="6f4e4-148">Compatibilidad con SSL agregando y OAuth</span><span class="sxs-lookup"><span data-stu-id="6f4e4-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="6f4e4-149">Agregar un método de pago</span><span class="sxs-lookup"><span data-stu-id="6f4e4-149">Adding a payment method</span></span>
- <span data-ttu-id="6f4e4-150">Como un rol de administrador y un usuario a la aplicación</span><span class="sxs-lookup"><span data-stu-id="6f4e4-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="6f4e4-151">Restringir el acceso a páginas específicas y carpeta</span><span class="sxs-lookup"><span data-stu-id="6f4e4-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="6f4e4-152">Cargar un archivo a la aplicación web</span><span class="sxs-lookup"><span data-stu-id="6f4e4-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="6f4e4-153">Implementar la validación de entrada</span><span class="sxs-lookup"><span data-stu-id="6f4e4-153">Implementing input validation</span></span>
- <span data-ttu-id="6f4e4-154">Registrar las rutas para la aplicación web</span><span class="sxs-lookup"><span data-stu-id="6f4e4-154">Registering routes for the web application</span></span>
- <span data-ttu-id="6f4e4-155">Implementación de control de errores y registro de errores</span><span class="sxs-lookup"><span data-stu-id="6f4e4-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="6f4e4-156">Información general</span><span class="sxs-lookup"><span data-stu-id="6f4e4-156">Overview</span></span>

<span data-ttu-id="6f4e4-157">Esta serie de tutoriales es está diseñado para un usuario familiarizado con conceptos de programación, pero es nuevo en ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="6f4e4-158">Si ya está familiarizado con ASP.NET Web Forms, esta serie todavía puede ayudar a obtener información sobre las nuevas características de ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="6f4e4-159">Para los lectores familiarizados con conceptos de programación y formularios Web Forms de ASP.NET, consulte los tutoriales de formularios Web Forms adicionales proporcionados en el [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="6f4e4-160">El proporcionado en esta serie de tutoriales de ASP.NET 4.5 incluye las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="6f4e4-161">Una interfaz de usuario simple para crear proyectos que ofrece [compatibilidad para muchos marcos ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y Web API).</span><span class="sxs-lookup"><span data-stu-id="6f4e4-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="6f4e4-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un diseño, creación de temas y marco de diseño con capacidad de respuesta.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="6f4e4-163">[ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con el software que no sean IIS de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="6f4e4-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6f4e4-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="6f4e4-165">Una actualización de Entity Framework, lo que le permite:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="6f4e4-166">Recuperar y manipular datos como objetos fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="6f4e4-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="6f4e4-167">Obtener acceso a datos de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="6f4e4-167">Access data asynchronously</span></span>
  - <span data-ttu-id="6f4e4-168">Controlar errores transitorios de conexión</span><span class="sxs-lookup"><span data-stu-id="6f4e4-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="6f4e4-169">Instrucciones de registro SQL</span><span class="sxs-lookup"><span data-stu-id="6f4e4-169">Log SQL statements</span></span>

<span data-ttu-id="6f4e4-170">Para obtener una lista completa de característica ASP.NET 4.5, vea [ASP.NET and Web Tools para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="6f4e4-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="6f4e4-171">La aplicación de ejemplo Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="6f4e4-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="6f4e4-172">Son las siguientes capturas de pantalla de la aplicación de formularios Web Forms de ASP.NET que se crea en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="6f4e4-173">Al ejecutar la aplicación en Visual Studio, aparece la siguiente página principal de web.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

<span data-ttu-id="6f4e4-175">Puede registrar como un nuevo usuario, o inicie sesión como un usuario existente.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="6f4e4-176">El panel de navegación superior tiene vínculos a las categorías de productos y sus productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="6f4e4-177">Si selecciona **productos**, se muestran todos los productos disponibles.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="6f4e4-179">Si selecciona un producto específico, se muestran detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys - detalles del producto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="6f4e4-181">Como usuario, puede registrar e inicie sesión con la funcionalidad de formularios Web Forms plantilla predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="6f4e4-182">En este tutorial también se explica cómo iniciar sesión con una cuenta de Gmail existente.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="6f4e4-183">Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

<span data-ttu-id="6f4e4-185">Una vez que haya iniciado sesión como un usuario, puede agregar productos al carro de la compra y desprotección con PayPal.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="6f4e4-186">La aplicación de ejemplo está diseñada para trabajar en espacio aislado de desarrollador de PayPal.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="6f4e4-187">No hay ninguna transacción dinero real realiza.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-187">No actual money transaction takes place.</span></span>

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

<span data-ttu-id="6f4e4-189">PayPal confirma su información de cuenta, el orden y el pago.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="6f4e4-191">Después de volver de PayPal, puede revisar y completar su pedido.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="6f4e4-193">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6f4e4-193">Prerequisites</span></span>

<span data-ttu-id="6f4e4-194">Antes de empezar, asegúrese de que está instalado el software siguiente en el equipo:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="6f4e4-195">[Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6f4e4-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="6f4e4-196">.NET Framework se instala automáticamente.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="6f4e4-197">Esta serie de tutoriales usa Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="6f4e4-198">Puede usar cualquier que o Microsoft Visual Studio 2017 para completar esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="6f4e4-199">Tenga en cuenta lo siguiente acerca de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="6f4e4-200">Microsoft Visual Studio 2017 y Microsoft Visual Studio Community 2017 se conocen como *Visual Studio* a lo largo de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="6f4e4-201">Visual Studio 2017 se instala junto a las versiones anteriores ya instaladas.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="6f4e4-202">Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2017 y continuarán para abrir en versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="6f4e4-203">La primera vez inicia Visual Studio, se supone que ha seleccionado la *desarrollo Web* configuración.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="6f4e4-204">Para obtener más información, vea [Cómo: Seleccione la configuración del entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f4e4-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="6f4e4-205">Después de instalar los requisitos previos, está listo para empezar a crear el proyecto Web que se presentan en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="6f4e4-206">Descargue la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="6f4e4-206">Download the sample application</span></span>

 <span data-ttu-id="6f4e4-207">Puede descargar el applicatiion ejemplo completado en cualquier momento desde el sitio de ejemplos de MSDN:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="6f4e4-208">[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="6f4e4-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="6f4e4-209">Esta descarga contiene los elementos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-209">This download has the following items:</span></span>

- <span data-ttu-id="6f4e4-210">La aplicación de ejemplo en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="6f4e4-211">Los recursos utilizados para crear la aplicación de ejemplo en el *WingtipToys-activos* carpeta en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="6f4e4-212">La descarga es un *.zip* archivo.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-212">The download is a *.zip* file.</span></span> <span data-ttu-id="6f4e4-213">Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el *C#* carpeta en el archivo zip.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="6f4e4-214">Guardar el C# a la carpeta que se utiliza para trabajar con proyectos de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="6f4e4-215">De forma predeterminada, es la carpeta de proyectos de Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="6f4e4-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="6f4e4-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="6f4e4-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="6f4e4-217">Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="6f4e4-218">Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="6f4e4-219">Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="6f4e4-220">Visual Studio 2017 abre el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="6f4e4-221">A continuación, haga clic en el *Default.aspx* archivo **el Explorador de soluciones** y seleccione **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="6f4e4-222">Cuestionario de formularios Web Forms ASP.NET para revisar el contenido</span><span class="sxs-lookup"><span data-stu-id="6f4e4-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="6f4e4-223">Después de completar la serie de tutoriales, cuestionario a prueba sus conocimientos y reforzar los conceptos clave.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="6f4e4-224">Cada pregunta proporciona una explicación y vínculos a guías adicionales.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="6f4e4-225">Cuestionario de ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="6f4e4-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="6f4e4-226">Comentarios y soporte técnico de tutorial</span><span class="sxs-lookup"><span data-stu-id="6f4e4-226">Tutorial support and comments</span></span>

<span data-ttu-id="6f4e4-227">Para preguntas y comentarios, use la sección de preguntas y respuestas que se incluye en el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) página de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="6f4e4-228">Comentarios en esta serie de tutoriales son bienvenidos.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="6f4e4-229">Cuando se actualiza esta serie de tutoriales, se realizan todos los esfuerzos que tener en cuenta las correcciones o sugerencias para mejorar.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="6f4e4-230">Si se produce un error, los mensajes de error correspondiente podrían resultar confusos, sin buena explicación sobre cómo corregirlo.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="6f4e4-231">Para obtener ayuda, puede comprobar el [foros de ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="6f4e4-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="6f4e4-232">Otra buena fuente es la sección de preguntas y respuestas en el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) página de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="6f4e4-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="6f4e4-233">Siguiente</span><span class="sxs-lookup"><span data-stu-id="6f4e4-233">Next</span></span>](create-the-project.md)
