---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 | Documentos de Microsoft"
author: Erikre
description: "Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ee3e244c4ed29384d11c7acc1440692d3f9b23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="5d20f-103">Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d20f-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="5d20f-104">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="5d20f-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="5d20f-105">[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="5d20f-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="5d20f-106">Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="5d20f-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="5d20f-107">Prueba de ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="5d20f-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="5d20f-108">Pruebe sus conocimientos y reforzar los conceptos clave tomando el examen de ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5d20f-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="5d20f-109">Esta prueba se diseñó específicamente desde el contenido de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="5d20f-110">Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="5d20f-111">Introducción</span><span class="sxs-lookup"><span data-stu-id="5d20f-111">Introduction</span></span>

<span data-ttu-id="5d20f-112">Esta serie de tutoriales le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio Express 2013 para Web y ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d20f-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="5d20f-113">Con el nombre de la aplicación podrá crear **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="5d20f-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="5d20f-114">Es un ejemplo simplificado de un sitio web de almacén frontal que vende productos en línea.</span><span class="sxs-lookup"><span data-stu-id="5d20f-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="5d20f-115">Esta serie de tutoriales destaca las nuevas características disponibles en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d20f-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="5d20f-116">Los comentarios son bienvenidos y nos aseguraremos de hacer todo lo posible para actualizar esta serie de tutoriales en función de sus sugerencias.</span><span class="sxs-lookup"><span data-stu-id="5d20f-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="5d20f-117">Proyecto descargado</span><span class="sxs-lookup"><span data-stu-id="5d20f-117">Download completed project</span></span>

<span data-ttu-id="5d20f-118">Puede descargar un proyecto de C# que contiene el tutorial completado.</span><span class="sxs-lookup"><span data-stu-id="5d20f-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="5d20f-119">[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5d20f-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="5d20f-120">Revise el contenido realizando el examen de formularios Web Forms de ASP.NET relacionado</span><span class="sxs-lookup"><span data-stu-id="5d20f-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="5d20f-121">Después de completar este tutorial, probar su conocimiento y reforzar los conceptos clave tomando el [ASP.NET Web Forms cuestionario](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="5d20f-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="5d20f-122">Esta prueba se diseñó específicamente desde el contenido de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="5d20f-123">Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="5d20f-124">Prueba de ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="5d20f-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="5d20f-125">Audiencia</span><span class="sxs-lookup"><span data-stu-id="5d20f-125">Audience</span></span>

<span data-ttu-id="5d20f-126">Los destinatarios de esta serie de tutoriales son programadores experimentados que están familiarizados con ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5d20f-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="5d20f-127">Un desarrollador interesado en esta serie de tutoriales debe tener los conocimientos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5d20f-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="5d20f-128">Que están familiarizados con un objeto orientada a servicios de lenguaje de programación (OOP)</span><span class="sxs-lookup"><span data-stu-id="5d20f-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="5d20f-129">Familiarizado con conceptos de desarrollo de Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="5d20f-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="5d20f-130">Familiarizado con conceptos de base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="5d20f-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="5d20f-131">Familiarizado con conceptos de arquitectura de n niveles</span><span class="sxs-lookup"><span data-stu-id="5d20f-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="5d20f-132">Si está interesado en revisar los problemas descritos anteriormente, considere la posibilidad de revisar el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d20f-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="5d20f-133">Introducción a Visual C#</span><span class="sxs-lookup"><span data-stu-id="5d20f-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="5d20f-134">[Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="5d20f-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="5d20f-135">Base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="5d20f-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="5d20f-136">Arquitectura de varios niveles</span><span class="sxs-lookup"><span data-stu-id="5d20f-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="5d20f-137">Características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5d20f-137">Application Features</span></span>

<span data-ttu-id="5d20f-138">Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="5d20f-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="5d20f-139">El proyecto de aplicación Web (no el proyecto de sitio Web)</span><span class="sxs-lookup"><span data-stu-id="5d20f-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="5d20f-140">formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="5d20f-140">Web Forms</span></span>
- <span data-ttu-id="5d20f-141">Páginas principales, configuración</span><span class="sxs-lookup"><span data-stu-id="5d20f-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="5d20f-142">bootstrap</span><span class="sxs-lookup"><span data-stu-id="5d20f-142">Bootstrap</span></span>
- <span data-ttu-id="5d20f-143">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="5d20f-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="5d20f-144">La validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="5d20f-144">Request Validation</span></span>
- <span data-ttu-id="5d20f-145">Establecimiento inflexible de tipos controles de datos, modelo de enlace, las anotaciones de datos y el valor de proveedores</span><span class="sxs-lookup"><span data-stu-id="5d20f-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="5d20f-146">SSL y OAuth</span><span class="sxs-lookup"><span data-stu-id="5d20f-146">SSL and OAuth</span></span>
- <span data-ttu-id="5d20f-147">Identidad de ASP.NET, la configuración y la autorización</span><span class="sxs-lookup"><span data-stu-id="5d20f-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="5d20f-148">Validación discreto</span><span class="sxs-lookup"><span data-stu-id="5d20f-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="5d20f-149">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="5d20f-149">Routing</span></span>
- <span data-ttu-id="5d20f-150">Control de errores de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5d20f-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="5d20f-151">Tareas y escenarios de aplicación</span><span class="sxs-lookup"><span data-stu-id="5d20f-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="5d20f-152">Las tareas que se muestran en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="5d20f-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="5d20f-153">Crear, revisar y ejecutar el proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="5d20f-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="5d20f-154">Creación de la estructura de base de datos</span><span class="sxs-lookup"><span data-stu-id="5d20f-154">Creating the database structure</span></span>
- <span data-ttu-id="5d20f-155">Inicializar y la propagación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="5d20f-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="5d20f-156">Personalizar la interfaz de usuario mediante estilos, gráficos y una página maestra</span><span class="sxs-lookup"><span data-stu-id="5d20f-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="5d20f-157">Agregar páginas y navegación</span><span class="sxs-lookup"><span data-stu-id="5d20f-157">Adding pages and navigation</span></span>
- <span data-ttu-id="5d20f-158">Mostrar detalles de menú y los datos de producto</span><span class="sxs-lookup"><span data-stu-id="5d20f-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="5d20f-159">Creación de un carro de la compra</span><span class="sxs-lookup"><span data-stu-id="5d20f-159">Creating a shopping cart</span></span>
- <span data-ttu-id="5d20f-160">Compatibilidad con SSL agregando y OAuth</span><span class="sxs-lookup"><span data-stu-id="5d20f-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="5d20f-161">Agregar un método de pago</span><span class="sxs-lookup"><span data-stu-id="5d20f-161">Adding a payment method</span></span>
- <span data-ttu-id="5d20f-162">Como un rol de administrador y un usuario a la aplicación</span><span class="sxs-lookup"><span data-stu-id="5d20f-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="5d20f-163">Restringir el acceso a páginas específicas y carpeta</span><span class="sxs-lookup"><span data-stu-id="5d20f-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="5d20f-164">Cargar un archivo a la aplicación web</span><span class="sxs-lookup"><span data-stu-id="5d20f-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="5d20f-165">Implementar la validación de entrada</span><span class="sxs-lookup"><span data-stu-id="5d20f-165">Implementing input validation</span></span>
- <span data-ttu-id="5d20f-166">Registrar las rutas para la aplicación web</span><span class="sxs-lookup"><span data-stu-id="5d20f-166">Registering routes for the web application</span></span>
- <span data-ttu-id="5d20f-167">Implementar el control de errores y registro de errores</span><span class="sxs-lookup"><span data-stu-id="5d20f-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="5d20f-168">Información general</span><span class="sxs-lookup"><span data-stu-id="5d20f-168">Overview</span></span>

<span data-ttu-id="5d20f-169">Si está familiarizado con formularios Web Forms de ASP.NET pero no se haya familiarizado con conceptos de programación, tendrá el tutorial derecho.</span><span class="sxs-lookup"><span data-stu-id="5d20f-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="5d20f-170">Si ya está familiarizado con ASP.NET Web Forms, puede beneficiarse de esta serie de tutoriales por las nuevas características disponibles en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d20f-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="5d20f-171">Si no está familiarizado con conceptos de programación y formularios Web Forms de ASP.NET, vea los tutoriales adicionales proporcionados en los formularios Web Forms [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5d20f-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="5d20f-172">Específico del **más reciente** características de ASP.NET 4.5 proporcionados en este formularios Web Forms serie de tutoriales incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d20f-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="5d20f-173">Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y API Web).</span><span class="sxs-lookup"><span data-stu-id="5d20f-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="5d20f-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un marco de trabajo de diseño y creación de temas que proporciona capacidades de diseño y temas con capacidad de respuesta.</span><span class="sxs-lookup"><span data-stu-id="5d20f-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="5d20f-175">[ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de trabajo ASP.NET y funciona con software que no sean IIS de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="5d20f-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="5d20f-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de una actualización de Entity Framework que permite recuperar y manipular los datos como fuertemente tipados, acceder a los datos de forma asincrónica, controlen errores transitorios de conexión y registrar las instrucciones SQL.</span><span class="sxs-lookup"><span data-stu-id="5d20f-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="5d20f-177">Para obtener una lista completa de características de ASP.NET 4.5, consulte [ASP.NET y herramientas Web para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="5d20f-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="5d20f-178">La aplicación de ejemplo Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="5d20f-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="5d20f-179">Las capturas de pantalla siguientes proporcionan una vista rápida de la aplicación de formularios Web de ASP.NET que va a crear en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="5d20f-180">Cuando se ejecuta la aplicación desde Visual Studio Express 2013 para Web, verá la siguiente página de inicio de web.</span><span class="sxs-lookup"><span data-stu-id="5d20f-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

<span data-ttu-id="5d20f-182">Puede registrar como un nuevo usuario o inicie sesión como un usuario existente.</span><span class="sxs-lookup"><span data-stu-id="5d20f-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="5d20f-183">Navegación se proporciona en la parte superior de cada categoría de producto mediante la recuperación de los productos disponibles de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5d20f-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="5d20f-184">Al seleccionar el vínculo de productos, podrá ver una lista de todos los productos disponibles.</span><span class="sxs-lookup"><span data-stu-id="5d20f-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="5d20f-186">También puede ver detalles de los productos individuales haciendo clic en cualquiera de los productos enumerados.</span><span class="sxs-lookup"><span data-stu-id="5d20f-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - detalles de producto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="5d20f-188">Como un usuario, puede registrar e inicie sesión usando la función predeterminada de la plantilla de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5d20f-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="5d20f-189">Este tutorial también explica cómo iniciar sesión con una cuenta de Gmail existente.</span><span class="sxs-lookup"><span data-stu-id="5d20f-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="5d20f-190">Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5d20f-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

<span data-ttu-id="5d20f-192">Una vez que ha iniciado sesión como un usuario, puede agregar productos del carro de la compra y desprotección con PayPal.</span><span class="sxs-lookup"><span data-stu-id="5d20f-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="5d20f-193">Tenga en cuenta que esta aplicación de ejemplo está diseñada para funcionar con el espacio aislado de desarrollador de PayPal.</span><span class="sxs-lookup"><span data-stu-id="5d20f-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="5d20f-194">No hay ninguna transacción dinero real llevará a cabo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

<span data-ttu-id="5d20f-196">PayPal confirmará tu cuenta, el orden y la información de pago.</span><span class="sxs-lookup"><span data-stu-id="5d20f-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="5d20f-198">Después de volver de PayPal, puede revisar y completar el pedido.</span><span class="sxs-lookup"><span data-stu-id="5d20f-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="5d20f-200">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5d20f-200">Prerequisites</span></span>

<span data-ttu-id="5d20f-201">Antes de empezar, asegúrese de que tiene el siguiente software instalado en el equipo:</span><span class="sxs-lookup"><span data-stu-id="5d20f-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="5d20f-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="5d20f-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span> <span data-ttu-id="5d20f-203">.NET Framework se instala automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5d20f-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="5d20f-204">Esta serie de tutoriales usa Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="5d20f-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="5d20f-205">Puede usar el modo de Microsoft Visual Studio Express 2013 para Web o Microsoft Visual Studio 2013 para completar esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5d20f-206">Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se conocerán como Visual Studio a lo largo de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="5d20f-207">Si ya tiene instalada una versión de Visual Studio, el proceso de instalación instalará Visual Studio 2013 o Microsoft Visual Studio Express 2013 para Web junto a la versión existente.</span><span class="sxs-lookup"><span data-stu-id="5d20f-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="5d20f-208">Los sitios que crearon en versiones anteriores se pueden abrir en Visual Studio 2013 y continuarán abrir en versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5d20f-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5d20f-209">En este tutorial se da por supuesto que ha seleccionado la *desarrollo Web* colección de valores de la primera vez que inicia Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d20f-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="5d20f-210">Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d20f-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="5d20f-211">Descargar la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="5d20f-211">Download the Sample Application</span></span>

<span data-ttu-id="5d20f-212">Después de instalar los requisitos previos, está listo para empezar a crear el nuevo proyecto Web que se presenta en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="5d20f-213">Si le gustaría **opcionalmente** ejecutar la aplicación de ejemplo que crea esta serie de tutoriales, puede descargarlo desde el sitio de muestras de MSDN.</span><span class="sxs-lookup"><span data-stu-id="5d20f-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="5d20f-214">Esta descarga contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d20f-214">This download contains the following:</span></span>

- <span data-ttu-id="5d20f-215">La aplicación de ejemplo en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="5d20f-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="5d20f-216">Los recursos que usa para crear la aplicación de ejemplo en el *WingtipToys activos* carpeta en el *WingtipToys* carpeta.</span><span class="sxs-lookup"><span data-stu-id="5d20f-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="5d20f-217">Descargue el archivo desde el sitio de muestras de MSDN:</span><span class="sxs-lookup"><span data-stu-id="5d20f-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="5d20f-218">[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5d20f-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="5d20f-219">La descarga es un *.zip* archivo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-219">The download is a *.zip* file.</span></span> <span data-ttu-id="5d20f-220">Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el *C#*carpeta en el *.zip* archivo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-220">To see the completed project that this tutorial series creates, find and select the *C#*folder in the *.zip* file.</span></span> <span data-ttu-id="5d20f-221">Guardar el *C#* carpeta a la carpeta usada para trabajar con proyectos de Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5d20f-221">Save the *C#* folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="5d20f-222">De forma predeterminada, la carpeta de proyectos de Visual Studio 2013 es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d20f-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="5d20f-223">**C:\Users\*****&lt;nombre de usuario&gt;*** \Documents\Visual Studio 2013\Projects**</span><span class="sxs-lookup"><span data-stu-id="5d20f-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span></span>

<span data-ttu-id="5d20f-224">Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="5d20f-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="5d20f-225">Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambiar temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="5d20f-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="5d20f-226">Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="5d20f-227">Visual Studio 2013 se abrirá el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d20f-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="5d20f-228">A continuación, haga clic en el *Default.aspx* un archivo en la ventana Explorador de soluciones y haga clic en ver en el explorador en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="5d20f-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="5d20f-229">Comentarios y soporte técnico de tutorial</span><span class="sxs-lookup"><span data-stu-id="5d20f-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="5d20f-230">Utilice la sección de preguntas y r incluida con el [Introducción a formularios Web Forms de ASP.NET 4.5 y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ejemplo (C#) para cualquier pregunta o enviar comentarios.</span><span class="sxs-lookup"><span data-stu-id="5d20f-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="5d20f-231">Comentarios en esta serie de tutoriales son bienvenidos y cuando se actualiza esta serie de tutoriales se realizará todo lo posible para tener en cuenta correcciones o sugerencias para mejoras en el que se proporcionan en los comentarios del tutoriales.</span><span class="sxs-lookup"><span data-stu-id="5d20f-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="5d20f-232">Cuando se produce un error durante el desarrollo, o si el sitio Web no se ejecuta correctamente, los mensajes de error pueden provocar pistas complejos para el origen del problema o no podrían explicar cómo corregirlo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="5d20f-233">Para ayudarle con algunos escenarios comunes de problema, también puede usar el [foros ASP.NET](https://forums.asp.net/) o en la sección de preguntas y r incluido con el [Introducción a formularios Web Forms de ASP.NET 4.5 y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5d20f-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="5d20f-234">Si aparece un mensaje de error o algo no funciona tal y como se vaya a través de los tutoriales, asegúrese de comprobar las ubicaciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5d20f-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5d20f-235">Siguiente</span><span class="sxs-lookup"><span data-stu-id="5d20f-235">Next</span></span>](create-the-project.md)
