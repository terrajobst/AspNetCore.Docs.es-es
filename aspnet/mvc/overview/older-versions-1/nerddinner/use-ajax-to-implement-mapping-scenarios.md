---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar AJAX para implementar escenarios de asignación | Microsoft Docs
author: microsoft
description: Paso 11 muestra cómo integrar la compatibilidad con la asignación de AJAX en nuestra aplicación NerdDinner, lo que permite a los usuarios crear, editar o ver instancias dinners para ver la l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 173ba551ca453ad46dbfd83ce1a2eb4a9b8eba3f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374297"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Usar AJAX para implementar escenarios de asignación
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 11 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 11 muestra cómo integrar la compatibilidad con la asignación de AJAX en nuestra aplicación NerdDinner, lo que permite a los usuarios crear, editar o ver instancias dinners para ver la ubicación de la cena gráficamente.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner paso 11: La integración un mapa de AJAX

Ahora haremos nuestra aplicación un poco más visualmente interesante mediante la integración de compatibilidad con la asignación de AJAX. Esto permitirá a los usuarios crear, editar o ver instancias dinners para ver la ubicación de la cena gráficamente.

### <a name="creating-a-map-partial-view"></a>Creación de una vista parcial del mapa

Vamos a usar la funcionalidad de asignación en varios lugares dentro de nuestra aplicación. Para mantener nuestro código seco, encapsularemos la funcionalidad de asignación comunes dentro de una sola plantilla parcial que podemos volver a usar en varias acciones de controlador y vistas. Estudiaremos esta vista parcial "map.ascx" el nombre y crearla dentro del directorio \Views\Dinners.

Podemos crear el map.ascx parcial con el botón secundario en el directorio \Views\Dinners y eligiendo agregar -&gt;ver el comando de menú. A nombre de la vista "Map.ascx", protegerlo como una vista parcial e indican que vamos a pasarle una clase de modelo fuertemente tipado "comida":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Cuando hacemos clic en el botón "Agregar" se crearán nuestra plantilla parcial. A continuación, se actualizará el archivo Map.ascx para tener el siguiente contenido:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La primera &lt;script&gt; puntos de referencia para la biblioteca de asignación de Microsoft Virtual Earth 6.2. El segundo &lt;script&gt; referencia apunta a un archivo map.js que crearemos en breve que va a encapsular la lógica de asignación de Javascript comunes. El &lt;div id = "theMap"&gt; elemento es el contenedor HTML que se va a utilizar para hospedar el mapa de Virtual Earth.

A continuación, tenemos un incrustado &lt;script&gt; bloque que contiene dos funciones de JavaScript específicas de esta vista. La primera función utiliza jQuery para conexión de seguridad de una función que se ejecuta cuando la página está lista para ejecutar el script de cliente. Llama a una función auxiliar de LoadMap() que definiremos dentro de nuestro archivo de script Map.js para cargar el control de mapa de virtual earth. La segunda función es un controlador de eventos de devolución de llamada que se agrega un pin con el mapa que identifica una ubicación.

Tenga en cuenta cómo estamos usando un servidor &lt;% = %&gt; bloque dentro del bloque de script de cliente para incrustar la latitud y longitud de la cena deseamos asignar en el código JavaScript. Esto es una técnica útil para generar valores dinámicos que se pueden usar por el script de cliente (sin necesidad de una independiente llamada AJAX al servidor para recuperar los valores, lo que es más rápido). El &lt;% = %&gt; bloques se ejecutarán cuando se está representando la vista en el servidor, y por lo que la salida del archivo HTML sólo terminará con valores de JavaScript incrustados (por ejemplo: var latitud = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creación de una biblioteca de utilidades Map.js

Vamos a crear ahora el archivo Map.js que podemos usar para encapsular la funcionalidad de JavaScript para nuestro mapa del (e implementar los métodos LoadMap y LoadPin anteriores). Podemos hacer esto con el botón secundario en el directorio \Scripts dentro de nuestro proyecto y, a continuación, elija el "Add -&gt;nuevo elemento" comando de menú, seleccione el elemento de JScript y asígnele el nombre "Map.js".

A continuación es el código de JavaScript que vamos a agregar al archivo Map.js que interactuará con Virtual Earth a fin de mostrar el mapa y agregarle PIN ubicaciones para nuestro dinners:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>El mapa de la integración con formularios de editar y crear

Ahora Integraremos la compatibilidad con los mapas con nuestros escenarios Create y Edit existentes. La buena noticia es que esto es una tarea bastante sencilla y no precisar de nuestra intervención cambiar cualquiera de nuestro código de controlador. Porque nuestras vistas Create y Edit compartan una vista parcial "DinnerForm" común para implementar la interfaz de usuario de forma cena, podemos agregar la asignación en un solo lugar y tienen nuestros escenarios Create y Edit usarlo.

Todo lo que necesitamos tareas pendientes es abrir la vista parcial \Views\Dinners\DinnerForm.ascx y actualizarlo para incluir la nueva asignación parcial. A continuación está la DinnerForm actualizado aspecto que tendrá una vez que se agrega la asignación (Nota: se omiten los elementos de formulario HTML en el fragmento de código siguiente por razones de brevedad):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

El DinnerForm parcial anteriormente toma un objeto de tipo "DinnerFormViewModel" como su tipo de modelo (ya que necesita un objeto de la cena, así como un SelectList para rellenar la dropdownlist de países). Nuestro mapa parcial solo necesita un objeto de tipo "Cena" como su tipo de modelo y, por lo tanto, cuando se procesa el mapa parcial que estamos pasando simplemente la cena subpropiedad de DinnerFormViewModel a él:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La función de JavaScript se agregó a jQuery usa parcial para adjuntar un evento "desenfoque" en el cuadro de texto "Address" HTML. Probablemente habrá oído de eventos de "enfoque" que se activan cuando un usuario hace clic o pestañas en un cuadro de texto. Lo contrario es un evento de "desenfoque" que se desencadena cuando un usuario sale de un cuadro de texto. El controlador de eventos anterior borra los valores de cuadro de texto de latitud y longitud cuando esto sucede y, a continuación, traza la nueva ubicación de dirección en nuestro mapa. Un controlador de eventos de devolución de llamada que hemos definido dentro del archivo map.js, a continuación, actualizará los cuadros de texto de longitud y latitud en nuestro formulario con los valores devueltos por virtual earth basándose en la dirección que se le asignó.

Y ahora al ejecutar la aplicación de nuevo y haga clic en la pestaña "Host cena" vamos a ver un valor predeterminado de mapa muestra junto con los elementos de nuestro formulario cena estándares:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Cuando se escriba una dirección y, a continuación, pestaña ausente, el mapa se actualiza dinámicamente para mostrar la ubicación y nuestro controlador de eventos rellenará los cuadros de texto de la latitud y con los valores de ubicación:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si se guarda la cena nuevo y vuelva a abrirlo para su edición, se encontrará que se muestra la ubicación del mapa cuando se carga la página:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Cada vez que se cambia el campo de dirección, se actualizarán la asignación y las coordenadas de latitud y longitud.

Ahora que el mapa muestra la ubicación de la cena, también podemos cambiar los campos de formulario de latitud y longitud de la que se va a cuadros de texto visible en su lugar ser elementos ocultos (ya que el mapa actualiza automáticamente cada vez que se escribe una dirección). Tareas pendientes Esto se deberá cambiar del uso de la aplicación auxiliar de HTML Html.TextBox() al uso del método de aplicación auxiliar Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Y ahora los formularios son un poco más fácil de usar y evitar que se muestre la latitud y longitud sin procesar (mientras sigue almacenarlos con cada comida en la base de datos):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>La integración de la asignación con la vista de detalles

Ahora que tenemos el mapa integrado con nuestros escenarios Create y Edit, vamos a también integrarlo con nuestro escenario de detalles. Todo lo que necesitamos to-do es llamar a &lt;% Html.RenderPartial("map"); %&gt; dentro de la vista de detalles.

A continuación es el aspecto que tiene el código fuente a la vista de detalles completo (con la integración de mapa):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Y ahora cuando un usuario navega a una URL/detalles / [id] / dinners verá detalles sobre la cena, la ubicación de la cena de la asignación (junto con un icono de alfiler que, cuando mantenido sobre muestra el título de la cena y la dirección del mismo), y tener un vínculo de AJAX para RSVP fo r lo:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementar la búsqueda de ubicación en nuestra base de datos y el repositorio

Para finalizar la implementación de AJAX, vamos a agregar un mapa a la página principal de la aplicación que permite a los usuarios buscar gráficamente las cenas cerca de ellos.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Comenzaremos por implementar la compatibilidad dentro de nuestro capa de repositorio de datos y la base de datos para realizar una búsqueda de radius basados en la ubicación para instancias Dinners de forma eficaz. Podríamos usar el nuevo [funciones geoespaciales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementar esto, o también podemos usar un enfoque de la función SQL Gary Dryden descrito en este artículo: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) y Rob Conery blog sobre el uso con LINQ to SQL aquí: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar esta técnica, nos se abra el "Explorador de servidores" dentro de Visual Studio, seleccione la base de datos de NerdDinner y, a continuación, haga doble clic en el subnodo "funciones" en él y optar por crear una nuevo "función escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Pegaremos, a continuación, en la siguiente función DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

A continuación, vamos a crear una nueva función con valores de tabla en SQL Server lo llamaremos "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Esta función de la tabla "NearestDinners" usa la función auxiliar DistanceBetween para devolver todas las instancias Dinners 100 millas de la latitud y longitud, proporcionar:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para llamar a esta función, se le abrirá el diseñador LINQ to SQL haciendo doble clic en el archivo NerdDinner.dbml dentro de nuestro directorio \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Se luego arrastrará las funciones NearestDinners y DistanceBetween LINQ al diseñador SQL, lo que provocará que se agreguen como métodos en nuestra LINQ a SQL NerdDinnerDataContext clase:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

A continuación, se puede exponer un método de consulta "FindByLocation" en nuestra clase DinnerRepository que usa la función NearestDinner para devolver próximas Dinners que son menos de 100 millas de la ubicación especificada:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementar un método de acción de búsqueda de AJAX basadas en JSON

Ahora implementaremos un método de acción de controlador que aprovecha las ventajas del nuevo método de repositorio FindByLocation() para devolver una lista de los datos de Dinner que pueden usarse para rellenar un mapa. Contamos con este método de acción devolver los datos de la cena en formato JSON (JavaScript Object Notation) para que pueden manipularse fácilmente mediante JavaScript en el cliente.

Para implementar esto, vamos a crear una nueva clase "SearchController" con el botón secundario en el directorio \Controllers y eligiendo agregar -&gt;comando de menú del controlador. A continuación, implementaremos un método de acción "SearchByLocation" dentro de la nueva clase SearchController como a continuación:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Método de acción del SearchController SearchByLocation llama internamente al método de FindByLocation en DinnerRespository para obtener una lista de instancias dinners cercanos. En lugar de devolver los objetos de la cena directamente al cliente, sin embargo, en su lugar devuelve objetos JsonDinner. La clase JsonDinner expone un subconjunto de propiedades de la cena (por ejemplo: por razones de seguridad no revela los nombres de las personas que han confirmado una cena). También incluye una propiedad RSVPCount que no existe en la cena – y que se calcula dinámicamente contando el número de objetos RSVP asociados a una cena en particular.

A continuación, usamos el método auxiliar Json() en la clase base del controlador para devolver la secuencia de instancias dinners utilizando un formato basado en JSON. JSON es un formato de texto estándar para representar estructuras de datos simple. A continuación es un ejemplo de una lista con formato JSON de dos objetos JsonDinner aspecto cuando se devuelve desde el método de acción:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Una llamada al método basadas en JSON de AJAX mediante jQuery

Ahora estamos preparados actualizar la página principal de la aplicación NerdDinner para usar el método de acción del SearchController SearchByLocation. Tareas pendientes, se abrirá la plantilla de vista /Views/Home/Index.aspx y actualizarlo para que tenga un cuadro de texto, botón de búsqueda, nuestro mapa y un &lt;div&gt; elemento denominado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

A continuación, podemos agregar dos funciones de JavaScript a la página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La primera función de JavaScript carga el mapa de la página se carga por primera vez. La segunda cables de función de JavaScript de JavaScript, haga clic en el controlador de eventos en el botón de búsqueda. Cuando se presiona el botón llama a la función de JavaScript FindDinnersGivenLocation() que agregaremos a nuestro archivo Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Esta función FindDinnersGivenLocation() llama al mapa. Find() en el Control de Virtual Earth centrarla en la ubicación especificada. Cuando se devuelve el servicio de mapas virtual earth, el mapa. Método Find() invoca el método de devolución de llamada callbackUpdateMapDinners que se pasa como el argumento final.

El método callbackUpdateMapDinners() es donde se realiza el trabajo real. Usa método de aplicación auxiliar de $.post() de jQuery para realizar una llamada AJAX al método de acción SearchByLocation() de nuestro SearchController – pasándole la latitud y longitud del mapa recién centrado. Define una función insertada que se llamará cuando se completa el método auxiliar $.post() y devuelven los resultados de la cena con formato JSON de la SearchByLocation() se pasará el método de acción mediante una variable denominada "dinners". A continuación, realiza una instrucción foreach por cada cena devuelto y usa de la cena latitud y otras propiedades para agregar un nuevo pin en el mapa. También se agrega una entrada de la cena a la lista HTML de instancias dinners a la derecha del mapa. A continuación, cables vertical un evento de desplazamiento para los marcadores y la lista HTML para que se muestran detalles acerca de la cena cuando un usuario mantiene el mouse sobre ellos:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Y ahora al ejecutar la aplicación y visite la página principal que se mostrarán con un mapa. Cuando se escribe el nombre de una ciudad el mapa mostrará los próximos dinners cerca de ella:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Mantener el mouse sobre una cena mostrará detalles sobre él.

Haga clic en el título de la cena en la burbuja o en el lado derecho de la lista HTML navegará nos a la cena – que, opcionalmente, a continuación, podemos RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Paso siguiente

Ahora hemos implementado toda la funcionalidad de aplicación de nuestra aplicación NerdDinner. Veamos ahora vistazo a cómo podemos habilitar unitarias automatizadas las pruebas del mismo.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Siguiente](enable-automated-unit-testing.md)
