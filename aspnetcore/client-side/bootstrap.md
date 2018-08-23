---
title: Crear sitios atractivos y capacidad de respuesta con Bootstrap y ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar Bootstrap para desarrollar aplicaciones web dinámicas con ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836759"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Crear sitios atractivos y capacidad de respuesta con Bootstrap y ASP.NET Core

<a name="bootstrap-index"></a>

Por [Steve Smith](https://ardalis.com/)

Bootstrap es actualmente el marco web más popular para desarrollar aplicaciones web con capacidad de respuesta. Ofrece una serie de características y ventajas que pueden mejorar la experiencia del usuario con el sitio web, si tiene experiencia en desarrollo y diseño front-end o a un experto. Bootstrap se implementa como un conjunto de archivos CSS y JavaScript y está diseñado para ayudar a su sitio Web o aplicación escalar eficazmente desde teléfonos a tabletas hasta equipos de escritorio.

## <a name="get-started"></a>Primeros pasos

Hay varias maneras de empezar a trabajar con Bootstrap. Si está iniciando una nueva aplicación web en Visual Studio, puede elegir la plantilla de inicio predeterminado para ASP.NET Core, en la que procederá preinstalado Bootstrap case:

![Arrancar en la vista de solución de plantilla de inicio](bootstrap/_static/bootstrap-in-starter-template.png)

Adición de Bootstrap a un núcleo de ASP.NET proyecto es simplemente una cuestión de agregar a *bower.json* como una dependencia:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Se trata de la manera recomendada de agregar Bootstrap a un proyecto de ASP.NET Core.

También puede instalar utilizando uno de varios administradores de paquetes, como NuGet, npm o Bower de bootstrap. En cada caso, el proceso es básicamente el mismo:

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
> La manera recomendada para instalar las dependencias del lado cliente como Bootstrap en ASP.NET Core es a través de Bower (mediante *bower.json*, tal y como se muestra arriba). Se muestran el uso de npm y NuGet para demostrar cómo Bootstrap puede agregarse fácilmente a otros tipos de aplicaciones web, incluidas las versiones anteriores de ASP.NET.

Si está haciendo referencia a sus propias versiones locales de Bootstrap, deberá hacer referencia a ellos en las páginas que va a usar. En producción debe hacer referencia mediante una red CDN de bootstrap. En la plantilla de sitio ASP.NET Core predeterminada, el *_Layout.cshtml* archivo así como este:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Si va a usar cualquiera de los complementos de jQuery de Bootstrap, también deberá hacer referencia a jQuery.

## <a name="basic-templates-and-features"></a>Las características y las plantillas básicas

La plantilla más básica de Bootstrap se parece mucho a la *_Layout.cshtml* archivo que se muestra arriba y simplemente incluye un menú básico para la navegación y un lugar para representar el resto de la página.

### <a name="basic-navigation"></a>Navegación básica

La plantilla predeterminada usa un conjunto de `<div>` elementos para representar una barra de navegación superior y el cuerpo de la página principal. Si usa HTML5, puede reemplazar la primera `<div>` etiquetar con un `<nav>` etiqueta para obtener el mismo efecto, pero con una semántica más precisa. En este primer `<div>` puede ver que hay otras. En primer lugar, un `<div>` con una clase de "contenedor" y, a continuación, en que, más dos `<div>` elementos: "navbar-header" y "navbar-collapse". La etiqueta div de encabezado de la barra de navegación incluye un botón que va a aparecer cuando la pantalla está por debajo de un determinado ancho mínimo, que muestra 3 líneas horizontales (un llamado "icono de la hamburguesa"). El icono se representa utilizando puro HTML y CSS; no se requiere ninguna imagen. Este es el código que muestra el icono, con cada uno de los <span> etiquetas representar una de las barras en blanco:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

También incluye el nombre de la aplicación, que aparece en la esquina superior izquierda. El menú de navegación principal se representa mediante el `<ul>` elemento dentro de la segunda etiqueta div e incluye vínculos a casa, aproximadamente y póngase en contacto con. A continuación el panel de navegación, el cuerpo principal de cada página se representa en otro `<div>`, marcados con las clases de "contenedor" y "contenido del cuerpo". En el valor predeterminado simple \_archivo de diseño se muestra a continuación, el contenido de la página se representa por la vista concreta asociada con la página y, a continuación, un simple `<footer>` se agrega al final de la `<div>` elemento. Puede ver cómo integrado acerca de la página aparece con esta plantilla:

![Acerca de la página](bootstrap/_static/about-page-wide.png)

La barra de navegación contraída, con el botón "hamburguesa" en la esquina superior derecha, aparece cuando la ventana cae por debajo de un determinado ancho:

![acerca de la página con el menú de hamburguesa](bootstrap/_static/about-page-hamburger.png)

Al hacer clic en el icono se revela los elementos de menú en un cajón vertical que se desliza hacia abajo desde la parte superior de la página:

![página acerca de con menú hamburguesa expandido](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Vínculos y tipografía

Bootstrap configura el sitio tipografía básica, colores y el formato en su archivo CSS del enlace. Este archivo CSS incluye los estilos predeterminados para las tablas, botones, elementos de formulario, imágenes etc. ([más](http://getbootstrap.com/css/)). Una característica particularmente útil es el sistema de diseño de cuadrícula, que explicaré a continuación.

### <a name="grids"></a>Cuadrículas

Una de las características más populares de Bootstrap es su sistema de diseño de cuadrícula. Aplicaciones web modernas deben evitar el uso del `<table>` etiqueta para el diseño, en su lugar, restringir el uso de este elemento con los datos tabulares reales. En su lugar, columnas y filas se pueden disponer mediante una serie de `<div>` elementos y las clases CSS adecuadas. Hay varias ventajas de este enfoque, incluida la capacidad de ajustar el diseño de cuadrículas para mostrar verticalmente en las pantallas estrechas, como en teléfonos.

[Sistema de diseño de cuadrícula de bootstrap](http://getbootstrap.com/css/#grid) se basa en doce columnas. Este número se ha elegido porque pueden dividirse uniformemente en 1, 2, 3 o 4 columnas, y los anchos de columna pueden variar para dentro de 1/12 del ancho vertical de la pantalla. Para empezar a usar el sistema de diseño de cuadrícula, debe comenzar con un contenedor `<div>` y, a continuación, agregue una fila `<div>`, tal y como se muestra aquí:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

A continuación, agregue más `<div>` elementos para cada columna y especifique el número de columnas que `<div>` debe ocupar (fuera de 12) como parte de una clase CSS a partir de "col - md-". Por ejemplo, si desea simplemente tener dos columnas del mismo tamaño, podría usar una clase de "col-md-6" para cada uno de ellos. En este caso "md" es la abreviatura de "medium" y hace referencia a los tamaños de pantalla del equipo de escritorio de tamaño estándar. Hay cuatro opciones diferentes, que puede elegir entre y cada una se usará para anchos de mayor a menos que se reemplaza (por lo que si desea que el diseño corregirse, independientemente del ancho de pantalla, basta con especificar clases xs).

Prefijo de la clase CSS | Capa de dispositivo | Ancho
:---: | :---: | :---:
col-xs - | Teléfonos | < 768px
col-sm - | Tabletas | > = 768px
col-md - | Equipos de escritorio | > = 992px
col-lg: | Pantallas de escritorio más grandes | > = 1200 px

Al especificar dos columnas con "col-md-6" el diseño resultante será dos columnas con resoluciones de escritorio, pero estas dos columnas se apilarán verticalmente cuando se representa en dispositivos más pequeños (o una ventana del explorador estrecha en un equipo de escritorio), lo que permite a los usuarios ver fácilmente contenido sin necesidad de desplazarse horizontalmente.

Bootstrap siempre será un diseño de una sola columna, por lo que solo deberá especificar las columnas cuando desee que más de una columna. La única vez que desearía especificar explícitamente que un `<div>` sería ocupan 12 todas las columnas invalidar el comportamiento de un mayor nivel de dispositivo. Al especificar varias clases de nivel de dispositivo, deberá restablecer la representación de columnas en determinados puntos. Agregar un elemento div clearfix que solo es visible dentro de una ventanilla de determinados puede lograr esto, como se muestra aquí:

![cuadrícula de la ventanilla anchas y estrechas](bootstrap/_static/narrow-and-wide-viewport-grid.png)

En el ejemplo anterior, uno y dos comparten una fila en el diseño "md", mientras dos y tres comparten una fila en el diseño de "xs". Sin el clearfix `<div>`, dos y tres no se muestran correctamente en la vista "xs" (tenga en cuenta que se muestra solo uno, cuatro y cinco):

![cuadrícula sin usar clearfix](bootstrap/_static/grid-without-clearfix.png)

En este ejemplo, una única fila `<div>` se ha usado, y sigue Bootstrap principalmente hicieron lo correcto en relación con el diseño y el agrupamiento de las columnas. Normalmente, debe especificar una fila `<div>` para cada fila horizontal requiere el diseño y, por supuesto puede anidar las cuadrículas de Bootstrap dentro de uno a otro. Al hacerlo, cada cuadrícula anidada ocupará el 100% del ancho del elemento en el que se coloca, que, a continuación, se puede subdividir mediante las clases de columna.

### <a name="jumbotron"></a>Jumbotron

Si ha usado las plantillas de ASP.NET Core MVC predeterminada en Visual Studio 2012 o 2013, probablemente ha visto la Jumbotron en acción. Se refiere a una sección grande de ancho completo de una página que se puede usar para mostrar una imagen de fondo de gran tamaño, una llamada a la acción, una rotación o elementos similares. Para agregar un jumbotron a una página, agregue simplemente un `<div>` y asígnele una clase de "jumbotron", a continuación, coloque un contenedor `<div>` dentro y agregar contenido. Podemos ajustar fácilmente el estándar acerca de la página que se usará un jumbotron para los encabezados principales que muestra:

![ejemplo de Jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Botones

En la ilustración siguiente se muestran las clases del botón predeterminado y sus colores.

![botones con temas](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Distintivos

Distintivos hacen referencia a pequeño, normalmente numéricas llamadas junto a un elemento de navegación. Puede indicar un número de mensajes o notificaciones en espera, o la presencia de las actualizaciones. Especificar dichas notificaciones es tan sencillo como agregar un `<span>` que contiene el texto, con una clase de "notificación":

![notificaciones con temas](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertas

Es posible que deba mostrar algún tipo de notificación, alerta o mensaje de error a los usuarios de la aplicación. Es donde las clases estándar de alerta son útiles. Hay cuatro niveles de gravedad diferentes con esquemas de color asociado:

![alertas con temas](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menús y barras de exploración

Nuestro diseño ya incluye una barra de navegación estándar, pero el tema de Bootstrap admite opciones adicionales de la aplicación de estilos. También podemos optar para mostrar la barra de navegación vertical, en lugar de horizontalmente si lo que se prefiere, así como agregar subnavegación los elementos de menús desplegables de. Menús de navegación sencillas, como bandas de pestaña, se crean en la parte superior de `<ul>` elementos. Éstos se pueden crear muy basta con que se proporcionen con las clases CSS "nav" y "pestañas de navegación":

![TabStrip con temas](bootstrap/_static/theme-tabstrips.png)

Barras de exploración se compilan de forma similar, pero son un poco más complejos. Se inician con un `<nav>` o `<div>` con una clase de "barra de navegación," dentro del cual un div contenedor contiene el resto de los elementos. Nuestra página incluye una barra de navegación en su encabezado ya: se muestra a continuación simplemente se expande en este caso, agregar compatibilidad con un menú desplegable:

![barras de exploración con temas](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementos adicionales

También puede utilizarse el tema predeterminado para presentar las tablas HTML en un estilo bien formateado, incluida la compatibilidad con vistas seccionadas. Existen etiquetas con los estilos que son similares a las de los botones. Puede crear menús desplegables personalizados que admiten opciones de estilos adicionales más allá del HTML estándar `<select>` elemento, junto con barras de exploración como el nuestro sitio de inicio predeterminada ya está usando. Si necesita una barra de progreso, existen varios estilos que elegir, así como enumerar los grupos y paneles que incluyen un título y el contenido. Explorar opciones adicionales del tema de Bootstrap estándar aquí:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Más temas

Puede ampliar el tema de Bootstrap estándar mediante la invalidación de algunos o todos sus CSS, ajustar los colores y estilos para satisfacer las necesidades de su propia aplicación. Si desea iniciar desde un tema listos para su uso, hay varias galerías de tema disponibles en línea que se especializan en los temas de arranque, como WrapBootstrap.com (que tiene una variedad de temas comerciales) y Bootswatch.com (que ofrece temas gratuitos). Algunas de las plantillas de pago disponibles proporcionan un amplio abanico de funcionalidades en la parte superior del tema de Bootstrap básico, como la compatibilidad enriquecida con menús administrativos y paneles con medidores y gráficos enriquecidos. Un ejemplo de una plantilla de pago popular es Inspinia, que se muestra en la captura de pantalla siguiente:

![Inspinia de tema de ejemplo](bootstrap/_static/theme-inspinia.png)

Si desea cambiar el tema de Bootstrap, coloque el *bootstrap.css* archivo para el tema que desee en el **wwwroot/css** carpeta y cambie las referencias en *_Layout.cshtml* para que lo señale. Cambie los vínculos para todos los entornos:

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

Además de los elementos ya descritos, Bootstrap incluye compatibilidad para una variedad de [componentes de interfaz de usuario integrados](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap incluye conjuntos de iconos de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), con más de 200 iconos libremente disponibles para su uso dentro de la aplicación web con Bootstrap habilitado. Aquí es sólo una pequeña muestra:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Grupos de entrada

Grupos de entrada permiten la agrupación de botones con un elemento de entrada, o texto adicional que proporciona el usuario con una experiencia más intuitiva:

![grupos de entrada](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Rutas de navegación

Las rutas de navegación son un componente de interfaz de usuario común usado para mostrar su historial reciente o la profundidad de la jerarquía de navegación de un sitio a un usuario. Agregarlos fácilmente mediante la aplicación de la clase "ruta de exploración" a cualquier `<ol>` elemento de lista. Incluye compatibilidad integrada para la paginación mediante el uso de la clase "paginación" en un `<ul>` elemento dentro de un `<nav>`. Agregar vídeo y presentaciones de diapositivas con capacidad de respuesta incrustado mediante `<iframe>`, `<embed>`, `<video>`, o `<object>` elementos, que Bootstrap se estilo automáticamente. Especifique una relación de aspecto determinada mediante el uso de clases específicas, como "Insertar-con capacidad de respuesta-16by9".

## <a name="javascript-support"></a>Compatibilidad con JavaScript

Biblioteca de JavaScript de bootstrap incluye compatibilidad con la API para los componentes incluidos, lo que permite controlar su comportamiento mediante programación dentro de la aplicación. Además, *bootstrap.js* incluye más de una docena complementos personalizados de jQuery, que proporciona características adicionales, como las transiciones, cuadros de diálogo modales, desplácese hacia la detección (actualizando estilos según donde se ha desplazado el usuario en el documento), comportamiento de contraer, carruseles y colocación los menús de la ventana, por lo que no se desplazan fuera de la pantalla. No hay espacio suficiente para cubrir todos los complementos de JavaScript integrados en Bootstrap: para más información, visite [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Resumen

Bootstrap ofrece un marco web que puede usarse para diseñar y una amplia variedad de sitios Web y aplicaciones de estilo rápida y productiva. Tipografía básica y los estilos proporcionan una apariencia y funcionamiento agradable que fácilmente se pueden manipular mediante la compatibilidad de tema personalizado, que puede ser artesanal o adquirir comercialmente. Admite una serie de componentes web que, en el pasado, habría requerido costosos controles de terceros para lograr, al tiempo que admite los estándares web modernos y abiertos.
