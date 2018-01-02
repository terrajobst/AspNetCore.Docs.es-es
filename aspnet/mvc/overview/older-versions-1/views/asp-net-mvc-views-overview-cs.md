---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: "Vistas de MVC de ASP.NET de información general (C#) | Documentos de Microsoft"
author: StephenWalther
description: "¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta vistas y se muestra cómo puede t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de095b0621af3b6166a2e1cbcb1c63c26a88aa2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="35766-104">Vistas de MVC de ASP.NET de información general (C#)</span><span class="sxs-lookup"><span data-stu-id="35766-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="35766-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="35766-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="35766-106">¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML?</span><span class="sxs-lookup"><span data-stu-id="35766-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="35766-107">En este tutorial, Stephen Walther presenta vistas y se muestra cómo puede aprovechar las ventajas de la vista de datos y aplicaciones auxiliares HTML dentro de una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="35766-108">El propósito de este tutorial es proporcionar una breve introducción a las vistas de MVC de ASP.NET, ver los datos y aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="35766-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="35766-109">Al final de este tutorial, debe entender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="35766-110">Descripción de vistas</span><span class="sxs-lookup"><span data-stu-id="35766-110">Understanding Views</span></span>

<span data-ttu-id="35766-111">En ASP.NET o páginas Active Server, ASP.NET MVC no incluye todo lo que se corresponde directamente a una página.</span><span class="sxs-lookup"><span data-stu-id="35766-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="35766-112">En una aplicación de MVC de ASP.NET, no hay una página en disco que se corresponde con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="35766-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="35766-113">Lo más cercano a una página en una aplicación ASP.NET MVC es algo llama un *vista*.</span><span class="sxs-lookup"><span data-stu-id="35766-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="35766-114">Las solicitudes entrantes, aplicación de explorador se asignan a las acciones del controlador de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35766-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="35766-115">Una acción de controlador podría devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-115">A controller action might return a view.</span></span> <span data-ttu-id="35766-116">Sin embargo, una acción de controlador puede realizar algún otro tipo de acción como le estamos redirigiendo a otra acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="35766-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="35766-117">Lista 1 contiene un controlador simple denominado HomeController.</span><span class="sxs-lookup"><span data-stu-id="35766-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="35766-118">HomeController expone dos acciones del controlador denominadas Index() y Details().</span><span class="sxs-lookup"><span data-stu-id="35766-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="35766-119">**Lista 1 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="35766-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="35766-120">Puede invocar la primera acción, la acción Index(), escribiendo la dirección URL siguiente en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="35766-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="35766-121">/ Principal/índice</span><span class="sxs-lookup"><span data-stu-id="35766-121">/Home/Index</span></span>

<span data-ttu-id="35766-122">Puede invocar la segunda acción, la acción Details(), para ello, escriba esta dirección en el explorador:</span><span class="sxs-lookup"><span data-stu-id="35766-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="35766-123">/ Principal/detalles</span><span class="sxs-lookup"><span data-stu-id="35766-123">/Home/Details</span></span>

<span data-ttu-id="35766-124">La acción de Index() devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-124">The Index() action returns a view.</span></span> <span data-ttu-id="35766-125">Mayoría de las acciones que crea, devolverá vistas.</span><span class="sxs-lookup"><span data-stu-id="35766-125">Most actions that you create will return views.</span></span> <span data-ttu-id="35766-126">Sin embargo, una acción puede devolver otros tipos de resultados de la acción.</span><span class="sxs-lookup"><span data-stu-id="35766-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="35766-127">Por ejemplo, la acción Details() devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción de Index().</span><span class="sxs-lookup"><span data-stu-id="35766-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="35766-128">La acción de Index() contiene la única línea de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35766-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="35766-129">View();</span><span class="sxs-lookup"><span data-stu-id="35766-129">View();</span></span>

<span data-ttu-id="35766-130">Esta línea de código devuelve una vista que debe estar situada en la siguiente ruta de acceso en el servidor web:</span><span class="sxs-lookup"><span data-stu-id="35766-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="35766-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="35766-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="35766-132">La ruta de acceso a la vista se deduce el nombre del controlador y el nombre de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="35766-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="35766-133">Si lo prefiere, pueden ser explícitas acerca de la vista.</span><span class="sxs-lookup"><span data-stu-id="35766-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="35766-134">La siguiente línea de código devuelve una vista denominada a Fred:</span><span class="sxs-lookup"><span data-stu-id="35766-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="35766-135">Vista (Fred);</span><span class="sxs-lookup"><span data-stu-id="35766-135">View( Fred );</span></span>

<span data-ttu-id="35766-136">Cuando se ejecuta esta línea de código, se devuelve una vista de la ruta de acceso siguiente:</span><span class="sxs-lookup"><span data-stu-id="35766-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="35766-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="35766-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="35766-138">Si tiene previsto crear pruebas unitarias para la aplicación de ASP.NET MVC es una buena idea ser explícito acerca de los nombres de vista.</span><span class="sxs-lookup"><span data-stu-id="35766-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="35766-139">De este modo, puede crear una prueba unitaria para comprobar que la vista esperada devolvió una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="35766-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="35766-140">Agregar contenido a una vista</span><span class="sxs-lookup"><span data-stu-id="35766-140">Adding Content to a View</span></span>

<span data-ttu-id="35766-141">Una vista es un estándar de (documento HTML que puede contener secuencias de comandos X).</span><span class="sxs-lookup"><span data-stu-id="35766-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="35766-142">Utilizar secuencias de comandos para agregar contenido dinámico a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="35766-143">Por ejemplo, la vista en el listado 2 muestra la fecha y hora actuales.</span><span class="sxs-lookup"><span data-stu-id="35766-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="35766-144">**La lista 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="35766-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="35766-145">Tenga en cuenta que el cuerpo de la página HTML en el listado 2 contiene el script siguiente:</span><span class="sxs-lookup"><span data-stu-id="35766-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="35766-146">&lt;% Response.Write(DateTime.Now); %&gt;</span><span class="sxs-lookup"><span data-stu-id="35766-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="35766-147">Utilice los delimitadores de secuencia de comandos &lt;% y %&gt; para marcar el principio y final de una secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="35766-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="35766-148">Este script se escribe en C#.</span><span class="sxs-lookup"><span data-stu-id="35766-148">This script is written in C#.</span></span> <span data-ttu-id="35766-149">Muestra la fecha y hora actual mediante una llamada al método Response.Write () para representar el contenido en el explorador.</span><span class="sxs-lookup"><span data-stu-id="35766-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="35766-150">Los delimitadores de secuencia de comandos &lt;% y %&gt; puede utilizarse para ejecutar una o varias instrucciones.</span><span class="sxs-lookup"><span data-stu-id="35766-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="35766-151">Puesto que se llama a Response.Write () con tanta frecuencia, Microsoft le proporciona un acceso directo para llamar al método Response.Write ().</span><span class="sxs-lookup"><span data-stu-id="35766-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="35766-152">La vista en la lista 3 utiliza los delimitadores &lt;% = y %&gt; como método abreviado para llamar a Response.Write ().</span><span class="sxs-lookup"><span data-stu-id="35766-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="35766-153">**El listado 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="35766-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="35766-154">Puede usar cualquier lenguaje .NET Framework para generar contenido dinámico en una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="35766-155">Normalmente, ll utilice .NET de Visual Basic o C# para escribir sus controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="35766-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="35766-156">Usar aplicaciones auxiliares HTML para generar contenido de vista</span><span class="sxs-lookup"><span data-stu-id="35766-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="35766-157">Para que sea más fácil de agregar contenido a una vista, puede beneficiarse de lo que se denomina un *aplicación auxiliar HTML*.</span><span class="sxs-lookup"><span data-stu-id="35766-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="35766-158">Una aplicación auxiliar de HTML, por lo general, es un método que genera una cadena.</span><span class="sxs-lookup"><span data-stu-id="35766-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="35766-159">Puede usar aplicaciones auxiliares HTML para generar los elementos HTML estándar, como cuadros de texto, vínculos, listas desplegables y cuadros de lista.</span><span class="sxs-lookup"><span data-stu-id="35766-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="35766-160">Por ejemplo, la vista en el listado 4 se aprovecha de las aplicaciones auxiliares HTML tres--los ayudantes BeginForm(), TextBox() y Password()--para generar un inicio de sesión forman (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="35766-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="35766-161">**Listado 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="35766-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="35766-162">[![El cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35766-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="35766-163">**Figura 01**: un formulario de inicio de sesión estándar ([haga clic aquí para ver la imagen a tamaño completo](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="35766-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="35766-164">Todos los métodos de las aplicaciones auxiliares HTML se denominan en la propiedad Html de la vista.</span><span class="sxs-lookup"><span data-stu-id="35766-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="35766-165">Por ejemplo, presentar un cuadro de texto llamando al método Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="35766-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="35766-166">Observe que utilice los delimitadores de secuencia de comandos &lt;% = y %&gt; al llamar a aplicaciones auxiliares de la Html.TextBox() y Html.Password().</span><span class="sxs-lookup"><span data-stu-id="35766-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="35766-167">Estas aplicaciones auxiliares simplemente devuelven una cadena.</span><span class="sxs-lookup"><span data-stu-id="35766-167">These helpers simply return a string.</span></span> <span data-ttu-id="35766-168">Debe llamar a Response.Write () para representar la cadena en el explorador.</span><span class="sxs-lookup"><span data-stu-id="35766-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="35766-169">Uso de métodos auxiliares HTML es opcional.</span><span class="sxs-lookup"><span data-stu-id="35766-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="35766-170">Facilitan su vida al reducir la cantidad de HTML y el script que tiene que escribir.</span><span class="sxs-lookup"><span data-stu-id="35766-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="35766-171">La vista en el listado 5 representa el mismo formato exacto que la vista en el listado 4 sin usar aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="35766-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="35766-172">**Listado 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="35766-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="35766-173">También tiene la opción de crear sus propias aplicaciones auxiliares de HTML.</span><span class="sxs-lookup"><span data-stu-id="35766-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="35766-174">Por ejemplo, puede crear un método de aplicación auxiliar de GridView() que muestra un conjunto de registros de base de datos en una tabla HTML automáticamente.</span><span class="sxs-lookup"><span data-stu-id="35766-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="35766-175">Exploramos en este tema del tutorial **aplicaciones auxiliares de HTML personalizado de creación de**.</span><span class="sxs-lookup"><span data-stu-id="35766-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="35766-176">Usar datos de vista para pasar datos a una vista</span><span class="sxs-lookup"><span data-stu-id="35766-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="35766-177">Usar datos de la vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="35766-178">Piense en datos de la vista como un paquete que se envía a través del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="35766-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="35766-179">Todos los datos que se pasan de un controlador a una vista deben enviarse con este paquete.</span><span class="sxs-lookup"><span data-stu-id="35766-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="35766-180">Por ejemplo, el controlador en el listado 6 agrega un mensaje para ver los datos.</span><span class="sxs-lookup"><span data-stu-id="35766-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="35766-181">**Listado 6 - ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="35766-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="35766-182">El controlador de propiedad ViewData representa una colección de pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="35766-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="35766-183">En el listado 6, el método Index() agrega un elemento a la colección de datos de vista con el nombre de mensaje con el valor Hello World!.</span><span class="sxs-lookup"><span data-stu-id="35766-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="35766-184">Cuando la vista es devuelto por el método Index(), los datos de vista se pasan automáticamente a la vista.</span><span class="sxs-lookup"><span data-stu-id="35766-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="35766-185">La vista en la lista 7 recupera el mensaje de los datos de vista y procesa el mensaje en el explorador.</span><span class="sxs-lookup"><span data-stu-id="35766-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="35766-186">**Listado 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="35766-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="35766-187">Tenga en cuenta que la vista aprovecha las ventajas del método de aplicación auxiliar de HTML Html.Encode() cuando se procesa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="35766-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="35766-188">La aplicación auxiliar HTML Html.Encode() codifica caracteres especiales como &lt; y &gt; en caracteres que son seguros para mostrar en una página web.</span><span class="sxs-lookup"><span data-stu-id="35766-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="35766-189">Cada vez que se representa el contenido que un usuario envía a un sitio Web, se debe codificar el contenido para evitar ataques de inyección de código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="35766-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="35766-190">(Dado que se creó el mensaje nosotros mismos en el ProductController, no queremos t sea realmente necesario codificar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="35766-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="35766-191">Sin embargo, es un hábito recomendable siempre llame al método Html.Encode() al mostrar el contenido recuperado de datos de vista en una vista.)</span><span class="sxs-lookup"><span data-stu-id="35766-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="35766-192">En la lista 7, aprovechamos de datos de vista que se va a pasar un mensaje de cadena simple de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="35766-193">También puede utilizar los datos de vista para pasar otros tipos de datos, como una colección de registros de base de datos, de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="35766-194">Por ejemplo, si desea mostrar el contenido de la tabla de base de datos de productos en una vista, a continuación, podría pasar la colección de base de datos en la vista registra datos.</span><span class="sxs-lookup"><span data-stu-id="35766-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="35766-195">También tiene la opción de pasar los datos de la vista fuertemente tipada de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="35766-196">Exploramos en este tema del tutorial **descripción fuertemente tipados vista de los datos y vistas**.</span><span class="sxs-lookup"><span data-stu-id="35766-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="35766-197">Resumen</span><span class="sxs-lookup"><span data-stu-id="35766-197">Summary</span></span>

<span data-ttu-id="35766-198">Este tutorial proporciona una breve introducción a las vistas de MVC de ASP.NET, ver los datos y aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="35766-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="35766-199">En la primera sección, aprendió a agregar nuevas vistas al proyecto.</span><span class="sxs-lookup"><span data-stu-id="35766-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="35766-200">Ha aprendido que debe agregar una vista a la carpeta correcta para llamarlo desde un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="35766-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="35766-201">A continuación, analizamos el tema de las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="35766-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="35766-202">Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente contenido HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="35766-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="35766-203">Por último, aprendió a aprovechar las ventajas de los datos de vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="35766-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="35766-204">Siguiente</span><span class="sxs-lookup"><span data-stu-id="35766-204">Next</span></span>](creating-custom-html-helpers-cs.md)
