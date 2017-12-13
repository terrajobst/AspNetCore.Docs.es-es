---
title: "Configurar la localización de objeto portátil"
author: sebastienros
description: "En este artículo se presenta los archivos objeto portátil y se describe los pasos para su uso en una aplicación de ASP.NET Core con el marco de trabajo de Orchard Core."
keywords: "ASP.NET Core, localización, referencia cultural, idioma, objeto portátil"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="18b7e-104">Configurar localización objeto portátil con Orchard Core</span><span class="sxs-lookup"><span data-stu-id="18b7e-104">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="18b7e-105">Por [Sébastien Ros](https://github.com/sebastienros) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="18b7e-105">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="18b7e-106">Este artículo le guía por los pasos para usar archivos de objeto portátil (PO) en una aplicación de ASP.NET Core con el [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="18b7e-106">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="18b7e-107">**Nota:** Orchard Core no es un producto de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="18b7e-107">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="18b7e-108">Por lo tanto, Microsoft no proporciona compatibilidad con esta característica.</span><span class="sxs-lookup"><span data-stu-id="18b7e-108">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="18b7e-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18b7e-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="18b7e-110">¿Qué es un archivo de pedido?</span><span class="sxs-lookup"><span data-stu-id="18b7e-110">What is a PO file?</span></span>

<span data-ttu-id="18b7e-111">Archivos de pedido de compra se distribuyen como archivos de texto que contiene las cadenas traducidas para un idioma determinado.</span><span class="sxs-lookup"><span data-stu-id="18b7e-111">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="18b7e-112">Algunas ventajas de utilizar archivos de pedido de compra en su lugar *.resx* archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="18b7e-112">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="18b7e-113">Los archivos de pedido de compra admiten pluralización; *.resx* archivos no son compatibles con pluralización.</span><span class="sxs-lookup"><span data-stu-id="18b7e-113">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="18b7e-114">Los archivos de pedido de compra no se compilan como *.resx* archivos.</span><span class="sxs-lookup"><span data-stu-id="18b7e-114">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="18b7e-115">Por lo tanto, no son necesarios pasos de compilación y herramientas especializados.</span><span class="sxs-lookup"><span data-stu-id="18b7e-115">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="18b7e-116">Archivos de pedido de compra se funcionen bien con colaboración herramientas de edición en línea.</span><span class="sxs-lookup"><span data-stu-id="18b7e-116">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="18b7e-117">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="18b7e-117">Example</span></span>

<span data-ttu-id="18b7e-118">Este es un archivo de pedido de compra de ejemplo que contiene la traducción de dos cadenas en francés, incluida una con su forma plural:</span><span class="sxs-lookup"><span data-stu-id="18b7e-118">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="18b7e-119">*FR.PO*</span><span class="sxs-lookup"><span data-stu-id="18b7e-119">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="18b7e-120">En este ejemplo se utiliza la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="18b7e-120">This example uses the following syntax:</span></span>

- <span data-ttu-id="18b7e-121">`#:`: Un comentario que indica el contexto de la cadena que se deben traducir.</span><span class="sxs-lookup"><span data-stu-id="18b7e-121">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="18b7e-122">La misma cadena se puede traducir de forma diferente según donde se va a utilizar.</span><span class="sxs-lookup"><span data-stu-id="18b7e-122">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="18b7e-123">`msgid`: La cadena sin traducir.</span><span class="sxs-lookup"><span data-stu-id="18b7e-123">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="18b7e-124">`msgstr`: La cadena traducida.</span><span class="sxs-lookup"><span data-stu-id="18b7e-124">`msgstr`: The translated string.</span></span>

<span data-ttu-id="18b7e-125">En el caso de soporte técnico de pluralización, se pueden definir más entradas.</span><span class="sxs-lookup"><span data-stu-id="18b7e-125">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="18b7e-126">`msgid_plural`: La cadena plural sin traducir.</span><span class="sxs-lookup"><span data-stu-id="18b7e-126">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="18b7e-127">`msgstr[0]`: La cadena traducida en el caso de 0.</span><span class="sxs-lookup"><span data-stu-id="18b7e-127">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="18b7e-128">`msgstr[N]`: La cadena traducida para el caso N.</span><span class="sxs-lookup"><span data-stu-id="18b7e-128">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="18b7e-129">Encontrará la especificación de archivo de pedido de compra [aquí](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="18b7e-129">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="18b7e-130">Configuración de compatibilidad con archivos de pedido de compra en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18b7e-130">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="18b7e-131">En este ejemplo se basa en una aplicación de MVC de ASP.NET Core generada a partir de una plantilla de proyecto de Visual Studio de 2017.</span><span class="sxs-lookup"><span data-stu-id="18b7e-131">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="18b7e-132">Haciendo referencia al paquete</span><span class="sxs-lookup"><span data-stu-id="18b7e-132">Referencing the package</span></span>

<span data-ttu-id="18b7e-133">Agregue una referencia a la `OrchardCore.Localization.Core` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="18b7e-133">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="18b7e-134">Está disponible en [MyGet](https://www.myget.org/) en el origen de paquete siguientes: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="18b7e-134">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="18b7e-135">El *.csproj* archivo ahora contiene una línea similar al siguiente (el número de versión puede variar):</span><span class="sxs-lookup"><span data-stu-id="18b7e-135">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="18b7e-136">Registrar el servicio</span><span class="sxs-lookup"><span data-stu-id="18b7e-136">Registering the service</span></span>

<span data-ttu-id="18b7e-137">Agregar los servicios necesarios para la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="18b7e-137">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="18b7e-138">Agregar el middleware necesario para la `Configure` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="18b7e-138">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="18b7e-139">Agregue el código siguiente a la vista Razor de elección.</span><span class="sxs-lookup"><span data-stu-id="18b7e-139">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="18b7e-140">*About.cshtml* se utiliza en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="18b7e-140">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="18b7e-141">Un `IViewLocalizer` instancia está insertado y se usa para traducir el texto "¡Hello world!".</span><span class="sxs-lookup"><span data-stu-id="18b7e-141">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="18b7e-142">Crear un archivo de pedido de compra</span><span class="sxs-lookup"><span data-stu-id="18b7e-142">Creating a PO file</span></span>

<span data-ttu-id="18b7e-143">Cree un archivo denominado  *<culture code>.po* en la carpeta raíz de aplicación.</span><span class="sxs-lookup"><span data-stu-id="18b7e-143">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="18b7e-144">En este ejemplo, el nombre de archivo es *fr.po* porque se utiliza el idioma francés:</span><span class="sxs-lookup"><span data-stu-id="18b7e-144">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="18b7e-145">Este archivo almacena la cadena de traducir y la cadena traducida en francés.</span><span class="sxs-lookup"><span data-stu-id="18b7e-145">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="18b7e-146">Traducciones revertir a su referencia cultural principal, si es necesario.</span><span class="sxs-lookup"><span data-stu-id="18b7e-146">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="18b7e-147">En este ejemplo, el *fr.po* archivo se utiliza si la referencia cultural solicitada es `fr-FR` o `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="18b7e-147">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="18b7e-148">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="18b7e-148">Testing the application</span></span>

<span data-ttu-id="18b7e-149">Ejecute la aplicación y navegue a la dirección URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="18b7e-149">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="18b7e-150">El texto **Hola a todos!**</span><span class="sxs-lookup"><span data-stu-id="18b7e-150">The text **Hello world!**</span></span> <span data-ttu-id="18b7e-151">se muestra.</span><span class="sxs-lookup"><span data-stu-id="18b7e-151">is displayed.</span></span>

<span data-ttu-id="18b7e-152">Navegue a la dirección URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="18b7e-152">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="18b7e-153">El texto **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="18b7e-153">The text **Bonjour le monde!**</span></span> <span data-ttu-id="18b7e-154">se muestra.</span><span class="sxs-lookup"><span data-stu-id="18b7e-154">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="18b7e-155">Pluralización</span><span class="sxs-lookup"><span data-stu-id="18b7e-155">Pluralization</span></span>

<span data-ttu-id="18b7e-156">Archivos de pedido de compra son compatibles con formatos de pluralización, lo que resulta útil cuando la misma cadena debe traducirse diferente en función de una cardinalidad.</span><span class="sxs-lookup"><span data-stu-id="18b7e-156">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="18b7e-157">Esta tarea se realiza complicada por el hecho de que cada lenguaje define reglas personalizadas para seleccionar qué cadena que se va a utilizar basándose en la cardinalidad.</span><span class="sxs-lookup"><span data-stu-id="18b7e-157">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="18b7e-158">El paquete de localización Orchard proporciona una API para invocar automáticamente estas formas plurales diferentes.</span><span class="sxs-lookup"><span data-stu-id="18b7e-158">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="18b7e-159">Crear archivos de pedido de compra de pluralización</span><span class="sxs-lookup"><span data-stu-id="18b7e-159">Creating pluralization PO files</span></span>

<span data-ttu-id="18b7e-160">Agregue el siguiente contenido a los mencionados anteriormente *fr.po* archivo:</span><span class="sxs-lookup"><span data-stu-id="18b7e-160">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="18b7e-161">Vea [¿qué es un archivo de pedido?](#what-is-a-po-file) para obtener una explicación de lo que representa cada entrada en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="18b7e-161">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="18b7e-162">Agregar un idioma con formas de pluralización diferentes</span><span class="sxs-lookup"><span data-stu-id="18b7e-162">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="18b7e-163">Cadenas de inglés y francés se utilizan en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="18b7e-163">English and French strings were used in the previous example.</span></span> <span data-ttu-id="18b7e-164">Inglés y francés tienen dos formas de pluralización y comparten las mismas reglas de formato, que es la que está asignada una cardinalidad de uno a la primera forma plural.</span><span class="sxs-lookup"><span data-stu-id="18b7e-164">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="18b7e-165">Cualquier otro cardinalidad se asigna a la segunda forma plural.</span><span class="sxs-lookup"><span data-stu-id="18b7e-165">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="18b7e-166">No todos los lenguajes comparten las mismas reglas.</span><span class="sxs-lookup"><span data-stu-id="18b7e-166">Not all languages share the same rules.</span></span> <span data-ttu-id="18b7e-167">Esto se muestra con el idioma checo, que tiene tres formas plurales.</span><span class="sxs-lookup"><span data-stu-id="18b7e-167">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="18b7e-168">Crear la `cs.po` de archivos como se indica a continuación y observe cómo la pluralización tiene tres traducciones diferentes:</span><span class="sxs-lookup"><span data-stu-id="18b7e-168">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="18b7e-169">Para aceptar las adaptaciones checas, agregue `"cs"` a la lista de referencias culturales admitidas en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="18b7e-169">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="18b7e-170">Editar la *Views/Home/About.cshtml* archivo que representen cadenas localizadas, plurales para cardinalidades varios:</span><span class="sxs-lookup"><span data-stu-id="18b7e-170">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="18b7e-171">**Nota:** en un escenario real, utilizaría una variable para representar el número.</span><span class="sxs-lookup"><span data-stu-id="18b7e-171">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="18b7e-172">En este caso, se repita el mismo código con tres valores diferentes para exponer un caso muy específico.</span><span class="sxs-lookup"><span data-stu-id="18b7e-172">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="18b7e-173">Al cambiar las referencias culturales, vea lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="18b7e-173">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="18b7e-174">Para `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="18b7e-174">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="18b7e-175">Para `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="18b7e-175">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="18b7e-176">Para `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="18b7e-176">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="18b7e-177">Tenga en cuenta que para la referencia cultural Checa, las tres traducciones son diferentes.</span><span class="sxs-lookup"><span data-stu-id="18b7e-177">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="18b7e-178">Las referencias culturales de inglés y francés comparten la misma construcción para las últimas dos cadenas traducidas.</span><span class="sxs-lookup"><span data-stu-id="18b7e-178">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="18b7e-179">Tareas avanzadas</span><span class="sxs-lookup"><span data-stu-id="18b7e-179">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="18b7e-180">Cadenas contextualizing</span><span class="sxs-lookup"><span data-stu-id="18b7e-180">Contextualizing strings</span></span>

<span data-ttu-id="18b7e-181">Las aplicaciones a menudo contienen las cadenas que se deben traducir en varios lugares.</span><span class="sxs-lookup"><span data-stu-id="18b7e-181">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="18b7e-182">La misma cadena puede tener una traducción diferentes en determinadas ubicaciones dentro de una aplicación (las vistas de Razor o archivos de clases).</span><span class="sxs-lookup"><span data-stu-id="18b7e-182">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="18b7e-183">Un archivo de pedido de compra admite la noción de un contexto de archivo, que puede utilizarse para clasificar la cadena que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="18b7e-183">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="18b7e-184">Con un contexto de archivo, una cadena se puede traducir de forma diferente, según el contexto de archivo (o la falta de un contexto de archivo).</span><span class="sxs-lookup"><span data-stu-id="18b7e-184">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="18b7e-185">Los servicios de localización del pedido de compra usan el nombre de la clase completa o la vista que se utiliza al traducir una cadena.</span><span class="sxs-lookup"><span data-stu-id="18b7e-185">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="18b7e-186">Esto se logra estableciendo el valor en el `msgctxt` entrada.</span><span class="sxs-lookup"><span data-stu-id="18b7e-186">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="18b7e-187">Considere la posibilidad de una adición bastante irrelevante a la anterior *fr.po* ejemplo.</span><span class="sxs-lookup"><span data-stu-id="18b7e-187">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="18b7e-188">Una vista Razor ubicado en *Views/Home/About.cshtml* se puede definir como el contexto de archivo estableciendo reservado `msgctxt` valor de entrada:</span><span class="sxs-lookup"><span data-stu-id="18b7e-188">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="18b7e-189">Con el `msgctxt` establecido por lo tanto, la traducción de texto se produce cuando se desplace a `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="18b7e-189">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="18b7e-190">La traducción no se produzca cuando se desplace a `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="18b7e-190">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="18b7e-191">Cuando coincide con ninguna entrada específica con un contexto de archivo determinado, mecanismo de reserva del núcleo de huerto busca un archivo de pedido adecuado sin un contexto.</span><span class="sxs-lookup"><span data-stu-id="18b7e-191">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="18b7e-192">Suponiendo que existe no es ningún contexto de archivo específico definido para *Views/Home/Contact.cshtml*, navegación a `/Home/Contact?culture=fr-FR` carga un archivo de pedido de compra como:</span><span class="sxs-lookup"><span data-stu-id="18b7e-192">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="18b7e-193">Cambiar la ubicación de archivos de pedido de compra</span><span class="sxs-lookup"><span data-stu-id="18b7e-193">Changing the location of PO files</span></span>

<span data-ttu-id="18b7e-194">La ubicación predeterminada de los archivos de pedido puede cambiarse en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="18b7e-194">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="18b7e-195">En este ejemplo, los archivos de pedido de compra se cargan desde el *localización* carpeta.</span><span class="sxs-lookup"><span data-stu-id="18b7e-195">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="18b7e-196">Implementar una lógica personalizada para buscar archivos de localización</span><span class="sxs-lookup"><span data-stu-id="18b7e-196">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="18b7e-197">Cuando se necesita una lógica más compleja para buscar archivos de pedido de compra, la `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfaz puede implementarse y registrarse como un servicio.</span><span class="sxs-lookup"><span data-stu-id="18b7e-197">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="18b7e-198">Esto es útil cuando los archivos de pedido de compra se pueden almacenar en diversas ubicaciones o cuando los archivos deben encontrarse dentro de una jerarquía de carpetas.</span><span class="sxs-lookup"><span data-stu-id="18b7e-198">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="18b7e-199">Uso de un idioma predeterminado distinto pluralizan</span><span class="sxs-lookup"><span data-stu-id="18b7e-199">Using a different default pluralized language</span></span>

<span data-ttu-id="18b7e-200">El paquete incluye un `Plural` método de extensión que es específico de dos formas plurales.</span><span class="sxs-lookup"><span data-stu-id="18b7e-200">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="18b7e-201">Para los idiomas que requieren más formas plurales, cree un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="18b7e-201">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="18b7e-202">Con un método de extensión, no necesita proporcionar cualquier archivo de localización para el idioma predeterminado &mdash; las cadenas originales ya están disponibles directamente en el código.</span><span class="sxs-lookup"><span data-stu-id="18b7e-202">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="18b7e-203">Puede usar el modelo más genérico `Plural(int count, string[] pluralForms, params object[] arguments)` sobrecarga que acepta una matriz de cadenas de traducciones.</span><span class="sxs-lookup"><span data-stu-id="18b7e-203">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>