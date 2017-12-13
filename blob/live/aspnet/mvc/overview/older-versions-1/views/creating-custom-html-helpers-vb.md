---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Crear aplicaciones auxiliares HTML personalizado (VB) | Documentos de Microsoft
author: microsoft
description: "El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de la aplicación auxiliar HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: e389a03228995ce0a6926a53af38f26ad51372d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="ee386-104">Crear aplicaciones auxiliares HTML personalizado (VB)</span><span class="sxs-lookup"><span data-stu-id="ee386-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="ee386-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ee386-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ee386-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="ee386-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="ee386-107">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="ee386-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="ee386-108">Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="ee386-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="ee386-109">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="ee386-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="ee386-110">Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="ee386-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="ee386-111">En la primera parte de este tutorial, describen algunas de las aplicaciones auxiliares de HTML existente incluido con el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ee386-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="ee386-112">A continuación, describen dos métodos de creación de aplicaciones auxiliares de HTML personalizado: explican cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método compartido y mediante la creación de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="ee386-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="ee386-113">Descripción de las aplicaciones auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="ee386-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="ee386-114">Una aplicación auxiliar de HTML es simplemente un método que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="ee386-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="ee386-115">La cadena puede representar cualquier tipo de contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="ee386-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="ee386-116">Por ejemplo, puede usar aplicaciones auxiliares HTML para representar las etiquetas HTML estándar como HTML `<input>` y `<img>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="ee386-117">También puede usar aplicaciones auxiliares HTML para representar el contenido más complejo, como una franja de pestañas o una tabla HTML de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee386-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="ee386-118">El marco de ASP.NET MVC incluye el siguiente conjunto de aplicaciones auxiliares de HTML estándar (no es una lista completa):</span><span class="sxs-lookup"><span data-stu-id="ee386-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="ee386-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="ee386-119">Html.ActionLink()</span></span>
- <span data-ttu-id="ee386-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="ee386-120">Html.BeginForm()</span></span>
- <span data-ttu-id="ee386-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="ee386-121">Html.CheckBox()</span></span>
- <span data-ttu-id="ee386-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="ee386-122">Html.DropDownList()</span></span>
- <span data-ttu-id="ee386-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="ee386-123">Html.EndForm()</span></span>
- <span data-ttu-id="ee386-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="ee386-124">Html.Hidden()</span></span>
- <span data-ttu-id="ee386-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="ee386-125">Html.ListBox()</span></span>
- <span data-ttu-id="ee386-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="ee386-126">Html.Password()</span></span>
- <span data-ttu-id="ee386-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="ee386-127">Html.RadioButton()</span></span>
- <span data-ttu-id="ee386-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="ee386-128">Html.TextArea()</span></span>
- <span data-ttu-id="ee386-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="ee386-129">Html.TextBox()</span></span>

<span data-ttu-id="ee386-130">Por ejemplo, tenga en cuenta la forma en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="ee386-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="ee386-131">Este formulario se representa con la Ayuda de dos de las aplicaciones auxiliares de HTML estándar (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="ee386-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="ee386-132">Este formulario usa el `Html.BeginForm()` y `Html.TextBox()` métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="ee386-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="ee386-133">[![Representa la página con las aplicaciones auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee386-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="ee386-134">**Figura 01**: página se representa con aplicaciones auxiliares HTML ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee386-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="ee386-135">**Lista 1:`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ee386-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="ee386-136">El `Html.BeginForm()` método auxiliar se utiliza para crear el código HTML de apertura y cierre `<form>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="ee386-137">Tenga en cuenta que el `Html.BeginForm()` se denomina método dentro de un uso de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="ee386-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="ee386-138">La instrucción using garantiza que la `<form>` etiqueta se cierra al final del uso de bloque.</span><span class="sxs-lookup"><span data-stu-id="ee386-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="ee386-139">Si lo prefiere, en lugar de crear un uso de bloque, puede llamar al método de aplicación auxiliar Html.EndForm() para cerrar la `<form>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="ee386-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="ee386-140">Usar cualquier enfoque para la creación de apertura y cierre de `<form>` etiqueta que parezca más intuitivo.</span><span class="sxs-lookup"><span data-stu-id="ee386-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="ee386-141">El `Html.TextBox()` métodos auxiliares que se usan en el listado 1 para representar HTML `<input>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="ee386-142">Si selecciona Ver código fuente en el explorador, a continuación, vea el código fuente HTML en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="ee386-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="ee386-143">Observe que el origen contiene etiquetas HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="ee386-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee386-144">Tenga en cuenta que la `Html.TextBox()`-HTML auxiliar se representa con `<%= %>` etiquetas en lugar de `<% %>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="ee386-145">Si no incluye el signo igual, a continuación, nada se representa en el explorador.</span><span class="sxs-lookup"><span data-stu-id="ee386-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="ee386-146">El marco de MVC de ASP.NET contiene un pequeño conjunto de aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="ee386-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="ee386-147">Probablemente, debe extender el marco MVC con las aplicaciones auxiliares HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="ee386-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="ee386-148">En el resto de este tutorial, aprenderá dos métodos de creación de aplicaciones auxiliares de HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="ee386-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="ee386-149">**La lista 2:`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="ee386-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="ee386-150">Crear aplicaciones auxiliares HTML con métodos compartidos</span><span class="sxs-lookup"><span data-stu-id="ee386-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="ee386-151">Es la manera más fácil de crear una nueva aplicación auxiliar de HTML crear un método compartido que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="ee386-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="ee386-152">Por ejemplo, imagine que decide crear una nueva aplicación auxiliar de HTML que representa un elemento HTML `<label>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="ee386-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="ee386-153">Puede utilizar la clase en el listado 2 para representar un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="ee386-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="ee386-154">**La lista 2:`Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="ee386-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="ee386-155">No hay nada especial acerca de la clase en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="ee386-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="ee386-156">El `Label()` método simplemente devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="ee386-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="ee386-157">La vista de índice modificada en el listado 3 utiliza el `LabelHelper` para representar HTML `<label>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="ee386-158">Tenga en cuenta que la vista incluye una `<%@ imports %>` directiva que importa el espacio de nombres Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="ee386-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="ee386-159">**La lista 2:`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ee386-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="ee386-160">Crear aplicaciones auxiliares HTML con los métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="ee386-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="ee386-161">Si desea crear aplicaciones auxiliares de HTML que solo funcionan como las aplicaciones auxiliares de HTML estándar incluidos en el marco de MVC de ASP.NET, a continuación, debe crear métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="ee386-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="ee386-162">Métodos de extensión permiten agregar nuevos métodos a una clase existente.</span><span class="sxs-lookup"><span data-stu-id="ee386-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="ee386-163">Cuando se crea un método de aplicación auxiliar HTML, agregar nuevos métodos para la `HtmlHelper` clase representada por la propiedad de la vista Html.</span><span class="sxs-lookup"><span data-stu-id="ee386-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="ee386-164">El módulo de Visual Basic en el listado 3 agrega un método de extensión denominado `Label()` a la `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="ee386-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="ee386-165">Hay un par de cosas que debe tener en cuenta acerca de este módulo.</span><span class="sxs-lookup"><span data-stu-id="ee386-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="ee386-166">En primer lugar, tenga en cuenta que el módulo se decora con el `<Extension()>` atributo.</span><span class="sxs-lookup"><span data-stu-id="ee386-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="ee386-167">Para poder usar este atributo, debe importar el `System.Runtime.CompilerServices` espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="ee386-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="ee386-168">En segundo lugar, observe que el primer parámetro de la `Label()` método representa la `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="ee386-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="ee386-169">El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.</span><span class="sxs-lookup"><span data-stu-id="ee386-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="ee386-170">**Enumerar 3:`Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="ee386-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="ee386-171">Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Visual Studio Intellisense al igual que todos los otros métodos de una clase (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="ee386-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="ee386-172">La única diferencia es que dicha extensión se muestran métodos con un símbolo especial junto a ellos (un icono de flecha hacia abajo).</span><span class="sxs-lookup"><span data-stu-id="ee386-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="ee386-173">[![Mediante el método de extensión Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ee386-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="ee386-174">**Figura 02**: mediante el método de extensión Html.Label() ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ee386-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="ee386-175">La vista de índice modificada en el listado 4 utiliza el método de extensión Html.Label() para representar todos sus &lt;etiqueta&gt; etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ee386-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="ee386-176">**Enumerar 4:`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="ee386-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="ee386-177">Resumen</span><span class="sxs-lookup"><span data-stu-id="ee386-177">Summary</span></span>

<span data-ttu-id="ee386-178">En este tutorial, ha aprendido dos métodos de creación de aplicaciones auxiliares de HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="ee386-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="ee386-179">En primer lugar, aprendió a crear una personalizada `Label()` aplicación auxiliar de HTML mediante la creación de un método compartido que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="ee386-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="ee386-180">A continuación, ha aprendido cómo crear una personalizada `Label()` método de aplicación auxiliar HTML mediante la creación de un método de extensión en la `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="ee386-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="ee386-181">En este tutorial, centra en la creación de un método de aplicación auxiliar HTML muy simple.</span><span class="sxs-lookup"><span data-stu-id="ee386-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="ee386-182">Tenga en cuenta que una aplicación auxiliar de HTML puede ser tan complicada como desee.</span><span class="sxs-lookup"><span data-stu-id="ee386-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="ee386-183">Puede compilar aplicaciones auxiliares de HTML que representa el contenido enriquecido, como vistas de árbol, los menús o tablas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee386-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ee386-184">[Anterior](asp-net-mvc-views-overview-vb.md)
[Siguiente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ee386-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
