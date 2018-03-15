---
title: "Creación de sitios maravillosas y capacidad de respuesta con arranque y ASP.NET Core"
author: ardalis
description: "Obtenga información acerca de cómo utilizar el arranque para desarrollar aplicaciones web con capacidad de respuesta con ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: c3dfaa53e9e3277d025d014f65004e4c24a5acc4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Creación de sitios maravillosas y capacidad de respuesta con arranque y ASP.NET Core

<a name="bootstrap-index"></a>

Por [Steve Smith](https://ardalis.com/)

Bootstrap actualmente es el marco de trabajo web más popular para desarrollar aplicaciones web con capacidad de respuesta. Ofrece una serie de características y ventajas que pueden mejorar la experiencia de los usuarios con el sitio web, si tiene experiencia en el diseño de front-end y de desarrollo o de un experto. Bootstrap se implementa como un conjunto de archivos CSS y JavaScript y está diseñado para ayudar a su sitio Web o aplicación escala eficazmente desde teléfonos a tabletas a equipos de escritorio.

## <a name="get-started"></a>Primeros pasos

Hay varias maneras de empezar a trabajar con arranque. Si está iniciando una nueva aplicación web en Visual Studio, puede elegir la plantilla de inicio predeterminado para ASP.NET Core, en el que se vienen preinstalada Bootstrap mayúscula:

![Arrancar en la vista de solución de plantilla de inicio](bootstrap/_static/bootstrap-in-starter-template.png)

Adición de arranque a un núcleo de ASP.NET proyecto es simplemente cuestión de agregar a *bower.json* como una dependencia:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Se trata de la manera recomendada de agregar Bootstrap a un proyecto de ASP.NET Core.

También puede instalar bootstrap mediante uno de varios administradores de paquetes, como Bower, npm o NuGet. En cada caso, el proceso es básicamente el mismo:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> La manera recomendada para instalar las dependencias del lado cliente como programa previo de ASP.NET Core es a través de Bower (mediante *bower.json*, tal y como se muestra arriba). Se muestran el uso de npm/NuGet para demostrar cómo fácilmente arranque puede agregarse a otros tipos de aplicaciones web, incluidas las versiones anteriores de ASP.NET.

Si se están haciendo referencia a sus propias versiones locales de arranque, debe hacer referencia a ellos en todas las páginas que va a usar. En producción, a que debería hacer referencia arrancar con una CDN. En la plantilla de sitio ASP.NET de forma predeterminada, el *_Layout.cshtml* archivo tan similar a la siguiente:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Si se va a usar cualquiera de los complementos de jQuery de Bootstrap, también debe hacer referencia a jQuery.

## <a name="basic-templates-and-features"></a>Características y plantillas básicas

La plantilla de arranque más básica es muy similar a la *_Layout.cshtml* archivo que se muestra encima y simplemente incluye un menú básico para navegación y un lugar para representar el resto de la página.

### <a name="basic-navigation"></a>Navegación básica

La plantilla predeterminada utiliza un conjunto de `<div>` elementos que se representan una barra de navegación superior y el cuerpo principal de la página. Si usas HTML5, puede reemplazar la primera `<div>` etiquetar con un `<nav>` etiqueta que se va a obtener el mismo efecto, pero con una semántica más precisa. En este primer `<div>` verá hay varios otros. En primer lugar, un `<div>` con una clase de "contenedor" y, a continuación, en que, más dos `<div>` elementos: "encabezado de la barra de navegación" y "barra de navegación se contraen". El elemento div de encabezado de la barra de navegación incluye un botón que va a aparecer cuando la pantalla está por debajo de un determinado ancho mínimo, que muestra 3 líneas horizontales (un denominadas "icono de la hamburguesa"). El icono se representa con puro HTML y CSS; no se requiere ninguna imagen. Éste es el código que muestra el icono, con cada uno de los <span> etiquetas representar una de las barras blancas:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

También incluye el nombre de la aplicación, que aparece en la esquina superior izquierda. El menú de navegación principal se representa por la `<ul>` elemento dentro del segundo elemento div e incluye vínculos a casa, aproximadamente y póngase en contacto con. Vínculos adicionales para registrar y el inicio de sesión se agregan mediante la línea de _LoginPartial en línea 29. A continuación el panel de navegación, el cuerpo principal de cada página se representa en otra `<div>`, marcada con las clases "contenedor" y "contenido del cuerpo". En el archivo de _Layout predeterminado simple se muestra aquí, el contenido de la página se representa mediante la vista específica asociada a la página y, a continuación, una sencilla `<footer>` se agrega al final de la `<div>` elemento. Puede ver cómo integrado acerca de la página aparece con esta plantilla:

![Acerca de la página](bootstrap/_static/about-page-wide.png)

La barra de navegación contraída, con el botón de "hamburguesa" en la parte superior derecha, aparece cuando la ventana cae por debajo de un determinado ancho:

![acerca de la página con el menú de la hamburguesa](bootstrap/_static/about-page-hamburger.png)

Haga clic en el icono muestra los elementos de menú en un espacio vertical que se desliza hacia abajo desde la parte superior de la página:

![acerca de la página con el menú de la hamburguesa expandido](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografía y vínculos

Bootstrap configura tipografía básico del sitio, colores y el formato en el archivo CSS del enlace. Este archivo CSS incluye estilos predeterminados para las tablas, botones, elementos de formulario, imágenes etc. ([más](http://getbootstrap.com/css/)). Una característica útil es el sistema de diseño de cuadrícula, trata más adelante.

### <a name="grids"></a>Cuadrículas

Una de las características más populares de arranque es su sistema de diseño de cuadrícula. Aplicaciones web modernas deben evitar usar la `<table>` etiqueta para el diseño, en su lugar, restringir el uso de este elemento de datos tabulares reales. En su lugar, columnas y filas se pueden disponer mediante una serie de `<div>` elementos y las clases CSS adecuadas. Hay varias ventajas con respecto a este enfoque, incluida la capacidad para ajustar el diseño de cuadrículas para mostrar verticalmente en pantallas estrechas, como en teléfonos.

[Sistema de diseño de cuadrícula del arranque](http://getbootstrap.com/css/#grid) se basa en doce columnas. Se ha elegido este número porque puede dividirse uniformemente en 1, 2, 3 o 4 columnas y anchos de columna pueden variar a dentro de 1/12 del ancho vertical de la pantalla. Para empezar a usar el sistema de diseño de cuadrícula, debe comenzar con un contenedor de `<div>` y, a continuación, agregue una fila `<div>`, tal y como se muestra aquí:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

A continuación, agregar más `<div>` elementos para cada columna y especifique el número de columnas que `<div>` debe ocupar (fuera de 12) como parte de una clase CSS a partir de "col - md-". Por ejemplo, si desea simplemente tiene dos columnas del mismo tamaño, usaría una clase de "col-md-6" para cada uno de ellos. En este caso "md" es la abreviatura de "medio" y hace referencia a tamaños de pantalla del equipo de escritorio de tamaño estándar. Hay cuatro opciones diferentes, que puede elegir entre y cada una se usará para ancho superior a menos que se reemplaza (por lo que si desea que el diseño debe corregirse, independientemente del ancho de pantalla, basta con especificar clases extra).

Prefijo de la clase CSS | Nivel de dispositivo | Ancho
:---: | :---: | :---:
col-xs: | Teléfonos | < 768px
col-sm- | Tabletas | >= 768px
col-md- | Equipos de escritorio | >= 992px
col-lg: | Pantallas de escritorio más grandes | > = 1200 px

Al especificar dos columnas con "col-md-6" el diseño resultante será dos columnas en las soluciones de escritorio, pero estas dos columnas se apilan verticalmente cuando se representa en dispositivos más pequeños (o una ventana del explorador más restringida en un equipo de escritorio), lo que permite a los usuarios ver fácilmente contenido sin necesidad de desplazarse horizontalmente.

Bootstrap siempre predeterminada será un diseño de columna única, por lo que basta con especificar columnas cuando desee que más de una columna. La única vez que desearía especificar explícitamente que un `<div>` sería ocupan 12 todas las columnas invalidar el comportamiento de un mayor nivel de dispositivo. Al especificar varias clases de nivel de dispositivo, debe restablecer la presentación de las columnas en ciertos puntos. Agregar un elemento div clearfix que solo es visible dentro de una ventanilla de cierta puede lograrlo, como se muestra aquí:

![cuadrícula de la ventanilla anchas y estrechas](bootstrap/_static/narrow-and-wide-viewport-grid.png)

En el ejemplo anterior, uno y dos comparten una fila en el diseño "md", mientras que dos y tres comparten una fila en el diseño de "xs". Sin el clearfix `<div>`, dos y tres no se muestran correctamente en la vista de "xs" (tenga en cuenta que se muestra solo uno, cuatro y cinco):

![cuadrícula sin usar clearfix](bootstrap/_static/grid-without-clearfix.png)

En este ejemplo, una única fila `<div>` se utilizó, y sigue arranque principalmente ha sido lo correcto en relación con el diseño y apilado de las columnas. Por lo general, debe especificar una fila `<div>` para cada fila horizontal requiere el diseño y, por supuesto puede anidar arranque cuadrículas dentro de uno a otro. Al hacerlo, cada cuadrícula anidada ocupan 100% del ancho del elemento en el que se coloca, que, a continuación, se subdividen mediante las clases de columna.

### <a name="jumbotron"></a>Jumbotron

Si ha usado las plantillas ASP.NET MVC predeterminada en Visual Studio 2012 o 2013, probablemente ha visto el Jumbotron en acción. Hace referencia a una sección grande de ancho completo de una página que puede usarse para mostrar una imagen de fondo de gran tamaño, una llamada a la acción, una rotación o elementos similares. Para agregar una jumbotron a una página, basta con agregar un `<div>` y asígnele una clase de "jumbotron", a continuación, coloque un contenedor `<div>` dentro y agregar su contenido. Podemos ajustar fácilmente el estándar acerca de la página que se usará un jumbotron para los encabezados principales muestra:

![ejemplo de Jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Botones

En la ilustración siguiente se muestran las clases del botón predeterminado y sus colores.

![botones con temas](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Distintivos

Distintivos hacen referencia a las llamadas pequeño, normalmente numéricos junto a un elemento de navegación. Puede indicar un número de mensajes o notificaciones en espera, o a la presencia de actualizaciones. Especificar tales notificaciones es tan sencillo como agregar una <span> que contiene el texto, con una clase de "tarjeta":

![distintivos con temas](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertas

Puede que necesite mostrar algún tipo de notificación, la alerta o el mensaje de error a los usuarios de la aplicación. Es donde las clases estándar de alertas son útiles. Hay cuatro niveles de gravedad diferentes con combinaciones de colores asociados:

![alertas con temas](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menús y barras de exploración

Nuestro diseño ya incluye una barra de navegación estándar, pero el tema de arranque es compatible con opciones de estilo adicionales. Podemos realizar fácilmente decidimos para mostrar la barra de navegación verticalmente en lugar de horizontalmente si lo que se prefiere, así como agregar navegación secundaria elementos en los menús desplegables. Los menús de navegación sencillo, como bandas de ficha, se generan en la parte superior de <ul> elementos. Estos pueden crearse muy basta con que se proporcionen con las clases CSS "nav" y "nav pestañas":

![orientado con temas](bootstrap/_static/theme-tabstrips.png)

Barras de exploración se generan de forma similar, pero son un poco más complejas. Se inician con un `<nav>` o `<div>` con una clase de "barra de navegación," dentro del cual un elemento div contenedor contiene el resto de los elementos. Nuestra página incluye una barra de navegación en el encabezado ya; se muestra a continuación simplemente se expande en el objeto, agrega compatibilidad para un menú desplegable:

![barras de exploración con temas](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementos adicionales

También puede utilizarse el tema predeterminado para presentar tablas HTML en un estilo bien formateado, incluida la compatibilidad con vistas con bandas. Hay etiquetas con estilos que son similares a las de los botones. Puede crear menús de lista desplegable personalizados que admiten opciones de estilo adicionales más allá de HTML estándar `<select>` elemento, junto con barras de exploración como el nuestro sitio de inicio predeterminada ya está usando. Si necesita una barra de progreso, hay varios estilos para elegir, así como enumerar los grupos y paneles que incluyen un título y el contenido. Explorar opciones adicionales en el tema de arranque estándar aquí:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Más temas

Puede ampliar el tema de arranque estándar reemplazando algunos o todos sus CSS, ajustar los colores y estilos para satisfacer las necesidades de su propia aplicación. Si desea iniciar desde un tema listos para su uso, hay varias galerías del tema disponibles en línea que están especializados en temas de arranque, como WrapBootstrap.com (que tiene una gran variedad de temas comerciales) y Bootswatch.com (que ofrece temas libres). Algunas de las plantillas de pagadas disponibles proporcionan una gran cantidad de funcionalidad sobre el tema de arranque básica, como compatibilidad enriquecida para los menús administrativos y los paneles con medidores y gráficos enriquecidos. Un ejemplo de una plantilla de pago popular es Inspinia, actualmente para la venta de $18, que incluye una plantilla de MVC5 de ASP.NET además de AngularJS y versiones HTML estáticas. A continuación se muestra una captura de pantalla de ejemplo.

![Inspinia de tema de ejemplo](bootstrap/_static/theme-inspinia.png)

Si desea cambiar el tema del arranque, coloque el *bootstrap.css* archivo para el tema que desee en el **wwwroot/css** carpeta y cambie las referencias en *_Layout.cshtml* para que lo señale. Cambie los vínculos para todos los entornos:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Si desea crear su propio panel, puede iniciar desde el ejemplo libre está disponible aquí: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Componentes

Además de los elementos ya mencionados, arranque incluye compatibilidad para una gran variedad de [componentes de interfaz de usuario integrados](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap incluye conjuntos de iconos de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), con más de 200 iconos libremente disponibles para su uso dentro de la aplicación web habilitado para arranque. Aquí es sólo una pequeña muestra:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>grupos de entrada

Grupos de entrada permiten la agrupación de botones con un elemento de entrada, o de texto adicional que ofrece al usuario una experiencia más intuitiva:

![grupos de entrada](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Rutas de exploración

Rutas son un componente de interfaz de usuario común usado para mostrar su historial reciente o la profundidad de la jerarquía de navegación de un sitio a un usuario. Agregarlos fácilmente mediante la aplicación de la clase "ruta de navegación" a cualquier `<ol>` elemento de lista. Incluye compatibilidad integrada para la paginación mediante el uso de la clase "paginación" en un `<ul>` elemento dentro de un `<nav>`. Agregar presentaciones incrustados con capacidad de respuesta y vídeo con `<iframe>`, `<embed>`, `<video>`, o `<object>` elementos de arranque se estilo automáticamente. Especifique una relación de aspecto determinada mediante el uso de clases concretas como "Insertar-respondiendo-16by9".

## <a name="javascript-support"></a>Compatibilidad con JavaScript

Biblioteca de JavaScript del arranque incluye compatibilidad con la API para los componentes incluyen, lo que le permite controlar su comportamiento mediante programación dentro de la aplicación. Además, *bootstrap.js* incluye más de una docena complementos de jQuery personalizados, proporcionar características adicionales, como las transiciones, cuadros de diálogo modales, desplácese detección (actualicen los estilos en función de dónde se desplaza el usuario en el documento), comportamiento de contraer, cintas y colocación los menús de la ventana, por lo que no se desplazan fuera de la pantalla. No hay espacio suficiente para cubrir todos los complementos de JavaScript que se integran Bootstrap: para aprender más, visite [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Resumen

Bootstrap proporciona un marco web que puede usarse para diseñar y aplicar estilo a una gran variedad de sitios Web y aplicaciones de manera rápida y productiva. Su tipografía básico y estilos proporcionan una apariencia y funcionamiento agradable que pueden manipular fácilmente mediante la compatibilidad de tema personalizado, que se puede crear manualmente o adquirió mercado. Admite una serie de componentes web que en el pasado, tendrías que hacer costosos controles de otros fabricantes para lograr que la ejecución de los estándares web modernos y abrir.
