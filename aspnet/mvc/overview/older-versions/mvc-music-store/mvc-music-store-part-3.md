---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Vistas y ViewModels | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 3 cubre vistas y ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Vistas y ViewModels
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 3 cubre vistas y ViewModels.


Hasta ahora nos hemos solo ha devolver cadenas de acciones del controlador. Que es una buena forma de obtener una idea de cómo funcionan los controladores, pero es no cómo se desea compilar una aplicación web real. Vamos a desea una manera mejor de generar HTML a exploradores visitar nuestro sitio: uno que podemos usar archivos de plantilla para personalizar el contenido HTML con mayor facilidad enviar de vuelta. Es exactamente lo que las vistas.

## <a name="adding-a-view-template"></a>Agregar una plantilla de vista

Para usar una plantilla de vista, se podrá cambiar el método de índice HomeController para devolver un ActionResult y hacer que devuelva View(), al igual que a continuación:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

El cambio anterior indica que, en lugar de devolver una cadena y, a continuación, en su lugar, queremos usar una "vista" para generar un resultado.

Ahora vamos a agregar una plantilla de vista adecuada para el proyecto. Para ello se deberá coloque el cursor de texto dentro del método de acción de índice, a continuación, haga clic en y seleccione "Agregar vista". Se abrirá el cuadro de diálogo Agregar vista:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

El cuadro de diálogo "Agregar vista" nos permite rápida y fácilmente generar archivos de plantilla de la vista. De forma predeterminada, "Agregar vista" cuadro de diálogo rellena previamente el nombre de la plantilla de vista para crear para que coincida con el método de acción que va a usar. Dado que se usa el menú contextual de "Agregar vista" dentro del método de acción de Index() de nuestro HomeController, el cuadro de diálogo "Agregar vista" anterior tiene "Index" como el nombre de vista que previa se rellena de forma predeterminada. No es necesario cambiar cualquiera de las opciones de este cuadro de diálogo, haga clic en el botón Agregar.

Cuando se hace clic en el botón Agregar, Visual Web Developer creará una Index.cshtml nueva plantilla de vista para que podamos en el directorio \Views\Home, creando la carpeta si aún no existe.

![](mvc-music-store-part-3/_static/image2.png)

El nombre y una ubicación del archivo "Index.cshtml" es importante y sigue las convenciones de nomenclatura de MVC de ASP.NET de forma predeterminada. El nombre del directorio, \Views\Home, coincide con el controlador - que se denomina HomeController. El nombre de la plantilla de vista, índice, coincide con el método de acción de controlador que se va a mostrar la vista.

ASP.NET MVC permite evitar tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se utiliza esta convención de nomenclatura para devolver una vista. De forma predeterminada representará la plantilla de vista de \Views\Home\Index.cshtml cuando se escribe código similar al siguiente en nuestro HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer crea y abre la plantilla de la vista "Index.cshtml" después de que se hace clic en el botón "Agregar" en el cuadro de diálogo "Agregar vista". El contenido de Index.cshtml se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Esta vista está usando la sintaxis Razor, que es más concisa que el motor de vista de formularios Web Forms usado en formularios Web Forms de ASP.NET y las versiones anteriores de ASP.NET MVC. El motor de vistas de formularios Web Forms sigue estando disponible en ASP.NET MVC 3, pero muchos desarrolladores consideran que el motor de vista Razor ajuste realmente bien a desarrollo de ASP.NET MVC.

Las tres primeras líneas establecer el título de página con ViewBag.Title. Consultamos cómo funciona esto con más detalle más rápido, pero primero vamos a actualizar el texto del título de texto y ver la página. Actualización de la &lt;h2&gt; etiqueta que diga "Esta es la página principal", tal y como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Ejecuta la aplicación se muestra que nuestro nuevo texto esté visible en la página principal.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Utilizar un diseño para los elementos comunes del sitio

La mayoría de sitios Web tiene contenido que se comparte entre varias páginas: navegación, pies de página, imágenes de logotipo, las referencias de la hoja de estilos, etcetera. El motor de vista Razor facilita esta tarea administrar mediante una página denominada \_Layout.cshtml que se ha creado automáticamente para que podamos dentro de la carpeta Shared/vistas /.

![](mvc-music-store-part-3/_static/image4.png)

Haga doble clic en esta carpeta para ver el contenido, que se muestran a continuación.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Se mostrará el contenido de nuestro vistas individuales de la @RenderBodycomando () y cualquier contenido común que se va a aparecer fuera de la que pueden agregarse a la \_Layout.cshtml marcado. Le deseamos que nuestra tienda de música de MVC tenga un encabezado común con vínculos a nuestra área de página principal y el almacén en todas las páginas en el sitio, por lo que vamos a agregar a la plantilla directamente encima que @RenderBodyinstrucción ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Actualizar la hoja de estilos

La plantilla proyecto vacío de servidor incluye un archivo CSS muy simplificado que solo incluye estilos utilizados para mostrar mensajes de validación. El diseñador ha proporcionado algunos adicionales CSS e imágenes para definir la apariencia y funcionamiento para nuestro sitio, por lo que vamos a agregarlos ahora.

El archivo CSS y las imágenes actualizadas se incluyen en el directorio de contenido de Assets.zip MvcMusicStore que está disponible en [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store). Se podrá seleccionar ambos en el Explorador de Windows y colóquelos en la carpeta de contenido de nuestra solución en Visual Web Developer, tal y como se muestra a continuación:

![](mvc-music-store-part-3/_static/image5.png)

Se le pedirá que confirme si desea sobrescribir el archivo Site.css existente. Haga clic en Sí.

![](mvc-music-store-part-3/_static/image6.png)

La carpeta de contenido de la aplicación aparecerá ahora como sigue:

![](mvc-music-store-part-3/_static/image7.png)

Ahora vamos a ejecutar la aplicación y ver el aspecto de los cambios en la página principal.

![](mvc-music-store-part-3/_static/image8.png)

- Revisemos lo que ha cambiado: método de acción del HomeController el índice se encuentra y muestra la plantilla \Views\Home\Index.cshtmlView, aunque el código denominado "View() devuelto", porque la plantilla de vista de seguir la convención de nomenclatura estándar.
- La página de inicio muestra un mensaje de bienvenida simple que se define en la plantilla de vista \Views\Home\Index.cshtml.
- Está usando la página principal de nuestro \_Layout.cshtml plantilla, de modo que el mensaje de bienvenida se encuentra en el diseño de sitio estándar HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usar un modelo para pasar información a la vista

Una plantilla de vista que muestra codificado de forma rígida HTML no va a hacer que un sitio web muy interesante. Para crear un sitio web dinámico, se en su lugar conveniente pasar información de las acciones del controlador a nuestras plantillas de vista.

En el modelo Model-View-Controller, el término A que modelo hace referencia objetos que representan los datos de la aplicación. A menudo, los objetos del modelo corresponden a tablas de la base de datos, pero no tienen por qué.

Métodos de acción de controlador que devuelven un ActionResult pueden pasar un objeto de modelo a la vista. Esto permite que un controlador de paquete correctamente toda la información necesaria para generar una respuesta y, a continuación, pasar esta información a una plantilla de la vista utilizada para generar la respuesta HTML adecuada. Esto es más fácil comprender al verlo en acción, así que vamos a empezar a trabajar.

Primero vamos a crear algunas clases de modelo para representar géneros y álbumes en nuestra tienda. Empecemos creando una clase de género. Haga clic en la carpeta "Models" dentro del proyecto, elija la opción "Agregar clase" y "Genre.cs" un nombre al archivo.

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

A continuación, agregue una propiedad de nombre de cadena pública a la clase que se creó:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: en caso de que se lo pregunte, el {get; estableció;} está realizando la notación de uso de la característica de propiedades autoimplementadas de C#. Esto nos proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.*

A continuación, siga los mismos pasos para crear una clase de álbum (denominada Album.cs) que tiene un título y una propiedad de género:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Ahora se podrá modificar el StoreController para utilizar vistas que muestran la información dinámica de nuestro modelo. Si - para fines demostrativos - ahora se denominan nuestro álbumes basadas en el identificador de solicitud, se puede mostrar esa información como en la vista siguiente.

![](mvc-music-store-part-3/_static/image10.png)

Comenzaremos cambiando la acción de detalles del almacén que se presenta la información de un sólo álbum. Agregue una instrucción "using" en la parte superior de la **StoreControllers** clase para que incluya el espacio de nombres MvcMusicStore.Models, por lo que no es necesario escribir MvcMusicStore.Models.Album cada vez que deseamos utilizar la clase de álbum. La sección "instrucciones using" de esa clase debería aparecer ahora las indicadas a continuación.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

A continuación, actualizaremos la acción de controlador de detalles para que devuelva un ActionResult en lugar de una cadena, como hicimos en método de índice del HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Ahora se podrá modificar la lógica para devolver un objeto de álbum a la vista. Más adelante en este tutorial se recuperará los datos de una base de datos, pero por ahora vamos a utilizar "ficticio datos" para empezar a trabajar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: Si no está familiarizado con C#, puede suponer que utilizando var significa que la variable de álbum está enlazada a un tiempo de ejecución. No es correcto, el compilador de C# es utilizando la inferencia de tipos en función de lo que estamos asignar a la variable para determinar el que álbum es de tipo álbum y compilar la variable local álbum como tipo de álbum, por lo que tenemos comprobación en tiempo de compilación y el editor de código de Visual Studio soporte técnico.*

Vamos a crear ahora una plantilla de vista que usa nuestro álbum para generar una respuesta HTML. Antes de hacer se necesita compilar el proyecto para que el cuadro de diálogo Agregar vista sepa sobre nuestra clase álbum recién creado. Puede generar el proyecto seleccionando el Debug⇨Build MvcMusicStore elemento de menú (para crédito adicional, puede utilizar el método abreviado Ctrl-Mayús + B para compilar el proyecto).

![](mvc-music-store-part-3/_static/image11.png)

Ahora que hemos configurado nuestras clases auxiliares, estamos listos crear la plantilla de vista. Pulse el botón derecho dentro del método de detalles y seleccione "Agregar vista..." en el menú contextual.

![](mvc-music-store-part-3/_static/image12.png)

Vamos a crear una nueva plantilla de vista al igual que hicimos antes con HomeController. Dado que vamos a crear desde el StoreController de forma predeterminada se generarán en un archivo \Views\Store\Index.cshtml.

A diferencia de antes, vamos a Active la casilla de la vista "Fuertemente tipada de crear". A continuación, vamos a seleccionar nuestra clase "Álbum" dentro de la colocación de "Datos de vista de clase"-downlist. Esto hará que el cuadro de diálogo "Agregar vista" crear una plantilla de vista que se espera que un álbum objeto se pasará a la que se va a usar.

![](mvc-music-store-part-3/_static/image13.png)

Cuando se hace clic en el botón "Agregar" se creará la plantilla de vista de \Views\Store\Details.cshtml, que contiene el código siguiente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Observe la primera línea, que indica que esta vista está fuertemente tipado en nuestra clase álbum. El motor de vista Razor es consciente de que se ha pasado un objeto de álbum, por lo que podemos acceder fácilmente a las propiedades del modelo e incluso tiene la ventaja de IntelliSense en el editor de Visual Web Developer.

Actualización de la &lt;h2&gt; etiquetar para que muestre la propiedad de título del álbum mediante la modificación de esa línea para que aparezca como sigue.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Tenga en cuenta que IntelliSense se desencadena cuando se escribe el período tras el @Model (palabra clave), que muestra las propiedades y métodos que admite la clase de álbum.

Ahora vamos a volver a ejecutar el proyecto y visita la dirección URL de almacén/detalles/5. Veremos los detalles de un álbum como a continuación.

![](mvc-music-store-part-3/_static/image14.png)

Ahora nos aseguraremos de hacer una actualización similar para el método de acción Examinar del almacén. El método de actualización para que devuelva un ActionResult y modificar la lógica del método, por lo que crea un nuevo objeto de género y lo devuelve a la vista.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Pulse el botón derecho en el método de examinar y seleccione "Agregar vista..." en el menú contextual y, a continuación, agregar una vista fuertemente tipada agregue fuertemente tipados a la clase de género.

![](mvc-music-store-part-3/_static/image15.png)

Actualización de la &lt;h2&gt; elemento en la vista de código (en /Views/Store/Browse.cshtml) para mostrar la información de género.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

¿Ahora vamos a volver a ejecutar el proyecto y vaya a/almacén/examinar? Género = Disco URL. Veremos la página Examinar muestran del siguiente modo a continuación.

![](mvc-music-store-part-3/_static/image16.png)

Por último, vamos a hacer una actualización ligeramente más compleja para la **almacenar índice** método de acción y la vista para mostrar una lista de todos los géneros en nuestra tienda. Deberá hacerlo mediante una lista de géneros como nuestro objeto de modelo, en lugar de simplemente un género único.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Pulse el botón derecho en el método de acción del índice de almacén y seleccione Agregar vista como antes, seleccione género como la clase del modelo y presione el botón Agregar.

![](mvc-music-store-part-3/_static/image17.png)

En primer lugar, modificaremos la @model declaración para indicar que la vista esperen género varios objetos en lugar de solo uno. Cambio de la primera línea de /Store/Index.cshtml para que quede como sigue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Esto indica que el motor de vista Razor que va a trabajar con un objeto de modelo que puede contener varios objetos de género. Estamos usando un objeto IEnumerable&lt;género&gt; en lugar de una lista&lt;género&gt; puesto que es más genérico, lo que nos permite cambiar el tipo de modelo más adelante a cualquier tipo de objeto que admita la interfaz IEnumerable.

A continuación, podrá recorrer los objetos de género en el modelo tal como se muestra en el siguiente código de vista completada.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Observe que tenemos que totalmente compatible con IntelliSense cuando se escribe este código, por lo que cuando se escribe "@Model." se ve todos los métodos y propiedades admitidas por un objeto IEnumerable de tipo género.

![](mvc-music-store-part-3/_static/image18.png)

Dentro de nuestro bucle "foreach", Visual Web Developer sabe que cada elemento es de tipo género, por lo que vemos IntelliSence para cada tipo de género.

![](mvc-music-store-part-3/_static/image19.png)

A continuación, la característica de scaffolding examina el objeto de género y determina que cada uno tendrá una propiedad Name, por lo que recorre en bucle y los escriba. También genera vínculos de edición, detalles y Delete para cada elemento individual. Se podrá sacar provecho de esto más adelante en el Administrador de almacén, pero por ahora nos gustaría tener una lista simple en su lugar.

Cuando se ejecute la aplicación y busque/Store, vemos que se muestra el número y la lista de géneros.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Agregar vínculos entre páginas

Nuestra dirección URL/Store que enumera géneros actualmente enumera los nombres de género simplemente como texto sin formato. Vamos a cambiar esto para que en lugar de texto sin formato en su lugar, tenemos el vínculo de nombres de género a la dirección URL apropiada o explorar un almacén, por lo que al hacer clic en un género musical como "Disco", se le remitirá a/almacén/examinar? género = URL de Disco. Podríamos actualizar la plantilla de vista de \Views\Store\Index.cshtml al resultado de estos vínculos mediante código como a continuación **(no escriba lo siguiente en: vamos a mejorar en ella)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Eso funciona, pero podría provocar a resolver los problemas más adelante desde que se basa en una cadena codificada. Por ejemplo, si desea cambiar el nombre del controlador, es necesario revisar el código de la búsqueda de los vínculos que deben actualizarse.

Un enfoque alternativo, podemos utilizar es aprovechar las ventajas de un método de aplicación auxiliar HTML. ASP.NET MVC incluye métodos de aplicación auxiliar HTML que están disponibles en el código de plantilla de vista para efectuar una serie de tareas comunes como esta. El método de aplicación auxiliar de Html.ActionLink() es especialmente útil y resulta fácil crear HTML &lt;un&gt; vincula y se encarga de molestos detalles como asegurándose de que las rutas de acceso de dirección URL están correctamente la dirección URL codificada.

Html.ActionLink() tiene varias sobrecargas diferentes para permitir la especificación tanta información como necesite para sus vínculos. En el caso más simple, deberá proporcionar el texto del vínculo y el método de acción al que desplazarse cuando se hace clic en el hipervínculo en el cliente. Por ejemplo, podemos vinculamos a "/ almacén /" Index() método en la página de detalles del almacén con el texto del vínculo "Ir para el índice de almacén de" utilizando la siguiente llamada:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: en este caso, no necesitamos especificar el nombre del controlador porque nos estamos simplemente vincular a otra acción en el mismo controlador que se está procesando la vista actual.*

Los vínculos a la página Examinar deberá pasar un parámetro, sin embargo, por lo que vamos a usar otra sobrecarga del método Html.ActionLink que toma tres parámetros:

- 1. Texto del vínculo, que mostrará el nombre de género
- 2. Nombre de acción de controlador (Examinar)
- 3. Valores de parámetro de ruta, especificando el nombre (género) y el valor (nombre de género)

Colocar todo en conjunto, que le presentamos cómo escribiremos esos vínculos a la vista de índice de almacén:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Ahora cuando se vuelva a ejecuta el proyecto y tener acceso a la dirección URL /Store/ se mostrará una lista de géneros. Cada género es un hipervínculo: al hacer clic en nos llevará a nuestro/almacén/examinar? género =*[género]* dirección URL.

![](mvc-music-store-part-3/_static/image3.jpg)

El código HTML de la lista de género tiene el siguiente aspecto:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-2.md)
> [Siguiente](mvc-music-store-part-4.md)
