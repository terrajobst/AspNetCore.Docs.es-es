---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar la paginación de datos eficaz | Documentos de Microsoft
author: microsoft
description: Paso 8 muestra cómo agregar compatibilidad con la paginación a la dirección URL de /Dinners para que en lugar de mostrar 1000s de cenas a la vez, solo se mostrará 10 cenas próximos en...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="implement-efficient-data-paging"></a>Implementar la paginación de datos eficaces
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 8 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 8 muestra cómo agregar compatibilidad con la paginación a la dirección URL de /Dinners para que en lugar de mostrar 1000s de cenas a la vez, solo podrá mostrar 10 cenas próximas a la vez - y permitir que los usuarios finales página atrás y hacia delante a través de la lista completa de una manera fácil de usar de SEO.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner paso 8: Compatibilidad con la paginación

Si el sitio se realiza correctamente, tendrá miles de cenas próximos. Es necesario para asegurarse de que la interfaz de usuario se escala para administrar todas estas cenas y permite a los usuarios examinar ellos. Para habilitar esta opción, se agregará compatibilidad con la paginación a nuestro */Dinners* dirección URL para que en lugar de mostrar 1000s de cenas al mismo tiempo, se sólo podrá mostrar 10 cenas próximas a la vez - y permitir que los usuarios finales página atrás y hacia delante a través de toda la lista en una manera fácil de usar de SEO.

### <a name="index-action-method-recap"></a>Resumen de método de acción de Index()

El método de acción de Index() dentro de nuestra clase DinnersController actualmente parece que a continuación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Cuando se realiza una solicitud a la */Dinners* de dirección URL, se recupera una lista de todos los próximos cenas y, a continuación, presenta una lista de todos ellos:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Descripción IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  es una interfaz que se introdujo con LINQ como parte de .NET 3.5. Habilita escenarios eficaz "la ejecución diferida" que se puede aprovechar para implementar la compatibilidad con la paginación.

En nuestro DinnerRepository se está devolviendo un tipo IQueryable&lt;Dinner&gt; secuencia de nuestro método FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt; objeto devuelto por el método FindUpcomingDinners() encapsula una consulta para recuperar los objetos de la cena de nuestra base de datos usando LINQ to SQL. Lo que es importante, no ejecute la consulta con la base de datos hasta que se intenta acceso/iterar por los datos en la consulta, o hasta que se llame al método ToList() en él. El código que llama a nuestro método FindUpcomingDinners() puede optar por agregar filtros de operaciones "encadenados" adicionales al IQueryable&lt;Dinner&gt; objeto antes de ejecutar la consulta. LINQ to SQL, a continuación, es lo suficientemente inteligente como para ejecutar la consulta combinada con la base de datos cuando se solicitan los datos.

Para implementar la lógica de paginación podemos actualizar método de acción de nuestro DinnersController Index() para que se aplique operadores "Skip" y "Tomar" adicionales al IQueryable devuelto&lt;Dinner&gt; secuencia antes de llamar a ToList() en él:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

El código anterior omite los 10 primeros cenas próximos en la base de datos y, a continuación, devuelve 20 cenas. LINQ to SQL es lo suficientemente inteligente como para crear una consulta optimizada de SQL que realiza esta omisión lógica en la base de datos SQL y no en el servidor web. Esto significa que, incluso si se tienen millones de cenas próximos en la base de datos, se recuperarán solo el 10 que queremos como parte de esta solicitud (para convertirla eficaz y escalable).

### <a name="adding-a-page-value-to-the-url"></a>Agregue un valor de "página" a la dirección URL

En lugar de codificar un intervalo de páginas específicas, le interesará nuestras direcciones URL para incluir un parámetro de "página" que indica el intervalo de cena solicita un usuario.

#### <a name="using-a-querystring-value"></a>Uso de un valor de cadena de consulta

El código siguiente muestra cómo podemos actualizar el método de acción de Index() para admitir un parámetro de cadena de consulta y habilitar las direcciones URL como */Dinners? página = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

El método de acción de Index() anterior tiene un parámetro llamado "página". El parámetro se declara como un entero que acepta valores null (que es el tipo int? indica). Esto significa que la */Dinners? página = 2* URL hará que un valor de "2" se pasen como el valor del parámetro. El */Dinners* dirección URL (sin un valor de cadena de consulta) hará que un valor null que se pasa.

Le estamos multiplicar el valor de página por el tamaño de página (en este caso 10 filas) para determinar cuántos cenas que omita las. Usamos el [C# "fusión" operador null (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) lo que resulta útil cuando se trabaja con tipos que aceptan valores NULL. El código anterior asigna el valor 0 de página si el parámetro de página es null.

#### <a name="using-embedded-url-values"></a>Uso de valores de dirección URL incrustada

Una alternativa al uso de un valor de cadena de consulta sería incrustar el parámetro de página dentro de la propia dirección de URL real. Por ejemplo: */Dinners/Page/2* o */cenas/2*. ASP.NET MVC incluye un potente motor de enrutamiento de dirección URL que hace más fácil admitir escenarios similar al siguiente.

Podemos registrar reglas de enrutamientos personalizadas que se asignan cualquier dirección URL o la dirección URL entrante, dar formato a cualquier método de acción o clase de controlador que deseamos. Todo lo que necesitamos tareas es abrir el archivo Global.asax dentro de nuestro proyecto:

![](implement-efficient-data-paging/_static/image2.png)

Y, a continuación, registre una nueva regla de asignación mediante el método de aplicación auxiliar de MapRoute() como la primera llamada a las rutas. MapRoute() siguiente:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Anteriormente, se está registrando una nueva regla de enrutamiento denominada "UpcomingDinners". Estamos que indica que tiene el formato de dirección URL "cenas/página / {página}", donde {page} es un valor de parámetro incrustado en la dirección URL. El tercer parámetro al método MapRoute() indica que se deben asignar las direcciones URL que coinciden con este formato para el método de acción de Index() en la clase DinnersController.

Podemos utilizar el mismo código del Index() exacto que tuvimos antes con nuestro escenario de cadena de consulta: salvo ahora el parámetro "página" depende de la dirección URL y no la cadena de consulta:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Y ahora cuando se ejecute la aplicación y escriba en */Dinners* , veremos los 10 primeros cenas próximas:

![](implement-efficient-data-paging/_static/image3.png)

Y cuando se escriba en */Dinners/Page/1* veremos la página siguiente de cenas:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Agregar la interfaz de usuario de navegación de páginas

Será el último paso para completar el escenario de paginación implementar la exploración "anterior" y "siguiente" interfaz de usuario dentro de la plantilla de vista de permitir a los usuarios omitir fácilmente los datos de la cena.

Para implementar esto correctamente, se necesitará saber el número total de cenas en la base de datos, así como el modo en muchas páginas de datos Esto se traduce en. A continuación, se necesitará calcular si el valor de "página" actualmente solicitada está al principio o al final de los datos y mostrar u ocultar la interfaz de usuario "anterior" y "siguiente" en consecuencia. Podríamos implementamos esta lógica en el método de acción Index(). También podemos agregar una clase auxiliar a nuestro proyecto que encapsula esta lógica de una manera más reutilizable.

A continuación se muestra una clase de aplicación auxiliar de "PaginatedList" simple que se deriva de la lista&lt;T&gt; una clase de colección integrada en .NET Framework. Implementa una clase de colección reutilizable que puede usarse para paginar cualquier secuencia de datos IQueryable. En nuestra aplicación NerdDinner tendremos funcione a través de IQueryable&lt;Dinner&gt; resultados, pero podría así de sencilla usar con IQueryable&lt;producto&gt; o IQueryable&lt;cliente&gt;da como resultado en otros escenarios de aplicación:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Tenga en cuenta sobre cómo calcula y, a continuación, expone propiedades como "PageIndex", "PageSize", "TotalCount" y "TotalPages". A continuación, también expone dos propiedades de aplicación auxiliar "HasPreviousPage" y "HasNextPage" que indican si la página de datos de la colección al principio o al final de la secuencia original. El código anterior producirá dos consultas SQL ejecutar: el primero que se va a recuperar el recuento del número total de objetos de la cena (Esto no devuelve los objetos; en su lugar lleva a cabo una instrucción "SELECT COUNT" que devuelve un entero) y la segunda para recuperar solo las filas de datos que se necesita de nuestra base de datos para la página actual de datos.

A continuación, podemos actualizar el método de aplicación auxiliar DinnersController.Index() para crear un PaginatedList&lt;cena&gt; de nuestro DinnerRepository.FindUpcomingDinners() como resultado y pasa a la plantilla de vista:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

A continuación, podemos actualizar la plantilla de vista de \Views\Dinners\Index.aspx a heredar ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; en lugar de ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;y, a continuación, agregue el código siguiente a la parte inferior de la plantilla de vista para mostrar u ocultar la interfaz de usuario de navegación anterior y siguiente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Tenga en cuenta sobre cómo se está usando el método de aplicación auxiliar Html.RouteLink() para generar nuestro hipervínculos. Este método es similar al método auxiliar Html.ActionLink() que hemos usado anteriormente. La diferencia es que se está generando la dirección URL mediante el enrutamiento de regla que configuramos dentro de nuestro archivo Global.asax "UpcomingDinners". Esto garantiza que se va a generar las direcciones URL a nuestro método de acción de Index() que tienen el formato: */Dinners/página / {page}* : donde el valor de {page} es una variable proporcionamos anteriormente tomando como base el PageIndex actual.

Y ahora cuando ejecutamos nuestra aplicación nuevo veremos 10 cenas a la vez en el explorador:

![](implement-efficient-data-paging/_static/image5.png)

También tenemos &lt; &lt; &lt; y &gt; &gt; &gt; navegación interfaz de usuario en la parte inferior de la página que nos permite omitir hacia delante y hacia atrás a través de nuestros datos mediante las direcciones URL accesibles de motor de búsqueda:

![](implement-efficient-data-paging/_static/image6.png)

| **Tema de lado: Entender las implicaciones de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; es una característica muy eficaz que permite una variedad de escenarios interesantes de la ejecución aplazada (como paginación y la composición según las consultas). Como con todas las características eficaces, desea tener cuidado con cómo utilizarlo y asegúrese de que no se abusa. Es importante reconocer que devolver un tipo IQueryable&lt;T&gt; resultado desde el repositorio permite que el código que realiza la llamada a anexar en los métodos de operador encadenada a él y, por lo que participan en la ejecución de consulta final. Si no desea proporcionar esta capacidad de código que realiza la llamada, debe devolver hacer copia de IList&lt;T&gt; o IEnumerable&lt;T&gt; resultados - que contienen los resultados de una consulta que ya se ha ejecutado. Para escenarios de paginación requeriría insertar la lógica de paginación de datos reales en la llamada al método repositorio. En este escenario tengamos que actualizamos nuestro método de buscador FindUpcomingDinners() para que tenga una firma que cualquiera devuelve un PaginatedList: PaginatedList&lt; Dinner&gt; {} FindUpcomingDinners (pageIndex int, int pageSize) o un valor devuelto en espera una interfaz IList &lt;Dinner&gt;y utilizar un "totalCount" parámetro de salida para devolver el número total de cenas: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Paso siguiente

Ahora veamos cómo podemos agregar compatibilidad con autenticación y autorización a nuestra aplicación.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Siguiente](secure-applications-using-authentication-and-authorization.md)
