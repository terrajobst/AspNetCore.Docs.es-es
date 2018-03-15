---
title: Menor, Sass y la fuente Maravilla en ASP.NET Core
author: ardalis
description: Aprenda a usar menor, Sass y fuente Maravilla en aplicaciones de ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: e00a0929db9dff6c97c4b22468156f621a1a3820
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>Introducción a las aplicaciones de estilo con menos, Sass y fuente Maravilla en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Los usuarios de aplicaciones web tienen cada vez más altas expectativas en cuanto a estilo y experiencia general. Aplicaciones web modernas aprovechan con frecuencia completas herramientas y marcos de trabajo para definir y administrar su apariencia y funcionamiento de una manera coherente. Marcos de trabajo como [arranque](http://getbootstrap.com/) puede ir mucho a definir un conjunto común de estilos y opciones de diseño para los sitios web. Sin embargo, no trivial mayoría de los sitios también se beneficiará de poder definir y mantener estilos y archivos (CSS) de la hoja de estilos en cascada de forma eficaz, así como tener un acceso sencillo a los iconos de imagen no que ayudan a hacer más intuitiva interfaz del sitio. Ahí es donde lenguajes y herramientas que admiten [menos](http://lesscss.org/) y [Sass](http://sass-lang.com/), y las bibliotecas como [fuente Maravilla](http://fontawesome.io/), vienen en.

## <a name="css-preprocessor-languages"></a>Idiomas de preprocesador de CSS

Idiomas que se compilan en otros idiomas, con el fin de mejorar la experiencia de trabajar con el lenguaje subyacente, se conocen como preprocesadores. Hay dos preprocesadores populares de CSS: menos y Sass.  Estos preprocesadores agregar características a CSS, como la posibilidad de variables y reglas anidadas, lo que mejora la facilidad de mantenimiento de las hojas de estilos de grandes y complejas. CSS como un lenguaje es muy básica, que carecen de compatibilidad con incluso algo tan sencillo como variables y tiende a crear archivos CSS repetitivas e inflan. Agregar características de lenguaje de programación real a través de preprocesadores puede ayudar a reducir la duplicación y proporcionar una mejor organización de reglas de estilo. Visual Studio proporciona compatibilidad integrada para ambos menor y Sass, así como las extensiones que pueden mejorar la experiencia de desarrollo cuando se trabaja con estos idiomas.

Como un ejemplo rápido de cómo preprocesadores pueden mejorar la legibilidad y el mantenimiento de información de estilo, tenga en cuenta este CSS:

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

Con menor, esto se puede reescribir para eliminar la duplicación, todos con un *mixin* (denominada así porque permite "¡mix" propiedades de una clase o conjunto de reglas en el otro):

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

## <a name="less"></a>menos

El preprocesador de CSS inferior se ejecuta con Node.js. Para instalar menor, use el Administrador de paquetes de nodo (npm) desde un símbolo del sistema (-g significa "global"):

```console
npm install -g less
```

Si está utilizando Visual Studio, puede empezar a trabajar con menor agregando uno o más archivos Less al proyecto y, a continuación, configurando Gulp (o Grunt) para procesarlos en tiempo de compilación. Agregar un *estilos* carpeta al proyecto y, a continuación, agregue un nuevo menos con el nombre de archivo *main.less* en esta carpeta.

![Agregar archivo less](less-sass-fa/_static/add-less-file.png)

Una vez agregados, la estructura de carpetas debe tener un aspecto similar al siguiente:

![Estructura de carpetas](less-sass-fa/_static/folder-structure.png)

Ahora puede agregar algunos estilos básicos para el archivo, que se compila en CSS e implementa en la carpeta wwwroot Gulp.

Modificar *main.less* para incluir el contenido siguiente, que crea una paleta de colores simple desde un único color base.

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

`@base` y el otro @-prefixed elementos son variables. Cada uno de ellos representa un color. Excepto `@base`, está configurados con las funciones de color: aclarar, oscurecer y de número. Aclarar y oscurecer es prácticamente lo que cabría esperar; número ajusta el matiz de un color en un número de grados (alrededor de la rueda de color). El procesador de menos es lo suficientemente inteligente como para pasar por alto las variables que no se utilizan, por lo que para demostrar cómo funcionan estas variables, es necesario utilizarlas en algún lugar. Las clases `.baseColor`, etc. se muestran los valores calculados de cada una de las variables en el archivo CSS que se genera.

### <a name="get-started"></a>Primeros pasos

Crear un **archivo de configuración de npm** (*package.json*) en la carpeta del proyecto y editarlo para hacer referencia a `gulp` y `gulp-less`:

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

Instalar las dependencias en un símbolo del sistema en la carpeta del proyecto, o en Visual Studio **el Explorador de soluciones** (**dependencias > npm > Restaurar paquetes**).

```console
npm install
```

![FRENTE a restaurar los paquetes](less-sass-fa/_static/restore-packages.png)

En la carpeta del proyecto, cree una **Gulp archivo de configuración** (*gulpfile.js*) para definir el proceso automatizado.  Agregue una variable en la parte superior del archivo para representar menos y una tarea se ejecute menor:

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

Abra la **explorador del ejecutor de tareas** (**Vista > otras ventanas > Explorador del ejecutor de tareas**). Entre las tareas, verá una nueva tarea denominada `less`. Es posible que deba actualizar la ventana.

Ejecute la `less` tarea y obtener un resultado parecido al que se muestra aquí:

![menos ejecutor de tareas](less-sass-fa/_static/less-task-runner.png)

El *wwwroot/css* carpeta contiene ahora un nuevo archivo, *main.css*:

![css principal creado](less-sass-fa/_static/main-css-created.png)

Abra *main.css* y verá algo parecido a lo siguiente:

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

Agregar una página HTML sencilla para la *wwwroot* carpeta y referencia *main.css* para ver la paleta de colores en acción.

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

Puede ver que girar 180 grados en `@base` se usa para generar `@background` dio como resultado la rueda de color opuestas color de `@base`:

![en el ejemplo se prueba menos](less-sass-fa/_static/less-test-screenshot.png)

Menor también proporciona compatibilidad para reglas anidadas, así como las consultas de media anidadas. Por ejemplo, definir las jerarquías anidadas como menús pueden dar lugar a las reglas de CSS detalladas como estos:

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

Lo ideal es que todas las reglas de estilo relacionados se colocarán juntos en el archivo CSS, pero en la práctica que no hay nada aplicar esta regla excepto convención y quizás los comentarios del bloque.

Definir estas mismas reglas que reducen el uso es similar a esto:

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

Tenga en cuenta que en este caso, todos los elementos subordinados de `nav` están dentro de su ámbito. Ya no hay ninguna repetición de elementos primarios (`nav`, `li`, `a`), y el recuento de líneas total ha disminuido también (aunque algunos de que es el resultado de que los valores en las mismas líneas en el segundo ejemplo). Puede ser muy útil, organización ver todas las reglas de un elemento de interfaz de usuario concreto dentro de un ámbito explícitamente limitado, en este caso desactivar el resto del archivo de llaves.

El `&` sintaxis es una característica menos selector, con & que representa el elemento primario de selector actual. Por lo tanto, dentro de la una {...} bloque, `&` representa un `a` (etiqueta) y, por tanto, `&:link` es equivalente a `a:link`.

Consultas de medios, muy útiles para crear diseños de capacidad de respuesta, también pueden contribuir con un alto grado a repeticiones y la complejidad de CSS. Menor permite consultas de medios se anidan dentro de las clases, por lo que la definición de clase completo no tiene que repetirse dentro de otro nivel superior `@media` elementos. Por ejemplo, mostramos CSS para un menú dinámico:

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

Esto se puede definir mejor en menos como:

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

Otra característica de menor que ya hemos visto es su compatibilidad para operaciones matemáticas, lo que permite a los atributos de estilo se construyan a partir de variables definidas previamente. Esto hará que la actualización relacionados estilos mucho más fácil, ya que se puede modificar la variable de base y todos los valores dependientes cambian automáticamente.

Archivos CSS, especialmente para sitios de gran tamaño (y especialmente si se usan las consultas de medios), tienden a alcanzar un tamaño considerable con el tiempo, lo difícil trabajar con ellas. Menos archivos pueden definirse por separado, a continuación, extrae juntos mediante `@import` directivas. También se puede usar menor para importar CSS archivos individuales, también, si lo desea.

*Mixins* puede aceptar parámetros y menor admite una lógica condicional en forma de protecciones de mixin, que proporcionan una manera declarativa para definir cuándo determinadas mixins surten efecto. Un uso común de protecciones de mixin es ajustar los colores en función de la luz u oscuro el color de origen. Dado un mixin que acepta un parámetro para el color, un guardia mixin puede utilizarse para modificar el mixin en función de ese color:

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

Dada nuestra actual `@base` valo `#663333`, esta secuencia de comandos menos generará el código CSS siguiente:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Menor proporciona una serie de características adicionales, pero esto debe tener una idea de la potencia de este lenguaje de preprocesamiento.

## <a name="sass"></a>SASS

SASS es similar a la menor, lo que proporciona compatibilidad para muchas de las mismas características, pero con una sintaxis ligeramente diferente. Se compila con Ruby, en lugar de JavaScript y, por lo que tiene requisitos de instalación diferentes. El idioma de Sass original no usa llaves o punto y coma, sino que define ámbito mediante espacios en blanco y sangría. En la versión 3 de Sass, se introdujo una nueva sintaxis, **SCSS** ("CSS Sassy"). SCSS es similar a CSS en que se pasa por alto los espacios en blanco y niveles de sangría y en su lugar utiliza el punto y coma y llaves.

Para instalar Sass, normalmente se instala por primera vez Ruby (preinstalado en Mac) y, a continuación, ejecute:

```console
gem install sass
```

Sin embargo, si está ejecutando Visual Studio, puede empezar a trabajar con Sass prácticamente la misma manera como lo haría con menor. Abra *package.json* y agregar el paquete "gulp sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

A continuación, modifique *gulpfile.js* para agregar una variable de sass y una tarea para compilar los archivos Sass y colocar los resultados en la carpeta wwwroot:

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

Ahora puede agregar el archivo Sass *main2.scss* a la *estilos* carpeta en la raíz del proyecto:

![Agregar archivo scss](less-sass-fa/_static/add-scss-file.png)

Abra *main2.scss* y agregue lo siguiente:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Guarde todos los archivos. Ahora, al actualizar **explorador del ejecutor de tareas**, verá un `sass` tarea. Ejecútelo y, en la */wwwroot/css* carpeta. Ahora hay un *main2.css* archivo con este contenido:

```css
body {
    background-color: #CC0000;
}
```

SASS admite el anidamiento prácticamente lo mismo era que menor no, proporcionar beneficios similares. Los archivos se puede dividir función e incluido mediante la `@import` directiva:

```sass
@import 'anotherfile';
```

SASS admite mixins así, el uso de la `@mixin` palabra clave que se va a definirlos y `@include` para incluirlas, como en este ejemplo de [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Además de mixins, Sass también admite el concepto de herencia, lo que permite una clase Extender el otro. Es conceptualmente similar a un mixin, pero da como resultado menos código CSS. Se realiza mediante el `@extend` palabra clave. Para probar el mixins, agregue lo siguiente a su *main2.scss* archivo:

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

Examine la salida en *main2.css* después de ejecutar el `sass` de tareas en **explorador del ejecutor de tareas**:

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

Tenga en cuenta que todas las propiedades comunes de la alerta mixin se repiten en cada clase. El mixin realizó un buen trabajo para ayudar a eliminar la duplicación en tiempo de desarrollo, pero todavía está creando CSS con una gran cantidad de duplicación en él, lo que produce mayores que los archivos necesarios de CSS - un posible problema de rendimiento.

Ahora reemplace la alerta mixin con un `.alert` clase y cambiar `@include` a `@extend` (teniendo en cuenta ampliar `.alert`, no `alert`):

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

Ejecute Sass una vez más y examine el código CSS resultante:

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

Ahora las propiedades se definen únicamente como tantas veces como sea necesario y mejor se genera CSS.

SASS también incluye funciones y operaciones de lógica condicional, similares a un valor inferior. De hecho, las capacidades de los dos lenguajes son muy similares.

## <a name="less-or-sass"></a>¿Menor o Sass?

Todavía no hay ningún consenso en cuanto a si en general es mejor usar menor o Sass (o incluso si se prefiere la Sass original o la sintaxis SCSS más reciente en Sass). Probablemente la decisión más importante es **utilizar una de estas herramientas**, en lugar de simplemente codificados manualmente los archivos CSS. Una vez que haya realizado que ambos menor, la decisión y Sass son una buena elección.

## <a name="font-awesome"></a>Fuente Maravilla

Además de preprocesadores CSS, otro recurso excelente para aplicaciones web modernas de estilo es fantástica de fuente. Awesome de fuente es un Kit de herramientas que proporciona más de 500 iconos vectoriales escalables que pueden usarse libremente en sus aplicaciones web. Se diseñó originalmente para trabajar con arranque, pero no tiene ninguna dependencia en ese marco de trabajo o en cualquier biblioteca de JavaScript.

La manera más fácil empezar a trabajar con fuentes Maravilla consiste en Agregar una referencia a ella, mediante su ubicación de red (CDN) de entrega de contenido público:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

También puede agregarlo a su proyecto de Visual Studio, éste se agrega a las "dependencias" en *bower.json*:

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

Una vez que tenga una referencia a la fuente Maravilla en una página, puede agregar iconos a la aplicación aplicando fuente Maravilla clases, normalmente con el prefijo "fa-", a los elementos HTML alineado (como `<span>` o `<i>`).  Por ejemplo, puede agregar iconos a listas simples y los menús con código similar al siguiente:

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

Esto produce el siguiente en el explorador: tenga en cuenta el icono junto a cada elemento:

![iconos de la lista](less-sass-fa/_static/list-icons-screenshot.png)

Puede ver una lista completa de los iconos disponibles aquí:

http://fontawesome.io/icons/

## <a name="summary"></a>Resumen

Aplicaciones web modernas exigen cada vez más capacidad de respuesta, fluidos diseños que están limpios, intuitiva y fácil de usar desde una variedad de dispositivos. Administrar la complejidad de las hojas de estilos CSS necesaria para lograr estos objetivos mejor se realiza mediante un tipo de preprocesador menos o Sass. Además, kits de herramientas como fuente Maravilla rápidamente proporcionan iconos conocidos para los menús de navegación textual y experimentan de botones, mejorar la global del usuario de la aplicación.
