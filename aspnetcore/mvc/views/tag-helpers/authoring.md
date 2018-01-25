---
title: "Creación de aplicaciones auxiliares de etiquetas en el núcleo de ASP.NET"
author: rick-anderson
description: "Obtenga información acerca de cómo crear aplicaciones auxiliares de etiquetas en ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1f1b2c2e60a1337c15f019185c764d0a9ada1b5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="0712f-103">Aplicaciones auxiliares de etiquetas de autor en ASP.NET Core, un tutorial con ejemplos</span><span class="sxs-lookup"><span data-stu-id="0712f-103">Author Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="0712f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0712f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0712f-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0712f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="0712f-106">Empezar a trabajar con aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="0712f-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="0712f-107">Este tutorial proporciona una introducción a la programación de aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="0712f-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="0712f-108">[Introducción a las aplicaciones auxiliares de etiquetas](intro.md) describe las ventajas que proporciona aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="0712f-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="0712f-109">Una aplicación auxiliar para etiqueta es cualquier clase que implemente la `ITagHelper` interfaz.</span><span class="sxs-lookup"><span data-stu-id="0712f-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="0712f-110">Sin embargo, al crear una aplicación auxiliar de la etiqueta, derivan normalmente de `TagHelper`, esto le brinda acceso a la `Process` método.</span><span class="sxs-lookup"><span data-stu-id="0712f-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="0712f-111">Crear un nuevo proyecto de ASP.NET Core denominado **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="0712f-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="0712f-112">No necesita autenticación para este proyecto.</span><span class="sxs-lookup"><span data-stu-id="0712f-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="0712f-113">Cree una carpeta para almacenar las aplicaciones auxiliares de etiqueta denominado *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="0712f-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="0712f-114">El *TagHelpers* carpeta es *no* necesario, pero es una convención razonable.</span><span class="sxs-lookup"><span data-stu-id="0712f-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="0712f-115">Ahora vamos a empezar a escribir algunas aplicaciones auxiliares de etiqueta simple.</span><span class="sxs-lookup"><span data-stu-id="0712f-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="0712f-116">Una aplicación auxiliar de etiquetas mínima</span><span class="sxs-lookup"><span data-stu-id="0712f-116">A minimal Tag Helper</span></span>

<span data-ttu-id="0712f-117">En esta sección, se escribe una aplicación auxiliar de etiquetas que actualiza una etiqueta de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0712f-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="0712f-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0712f-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="0712f-119">El servidor usará nuestra herramienta de etiqueta de correo electrónico para convertir ese marcado en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="0712f-120">Es decir, una etiqueta delimitadora lo hace un vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0712f-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="0712f-121">Puede hacerlo si está escribiendo un motor de blogs y lo necesite para enviar correo electrónico de marketing, soporte técnico y otros contactos, todo ello en el mismo dominio.</span><span class="sxs-lookup"><span data-stu-id="0712f-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="0712f-122">Agregue el siguiente `EmailTagHelper` clase a la *TagHelpers* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="0712f-123">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="0712f-123">**Notes:**</span></span>
    
    * <span data-ttu-id="0712f-124">Aplicaciones auxiliares de etiquetas usan una convención de nomenclatura que tenga como destino de los elementos de la clase raíz (menos la *TagHelper* parte del nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="0712f-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="0712f-125">En este ejemplo, el nombre de raíz de **correo electrónico**TagHelper es *correo electrónico*, por lo que el `<email>` se destinará la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="0712f-126">Esta convención de nomenclatura debería funcionar para la mayoría de aplicaciones auxiliares de etiquetas, más adelante voy a explicar cómo reemplazarlo.</span><span class="sxs-lookup"><span data-stu-id="0712f-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="0712f-127">La clase `EmailTagHelper` se deriva de `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="0712f-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="0712f-128">La `TagHelper` clase proporciona propiedades y métodos para escribir aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="0712f-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="0712f-129">Reemplazados `Process` método controla lo que hace la aplicación auxiliar de la etiqueta cuando se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="0712f-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="0712f-130">El `TagHelper` clase también proporciona una versión asincrónica (`ProcessAsync`) con los mismos parámetros.</span><span class="sxs-lookup"><span data-stu-id="0712f-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="0712f-131">El parámetro de contexto a `Process` (y `ProcessAsync`) contiene información relacionada con la ejecución de la etiqueta HTML actual.</span><span class="sxs-lookup"><span data-stu-id="0712f-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="0712f-132">El parámetro de salida para `Process` (y `ProcessAsync`) contiene un elemento HTML con estado relacionado con el origen original que se usa para generar una etiqueta HTML y contenido.</span><span class="sxs-lookup"><span data-stu-id="0712f-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="0712f-133">El nombre de la clase tiene un sufijo de **TagHelper**, que es *no* necesario, pero que considera una convención de práctica recomendada.</span><span class="sxs-lookup"><span data-stu-id="0712f-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="0712f-134">Podría declarar la clase como:</span><span class="sxs-lookup"><span data-stu-id="0712f-134">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="0712f-135">Para realizar la `EmailTagHelper` clase disponible para todas las vistas de nuestro Razor, agregue el `addTagHelper` la directiva a la *Views/_ViewImports.cshtml* archivo:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="0712f-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="0712f-136">El código anterior utiliza la sintaxis de comodín para especificar que todas las aplicaciones auxiliares de etiqueta en el ensamblado estará disponibles.</span><span class="sxs-lookup"><span data-stu-id="0712f-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="0712f-137">La primera cadena después `@addTagHelper` especifica la aplicación auxiliar de etiquetas para cargar (utilice "\*" para todas las aplicaciones auxiliares de etiquetas), y la segunda cadena "AuthoringTagHelpers" especifica el ensamblado de la aplicación auxiliar de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="0712f-138">Además, tenga en cuenta que la segunda línea se pondrá en las aplicaciones auxiliares de la etiqueta principal de ASP.NET MVC usando la sintaxis de comodines (esas aplicaciones auxiliares se tratan en [Introducción a las aplicaciones auxiliares de etiquetas](intro.md).) Es el `@addTagHelper` directiva que hace la aplicación auxiliar de etiquetas esté disponible para la vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="0712f-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="0712f-139">Como alternativa, puede proporcionar el nombre completo (FQN) de una aplicación auxiliar de etiquetas tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="0712f-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="0712f-140">Para agregar una aplicación auxiliar de etiqueta a una vista con un FQN, agregue primero la FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, a continuación, el nombre del ensamblado (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="0712f-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="0712f-141">La mayoría de los programadores preferirán utilizar la sintaxis de comodines.</span><span class="sxs-lookup"><span data-stu-id="0712f-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="0712f-142">[Introducción a las aplicaciones auxiliares de etiquetas](intro.md) entra en detalles acerca de la sintaxis de agregar, quitar, jerarquía y caracteres comodín de la aplicación auxiliar de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="0712f-143">Actualizar el marcado en el *Views/Home/Contact.cshtml* archivo con estos cambios:</span><span class="sxs-lookup"><span data-stu-id="0712f-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="0712f-144">Ejecutar la aplicación y usar su explorador favorito para ver el código fuente HTML para que pueda comprobar que las etiquetas de correo electrónico se reemplazan con marcado delimitador (por ejemplo, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="0712f-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="0712f-145">*Compatibilidad con* y *Marketing* se representan como vínculos, pero no tienen un `href` atributo para que sean funcionales.</span><span class="sxs-lookup"><span data-stu-id="0712f-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="0712f-146">Se corregirá en la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="0712f-146">We'll fix that in the next section.</span></span>

<span data-ttu-id="0712f-147">Nota: Al igual que las etiquetas HTML, etiquetas y atributos, nombres de clases y atributos en Razor y C# no son distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0712f-147">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="0712f-148">SetAttribute y SetContent</span><span class="sxs-lookup"><span data-stu-id="0712f-148">SetAttribute and SetContent</span></span>

<span data-ttu-id="0712f-149">En esta sección, actualizaremos el `EmailTagHelper` para que se va a crear una etiqueta de delimitador válido para correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0712f-149">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="0712f-150">Le mantendremos para disponer de información de una vista Razor (en forma de un `mail-to` atributo) y que utilizar al generar el delimitador.</span><span class="sxs-lookup"><span data-stu-id="0712f-150">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="0712f-151">Actualización de la `EmailTagHelper` clase por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-151">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="0712f-152">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="0712f-152">**Notes:**</span></span>

* <span data-ttu-id="0712f-153">Nombres de propiedad y clase la grafía Pascal para aplicaciones auxiliares de etiquetas se convierten en sus [reducir caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="0712f-153">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="0712f-154">Por lo tanto, para usar el `MailTo` atributo, deberá usar `<email mail-to="value"/>` equivalente.</span><span class="sxs-lookup"><span data-stu-id="0712f-154">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="0712f-155">La última línea establece el contenido completado para nuestra herramienta etiqueta mínimamente funcional.</span><span class="sxs-lookup"><span data-stu-id="0712f-155">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="0712f-156">La línea resaltada muestra la sintaxis para agregar atributos:</span><span class="sxs-lookup"><span data-stu-id="0712f-156">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="0712f-157">Este enfoque funciona para el atributo "href" siempre y cuando no existe actualmente en la colección de atributos.</span><span class="sxs-lookup"><span data-stu-id="0712f-157">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="0712f-158">También puede usar el `output.Attributes.Add` método para agregar un atributo de aplicación auxiliar de etiquetas al final de la colección de atributos de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-158">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="0712f-159">Actualizar el marcado en el *Views/Home/Contact.cshtml* archivo con estos cambios:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="0712f-159">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="0712f-160">Ejecute la aplicación y compruebe que éste genera los enlaces correctos.</span><span class="sxs-lookup"><span data-stu-id="0712f-160">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="0712f-161">Si fuera a escribir el correo electrónico etiqueta de autocierre (`<email mail-to="Rick" />`), el resultado final también sería autocierre.</span><span class="sxs-lookup"><span data-stu-id="0712f-161">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="0712f-162">Para habilitar la capacidad para escribir la etiqueta con una etiqueta de apertura (`<email mail-to="Rick">`) debe decorar la clase con la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-162">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="0712f-163">Con una aplicación auxiliar autocierre etiqueta de correo electrónico, el resultado sería `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="0712f-163">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="0712f-164">Las etiquetas de cierre automático no son HTML válido, así, no deseará crear uno, pero podría ser conveniente crear una aplicación auxiliar de etiquetas que es de cierre automático.</span><span class="sxs-lookup"><span data-stu-id="0712f-164">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="0712f-165">Aplicaciones auxiliares de etiquetas establecer el tipo de la `TagMode` propiedad después de leer una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-165">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="0712f-166">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="0712f-166">ProcessAsync</span></span>

<span data-ttu-id="0712f-167">En esta sección, se escribirá una aplicación auxiliar de correo electrónico asincrónica.</span><span class="sxs-lookup"><span data-stu-id="0712f-167">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="0712f-168">Reemplace el `EmailTagHelper` clase con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-168">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="0712f-169">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="0712f-169">**Notes:**</span></span>

    * <span data-ttu-id="0712f-170">Esta versión usa asincrónico `ProcessAsync` método.</span><span class="sxs-lookup"><span data-stu-id="0712f-170">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="0712f-171">Asincrónico `GetChildContentAsync` devuelve un `Task` que contiene el `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="0712f-171">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="0712f-172">Use la `output` para obtener el contenido del elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="0712f-172">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="0712f-173">Realice el siguiente cambio en el *Views/Home/Contact.cshtml* de archivos para la aplicación auxiliar de la etiqueta puede obtener el correo electrónico de destino.</span><span class="sxs-lookup"><span data-stu-id="0712f-173">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="0712f-174">Ejecute la aplicación y compruebe que éste genera vínculos de correo electrónico válida.</span><span class="sxs-lookup"><span data-stu-id="0712f-174">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="0712f-175">RemoveAll, PreContent.SetHtmlContent y PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="0712f-175">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="0712f-176">Agregue el siguiente `BoldTagHelper` clase a la *TagHelpers* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-176">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="0712f-177">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="0712f-177">**Notes:**</span></span>
    
    * <span data-ttu-id="0712f-178">El `[HtmlTargetElement]` atributo pasadas coincidirá con un parámetro de atributo que especifica que cualquier elemento HTML que contiene un atributo HTML denominado "bold", y el `Process` se ejecutará el método de invalidación de la clase.</span><span class="sxs-lookup"><span data-stu-id="0712f-178">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="0712f-179">En nuestro ejemplo, el `Process` método quita el atributo "negrita" y rodea el contenedor marcado con `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="0712f-179">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="0712f-180">Dado que no desea reemplazar el contenido de la etiqueta existente, debe escribir la apertura `<strong>` etiquetar con el `PreContent.SetHtmlContent` método y el cierre `</strong>` etiquetar con los `PostContent.SetHtmlContent` método.</span><span class="sxs-lookup"><span data-stu-id="0712f-180">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="0712f-181">Modificar el *About.cshtml* vista para que contenga un `bold` valor de atributo.</span><span class="sxs-lookup"><span data-stu-id="0712f-181">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="0712f-182">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0712f-182">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="0712f-183">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0712f-183">Run the app.</span></span> <span data-ttu-id="0712f-184">Puede utilizar el explorador que prefiera para inspeccionar el origen y compruebe el marcado.</span><span class="sxs-lookup"><span data-stu-id="0712f-184">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="0712f-185">El `[HtmlTargetElement]` atributo anterior solo tiene como destino la marca HTML que proporciona un nombre de atributo de "bold".</span><span class="sxs-lookup"><span data-stu-id="0712f-185">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="0712f-186">El `<bold>` no se ha modificado el elemento por la aplicación auxiliar de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-186">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="0712f-187">Convierta en comentario la `[HtmlTargetElement]` la línea de atributo y serán destinos `<bold>` etiquetas, es decir, el formato HTML del formulario `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="0712f-187">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="0712f-188">Recuerde que la convención de nomenclatura predeterminado coincidirá con el nombre de clase **negrita**TagHelper a `<bold>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="0712f-188">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="0712f-189">Ejecute la aplicación y compruebe que la `<bold>` etiqueta es procesada por la aplicación auxiliar de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0712f-189">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="0712f-190">Al decorar una clase con varios `[HtmlTargetElement]` da como resultado una operación lógica OR de los destinos de atributos.</span><span class="sxs-lookup"><span data-stu-id="0712f-190">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="0712f-191">Por ejemplo, con el código siguiente, una etiqueta de negrita o un atributo negrita coincidirá.</span><span class="sxs-lookup"><span data-stu-id="0712f-191">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="0712f-192">Cuando se agregan varios atributos a la misma instrucción, el tiempo de ejecución los trata como un AND lógico.</span><span class="sxs-lookup"><span data-stu-id="0712f-192">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="0712f-193">Por ejemplo, en el código siguiente, un elemento HTML debe denominarse "bold" con un atributo denominado "bold" (`<bold bold />`) para hacer coincidir.</span><span class="sxs-lookup"><span data-stu-id="0712f-193">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="0712f-194">También puede usar el `[HtmlTargetElement]` para cambiar el nombre del elemento de destino.</span><span class="sxs-lookup"><span data-stu-id="0712f-194">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="0712f-195">Por ejemplo, si deseara la `BoldTagHelper` destino `<MyBold>` etiquetas, usaría el siguiente atributo:</span><span class="sxs-lookup"><span data-stu-id="0712f-195">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="0712f-196">Pasar un modelo a una aplicación auxiliar de etiqueta</span><span class="sxs-lookup"><span data-stu-id="0712f-196">Pass a model to a Tag Helper</span></span>

1.  <span data-ttu-id="0712f-197">Agregar un *modelos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-197">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="0712f-198">Agregue la clase `WebsiteContext` siguiente a la carpeta *Models*:</span><span class="sxs-lookup"><span data-stu-id="0712f-198">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="0712f-199">Agregue el siguiente `WebsiteInformationTagHelper` clase a la *TagHelpers* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-199">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="0712f-200">**Notas:**</span><span class="sxs-lookup"><span data-stu-id="0712f-200">**Notes:**</span></span>
    
    * <span data-ttu-id="0712f-201">Como se mencionó anteriormente, aplicaciones auxiliares de etiquetas traduce los nombres de clase de C# la grafía Pascal y propiedades para aplicaciones auxiliares de etiquetas en [reducir caso kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="0712f-201">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="0712f-202">Por lo tanto, para usar el `WebsiteInformationTagHelper` en Razor, se escribirá `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="0712f-202">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="0712f-203">No se está identificando explícitamente el elemento de destino con el `[HtmlTargetElement]` atributo, por lo que el valor predeterminado de `website-information` se destinará.</span><span class="sxs-lookup"><span data-stu-id="0712f-203">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="0712f-204">Si ha aplicado el atributo siguiente (tenga en cuenta no es el caso de kebab pero coincide con el nombre de clase):</span><span class="sxs-lookup"><span data-stu-id="0712f-204">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="0712f-205">La etiqueta de caso kebab inferior `<website-information />` coincidirían.</span><span class="sxs-lookup"><span data-stu-id="0712f-205">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="0712f-206">Si desea usar el `[HtmlTargetElement]` atributo, usaría el caso de kebab tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="0712f-206">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="0712f-207">Elementos que se cierre automático no tienen ningún contenido.</span><span class="sxs-lookup"><span data-stu-id="0712f-207">Elements that are self-closing have no content.</span></span> <span data-ttu-id="0712f-208">En este ejemplo, el marcado de Razor usará una etiqueta de cierre automático, pero se creará la aplicación auxiliar de etiqueta un [sección](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (que no cierre automático y se escribe el contenido dentro de la `section` elemento).</span><span class="sxs-lookup"><span data-stu-id="0712f-208">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="0712f-209">Por lo tanto, debe establecer `TagMode` a `StartTagAndEndTag` para escribir la salida.</span><span class="sxs-lookup"><span data-stu-id="0712f-209">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="0712f-210">Como alternativa, puede convertir en comentario la línea donde se establece `TagMode` y escribir marcado con una etiqueta de cierre.</span><span class="sxs-lookup"><span data-stu-id="0712f-210">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="0712f-211">(Marcado de ejemplo se proporciona más adelante en este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="0712f-211">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="0712f-212">El `$` (signo de dólar) en la siguiente línea usa un [interpolan cadena](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="0712f-212">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="0712f-213">Agregue el siguiente marcado para la *About.cshtml* vista.</span><span class="sxs-lookup"><span data-stu-id="0712f-213">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="0712f-214">El marcado resaltado muestra la información del sitio web.</span><span class="sxs-lookup"><span data-stu-id="0712f-214">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="0712f-215">En el marcado de Razor que se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="0712f-215">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="0712f-216">Razor sepa el `info` atributo es una clase, no una cadena, y desea escribir código C#.</span><span class="sxs-lookup"><span data-stu-id="0712f-216">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="0712f-217">Cualquier atributo de aplicación auxiliar de etiqueta no es una cadena debe escribirse sin el `@` caracteres.</span><span class="sxs-lookup"><span data-stu-id="0712f-217">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="0712f-218">Ejecutar la aplicación y navegue hasta la vista acerca de para ver la información del sitio web.</span><span class="sxs-lookup"><span data-stu-id="0712f-218">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="0712f-219">Puede usar el siguiente marcado con una etiqueta de cierre y quite la línea con `TagMode.StartTagAndEndTag` en la aplicación auxiliar de etiqueta:</span><span class="sxs-lookup"><span data-stu-id="0712f-219">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="0712f-220">Aplicación auxiliar de etiquetas de condición</span><span class="sxs-lookup"><span data-stu-id="0712f-220">Condition Tag Helper</span></span>

<span data-ttu-id="0712f-221">La aplicación auxiliar de etiquetas de condición presenta la salida cuando se pasa un valor true.</span><span class="sxs-lookup"><span data-stu-id="0712f-221">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="0712f-222">Agregue el siguiente `ConditionTagHelper` clase a la *TagHelpers* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-222">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="0712f-223">Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="0712f-223">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

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
    
3.  <span data-ttu-id="0712f-224">Reemplace el `Index` método en el `Home` controlador con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-224">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="0712f-225">Ejecutar la aplicación y vaya a la página principal.</span><span class="sxs-lookup"><span data-stu-id="0712f-225">Run the app and browse to the home page.</span></span> <span data-ttu-id="0712f-226">El marcado en el atributo conditional `div` no puede representar.</span><span class="sxs-lookup"><span data-stu-id="0712f-226">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="0712f-227">Anexar la cadena de consulta `?approved=true` a la dirección URL (por ejemplo, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="0712f-227">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="0712f-228">`approved`se establece en true y el atributo conditional aparecerá marcado.</span><span class="sxs-lookup"><span data-stu-id="0712f-228">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="0712f-229">Use la [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador para especificar el atributo de destino en lugar de especificar una cadena como lo hizo con la aplicación auxiliar de etiqueta negrita:</span><span class="sxs-lookup"><span data-stu-id="0712f-229">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="0712f-230">El [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador protegerá el código nunca se debería refactorizar (que queramos cambie el nombre a `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="0712f-230">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="0712f-231">Evitar conflictos de aplicación auxiliar de etiqueta</span><span class="sxs-lookup"><span data-stu-id="0712f-231">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="0712f-232">En esta sección, se escribe un par de aplicaciones auxiliares de etiquetas de la vinculación automática.</span><span class="sxs-lookup"><span data-stu-id="0712f-232">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="0712f-233">La primera reemplazará el marcado que contiene una dirección URL a partir de HTTP a un HTML delimitador etiqueta que contiene la misma dirección URL (y, por tanto, lo que produce un vínculo a la dirección URL).</span><span class="sxs-lookup"><span data-stu-id="0712f-233">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="0712f-234">El segundo se haga lo mismo para una dirección URL a partir de World Wide Web.</span><span class="sxs-lookup"><span data-stu-id="0712f-234">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="0712f-235">Dado que estas aplicaciones auxiliares de dos están estrechamente relacionados y puede refactorizar en el futuro, se podrá mantener en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="0712f-235">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="0712f-236">Agregue el siguiente `AutoLinkerHttpTagHelper` clase a la *TagHelpers* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0712f-236">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="0712f-237">El `AutoLinkerHttpTagHelper` clase destinos `p` elementos y se utiliza [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para crear el delimitador.</span><span class="sxs-lookup"><span data-stu-id="0712f-237">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="0712f-238">Agregue el siguiente marcado al final de la *Views/Home/Contact.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="0712f-238">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="0712f-239">Ejecutar la aplicación y compruebe que la aplicación auxiliar de la etiqueta representa el delimitador correctamente.</span><span class="sxs-lookup"><span data-stu-id="0712f-239">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="0712f-240">Actualización de la `AutoLinker` clase para que incluya la `AutoLinkerWwwTagHelper` que convertirá el texto de World Wide Web en una etiqueta delimitadora que también contiene el texto original de World Wide Web.</span><span class="sxs-lookup"><span data-stu-id="0712f-240">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="0712f-241">El código actualizado se resalta a continuación:</span><span class="sxs-lookup"><span data-stu-id="0712f-241">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="0712f-242">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0712f-242">Run the app.</span></span> <span data-ttu-id="0712f-243">Observe el texto de World Wide Web se representa como un vínculo, pero no es el texto HTTP.</span><span class="sxs-lookup"><span data-stu-id="0712f-243">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="0712f-244">Si coloca un punto de interrupción en ambas clases, puede ver que la clase de aplicación auxiliar de etiqueta HTTP se ejecuta primera.</span><span class="sxs-lookup"><span data-stu-id="0712f-244">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="0712f-245">El problema es que se almacena en caché el resultado de la aplicación auxiliar de etiqueta y, cuando se ejecuta la aplicación auxiliar de etiqueta de World Wide Web, sobrescribe la salida almacenada en caché de la aplicación auxiliar de etiqueta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0712f-245">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="0712f-246">Más adelante en el tutorial veremos cómo controlar el orden en que se ejecutan aplicaciones auxiliares de etiquetas en.</span><span class="sxs-lookup"><span data-stu-id="0712f-246">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="0712f-247">Se corregirá el código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-247">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="0712f-248">En la primera edición de las aplicaciones auxiliares la vinculación automática de etiqueta, que obtuvo el contenido del destino con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-248">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="0712f-249">Es decir, se llama a `GetChildContentAsync` mediante la `TagHelperOutput` pasará el `ProcessAsync` método.</span><span class="sxs-lookup"><span data-stu-id="0712f-249">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="0712f-250">Como se mencionó anteriormente, porque el resultado se almacena en caché, la última etiqueta auxiliares para ejecutar wins.</span><span class="sxs-lookup"><span data-stu-id="0712f-250">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="0712f-251">Había corregido el error con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0712f-251">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="0712f-252">El código anterior comprueba si se ha modificado el contenido y, si existe, obtiene el contenido del búfer de salida.</span><span class="sxs-lookup"><span data-stu-id="0712f-252">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="0712f-253">Ejecute la aplicación y compruebe que los dos vínculos funcionan según lo esperado.</span><span class="sxs-lookup"><span data-stu-id="0712f-253">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="0712f-254">Aunque podría parecer que nuestra herramienta de etiqueta del vinculador automática es correcta y completa, tiene un pequeño problema.</span><span class="sxs-lookup"><span data-stu-id="0712f-254">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="0712f-255">Si la aplicación auxiliar de etiqueta de World Wide Web se ejecuta primera, los vínculos de World Wide Web no será correcta.</span><span class="sxs-lookup"><span data-stu-id="0712f-255">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="0712f-256">Actualice el código y agregue el `Order` sobrecarga para controlar el orden en que la etiqueta se ejecuta en.</span><span class="sxs-lookup"><span data-stu-id="0712f-256">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="0712f-257">El `Order` propiedad determina el orden de ejecución en relación con otras aplicaciones auxiliares de etiquetas como destino el mismo elemento.</span><span class="sxs-lookup"><span data-stu-id="0712f-257">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="0712f-258">El valor de orden predeterminado es cero y se ejecutan en primer lugar instancias con valores más bajos.</span><span class="sxs-lookup"><span data-stu-id="0712f-258">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="0712f-259">El código anterior se garantizará que la aplicación auxiliar de etiqueta HTTP se ejecuta antes de la aplicación auxiliar de etiqueta de World Wide Web.</span><span class="sxs-lookup"><span data-stu-id="0712f-259">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="0712f-260">Cambio `Order` a `MaxValue` y compruebe que el marcado generado para la etiqueta de World Wide Web es incorrecto.</span><span class="sxs-lookup"><span data-stu-id="0712f-260">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="0712f-261">Inspeccionar y recuperar el contenido secundario</span><span class="sxs-lookup"><span data-stu-id="0712f-261">Inspect and retrieve child content</span></span>

<span data-ttu-id="0712f-262">Las aplicaciones auxiliares de etiquetas proporcionan varias propiedades para recuperar el contenido.</span><span class="sxs-lookup"><span data-stu-id="0712f-262">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="0712f-263">El resultado de `GetChildContentAsync` se pueden anexar a `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="0712f-263">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="0712f-264">Puede inspeccionar el resultado de `GetChildContentAsync` con `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="0712f-264">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="0712f-265">Si modifica `output.Content`, el cuerpo de TagHelper no se puede ejecutar o representa a menos que llame a `GetChildContentAsync` igual que en nuestro ejemplo de vinculador automática:</span><span class="sxs-lookup"><span data-stu-id="0712f-265">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="0712f-266">Varias llamadas a `GetChildContentAsync` devuelve el mismo valor y no volver a ejecutar la `TagHelper` cuerpo a menos que se pasa un parámetro false que indica para que no utilice el resultado almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="0712f-266">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
