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
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Configurar localización objeto portátil con Orchard Core

Por [Sébastien Ros](https://github.com/sebastienros) y [Scott Addie](https://twitter.com/Scott_Addie)

Este artículo le guía por los pasos para usar archivos de objeto portátil (PO) en una aplicación de ASP.NET Core con el [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Nota:** Orchard Core no es un producto de Microsoft. Por lo tanto, Microsoft no proporciona compatibilidad con esta característica.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>¿Qué es un archivo de pedido?

Archivos de pedido de compra se distribuyen como archivos de texto que contiene las cadenas traducidas para un idioma determinado. Algunas ventajas de utilizar archivos de pedido de compra en su lugar *.resx* archivos incluyen:
- Los archivos de pedido de compra admiten pluralización; *.resx* archivos no son compatibles con pluralización.
- Los archivos de pedido de compra no se compilan como *.resx* archivos. Por lo tanto, no son necesarios pasos de compilación y herramientas especializados.
- Archivos de pedido de compra se funcionen bien con colaboración herramientas de edición en línea.

### <a name="example"></a>Ejemplo

Este es un archivo de pedido de compra de ejemplo que contiene la traducción de dos cadenas en francés, incluida una con su forma plural:

*FR.PO*

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

En este ejemplo se utiliza la sintaxis siguiente:

- `#:`: Un comentario que indica el contexto de la cadena que se deben traducir. La misma cadena se puede traducir de forma diferente según donde se va a utilizar.
- `msgid`: La cadena sin traducir.
- `msgstr`: La cadena traducida.

En el caso de soporte técnico de pluralización, se pueden definir más entradas.

- `msgid_plural`: La cadena plural sin traducir.
- `msgstr[0]`: La cadena traducida en el caso de 0.
- `msgstr[N]`: La cadena traducida para el caso N.

Encontrará la especificación de archivo de pedido de compra [aquí](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Configuración de compatibilidad con archivos de pedido de compra en ASP.NET Core

En este ejemplo se basa en una aplicación de MVC de ASP.NET Core generada a partir de una plantilla de proyecto de Visual Studio de 2017.

### <a name="referencing-the-package"></a>Haciendo referencia al paquete

Agregue una referencia a la `OrchardCore.Localization.Core` paquete NuGet. Está disponible en [MyGet](https://www.myget.org/) en el origen de paquete siguientes: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

El *.csproj* archivo ahora contiene una línea similar al siguiente (el número de versión puede variar):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Registrar el servicio

Agregar los servicios necesarios para la `ConfigureServices` método *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Agregar el middleware necesario para la `Configure` método *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Agregue el código siguiente a la vista Razor de elección. *About.cshtml* se utiliza en este ejemplo.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

Un `IViewLocalizer` instancia está insertado y se usa para traducir el texto "¡Hello world!".

### <a name="creating-a-po-file"></a>Crear un archivo de pedido de compra

Cree un archivo denominado  *<culture code>.po* en la carpeta raíz de aplicación. En este ejemplo, el nombre de archivo es *fr.po* porque se utiliza el idioma francés:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Este archivo almacena la cadena de traducir y la cadena traducida en francés. Traducciones revertir a su referencia cultural principal, si es necesario. En este ejemplo, el *fr.po* archivo se utiliza si la referencia cultural solicitada es `fr-FR` o `fr-CA`.

### <a name="testing-the-application"></a>Probar la aplicación

Ejecute la aplicación y navegue a la dirección URL `/Home/About`. El texto **Hola a todos!** se muestra.

Navegue a la dirección URL `/Home/About?culture=fr-FR`. El texto **Bonjour le monde!** se muestra.

## <a name="pluralization"></a>Pluralización

Archivos de pedido de compra son compatibles con formatos de pluralización, lo que resulta útil cuando la misma cadena debe traducirse diferente en función de una cardinalidad. Esta tarea se realiza complicada por el hecho de que cada lenguaje define reglas personalizadas para seleccionar qué cadena que se va a utilizar basándose en la cardinalidad.

El paquete de localización Orchard proporciona una API para invocar automáticamente estas formas plurales diferentes.

### <a name="creating-pluralization-po-files"></a>Crear archivos de pedido de compra de pluralización

Agregue el siguiente contenido a los mencionados anteriormente *fr.po* archivo:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Vea [¿qué es un archivo de pedido?](#what-is-a-po-file) para obtener una explicación de lo que representa cada entrada en este ejemplo.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Agregar un idioma con formas de pluralización diferentes

Cadenas de inglés y francés se utilizan en el ejemplo anterior. Inglés y francés tienen dos formas de pluralización y comparten las mismas reglas de formato, que es la que está asignada una cardinalidad de uno a la primera forma plural. Cualquier otro cardinalidad se asigna a la segunda forma plural.

No todos los lenguajes comparten las mismas reglas. Esto se muestra con el idioma checo, que tiene tres formas plurales.

Crear la `cs.po` de archivos como se indica a continuación y observe cómo la pluralización tiene tres traducciones diferentes:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Para aceptar las adaptaciones checas, agregue `"cs"` a la lista de referencias culturales admitidas en el `ConfigureServices` método:

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

Editar la *Views/Home/About.cshtml* archivo que representen cadenas localizadas, plurales para cardinalidades varios:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Nota:** en un escenario real, utilizaría una variable para representar el número. En este caso, se repita el mismo código con tres valores diferentes para exponer un caso muy específico.

Al cambiar las referencias culturales, vea lo siguiente:

Para `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Para `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Para `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Tenga en cuenta que para la referencia cultural Checa, las tres traducciones son diferentes. Las referencias culturales de inglés y francés comparten la misma construcción para las últimas dos cadenas traducidas.

## <a name="advanced-tasks"></a>Tareas avanzadas

### <a name="contextualizing-strings"></a>Cadenas contextualizing

Las aplicaciones a menudo contienen las cadenas que se deben traducir en varios lugares. La misma cadena puede tener una traducción diferentes en determinadas ubicaciones dentro de una aplicación (las vistas de Razor o archivos de clases). Un archivo de pedido de compra admite la noción de un contexto de archivo, que puede utilizarse para clasificar la cadena que se va a representar. Con un contexto de archivo, una cadena se puede traducir de forma diferente, según el contexto de archivo (o la falta de un contexto de archivo).

Los servicios de localización del pedido de compra usan el nombre de la clase completa o la vista que se utiliza al traducir una cadena. Esto se logra estableciendo el valor en el `msgctxt` entrada.

Considere la posibilidad de una adición bastante irrelevante a la anterior *fr.po* ejemplo. Una vista Razor ubicado en *Views/Home/About.cshtml* se puede definir como el contexto de archivo estableciendo reservado `msgctxt` valor de entrada:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Con el `msgctxt` establecido por lo tanto, la traducción de texto se produce cuando se desplace a `/Home/About?culture=fr-FR`. La traducción no se produzca cuando se desplace a `/Home/Contact?culture=fr-FR`.

Cuando coincide con ninguna entrada específica con un contexto de archivo determinado, mecanismo de reserva del núcleo de huerto busca un archivo de pedido adecuado sin un contexto. Suponiendo que existe no es ningún contexto de archivo específico definido para *Views/Home/Contact.cshtml*, navegación a `/Home/Contact?culture=fr-FR` carga un archivo de pedido de compra como:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Cambiar la ubicación de archivos de pedido de compra

La ubicación predeterminada de los archivos de pedido puede cambiarse en `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

En este ejemplo, los archivos de pedido de compra se cargan desde el *localización* carpeta.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementar una lógica personalizada para buscar archivos de localización

Cuando se necesita una lógica más compleja para buscar archivos de pedido de compra, la `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfaz puede implementarse y registrarse como un servicio. Esto es útil cuando los archivos de pedido de compra se pueden almacenar en diversas ubicaciones o cuando los archivos deben encontrarse dentro de una jerarquía de carpetas.

### <a name="using-a-different-default-pluralized-language"></a>Uso de un idioma predeterminado distinto pluralizan

El paquete incluye un `Plural` método de extensión que es específico de dos formas plurales. Para los idiomas que requieren más formas plurales, cree un método de extensión. Con un método de extensión, no necesita proporcionar cualquier archivo de localización para el idioma predeterminado &mdash; las cadenas originales ya están disponibles directamente en el código.

Puede usar el modelo más genérico `Plural(int count, string[] pluralForms, params object[] arguments)` sobrecarga que acepta una matriz de cadenas de traducciones.