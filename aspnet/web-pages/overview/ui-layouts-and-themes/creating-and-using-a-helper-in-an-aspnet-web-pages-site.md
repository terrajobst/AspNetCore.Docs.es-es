---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Crear y usar una aplicación auxiliar en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye el código y el marcado para rendimiento...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: c2edc10e01f4d2358dd0b0bdf450348f01eb2bfa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811904"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f1139-104">Crear y usar una aplicación auxiliar en un sitio ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f1139-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="f1139-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f1139-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f1139-106">En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="f1139-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="f1139-107">Un *auxiliar* es un componente reutilizable que incluye el código y marcado para realizar una tarea que puede ser complejo o una tarea tediosa.</span><span class="sxs-lookup"><span data-stu-id="f1139-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="f1139-108">**Lo que aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="f1139-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="f1139-109">Cómo crear y usar un complemento simple.</span><span class="sxs-lookup"><span data-stu-id="f1139-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="f1139-110">Estas son las características ASP.NET incorporadas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="f1139-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="f1139-111">El `@helper` sintaxis.</span><span class="sxs-lookup"><span data-stu-id="f1139-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f1139-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f1139-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f1139-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f1139-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f1139-114">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f1139-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="f1139-115">Información general de las aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="f1139-115">Overview of Helpers</span></span>

<span data-ttu-id="f1139-116">Si tiene que realizar las mismas tareas en distintas páginas del sitio, puede usar una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1139-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="f1139-117">Las páginas Web ASP.NET incluye una serie de aplicaciones auxiliares, y hay muchas más que puede descargar e instalar.</span><span class="sxs-lookup"><span data-stu-id="f1139-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="f1139-118">(Se muestra una lista de las aplicaciones auxiliares integradas en ASP.NET Web Pages en el [referencia rápida de la API de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Si ninguna de las aplicaciones auxiliares existentes satisfacen sus necesidades, puede crear su propia aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1139-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="f1139-119">Una aplicación auxiliar le permite usar un bloque de código común en varias páginas.</span><span class="sxs-lookup"><span data-stu-id="f1139-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="f1139-120">Suponga que en la página a menudo desea crear un elemento de la nota que se distinguen los párrafos normales.</span><span class="sxs-lookup"><span data-stu-id="f1139-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="f1139-121">Quizás la nota se crea como un `<div>` elemento que haya un estilo como un cuadro con un borde.</span><span class="sxs-lookup"><span data-stu-id="f1139-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="f1139-122">En lugar de agregar este mismo marcado para una página cada vez que desea mostrar una nota, puede empaquetar el marcado como una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1139-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="f1139-123">A continuación, puede insertar la nota con una sola línea de código en cualquier lugar necesita.</span><span class="sxs-lookup"><span data-stu-id="f1139-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="f1139-124">Usar una aplicación auxiliar así hace que el código en cada una de las páginas más sencillo y más fácil de leer.</span><span class="sxs-lookup"><span data-stu-id="f1139-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="f1139-125">También resulta más fácil de mantener su sitio, porque si necesita cambiar el aspecto de las notas, puede cambiar el marcado en un solo lugar.</span><span class="sxs-lookup"><span data-stu-id="f1139-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="f1139-126">Creación de una aplicación auxiliar</span><span class="sxs-lookup"><span data-stu-id="f1139-126">Creating a Helper</span></span>

<span data-ttu-id="f1139-127">Este procedimiento muestra cómo crear la aplicación auxiliar que crea la nota, como acabamos de describir.</span><span class="sxs-lookup"><span data-stu-id="f1139-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="f1139-128">Este es un ejemplo sencillo, pero la aplicación auxiliar personalizada puede incluir cualquier marcado y código ASP.NET que necesita.</span><span class="sxs-lookup"><span data-stu-id="f1139-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="f1139-129">En la carpeta raíz del sitio Web, cree una carpeta denominada *aplicación\_código*.</span><span class="sxs-lookup"><span data-stu-id="f1139-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="f1139-130">Esto es un nombre de carpeta reservado en ASP.NET donde puede colocar el código para los componentes como aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="f1139-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="f1139-131">En el *aplicación\_código* crear una nueva carpeta *.cshtml* de archivo y asígnele el nombre *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f1139-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="f1139-132">Reemplace el contenido existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f1139-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="f1139-133">El código usa el `@helper` sintaxis para declarar un nueva aplicación auxiliar denominado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f1139-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="f1139-134">Esta aplicación auxiliar determinada le permite pasar un parámetro denominado `content` que puede contener una combinación de texto y marcado.</span><span class="sxs-lookup"><span data-stu-id="f1139-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="f1139-135">La aplicación auxiliar inserta la cadena en el cuerpo de la nota mediante el `@content` variable.</span><span class="sxs-lookup"><span data-stu-id="f1139-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="f1139-136">Tenga en cuenta que el archivo se denomina *MyHelpers.cshtml*, pero la aplicación auxiliar se denomina `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f1139-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="f1139-137">Puede colocar varias de las aplicaciones auxiliares personalizadas en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="f1139-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="f1139-138">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="f1139-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="f1139-139">Uso de la aplicación auxiliar en una página</span><span class="sxs-lookup"><span data-stu-id="f1139-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="f1139-140">En la carpeta raíz, cree un nuevo archivo en blanco denominado *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f1139-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="f1139-141">Agregue el código siguiente al archivo:</span><span class="sxs-lookup"><span data-stu-id="f1139-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="f1139-142">Para llamar a la aplicación auxiliar que creó, use `@` seguido por el nombre de archivo donde es la aplicación auxiliar, un punto y, a continuación, el nombre de la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1139-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="f1139-143">(Si tiene varias carpetas en el *aplicación\_código* carpeta, puede usar la sintaxis `@FolderName.FileName.HelperName` para llamar a esta persona en cualquier anidado nivel de carpeta).</span><span class="sxs-lookup"><span data-stu-id="f1139-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="f1139-144">El texto que agregue comillas entre paréntesis es el texto que se mostrará la aplicación auxiliar como parte de la nota en la página web.</span><span class="sxs-lookup"><span data-stu-id="f1139-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="f1139-145">Guarde la página y ejecútelo en un explorador.</span><span class="sxs-lookup"><span data-stu-id="f1139-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="f1139-146">La aplicación auxiliar genera directamente el elemento tenga en cuenta que se llamó a la aplicación auxiliar: entre los dos párrafos.</span><span class="sxs-lookup"><span data-stu-id="f1139-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Captura de pantalla que muestra la página en el explorador y cómo la aplicación auxiliar genera marcado que coloca un cuadro alrededor del texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="f1139-148">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f1139-148">Additional Resources</span></span>


<span data-ttu-id="f1139-149">[Menú horizontal como una aplicación auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="f1139-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="f1139-150">Por Mike Pope esta entrada de blog muestra cómo crear un menú horizontal como una aplicación auxiliar con marcado, CSS y código.</span><span class="sxs-lookup"><span data-stu-id="f1139-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="f1139-151">[Aprovechar HTML5 en ASP.NET Web Pages aplicaciones auxiliares de WebMatrix y ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1139-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="f1139-152">Esta entrada de blog por Sam Abraham muestra una aplicación auxiliar que representa un HTML5 `Canvas` elemento.</span><span class="sxs-lookup"><span data-stu-id="f1139-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="f1139-153">[La diferencia entre @Helpers y @Functions en WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="f1139-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="f1139-154">Se describe en este artículo blog de Mike Brind `@helper` sintaxis y `@function` sintaxis y cuándo usar cada uno.</span><span class="sxs-lookup"><span data-stu-id="f1139-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
