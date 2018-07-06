---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Uso de la clase TagBuilder para compilar aplicaciones auxiliares HTML (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder. Puede usar la clase TagBuilder para fácilmente...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813231"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="6f3fd-104">Uso de la clase TagBuilder para compilar aplicaciones auxiliares HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="6f3fd-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="6f3fd-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6f3fd-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6f3fd-106">Stephen Walther presenta una clase de utilidad en el marco de MVC de ASP.NET con el nombre de la clase TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="6f3fd-107">Puede usar la clase TagBuilder para compilar fácilmente las etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="6f3fd-108">El marco ASP.NET MVC incluye una clase de utilidad con el nombre de la clase TagBuilder que puede usar al compilar aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="6f3fd-109">La clase TagBuilder, tal como sugiere el nombre de la clase, permite crear fácilmente las etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="6f3fd-110">En este breve tutorial, se proporciona una visión general de la clase TagBuilder y obtenga información sobre cómo usar esta clase al compilar una aplicación auxiliar HTML simple que representa HTML &lt;img&gt; etiquetas.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="6f3fd-111">Información general de la clase TagBuilder</span><span class="sxs-lookup"><span data-stu-id="6f3fd-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="6f3fd-112">La clase TagBuilder está contenida en el espacio de nombres System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="6f3fd-113">Tiene cinco métodos:</span><span class="sxs-lookup"><span data-stu-id="6f3fd-113">It has five methods:</span></span>

- <span data-ttu-id="6f3fd-114">AddCssClass() - le permite agregar un nuevo *clase = ""* a una etiqueta de atributo.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="6f3fd-115">GenerateId() - le permite agregar un atributo de identificador a una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="6f3fd-116">Este método reemplaza automáticamente a los puntos en el Id. (de forma predeterminada, los períodos se reemplazan por caracteres de subrayado)</span><span class="sxs-lookup"><span data-stu-id="6f3fd-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="6f3fd-117">MergeAttribute() - le permite agregar atributos a una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="6f3fd-118">Existen varias sobrecargas de este método.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="6f3fd-119">SetInnerText(): permite establecer el texto interno de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="6f3fd-120">El texto interno es codifica como HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="6f3fd-121">ToString() - le permite representar la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="6f3fd-122">Puede especificar si desea crear una etiqueta normal, una etiqueta de apertura, una etiqueta de cierre o una etiqueta de autocierre.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="6f3fd-123">La clase TagBuilder tiene cuatro propiedades importantes:</span><span class="sxs-lookup"><span data-stu-id="6f3fd-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="6f3fd-124">Atributos: representa todos los atributos de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="6f3fd-125">IdAttributeDotReplacement - representa el carácter utilizado por el método GenerateId() para reemplazar los períodos (el valor predeterminado es un carácter de subrayado).</span><span class="sxs-lookup"><span data-stu-id="6f3fd-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="6f3fd-126">InnerHTML - representa el contenido interno de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="6f3fd-127">Asigne una cadena a esta propiedad *no* HTML codificar la cadena.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="6f3fd-128">TagName - representa el nombre de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="6f3fd-129">Estos métodos y propiedades proporcionan todos los métodos básicos y las propiedades que necesita para crear una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="6f3fd-130">Realmente no es necesario usar la clase TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="6f3fd-131">Podría utilizar una clase StringBuilder en su lugar.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="6f3fd-132">Sin embargo, la clase TagBuilder facilita la vida un poco.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="6f3fd-133">Creación de una aplicación auxiliar HTML de imagen</span><span class="sxs-lookup"><span data-stu-id="6f3fd-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="6f3fd-134">Cuando se crea una instancia de la clase TagBuilder, pase el nombre de la etiqueta a la que desea crear al constructor TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="6f3fd-135">A continuación, puede llamar a métodos, como los métodos AddCssClass y MergeAttribute() para modificar los atributos de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="6f3fd-136">Por último, llame al método ToString() para representar la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="6f3fd-137">Por ejemplo, el listado 1 contiene una aplicación auxiliar de imagen HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="6f3fd-138">La aplicación auxiliar de imagen se implementa internamente con un TagBuilder que representa un elemento HTML &lt;img&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="6f3fd-139">**Listado 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="6f3fd-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="6f3fd-140">La clase en el listado 1 contiene dos métodos sobrecargados estáticos con el nombre de imagen.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="6f3fd-141">Cuando se llama al método Image(), puede pasar un objeto que representa un conjunto de atributos HTML o no.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="6f3fd-142">Tenga en cuenta cómo se usa el método TagBuilder.MergeAttribute() para agregar atributos individuales, como el atributo src en el TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="6f3fd-143">Además, tenga en cuenta cómo se usa el método TagBuilder.MergeAttributes() para agregar una colección de atributos para el TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="6f3fd-144">El método MergeAttributes() acepta un diccionario&lt;cadena, objeto&gt; parámetro.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="6f3fd-145">La clase RouteValueDictionary se utiliza para convertir el objeto que representa la colección de atributos en un diccionario&lt;cadena, objeto&gt;.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="6f3fd-146">Después de crear la aplicación auxiliar de imagen, puede usar la aplicación auxiliar en las vistas de MVC de ASP.NET como cualquiera de las otras estándar las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="6f3fd-147">La vista en el listado 2 usa la aplicación auxiliar de imagen para mostrar dos veces la misma imagen de una consola Xbox (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="6f3fd-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="6f3fd-148">Se llama a la aplicación auxiliar de Image() con y sin una colección de atributos HTML.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="6f3fd-149">**Listado 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6f3fd-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="6f3fd-150">[![El cuadro de diálogo nuevo proyecto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f3fd-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="6f3fd-151">**Figura 01**: uso de la aplicación auxiliar de imagen ([haga clic aquí para ver imagen en tamaño completo](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6f3fd-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="6f3fd-152">Tenga en cuenta que debe importar el espacio de nombres asociado a la aplicación auxiliar de imagen en la parte superior de la vista Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="6f3fd-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="6f3fd-153">La aplicación auxiliar se importa con la directiva siguiente:</span><span class="sxs-lookup"><span data-stu-id="6f3fd-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="6f3fd-154">[Anterior](creating-custom-html-helpers-cs.md)
> [Siguiente](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6f3fd-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
