---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores y vistas para implementar una interfaz de usuario de la lista y detalles | Documentos de Microsoft
author: microsoft
description: "Paso 4 muestra cómo agregar un controlador a la aplicación que aprovecha las ventajas de nuestro modelo para proporcionar a los usuarios una experiencia de exploración de datos/detalles del anuncio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 2f9148a2d419863229e2c5a2a0c98984001fcee5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores y vistas para implementar una interfaz de usuario de la lista y detalles
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 4 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 4 muestra cómo agregar un controlador a la aplicación que aprovecha las ventajas de nuestro modelo para proporcionar a los usuarios una experiencia de navegación de lista y detalles de datos para cenas en nuestro sitio NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner paso 4: Los controladores y vistas

Con marcos de trabajo web tradicionales (ASP clásico, PHP, formularios Web Forms de ASP.NET, etcetera), las direcciones URL entrantes se asignan normalmente a archivos en disco. Por ejemplo: una solicitud para una dirección URL, como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".

Marcos de trabajo basado en Web de MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente. En lugar de asignar direcciones URL entrantes a los archivos, se asignan las direcciones URL en su lugar a métodos en clases. Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control proporcionados por el usuario, recuperar y guardar los datos y determinar la respuesta para enviar al cliente (Mostrar HTML, descargar un archivo, redirija a otra Dirección URL, etcetera).

Ahora que hemos creado un modelo básico para nuestra aplicación NerdDinner, el siguiente paso será agregar un controlador a la aplicación que aprovecha las ventajas del mismo para proporcionar a los usuarios una experiencia de navegación de lista y detalles de datos para cenas en nuestro sitio.

### <a name="adding-a-dinnerscontroller-controller"></a>Cómo agregar un controlador de DinnersController

Se podrá comenzar con el botón secundario en la carpeta "Controladores" dentro de nuestro proyecto web y, a continuación, seleccione la **Add -&gt;controlador** comando de menú (también puede ejecutar este comando escribiendo Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Se le asigne un nombre del nuevo controlador de "DinnersController" y haga clic en el botón "Agregar". Visual Studio, a continuación, agregará un archivo DinnersController.cs bajo el directorio \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

También se abrirá la nueva clase de DinnersController en el editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Agregar Index() y métodos de acción de Details() a la clase DinnersController

Queremos permitir a que los visitantes con nuestra aplicación para examinar una lista de las próximas cenas y que puedan hacer clic en cualquier cena en la lista para ver detalles concretos sobre él. Deberá hacerlo mediante la publicación de las siguientes direcciones URL de nuestra aplicación:

| **URL** | **Propósito** |
| --- | --- |
| */Dinners/* | Mostrar una lista HTML de cenas próximos |
| */Dinners/detalles / [id]* | Mostrar los detalles sobre una cena específica indicado por un parámetro "id" incrustado en la dirección URL: que coincidirá con la DinnerID de la cena de la base de datos. Por ejemplo: /Dinners/Details/2 podría mostrar una página HTML con detalles acerca de la cena cuyo valor de DinnerID es 2. |

Publicaremos implementaciones iniciales de estas direcciones URL mediante la adición de dos público "métodos de acción" a nuestra clase DinnersController como a continuación:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

A continuación, se podrá ejecutar la aplicación NerdDinner y usar nuestro navegador para invocar a los mismos. Escribir en el *"/ cenas /"* hará que la dirección URL de nuestro *Index()* método de ejecución y le enviará la respuesta siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Escribir en el *"/ cenas/detalles/2"* hará que la dirección URL de nuestro *Details()* método para ejecutar y volver a enviar la respuesta siguiente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Tal vez se pregunte: ¿Cómo supo MVC de ASP.NET para crear nuestra clase DinnersController e invocar los métodos? Para entender que vamos a echar un vistazo a cómo funciona el enrutamiento.

### <a name="understanding-aspnet-mvc-routing"></a>Enrutamiento de descripción ASP.NET MVC

ASP.NET MVC incluye un potente motor de enrutamiento de dirección URL que proporciona una gran flexibilidad para controlar cómo se asignan las direcciones URL a las clases de controlador. Nos permite personalizar completamente cómo ASP.NET MVC elige qué clase de controlador para crear, método que se debe invocar en él, así como configurar maneras diferentes que las variables pueden automáticamente analizar desde la dirección URL o la cadena de consulta y pasa al método como parámetro argumentos. Ofrece la flexibilidad para totalmente optimizar un sitio para SEO (optimización de motor de búsqueda), así como cualquier estructura de dirección URL que deseamos desde una aplicación de publicación.

De forma predeterminada, los nuevos proyectos de ASP.NET MVC vienen con un conjunto preconfigurado de reglas de enrutamiento de direcciones URL ya registrado. Esto nos permite fácilmente empezar a trabajar en una aplicación sin tener que configurar explícitamente nada. Los registros de regla de enrutamiento predeterminada se pueden encontrar la clase de "Aplicación" de nuestros proyectos: que se pueden abrir haciendo doble clic en el archivo "Global.asax" en la raíz de nuestro proyecto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Las reglas de enrutamiento de ASP.NET MVC predeterminada se registran en el método "RegisterRoutes" de esta clase:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Las rutas de". MapRoute() "llamada al método anterior registra una regla de enrutamiento predeterminada que asigna las direcciones URL entrantes a las clases de controlador con el formato de dirección URL:" / {controller} / {action} / {id} ", donde"controller"es el nombre de la clase de controlador para crear una instancia,"action"es el nombre de un método público que se invoca en él y "id" es un parámetro opcional incrustado en la dirección URL que puede pasarse como argumento al método. El tercer parámetro pasado a la llamada al método de "MapRoute()" es un conjunto de valores predeterminados que se va a usar para los valores de controlador / / Id. de acción en caso de que no están presentes en la dirección URL (controlador = "Home", acción = "Index", Id = "").

A continuación se muestra una tabla que muestra cómo una variedad de direcciones URL se asignan mediante el valor predeterminado "*/ {controladores} / {action} / {id}"*regla de ruta:

| **URL** | **Clase de controlador** | **Método de acción** | **Parámetros pasados** |
| --- | --- | --- | --- |
| */ Cenas/detalles/2* | DinnersController | Details(ID) | Id. = 2 |
| */ Cenas/editar/5* | DinnersController | Edit(ID) | Id. = 5 |
| */ Cenas/crear* | DinnersController | Método Create() | N/D |
| */ Cenas* | DinnersController | Index() | N/D |
| */ Principal* | HomeController | Index() | N/D |
| */* | HomeController | Index() | N/D |

Las tres últimas filas muestran los valores predeterminados (controlador = Home, acción = índice, Id = "") que se va a usar. Dado que el método "Index" está registrado como el nombre de acción predeterminada si no se especifica uno, el "/ cenas" y "/ Home" causa de las direcciones URL del método de acción Index() que se invocará en sus clases de controlador. Puesto que el controlador "Hogar" está registrado como el controlador predeterminado si no se especifica uno, la dirección URL "/" da lugar a la HomeController crearse y el método de acción Index() en lo que se debe invocar.

Si no le gusta estas reglas de enrutamiento de direcciones URL predeterminadas, lo bueno es que es fáciles cambiar: solo editarlos dentro del método RegisterRoutes anterior. Para nuestra aplicación NerdDinner, sin embargo, no vamos a cambiar cualquiera de las reglas de enrutamiento de dirección URL predeterminada: en su lugar, utilizaremos como-es.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Usar el DinnerRepository de nuestro DinnersController

Vamos a reemplazar ahora la implementación actual de la DinnersController Index() y Details() métodos de acción con las implementaciones que usan el modelo.

Vamos a usar la clase DinnerRepository que hemos creado anteriormente para implementar el comportamiento. Comenzaremos comienza agregando una instrucción "using" que hace referencia el espacio de nombres "NerdDinner.Models" y, a continuación, declarar una instancia de nuestro DinnerRepository como un campo en nuestra clase DinnerController.

Más adelante en este capítulo se le presentan el concepto de "Inyección de dependencia" y mostrar otra manera de nuestro controladores obtener una referencia a un DinnerRepository que permite una mejor unidad pruebas – pero para derecho ahora vamos a crear solo una instancia de nuestro DinnerRepository en línea como a continuación.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Ahora estamos preparados para generar una respuesta HTML con los objetos de modelo de datos recuperados.

### <a name="using-views-with-our-controller"></a>Usar vistas con nuestro controlador

Aunque es posible escribir código en nuestros métodos de acción para ensamblar HTML y, a continuación, use la *Response.Write ()* método auxiliar para enviar al cliente, que enfoque es bastante difícil de manejar rápidamente. Un enfoque mucho mejor es para que podamos realizar solo aplicación y la lógica de datos dentro de nuestro DinnersController los métodos de acción y, a continuación, pasar los datos necesarios para representar una respuesta HTML a una plantilla de "vista" independiente que es responsable de generar la representación de HTML del mismo. Como veremos en un momento, una plantilla de "ver" es un archivo de texto que normalmente contiene una combinación de marcado HTML y código de representación incrustado.

Separar la lógica del controlador de la representación de la vista ofrece varias ventajas grandes. En particular ayuda a aplicar una "separación de intereses" clara entre el código de la aplicación y el código de formato o representación de interfaz de usuario. Esto facilita mucho a la lógica de negocios de prueba unitaria de aislamiento de la lógica de procesamiento de la interfaz de usuario. Facilita más adelante modificar las plantillas de representación de la interfaz de usuario sin tener que realizar cambios de código de aplicación. Y puede hacer más fácil para los desarrolladores y diseñadores colaborar juntos en los proyectos.

Podemos actualizar nuestra clase DinnersController para indicar que desea usar una plantilla de vista para devolver una respuesta de IU de HTML si cambia las firmas de método de nuestros métodos de acción de dos de tener un tipo de valor devuelto de "void" en su lugar, tener un tipo de valor devuelto de "ActionResult". A continuación, podemos llamar a la *View()* método auxiliar en la clase base del controlador para devolver un objeto "ViewResult" como las siguientes:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma de la *View()* método auxiliar que usamos anteriormente es similar a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

El primer parámetro a la *View()* método auxiliar es el nombre del archivo de plantilla de la vista que desea utilizar para representar la respuesta HTML. El segundo parámetro es un objeto de modelo que contiene los datos que necesita la plantilla de vista para representar la respuesta HTML.

En el método de acción Index() estamos llamando a la *View()* método auxiliar y que indica que van a presentar una lista de HTML de cenas usando una plantilla de la vista "Index". La plantilla de vista que estamos pasando una secuencia de objetos de la cena para generar la lista de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

En el método de acción Details() se intenta recuperar un objeto de la cena con el identificador proporcionado en la dirección URL. Si se encuentra una cena válida llamamos el *View()* método auxiliar, que indica que deseamos utilizar una plantilla de la vista "Detalles" para representar el objeto recuperado de la cena. Si se solicita una cena no válido, se representará un mensaje de error útil que indica que la cena no existe con una plantilla de vista de "NotFound" (y una versión sobrecargada de la *View()* método auxiliar que simplemente toma el nombre de plantilla ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Implementemos ahora las plantillas de vista de "NotFound" y "Detalles", "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementar la plantilla de vista de "NotFound"

Comenzaremos mediante la implementación de la plantilla de vista de "NotFound", que muestra un mensaje de error descriptivo que indica que no se encuentra la cena solicitada.

Se deberá crear una nueva plantilla de la vista, coloque el cursor de texto dentro de un método de acción de controlador y, a continuación, haga clic en y elija el comando de menú "Agregar vista" (también podemos ejecutar este comando escribiendo Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Se abrirá un cuadro de diálogo "Agregar vista" como a continuación. De forma predeterminada, que el cuadro de diálogo rellenará automáticamente el nombre de la vista para crear para que coincida con el nombre del método de acción el cursor se encontraba cuando se inicia el cuadro de diálogo (en este caso, "Detalles"). Puesto que deseamos implementa la plantilla de "NotFound" por primera vez, se podrá invalidar este nombre de vista y establézcalo como "NotFound" en su lugar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Cuando se hace clic en el botón "Agregar", Visual Studio creará una nueva plantilla de vista de "NotFound.aspx" para que podamos dentro del directorio "\Views\Dinners" (que también creará si aún no existe el directorio):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

También se abrirá nuestra nueva plantilla de vista de "NotFound.aspx" en el editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Plantillas de vista de forma predeterminada tienen dos "áreas de contenido" donde podemos agregar contenido y código. La primera permite personalizar el "título" de la página HTML que se devuelve. La segunda permite personalizar el "contenido principal" de la página HTML que se devuelve.

Para implementar la plantilla de vista de "NotFound" vamos a agregar algún contenido básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Se puede, a continuación, pruébelo dentro del explorador. Para hacer esto, vamos a solicitud del *"/ cenas/detalles/9999"* dirección URL. Esto hará referencia a una cena que actualmente no existe en la base de datos y hará que el método de acción DinnersController.Details() procesar la plantilla de vista de "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Único lo que se notará en la captura de pantalla anterior es que la plantilla de vista básica ha heredado una serie de HTML que rodea el contenido en la pantalla principal. Esto es porque la plantilla de la vista está usando una plantilla de "página maestra" que nos permite aplicar un diseño coherente en todas las vistas en el sitio. Hablaremos de cómo funcionan más en la parte posterior de este tutorial las páginas maestras.

### <a name="implementing-the-details-view-template"></a>Implementar la plantilla de la vista "Detalles"

Implementemos ahora la plantilla de la vista "Detalles" – que generará HTML para un único modelo de la cena.

Se podrá hacerlo, coloque el cursor de texto dentro del método de acción de detalles y, a continuación, haga clic en y elija el comando de menú "Agregar vista" (o presione Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Se abrirá el cuadro de diálogo "Agregar vista". Se conservará el nombre de vista predeterminado ("Detalles"). También se podrá la casilla "Crear una vista fuertemente tipada" en el cuadro de diálogo y seleccionar (usando la lista desplegable de combobox) el nombre del tipo de modelo que estamos pasando desde el controlador a la vista. Para esta vista que estamos pasando un objeto de la cena (el nombre completo de este tipo es: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A diferencia de la plantilla anterior, donde hemos optado por crear una "Vista vacía", este tiempo que se elegirá automáticamente "aplicar la técnica scaffolding" la vista mediante una plantilla de "Detalles". Podemos indicar esto cambiando la lista desplegable "Ver contenido" en el cuadro de diálogo anterior.

"Scaffolding" generará una implementación inicial de nuestra plantilla de vista de detalles basándose en el objeto de la cena que pasamos a él. Esto proporciona una manera sencilla para que podamos empezar a trabajar rápidamente en nuestra implementación de la plantilla de vista.

Cuando se hace clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de la vista "Details.aspx" para que podamos dentro de nuestro directorio "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

También se abrirá nuestra nueva plantilla de vista de "Details.aspx" en el editor de código. Contendrá una implementación scaffold inicial de una vista de detalles basada en un modelo de la cena. El motor de scaffolding usa la reflexión de .NET para mirar las propiedades públicas que se exponen en la clase que lo ha pasado y lo agregará contenido adecuado en función de cada tipo que se encuentra:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Se puede solicitar la *"/ cenas/detalles/1"* dirección URL para ver el aspecto de esta implementación de scaffold "Detalles" en el explorador. Con esta dirección URL se mostrará uno de los cenas que hemos agregado manualmente a nuestra base de datos cuando se crea por primera vez:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Esto nos obtiene en marcha rápidamente y nos ofrece una implementación inicial de la vista Details.aspx. A continuación, podemos ir y retocar para personalizar la interfaz de usuario para satisfacción nuestra.

Cuando se examine más detenidamente en la plantilla Details.aspx, buscaremos que lo contiene HTML estático, así como código de representación incrustado. &lt;%%&gt; fragmentos de código ejecutan código cuando se representa la plantilla de vista, y &lt;% = %&gt; fragmentos de código ejecutar el código incluido dentro de ellos y, a continuación, presentar el resultado en el flujo de salida de la plantilla.

Podemos escribir código dentro de la vista que tiene acceso al objeto de modelo de "Cena" que se ha pasado desde nuestro controlador con una propiedad de "Modelo" fuertemente tipada. Visual Studio proporciona nos con intellisense completo de código al obtener acceso a esta propiedad "Modelo" en el editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos a hacer algunos ajustes de forma que el origen de la plantilla de vista de detalles final sea similar a continuación:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Cuando tenemos acceso a la *"/ cenas/detalles/1"* URL nuevo lo hará ahora representación como las siguientes:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementar la plantilla de la vista "Índice"

Implementemos ahora la plantilla de vista "Index": lo que generará una lista de las próximas cenas. Tareas pendientes, esta se coloque el cursor en el texto dentro del método de acción de índice y, a continuación, haga haga clic en y elija el comando de menú "Agregar vista" (o presione Ctrl-M, Ctrl-V).

En el cuadro de diálogo "Agregar vista" Comenzaremos mantener la plantilla de vista denominada "Índice" y seleccione la casilla de verificación "Crear una vista fuertemente tipada". Este tiempo se elegirá automáticamente generar una plantilla de vista de "lista de" y seleccione "NerdDinner.Models.Dinner" como el tipo de modelo, pasado a la vista (que ya hayamos indicado vamos a crear una "lista" scaffold hará que el cuadro de diálogo Agregar vista asuma estamos pasar una secuencia de objetos de la cena de nuestro controlador a la vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Cuando se hace clic en el botón "Agregar", Visual Studio creará un nuevo archivo de plantilla de la vista "Index.aspx" para que podamos dentro de nuestro directorio "\Views\Dinners". Le "aplicar la técnica scaffolding" una implementación inicial dentro de él que proporciona una lista de tabla HTML de la cenas pasamos a la vista.

Cuando se ejecutan la aplicación y el acceso a la *"/ cenas /"* dirección URL representará nuestra lista de cenas así:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solución de tabla anterior nos da un diseño de cuadrícula de nuestros datos cena que no son bastante lo que queremos para nuestro consumidor orientada hacia el anuncio de la cena. Podemos actualizar la plantilla de vista Index.aspx y modifíquela para que enumere menos columnas de datos y usar un &lt;ul&gt; elemento para representarlos en lugar de una tabla mediante el código siguiente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Estamos utilizando la palabra clave de "var" dentro de la instrucción foreach anterior que se ejecute un bucle sobre cada cena en nuestro modelo. Quienes no conocen con C# 3.0 que piense que utilizando "var" significa que el objeto de la cena está enlazada a un tiempo de ejecución. En su lugar, significa que el compilador usa la inferencia de tipos con respecto a la propiedad "Modelo" fuertemente tipada (que es de tipo "IEnumerable&lt;Dinner&gt;") y compilar la variable local "cena" como un tipo en cena, lo que significa que obtenemos completas IntelliSense y buscar dentro de bloques de código de tiempo de compilación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Cuando se alcanza la actualización el */Dinners* dirección URL en el explorador nuestra vista actualizada ahora parece a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Esto se examina un mejor – pero todavía no está completamente allí. El último paso consiste en habilitar los usuarios finales a clic cenas individuales en la lista y ver los detalles acerca de ellos. Se implementará mediante la representación de los elementos de hipervínculo HTML que vincula al método de acción detalles en nuestra DinnersController.

Podemos generar estos hipervínculos dentro de la vista de índice en uno de dos maneras. La primera consiste en crear manualmente HTML &lt;una&gt; elementos como las siguientes, donde se incrustar &lt;%%&gt; se bloquea dentro del &lt;un&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Una solución alternativa, podemos utilizar consiste en aprovechar el método de aplicación auxiliar "Html.ActionLink()" integrado en ASP.NET MVC que admite la creación mediante programación de un elemento HTML &lt;un&gt; elemento que se vincula a otro método de acción en un Controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

El primer parámetro al método auxiliar Html.ActionLink() es el texto del vínculo para mostrar (en este caso el título de la cena), el segundo parámetro es el nombre de acción de controlador que desea generar el vínculo para (en este caso, el método Details) y el tercer parámetro es un conjunto de parámetros que se enviarán a la acción (que se implementa como un tipo anónimo con valores de nombre de propiedad). En este caso se está especificando el parámetro "id" de la cena se desea establecer un vínculo y, dado que la dirección URL de enrutamiento predeterminado de regla en ASP.NET MVC es "{Controller} / {Action} / {id}" el método de aplicación auxiliar Html.ActionLink() generará el siguiente resultado:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para nuestro vista Index.aspx se podrá usar el enfoque de método de aplicación auxiliar de Html.ActionLink() y tener cada cena en el vínculo de la lista a la dirección URL de detalles adecuados:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Y ahora cuando se alcanza la */Dinners* URL nuestra lista cena es similar a continuación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Cuando se hace clic en cualquiera de los cenas en la lista se desplazará para ver detalles acerca de él:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Basada en convenciones de nomenclatura y la estructura de directorios \Views

Las aplicaciones de ASP.NET MVC predeterminada usar un directorio basado en la convención de nomenclatura estructura al resolver las plantillas de vista. Esto permite a los desarrolladores evitar tener que completar una ruta de acceso de ubicación al hacer referencia a vistas desde dentro de una clase de controlador. De forma predeterminada ASP.NET MVC buscará el archivo de plantilla de la vista en el * \Views\[ControllerName]\* directorio debajo de la aplicación.

Por ejemplo, hemos trabajado en la clase DinnersController, que hace referencia explícitamente a tres plantillas de vista: "Index", "Detalles" y "NotFound". ASP.NET MVC predeterminada será de estas vistas dentro de la *\Views\Dinners* directorio bajo el directorio raíz de la aplicación:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe encima cómo hay actualmente hay tres clases de controlador dentro del proyecto (DinnersController, HomeController y AccountController: las dos últimas se agregaran de forma predeterminada cuando se crea el proyecto), y hay tres subdirectorios (uno para cada uno controlador) dentro del directorio \Views.

Vistas que se hace referencia desde los controladores de inicio y las cuentas solucionará automáticamente sus plantillas de vista de los respectivos *\Views\Home* y *\Views\Account* directorios. El *\Views\Shared* subdirectorio proporciona una manera de almacenar las plantillas de vista que se reutilizan entre varios controladores dentro de la aplicación. Cuando ASP.NET MVC intenta resolver una plantilla de vista, primero compruebe dentro de la *\Views\[controlador]* directorio específico, y si no se encuentra la plantilla de vista no existe tendrá una apariencia dentro de la *\Views\ Compartido* directory.

En cuanto a las plantillas de vista individuales de nomenclatura, el uso recomendado es que la plantilla de vista comparten el mismo nombre que el método de acción que ha provocado que representar. Por ejemplo, por encima de nuestro "índice" método de acción está usando la vista "Índice" para representar el resultado de la vista y el método de acción "Detalles" está usando la vista "Detalles" para representar sus resultados. Esto facilita ver rápidamente la plantilla que está asociada a cada acción.

Los desarrolladores no es necesario especificar explícitamente el nombre de la plantilla de vista cuando la plantilla de vista tiene el mismo nombre que el método de acción que se va a invocar en el controlador. Podemos en su lugar, simplemente pasamos el objeto de modelo para el método de aplicación auxiliar de "View()" (sin especificar el nombre de vista) y ASP.NET MVC deducirá automáticamente que desea usar el *\Views\[ControllerName]\[ActionName]* plantilla de vista en el disco para representarlo.

Esto nos permite limpiar un poco el código del controlador, y evitar la duplicación del nombre dos veces en el código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

El código anterior es todo lo que es necesario para implementar una buena cena/detalles del anuncio experiencia para el sitio.

#### <a name="next-step"></a>Paso siguiente

Ahora tenemos una cena nice exploración compilada.

Permite ahora habilitar compatibilidad de edición de formularios de datos CRUD (creación, lectura, actualización, eliminación).

>[!div class="step-by-step"]
[Anterior](build-a-model-with-business-rule-validations.md)
[Siguiente](provide-crud-create-read-update-delete-data-form-entry-support.md)
