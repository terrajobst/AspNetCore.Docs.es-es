---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "Cómo agregar un controlador | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 878d957344a08450b82b0249d8ca2a205810da4a
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="a7b49-102">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="a7b49-102">Adding a Controller</span></span>
====================
<span data-ttu-id="a7b49-103">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a7b49-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="a7b49-104">MVC es el acrónimo *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="a7b49-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="a7b49-105">MVC es un modelo para desarrollar aplicaciones que están bien diseñada, comprobable y fáciles de mantener.</span><span class="sxs-lookup"><span data-stu-id="a7b49-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="a7b49-106">Las aplicaciones basadas en MVC contienen:</span><span class="sxs-lookup"><span data-stu-id="a7b49-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="a7b49-107">**M** odels: las clases que representan los datos de la aplicación y que usan una lógica de validación para aplicar las reglas de negocios para esos datos.</span><span class="sxs-lookup"><span data-stu-id="a7b49-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="a7b49-108">**V** iews: archivos de plantilla que la aplicación usa para generar dinámicamente las respuestas HTML.</span><span class="sxs-lookup"><span data-stu-id="a7b49-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="a7b49-109">**C** ontrollers: las clases que controlan las solicitudes entrantes de explorador, recuperar datos del modelo y, a continuación, especificar plantillas de vista que devuelven una respuesta al explorador.</span><span class="sxs-lookup"><span data-stu-id="a7b49-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="a7b49-110">Comenzaremos ser que cubren todos estos conceptos en esta serie de tutoriales y muestran cómo utilizarlas para crear una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7b49-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="a7b49-111">En el paso anterior MVC predeterminada se ha seleccionado la plantilla.</span><span class="sxs-lookup"><span data-stu-id="a7b49-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="a7b49-112">Esto crea el hogar, cuenta y administrar controladores de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a7b49-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="a7b49-113">Vamos a empezar a crear una clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="a7b49-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="a7b49-114">En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, haga clic en **agregar**, a continuación, **controlador**.</span><span class="sxs-lookup"><span data-stu-id="a7b49-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="a7b49-115">En el **agregar scaffolding** cuadro de diálogo, haga clic en **controlador de MVC 5 - vacío**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="a7b49-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="a7b49-116">El nuevo controlador el nombre "HelloWorldController" y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="a7b49-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Agregar controlador](adding-a-controller/_static/image3.png)

<span data-ttu-id="a7b49-118">Observe que en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs* y una nueva carpeta *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="a7b49-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="a7b49-119">El controlador está abierto en el IDE.</span><span class="sxs-lookup"><span data-stu-id="a7b49-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="a7b49-120">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a7b49-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="a7b49-121">Los métodos de controlador devolverá una cadena HTML como un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a7b49-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="a7b49-122">El controlador se denomina `HelloWorldController` y el primer método se denomina `Index`.</span><span class="sxs-lookup"><span data-stu-id="a7b49-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="a7b49-123">Vamos a invocarlo desde un explorador.</span><span class="sxs-lookup"><span data-stu-id="a7b49-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="a7b49-124">Ejecute la aplicación (presione F5 o CTRL+F5).</span><span class="sxs-lookup"><span data-stu-id="a7b49-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="a7b49-125">En el explorador, anexar &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a7b49-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="a7b49-126">(Por ejemplo, en la ilustración siguiente, su `http://localhost:1234/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a7b49-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="a7b49-127">En el método anterior, el código devuelve una cadena directamente.</span><span class="sxs-lookup"><span data-stu-id="a7b49-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="a7b49-128">Indicar el sistema para devolver algo de HTML y así ha sido!</span><span class="sxs-lookup"><span data-stu-id="a7b49-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="a7b49-129">ASP.NET MVC invoca las clases de controlador diferente (y métodos de acción diferentes dentro de ellas) dependiendo de la dirección URL entrante.</span><span class="sxs-lookup"><span data-stu-id="a7b49-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="a7b49-130">La lógica de enrutamiento de dirección URL predeterminada utilizada por ASP.NET MVC utiliza un formato similar al siguiente para determinar qué código debe invocar:</span><span class="sxs-lookup"><span data-stu-id="a7b49-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="a7b49-131">Establecer el formato para el enrutamiento en el *aplicación\_Start/RouteConfig.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="a7b49-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="a7b49-132">Cuando ejecute la aplicación y no tiene que proporcionar los segmentos de dirección URL, el valor predeterminado es el controlador "Home" y el método de acción "Índice" especificado en la sección valores predeterminados del código anterior.</span><span class="sxs-lookup"><span data-stu-id="a7b49-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="a7b49-133">La primera parte de la dirección URL determina la clase de controlador que se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="a7b49-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="a7b49-134">Por lo que */HelloWorld* se asigna a la `HelloWorldController` clase.</span><span class="sxs-lookup"><span data-stu-id="a7b49-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="a7b49-135">La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="a7b49-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="a7b49-136">Por lo que */HelloWorld/índice* provocaría que el `Index` método de la `HelloWorldController` clase que se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="a7b49-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="a7b49-137">Tenga en cuenta que sólo tuvimos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a7b49-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="a7b49-138">Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.</span><span class="sxs-lookup"><span data-stu-id="a7b49-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="a7b49-139">La tercera parte del segmento de dirección URL (`Parameters`) es para los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a7b49-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="a7b49-140">Veremos enrutar los datos más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a7b49-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="a7b49-141">Vaya a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="a7b49-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="a7b49-142">El `Welcome` método se ejecuta y devuelve la cadena &quot;éste es el método de acción bienvenida... &quot;.</span><span class="sxs-lookup"><span data-stu-id="a7b49-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="a7b49-143">La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="a7b49-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="a7b49-144">Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción.</span><span class="sxs-lookup"><span data-stu-id="a7b49-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="a7b49-145">Todavía no ha usado el elemento `[Parameters]` de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a7b49-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="a7b49-146">Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información de parámetro de la dirección URL para el controlador (por ejemplo, */HelloWorld/bienvenida? name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="a7b49-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="a7b49-147">Cambiar el `Welcome` método debe incluir dos parámetros, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="a7b49-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="a7b49-148">Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="a7b49-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="a7b49-149">Nota de seguridad: El código anterior se usa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/en-us/library/ee360286(v=vs.110).aspx) para proteger la aplicación de entrada malintencionada (es decir, JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a7b49-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/en-us/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="a7b49-150">Para obtener más información, consulte [Cómo: proteger frente a ataques mediante secuencias de una aplicación Web mediante la aplicación de la codificación de HTML a cadenas](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a7b49-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="a7b49-151">Ejecute la aplicación y busque la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="a7b49-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="a7b49-152">Puede probar valores diferentes para `name` y `numtimes` en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a7b49-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="a7b49-153">El [sistema de enlace de modelo de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="a7b49-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="a7b49-154">En el ejemplo anterior, el segmento de dirección URL ( `Parameters`) no se utiliza, el `name` y `numTimes` parámetros se pasan como [las cadenas de consulta](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="a7b49-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="a7b49-155">El carácter comodín ?</span><span class="sxs-lookup"><span data-stu-id="a7b49-155">The ?</span></span> <span data-ttu-id="a7b49-156">(signo de interrogación) en la dirección URL anterior es un separador y siguen las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a7b49-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="a7b49-157">El carácter &amp; separa las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a7b49-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="a7b49-158">Reemplace el método Bienvenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7b49-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="a7b49-159">Ejecute la aplicación y escriba la dirección URL siguiente:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="a7b49-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="a7b49-160">Esta vez el tercer segmento de dirección URL coincide con el parámetro de ruta `ID.` el `Welcome` método de acción contiene un parámetro (`ID`) que coinciden con la especificación de dirección URL de la `RegisterRoutes` método.</span><span class="sxs-lookup"><span data-stu-id="a7b49-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="a7b49-161">En las aplicaciones de ASP.NET MVC, es más habitual para pasar parámetros como datos de ruta (al igual que hicimos con el identificador anterior) que se pasan como cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a7b49-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="a7b49-162">También puede agregar una ruta para pasar tanto el `name` y `numtimes` en parámetros como datos de ruta en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a7b49-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="a7b49-163">En el *aplicación\_Start\RouteConfig.cs* , agregue la ruta de "Hello":</span><span class="sxs-lookup"><span data-stu-id="a7b49-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="a7b49-164">Ejecute la aplicación y vaya a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="a7b49-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="a7b49-165">Para muchas aplicaciones de MVC, la ruta predeterminada funciona bien.</span><span class="sxs-lookup"><span data-stu-id="a7b49-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="a7b49-166">Aprenderá más adelante en este tutorial para pasar datos mediante el enlazador de modelos y no tendrá que modificar la ruta predeterminada para.</span><span class="sxs-lookup"><span data-stu-id="a7b49-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="a7b49-167">En estos ejemplos, el controlador ha realizado la &quot;VC&quot; parte de MVC, es decir, el trabajo de vista y controlador.</span><span class="sxs-lookup"><span data-stu-id="a7b49-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="a7b49-168">El controlador devuelve HTML directamente.</span><span class="sxs-lookup"><span data-stu-id="a7b49-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="a7b49-169">Normalmente no desea devolver HTML directamente, desde que se vuelve muy complicada al código de controladores.</span><span class="sxs-lookup"><span data-stu-id="a7b49-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="a7b49-170">En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="a7b49-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="a7b49-171">Echemos un vistazo siguiente en cómo podemos hacer esto.</span><span class="sxs-lookup"><span data-stu-id="a7b49-171">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a7b49-172">[Anterior](getting-started.md)
[Siguiente](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a7b49-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
