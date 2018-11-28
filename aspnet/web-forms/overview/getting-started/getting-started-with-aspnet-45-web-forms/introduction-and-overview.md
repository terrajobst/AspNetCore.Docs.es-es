---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450689"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="44a03-103">Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="44a03-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="44a03-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="44a03-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="44a03-105">[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="44a03-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="44a03-106">Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="44a03-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> 

## <a name="introduction"></a><span data-ttu-id="44a03-107">Introducción</span><span class="sxs-lookup"><span data-stu-id="44a03-107">Introduction</span></span>

<span data-ttu-id="44a03-108">Esta serie de tutoriales le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio Express 2013 para Web y ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="44a03-108">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="44a03-109">La aplicación que se va a crear se denomina **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="44a03-109">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="44a03-110">Es un ejemplo simplificado de un sitio web de front-store que vende productos en línea.</span><span class="sxs-lookup"><span data-stu-id="44a03-110">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="44a03-111">Esta serie de tutoriales resalta las nuevas características disponibles en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="44a03-111">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="44a03-112">Los comentarios son bienvenidos y haremos todo lo posible para actualizar esta serie de tutoriales en función de sus sugerencias.</span><span class="sxs-lookup"><span data-stu-id="44a03-112">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="44a03-113">Proyecto de descarga completada</span><span class="sxs-lookup"><span data-stu-id="44a03-113">Download completed project</span></span>

<span data-ttu-id="44a03-114">Puede descargar un proyecto de C# que contiene el tutorial completado.</span><span class="sxs-lookup"><span data-stu-id="44a03-114">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="44a03-115">[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="44a03-115">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="44a03-116">Revise el contenido realizando el examen de formularios Web Forms ASP.NET relacionado</span><span class="sxs-lookup"><span data-stu-id="44a03-116">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="44a03-117">Después de completar este tutorial, ponga a prueba sus conocimientos y reforzar los conceptos clave tomando el [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="44a03-117">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="44a03-118">Este examen se diseñó específicamente desde el contenido de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-118">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="44a03-119">Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.</span><span class="sxs-lookup"><span data-stu-id="44a03-119">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="44a03-120">Cuestionario de ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="44a03-120">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="44a03-121">Audiencia</span><span class="sxs-lookup"><span data-stu-id="44a03-121">Audience</span></span>

<span data-ttu-id="44a03-122">Los destinatarios de esta serie de tutoriales son que los desarrolladores con experiencia que están familiarizados con ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="44a03-122">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="44a03-123">Un desarrollador interesado en esta serie de tutoriales debe tener los conocimientos siguientes:</span><span class="sxs-lookup"><span data-stu-id="44a03-123">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="44a03-124">Está familiarizado con un objeto orientada a servicios de lenguaje de programación (OOP)</span><span class="sxs-lookup"><span data-stu-id="44a03-124">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="44a03-125">Está familiarizado con conceptos de desarrollo Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="44a03-125">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="44a03-126">Está familiarizado con conceptos de base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="44a03-126">Familiar with relational database concepts</span></span>
- <span data-ttu-id="44a03-127">Está familiarizado con conceptos de arquitectura de n niveles</span><span class="sxs-lookup"><span data-stu-id="44a03-127">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="44a03-128">Si está interesado en la revisión de los problemas descritos anteriormente, considere la posibilidad de revisar el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="44a03-128">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="44a03-129">Introducción a Visual C#</span><span class="sxs-lookup"><span data-stu-id="44a03-129">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="44a03-130">[Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="44a03-130">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="44a03-131">Base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="44a03-131">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="44a03-132">Arquitectura de varios niveles</span><span class="sxs-lookup"><span data-stu-id="44a03-132">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="44a03-133">Características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="44a03-133">Application Features</span></span>

<span data-ttu-id="44a03-134">Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="44a03-134">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="44a03-135">El proyecto de aplicación Web (no el proyecto de sitio Web)</span><span class="sxs-lookup"><span data-stu-id="44a03-135">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="44a03-136">Formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="44a03-136">Web Forms</span></span>
- <span data-ttu-id="44a03-137">Páginas maestras, configuración</span><span class="sxs-lookup"><span data-stu-id="44a03-137">Master Pages, Configuration</span></span>
- <span data-ttu-id="44a03-138">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="44a03-138">Bootstrap</span></span>
- <span data-ttu-id="44a03-139">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="44a03-139">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="44a03-140">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="44a03-140">Request Validation</span></span>
- <span data-ttu-id="44a03-141">Fuertemente tipadas en controles de datos, enlace, las anotaciones de datos de modelos y los proveedores de valor</span><span class="sxs-lookup"><span data-stu-id="44a03-141">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="44a03-142">SSL y OAuth</span><span class="sxs-lookup"><span data-stu-id="44a03-142">SSL and OAuth</span></span>
- <span data-ttu-id="44a03-143">ASP.NET Identity, configuración y la autorización</span><span class="sxs-lookup"><span data-stu-id="44a03-143">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="44a03-144">Validación discreta</span><span class="sxs-lookup"><span data-stu-id="44a03-144">Unobtrusive Validation</span></span>
- <span data-ttu-id="44a03-145">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="44a03-145">Routing</span></span>
- <span data-ttu-id="44a03-146">Control de errores de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="44a03-146">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="44a03-147">Tareas y escenarios de aplicación</span><span class="sxs-lookup"><span data-stu-id="44a03-147">Application Scenarios and Tasks</span></span>

<span data-ttu-id="44a03-148">Las tareas que se muestra en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="44a03-148">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="44a03-149">Crear, revisar y ejecutar el proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="44a03-149">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="44a03-150">Creación de la estructura de base de datos</span><span class="sxs-lookup"><span data-stu-id="44a03-150">Creating the database structure</span></span>
- <span data-ttu-id="44a03-151">Inicializar y la propagación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="44a03-151">Initializing and seeding the database</span></span>
- <span data-ttu-id="44a03-152">Personalizar la interfaz de usuario mediante los estilos, gráficos y una página maestra</span><span class="sxs-lookup"><span data-stu-id="44a03-152">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="44a03-153">Agregar páginas y navegación</span><span class="sxs-lookup"><span data-stu-id="44a03-153">Adding pages and navigation</span></span>
- <span data-ttu-id="44a03-154">Mostrar detalles de menú y los datos de productos</span><span class="sxs-lookup"><span data-stu-id="44a03-154">Displaying menu details and product data</span></span>
- <span data-ttu-id="44a03-155">Creación de un carro de la compra</span><span class="sxs-lookup"><span data-stu-id="44a03-155">Creating a shopping cart</span></span>
- <span data-ttu-id="44a03-156">Compatibilidad con SSL agregando y OAuth</span><span class="sxs-lookup"><span data-stu-id="44a03-156">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="44a03-157">Agregar un método de pago</span><span class="sxs-lookup"><span data-stu-id="44a03-157">Adding a payment method</span></span>
- <span data-ttu-id="44a03-158">Como un rol de administrador y un usuario a la aplicación</span><span class="sxs-lookup"><span data-stu-id="44a03-158">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="44a03-159">Restringir el acceso a páginas específicas y carpeta</span><span class="sxs-lookup"><span data-stu-id="44a03-159">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="44a03-160">Cargar un archivo a la aplicación web</span><span class="sxs-lookup"><span data-stu-id="44a03-160">Uploading a file to the web application</span></span>
- <span data-ttu-id="44a03-161">Implementar la validación de entrada</span><span class="sxs-lookup"><span data-stu-id="44a03-161">Implementing input validation</span></span>
- <span data-ttu-id="44a03-162">Registrar las rutas para la aplicación web</span><span class="sxs-lookup"><span data-stu-id="44a03-162">Registering routes for the web application</span></span>
- <span data-ttu-id="44a03-163">Implementación de control de errores y registro de errores</span><span class="sxs-lookup"><span data-stu-id="44a03-163">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="44a03-164">Información general</span><span class="sxs-lookup"><span data-stu-id="44a03-164">Overview</span></span>

<span data-ttu-id="44a03-165">Si está familiarizado con ASP.NET Web Forms, pero están familiarizados con conceptos de programación, tiene el tutorial adecuado.</span><span class="sxs-lookup"><span data-stu-id="44a03-165">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="44a03-166">Si ya está familiarizado con ASP.NET Web Forms, puede beneficiarse de esta serie de tutoriales por las nuevas características disponibles en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="44a03-166">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="44a03-167">Si no está familiarizado con conceptos de programación y formularios Web Forms de ASP.NET, consulte los tutoriales adicionales proporcionados en los formularios Web Forms [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="44a03-167">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="44a03-168">Específico del **más reciente** ASP.NET 4.5 características proporcionadas en este formularios Web Forms de serie de tutoriales incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="44a03-168">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="44a03-169">Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y Web API).</span><span class="sxs-lookup"><span data-stu-id="44a03-169">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="44a03-170">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un marco de diseño y creación de temas que proporciona capacidades de diseño y creación de temas con capacidad de respuesta.</span><span class="sxs-lookup"><span data-stu-id="44a03-170">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="44a03-171">[ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con el software que no sean IIS de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="44a03-171">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="44a03-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de una actualización de Entity Framework que permite recuperar y manipular los datos como fuertemente tipados, acceder a los datos de forma asincrónica, controlan errores transitorios de conexión y registrar las instrucciones SQL.</span><span class="sxs-lookup"><span data-stu-id="44a03-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="44a03-173">Para obtener una lista completa de las características de ASP.NET 4.5, vea [ASP.NET and Web Tools para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="44a03-173">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="44a03-174">La aplicación de ejemplo Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="44a03-174">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="44a03-175">Las capturas de pantalla siguientes proporcionan una vista rápida de la aplicación de formularios Web ASP.NET que va a crear en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-175">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="44a03-176">Cuando se ejecuta la aplicación desde Visual Studio Express 2013 para Web, verá la siguiente página principal de web.</span><span class="sxs-lookup"><span data-stu-id="44a03-176">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

<span data-ttu-id="44a03-178">Puede registrar como un nuevo usuario, o inicie sesión como un usuario existente.</span><span class="sxs-lookup"><span data-stu-id="44a03-178">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="44a03-179">Navegación se proporciona en la parte superior de cada categoría de producto mediante la recuperación de los productos disponibles de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="44a03-179">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="44a03-180">Al seleccionar el vínculo de productos, podrá ver una lista de todos los productos disponibles.</span><span class="sxs-lookup"><span data-stu-id="44a03-180">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="44a03-182">También puede ver los detalles de los productos individuales haciendo clic en cualquiera de los productos enumerados.</span><span class="sxs-lookup"><span data-stu-id="44a03-182">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - detalles del producto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="44a03-184">Como usuario, puede registrar e inicie sesión con la funcionalidad predeterminada de la plantilla de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="44a03-184">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="44a03-185">En este tutorial también se explica cómo iniciar sesión con una cuenta de Gmail.</span><span class="sxs-lookup"><span data-stu-id="44a03-185">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="44a03-186">Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="44a03-186">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

<span data-ttu-id="44a03-188">Una vez que haya iniciado sesión como un usuario, puede agregar productos al carro de la compra y desprotección con PayPal.</span><span class="sxs-lookup"><span data-stu-id="44a03-188">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="44a03-189">Tenga en cuenta que esta aplicación de ejemplo está diseñada para funcionar con el espacio aislado de desarrollador de PayPal.</span><span class="sxs-lookup"><span data-stu-id="44a03-189">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="44a03-190">No hay ninguna transacción real dinero llevará a cabo.</span><span class="sxs-lookup"><span data-stu-id="44a03-190">No actual money transaction will take place.</span></span>

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

<span data-ttu-id="44a03-192">PayPal confirmará su cuenta, el orden y la información de pago.</span><span class="sxs-lookup"><span data-stu-id="44a03-192">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="44a03-194">Después de volver de PayPal, puede revisar y completar su pedido.</span><span class="sxs-lookup"><span data-stu-id="44a03-194">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="44a03-196">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="44a03-196">Prerequisites</span></span>

<span data-ttu-id="44a03-197">Antes de empezar, asegúrese de que tiene el siguiente software instalado en el equipo:</span><span class="sxs-lookup"><span data-stu-id="44a03-197">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="44a03-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="44a03-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="44a03-199">.NET Framework se instala automáticamente.</span><span class="sxs-lookup"><span data-stu-id="44a03-199">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="44a03-200">Esta serie de tutoriales usa Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="44a03-200">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="44a03-201">Puede usar en Microsoft Visual Studio Express 2013 para Web o Microsoft Visual Studio 2013 para completar esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-201">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="44a03-202">Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se hará referencia a que Visual Studio a lo largo de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-202">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="44a03-203">Si ya tiene instalada una versión de Visual Studio, el proceso de instalación instalará Microsoft Visual Studio Express 2013 para Web o Visual Studio 2013 junto a la versión existente.</span><span class="sxs-lookup"><span data-stu-id="44a03-203">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="44a03-204">Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2013 y continuarán para abrir en versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="44a03-204">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="44a03-205">En este tutorial se supone que seleccionó el *desarrollo Web* colección de valores de la primera vez que inicia Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44a03-205">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="44a03-206">Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="44a03-206">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="44a03-207">Descargue la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="44a03-207">Download the Sample Application</span></span>

<span data-ttu-id="44a03-208">Después de instalar los requisitos previos, está listo para empezar a crear el nuevo proyecto Web que se presenta en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-208">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="44a03-209">Si le gustaría **opcionalmente** ejecutar la aplicación de ejemplo que crea esta serie de tutoriales, puede descargarlo desde el sitio de ejemplos de MSDN.</span><span class="sxs-lookup"><span data-stu-id="44a03-209">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="44a03-210">Esta descarga contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="44a03-210">This download contains the following:</span></span>

- <span data-ttu-id="44a03-211">La aplicación de ejemplo en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="44a03-211">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="44a03-212">Los recursos utilizados para crear la aplicación de ejemplo en el *WingtipToys-activos* carpeta en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="44a03-212">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="44a03-213">Descargue el archivo desde el sitio de ejemplos de MSDN:</span><span class="sxs-lookup"><span data-stu-id="44a03-213">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="44a03-214">[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="44a03-214">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="44a03-215">La descarga es un <em>.zip</em> archivo.</span><span class="sxs-lookup"><span data-stu-id="44a03-215">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="44a03-216">Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el <em>C#</em>carpeta en el <em>.zip</em> archivo.</span><span class="sxs-lookup"><span data-stu-id="44a03-216">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="44a03-217">Guardar el <em>C#</em> carpeta a la carpeta que se utiliza para trabajar con proyectos de Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="44a03-217">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="44a03-218">De forma predeterminada, la carpeta de proyectos de Visual Studio 2013 es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="44a03-218">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="44a03-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="44a03-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="44a03-220">Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="44a03-220">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="44a03-221">Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="44a03-221">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="44a03-222">Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo.</span><span class="sxs-lookup"><span data-stu-id="44a03-222">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="44a03-223">Visual Studio 2013 se abrirá el proyecto.</span><span class="sxs-lookup"><span data-stu-id="44a03-223">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="44a03-224">A continuación, haga clic en el *Default.aspx* de archivos en la ventana Explorador de soluciones y haga clic en ver en el explorador en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="44a03-224">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="44a03-225">Comentarios y soporte técnico de tutorial</span><span class="sxs-lookup"><span data-stu-id="44a03-225">Tutorial Support and Comments</span></span>

<span data-ttu-id="44a03-226">Use la sección de preguntas y respuestas incluida con el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ejemplo (C#) para cualquier pregunta o comentario.</span><span class="sxs-lookup"><span data-stu-id="44a03-226">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="44a03-227">Comentarios en esta serie de tutoriales son bienvenidos y, cuando se actualiza esta serie de tutoriales se realizarán todos los esfuerzos para tener en cuenta correcciones o sugerencias de mejoras que se proporcionan en los comentarios del tutoriales.</span><span class="sxs-lookup"><span data-stu-id="44a03-227">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="44a03-228">Cuando se produce un error durante el desarrollo, o si el sitio Web no se ejecuta correctamente, los mensajes de error pueden dar pistas complejos en el origen del problema o no es posible que se explica cómo corregirlo.</span><span class="sxs-lookup"><span data-stu-id="44a03-228">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="44a03-229">Para ayudarle con algunos escenarios comunes de problema, también puede usar el [foros de ASP.NET](https://forums.asp.net/) o en la sección de preguntas y respuestas incluido con el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ejemplo.</span><span class="sxs-lookup"><span data-stu-id="44a03-229">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="44a03-230">Si recibe un mensaje de error o algo no funciona al avanzar por los tutoriales, asegúrese de comprobar las ubicaciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="44a03-230">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="44a03-231">Siguiente</span><span class="sxs-lookup"><span data-stu-id="44a03-231">Next</span></span>](create-the-project.md)
