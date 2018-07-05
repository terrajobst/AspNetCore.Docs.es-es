---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Uso de la clase TagBuilder para compilar aplicaciones auxiliares HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder. Puede usar la clase TagBuilder para fácilmente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: b95073a53e73ebe4035ef9b8dcdf84dc3c4febee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370962"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="a3617-104">Uso de la clase TagBuilder para compilar aplicaciones auxiliares HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="a3617-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="a3617-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a3617-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a3617-106">Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a3617-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="a3617-107">Puede usar la clase TagBuilder para compilar fácilmente las etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="a3617-108">El marco ASP.NET MVC incluye una clase de utilidad con el nombre de la clase TagBuilder que puede usar al compilar aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="a3617-109">La clase TagBuilder, tal como sugiere el nombre de la clase, permite crear fácilmente las etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="a3617-110">En este breve tutorial, se proporciona una visión general de la clase TagBuilder y obtenga información sobre cómo usar esta clase al compilar una aplicación auxiliar HTML simple que representa HTML &lt;img&gt; etiquetas.</span><span class="sxs-lookup"><span data-stu-id="a3617-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="a3617-111">Información general de la clase TagBuilder</span><span class="sxs-lookup"><span data-stu-id="a3617-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="a3617-112">La clase TagBuilder está contenida en el espacio de nombres System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="a3617-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="a3617-113">Tiene cinco métodos:</span><span class="sxs-lookup"><span data-stu-id="a3617-113">It has five methods:</span></span>

- <span data-ttu-id="a3617-114">AddCssClass(): le permite agregar un nuevo *clase = ""* a una etiqueta de atributo.</span><span class="sxs-lookup"><span data-stu-id="a3617-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="a3617-115">GenerateId(): le permite agregar un atributo de identificador a una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="a3617-116">Este método reemplaza automáticamente a los puntos en el Id. (de forma predeterminada, los períodos se reemplazan por caracteres de subrayado)</span><span class="sxs-lookup"><span data-stu-id="a3617-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="a3617-117">MergeAttribute(): le permite agregar atributos a una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="a3617-118">Existen varias sobrecargas de este método.</span><span class="sxs-lookup"><span data-stu-id="a3617-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="a3617-119">SetInnerText(): permite establecer el texto interno de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="a3617-120">El texto interno es codifica como HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="a3617-121">ToString() – le permite representar la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="a3617-122">Puede especificar si desea crear una etiqueta normal, una etiqueta de apertura, una etiqueta de cierre o una etiqueta de autocierre.</span><span class="sxs-lookup"><span data-stu-id="a3617-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="a3617-123">La clase TagBuilder tiene cuatro propiedades importantes:</span><span class="sxs-lookup"><span data-stu-id="a3617-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="a3617-124">Atributos: representa todos los atributos de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="a3617-125">IdAttributeDotReplacement: representa el carácter utilizado por el método GenerateId() para reemplazar los períodos (el valor predeterminado es un carácter de subrayado).</span><span class="sxs-lookup"><span data-stu-id="a3617-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="a3617-126">InnerHTML: representa el contenido interno de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="a3617-127">Asigne una cadena a esta propiedad *no* HTML codificar la cadena.</span><span class="sxs-lookup"><span data-stu-id="a3617-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="a3617-128">TagName: representa el nombre de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="a3617-129">Estos métodos y propiedades proporcionan todos los métodos básicos y las propiedades que necesita para crear una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="a3617-130">Realmente no es necesario usar la clase TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a3617-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="a3617-131">Podría utilizar una clase StringBuilder en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a3617-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="a3617-132">Sin embargo, la clase TagBuilder facilita la vida un poco.</span><span class="sxs-lookup"><span data-stu-id="a3617-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="a3617-133">Creación de una aplicación auxiliar HTML de imagen</span><span class="sxs-lookup"><span data-stu-id="a3617-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="a3617-134">Cuando se crea una instancia de la clase TagBuilder, pase el nombre de la etiqueta a la que desea crear al constructor TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a3617-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="a3617-135">A continuación, puede llamar a métodos, como los métodos AddCssClass y MergeAttribute() para modificar los atributos de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="a3617-136">Por último, llame al método ToString() para representar la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="a3617-137">Por ejemplo, el listado 1 contiene una aplicación auxiliar de imagen HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="a3617-138">La aplicación auxiliar de imagen se implementa internamente con un TagBuilder que representa un elemento HTML &lt;img&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a3617-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="a3617-139">**Listado 1 – Helpers\ImageHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="a3617-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="a3617-140">El módulo en el listado 1 contiene dos métodos sobrecargados denominados Image().</span><span class="sxs-lookup"><span data-stu-id="a3617-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="a3617-141">Cuando se llama al método Image(), puede pasar un objeto que representa un conjunto de atributos HTML o no.</span><span class="sxs-lookup"><span data-stu-id="a3617-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="a3617-142">Tenga en cuenta cómo se usa el método TagBuilder.MergeAttribute() para agregar atributos individuales, como el atributo src en el TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a3617-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="a3617-143">Además, tenga en cuenta cómo se usa el método TagBuilder.MergeAttributes() para agregar una colección de atributos para el TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a3617-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="a3617-144">El método MergeAttributes() acepta un diccionario&lt;cadena, objeto&gt; parámetro.</span><span class="sxs-lookup"><span data-stu-id="a3617-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="a3617-145">La clase RouteValueDictionary se utiliza para convertir el objeto que representa la colección de atributos en un diccionario&lt;cadena, objeto&gt;.</span><span class="sxs-lookup"><span data-stu-id="a3617-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="a3617-146">Después de crear la aplicación auxiliar de imagen, puede usar la aplicación auxiliar en las vistas de MVC de ASP.NET como cualquiera de las otras estándar las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="a3617-147">La vista en el listado 2 usa la aplicación auxiliar de imagen para mostrar dos veces la misma imagen de una consola Xbox (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="a3617-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="a3617-148">Se llama a la aplicación auxiliar de Image() con y sin una colección de atributos HTML.</span><span class="sxs-lookup"><span data-stu-id="a3617-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="a3617-149">**Listado 2 – Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a3617-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="a3617-150">[![El cuadro de diálogo nuevo proyecto](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3617-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="a3617-151">**Figura 01**: uso de la aplicación auxiliar de imagen ([haga clic aquí para ver imagen en tamaño completo](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a3617-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="a3617-152">Tenga en cuenta que debe importar el espacio de nombres asociado a la aplicación auxiliar de imagen en la parte superior de la vista Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="a3617-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="a3617-153">La aplicación auxiliar se importa con la directiva siguiente:</span><span class="sxs-lookup"><span data-stu-id="a3617-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="a3617-154">En una aplicación de Visual Basic, el espacio de nombres predeterminado es el mismo que el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a3617-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3617-155">[Anterior](creating-custom-html-helpers-vb.md)
> [Siguiente](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3617-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
