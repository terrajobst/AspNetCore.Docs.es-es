---
title: "Aplicaciones auxiliares de etiquetas en el núcleo de ASP.NET"
author: rick-anderson
description: "Obtenga información acerca de qué son las aplicaciones auxiliares de etiquetas y cómo se utilizan en ASP.NET Core."
keywords: "Núcleo de ASP.NET, aplicaciones auxiliares de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06644b8359fb5ccc2e61a17a4c6e20e354d5ceef
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>Introducción a las aplicaciones auxiliares de etiquetas en el núcleo de ASP.NET 

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>¿Qué aplicaciones auxiliares de etiquetas?

Aplicaciones auxiliares de etiquetas que el código de servidor pueda participar en la creación y representar elementos HTML en archivos de Razor. Por ejemplo, la integrada `ImageTagHelper` puede anexar un número de versión para el nombre de imagen. Cada vez que cambie la imagen, el servidor genera una nueva versión única para la imagen, por lo que se garantiza que los clientes para obtener la imagen actual (en lugar de una imagen almacenada en caché obsoleta). Hay muchas aplicaciones auxiliares de etiquetas integradas para las tareas comunes: como la creación de formularios, vínculos, activos de carga y los paquetes más - y aún más disponibles en repositorios públicos de GitHub y como NuGet. Aplicaciones auxiliares de etiquetas se crean en C# y se dirigen a los elementos HTML en función de nombre de elemento o atributo o etiqueta primaria. Por ejemplo, la integrada `LabelTagHelper` puede tener como destino el HTML `<label>` elemento cuando el `LabelTagHelper` se aplican atributos. Si está familiarizado con [aplicaciones auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), aplicaciones auxiliares de etiquetas reducir las transiciones explícitas entre HTML y C# en las vistas de Razor. En muchos casos, las aplicaciones auxiliares HTML proporcionan un enfoque alternativo para una aplicación auxiliar de etiqueta específico, pero es importante reconocer que aplicaciones auxiliares de etiquetas no reemplazan métodos auxiliares HTML y no es una aplicación auxiliar de etiquetas para cada aplicación auxiliar HTML. [Aplicaciones auxiliares en comparación con las aplicaciones auxiliares HTML de etiquetas](#tag-helpers-compared-to-html-helpers) se explican las diferencias con más detalle.

## <a name="what-tag-helpers-provide"></a>¿Qué proporciona aplicaciones auxiliares de etiquetas

**Una experiencia de desarrollo compatible con HTML** para la mayor parte, con aplicaciones auxiliares de etiquetas de marcado de Razor es similar a HTML estándar. Diseñadores de front-end familiarizados con HTML, CSS y JavaScript pueden editar Razor sin tener que conocer la sintaxis Razor de C#.

**Un entorno rico de IntelliSense para crear marcado HTML y Razor** trata de un contraste agudo para aplicaciones auxiliares de HTML, el enfoque anterior para la creación del servidor de marcado en las vistas de Razor. [Aplicaciones auxiliares en comparación con las aplicaciones auxiliares HTML de etiquetas](#tag-helpers-compared-to-html-helpers) se explican las diferencias con más detalle. [Compatibilidad con IntelliSense para las aplicaciones auxiliares de etiquetas](#intellisense-support-for-tag-helpers) explica el entorno de IntelliSense. Incluso los desarrolladores que tienen experimentados con sintaxis Razor C# son más productivos mediante aplicaciones auxiliares de etiquetas que escribir marcado de C# Razor.

**Una manera para que sea más productiva y pueden generar más sólida y confiable y código mantenible con información sólo está disponible en el servidor** por ejemplo, históricamente era el mantra sobre la actualización de imágenes cambiar el nombre de la imagen al cambiar la imagen. Las imágenes deben almacenarse en caché activamente por motivos de rendimiento y, a menos que cambie el nombre de una imagen, corre el riesgo de los clientes obtengan una copia obsoleta. Históricamente, una vez se ha editado una imagen, el nombre tenía cambiarse y todas las referencias a la imagen de la aplicación web necesita actualizarse. Esto no sólo es muy trabajo intensivo, también es propenso a errores (pudo faltaba una referencia, escriba accidentalmente la cadena incorrecta, etcetera.) Integrado `ImageTagHelper` puede hacer esto por usted automáticamente. La `ImageTagHelper` puede anexar una versión número al nombre de imagen, por lo que cada vez que cambie la imagen, el servidor genera automáticamente una nueva versión única para la imagen. Se garantiza que los clientes para obtener la imagen actual. Este ahorro solidez y trabajo viene esencialmente gratuita mediante el uso de la `ImageTagHelper`.

La mayoría de las aplicaciones auxiliares de etiquetas integradas elementos HTML existentes de destino y proporciona atributos de servidor para el elemento. Por ejemplo, el `<input>` elemento utilizado en muchas de las vistas en el *Views/Account* carpeta contiene el `asp-for` atributo, que extrae el nombre de la propiedad de modelo especificado en el HTML representado. El siguiente marcado de Razor:

```html
<label asp-for="Email"></label>
```

Genera el siguiente código HTML:

```html
<label for="Email">Email</label>
```

El `asp-for` atributo debe ponerse a disposición por la `For` propiedad en el `LabelTagHelper`. Vea [creación de aplicaciones auxiliares de etiquetas](authoring.md) para obtener más información.

## <a name="managing-tag-helper-scope"></a>Administración de ámbito de aplicación auxiliar de etiqueta

Ámbito de las aplicaciones auxiliares de etiqueta se controla mediante una combinación de `@addTagHelper`, `@removeTagHelper`y el "!" caracteres de desactivación.

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`pone a disposición aplicaciones auxiliares de etiquetas

Si creas una nueva aplicación web de ASP.NET Core denominada *AuthoringTagHelpers* (con sin autenticación), el siguiente *Views/_ViewImports.cshtml* archivo se agregará al proyecto:

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

El `@addTagHelper` directiva pone a disposición de la vista de aplicaciones auxiliares de etiquetas. En este caso, el archivo de vista es *Views/_ViewImports.cshtml*, cuyo valor predeterminado es heredada por todos los archivos de vista en el *vistas* carpeta y los subdirectorios; ofrecer aplicaciones auxiliares de etiquetas. El código anterior utiliza la sintaxis de carácter comodín ("\*") para especificar que todas las aplicaciones auxiliares de etiquetas en el ensamblado especificado (*Microsoft.AspNetCore.Mvc.TagHelpers*) estará disponible para todos los archivos de vista la *vistas* directorio o subdirectorio. El primer parámetro después de `@addTagHelper` especifica las aplicaciones auxiliares de etiquetas para cargar (usamos "\*" para todas las aplicaciones auxiliares de etiquetas), y el segundo parámetro "Microsoft.AspNetCore.Mvc.TagHelpers" especifica el ensamblado que contiene las aplicaciones auxiliares de etiquetas. *Microsoft.AspNetCore.Mvc.TagHelpers* es el ensamblado para los asistentes de etiqueta de principales de ASP.NET integrado.

Para exponer todas las aplicaciones auxiliares de etiquetas en este proyecto (que crea un ensamblado denominado *AuthoringTagHelpers*), utilizaría lo siguiente:

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Si el proyecto contiene un `EmailTagHelper` con el espacio de nombres predeterminado (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), puede proporcionar el nombre completo (FQN) de la aplicación auxiliar de etiqueta:

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Para agregar una aplicación auxiliar de etiqueta a una vista con un FQN, agregue primero la FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, a continuación, el nombre del ensamblado (*AuthoringTagHelpers*). Mayoría de los desarrolladores prefiere usar la "\*" sintaxis de caracteres comodín. La sintaxis de comodines permite insertar el carácter comodín "\*" como el sufijo en un FQN. Por ejemplo, cualquiera de las siguientes directivas pondrá en el `EmailTagHelper`:

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Como se mencionó anteriormente, agregar el `@addTagHelper` directivas para la *Views/_ViewImports.cshtml* archivo pone a disposición de todos los archivos de vista de la aplicación auxiliar de etiqueta el *vistas* directorio y subdirectorios. Puede usar el `@addTagHelper` la directiva en los archivos de la vista específica si desea participar en exponer la aplicación auxiliar de etiqueta a solo aquellas vistas.

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Quita las aplicaciones auxiliares de etiquetas

El `@removeTagHelper` tiene los mismos parámetros como `@addTagHelper`, y quita una aplicación auxiliar de etiquetas que se agregó anteriormente. Por ejemplo, `@removeTagHelper` aplicados en una vista específica se quitan la aplicación auxiliar de etiqueta especificada de la vista. Usar `@removeTagHelper` en un *Views/Folder/_ViewImports.cshtml* archivo quita la aplicación auxiliar de etiqueta especificada de todas las vistas en *carpeta*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Controlar el ámbito de aplicación auxiliar de etiqueta con el *_ViewImports.cshtml* archivo

Puede agregar un *_ViewImports.cshtml* a cualquier carpeta de la vista y la vista motor aplica las directivas de ambos ese archivo y la *Views/_ViewImports.cshtml* archivo. Si agregó vacío *Views/Home/_ViewImports.cshtml* de archivos para el *inicio* vistas, no habría ningún cambio porque la *_ViewImports.cshtml* archivo es suma. Cualquier `@addTagHelper` directivas se agrega a la *Views/Home/_ViewImports.cshtml* archivo (que no están en el valor predeterminado *Views/_ViewImports.cshtml* archivo) expondría esas aplicaciones auxiliares de etiquetas a las vistas solo en el *Inicio* carpeta.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>No participar en de los elementos individuales

Puede deshabilitar una aplicación auxiliar de etiquetas en el nivel de elemento con el carácter de desactivación de aplicación auxiliar de etiqueta ("!"). Por ejemplo, `Email` validación está deshabilitada en el `<span>` con el carácter de desactivación de aplicación auxiliar de etiqueta:

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Debe aplicar el carácter de desactivación de aplicación auxiliar de etiqueta a la apertura y la etiqueta de cierre. (El editor de Visual Studio agrega automáticamente el carácter de cancelación para la etiqueta de cierre al agregar una a la etiqueta de apertura). Después de agregar el carácter de cancelación, el elemento y atributos de la aplicación auxiliar de etiqueta ya no se muestran en una fuente distintos.

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Usar `@tagHelperPrefix` para hacer uso de la aplicación auxiliar de etiqueta explícita

El `@tagHelperPrefix` directiva permite especificar una cadena de prefijo de etiqueta para habilitar la compatibilidad de la aplicación auxiliar de etiqueta y hacer uso de la aplicación auxiliar de etiqueta explícita. Por ejemplo, podría agregar el código siguiente para el *Views/_ViewImports.cshtml* archivo:

```html
@tagHelperPrefix th:
```
En la imagen de código siguiente, se establece el prefijo de etiqueta auxiliares en `th:`, por lo que solo los elementos con el prefijo `th:` admite aplicaciones auxiliares de etiquetas (elementos de aplicación auxiliar de la etiqueta tienen una fuente distintos). El `<label>` y `<input>` elementos tienen el prefijo de etiqueta auxiliares y están habilitadas para auxiliar de etiqueta, mientras el `<span>` elemento no lo hace.

![imagen](intro/_static/thp.png)

Las mismas reglas de jerarquía que se aplican a `@addTagHelper` también se aplican a `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Compatibilidad con IntelliSense para las aplicaciones auxiliares de etiquetas

Cuando se crea una nueva aplicación web ASP.NET en Visual Studio, agrega el paquete NuGet "Microsoft.AspNetCore.Razor.Tools". Este es el paquete que agrega herramientas de aplicación auxiliar de etiqueta.

Considere la posibilidad de escribir un elemento HTML `<label>` elemento. Tan pronto como escribe `<l` en el editor de Visual Studio, IntelliSense muestra elementos coincidentes:

![imagen](intro/_static/label.png)

No solo obtendrá la Ayuda HTML, pero el icono (el "@" símbolo con "<>" debajo de él).

![imagen](intro/_static/tagSym.png)

identifica el elemento como destino de aplicaciones auxiliares de etiquetas. Elementos HTML puros (como el `fieldset`) muestra el icono "<>".

Un código de HTML puro `<label>` la etiqueta HTML (con el tema de color de Visual Studio de forma predeterminada) se muestra en una fuente marrón, los atributos en rojo, y los valores de atributo en azul.

![imagen](intro/_static/LableHtmlTag.png)

Después de escribir `<label`, IntelliSense muestra los atributos HTML/CSS disponibles y los atributos de aplicación auxiliar de la etiqueta de destino:

![imagen](intro/_static/labelattr.png)

Finalización de instrucciones de IntelliSense permite especificar la tecla tab para completar la instrucción con el valor seleccionado:

![imagen](intro/_static/stmtcomplete.png)

Tan pronto como se escribe un atributo de aplicación auxiliar de etiquetas, cambian las fuentes de etiquetas y atributos. Con el valor predeterminado "Blue" de Visual Studio o el tema de color "Light", la fuente es negrita púrpura. Si usas el tema "Oscuro" de la fuente es verde azulado negrita. Las imágenes en este documento se realizaron con el tema predeterminado.

![imagen](intro/_static/labelaspfor2.png)

Puede escribir el Visual Studio *CompleteWord* acceso directo (Ctrl + barra espaciadora es el [predeterminado](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) dentro de las comillas dobles (""), y ahora se encuentran en C#, al igual que sería en una clase de C#. IntelliSense muestra todos los métodos y propiedades en el modelo de páginas. Los métodos y propiedades están disponibles porque el tipo de propiedad es `ModelExpression`. En la imagen siguiente, estoy editando el `Register` vista, por lo que el `RegisterViewModel` está disponible.

![imagen](intro/_static/intellemail.png)

IntelliSense muestra las propiedades y métodos disponibles para el modelo en la página. El entorno enriquecido de IntelliSense le ayuda a seleccionar la clase CSS:

![imagen](intro/_static/iclass.png)

![imagen](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Aplicaciones auxiliares de etiquetas en comparación con las aplicaciones auxiliares HTML

Aplicaciones auxiliares de etiquetas asociar a elementos HTML en las vistas de Razor, mientras que [aplicaciones auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) se invocan como métodos entremezcladas con HTML en las vistas de Razor. Tenga en cuenta el siguiente marcado de Razor, que crea una etiqueta HTML con la clase CSS "título":

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

El en (`@`) símbolo solicita Razor es el inicio del código. Los dos parámetros siguientes ("FirstName" y "nombre:") son cadenas, por lo que [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) no sirve de ayuda. El último argumento:

```html
new {@class="caption"}
```

Es un objeto anónimo que se usa para representar los atributos. Dado que **clase** es una palabra reservada en C#, utilice la `@` símbolos para forzar que C# para interpretar "@class=" como un símbolo (nombre de propiedad). Para un diseñador de front-end (alguien familiarizado con HTML, CSS y JavaScript y otras tecnologías de cliente pero no está familiarizado con C# y Razor), la mayor parte de la línea es externa. Toda la línea debe crearse con ninguna ayuda de IntelliSense.

Mediante el `LabelTagHelper`, el mismo marcado puede escribirse como:

![imagen](intro/_static/label2.png)

Con la versión del auxiliar de etiquetas, tan pronto como escribe `<l` en el editor de Visual Studio, IntelliSense muestra elementos coincidentes:

![imagen](intro/_static/label.png)

IntelliSense le ayuda a escribir toda la línea. El `LabelTagHelper` también tiene como valor predeterminado para establecer el contenido de la `asp-for` ("FirstName") de valor de atributo a "First Name"; Convierte la grafía camel propiedades en una frase formada por el nombre de propiedad con un espacio donde se produce cada nueva letra mayúscula. En el siguiente marcado:

![imagen](intro/_static/label2.png)

genera:

```html
<label class="caption" for="FirstName">First Name</label>
```

No se utiliza la grafía de camel al contenido de grafía de frase si agrega contenido a la `<label>`. Por ejemplo:

![imagen](intro/_static/1stName.png)

genera:

```html
<label class="caption" for="FirstName">Name First</label>
```

La imagen de código siguiente muestra la parte del formulario de la *Views/Account/Register.cshtml* vista Razor generada a partir de la plantilla MVC de 4.5 ASP.NET heredada incluida con Visual Studio 2015.

![imagen](intro/_static/regCS.png)

El editor de Visual Studio muestra el código de C# con un fondo gris. Por ejemplo, el `AntiForgeryToken` aplicación auxiliar HTML:

```html
@Html.AntiForgeryToken()
```

se muestra con un fondo gris. La mayoría del marcado en la vista de registro es C#. Compárelo con el enfoque equivalente mediante aplicaciones auxiliares de etiquetas:

![imagen](intro/_static/regTH.png)

El marcado es mucho más ordenados y fáciles de leer, modificar y mantener que el enfoque de aplicaciones auxiliares HTML. El código de C# se reduce al mínimo que necesita saber sobre el servidor. El editor de Visual Studio muestra marcado dirigido por una aplicación auxiliar de etiquetas en una fuente distintos.

Tenga en cuenta el *correo electrónico* grupo:

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

Cada uno de los atributos "asp-" tiene un valor de "Email", pero "Email" no es una cadena. En este contexto, "Email" es la propiedad de expresión de modelo en C# para el `RegisterViewModel`.

El editor de Visual Studio le ayuda a escribir **todos los** del marcado en el método de aplicación auxiliar de la etiqueta del formulario de registro, mientras que Visual Studio no proporciona ninguna ayuda para la mayor parte del código en el enfoque de aplicaciones auxiliares HTML. [Compatibilidad con IntelliSense para las aplicaciones auxiliares de etiquetas](#intellisense-support-for-tag-helpers) entra en detalle sobre cómo trabajar con aplicaciones auxiliares de etiquetas en el editor de Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Aplicaciones auxiliares de etiquetas en comparación con controles de servidor Web

* Aplicaciones auxiliares de etiquetas no poseen el elemento que están asociados a; basta con que participan en la representación del elemento y el contenido. ASP.NET [controles de servidor Web](https://msdn.microsoft.com/library/7698y1f0.aspx) se declaran y se invoca en una página.

* [Controles de servidor Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) tienen un ciclo de vida no trivial que puede dificultar la desarrollar y depurar.

* Controles de servidor Web permiten agregar funcionalidad a los elementos de Document Object Model (DOM) de cliente mediante un control de cliente. Aplicaciones auxiliares de etiquetas no tengan ningún DOM.

* Controles de servidor Web incluyen la detección automática del explorador. Aplicaciones auxiliares de etiquetas debe tener ningún conocimiento del explorador.

* Varias aplicaciones auxiliares de etiquetas pueden actuar en el mismo elemento (vea [conflictos de aplicación auxiliar de etiqueta de evitar](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) mientras que normalmente no se puede crear controles de servidor Web.

* Aplicaciones auxiliares de etiquetas pueden modificar la etiqueta y el contenido de los elementos HTML que va al ámbito, pero directamente no modifique nada en una página. Controles de servidor Web tienen un ámbito menos específico y pueden realizar acciones que afectan a otras partes de la página; Habilitar efectos secundarios imprevistos.

* Controles de servidor Web usan convertidores de tipos para convertir cadenas en objetos. Con aplicaciones auxiliares de etiquetas, trabaja forma nativa en C#, por lo que no necesita la conversión de tipos.

* Uso de controles de servidor Web [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) para implementar el comportamiento de tiempo de ejecución y tiempo de diseño de componentes y controles. `System.ComponentModel`incluye las clases base e interfaces para implementar atributos y convertidores de tipos, componentes de licencias y orígenes de enlace a datos. Esto contrasta con las aplicaciones auxiliares de etiquetas, que normalmente se derivan de `TagHelper`y el `TagHelper` clase base expone dos métodos: `Process` y `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Personalizar la fuente del elemento de aplicación auxiliar de etiqueta

Puede personalizar la fuente y el color de **herramientas** > **opciones** > **entorno** > **fuentes y colores**:

![imagen](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Creación de aplicaciones auxiliares de etiquetas](authoring.md)
* [Trabajar con formularios](../working-with-forms.md)
* [TagHelperSamples en GitHub](https://github.com/dpaquette/TagHelperSamples) contiene ejemplos de aplicación auxiliar de etiqueta para trabajar con [arranque](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
