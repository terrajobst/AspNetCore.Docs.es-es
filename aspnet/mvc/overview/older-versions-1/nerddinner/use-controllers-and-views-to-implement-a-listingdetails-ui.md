---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores y vistas para implementar una interfaz de usuario de la lista/detalles | Microsoft Docs
author: microsoft
description: Paso 4 muestra cómo agregar un controlador a la aplicación que aprovecha las ventajas de nuestro modelo para proporcionar a los usuarios una experiencia de navegación de la lista/detalles de datos...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 203a12473f79f38f7162d360d2179ca7c4a30303
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837907"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores y vistas para implementar una interfaz de usuario de la lista/detalles
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 4 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 4 muestra cómo agregar un controlador a la aplicación que aprovecha las ventajas de nuestro modelo para proporcionar a los usuarios una experiencia de navegación de lista/detalles de datos para las cenas en nuestro sitio de NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner paso 4: Los controladores y vistas

Con marcos web tradicional (ASP clásico, PHP, ASP.NET Web Forms, etcetera), las direcciones URL entrantes suelen asignarse a los archivos en disco. Por ejemplo: una solicitud para una dirección URL como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".

Marcos basados en Web MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente. En lugar de asignar direcciones URL entrantes a los archivos, en su lugar asignar URL a métodos en clases. Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control de entrada del usuario, recuperar y guardar los datos y determinar la respuesta enviada al cliente (Mostrar HTML, descargar un archivo, redirigir a otro Dirección URL, etcetera).

Ahora que hemos creado un modelo básico para nuestra aplicación NerdDinner, el siguiente paso será agregar un controlador a la aplicación que se aprovecha para proporcionar a los usuarios una experiencia de navegación de lista/detalles de datos para las cenas en nuestro sitio.

### <a name="adding-a-dinnerscontroller-controller"></a>Agregar un controlador de DinnersController

Nos comenzar con el botón secundario en la carpeta "Controladores" dentro de nuestro proyecto web y, a continuación, seleccione el **Add -&gt;controlador** comando de menú (también puede ejecutar este comando escribiendo Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Se le asigne un nombre del nuevo controlador "DinnersController" y haga clic en el botón "Agregar". Visual Studio, a continuación, agregará un archivo DinnersController.cs bajo nuestro directorio \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

También se abrirá la nueva clase DinnersController dentro del editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Adición de Index() y Details() los métodos de acción a la clase DinnersController

Queremos permitir a que los visitantes con nuestra aplicación para examinar una lista de instancias dinners próximas y permitirles hacer clic en cualquier cena en la lista para ver los detalles específicos sobre él. Haremos esto mediante la publicación de las siguientes direcciones URL desde nuestra aplicación:

| **URL** | **Propósito** |
| --- | --- |
| */Dinners/* | Mostrar una lista HTML de próximas dinners |
| */ Dinners/detalles / [id]* | Mostrar los detalles sobre una cena específico indicado por un parámetro "id" incrustado dentro de la dirección URL: que coincidirá con el DinnerID de la cena en la base de datos. Por ejemplo: /Dinners/Details/2 mostraría una página HTML con detalles acerca de la cena cuyo valor DinnerID es 2. |

Implementaciones iniciales de estas direcciones URL se publicarán agregando dos público "métodos de acción" a nuestra clase DinnersController como a continuación:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

A continuación, se podrá ejecutar la aplicación NerdDinner y use nuestro explorador para invocarlos. Escribir en el *"/ Dinners /"* hará que la dirección URL de nuestro *Index()* método a ejecutar y lo enviará de vuelta la respuesta siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Escribir en el *"/ Dinners/detalles/2"* hará que la dirección URL de nuestro *Details()* método para ejecutar y volver a enviar la respuesta siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Tal vez se pregunte: ¿Cómo supo ASP.NET MVC para crear nuestra clase DinnersController e invocar dichos métodos? Comprender que vamos a echar un vistazo a cómo funciona el enrutamiento.

### <a name="understanding-aspnet-mvc-routing"></a>Understanding ASP.NET MVC enrutamiento

ASP.NET MVC incluye un eficaz motor de enrutamiento de dirección URL que proporciona mucha flexibilidad para controlar cómo se asignan las direcciones URL a las clases de controlador. Permite personalizar completamente cómo ASP.NET MVC elige qué clase de controlador para crear el método va a invocar en él, así como configurar maneras diferentes que las variables pueden se analiza desde la dirección URL o la cadena de consulta automáticamente y se pasa al método como parámetro argumentos. Ofrece la flexibilidad necesaria para totalmente optimizar un sitio para SEO (optimización de motor de búsqueda), así como publicar cualquier estructura de dirección URL que queremos desde una aplicación.

De forma predeterminada, los nuevos proyectos de ASP.NET MVC vienen con un conjunto preconfigurado de reglas de enrutamiento de direcciones URL ya está registrado. Esto nos permite empezar fácilmente en una aplicación sin tener que configurar explícitamente nada. Los registros de regla de enrutamiento predeterminada pueden encontrarse dentro de la clase "Application" de nuestros proyectos, que se pueden abrir haciendo doble clic en el archivo "Global.asax" en la raíz de nuestro proyecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Las reglas de enrutamiento de ASP.NET MVC predeterminada se registran en el método "RegisterRoutes" de esta clase:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Las rutas de". MapRoute() "llamada al método anterior registra una regla de enrutamiento predeterminado que asigna las direcciones URL entrantes a las clases de controlador con el formato de dirección URL:" / {controller} / {action} / {id} ", donde"controlador"es el nombre de la clase de controlador para crear una instancia,"action"es el nombre de un método público que se invocará en la base de datos e "id" es un parámetro opcional incrustado dentro de la dirección URL que puede pasarse como argumento al método. El tercer parámetro transferido a la llamada al método "MapRoute()" es un conjunto de valores predeterminados que se usará para los valores de controlador / / Id. de acción en caso de que no están presentes en la dirección URL (controlador = "Hogar" Action = "Index", Id = "").

A continuación es una tabla que muestra cómo una variedad de direcciones URL se asignan mediante el valor predeterminado "<em>/ {controladores} / {action} / {id}"</em>regla de ruta:

| **URL** | **Clase de controlador** | **Método de acción** | **Parámetros pasados** |
| --- | --- | --- | --- |
| */ Dinners/detalles/2* | DinnersController | Details(ID) | Id. = 2 |
| */ Dinners/Edit/5* | DinnersController | Edit(ID) | Id. = 5 |
| */ Dinners/crear* | DinnersController | Create() | N/D |
| */ Dinners* | DinnersController | Index() | N/D |
| */ Home* | HomeController | Index() | N/D |
| */* | HomeController | Index() | N/D |

Las tres últimas filas muestran los valores predeterminados (controlador = Home, acción = índice, Id = "") que se va a usar. Dado que el método "Index" está registrado como el nombre de acción predeterminada si no se especifica uno, el "/ Dinners" y "/ Home" causa de las direcciones URL del método de acción Index() que se invocará en sus clases de controlador. Dado que el controlador "Home" está registrado como el controlador predeterminado si no se especifica uno, la dirección URL "/" hace HomeController crearse y el método de acción Index() en que se invoque.

Si no le gusta estas reglas de enrutamiento de dirección URL predeterminada, la buena noticia es que son fáciles de cambiar: solo editarlos dentro del método RegisterRoutes anterior. Para nuestra aplicación NerdDinner, sin embargo, no vamos a cambiar cualquiera de las reglas de enrutamiento de dirección URL predeterminada, en lugar de ello, utilizaremos como-es.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Uso de la instancia DinnerRepository desde nuestro DinnersController

Vamos a reemplazar ahora nuestra implementación actual de la DinnersController Index() y Details() métodos de acción con las implementaciones que usan nuestro modelo.

Vamos a usar la clase de instancia DinnerRepository que hemos creado anteriormente para implementar el comportamiento. Estudiaremos comience agregando una instrucción "using" que hace referencia el espacio de nombres "NerdDinner.Models" y, a continuación, declare una instancia de nuestra instancia DinnerRepository como un campo en nuestra clase DinnerController.

Más adelante en este capítulo se presentaré el concepto de "Inserción de dependencias" y mostrar otra manera de los controladores obtener una referencia a una instancia DinnerRepository que permite una mejor unidad pruebas – pero derecho ahora simplemente vamos a crear una instancia de nuestra instancia DinnerRepository en línea como a continuación.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Ahora estamos preparados generar una respuesta HTML con nuestros objetos del modelo de datos recuperados.

### <a name="using-views-with-our-controller"></a>Utilizar vistas con nuestro controlador

Aunque es posible escribir código en nuestros métodos de acción para ensamblar HTML y, a continuación, utilice el *Response.Write ()* método auxiliar para enviar al cliente, que enfoque pasa a ser bastante difícil rápidamente. Un enfoque mucho mejor es para que podamos realizar solo aplicación y la lógica de datos dentro de nuestros métodos de acción de DinnersController y, a continuación, pasar los datos necesarios para presentar una respuesta HTML a una plantilla de vista"independiente" que es responsable de generar la representación de HTML del mismo. Como veremos en un momento, una plantilla de "ver" es un archivo de texto que normalmente contiene una combinación de marcado HTML y código de representación incrustado.

Separar la lógica del controlador de la representación de la vista aporta varias ventajas grandes. En particular ayuda a exigir una "separación de preocupaciones" clara entre el código de aplicación y el código de formato de representación/interfaz de usuario. Esto facilita enormemente en la lógica de aplicación de prueba unitaria de forma aislada de la lógica de procesamiento de la interfaz de usuario. Facilita más tarde modificar las plantillas de representación de la interfaz de usuario sin tener que realizar cambios de código de aplicación. Y puede que resulte más fácil para los desarrolladores y diseñadores colaborar en proyectos.

Podemos actualizar nuestra clase DinnersController para indicar que queremos usar una plantilla de vista para volver a enviar una respuesta de IU de HTML cambiando las firmas de método de nuestros métodos de acción de dos de tener un tipo de valor devuelto de "void" en su lugar, tener un tipo de valor devuelto de "ActionResult". A continuación, podemos llamar a la *View()* método auxiliar en la clase base del controlador para devolver un objeto "ViewResult" similar a la siguiente:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma de la *View()* método auxiliar, usamos anteriormente como el siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

El primer parámetro para el *View()* método auxiliar es el nombre del archivo de plantilla de vista que desea usar para representar la respuesta HTML. El segundo parámetro es un objeto de modelo que contiene los datos que necesita la plantilla de vista para representar la respuesta HTML.

Dentro de nuestro método de acción Index() estamos llamando a la *View()* método auxiliar y que indica que van a representar un listado de HTML de instancias dinners mediante una plantilla de vista "Index". La plantilla de vista que estamos pasando una secuencia de objetos de la cena para generar la lista de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dentro de nuestro método de acción Details() se intenta recuperar un objeto de la cena con el identificador proporcionado en la dirección URL. Si se encuentra una cena válida que llamamos el *View()* método auxiliar, que indica que deseamos utilizar una plantilla de vista "Detalles" para representar el objeto recuperado de la cena. Si se solicita una cena no válido, se representará un mensaje de error útiles que indica que la cena no existe con una plantilla de vista de "NotFound" (y una versión sobrecargada de la *View()* método auxiliar que simplemente toma el nombre de plantilla ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Implementemos ahora las plantillas de vista de "NotFound", "Detalles" y "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementar la plantilla de vista de "NotFound"

Comenzaremos por implementar la plantilla de vista de "NotFound", que muestra un mensaje de error descriptivo que indica que no se encuentra la cena solicitada.

Crearemos una nueva plantilla de vista colocando el cursor de texto dentro de un método de acción de controlador y, a continuación, haga clic en y elija el comando de menú "Agregar vista" (también podemos ejecutar este comando escribiendo Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Se abrirá un cuadro de diálogo "Agregar vista", como a continuación. De forma predeterminada, que el cuadro de diálogo rellenará automáticamente el nombre de la vista para crear para que coincida con el nombre del método de acción el cursor se encontraba cuando se inicia el cuadro de diálogo (en este caso, "Detalles"). Puesto que deseamos implementar primero la plantilla de "NotFound", se deberá invalidar este nombre de la vista y establecerlo en su lugar "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista de "NotFound.aspx" para nosotros dentro del directorio "\Views\Dinners" (que también se creará si aún no existe el directorio):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

También abrirá nuestra nueva plantilla de vista "NotFound.aspx" en el editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Plantillas de vista de forma predeterminada tienen dos "áreas de contenido" que podemos agregar contenido y el código. La primera nos permite personalizar la "title" de la página HTML enviado de vuelta. El segundo nos permite personalizar el "contenido principal" de la página HTML enviado de vuelta.

Para implementar la plantilla de vista de "NotFound" vamos a agregar algún contenido básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Se puede, a continuación, pruebe el código dentro del explorador. Para hacer esto, vamos a petición la *"/ Dinners/detalles/9999"* dirección URL. Esto hará referencia a una cena que actualmente no existe en la base de datos y hará que el método de acción DinnersController.Details() representar la plantilla de vista de "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Una cosa que observará en la captura de pantalla anterior es que la plantilla de vista básica ha heredado de un montón de HTML que rodea el contenido en la pantalla principal. Esto es porque la plantilla de vista está usando una plantilla de "página principal" que nos permite aplicar un diseño coherente en todas las vistas en el sitio. Trataremos cómo funcionan en una parte posterior de este tutorial más las páginas maestras.

### <a name="implementing-the-details-view-template"></a>Implementar la plantilla de vista "Detalles"

Implementemos ahora la plantilla de vista "Detalles": lo que generará HTML para un modelo único de la cena.

Se deberá hacer esto colocando el cursor de texto dentro del método de acción de detalles y, a continuación, haga clic en y elija el comando de menú "Agregar vista" (o presione Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Se abrirá el cuadro de diálogo "Agregar vista". Mantendremos el nombre de vista predeterminado ("Detalles"). También la casilla "Crear una vista fuertemente tipada" en el cuadro de diálogo y seleccionar (mediante la lista desplegable de combobox) el nombre del tipo de modelo que estamos pasando desde el controlador a la vista. Para esta vista que estamos pasando un objeto de la cena (el nombre completo de este tipo es: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A diferencia de la plantilla anterior, donde se decidió crear una "Vista vacía", esta vez que aquí se elegirá automáticamente "aplicar la técnica scaffolding" la vista mediante una plantilla de "Detalles". Podemos indicar esto cambiando la lista desplegable "Ver el contenido" en el cuadro de diálogo anterior.

"Scaffolding" generará una implementación inicial de la plantilla de vista de detalles basándose en el objeto Dinner que estamos pasando a él. Esto proporciona una manera fácil para que podamos empezar a trabajar rápidamente en nuestra implementación de plantilla de vista.

Cuando hacemos clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de vista "Details.aspx" para nosotros en nuestro directorio "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

También se abrirá nuestra nueva plantilla de vista "Details.aspx" en el editor de código. Contendrá una implementación de scaffold inicial de una vista de detalles basada en un modelo de la cena. El motor de scaffolding usa la reflexión de .NET para ver las propiedades públicas expuestas en la clase que lo ha pasado y agregará el contenido adecuado en función de cada tipo que se encuentra:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Podemos solicitamos la *"/ Dinners/detalles/1"* dirección URL para ver el aspecto de esta implementación de scaffold "Detalles" en el explorador. Utilizando esta dirección URL, se mostrará una de las instancias dinners que se agregan manualmente a nuestra base de datos cuando se crea por primera vez:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Esto obtiene nos en marcha rápidamente y nos ofrece una implementación inicial de nuestra vista Details.aspx. A continuación, podemos ir y ajustarlo para personalizar la interfaz de usuario para satisfacción nuestra.

Cuando se examina más detenidamente en la plantilla Details.aspx, buscaremos que lo contiene HTML estático, así como código de representación incrustado. &lt;%%&gt; fragmentos de código insertados ejecutan código cuando se representa la plantilla de vista, y &lt;% = %&gt; fragmentos de código insertados ejecute el código contenido en ellos y, a continuación, representar el resultado al flujo de salida de la plantilla.

Podemos escribir código dentro de nuestra vista que tiene acceso al objeto de modelo de "Cena" que se ha pasado desde nuestro controlador mediante una propiedad de "Modelo" fuertemente tipado. Visual Studio nos proporciona intellisense de código completo al obtener acceso a esta propiedad "Model" dentro del editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos a hacer algunos ajustes para que el origen de nuestra plantilla de vista de detalles final se parece a esta:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Cuando tenemos acceso a la *"/ Dinners/detalles/1"* URL nuevo se va a representar como el siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementar la plantilla de vista "Index"

Implementemos ahora la plantilla de vista "Index": lo que generará una lista de instancias Dinners próximas. Tareas pendientes, esta se coloque el cursor de texto dentro del método de acción del índice y a la derecha, a continuación, haga clic en y elija el comando de menú "Agregar vista" (o presione Ctrl-M, Ctrl-V).

En el cuadro de diálogo "Agregar vista" se mantendrán la plantilla de vista denominada "Índice" y seleccione la casilla de verificación "Crear una vista fuertemente tipada". Este tiempo se elegirá automáticamente generar una plantilla de vista "List" y seleccione "NerdDinner.Models.Dinner" como el tipo de modelo, pasado a la vista (que ya hayamos indicado vamos a crear una "lista" scaffold hará que el cuadro de diálogo Agregar vista se supone que se están pasar una secuencia de objetos de la cena de nuestro controlador a la vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Cuando hacemos clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de vista "Index.aspx" para nosotros en nuestro directorio "\Views\Dinners". Aplicará "la técnica scaffolding" una implementación inicial dentro de él que proporciona una lista de tabla HTML de las instancias Dinners pasamos a la vista.

Cuando se ejecutan la aplicación y el acceso a la *"/ Dinners /"* representará nuestra lista de instancias dinners de dirección URL de este modo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solución de la tabla anterior nos da un diseño de cuadrícula de nuestros datos cena, que no es bastante lo que queremos para nuestro consumidor orientado a la lista de la cena. Podemos actualizar la plantilla de vista Index.aspx y modificarlo para mostrar menos columnas de datos y utilizar un &lt;ul&gt; elemento para representarlos en lugar de una tabla con el código siguiente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Usamos la palabra clave de "var" dentro de la instrucción foreach anterior se recorrer cada cena en nuestro modelo. Aquellos familiarizados con C# 3.0 podrían pensar que usar "var" significa que el objeto de la cena es en tiempo de ejecución. En su lugar, significa que el compilador está utilizando la inferencia de tipos con respecto a la propiedad fuertemente tipada "Model" (que es de tipo "IEnumerable&lt;Dinner&gt;") y compilar la variable local "cena" como un tipo de la cena: lo que significa que obtenemos completas IntelliSense y buscando dentro de bloques de código de tiempo de compilación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Cuando se actualice la vista el */dinners* dirección URL de nuestro explorador nuestra vista actualizada ahora parece a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Esto está buscando una mejor – pero todavía no está completamente allí. El último paso es habilitar a los usuarios finales clic Dinners individuales en la lista y ver los detalles sobre ellos. Se implementará con la representación de elementos de hipervínculo HTML que vincula al método de acción de detalles en nuestra DinnersController.

Podemos generar estas hipervínculos dentro de nuestra vista de índice en una de dos maneras. La primera consiste en crear manualmente HTML &lt;un&gt; elementos como el siguiente, donde incrustamos &lt;%%&gt; se bloquea dentro la &lt;un&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Es un enfoque alternativo, podemos usar para aprovechar el método de aplicación auxiliar "Html.ActionLink()" integrado en ASP.NET MVC que admite la creación mediante programación un elemento HTML &lt;un&gt; elemento que se vincula a otro método de acción en un Controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

El primer parámetro al método auxiliar Html.ActionLink() es el texto del vínculo para mostrar (en este caso el título de la cena), el segundo parámetro es el nombre de acción de controlador que queremos generar el vínculo para (en este caso, el método detalles), y el tercer parámetro es un conjunto de parámetros para enviar a la acción (implementada como un tipo anónimo con valores y nombres de propiedad). En este caso vamos a especificar el parámetro "id" de la cena se desea establecer un vínculo y, debido a la dirección URL de enrutamiento predeterminado de reglas en ASP.NET MVC es "{Controller} / {Action} / {id}" el método auxiliar Html.ActionLink() generará el siguiente resultado:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para nuestra vista Index.aspx se utilizará el enfoque de método de aplicación auxiliar de Html.ActionLink() y tener cada cena en el vínculo de la lista a la dirección URL de los detalles adecuados:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Y ahora cuando se presione el */dinners* URL nuestra lista cena como el siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Cuando hacemos clic en cualquiera de las instancias Dinners en la lista navegaremos para ver detalles acerca de él:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Basado en convenciones de nomenclatura y la estructura de directorios \Views

Las aplicaciones de ASP.NET MVC predeterminada usan un directorio basado en convenciones de nomenclatura estructura al resolver las plantillas de vista. Esto permite a los desarrolladores evitar tener que completar una ruta de acceso al hacer referencia a vistas desde dentro de una clase de controlador. De forma predeterminada ASP.NET MVC buscará el archivo de plantilla de vista dentro de la * \Views\[Nombredelcontrolador]\* directorio debajo de la aplicación.

Por ejemplo, hemos trabajado en la clase DinnersController: lo que hace referencia explícitamente a tres plantillas de vista: "Index", "Detalles" y "NotFound". ASP.NET MVC predeterminada buscará estas vistas dentro de la *\Views\Dinners* directorio bajo el directorio raíz de la aplicación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Tenga en cuenta anteriormente cómo hay actualmente son tres clases de controlador en el proyecto (DinnersController, HomeController y AccountController: se agregaron los dos últimos de forma predeterminada cuando se crea el proyecto), y hay tres de los subdirectorios (uno para cada controlador) dentro del directorio \Views.

Las vistas que se hace referencia desde los controladores de inicio y las cuentas resolverán automáticamente sus plantillas de vista de los respectivos *\Views\Home* y *\Views\Account* directorios. El *\Views\Shared* subdirectorio proporciona una manera de almacenar las plantillas de vista que se reutilizan entre varios controladores dentro de la aplicación. Cuando intente resolver una plantilla de vista ASP.NET MVC, comprobará primero dentro de la *\Views\[controlador]* directorio específico, y si no encuentra la plantilla de vista se buscará en el *\Views\ Compartido* directory.

Cuando se trata de nomenclatura de plantillas de vista individuales, la recomendación es que la plantilla de vista comparten el mismo nombre que el método de acción que ha provocado que representar. Por ejemplo, por encima de nuestro índice"" método de acción es mediante la vista de "Índice" para representar el resultado de la vista y el método de acción "Detalles" está usando la vista "Detalles" para representar sus resultados. Esto facilita ver rápidamente qué plantilla se asocia con cada acción.

Los desarrolladores no es necesario especificar explícitamente el nombre de la plantilla de vista cuando la plantilla de vista tiene el mismo nombre que el método de acción que se invoca en el controlador. En su lugar, simplemente para pasar el objeto de modelo al método auxiliar "View()" (sin especificar el nombre de vista) y ASP.NET MVC deducirá automáticamente que deseamos utilizar el *\Views\[Nombredelcontrolador]\[ActionName]* plantilla de vista en el disco para representarlo.

Esto nos permite limpiar un poco nuestro código de controlador y evitar la duplicación de nombre dos veces en nuestro código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

El código anterior es todo lo que es necesario para implementar una buena cena lista/detalles experiencia para el sitio.

#### <a name="next-step"></a>Paso siguiente

Ahora tenemos una buena cena experiencia integrada de exploración.

Vamos a ahora habilitar la compatibilidad de edición de formularios de datos CRUD (creación, lectura, actualización, eliminación).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Siguiente](provide-crud-create-read-update-delete-data-form-entry-support.md)
