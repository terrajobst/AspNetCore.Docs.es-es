---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Vistas y ViewModels | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 3 aborda las vistas y ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 0cfdc2864221a0efc81eb362571c72f20eb05af8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368740"
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Vistas y ViewModels
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 3 aborda las vistas y ViewModels.


Hasta ahora nos hemos simplemente ha devolver cadenas acciones de controlador. Que es una buena forma de hacerse una idea de cómo funcionan los controladores, pero es no cómo se desea crear una aplicación web real. Vamos a quiera una manera mejor de generar HTML a exploradores que visitan nuestro sitio: uno donde podemos usar los archivos de plantilla para personalizar más fácilmente el contenido HTML volver a enviar. Eso es exactamente lo que hacer las vistas.

## <a name="adding-a-view-template"></a>Adición de una plantilla de vista

Para usar una plantilla de vista, se deberá cambiar el método de índice de HomeController para devolver un ActionResult y que devuelva View(), al igual que a continuación:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

El cambio anterior indica que, en lugar de devolver una cadena, queremos usar una "vista" para generar un resultado de vuelta en su lugar.

Ahora vamos a agregar una plantilla de vista adecuada para nuestro proyecto. Para ello se deberá colocar el cursor de texto dentro del método de acción del índice, a continuación, haga clic en y seleccione "Agregar vista". Se abrirá el cuadro de diálogo Agregar vista:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

El cuadro de diálogo "Agregar vista" nos permite rápida y fácilmente generar los archivos de plantilla de vista. Cuadro de diálogo rellena previamente el nombre de la plantilla de vista para crear para que coincida con el método de acción que va a usar de manera predeterminada en "Agregar vista". Dado que hemos usado el menú contextual "Agregar vista" dentro del método de acción de Index() de nuestro HomeController, el cuadro de diálogo "Agregar vista" anterior tiene "Index" como el nombre de vista que se rellenan de forma predeterminada. No necesitamos cambiar cualquiera de las opciones de este cuadro de diálogo, haga clic en el botón Agregar.

Cuando hacemos clic en el botón Agregar, Visual Web Developer creará un Index.cshtml nueva plantilla de vista para nosotros en el directorio \Views\Home, creación de la carpeta si aún no existe.

![](mvc-music-store-part-3/_static/image2.png)

El nombre y una ubicación del archivo "Index.cshtml" es importante y sigue las convenciones de nomenclatura predeterminada ASP.NET MVC. El nombre del directorio, \Views\Home, coincide con el controlador - que se denomina HomeController. El nombre de plantilla de vista Index, coincide con el método de acción de controlador que se va a mostrar la vista.

ASP.NET MVC nos permite evitar tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando usamos esta convención de nomenclatura para devolver una vista. De forma predeterminada procesará la plantilla de vista \Views\Home\Index.cshtml cuando escribimos código similar al siguiente a continuación dentro de nuestro HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer crea y abre la plantilla de vista "Index.cshtml" después de que se hace clic en el botón "Agregar" en el cuadro de diálogo "Agregar vista". A continuación, se muestra el contenido de Index.cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Esta vista es mediante la sintaxis de Razor, que es más concisa que el motor de vistas de formularios Web Forms usado en formularios Web Forms de ASP.NET y las versiones anteriores de ASP.NET MVC. El motor de vistas de formularios Web Forms todavía está disponible en ASP.NET MVC 3, pero muchos desarrolladores consideran que el motor de vistas Razor se ajusta a desarrollo de ASP.NET MVC realmente bien.

Las tres primeras líneas establecen el título de página con ViewBag.Title. Se examinará cómo funciona esto con más detalle en breve, pero primero vamos a actualizar el texto del título de texto y ver la página. Actualización de la &lt;h2&gt; etiqueta a la que quiere decir "Esta es la página principal", tal como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Ejecuta la aplicación de muestra que nuestro nuevo texto esté visible en la página principal.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Uso de un diseño para los elementos comunes del sitio

La mayoría de los sitios Web tienen contenido que se comparte entre varias páginas: exploración, pies de página, las imágenes de logotipo, referencias de hoja de estilos, etcetera. El motor de vistas Razor facilita esta tarea administrar el uso de una página denominada \_Layout.cshtml que se ha creado automáticamente para que podamos dentro de la carpeta/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Haga doble clic en esta carpeta para ver el contenido, que se indican a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Se mostrará el contenido de nuestras vistas individuales de la @RenderBodycomando () y cualquier contenido común que se va a aparecer fuera de la que se pueden agregar a la \_Layout.cshtml marcado. Desearemos nuestro Store música de MVC para tener un encabezado común con vínculos a nuestra área de página principal y Store de todas las páginas del sitio, por lo que vamos a agregar a la plantilla directamente encima de esa @RenderBodyinstrucción ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Actualización de la hoja de estilos

La plantilla proyecto vacío incluye un archivo CSS muy simplificado que solo incluye los estilos que se utiliza para mostrar mensajes de validación. El diseñador ha proporcionado algunos adicionales CSS e imágenes para definir la apariencia de nuestro sitio, así que agregaremos los ahora.

El archivo CSS e imágenes actualizados se incluyen en el directorio de contenido de Assets.zip MvcMusicStore que está disponible en [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store). Seleccionaremos ambos en el Explorador de Windows y colocarlos en la carpeta de contenido de nuestra solución en Visual Web Developer, tal como se muestra a continuación:

![](mvc-music-store-part-3/_static/image5.png)

Le pedirá que confirme si desea sobrescribir el archivo Site.css existente. Haga clic en Sí.

![](mvc-music-store-part-3/_static/image6.png)

La carpeta de contenido de la aplicación aparecerá ahora como sigue:

![](mvc-music-store-part-3/_static/image7.png)

Ahora vamos a ejecutar la aplicación y ver el aspecto de los cambios en la página principal.

![](mvc-music-store-part-3/_static/image8.png)

- Vamos a revisar lo que ha cambiado: método de acción de HomeController el índice se encuentra y muestra la plantilla \Views\Home\Index.cshtmlView, aunque nuestro código llamado "return View()", porque la plantilla de vista seguido la convención de nomenclatura estándar.
- Un simple mensaje de bienvenida que se define dentro de la plantilla de vista \Views\Home\Index.cshtml muestra la página principal.
- Está usando la página principal de nuestra \_Layout.cshtml plantilla, por lo que se encuentra el mensaje de bienvenida del diseño del sitio estándar HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usar un modelo para pasar información a nuestro punto de vista

Una plantilla de vista que muestra HTML codificados no va a hacer que un sitio web muy interesantes. Para crear un sitio web dinámico, en su lugar, desearemos pasar información de las acciones del controlador a nuestras plantillas de vista.

En el patrón Model-View-Controller, el término A que modelo hace referencia objetos que representan los datos de la aplicación. A menudo, los objetos del modelo se corresponden con las tablas de la base de datos, pero no tienen que.

Los métodos de acción de controlador que devuelven un ActionResult pueden pasar un objeto de modelo a la vista. Esto permite que un controlador para el paquete correctamente toda la información necesaria para generar una respuesta y, a continuación, pase la esta información para una plantilla de vista a usar para generar la respuesta adecuada de HTML. Esto es más fácil comprender por verlo en acción, así que vamos a empezar a trabajar.

En primer lugar, vamos a crear algunas clases de modelo para representar los géneros y álbumes dentro de nuestra tienda. Comencemos por crear una clase de género. Haga clic en la carpeta "Models" dentro del proyecto, elija la opción "Agregar clase" y "Genre.cs" el nombre del archivo.

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

A continuación, agregue una propiedad de nombre de cadena pública a la clase que se creó:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: en caso lo está preguntando, el {get; configurar;} notación está haciendo uso de la característica propiedades autoimplementadas de C#. Esto nos proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.*

A continuación, siga los mismos pasos para crear una clase de álbum (denominada Album.cs) que tiene un título y una propiedad de género:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Ahora podemos modificar el StoreController para usar las vistas que muestran información dinámica de nuestro modelo. Si - para fines de demostración - ahora hemos llamado a nuestro álbumes según el identificador de solicitud, podríamos mostramos esa información como se muestra en la vista siguiente.

![](mvc-music-store-part-3/_static/image10.png)

Comenzaremos por cambiar la acción de Store detalles para que muestre la información de un álbum. Agregue una instrucción "using" en la parte superior de la **StoreControllers** clase para que incluya el espacio de nombres MvcMusicStore.Models, por lo que no necesitamos escribir MvcMusicStore.Models.Album cada vez que deseamos utilizar la clase del álbum. La sección "using" de esa clase debería aparecer ahora como sigue.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

A continuación, actualizaremos la acción de controlador de detalles para que devuelva un ActionResult en lugar de una cadena, como hicimos con el método de índice de HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Ahora podemos modificar la lógica para devolver un objeto de álbum a la vista. Más adelante en este tutorial se recuperará los datos de una base de datos, pero por ahora vamos a utilizar "ficticio datos" para empezar a trabajar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: Si no está familiarizado con C#, se puede suponer que usar var significa que nuestra variable álbum es en tiempo de ejecución. Esto no es correcto: el compilador de C# usa la inferencia de tipos en función de lo que estamos asignar a la variable para determinar el que álbum es de tipo álbum y compilar la variable local álbum como tipo de álbum, por lo que obtenemos la comprobación en tiempo de compilación y el editor de código de Visual Studio soporte técnico.*

Ahora creemos una plantilla de vista que usa nuestro álbum para generar una respuesta HTML. Antes de hacerlo es necesario compilar el proyecto para que el cuadro de diálogo Agregar vista sepa sobre nuestra clase álbum recién creado. Puede compilar el proyecto seleccionando el Debug⇨Build MvcMusicStore elemento de menú (para obtener información adicional, puede utilizar el método abreviado Ctrl-Mayús-B para compilar el proyecto).

![](mvc-music-store-part-3/_static/image11.png)

Ahora que hemos configurado nuestra clases auxiliares, estamos listos para crear la plantilla de vista. Haga doble clic dentro del método de detalles y seleccione "Agregar vista..." en el menú contextual.

![](mvc-music-store-part-3/_static/image12.png)

Vamos a crear una nueva plantilla de vista como hicimos antes con HomeController. Dado que vamos a crear desde el StoreController de forma predeterminada se generará en un archivo \Views\Store\Index.cshtml.

A diferencia de antes, vamos a activar la casilla de la vista "Crear fuertemente tipada". A continuación, vamos a seleccionar de nuestra clase "Álbum" dentro de la lista-downlist "Ver datos-class". Esto hará que el cuadro de diálogo "Agregar vista" crear una plantilla de vista que se espera que un álbum de objeto se pasa al que se va a usar.

![](mvc-music-store-part-3/_static/image13.png)

Cuando hacemos clic en el botón "Agregar" se creará la plantilla de vista \Views\Store\Details.cshtml, que contiene el código siguiente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Tenga en cuenta la primera línea, que indica que esta vista está fuertemente tipado en nuestra clase álbum. El motor de vistas Razor entiende que se ha pasado un objeto de álbum, para que podamos acceder fácilmente a las propiedades del modelo y aún tiene la ventaja de IntelliSense en el editor de Visual Web Developer.

Actualización de la &lt;h2&gt; etiquetar para que se muestre la propiedad de título del álbum mediante la modificación de esa línea debe aparecer como sigue.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Tenga en cuenta que IntelliSense se activará cuando se escribe el período tras el @Model palabra clave, que muestra las propiedades y métodos que admite la clase del álbum.

Ahora vamos a volver a ejecutar nuestro proyecto y visite la dirección URL de Store/detalles/5. Veremos los detalles de un álbum, como a continuación.

![](mvc-music-store-part-3/_static/image14.png)

Ahora nos aseguraremos de hacer una actualización similar para el método de acción examinar Store. Actualice el método para que devuelva un ActionResult y modificar la lógica del método, por lo que crea un nuevo objeto de género y lo devuelve a la vista.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Haga clic en el método de exploración y seleccione "Agregar vista..." en el menú contextual, a continuación, agregará una vista que está fuertemente tipada agregue fuertemente tipados a la clase de género.

![](mvc-music-store-part-3/_static/image15.png)

Actualización de la &lt;h2&gt; elemento en la vista de código (en /Views/Store/Browse.cshtml) para mostrar la información de género.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

¿Ahora vamos a volver a ejecutar nuestro proyecto y vaya a/Store/examinar? Género = la dirección URL del Disco. Veremos la página Examinar muestra como a continuación.

![](mvc-music-store-part-3/_static/image16.png)

Por último, vamos a hacer una actualización de un poco más compleja la **Store índice** método de acción y vista para mostrar una lista de todos los géneros en nuestra tienda. Haremos esto mediante el uso de una lista de géneros como el objeto del modelo, en lugar de simplemente un género único.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Haga clic en el método de acción del índice de Store y seleccione Agregar vista como antes, seleccione el género como la clase del modelo y presione el botón Agregar.

![](mvc-music-store-part-3/_static/image17.png)

Primero, cambiaremos el @model declaración para indicar que la vista esperen género varios objetos en lugar de solo uno. Cambio de la primera línea del /Store/Index.cshtml para que quede como sigue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Esto indica que el motor de vistas de Razor que va a trabajar con un objeto de modelo que puede contener varios objetos de género. Estamos usando un objeto IEnumerable&lt;género&gt; en lugar de una lista&lt;género&gt; , ya que es más genérico, lo que nos permite cambiar más adelante nuestro tipo de modelo para cualquier tipo de objeto que admite la interfaz IEnumerable.

A continuación, podrá recorrer los objetos de género en el modelo tal como se muestra en el siguiente código de la vista completa.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Observe que tenemos compatibilidad completa con IntelliSense tal como se escriba este código, por lo que cuando se escribe "@Model." vemos que todos los métodos y las propiedades que admite un IEnumerable de tipo género.

![](mvc-music-store-part-3/_static/image18.png)

Dentro de nuestro bucle "foreach", Visual Web Developer sepa que cada elemento es de tipo género, por lo que vemos IntelliSence para cada tipo género.

![](mvc-music-store-part-3/_static/image19.png)

A continuación, la característica de scaffolding examina el objeto de género y determina que cada uno tendrá una propiedad de nombre, por lo que recorre en bucle y escribe. También genera vínculos de edición, detalles y eliminación a cada elemento individual. Deberá aprovechamos más adelante en el administrador del almacén, pero por ahora nos gustaría tener una lista simple en su lugar.

Cuando se ejecute la aplicación y vaya a/Store, vemos que se muestra el recuento y la lista de géneros.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Agregar vínculos entre páginas

La dirección URL/Store que enumera los géneros actualmente enumera los nombres de género simplemente como texto sin formato. Vamos a cambiar esto por lo que instead of plain text en su lugar, tenemos el vínculo de los nombres de género a la dirección URL de exploración/Store apropiada, por lo que al hacer clic en un género musical, como "Disco" se le remitirá a/Store/examinar? genre = dirección URL del Disco. Podríamos actualizar nuestra plantilla de vista \Views\Store\Index.cshtml al resultado de estos vínculos mediante código como a continuación **(no escriba lo siguiente en: vamos a mejorarlo)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Eso funciona, pero que podría dar lugar a problemas más adelante, ya que depende de una cadena estática. Por ejemplo, si quisiéramos cambiar el nombre del controlador, es necesario buscar a través de nuestro código busca vínculos que deben actualizarse.

Es un enfoque alternativo, que podemos usar para aprovechar las ventajas de un método de aplicación auxiliar HTML. ASP.NET MVC incluye métodos de aplicación auxiliar HTML que están disponibles desde nuestro código de plantilla de vista para realizar una serie de tareas comunes de esta manera. El método auxiliar Html.ActionLink() es especialmente útil y facilita la tarea de generar HTML &lt;un&gt; vincula y se encarga de molestos detalles como asegurándose de que las rutas de acceso de dirección URL están correctamente codificado como URL.

Html.ActionLink() tiene varias sobrecargas diferentes para permitir la especificación de toda la información que necesita para sus vínculos. En el caso más simple, deberá proporcionar el texto del vínculo y el método de acción al que desplazarse cuando se hace clic en el hipervínculo en el cliente. Por ejemplo, podemos vinculamos a "/ Store /" Index() método en la página de detalles de Store con el texto del vínculo "Ir a la Store Index" mediante la siguiente llamada:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: en este caso, no necesitamos especificar el nombre del controlador porque nos estamos simplemente vincula a otra acción dentro del mismo controlador que está representando la vista actual.*

Los vínculos a la página Examinar deberá pasar un parámetro, sin embargo, por lo que vamos a usar otra sobrecarga del método Html.ActionLink toma tres parámetros:

- 1. Texto del vínculo, que mostrará el nombre de género
- 2. Nombre de acción de controlador (Examinar)
- 3. Valores de parámetro de ruta, especificando el nombre (género) y el valor (nombre de género)

Poner todo junto, que le presentamos cómo vamos a escribir esos vínculos a la vista de índice Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Ahora, cuando se vuelva a ejecuta nuestro proyecto y tener acceso a la dirección URL de /Store/ veremos un listado con géneros. Cada género es un hipervínculo: cuando hace clic en nos llevará a nuestro/Store/examinar? genre =*[género]* dirección URL.

![](mvc-music-store-part-3/_static/image3.jpg)

El código HTML de la lista de género tiene este aspecto:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-2.md)
> [Siguiente](mvc-music-store-part-4.md)
