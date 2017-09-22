---
title: Menor, Sass y la fuente Maravilla en ASP.NET Core
author: ardalis
description: Aprenda a usar menor, Sass y fuente Maravilla en aplicaciones de ASP.NET Core.
keywords: "Núcleo de ASP.NET, menos, Sass, fuente Maravilla, preprocesadores"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 159377300d33e98393fd6705d0fec578f8f6b735
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="40eb1-104">Introducción a las aplicaciones de estilo con menos, Sass y fuente Maravilla en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40eb1-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="40eb1-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="40eb1-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="40eb1-106">Los usuarios de aplicaciones web tienen cada vez más altas expectativas en cuanto a estilo y experiencia general.</span><span class="sxs-lookup"><span data-stu-id="40eb1-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="40eb1-107">Aplicaciones web modernas aprovechan con frecuencia completas herramientas y marcos de trabajo para definir y administrar su apariencia y funcionamiento de una manera coherente.</span><span class="sxs-lookup"><span data-stu-id="40eb1-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="40eb1-108">Marcos de trabajo como [arranque](http://getbootstrap.com/) puede ir mucho a definir un conjunto común de estilos y opciones de diseño para los sitios web.</span><span class="sxs-lookup"><span data-stu-id="40eb1-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="40eb1-109">Sin embargo, no trivial mayoría de los sitios también se beneficiará de poder definir y mantener estilos y archivos (CSS) de la hoja de estilos en cascada de forma eficaz, así como tener un acceso sencillo a los iconos de imagen no que ayudan a hacer más intuitiva interfaz del sitio.</span><span class="sxs-lookup"><span data-stu-id="40eb1-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="40eb1-110">Ahí es donde lenguajes y herramientas que admiten [menos](http://lesscss.org/) y [Sass](http://sass-lang.com/), y las bibliotecas como [fuente Maravilla](http://fontawesome.io/), vienen en.</span><span class="sxs-lookup"><span data-stu-id="40eb1-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="40eb1-111">Idiomas de preprocesador de CSS</span><span class="sxs-lookup"><span data-stu-id="40eb1-111">CSS preprocessor languages</span></span>

<span data-ttu-id="40eb1-112">Idiomas que se compilan en otros idiomas, con el fin de mejorar la experiencia de trabajar con el lenguaje subyacente, se conocen como preprocesadores.</span><span class="sxs-lookup"><span data-stu-id="40eb1-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="40eb1-113">Hay dos preprocesadores populares de CSS: menos y Sass.</span><span class="sxs-lookup"><span data-stu-id="40eb1-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="40eb1-114">Estos preprocesadores agregar características a CSS, como la posibilidad de variables y reglas anidadas, lo que mejora la facilidad de mantenimiento de las hojas de estilos de grandes y complejas.</span><span class="sxs-lookup"><span data-stu-id="40eb1-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="40eb1-115">CSS como un lenguaje es muy básica, que carecen de compatibilidad con incluso algo tan sencillo como variables y tiende a crear archivos CSS repetitivas e inflan.</span><span class="sxs-lookup"><span data-stu-id="40eb1-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="40eb1-116">Agregar características de lenguaje de programación real a través de preprocesadores puede ayudar a reducir la duplicación y proporcionar una mejor organización de reglas de estilo.</span><span class="sxs-lookup"><span data-stu-id="40eb1-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="40eb1-117">Visual Studio proporciona compatibilidad integrada para ambos menor y Sass, así como las extensiones que pueden mejorar la experiencia de desarrollo cuando se trabaja con estos idiomas.</span><span class="sxs-lookup"><span data-stu-id="40eb1-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="40eb1-118">Como un ejemplo rápido de cómo preprocesadores pueden mejorar la legibilidad y el mantenimiento de información de estilo, tenga en cuenta este CSS:</span><span class="sxs-lookup"><span data-stu-id="40eb1-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="40eb1-119">Con menor, esto se puede reescribir para eliminar la duplicación, todos con un *mixin* (denominada así porque permite "¡mix" propiedades de una clase o conjunto de reglas en el otro):</span><span class="sxs-lookup"><span data-stu-id="40eb1-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="40eb1-120">menos</span><span class="sxs-lookup"><span data-stu-id="40eb1-120">Less</span></span>

<span data-ttu-id="40eb1-121">El preprocesador de CSS inferior se ejecuta con Node.js.</span><span class="sxs-lookup"><span data-stu-id="40eb1-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="40eb1-122">Para instalar menor, use el Administrador de paquetes de nodo (npm) desde un símbolo del sistema (-g significa "global"):</span><span class="sxs-lookup"><span data-stu-id="40eb1-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="40eb1-123">Si está utilizando Visual Studio, puede empezar a trabajar con menor agregando uno o más archivos Less al proyecto y, a continuación, configurando Gulp (o Grunt) para procesarlos en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="40eb1-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="40eb1-124">Agregar un *estilos* carpeta al proyecto y, a continuación, agregue un nuevo menos con el nombre de archivo *main.less* en esta carpeta.</span><span class="sxs-lookup"><span data-stu-id="40eb1-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Agregar archivo less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="40eb1-126">Una vez agregados, la estructura de carpetas debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="40eb1-126">Once added, your folder structure should look something like this:</span></span>

![Estructura de carpetas](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="40eb1-128">Ahora puede agregar algunos estilos básicos para el archivo, que se compila en CSS e implementa en la carpeta wwwroot Gulp.</span><span class="sxs-lookup"><span data-stu-id="40eb1-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="40eb1-129">Modificar *main.less* para incluir el contenido siguiente, que crea una paleta de colores simple desde un único color base.</span><span class="sxs-lookup"><span data-stu-id="40eb1-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="40eb1-130">`@base`y el otro @-prefixed elementos son variables.</span><span class="sxs-lookup"><span data-stu-id="40eb1-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="40eb1-131">Cada uno de ellos representa un color.</span><span class="sxs-lookup"><span data-stu-id="40eb1-131">Each of them represents a color.</span></span> <span data-ttu-id="40eb1-132">Excepto `@base`, se establecen mediante las funciones de color: aclarar, oscurecer y de número.</span><span class="sxs-lookup"><span data-stu-id="40eb1-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="40eb1-133">Aclarar y oscurecer es prácticamente lo que cabría esperar; número ajusta el matiz de un color en un número de grados (alrededor de la rueda de color).</span><span class="sxs-lookup"><span data-stu-id="40eb1-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="40eb1-134">El procesador de menos es lo suficientemente inteligente como para pasar por alto las variables que no se utilizan, por lo que para demostrar cómo funcionan estas variables, es necesario utilizarlas en algún lugar.</span><span class="sxs-lookup"><span data-stu-id="40eb1-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="40eb1-135">Las clases `.baseColor`, etc. se muestran los valores calculados de cada una de las variables en el archivo CSS que se genera.</span><span class="sxs-lookup"><span data-stu-id="40eb1-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="40eb1-136">Introducción</span><span class="sxs-lookup"><span data-stu-id="40eb1-136">Getting started</span></span>

<span data-ttu-id="40eb1-137">Crear un **archivo de configuración de npm** (*package.json*) en la carpeta del proyecto y editarlo para hacer referencia a `gulp` y `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="40eb1-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="40eb1-138">Instalar las dependencias en un símbolo del sistema en la carpeta del proyecto, o en Visual Studio **el Explorador de soluciones** (**dependencias > npm > Restaurar paquetes**).</span><span class="sxs-lookup"><span data-stu-id="40eb1-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![FRENTE a restaurar los paquetes](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="40eb1-140">En la carpeta del proyecto, cree una **Gulp archivo de configuración** (*gulpfile.js*) para definir el proceso automatizado.</span><span class="sxs-lookup"><span data-stu-id="40eb1-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="40eb1-141">Agregue una variable en la parte superior del archivo para representar menos y una tarea se ejecute menor:</span><span class="sxs-lookup"><span data-stu-id="40eb1-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="40eb1-142">Abra la **explorador del ejecutor de tareas** (**Vista > otras ventanas > Explorador del ejecutor de tareas**).</span><span class="sxs-lookup"><span data-stu-id="40eb1-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="40eb1-143">Entre las tareas, verá una nueva tarea denominada `less`.</span><span class="sxs-lookup"><span data-stu-id="40eb1-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="40eb1-144">Es posible que deba actualizar la ventana.</span><span class="sxs-lookup"><span data-stu-id="40eb1-144">You might have to refresh the window.</span></span>

<span data-ttu-id="40eb1-145">Ejecute la `less` tarea y obtener un resultado parecido al que se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="40eb1-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![menos ejecutor de tareas](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="40eb1-147">El *wwwroot/css* carpeta contiene ahora un nuevo archivo, *main.css*:</span><span class="sxs-lookup"><span data-stu-id="40eb1-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principal creado](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="40eb1-149">Abra *main.css* y verá algo parecido a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="40eb1-149">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="40eb1-150">Agregar una página HTML sencilla para la *wwwroot* carpeta y referencia *main.css* para ver la paleta de colores en acción.</span><span class="sxs-lookup"><span data-stu-id="40eb1-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="40eb1-151">Puede ver que girar 180 grados en `@base` se usa para generar `@background` dio como resultado la rueda de color opuestas color de `@base`:</span><span class="sxs-lookup"><span data-stu-id="40eb1-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![en el ejemplo se prueba menos](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="40eb1-153">Menor también proporciona compatibilidad para reglas anidadas, así como las consultas de media anidadas.</span><span class="sxs-lookup"><span data-stu-id="40eb1-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="40eb1-154">Por ejemplo, definir las jerarquías anidadas como menús pueden dar lugar a las reglas de CSS detalladas como estos:</span><span class="sxs-lookup"><span data-stu-id="40eb1-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="40eb1-155">Lo ideal es que todas las reglas de estilo relacionados se colocarán juntos en el archivo CSS, pero en la práctica que no hay nada aplicar esta regla excepto convención y quizás los comentarios del bloque.</span><span class="sxs-lookup"><span data-stu-id="40eb1-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="40eb1-156">Definir estas mismas reglas que reducen el uso es similar a esto:</span><span class="sxs-lookup"><span data-stu-id="40eb1-156">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="40eb1-157">Tenga en cuenta que en este caso, todos los elementos subordinados de `nav` están dentro de su ámbito.</span><span class="sxs-lookup"><span data-stu-id="40eb1-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="40eb1-158">Ya no hay ninguna repetición de elementos primarios (`nav`, `li`, `a`), y el recuento de líneas total ha disminuido también (aunque algunos de los que es el resultado de que los valores en las mismas líneas en el segundo ejemplo).</span><span class="sxs-lookup"><span data-stu-id="40eb1-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="40eb1-159">Puede ser muy útil, organización ver todas las reglas de un elemento de interfaz de usuario concreto dentro de un ámbito explícitamente limitado, en este caso desactivar el resto del archivo de llaves.</span><span class="sxs-lookup"><span data-stu-id="40eb1-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="40eb1-160">El `&` sintaxis es una característica menos selector, con & que representa el elemento primario de selector actual.</span><span class="sxs-lookup"><span data-stu-id="40eb1-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="40eb1-161">Por lo tanto, dentro de la una {...}</span><span class="sxs-lookup"><span data-stu-id="40eb1-161">So, within the a {...}</span></span> <span data-ttu-id="40eb1-162">bloque, `&` representa un `a` (etiqueta) y, por tanto, `&:link` es equivalente a `a:link`.</span><span class="sxs-lookup"><span data-stu-id="40eb1-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="40eb1-163">Consultas de medios, muy útiles para crear diseños de capacidad de respuesta, también pueden contribuir con un alto grado a repeticiones y la complejidad de CSS.</span><span class="sxs-lookup"><span data-stu-id="40eb1-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="40eb1-164">Menor permite consultas de medios se anidan dentro de las clases, por lo que la definición de clase completo no tiene que repetirse dentro de otro nivel superior `@media` elementos.</span><span class="sxs-lookup"><span data-stu-id="40eb1-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="40eb1-165">Por ejemplo, mostramos CSS para un menú dinámico:</span><span class="sxs-lookup"><span data-stu-id="40eb1-165">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="40eb1-166">Esto se puede definir mejor en menos como:</span><span class="sxs-lookup"><span data-stu-id="40eb1-166">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="40eb1-167">Otra característica de menor que ya hemos visto es su compatibilidad para operaciones matemáticas, lo que permite a los atributos de estilo se construyan a partir de variables definidas previamente.</span><span class="sxs-lookup"><span data-stu-id="40eb1-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="40eb1-168">Esto hará que la actualización relacionados estilos mucho más fácil, ya que se puede modificar la variable de base y todos los valores dependientes cambian automáticamente.</span><span class="sxs-lookup"><span data-stu-id="40eb1-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="40eb1-169">Archivos CSS, especialmente para sitios de gran tamaño (y especialmente si se usan las consultas de medios), tienden a alcanzar un tamaño considerable con el tiempo, lo difícil trabajar con ellas.</span><span class="sxs-lookup"><span data-stu-id="40eb1-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="40eb1-170">Menos archivos pueden definirse por separado, a continuación, extrae juntos mediante `@import` directivas.</span><span class="sxs-lookup"><span data-stu-id="40eb1-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="40eb1-171">También se puede usar menor para importar CSS archivos individuales, también, si lo desea.</span><span class="sxs-lookup"><span data-stu-id="40eb1-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="40eb1-172">*Mixins* puede aceptar parámetros y menor admite una lógica condicional en forma de protecciones de mixin, que proporcionan una manera declarativa para definir cuándo determinadas mixins surten efecto.</span><span class="sxs-lookup"><span data-stu-id="40eb1-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="40eb1-173">Un uso común de protecciones de mixin es ajustar los colores en función de la luz u oscuro el color de origen.</span><span class="sxs-lookup"><span data-stu-id="40eb1-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="40eb1-174">Dado un mixin que acepta un parámetro para el color, un guardia mixin puede utilizarse para modificar el mixin en función de ese color:</span><span class="sxs-lookup"><span data-stu-id="40eb1-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="40eb1-175">Dada nuestra actual `@base` valo `#663333`, esta secuencia de comandos menos generará el código CSS siguiente:</span><span class="sxs-lookup"><span data-stu-id="40eb1-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="40eb1-176">Menor proporciona una serie de características adicionales, pero esto debe tener una idea de la potencia de este lenguaje de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="40eb1-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="40eb1-177">SASS</span><span class="sxs-lookup"><span data-stu-id="40eb1-177">Sass</span></span>

<span data-ttu-id="40eb1-178">SASS es similar a la menor, lo que proporciona compatibilidad para muchas de las mismas características, pero con una sintaxis ligeramente diferente.</span><span class="sxs-lookup"><span data-stu-id="40eb1-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="40eb1-179">Se compila con Ruby, en lugar de JavaScript y, por lo que tiene requisitos de instalación diferentes.</span><span class="sxs-lookup"><span data-stu-id="40eb1-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="40eb1-180">El idioma de Sass original no usa llaves o puntos y comas, sino que define ámbito mediante espacios en blanco y sangría.</span><span class="sxs-lookup"><span data-stu-id="40eb1-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="40eb1-181">En la versión 3 de Sass, se introdujo una nueva sintaxis, **SCSS** ("CSS Sassy").</span><span class="sxs-lookup"><span data-stu-id="40eb1-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="40eb1-182">SCSS es similar a CSS en que se pasa por alto los espacios en blanco y niveles de sangría y en su lugar utiliza el punto y coma y llaves.</span><span class="sxs-lookup"><span data-stu-id="40eb1-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="40eb1-183">Para instalar Sass, normalmente se instala por primera vez Ruby (preinstalado en Mac) y, a continuación, ejecute:</span><span class="sxs-lookup"><span data-stu-id="40eb1-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="40eb1-184">Sin embargo, si está ejecutando Visual Studio, puede empezar a trabajar con Sass prácticamente la misma manera como lo haría con menor.</span><span class="sxs-lookup"><span data-stu-id="40eb1-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="40eb1-185">Abra *package.json* y agregar el paquete "gulp sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="40eb1-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="40eb1-186">A continuación, modifique *gulpfile.js* para agregar una variable de sass y una tarea para compilar los archivos Sass y colocar los resultados en la carpeta wwwroot:</span><span class="sxs-lookup"><span data-stu-id="40eb1-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="40eb1-187">Ahora puede agregar el archivo Sass *main2.scss* a la *estilos* carpeta en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="40eb1-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Agregar archivo scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="40eb1-189">Abra *main2.scss* y agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="40eb1-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="40eb1-190">Guarde todos los archivos.</span><span class="sxs-lookup"><span data-stu-id="40eb1-190">Save all of your files.</span></span> <span data-ttu-id="40eb1-191">Ahora, al actualizar **explorador del ejecutor de tareas**, verá un `sass` tarea.</span><span class="sxs-lookup"><span data-stu-id="40eb1-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="40eb1-192">Ejecútelo y, en la */wwwroot/css* carpeta.</span><span class="sxs-lookup"><span data-stu-id="40eb1-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="40eb1-193">Ahora hay un *main2.css* archivo con este contenido:</span><span class="sxs-lookup"><span data-stu-id="40eb1-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="40eb1-194">SASS admite el anidamiento prácticamente lo mismo era que menor no, proporcionar beneficios similares.</span><span class="sxs-lookup"><span data-stu-id="40eb1-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="40eb1-195">Los archivos se puede dividir función e incluido mediante la `@import` directiva:</span><span class="sxs-lookup"><span data-stu-id="40eb1-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="40eb1-196">SASS admite mixins así, el uso de la `@mixin` palabra clave que se va a definirlos y `@include` para incluirlas, como en este ejemplo de [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="40eb1-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="40eb1-197">Además de mixins, Sass también admite el concepto de herencia, lo que permite una clase Extender el otro.</span><span class="sxs-lookup"><span data-stu-id="40eb1-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="40eb1-198">Es conceptualmente similar a un mixin, pero da como resultado menos código CSS.</span><span class="sxs-lookup"><span data-stu-id="40eb1-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="40eb1-199">Se realiza mediante el `@extend` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="40eb1-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="40eb1-200">Para probar el mixins, agregue lo siguiente a su *main2.scss* archivo:</span><span class="sxs-lookup"><span data-stu-id="40eb1-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="40eb1-201">Examine la salida en *main2.css* después de ejecutar el `sass` de tareas en **explorador del ejecutor de tareas**:</span><span class="sxs-lookup"><span data-stu-id="40eb1-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="40eb1-202">Tenga en cuenta que todas las propiedades comunes de la alerta mixin se repiten en cada clase.</span><span class="sxs-lookup"><span data-stu-id="40eb1-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="40eb1-203">El mixin realizó un buen trabajo para ayudar a eliminar la duplicación en tiempo de desarrollo, pero todavía está creando CSS con una gran cantidad de duplicación en él, lo que produce mayores que los archivos necesarios de CSS - un posible problema de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="40eb1-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="40eb1-204">Ahora reemplace la alerta mixin con un `.alert` clase y cambiar `@include` a `@extend` (teniendo en cuenta ampliar `.alert`, no `alert`):</span><span class="sxs-lookup"><span data-stu-id="40eb1-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="40eb1-205">Ejecute Sass una vez más y examine el código CSS resultante:</span><span class="sxs-lookup"><span data-stu-id="40eb1-205">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="40eb1-206">Ahora las propiedades se definen únicamente como tantas veces como sea necesario y mejor se genera CSS.</span><span class="sxs-lookup"><span data-stu-id="40eb1-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="40eb1-207">SASS también incluye funciones y operaciones de lógica condicional, similares a un valor inferior.</span><span class="sxs-lookup"><span data-stu-id="40eb1-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="40eb1-208">De hecho, las capacidades de los dos lenguajes son muy similares.</span><span class="sxs-lookup"><span data-stu-id="40eb1-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="40eb1-209">¿Menor o Sass?</span><span class="sxs-lookup"><span data-stu-id="40eb1-209">Less or Sass?</span></span>

<span data-ttu-id="40eb1-210">Todavía no hay ningún consenso en cuanto a si en general es mejor usar menor o Sass (o incluso si se prefiere la Sass original o la sintaxis SCSS más reciente en Sass).</span><span class="sxs-lookup"><span data-stu-id="40eb1-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="40eb1-211">Probablemente la decisión más importante es **utilizar una de estas herramientas**, en lugar de simplemente codificados manualmente los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="40eb1-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="40eb1-212">Una vez que haya realizado que ambos menor, la decisión y Sass son una buena elección.</span><span class="sxs-lookup"><span data-stu-id="40eb1-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="40eb1-213">Fuente Maravilla</span><span class="sxs-lookup"><span data-stu-id="40eb1-213">Font Awesome</span></span>

<span data-ttu-id="40eb1-214">Además de preprocesadores CSS, otro recurso excelente para aplicaciones web modernas de estilo es fantástica de fuente.</span><span class="sxs-lookup"><span data-stu-id="40eb1-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="40eb1-215">Awesome de fuente es un Kit de herramientas que proporciona más de 500 iconos vectoriales escalables que pueden usarse libremente en sus aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="40eb1-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="40eb1-216">Se diseñó originalmente para trabajar con arranque, pero no tiene ninguna dependencia en ese marco de trabajo o en cualquier biblioteca de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="40eb1-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="40eb1-217">La manera más fácil empezar a trabajar con fuentes Maravilla consiste en Agregar una referencia a ella, mediante su ubicación de red (CDN) de entrega de contenido público:</span><span class="sxs-lookup"><span data-stu-id="40eb1-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="40eb1-218">También puede agregarlo a su proyecto de Visual Studio, éste se agrega a las "dependencias" en *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="40eb1-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="40eb1-219">Una vez que tenga una referencia a la fuente Maravilla en una página, puede agregar iconos a la aplicación aplicando fuente Maravilla clases, normalmente con el prefijo "fa-", a los elementos HTML alineado (como `<span>` o `<i>`).</span><span class="sxs-lookup"><span data-stu-id="40eb1-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="40eb1-220">Por ejemplo, puede agregar iconos a listas simples y los menús con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="40eb1-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="40eb1-221">Esto produce el siguiente en el explorador: tenga en cuenta el icono junto a cada elemento:</span><span class="sxs-lookup"><span data-stu-id="40eb1-221">This produces the following in the browser - note the icon beside each item:</span></span>

![iconos de la lista](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="40eb1-223">Puede ver una lista completa de los iconos disponibles aquí:</span><span class="sxs-lookup"><span data-stu-id="40eb1-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="40eb1-224">http://fontawesome.IO/Icons/</span><span class="sxs-lookup"><span data-stu-id="40eb1-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="40eb1-225">Resumen</span><span class="sxs-lookup"><span data-stu-id="40eb1-225">Summary</span></span>

<span data-ttu-id="40eb1-226">Aplicaciones web modernas exigen cada vez más capacidad de respuesta, fluidos diseños que están limpios, intuitiva y fácil de usar desde una variedad de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="40eb1-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="40eb1-227">Administrar la complejidad de las hojas de estilos CSS necesaria para lograr estos objetivos mejor se realiza mediante un tipo de preprocesador menos o Sass.</span><span class="sxs-lookup"><span data-stu-id="40eb1-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="40eb1-228">Además, kits de herramientas como fuente Maravilla rápidamente proporcionan iconos conocidos para los menús de navegación textual y experimentan de botones, mejorar la global del usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40eb1-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
