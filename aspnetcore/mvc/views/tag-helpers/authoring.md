---
title: Crear aplicaciones auxiliares de etiquetas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear aplicaciones auxiliares de etiquetas en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 85809a0f7bfe0adcf535c2be7f5b77e8abd947fd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="51d91-103">Crear aplicaciones auxiliares de etiquetas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51d91-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="51d91-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51d91-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51d91-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51d91-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="51d91-106">Introducción a las aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="51d91-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="51d91-107">En este tutorial se proporciona una introducción a la programación de aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="51d91-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="51d91-108">En [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) se describen las ventajas que proporcionan las aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="51d91-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="51d91-109">Una aplicación auxiliar de etiquetas es una clase que implementa la interfaz `ITagHelper`.</span><span class="sxs-lookup"><span data-stu-id="51d91-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="51d91-110">A pesar de ello, cuando se crea una aplicación auxiliar de etiquetas, normalmente se deriva de `TagHelper`, lo que da acceso al método `Process`.</span><span class="sxs-lookup"><span data-stu-id="51d91-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="51d91-111">Cree un proyecto de ASP.NET Core denominado **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="51d91-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="51d91-112">No necesita autenticación para este proyecto.</span><span class="sxs-lookup"><span data-stu-id="51d91-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="51d91-113">Cree una carpeta para almacenar las aplicaciones auxiliares de etiquetas denominada *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="51d91-114">La carpeta *TagHelpers* *no* es necesaria, pero es una convención razonable.</span><span class="sxs-lookup"><span data-stu-id="51d91-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="51d91-115">Ahora vamos a empezar a escribir algunas aplicaciones auxiliares de etiquetas simples.</span><span class="sxs-lookup"><span data-stu-id="51d91-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="51d91-116">Aplicación auxiliar de etiquetas mínima</span><span class="sxs-lookup"><span data-stu-id="51d91-116">A minimal Tag Helper</span></span>

<span data-ttu-id="51d91-117">En esta sección, escribirá una aplicación auxiliar de etiquetas que actualice una etiqueta de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="51d91-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="51d91-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="51d91-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="51d91-119">El servidor usará nuestra aplicación auxiliar de etiquetas de correo electrónico para convertir ese marcado en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="51d91-120">Es decir, una etiqueta delimitadora lo convierte en un vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="51d91-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="51d91-121">Tal vez le interese si está escribiendo un motor de blogs y necesita que envíe correos electrónicos a contactos de marketing, soporte técnico y de otro tipo, todos ellos en el mismo dominio.</span><span class="sxs-lookup"><span data-stu-id="51d91-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="51d91-122">Agregue la siguiente clase `EmailTagHelper` a la carpeta *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
   <span data-ttu-id="51d91-123">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="51d91-123">**Notes:**</span></span>
    
   * <span data-ttu-id="51d91-124">Las aplicaciones auxiliares de etiquetas usan una convención de nomenclatura que tiene como destino los elementos de la clase raíz (menos la parte *TagHelper* del nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="51d91-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="51d91-125">En este ejemplo, el nombre raíz de **Email**TagHelper es *email*, por lo que el destino será la etiqueta `<email>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="51d91-126">Esta convención de nomenclatura debería funcionar para la mayoría de las aplicaciones auxiliares de etiquetas. Más adelante veremos cómo invalidarla.</span><span class="sxs-lookup"><span data-stu-id="51d91-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
   * <span data-ttu-id="51d91-127">La clase `EmailTagHelper` se deriva de `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="51d91-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="51d91-128">La clase `TagHelper` proporciona métodos y propiedades para escribir aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="51d91-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
   * <span data-ttu-id="51d91-129">El método `Process` invalidado controla lo que hace la aplicación auxiliar de etiquetas cuando se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="51d91-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="51d91-130">La clase `TagHelper` también proporciona una versión asincrónica (`ProcessAsync`) con los mismos parámetros.</span><span class="sxs-lookup"><span data-stu-id="51d91-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
   * <span data-ttu-id="51d91-131">El parámetro de contexto para `Process` (y `ProcessAsync`) contiene información relacionada con la ejecución de la etiqueta HTML actual.</span><span class="sxs-lookup"><span data-stu-id="51d91-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
   * <span data-ttu-id="51d91-132">El parámetro de salida para `Process` (y `ProcessAsync`) contiene un elemento HTML con estado que representa el origen original usado para generar una etiqueta y contenido HTML.</span><span class="sxs-lookup"><span data-stu-id="51d91-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
   * <span data-ttu-id="51d91-133">El nombre de nuestra clase tiene un sufijo **TagHelper**, que *no* es necesario, pero es una convención recomendada.</span><span class="sxs-lookup"><span data-stu-id="51d91-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="51d91-134">Podría declarar la clase de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-134">You could declare the class as:</span></span>
    
   ```csharp
   public class Email : TagHelper
   ```

2. <span data-ttu-id="51d91-135">Para hacer que la clase `EmailTagHelper` esté disponible para todas nuestras vistas de Razor, agregue la directiva `addTagHelper` al archivo *Views/_ViewImports.cshtml*: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="51d91-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
   <span data-ttu-id="51d91-136">El código anterior usa la sintaxis de comodines para especificar que todas las aplicaciones auxiliares de etiquetas del ensamblado estarán disponibles.</span><span class="sxs-lookup"><span data-stu-id="51d91-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="51d91-137">La primera cadena después de `@addTagHelper` especifica la aplicación auxiliar de etiquetas que se va a cargar (use "\*" para todas las aplicaciones auxiliares de etiquetas), mientras que la segunda cadena "AuthoringTagHelpers" especifica el ensamblado en el que se encuentra la aplicación auxiliar de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="51d91-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="51d91-138">Además, tenga en cuenta que la segunda línea incorpora las aplicaciones auxiliares de etiquetas de ASP.NET Core MVC mediante la sintaxis de comodines (esas aplicaciones auxiliares se tratan en el tema [Introducción a las aplicaciones auxiliares de etiquetas](intro.md)). Es la directiva `@addTagHelper` la que hace que la aplicación auxiliar de etiquetas esté disponible para la vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="51d91-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="51d91-139">Como alternativa, puede proporcionar el nombre completo (FQN) de una aplicación auxiliar de etiquetas como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="51d91-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="51d91-140">Para agregar una aplicación auxiliar de etiquetas a una vista con un FQN, agregue primero el FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, después, el nombre del ensamblado (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="51d91-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="51d91-141">La mayoría de los desarrolladores prefiere usar la sintaxis de comodines.</span><span class="sxs-lookup"><span data-stu-id="51d91-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="51d91-142">En [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) se describe en detalle la adición y eliminación de aplicaciones auxiliares de etiquetas, la jerarquía y la sintaxis de comodines.</span><span class="sxs-lookup"><span data-stu-id="51d91-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3. <span data-ttu-id="51d91-143">Actualice el marcado del archivo *Views/Home/Contact.cshtml* con estos cambios:</span><span class="sxs-lookup"><span data-stu-id="51d91-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4. <span data-ttu-id="51d91-144">Ejecute la aplicación y use su explorador favorito para ver el código fuente HTML, a fin de comprobar que las etiquetas de correo electrónico se han reemplazado por un marcado delimitador (por ejemplo, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="51d91-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="51d91-145">*Support* y *Marketing* se representan como vínculos, pero no tienen un atributo `href` que los haga funcionales.</span><span class="sxs-lookup"><span data-stu-id="51d91-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="51d91-146">Esto lo corregiremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="51d91-146">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="51d91-147">SetAttribute y SetContent</span><span class="sxs-lookup"><span data-stu-id="51d91-147">SetAttribute and SetContent</span></span>

<span data-ttu-id="51d91-148">En esta sección, actualizaremos `EmailTagHelper` para que cree una etiqueta delimitadora válida para correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="51d91-148">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="51d91-149">Lo actualizaremos para que tome información de una vista de Razor (en forma de atributo `mail-to`) y la use al generar el delimitador.</span><span class="sxs-lookup"><span data-stu-id="51d91-149">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="51d91-150">Actualice la clase `EmailTagHelper` con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-150">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="51d91-151">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="51d91-151">**Notes:**</span></span>

* <span data-ttu-id="51d91-152">Los nombres de clase y propiedad con grafía Pascal para las aplicaciones auxiliares de etiquetas se convierten a su [grafía kebab en minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="51d91-152">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="51d91-153">Por tanto, para usar el atributo `MailTo`, usará su equivalente `<email mail-to="value"/>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-153">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="51d91-154">La última línea establece el contenido completado para nuestra aplicación auxiliar de etiquetas mínimamente funcional.</span><span class="sxs-lookup"><span data-stu-id="51d91-154">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="51d91-155">La línea resaltada muestra la sintaxis para agregar atributos:</span><span class="sxs-lookup"><span data-stu-id="51d91-155">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="51d91-156">Este enfoque funciona para el atributo "href" siempre y cuando no exista actualmente en la colección de atributos.</span><span class="sxs-lookup"><span data-stu-id="51d91-156">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="51d91-157">También puede usar el método `output.Attributes.Add` para agregar un atributo de aplicación auxiliar de etiquetas al final de la colección de atributos de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="51d91-157">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="51d91-158">Actualice el marcado del archivo *Views/Home/Contact.cshtml* con estos cambios: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="51d91-158">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2. <span data-ttu-id="51d91-159">Ejecute la aplicación y compruebe que genera los vínculos correctos.</span><span class="sxs-lookup"><span data-stu-id="51d91-159">Run the app and verify that it generates the correct links.</span></span>
    
   > [!NOTE]
   > <span data-ttu-id="51d91-160">Si escribe la etiqueta de correo electrónico como de autocierre (`<email mail-to="Rick" />`), la salida final también será de autocierre.</span><span class="sxs-lookup"><span data-stu-id="51d91-160">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="51d91-161">Para habilitar la capacidad de escribir la etiqueta únicamente con una etiqueta de apertura (`<email mail-to="Rick">`) debe decorar la clase con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-161">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   > 
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
   <span data-ttu-id="51d91-162">Con una aplicación auxiliar de etiquetas de correo electrónico de autocierre, el resultado sería `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="51d91-162">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="51d91-163">Las etiquetas delimitadoras de autocierre no son HTML válido, por lo que no le interesa crear una, pero tal vez le convenga crear una aplicación auxiliar de etiquetas de autocierre.</span><span class="sxs-lookup"><span data-stu-id="51d91-163">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="51d91-164">Las aplicaciones auxiliares de etiquetas establecen el tipo de la propiedad `TagMode` después de leer una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="51d91-164">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="51d91-165">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="51d91-165">ProcessAsync</span></span>

<span data-ttu-id="51d91-166">En esta sección, escribiremos una aplicación auxiliar de correo electrónico asincrónica.</span><span class="sxs-lookup"><span data-stu-id="51d91-166">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="51d91-167">Reemplace la clase `EmailTagHelper` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="51d91-167">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="51d91-168">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="51d91-168">**Notes:**</span></span>

   * <span data-ttu-id="51d91-169">Esta versión usa el método `ProcessAsync` asincrónico.</span><span class="sxs-lookup"><span data-stu-id="51d91-169">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="51d91-170">El método `GetChildContentAsync` asincrónico devuelve un valor `Task` que contiene `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="51d91-170">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="51d91-171">Use el parámetro `output` para obtener el contenido del elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="51d91-171">Use the `output` parameter to get contents of the HTML element.</span></span>

2. <span data-ttu-id="51d91-172">Realice el cambio siguiente en el archivo *Views/Home/Contact.cshtml* para que la aplicación auxiliar de etiquetas pueda obtener el correo electrónico de destino.</span><span class="sxs-lookup"><span data-stu-id="51d91-172">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3. <span data-ttu-id="51d91-173">Ejecute la aplicación y compruebe que genera vínculos de correo electrónico válidos.</span><span class="sxs-lookup"><span data-stu-id="51d91-173">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="51d91-174">RemoveAll, PreContent.SetHtmlContent y PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="51d91-174">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="51d91-175">Agregue la siguiente clase `BoldTagHelper` a la carpeta *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-175">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   <span data-ttu-id="51d91-176">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="51d91-176">**Notes:**</span></span>
    
   * <span data-ttu-id="51d91-177">El atributo `[HtmlTargetElement]` pasa un parámetro de atributo que especifica que todos los elementos HTML que contengan un atributo HTML denominado "bold" coincidirán, y se ejecutará el método de invalidación `Process` de la clase.</span><span class="sxs-lookup"><span data-stu-id="51d91-177">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="51d91-178">En nuestro ejemplo, el método `Process` quita el atributo "bold" y rodea el marcado contenedor con `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-178">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
   * <span data-ttu-id="51d91-179">Dado que no le interesa reemplazar el contenido existente de la etiqueta, debe escribir la etiqueta de apertura `<strong>` con el método `PreContent.SetHtmlContent` y la etiqueta de cierre `</strong>` con el método `PostContent.SetHtmlContent`.</span><span class="sxs-lookup"><span data-stu-id="51d91-179">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2. <span data-ttu-id="51d91-180">Modifique la vista *About.cshtml* para que contenga un valor de atributo `bold`.</span><span class="sxs-lookup"><span data-stu-id="51d91-180">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="51d91-181">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="51d91-181">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3. <span data-ttu-id="51d91-182">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51d91-182">Run the app.</span></span> <span data-ttu-id="51d91-183">Puede usar el explorador que prefiera para inspeccionar el origen y comprobar el marcado.</span><span class="sxs-lookup"><span data-stu-id="51d91-183">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="51d91-184">El atributo `[HtmlTargetElement]` anterior solo tiene como destino el marcado HTML que proporciona el nombre de atributo "bold".</span><span class="sxs-lookup"><span data-stu-id="51d91-184">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="51d91-185">La aplicación auxiliar de etiquetas no ha modificado el elemento `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-185">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="51d91-186">Convierta en comentario la línea de atributo `[HtmlTargetElement]` y de forma predeterminada tendrá como destino las etiquetas `<bold>`, es decir, el marcado HTML con formato `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-186">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="51d91-187">Recuerde que la convención de nomenclatura predeterminada hará coincidir el nombre de clase **Bold**TagHelper con las etiquetas `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-187">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="51d91-188">Ejecute la aplicación y compruebe que la aplicación auxiliar de etiquetas procesa la etiqueta `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="51d91-188">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="51d91-189">El proceso de decorar una clase con varios atributos `[HtmlTargetElement]` tiene como resultado una operación OR lógica de los destinos.</span><span class="sxs-lookup"><span data-stu-id="51d91-189">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="51d91-190">Por ejemplo, si se usa el código siguiente, una etiqueta bold o un atributo bold coincidirán.</span><span class="sxs-lookup"><span data-stu-id="51d91-190">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="51d91-191">Cuando se agregan varios atributos a la misma instrucción, el tiempo de ejecución los trata como una operación AND lógica.</span><span class="sxs-lookup"><span data-stu-id="51d91-191">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="51d91-192">Por ejemplo, en el código siguiente, un elemento HTML debe denominarse "bold" con un atributo denominado "bold" (`<bold bold />`) para que coincida.</span><span class="sxs-lookup"><span data-stu-id="51d91-192">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="51d91-193">También puede usar `[HtmlTargetElement]` para cambiar el nombre del elemento de destino.</span><span class="sxs-lookup"><span data-stu-id="51d91-193">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="51d91-194">Por ejemplo, si quiere que `BoldTagHelper` tenga como destino etiquetas `<MyBold>`, use el atributo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-194">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="51d91-195">Pasar un modelo a una aplicación auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="51d91-195">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="51d91-196">Agregue una carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="51d91-196">Add a *Models* folder.</span></span>

2. <span data-ttu-id="51d91-197">Agregue la clase `WebsiteContext` siguiente a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="51d91-197">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3. <span data-ttu-id="51d91-198">Agregue la siguiente clase `WebsiteInformationTagHelper` a la carpeta *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-198">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
   <span data-ttu-id="51d91-199">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="51d91-199">**Notes:**</span></span>
    
   * <span data-ttu-id="51d91-200">Como se ha indicado anteriormente, las aplicaciones auxiliares de etiquetas convierten las propiedades y nombres de clase de C# con grafía Pascal para aplicaciones auxiliares de etiquetas en [grafía kebab en minúsculas](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="51d91-200">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="51d91-201">Por tanto, para usar `WebsiteInformationTagHelper` en Razor, deberá escribir `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="51d91-201">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
   * <span data-ttu-id="51d91-202">No está identificando de manera explícita el elemento de destino con el atributo `[HtmlTargetElement]`, por lo que el destino será el valor predeterminado de `website-information`.</span><span class="sxs-lookup"><span data-stu-id="51d91-202">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="51d91-203">Si ha aplicado el atributo siguiente (tenga en cuenta que no tiene grafía kebab, pero coincide con el nombre de clase):</span><span class="sxs-lookup"><span data-stu-id="51d91-203">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```
    
   <span data-ttu-id="51d91-204">La etiqueta con grafía kebab en minúsculas `<website-information />` no coincidiría.</span><span class="sxs-lookup"><span data-stu-id="51d91-204">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="51d91-205">Si quiere usar el atributo `[HtmlTargetElement]`, debe usar la grafía kebab como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="51d91-205">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```
    
   * <span data-ttu-id="51d91-206">Los elementos que son de autocierre no tienen contenido.</span><span class="sxs-lookup"><span data-stu-id="51d91-206">Elements that are self-closing have no content.</span></span> <span data-ttu-id="51d91-207">En este ejemplo, el marcado de Razor usará una etiqueta de autocierre, pero la aplicación auxiliar de etiquetas creará un elemento [section](http://www.w3.org/TR/html5/sections.html#the-section-element) (que no es de autocierre, y el contenido se escribirá dentro del elemento `section`).</span><span class="sxs-lookup"><span data-stu-id="51d91-207">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="51d91-208">Por tanto, debe establecer `TagMode` en `StartTagAndEndTag` para escribir la salida.</span><span class="sxs-lookup"><span data-stu-id="51d91-208">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="51d91-209">Como alternativa, puede convertir en comentario la línea donde se establece `TagMode` y escribir marcado con una etiqueta de cierre.</span><span class="sxs-lookup"><span data-stu-id="51d91-209">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="51d91-210">(Más adelante en este tutorial se proporciona marcado de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="51d91-210">(Example markup is provided later in this tutorial.)</span></span>
    
   * <span data-ttu-id="51d91-211">El signo de dólar `$` de la línea siguiente usa una [cadena interpolada](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="51d91-211">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

4. <span data-ttu-id="51d91-212">Agregue el marcado siguiente a la vista *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="51d91-212">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="51d91-213">En el marcado resaltado se muestra la información del sitio web.</span><span class="sxs-lookup"><span data-stu-id="51d91-213">The highlighted markup displays the web site information.</span></span>
    
   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
   > [!NOTE]
   > <span data-ttu-id="51d91-214">En el marcado de Razor que se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="51d91-214">In the Razor markup shown below:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   > 
   > <span data-ttu-id="51d91-215">Razor sabe que el atributo `info` es una clase, no una cadena, y usted quiere escribir código de C#.</span><span class="sxs-lookup"><span data-stu-id="51d91-215">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="51d91-216">Todos los atributos de aplicaciones auxiliares de etiquetas que no sean una cadena deben escribirse sin el carácter `@`.</span><span class="sxs-lookup"><span data-stu-id="51d91-216">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5. <span data-ttu-id="51d91-217">Ejecute la aplicación y vaya a la vista About para ver la información del sitio web.</span><span class="sxs-lookup"><span data-stu-id="51d91-217">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="51d91-218">Puede usar el marcado siguiente con una etiqueta de cierre y quitar la línea con `TagMode.StartTagAndEndTag` de la aplicación auxiliar de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="51d91-218">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="51d91-219">Aplicación auxiliar de etiquetas de condición</span><span class="sxs-lookup"><span data-stu-id="51d91-219">Condition Tag Helper</span></span>

<span data-ttu-id="51d91-220">La aplicación auxiliar de etiquetas de condición representa la salida cuando se pasa un valor true.</span><span class="sxs-lookup"><span data-stu-id="51d91-220">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="51d91-221">Agregue la siguiente clase `ConditionTagHelper` a la carpeta *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-221">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2. <span data-ttu-id="51d91-222">Reemplace el contenido del archivo *Views/Home/Index.cshtml* por el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-222">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   ```cshtml
   @using AuthoringTagHelpers.Models
   @model WebsiteContext
    
   @{
       ViewData["Title"] = "Home Page";
   }
    
   <div>
       <h3>Information about our website (outdated):</h3>
       <website-information info=@Model />
       <div condition="@Model.Approved">
           <p>
               This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
               Visit www.contoso.com for more information.
           </p>
       </div>
   </div>
   ```
    
3. <span data-ttu-id="51d91-223">Reemplace el método `Index` del controlador `Home` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-223">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4. <span data-ttu-id="51d91-224">Ejecute la aplicación y vaya a la página principal.</span><span class="sxs-lookup"><span data-stu-id="51d91-224">Run the app and browse to the home page.</span></span> <span data-ttu-id="51d91-225">El marcado del elemento condicional `div` no se representará.</span><span class="sxs-lookup"><span data-stu-id="51d91-225">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="51d91-226">Anexe la cadena de consulta `?approved=true` a la dirección URL (por ejemplo, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="51d91-226">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="51d91-227">`approved` se establece en true y se muestra el marcado condicional.</span><span class="sxs-lookup"><span data-stu-id="51d91-227">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="51d91-228">Use el operador [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) para especificar el atributo de destino en lugar de especificar una cadena, como hizo con la aplicación auxiliar de etiquetas bold:</span><span class="sxs-lookup"><span data-stu-id="51d91-228">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
> 
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
> 
> <span data-ttu-id="51d91-229">El operador [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) protegerá el código si en algún momento debe refactorizarse (tal vez interese cambiar el nombre a `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="51d91-229">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="51d91-230">Evitar conflictos de aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="51d91-230">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="51d91-231">En esta sección, escribirá un par de aplicaciones auxiliares de etiquetas de vinculación automática.</span><span class="sxs-lookup"><span data-stu-id="51d91-231">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="51d91-232">La primera reemplazará el marcado que contiene una dirección URL que empieza con HTTP por una etiqueta delimitadora HTML que contiene la misma dirección URL (y, por tanto, produce un vínculo a la dirección URL).</span><span class="sxs-lookup"><span data-stu-id="51d91-232">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="51d91-233">La segunda hará lo mismo para una dirección URL que empieza con WWW.</span><span class="sxs-lookup"><span data-stu-id="51d91-233">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="51d91-234">Dado que estas dos aplicaciones auxiliares están estrechamente relacionadas y tal vez las refactorice en el futuro, las guardaremos en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="51d91-234">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="51d91-235">Agregue la siguiente clase `AutoLinkerHttpTagHelper` a la carpeta *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="51d91-235">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="51d91-236">La clase `AutoLinkerHttpTagHelper` tiene como destino elementos `p` y usa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para crear el delimitador.</span><span class="sxs-lookup"><span data-stu-id="51d91-236">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2. <span data-ttu-id="51d91-237">Agregue el marcado siguiente al final del archivo *Views/Home/Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51d91-237">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3. <span data-ttu-id="51d91-238">Ejecute la aplicación y compruebe que la aplicación auxiliar de etiquetas representa el delimitador correctamente.</span><span class="sxs-lookup"><span data-stu-id="51d91-238">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4. <span data-ttu-id="51d91-239">Actualice la clase `AutoLinker` para que incluya la aplicación auxiliar de etiquetas `AutoLinkerWwwTagHelper` que convertirá el texto www en una etiqueta delimitadora que también contenga el texto www original.</span><span class="sxs-lookup"><span data-stu-id="51d91-239">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="51d91-240">El código actualizado aparece resaltado a continuación:</span><span class="sxs-lookup"><span data-stu-id="51d91-240">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5. <span data-ttu-id="51d91-241">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51d91-241">Run the app.</span></span> <span data-ttu-id="51d91-242">Observe que el texto www se representa como un vínculo, a diferencia del texto HTTP.</span><span class="sxs-lookup"><span data-stu-id="51d91-242">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="51d91-243">Si coloca un punto de interrupción en ambas clases, verá que la clase de la aplicación auxiliar de etiquetas HTTP se ejecuta primero.</span><span class="sxs-lookup"><span data-stu-id="51d91-243">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="51d91-244">El problema es que la salida de la aplicación auxiliar de etiquetas se almacena en caché y, cuando se ejecuta la aplicación auxiliar de etiquetas WWW, sobrescribe la salida almacenada en caché desde la aplicación auxiliar de etiquetas HTTP.</span><span class="sxs-lookup"><span data-stu-id="51d91-244">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="51d91-245">Más adelante en el tutorial veremos cómo se controla el orden en el que se ejecutan las aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="51d91-245">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="51d91-246">Corregiremos el código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-246">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="51d91-247">La primera vez que editó las aplicaciones auxiliares de etiquetas de vinculación automática, obtuvo el contenido del destino con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-247">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   > 
   > <span data-ttu-id="51d91-248">Es decir, ha llamado a `GetChildContentAsync` mediante la salida `TagHelperOutput` pasada al método `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="51d91-248">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="51d91-249">Como ya se ha indicado, como la salida se almacena en caché, prevalece la última aplicación auxiliar de etiquetas que se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="51d91-249">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="51d91-250">Para corregir el error, ha usado el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="51d91-250">You fixed that problem with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   > 
   > <span data-ttu-id="51d91-251">El código anterior comprueba si se ha modificado el contenido y, en caso afirmativo, obtiene el contenido del búfer de salida.</span><span class="sxs-lookup"><span data-stu-id="51d91-251">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6. <span data-ttu-id="51d91-252">Ejecute la aplicación y compruebe que los dos vínculos funcionan según lo previsto.</span><span class="sxs-lookup"><span data-stu-id="51d91-252">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="51d91-253">Aunque podría parecer que nuestra aplicación auxiliar de etiquetas de vinculación automática es correcta y está completa, tiene un pequeño problema.</span><span class="sxs-lookup"><span data-stu-id="51d91-253">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="51d91-254">Si la aplicación auxiliar de etiquetas WWW se ejecuta en primer lugar, los vínculos www no serán correctos.</span><span class="sxs-lookup"><span data-stu-id="51d91-254">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="51d91-255">Actualice el código mediante la adición de la sobrecarga `Order` para controlar el orden en el que se ejecuta la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="51d91-255">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="51d91-256">La propiedad `Order` determina el orden de ejecución en relación con las demás aplicaciones auxiliares de etiquetas que tienen como destino el mismo elemento.</span><span class="sxs-lookup"><span data-stu-id="51d91-256">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="51d91-257">El valor de orden predeterminado es cero, y se ejecutan en primer lugar las instancias con los valores más bajos.</span><span class="sxs-lookup"><span data-stu-id="51d91-257">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
   <span data-ttu-id="51d91-258">El código anterior garantizará que la aplicación auxiliar de etiquetas HTTP se ejecute antes que la aplicación auxiliar de etiquetas WWW.</span><span class="sxs-lookup"><span data-stu-id="51d91-258">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="51d91-259">Cambie `Order` a `MaxValue` y compruebe que el marcado generado para la etiqueta WWW es incorrecto.</span><span class="sxs-lookup"><span data-stu-id="51d91-259">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="51d91-260">Inspeccionar y recuperar contenido secundario</span><span class="sxs-lookup"><span data-stu-id="51d91-260">Inspect and retrieve child content</span></span>

<span data-ttu-id="51d91-261">Las aplicaciones auxiliares de etiquetas proporcionan varias propiedades para recuperar contenido.</span><span class="sxs-lookup"><span data-stu-id="51d91-261">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="51d91-262">El resultado de `GetChildContentAsync` se pueden anexar a `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="51d91-262">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="51d91-263">Puede inspeccionar el resultado de `GetChildContentAsync` con `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="51d91-263">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="51d91-264">Si modifica `output.Content`, el cuerpo de TagHelper no se ejecutará ni representará a menos que llame a `GetChildContentAsync`, como en nuestro ejemplo de vinculación automática:</span><span class="sxs-lookup"><span data-stu-id="51d91-264">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="51d91-265">Varias llamadas a `GetChildContentAsync` devuelven el mismo valor y no vuelven a ejecutar el cuerpo de `TagHelper` a menos que se pase un parámetro false que indique que no se use el resultado almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="51d91-265">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
