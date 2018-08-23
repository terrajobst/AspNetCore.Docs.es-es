---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Mejora del rendimiento con salida de almacenamiento en caché (VB) | Microsoft Docs
author: microsoft
description: En este tutorial, obtendrá información sobre cómo puede mejorar considerablemente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas de la caché de resultados. Que...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 9402d7e053ef11eeefa92d112b05ec255d5ec6f7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836080"
---
<a name="improving-performance-with-output-caching-vb"></a>Mejora del rendimiento con el almacenamiento en caché (VB)
====================
por [Microsoft](https://github.com/microsoft)

> En este tutorial, obtendrá información sobre cómo puede mejorar considerablemente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas de la caché de resultados. Aprenda a almacenar en caché el resultado devuelto de una acción de controlador para que no tenga el mismo contenido a crearse cada vez que un nuevo usuario invoca la acción.


El objetivo de este tutorial es explicar cómo puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC aprovechando las ventajas de la caché de resultados. La caché de resultados le permite almacenar en caché el contenido devuelto por una acción de controlador. De este modo, no necesita el mismo contenido va a generar cada vez que se invoca la misma acción de controlador.

Por ejemplo, imagine que su aplicación ASP.NET MVC muestra una lista de registros de base de datos en una vista llamada Index. Normalmente, cada vez que un usuario invoca la acción del controlador que devuelve la vista de índice, el conjunto de registros de base de datos se debe recuperar de la base de datos mediante la ejecución de una consulta de base de datos.

Si, por otro lado, aprovechar las ventajas de la caché de resultados, a continuación, no puede ejecutar una consulta de base de datos cada vez que cualquier usuario invoca la misma acción de controlador. La vista se puede recuperar de la memoria caché en lugar de regeneración de la acción del controlador. Almacenamiento en caché permite evitar la ejecución redundante trabaja en el servidor.

#### <a name="enabling-output-caching"></a>Habilitación de la caché de resultados

Habilitar almacenamiento en caché de salida mediante la adición de un &lt;OutputCache&gt; atributo a una acción de controlador individuales o una clase de controlador completo. Por ejemplo, el controlador en el listado 1 expone una acción denominada Index(). El resultado de la acción de Index() se almacena en caché durante 10 segundos.

**Listado 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


En las versiones Beta de ASP.NET MVC, la caché de resultados no funciona para una dirección URL como [ http://www.MySite.com/ ](http://www.mysite.com/). En su lugar, debe especificar una dirección URL como [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).


En el listado 1, se almacena en caché el resultado de la acción de Index() durante 10 segundos. Si lo prefiere, puede especificar una duración mucho mayor de la memoria caché. Por ejemplo, si desea almacenar en caché el resultado de una acción del controlador durante un día, a continuación, puede especificar una duración de caché de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

No hay ninguna garantía de que el contenido se almacenará en caché para la cantidad de tiempo que especifique. Cuando los recursos de memoria son bajos, la memoria caché inicia la expulsar contenido automáticamente.

El controlador Home en el listado 1 devuelve la vista de índice en el listado 2. No hay nada especial acerca de esta vista. La vista de índice simplemente muestra la hora actual (consulte la figura 1).

**Listado 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Ilustración 1: vista de índice de almacenamiento en caché**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Si se invoca la acción de Index() varias veces, escriba/Home de dirección URL/índice en la barra de direcciones del explorador y presionar el botón de actualización y volver a cargar en el explorador varias veces y, después, la hora mostrada por la vista de índice no cambiará durante 10 segundos. Se muestra al mismo tiempo porque la vista se almacena en caché.

Es importante comprender que la misma vista se almacena en caché para los usuarios que visitan su aplicación. Cualquier persona que invoca la acción de Index() obtendrá la misma versión en caché de la vista de índice. Esto significa que la cantidad de trabajo que el servidor web debe realizar para dar servicio a la vista de índice se reduce drásticamente.

La vista en el listado 2 ocurre a hacer algo realmente sencillo. La vista solo muestra la hora actual. Sin embargo, podría simplemente como caché fácilmente una vista que muestra un conjunto de registros de base de datos. En ese caso, el conjunto de registros de base de datos no necesitaría que deben recuperarse de la base de datos cada vez que se invoca la acción del controlador que devuelve la vista. Almacenamiento en caché puede reducir la cantidad de trabajo que deben llevar a cabo el servidor web y el servidor de base de datos.

No use la página &lt;% @ OutputCache %&gt; la directiva en una vista de MVC. Esta directiva es hemorragia a través del mundo de formularios Web Forms y no debe usarse en una aplicación ASP.NET MVC. 

#### <a name="where-content-is-cached"></a>Donde se almacena en caché de contenido

De forma predeterminada, cuando se usa el &lt;OutputCache&gt; atributo, el contenido se almacena en caché en tres ubicaciones: el servidor web, los servidores proxy y el explorador web. Puede controlar exactamente dónde haya almacenado en caché mediante la modificación de la propiedad Location de la &lt;OutputCache&gt; atributo.

Puede establecer la propiedad Location en cualquiera de los siguientes valores:

> · Cualquier
> 
> · Cliente
> 
> · Nivel inferior
> 
> · Servidor
> 
> · Ninguno
> 
> · ServerAndClient


De forma predeterminada, la propiedad de ubicación tiene el valor Any. Sin embargo, hay situaciones en que desee solo en el explorador o solo en el servidor de caché. Por ejemplo, si se almacenan en caché información personalizada para cada usuario, a continuación, se debe no almacenar en caché la información en el servidor. Si va a mostrar información diferente a distintos usuarios, a continuación, se debe almacenar en caché la información únicamente en el cliente.

Por ejemplo, el controlador en el listado 3 expone una acción denominada GetName() que devuelve el nombre de usuario actual. Si Jack inicia sesión en el sitio Web e invoca la acción GetName(), a continuación, la acción devuelve la cadena "Jack Hi". Si, posteriormente, Jill inicia sesión en el sitio Web e invoca la acción GetName(), a continuación, también obtendrá la cadena "Jack Hi". La cadena se almacena en caché en el servidor web para todos los usuarios después de Jack inicialmente invoca la acción del controlador.

**Listado 3 – Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Probablemente, el controlador en el listado 3 no funciona la forma que desee. No desea que aparezca el mensaje "¡Hi Jack" a Jill.

Nunca se debe almacenar en caché contenido personalizado en la caché del servidor. Sin embargo, es posible que desee almacenar en caché el contenido personalizado en la caché del explorador para mejorar el rendimiento. Si almacenar en caché contenido en el explorador y un usuario invoca la acción del controlador mismo varias veces, se puede recuperar el contenido desde la caché del explorador en lugar del servidor.

El controlador modificado en el listado 4 se almacena en caché el resultado de la acción GetName(). Sin embargo, el contenido se almacena en caché solo en el explorador y no en el servidor. De este modo, cuando varios usuarios invocan el método GetName(), cada persona obtiene su propio nombre de usuario y no otro nombre del usuario.

**Listado 4 – Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Tenga en cuenta que el &lt;OutputCache&gt; atributo en el listado 4 incluye una propiedad de ubicación establecida en el valor OutputCacheLocation.Client. El &lt;OutputCache&gt; atributo también incluye una propiedad NoStore. La propiedad NoStore se usa para informar a los exploradores y servidores proxy que no debe almacenar una copia permanente del contenido almacenado en caché.

#### <a name="varying-the-output-cache"></a>Variar la memoria caché de salida

En algunas situaciones, es posible que desee distintas versiones en caché del mismo contenido. Por ejemplo, imagine que va a crear una página de maestro y detalles. La página principal muestra una lista de títulos de películas. Al hacer clic en un título, obtener detalles de la película seleccionada.

Si almacena en caché la página de detalles, a continuación, los detalles de la misma película aparecerá con independencia de qué película haga clic en. La primera película seleccionada por el primer usuario se mostrará para todos los usuarios futuros.

Puede corregir este problema aprovechando las ventajas de la propiedad VaryByParam de la &lt;OutputCache&gt; atributo. Esta propiedad le permite crear diferentes versiones en caché del mismo contenido cuando un parámetro de formulario o parámetro de cadena de consulta varía.

Por ejemplo, el controlador en el listado 5 expone dos acciones denominadas Master() y Details(). La acción Master() devuelve una lista de títulos de películas y la acción Details() devuelve los detalles de la película seleccionada.

**Listado 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

La acción Master() incluye una propiedad VaryByParam con el valor "none". Se devuelve cuando se visualiza el Master() se invoca la acción, la misma versión en caché del patrón. Los parámetros de formulario o la cadena de consulta son parámetros omite (consulte la figura 2).

**Ilustración 2: la vista /Movies/Master**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Ilustración 3: la vista de detalles/Movies /**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

La acción Details() incluye una propiedad VaryByParam con el valor "Id". Cuando los distintos valores del parámetro del identificador se pasan a la acción del controlador, se generan diferentes versiones en caché de la vista de detalles.

Es importante entender que utilizando los resultados de la propiedad VaryByParam en más de almacenamiento en caché y no inferior. Se crea una versión almacenada en caché diferente de la vista de detalles para cada versión del parámetro del identificador.

Puede establecer la propiedad VaryByParam en los siguientes valores:

> \* = Crear una versión almacenada en caché diferente cada vez que varía de un parámetro de cadena de consulta o formulario.
> 
> None = nunca crear diferentes versiones en caché
> 
> Lista de puntos y comas de parámetros = crear diferentes versiones en caché siempre que cualquiera de los parámetros de cadena de consulta o formulario en la lista varía


#### <a name="creating-a-cache-profile"></a>Crear un perfil de caché

Como alternativa a la configuración de las propiedades de la caché de salida modificando las propiedades de la &lt;OutputCache&gt; atributo, puede crear un perfil de caché en el archivo de configuración (web.config) de la web. Crear un perfil de caché en el archivo de configuración web ofrece dos ventajas importantes.

En primer lugar, mediante la configuración de caché de resultados en el archivo de configuración web, puede controlar cómo almacenar las acciones de controlador en caché contenido en una ubicación central. Puede crear un perfil de caché y aplicar el perfil a varios controladores o acciones de controlador.

En segundo lugar, puede modificar el archivo de configuración web sin volver a compilar la aplicación. Si tiene que deshabilitar el almacenamiento en caché para una aplicación que ya se ha implementado en producción, simplemente puede modificar los perfiles de caché definidos en el archivo de configuración web. Se detectarán automáticamente y aplica los cambios realizados en el archivo de configuración web.

Por ejemplo, el &lt;almacenamiento en caché&gt; sección de configuración web en el listado 6 define un perfil de caché denominado Cache1Hour. El &lt;almacenamiento en caché&gt; sección debe aparecer dentro de la &lt;system.web&gt; sección de un archivo de configuración web.

**Listado 6: almacenamiento en caché de sección de web.config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

El controlador en la lista 7 muestra cómo puede aplicar el perfil Cache1Hour a una acción de controlador con el &lt;OutputCache&gt; atributo.

**Listado 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Si se invoca la acción de Index() expuesta por el controlador en la lista 7 se devolverá el mismo tiempo durante una hora.

#### <a name="summary"></a>Resumen

Almacenamiento en caché de salida proporciona un método muy fácil de lo que mejora notablemente el rendimiento de las aplicaciones de ASP.NET MVC. En este tutorial, ha aprendido a usar el &lt;OutputCache&gt; atributo para almacenar en caché el resultado de las acciones de controlador. También ha aprendido cómo modificar las propiedades de la &lt;OutputCache&gt; atributo como las propiedades de duración y VaryByParam para modificar cómo se almacena en caché de contenido. Por último, ha aprendido a definir perfiles de caché en el archivo de configuración web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-vb.md)
> [Siguiente](adding-dynamic-content-to-a-cached-page-vb.md)
