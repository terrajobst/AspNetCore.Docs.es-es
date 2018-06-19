---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar AJAX para implementar escenarios de asignación | Documentos de Microsoft
author: microsoft
description: Paso 11 muestra cómo integrar la compatibilidad con asignación de AJAX en nuestra aplicación NerdDinner, permitiendo a los usuarios que son crear, editar o ver cenas para ver la l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872720"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Usar AJAX para implementar escenarios de asignación
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 11 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 11 muestra cómo integrar la compatibilidad con asignación de AJAX en nuestra aplicación NerdDinner, permitiendo a los usuarios que son crear, editar o ver cenas para ver la ubicación de la cena de forma gráfica.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner paso 11: Integrar un mapa de AJAX

Ahora haremos nuestra aplicación un poco más visualmente interesante mediante la integración de compatibilidad con asignación de AJAX. Esto permitirá a los usuarios que son crear, editar o ver cenas para ver la ubicación de la cena de forma gráfica.

### <a name="creating-a-map-partial-view"></a>Crear una vista parcial del mapa

Vamos a usar la funcionalidad de asignación en varios lugares de la aplicación. Para mantener el código SECA se podrá encapsulan la funcionalidad de asignación comunes dentro de una única plantilla parcial que podemos volver a usar entre varias acciones del controlador y vistas. Comenzaremos el nombre de esta vista parcial "map.ascx" y crear en el directorio \Views\Dinners.

Podemos crear la map.ascx parcial, el botón secundario en el directorio \Views\Dinners y elija Agregar -&gt;ver el comando de menú. Se le mencione la vista "Map.ascx", compruebe como una vista parcial e indicar que vamos a pasar una clase de modelo "Cena" fuertemente tipado:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Cuando se hace clic en el botón "Agregar" se creará nuestro parcial de plantillas. A continuación, actualizaremos el archivo Map.ascx para incluir el contenido siguiente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La primera &lt;script&gt; puntos de referencia para la biblioteca de asignación de Microsoft Virtual Earth 6.2. El segundo &lt;script&gt; puntos de referencia para un archivo map.js que se va a crear en breve que va a encapsular la lógica de asignación de Javascript comunes. El &lt;div id = "theMap"&gt; elemento es el contenedor HTML que va a usar para hospedar el mapa de Virtual Earth.

A continuación, tenemos un incrustado &lt;script&gt; bloque que contiene dos funciones de JavaScript específicas a esta vista. La primera función utiliza jQuery cable de seguridad de una función que se ejecuta cuando la página está lista para ejecutar el script de cliente. Llama a una función de aplicación auxiliar de LoadMap() que se definirá dentro de nuestro archivo de script Map.js para cargar el control de mapa de virtual earth. La segunda función es un controlador de eventos de devolución de llamada que se agrega un pin al mapa que identifica una ubicación.

Tenga en cuenta cómo estamos usando un servidor &lt;% = %&gt; bloque dentro del bloque de script de cliente para incrustar la latitud y longitud de la cena que queremos asignar en el código JavaScript. Se trata de una técnica útil para generar valores dinámicos que pueden usarse en el script de cliente (sin necesidad de una independiente llamada AJAX al servidor para recuperar los valores, que resulta más rápido). El &lt;% = %&gt; bloques se ejecutarán cuando la vista se está procesando en el servidor, y por lo que la salida del HTML simplemente terminará con valores de JavaScript incrustados (por ejemplo: latitud var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Crear una biblioteca de utilidades de Map.js

Vamos a crear ahora el archivo Map.js que podemos usar encapsulan la funcionalidad de JavaScript para la asignación (e implementar los métodos LoadMap y LoadPin anteriores). Podemos hacer esto con el botón secundario en el directorio \Scripts dentro de nuestro proyecto y, a continuación, elija el "Add -&gt;nuevo elemento" comando de menú, seleccione el elemento de JScript y asígnele el nombre "Map.js".

A continuación se muestra el código de JavaScript se agregará al archivo Map.js que interactuará con Virtual Earth a fin de mostrar el mapa y agregarle los PIN de ubicaciones para nuestro cenas:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integración de la asignación con crear y editar formularios

Ahora se podrá integrar la compatibilidad con el mapa con cuatro escenarios de creación y edición de existente. Lo bueno es que esto es bastante fácil tareas pendientes y no tenemos que cambiar el código del controlador. Puesto que nuestro vistas de creación y edición de comparten una vista parcial "DinnerForm" común para implementar la interfaz de usuario de forma cena, podemos agregar el mapa en un solo lugar y tiene cuatro escenarios de creación y edición de usarlo.

Lo que debemos tareas es abrir la vista parcial \Views\Dinners\DinnerForm.ascx y actualizarlo para incluir la nueva asignación parcial. A continuación se muestra el DinnerForm actualizado aspecto que tendrá una vez que se agrega la asignación (Nota: se omiten los elementos de formulario HTML en el fragmento de código a continuación por razones de brevedad):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

El DinnerForm parcial anteriormente toma un objeto de tipo "DinnerFormViewModel" como su tipo de modelo (porque se tiene un objeto de la cena, así como un SelectList para rellenar la dropdownlist de países). Nuestro mapa parcial solo necesita un objeto de tipo "Cena" como su tipo de modelo y, por lo tanto, cuando se procesa el mapa parcial que estamos pasando simplemente la cena subpropiedad de DinnerFormViewModel a él:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La función de JavaScript se agregaron a jQuery parcial utiliza para adjuntar un evento "desenfoque" en el cuadro de texto de "Dirección" HTML. Probablemente habrá oído de eventos "foco" que se desencadenan cuando un usuario hace clic o pestañas en un cuadro de texto. Lo contrario es un evento de "desenfoque" que se desencadena cuando un usuario cierra un cuadro de texto. El controlador de eventos anterior borra los valores de cuadro de texto de latitud y longitud cuando esto sucede y, a continuación, traza la nueva ubicación de dirección en el mapa. Un controlador de eventos de devolución de llamada que se define en el archivo map.js, a continuación, actualizará los cuadros de texto de longitud y latitud en nuestro formulario con los valores devueltos por virtual earth basándose en la dirección que se le asignó.

Y ahora cuando se ejecute de nuevo la aplicación y haga clic en ella "Host cena" veremos, el valor predeterminado es asignar muestran junto con los elementos de nuestro formulario cena estándares:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Cuando se escribe en una dirección y, a continuación, pestaña inmediatamente, el mapa se actualiza dinámicamente para mostrar la ubicación y el controlador de eventos rellenará los cuadros de texto de latitud y con los valores de ubicación:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si se guarda la cena nuevo y vuelva a abrirlo para modificarlo, buscaremos que se muestra la ubicación del mapa cuando se carga la página:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Cada vez que se cambia el campo de dirección, se actualizarán la asignación y las coordenadas de latitud y longitud.

Ahora que el mapa muestra la ubicación de la cena, también podemos cambiar los campos de formulario de latitud y longitud de la que se va a cuadros de texto visibles en su lugar se elementos ocultos (ya que la asignación actualiza automáticamente cada vez que se especifique una dirección). Tareas pendientes Esto se le cambia de utilizar la aplicación auxiliar de HTML Html.TextBox() al uso del método de aplicación auxiliar de Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Y ahora los formularios son un poco más fácil de usar y evitar que se muestre la latitud sin formato (mientras todavía almacenarlas con cada cena en la base de datos):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrar el mapa con la vista de detalles

Ahora que tenemos la asignación que se integra con cuatro escenarios de creación y edición, vamos a también integrarlo con nuestro escenario de detalles. Lo que debemos tarea consiste en llamar a &lt;% Html.RenderPartial("map"); %&gt; dentro de la vista de detalles.

A continuación se muestra el aspecto que tiene el código fuente a la vista de detalles completo (con la integración de asignación):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Y ahora cuando un usuario navega a una dirección /Dinners/detalles / [id] URL podrá ver detalles acerca de la cena, la ubicación de la cena de la asignación (junto con un icono de alfiler que, cuando mantiene el mouse sobre muestra el título de la cena y la dirección del mismo), y tener un vínculo de AJAX al RSVP fo r:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementar la búsqueda de ubicación en la base de datos y del repositorio

Para terminar de desactivar nuestra implementación de AJAX, vamos a agregar un mapa a la página principal de la aplicación que permite a los usuarios buscar gráficamente cenas cerca de ellos.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Comenzaremos mediante la implementación de soporte técnico dentro de la capa de repositorio y base de datos para realizar eficazmente una búsqueda de radius basados en la ubicación de cenas. Podríamos usar el nuevo [geospatial características de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) para hacerlo, o también podemos usar un enfoque de la función SQL Gary Dryden se tratan en este artículo: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) y Rob Conery escribió en su blog sobre el uso con LINQ to SQL aquí: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar esta técnica, se se abra el Explorador de servidores de"" dentro de Visual Studio, seleccione la base de datos NerdDinner y, a continuación, haga doble clic en el subnodo de "funciones" en él y optar por crear una nuevo "función escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

A continuación, se pegará en la siguiente función DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

A continuación, vamos a crear una nueva función con valores de tabla en SQL Server que llamaremos "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Esta función de tabla "NearestDinners" usa la función auxiliar DistanceBetween para devolver todos los cenas 100 millas de la latitud y longitud se proporcionar:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para llamar a esta función, se le abrirán el diseñador LINQ to SQL haciendo doble clic en el archivo NerdDinner.dbml dentro de nuestro directorio \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

A continuación, arrastraré las funciones NearestDinners y DistanceBetween en LINQ al diseñador SQL, lo que provocará que se agregará como métodos en nuestro LINQ para SQL NerdDinnerDataContext clase:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

A continuación, se puede exponer un método de consulta "FindByLocation" en nuestra clase DinnerRepository que usa la función NearestDinner para devolver las próximas cenas que son menos de 100 millas de la ubicación especificada:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementa un método de acción de búsqueda basada en JSON AJAX

Ahora implementaremos un método de acción de controlador que aprovecha las ventajas del nuevo método de repositorio FindByLocation() para devolver una lista de datos de la cena que pueden usarse para rellenar un mapa. Contamos con este método de acción devolver los datos de la cena en un formato JSON (JavaScript Object Notation) para que los se puede manipular fácilmente mediante JavaScript en el cliente.

Para hacerlo, vamos a crear una nueva clase de "SearchController", el botón secundario en el directorio \Controllers y elija Agregar -&gt;comando de menú del controlador. A continuación, implementaremos un método de acción "SearchByLocation" dentro de la nueva clase SearchController como a continuación:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Método de acción del SearchController SearchByLocation internamente llama al método de FindByLocation en DinnerRespository para obtener una lista de cenas cercanos. En lugar de devolver los objetos de la cena directamente al cliente, sin embargo, en su lugar devuelve objetos JsonDinner. La clase JsonDinner expone un subconjunto de propiedades de la cena (por ejemplo: por razones de seguridad no revela los nombres de las personas que han para RSVP para una cena). También incluye una propiedad RSVPCount que no existe en la cena – y que se calcula dinámicamente contando el número de objetos RSVP asociados a una cena determinada.

A continuación, usamos el método de aplicación auxiliar de Json() en la clase base del controlador para devolver la secuencia de cenas con un formato de conexión basada en JSON. JSON es un formato de texto estándar para representar estructuras de datos simples. A continuación se muestra un ejemplo de una lista con formato JSON de dos objetos de JsonDinner aspecto cuando se devuelve desde el método de acción:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Llamar al método basada en JSON AJAX mediante jQuery

Ahora estamos preparados para actualizar la página principal de la aplicación de NerdDinner para utilizar el método de acción del SearchController SearchByLocation. Tareas pendientes, se abrirá la plantilla de vista de /Views/Home/Index.aspx y actualizarla para que tenga un cuadro de texto, botón de búsqueda, el mapa y un &lt;div&gt; elemento denominado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

A continuación, podemos agregar dos funciones de JavaScript a la página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La primera función de JavaScript carga el mapa cuando se carga la página por primera vez. El segundo cables de función de JavaScript de JavaScript, haga clic en el controlador de eventos en el botón de búsqueda. Cuando se presiona el botón llama a la función de JavaScript de FindDinnersGivenLocation() que vamos a agregar a nuestro archivo Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Esta función FindDinnersGivenLocation() llama mapa. Find() en el Control de tierra Virtual a centrarse en la ubicación especificada. Cuando se devuelve el servicio de mapas de virtual earth, el mapa. Método Find() invoca el método de devolución de llamada de callbackUpdateMapDinners que se pasa como el argumento final.

El método callbackUpdateMapDinners() es donde se realiza el trabajo real. Usa método de aplicación auxiliar de $.post() de jQuery para realizar una llamada de AJAX al método de acción SearchByLocation() de nuestro SearchController – pasándole la latitud y longitud de la asignación centrada recién. Define una función insertada que se llamará cuando se completa el método de aplicación auxiliar $.post() y devuelven los resultados de la cena con formato JSON de la SearchByLocation() se pasará el método de acción mediante una variable denominada "cenas". A continuación, realiza una instrucción foreach por cada cena devuelto y usa de la cena latitud y otras propiedades para agregar un nuevo código pin en el mapa. También se agrega una entrada de la cena a la lista HTML de cenas a la derecha de la asignación. A continuación, cables vertical un evento para los marcadores y la lista HTML para que se muestren los detalles acerca de la cena cuando un usuario mantiene el mouse sobre ellos:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Y ahora cuando se ejecute la aplicación y visite la página de inicio que se mostrarán con un mapa. Cuando se escriba el nombre de una ciudad el mapa muestra la próximas cenas cerca de ella:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Mantiene el mouse sobre una cena mostrará detalles sobre él.

Haga clic en el título de la cena en la burbuja o en el lado derecho de la lista HTML, se le remitirá nos a la cena: que se si lo desea, a continuación, podemos RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Paso siguiente

Ahora hemos implementado todas las funciones de aplicación de nuestra aplicación NerdDinner. Supongamos ahora vistazo a cómo podemos habilitar automatizada unidad pruebas del mismo.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Siguiente](enable-automated-unit-testing.md)
