---
title: Aplicaciones auxiliares de etiquetas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre qué son las aplicaciones auxiliares de etiquetas y cómo se usan en ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: c2af9099fe439e1cdbf9ba86ffae3b2b0f67391e
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751701"
---
# <a name="tag-helpers-in-aspnet-core"></a>Aplicaciones auxiliares de etiquetas en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>¿Qué son las aplicaciones auxiliares de etiquetas?

Las aplicaciones auxiliares de etiquetas permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor. Por ejemplo, la aplicación auxiliar `ImageTagHelper` integrada puede anexar un número de versión al nombre de imagen. Cada vez que la imagen cambia, el servidor genera una nueva versión única para la imagen, lo que garantiza que los clientes puedan obtener la imagen actual (en lugar de una imagen obsoleta almacenada en caché). Hay muchas aplicaciones auxiliares de etiquetas integradas para tareas comunes (como la creación de formularios, vínculos, carga de activos, etc.) y existen muchas más a disposición en repositorios públicos de GitHub y como paquetes NuGet. Las aplicaciones auxiliares de etiquetas se crean en C# y tienen como destino elementos HTML en función del nombre de elemento, el nombre de atributo o la etiqueta principal. Por ejemplo, la aplicación auxiliar `LabelTagHelper` integrada puede tener como destino el elemento HTML `<label>` cuando se aplican atributos `LabelTagHelper`. Si está familiarizado con las [aplicaciones auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), las aplicaciones auxiliares de etiquetas reducen las transiciones explícitas entre HTML y C# en las vistas de Razor. En muchos casos, las aplicaciones auxiliares HTML proporcionan un método alternativo para una aplicación auxiliar de etiquetas específica, pero es importante tener en cuenta que las aplicaciones auxiliares de etiquetas no reemplazan a las aplicaciones auxiliares HTML y que no hay una aplicación auxiliar de etiquetas para cada aplicación auxiliar HTML. En [Comparación entre las aplicaciones auxiliares de etiquetas y las aplicaciones auxiliares HTML](#tag-helpers-compared-to-html-helpers) se explican las diferencias con más detalle.

## <a name="what-tag-helpers-provide"></a>¿Qué proporcionan las aplicaciones auxiliares de etiquetas?

**Una experiencia de desarrollo compatible con HTML** La mayor parte del marcado de Razor con aplicaciones auxiliares de etiquetas es similar a HTML estándar. Los diseñadores de front-end familiarizados con HTML, CSS y JavaScript pueden editar Razor sin tener que aprender la sintaxis Razor de C#.

**Un entorno de IntelliSense enriquecido para crear marcado HTML y Razor** Este es un marcado contraste con las aplicaciones auxiliares HTML, el método anterior para la creación en el lado servidor de marcado en vistas de Razor. En [Comparación entre las aplicaciones auxiliares de etiquetas y las aplicaciones auxiliares HTML](#tag-helpers-compared-to-html-helpers) se explican las diferencias con más detalle. En [Compatibilidad de IntelliSense con aplicaciones auxiliares de etiquetas](#intellisense-support-for-tag-helpers) se explica el entorno de IntelliSense. Incluso los desarrolladores que tienen experiencia con la sintaxis Razor de C# son más productivos cuando usan aplicaciones auxiliares de etiquetas que al escribir marcado de Razor de C#.

**Una forma de ser más productivo y generar código más sólido, confiable y fácil de mantener con información que solo está disponible en el servidor** Por ejemplo, lo habitual a la hora de actualizar las imágenes era cambiar el nombre de la imagen cuando se modificaba. Las imágenes debían almacenarse en caché de forma activa por motivos de rendimiento y, a menos que se cambiase el nombre de una imagen, se corría el riesgo de que los clientes obtuviesen una copia obsoleta. Antes, después de editar una imagen, era necesario cambiarle el nombre y actualizar todas las referencias a la imagen en la aplicación web. Esto no solo exigía mucho trabajo, sino que era propenso a errores (por ejemplo, omitir una referencia, incluir accidentalmente una cadena incorrecta, etc.). La aplicación auxiliar `ImageTagHelper` integrada puede hacerlo automáticamente. `ImageTagHelper` puede anexar un número de versión al nombre de la imagen, por lo que cada vez que la imagen cambia, el servidor genera automáticamente una nueva versión única de la imagen. Esto garantiza que los clientes obtengan la imagen actual. Esta solidez y ahorro de trabajo se consiguen de forma gratuita mediante el uso de `ImageTagHelper`.

La mayoría de las aplicaciones auxiliares de etiquetas integradas tienen como destino elementos HTML estándar y proporcionan atributos del lado servidor del elemento. Por ejemplo, el elemento `<input>` que muchas vistas usan en la carpeta *Views/Account* contiene el atributo `asp-for`. Este atributo extrae el nombre de la propiedad de modelo especificada en el HTML representado. Pensemos en una vista de Razor con el siguiente modelo:

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

El siguiente marcado de Razor:

```cshtml
<label asp-for="Movie.Title"></label>
```

Se genera el siguiente código HTML:

```html
<label for="Movie_Title">Title</label>
```

El atributo `asp-for` está disponible por medio de la propiedad `For` en [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0). Vea [Creación de aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/authoring) para más información.

## <a name="managing-tag-helper-scope"></a>Administración del ámbito de las aplicaciones auxiliares de etiquetas

El ámbito de las aplicaciones auxiliares de etiquetas se controla mediante una combinación de `@addTagHelper`, `@removeTagHelper` y el carácter de exclusión "!".

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` hace que las aplicaciones auxiliares de etiquetas estén disponibles

Si crea una aplicación web de ASP.NET Core denominada *AuthoringTagHelpers*, el siguiente archivo *Views/_ViewImports.cshtml* se agregará al proyecto:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

La directiva `@addTagHelper` hace que las aplicaciones auxiliares de etiquetas estén disponibles en la vista. En este caso, el archivo de vista es *Pages/_ViewImports.cshtml*, que heredan de forma predeterminada todos los archivos contenidos en la carpeta *Pages* y sus subcarpetas, lo que hace que las aplicaciones auxiliares de etiquetas estén disponibles. El código anterior usa la sintaxis de comodines ("\*") para especificar que todas las aplicaciones auxiliares de etiquetas del ensamblado especificado (*Microsoft.AspNetCore.Mvc.TagHelpers*) estarán disponibles para todos los archivos de vista del directorio o subdirectorio *Views*. El primer parámetro después de `@addTagHelper` especifica las aplicaciones auxiliares de etiquetas que se van a cargar (usamos "\*" para todas las aplicaciones auxiliares de etiquetas), y el segundo parámetro ("Microsoft.AspNetCore.Mvc.TagHelpers") especifica el ensamblado que contiene las aplicaciones auxiliares de etiquetas. *Microsoft.AspNetCore.Mvc.TagHelpers* es el ensamblado para las aplicaciones auxiliares de etiquetas integradas de ASP.NET Core.

Para exponer todas las aplicaciones auxiliares de etiquetas de este proyecto (que crea un ensamblado denominado *AuthoringTagHelpers*), use lo siguiente:

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Si el proyecto contiene una aplicación auxiliar `EmailTagHelper` con el espacio de nombres predeterminado (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), puede proporcionar el nombre completo (FQN) de la aplicación auxiliar de etiquetas:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Para agregar una aplicación auxiliar de etiquetas a una vista con un FQN, agregue primero el FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, después, el nombre del ensamblado (*AuthoringTagHelpers*). La mayoría de los desarrolladores prefiere usar la sintaxis de comodines "\*". La sintaxis de comodines permite insertar el carácter comodín "\*" como sufijo en un FQN. Por ejemplo, cualquiera de las siguientes directivas incorporará `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Como se ha mencionado anteriormente, al agregar la directiva `@addTagHelper` al archivo *Views/_ViewImports.cshtml* la aplicación auxiliar de etiquetas se pone a disposición de todos los archivos de vista del directorio *Views* y los subdirectorios. Puede usar la directiva `@addTagHelper` en archivos de vista específicos si quiere exponer la aplicación auxiliar de etiquetas solo a esas vistas.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` quita las aplicaciones auxiliares de etiquetas

`@removeTagHelper` tiene los mismos parámetros que `@addTagHelper`, y quita una aplicación auxiliar de etiquetas que se ha agregado anteriormente. Por ejemplo, si se aplica `@removeTagHelper` a una vista específica, se quita de la vista la aplicación auxiliar de etiquetas especificada. Si se usa `@removeTagHelper` en un archivo *Views/Folder/_ViewImports.cshtml*, se quita la aplicación auxiliar de etiquetas especificada de todas las vistas de *Folder*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Controlar el ámbito de la aplicación auxiliar de etiquetas con el archivo *_ViewImports.cshtml*

Si agrega un archivo *_ViewImports.cshtml* a cualquier carpeta de vistas, el motor de vistas aplica las directivas de ese archivo y del archivo *Views/_ViewImports.cshtml*. Si agregara un archivo *Views/Home/_ViewImports.cshtml* vacío para las vistas de *Home*, no se produciría ningún cambio porque el archivo *_ViewImports.cshtml* se suma. Todas las directivas `@addTagHelper` que agregue al archivo *Views/Home/_ViewImports.cshtml* (que no estén en el archivo predeterminado *Views/_ViewImports.cshtml*) expondrán esas aplicaciones auxiliares de etiquetas únicamente a las vistas de la carpeta *Home*.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Excluir elementos individuales

Puede deshabilitar una aplicación auxiliar de etiquetas en el nivel de elemento con el carácter de exclusión ("!") de la aplicación auxiliar de etiquetas. Por ejemplo, la validación de `Email` se deshabilita en `<span>` con el carácter de exclusión de la aplicación auxiliar de etiquetas:

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Debe aplicar el carácter de exclusión de la aplicación auxiliar de etiquetas en la etiqueta de apertura y de cierre. (El editor de Visual Studio agrega automáticamente el carácter de exclusión a la etiqueta de cierre cuando se agrega uno a la etiqueta de apertura). Después de agregar el carácter de exclusión, el elemento y los atributos de la aplicación auxiliar de etiquetas ya no se muestran en una fuente distinta.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Usar `@tagHelperPrefix` para hacer explícito el uso de la aplicación auxiliar de etiquetas

La directiva `@tagHelperPrefix` permite especificar una cadena de prefijo de etiqueta para habilitar la compatibilidad con la aplicación auxiliar de etiquetas y hacer explícito su uso. Por ejemplo, podría agregar el marcado siguiente al archivo *Views/_ViewImports.cshtml*:

```cshtml
@tagHelperPrefix th:
```
En la imagen de código siguiente, el prefijo de la aplicación auxiliar de etiquetas se establece en `th:`, por lo que solo los elementos con el prefijo `th:` admiten aplicaciones auxiliares de etiquetas (los elementos habilitados para aplicaciones auxiliares de etiquetas tienen una fuente distinta). Los elementos `<label>` y `<input>` tienen el prefijo de las aplicaciones auxiliares de etiquetas y están habilitados para estas, a diferencia del elemento `<span>`.

![imagen](intro/_static/thp.png)

Las mismas reglas de jerarquía que se aplican a `@addTagHelper` también se aplican a `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Compatibilidad de IntelliSense con aplicaciones auxiliares de etiquetas

Cuando se crea una aplicación web ASP.NET Core en Visual Studio, se agrega el paquete NuGet "Microsoft.AspNetCore.Razor.Tools". Este es el paquete que agrega las herramientas de las aplicaciones auxiliares de etiquetas.

Considere la posibilidad de escribir un elemento HTML `<label>`. En cuanto escriba `<l` en el editor de Visual Studio, IntelliSense mostrará elementos coincidentes:

![imagen](intro/_static/label.png)

No solo obtendrá ayuda HTML, sino el icono (el "@" symbol with "<>" situado debajo).

![imagen](intro/_static/tagSym.png)

Esto identifica el elemento como el destino de las aplicaciones auxiliares de etiquetas. Los elementos HTML puros (como `fieldset`) muestran el icono "<>".

Una etiqueta HTML pura `<label>` muestra la etiqueta HTML (con el tema de color de Visual Studio predeterminado) en una fuente marrón, con los atributos en rojo y los valores de atributo en azul.

![imagen](intro/_static/LableHtmlTag.png)

Después de escribir `<label`, IntelliSense muestra los atributos HTML/CSS disponibles y los atributos destinados a la aplicación auxiliar de etiquetas:

![imagen](intro/_static/labelattr.png)

La finalización de instrucciones de IntelliSense permite presionar la tecla TAB para completar la instrucción con el valor seleccionado:

![imagen](intro/_static/stmtcomplete.png)

En cuanto se especifica un atributo de la aplicación auxiliar de etiquetas, las fuentes de las etiquetas y los atributos cambian. Si usa el tema de color predeterminado "Azul" o "Claro" de Visual Studio, la fuente es púrpura en negrita. Si usa el tema "Oscuro", la fuente es verde azulado en negrita. Las imágenes de este documento se han realizado con el tema predeterminado.

![imagen](intro/_static/labelaspfor2.png)

Puede usar el acceso directo *CompleteWord* de Visual Studio (el valor [predeterminado](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) es Ctrl+barra espaciadora) entre comillas dobles (""). Ahora se encuentra en C#, como si estuviera en una clase de C#. IntelliSense muestra todos los métodos y propiedades en el modelo de páginas. Los métodos y las propiedades están disponibles porque el tipo de propiedad es `ModelExpression`. En la imagen siguiente se edita la vista `Register`, por lo que `RegisterViewModel` está disponible.

![imagen](intro/_static/intellemail.png)

IntelliSense muestra las propiedades y los métodos disponibles para el modelo en la página. El entorno enriquecido de IntelliSense le ayuda a seleccionar la clase CSS:

![imagen](intro/_static/iclass.png)

![imagen](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Comparación entre las aplicaciones auxiliares de etiquetas y las aplicaciones auxiliares HTML

Las aplicaciones auxiliares de etiquetas se asocian a elementos HTML en las vistas de Razor, mientras que las [aplicaciones auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) se invocan como métodos intercalados con HTML en las vistas de Razor. Observe el siguiente marcado de Razor, que crea una etiqueta HTML con la clase CSS "caption":

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

El símbolo de arroba (`@`) le indica a Razor que este es el inicio del código. Los dos parámetros siguientes ("FirstName" y "First Name:") son cadenas, por lo que [IntelliSense](/visualstudio/ide/using-intellisense) no sirve de ayuda. El último argumento:

```cshtml
new {@class="caption"}
```

Es un objeto anónimo que se usa para representar atributos. Dado que <strong>class</strong> es una palabra reservada en C#, use el símbolo `@` para forzar a C# a interpretar "@class=" como un símbolo (nombre de propiedad). Para un diseñador de front-end (es decir, alguien familiarizado con HTML, CSS, JavaScript y otras tecnologías de cliente, pero desconocedor de C# y Razor), la mayor parte de la línea le resulta ajena. Es necesario crear toda la línea sin ayuda de IntelliSense.

Si usa `LabelTagHelper`, se puede escribir el mismo marcado de la manera siguiente:

![imagen](intro/_static/label2.png)

Con la versión de la aplicación auxiliar de etiquetas, en cuanto escriba `<l` en el editor de Visual Studio, IntelliSense mostrará elementos coincidentes:

![imagen](intro/_static/label.png)

IntelliSense le ayuda a escribir toda la línea. `LabelTagHelper` también tiene como valor predeterminado el establecimiento del contenido del valor de atributo `asp-for` ("FirstName") en "First Name"; es decir, convierte las propiedades con grafía Camel en una oración formada por el nombre de la propiedad con un espacio donde aparece cada nueva letra mayúscula. En el marcado siguiente:

![imagen](intro/_static/label2.png)

Se genera:

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

El contenido con grafía Camel convertido en grafía de oración no se usa si agrega contenido a `<label>`. Por ejemplo:

![imagen](intro/_static/1stName.png)

Se genera:

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

En la imagen de código siguiente se muestra la parte Form de la vista de Razor *Views/Account/Register.cshtml* generada a partir de la plantilla heredada de MVC de ASP.NET 4.5.x incluida con Visual Studio 2015.

![imagen](intro/_static/regCS.png)

En el editor de Visual Studio se muestra el código de C# con un fondo gris. Por ejemplo, la aplicación auxiliar HTML `AntiForgeryToken`:

```cshtml
@Html.AntiForgeryToken()
```

Se muestra con un fondo gris. La mayor parte del marcado en la vista de registro es de C#. Compárelo con el método equivalente con aplicaciones auxiliares de etiquetas:

![imagen](intro/_static/regTH.png)

El marcado es mucho más ordenado y más fácil de leer, modificar y mantener que en el método de aplicaciones auxiliares HTML. El código de C# se reduce a la cantidad mínima que el servidor necesita conocer. En el editor de Visual Studio se muestra el marcado de destino de una aplicación auxiliar de etiquetas en una fuente distinta.

Observe el grupo *Email*:

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

Cada uno de los atributos "asp-" tiene un valor de "Email", pero "Email" no es una cadena. En este contexto, "Email" es la propiedad de expresión del modelo de C# para `RegisterViewModel`.

El editor de Visual Studio le ayuda a escribir **todo** el marcado en el método de aplicación auxiliar de etiquetas del formulario de registro, mientras que Visual Studio no proporciona ninguna ayuda para la mayor parte del código en el método de aplicaciones auxiliares HTML. En [Compatibilidad de IntelliSense con aplicaciones auxiliares de etiquetas](#intellisense-support-for-tag-helpers) se explica en detalle cómo trabajar con aplicaciones auxiliares de etiquetas en el editor de Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Comparación entre las aplicaciones auxiliares de etiquetas y los controles de servidor web

* Las aplicaciones auxiliares de etiquetas no poseen el elemento al que están asociadas; simplemente participan en la representación del elemento y el contenido. Los [controles de servidor web](https://msdn.microsoft.com/library/7698y1f0.aspx) de ASP.NET se declaran y se invocan en una página.

* Los [controles de servidor web](https://msdn.microsoft.com/library/zsyt68f1.aspx) tienen un ciclo de vida no trivial que puede dificultar el desarrollo y la depuración.

* Los controles de servidor web permiten agregar funcionalidad a los elementos Document Object Model (DOM) de cliente mediante el uso de un control cliente. Las aplicaciones auxiliares de etiquetas no tienen ningún DOM.

* Los controles de servidor web incluyen la detección automática del explorador. Las aplicaciones auxiliares de etiquetas no tienen conocimiento del explorador.

* Varias aplicaciones auxiliares de etiquetas pueden actuar en el mismo elemento (vea [Evitar conflictos de aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts)), mientras que normalmente no se pueden crear controles de servidor web.

* Las aplicaciones auxiliares de etiquetas pueden modificar la etiqueta y el contenido de los elementos HTML que tienen como ámbito, pero no modifican directamente ningún otro elemento de una página. Los controles de servidor web tienen un ámbito menos específico y pueden realizar acciones que afectan a otras partes de la página, lo que puede tener efectos secundarios imprevistos.

* Los controles de servidor web usan convertidores de tipos para convertir cadenas en objetos. Con las aplicaciones auxiliares de etiquetas, trabajará de forma nativa en C#, por lo que no necesitará ninguna conversión de tipos.

* Los controles de servidor web usan [System.ComponentModel](/dotnet/api/system.componentmodel) para implementar el comportamiento de componentes y controles en tiempo de diseño y en tiempo de ejecución. `System.ComponentModel` incluye las clases base y las interfaces para implementar atributos y convertidores de tipos, enlazarlos con orígenes de datos y generar licencias para los componentes. Compare esto con las aplicaciones auxiliares de etiquetas, que normalmente se derivan de `TagHelper`, y la clase base `TagHelper` solo expone dos métodos, `Process` y `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Personalizar la fuente de elemento de aplicaciones auxiliares de etiquetas

Puede personalizar la fuente y el color en **Herramientas** > **Opciones** > **Entorno** > **Fuentes y colores**:

![imagen](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Creación de aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/authoring)
* [Trabajar con formularios](xref:mvc/views/working-with-forms)
* [TagHelperSamples en GitHub](https://github.com/dpaquette/TagHelperSamples) contiene ejemplos de aplicaciones auxiliares de etiquetas para trabajar con [Bootstrap](http://getbootstrap.com/).
