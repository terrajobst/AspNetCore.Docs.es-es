---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar la paginación eficiente de los datos | Microsoft Docs
author: microsoft
description: Paso 8 muestra cómo agregar compatibilidad con la paginación a nuestra dirección URL de/dinners para que en lugar de mostrar 1000s de instancias dinners a la vez, se mostrará solo 10 instancias dinners próximos a...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842148"
---
<a name="implement-efficient-data-paging"></a>Implementar la paginación eficiente de los datos
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 8 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 8 muestra cómo agregar compatibilidad con la paginación a nuestra dirección URL de/dinners para que en lugar de mostrar 1000s de instancias dinners a la vez, solo podrá mostrar 10 instancias dinners próximas a la vez - y permitir a los usuarios finales página atrás y hacia delante a través de la lista completa de una manera descriptiva SEO.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner paso 8: Compatibilidad con la paginación

Si el sitio se realiza correctamente, tendrá miles de instancias dinners próximas. Es necesario para asegurarse de que se adapta para administrar todas estas cenas tienen nuestra interfaz de usuario y permite a los usuarios examinar ellos. Para habilitar esto, vamos a agregar compatibilidad con la paginación a nuestro */dinners* dirección URL para que en lugar de mostrar 1000s de instancias dinners al mismo tiempo, se le solo mostrar 10 instancias dinners próximas a la vez - y permiten a los usuarios finales página atrás y hacia delante a través de la lista completa de una manera descriptiva SEO.

### <a name="index-action-method-recap"></a>Resumen de método de acción de Index()

El método de acción Index() dentro de nuestra clase DinnersController actualmente es similar a la siguiente:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Cuando se realiza una solicitud a la */dinners* dirección URL, se recupera una lista de todas las próximas dinners y, a continuación, presenta una lista de todas ellas:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Descripción IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  es una interfaz que se introdujo con LINQ como parte de .NET 3.5. Habilita escenarios eficaces "aplazamiento" se puede emplear las ventajas de implementar la compatibilidad con la paginación.

En nuestra instancia DinnerRepository nos estamos devolviendo un tipo IQueryable&lt;Dinner&gt; secuencia desde nuestro método FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt; objeto devuelto por el método FindUpcomingDinners() encapsula una consulta para recuperar objetos de la cena de nuestra base de datos mediante LINQ to SQL. Lo que es importante, no se ejecutará la consulta en la base de datos hasta que se intente acceso/iterar por los datos en la consulta, o hasta que llamamos al método ToList() en él. El código que llama a nuestro método FindUpcomingDinners() puede optar por agregar filtros de las operaciones "encadenados" adicionales al IQueryable&lt;Dinner&gt; objeto antes de ejecutar la consulta. LINQ to SQL, a continuación, es lo suficientemente inteligente como para ejecutar la consulta combinada en la base de datos cuando se solicitan los datos.

Para implementar la lógica de paginación podemos actualizar método de acción de Index() de nuestro DinnersController para que se aplique operadores "Skip" y "Take" adicionales al IQueryable devuelto&lt;Dinner&gt; secuencia antes de llamar a ToList() en él:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

El código anterior omite los 10 primeros dinners próximos en la base de datos y, a continuación, devuelve las 20 cenas. LINQ to SQL es lo suficientemente inteligente como para crear una consulta optimizada de SQL que realiza esta omitiendo la lógica en la base de datos SQL y no en el servidor web. Esto significa que, incluso si tenemos millones de instancias Dinners próximos en la base de datos, se recuperarán sólo las 10 que queremos como parte de esta solicitud (lo que eficaz y escalable).

### <a name="adding-a-page-value-to-the-url"></a>Agregar un valor de "página" a la dirección URL

En lugar de codificar un intervalo de páginas específico, desearemos nuestras direcciones URL para incluir un parámetro de "página" que indica el intervalo de la cena solicita un usuario.

#### <a name="using-a-querystring-value"></a>Uso de un valor de cadena de consulta

El código siguiente muestra cómo podemos actualizar nuestro método de acción de Index() para admitir un parámetro de cadena de consulta y habilitar las direcciones URL como */dinners? página = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

El método de acción de Index() anterior tiene un parámetro denominado "página". El parámetro se declara como un entero que acepta valores null (que es lo que int? indica). Esto significa que el */dinners? página = 2* URL hará que un valor de "2" que se pasarán como el valor del parámetro. El */dinners* dirección URL (sin un valor de cadena de consulta) hará que un valor null que se pasa.

Nos estamos multiplicar el valor de la página por el tamaño de página (en este caso 10 filas) para determinar cuántas instancias dinners para pasarla por alto. Usamos el [C# "" operador de incorporación null (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) lo que resulta útil cuando se trabaja con tipos que aceptan valores NULL. El código anterior asigna el valor 0 de página si el parámetro de página es null.

#### <a name="using-embedded-url-values"></a>Con los valores de dirección URL incrustada

Una alternativa al uso de un valor de cadena de consulta sería incrustar el parámetro de página dentro de la propia URL. Por ejemplo: */Dinners/Page/2* o */Dinners/2*. ASP.NET MVC incluye un eficaz motor de enrutamiento de dirección URL que facilita la compatibilidad con escenarios como éste.

Podemos registrar reglas de enrutamientos personalizadas que se asigna ninguna dirección URL o la dirección URL entrante, dar formato a cualquier método de clase o una acción de controlador que deseamos. Todo lo que necesitamos to-do es abrir el archivo Global.asax dentro de nuestro proyecto:

![](implement-efficient-data-paging/_static/image2.png)

Y, a continuación, registre una nueva regla de asignación mediante el método auxiliar MapRoute() como la primera llamada a las rutas. MapRoute() siguiente:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Anteriormente, se está registrando una nueva regla de enrutamiento denominada "UpcomingDinners". Nos estamos indicando que tiene el formato de dirección URL "Dinners/página {páginas}", donde {page} es un valor de parámetro incrustado dentro de la dirección URL. El tercer parámetro al método MapRoute() indica que deberíamos asignamos las direcciones URL que coinciden con este formato para el método de acción Index() en la clase DinnersController.

Podemos usar exactamente el mismo Index() código que teníamos antes con nuestro escenario de cadena de consulta: excepto ahora nuestro parámetro "página" procederán de la dirección URL y no la cadena de consulta:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Y ahora al ejecutar la aplicación y escriba en */dinners* veremos los 10 primeros dinners próximas:

![](implement-efficient-data-paging/_static/image3.png)

Y cuando se escriba en */Dinners/Page/1* se verá la página siguiente de instancias dinners:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Adición de la interfaz de usuario de navegación de páginas

El último paso para completar este escenario de paginación será implementar la navegación de "siguiente" y "anterior" interfaz de usuario dentro de nuestra plantilla de vista para permitir que los usuarios saltar fácilmente los datos de la cena.

Para implementar esto correctamente, necesitaremos saber el número total de instancias Dinners de la base de datos, así como la forma muchas páginas de datos de esto se traduce en. Necesitaremos calcular si el valor de "página" solicitada actualmente está al principio o al final de los datos y mostrar u ocultar la interfaz de usuario "anterior" y "siguiente" en consecuencia. Podríamos implementamos esta lógica dentro de nuestro método de acción Index(). También podemos agregar una clase auxiliar para nuestro proyecto que encapsula esta lógica de una manera más reutilizable.

A continuación es una clase auxiliar "PaginatedList" simple que se deriva de la lista&lt;T&gt; una clase de colección integrados en .NET Framework. Implementa una clase de colección reutilizable que puede usarse para paginar cualquier secuencia de datos de IQueryable. En nuestra aplicación NerdDinner tendremos trabajar a través de IQueryable&lt;Dinner&gt; resultados, pero podría fácilmente usarse contra IQueryable&lt;producto&gt; o IQueryable&lt;cliente&gt;da como resultado en otros escenarios de aplicación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Tenga en cuenta sobre cómo calcula y, a continuación, expone propiedades como "PageIndex", "PageSize", "TotalCount" y "TotalPages". También, a continuación, expone dos propiedades de aplicación auxiliar "HasPreviousPage" y "HasNextPage" que indican si la página de datos de la colección está al principio o al final de la secuencia original. El código anterior provocará dos consultas SQL ejecutarlas: la primera para recuperar el recuento del número total de objetos de la cena (Esto no devuelve los objetos; en su lugar realiza una instrucción "SELECT COUNT" que devuelve un entero) y la segunda para recuperar solo las filas de datos que necesitamos, desde nuestra base de datos para la página de datos actual.

A continuación, podemos actualizar nuestro método de aplicación auxiliar DinnersController.Index() para crear un PaginatedList&lt;Dinner&gt; desde nuestro DinnerRepository.FindUpcomingDinners() resultado y pasarlo a nuestra plantilla de vista:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

A continuación, podemos actualizar la plantilla de vista \Views\Dinners\Index.aspx va a heredar ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; en lugar de ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;y, a continuación, agregue el código siguiente a la parte inferior de la plantilla de vista para mostrar u ocultar la interfaz de usuario de navegación anterior y siguiente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Tenga en cuenta sobre cómo estamos usando el método auxiliar Html.RouteLink() para generar nuestra hipervínculos. Este método es similar al método auxiliar Html.ActionLink() que hemos usado anteriormente. La diferencia es que se genera la dirección URL con el "UpcomingDinners" configuramos dentro de nuestro archivo Global.asax de regla de enrutamiento. Esto garantiza que vamos a generar las direcciones URL para el método de acción Index() que tienen el formato: */dinners/página / {page}* : donde el valor de {la página de} es una variable, ofrecemos anteriormente según el PageIndex actual.

Y ahora Cuando ejecutemos nuestra aplicación nuevo veremos 10 instancias dinners a la vez en el explorador:

![](implement-efficient-data-paging/_static/image5.png)

También tenemos &lt; &lt; &lt; y &gt; &gt; &gt; navegación de la interfaz de usuario en la parte inferior de la página que nos permite saltar hacia delante y hacia atrás a través de nuestros datos mediante direcciones URL accesibles de motor de búsqueda:

![](implement-efficient-data-paging/_static/image6.png)

| **Tema de lado: Entender las implicaciones de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; es una característica muy eficaz que permite una variedad de escenarios de ejecución aplazada interesantes (como paginación y la composición según las consultas). Como con todas las características eficaces, desea tener cuidado con cómo utilizarlo y asegúrese de que no está en peligro. Es importante reconocer que devolver un IQueryable&lt;T&gt; resultado desde el repositorio permite que el código que realiza la llamada anexar en los métodos de operador encadenadas al mismo y, por lo que participan en la ejecución de consultas ultimate. Si no desea proporcionar esta capacidad de código de llamada, a continuación, se debe devolver hacer una copia de IList&lt;T&gt; o IEnumerable&lt;T&gt; results - que contienen los resultados de una consulta que ya se ha ejecutado. Para escenarios de paginación esto requeriría que se va a insertar la lógica de paginación de datos reales en el método del repositorio que se llama. En este escenario podríamos Actualizamos nuestro método de buscador FindUpcomingDinners() para que tenga una firma que cualquiera devuelve un PaginatedList: PaginatedList&lt; Dinner&gt; {} FindUpcomingDinners (pageIndex int, int pageSize) o devolver un IList &lt;Dinner&gt;y utilice "totalCount" parámetro de salida para devolver el recuento total de instancias Dinners: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos agregar compatibilidad con autenticación y autorización a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Siguiente](secure-applications-using-authentication-and-authorization.md)
