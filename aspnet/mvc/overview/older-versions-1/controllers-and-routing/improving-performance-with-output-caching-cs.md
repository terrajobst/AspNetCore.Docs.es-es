---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Salida de mejorar el rendimiento con almacenamiento en caché (C#) | Documentos de Microsoft
author: microsoft
description: En este tutorial, aprenderá cómo puede mejorar drásticamente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas del almacenamiento en caché de salida. El programador...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 8958caa5a0ccad669ca861bed261102625be5cb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873802"
---
<a name="improving-performance-with-output-caching-c"></a>Mejorar el rendimiento con salida de almacenamiento en caché (C#)
====================
por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá cómo puede mejorar drásticamente el rendimiento de las aplicaciones web de ASP.NET MVC aprovechando las ventajas del almacenamiento en caché de salida. Obtenga información acerca de cómo almacenar en caché el resultado devuelto de una acción de controlador para que no tenga el mismo contenido a crearse cada vez que un nuevo usuario invoca la acción.


El objetivo de este tutorial es explicar cómo puede mejorar drásticamente el rendimiento de una aplicación ASP.NET MVC aprovechando las ventajas de la caché de resultados. La caché de resultados permite almacenar en caché el contenido devuelto por una acción de controlador. De este modo, el mismo contenido no necesita generar cada vez que se invoca la misma acción de controlador.

Por ejemplo, imagine que la aplicación de ASP.NET MVC muestra una lista de registros de base de datos en una vista con el nombre de índice. Normalmente, cada vez que un usuario invoca la acción del controlador que devuelve la vista de índice, el conjunto de registros de base de datos debe recuperarse de la base de datos mediante la ejecución de una consulta de base de datos.

Si, por otro lado, sacar partido de la caché de resultados, a continuación, puede evitar la ejecución de una consulta de base de datos cada vez que cualquier usuario invoca la misma acción de controlador. La vista se puede recuperar de la caché en lugar de que se va a volver a generar desde la acción del controlador. Habilita el almacenamiento en caché para evitar realizar redundantes trabaja en el servidor.

## <a name="enabling-output-caching"></a>Habilitar caché de resultados

Habilitar caché de resultados mediante la adición de un atributo [OutputCache] para la acción de un controlador individual o una clase de controlador todo. Por ejemplo, el controlador en el listado 1 expone una acción denominada Index(). El resultado de la acción de Index() se almacena en caché durante 10 segundos.

**Lista 1 – controllers\homecontroller**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

En las versiones Beta de ASP.NET MVC, almacenamiento en caché de salida no funciona para una dirección URL como [ http://www.MySite.com/ ](http://www.mysite.com/). En su lugar, debe especificar una dirección URL como [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

En la lista 1, se almacena en caché el resultado de la acción de Index() durante 10 segundos. Si lo prefiere, puede especificar una duración mucho mayor de memoria caché. Por ejemplo, si desea almacenar en caché el resultado de una acción del controlador durante un día, a continuación, puede especificar una duración de caché de 86400 segundos (60 segundos \* 60 minutos \* 24 horas).

No hay ninguna garantía de que el contenido se almacenará en caché para la cantidad de tiempo que especifique. Cuando los recursos de memoria son bajos, la memoria caché inicia automáticamente contenido expulsar.

El controlador Home en el listado 1 devuelve la vista de índice en el listado 2. No hay nada especial acerca de esta vista. La vista de índice simplemente muestra la hora actual (consulte la figura 1).

**La lista 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Ilustración 1: vista de índice de almacenamiento en caché**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Si se invoca la acción de Index() varias veces escribiendo la URL /Home/índice en la barra de direcciones del explorador y utilizar el botón de actualización y volver a cargar en el explorador varias veces, a continuación, el tiempo que se muestra la vista de índice no cambiará durante 10 segundos. Dado que la vista se almacena en caché, se muestra el mismo tiempo.

Es importante comprender que la misma vista se almacena en caché para los usuarios que visitan su aplicación. Cualquier persona que invoca la acción de Index() obtendrá la misma versión en caché de la vista de índice. Esto significa que la cantidad de trabajo que debe realizar en el servidor web para dar servicio a la vista de índice se reduce drásticamente.

La vista en el listado 2 antes de hacer algo muy simple. La vista solo muestra la hora actual. Sin embargo, se puede realizar al igual que la caché fácilmente una vista que muestra un conjunto de registros de base de datos. En ese caso, el conjunto de registros de base de datos no necesitaría va a recuperar de la base de datos cada vez que se invoca la acción del controlador que devuelve la vista. Almacenamiento en caché puede reducir la cantidad de trabajo que deben realizar en el servidor web y el servidor de base de datos.

No use la página &lt;% @ OutputCache %&gt; directivas en una vista de MVC. Esta directiva es sangrado en el mundo de formularios Web Forms y no debe usarse en una aplicación ASP.NET MVC.

## <a name="where-content-is-cached"></a>Donde se almacena en caché de contenido

De forma predeterminada, cuando se usa el atributo [OutputCache], contenido se almacena en caché en tres ubicaciones: el servidor web, los servidores proxy y el explorador web. Puede controlar exactamente donde se haya almacenado en caché mediante la modificación de la propiedad Location del atributo [OutputCache].

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


De forma predeterminada, la propiedad Location tiene el valor de cualquier. Sin embargo, hay situaciones en las que puede caché únicamente en el explorador o en el servidor. Por ejemplo, si son almacenamiento en caché de información que está personalizado para cada usuario, a continuación, se debe almacenar en caché la información en el servidor. Si va a mostrar información diferente a distintos usuarios, a continuación, se debe almacenar en caché la información únicamente en el cliente.

Por ejemplo, el controlador en el listado 3 expone una acción denominada GetName() que devuelve el nombre de usuario actual. Si inicia sesión en el sitio Web de conector e invoca la acción GetName(), a continuación, la acción devuelve la cadena "Hi conector". Si, posteriormente, Jill inicia sesión en el sitio Web e invoca la acción GetName(), a continuación, también obtendrá la cadena "Hi conector". La cadena se almacena en caché en el servidor web para todos los usuarios después de conector inicialmente, se invoca la acción del controlador.

**Listing 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Probablemente, el controlador en el listado 3 no funciona la manera en que desea. No desea mostrar el mensaje "¡Hi conector" a Jill.

Nunca se debe almacenar en caché contenido personalizado en la caché del servidor. Sin embargo, puede almacenar en caché el contenido personalizado en la caché del explorador para mejorar el rendimiento. Si se almacenan en caché contenido en el explorador y un usuario, la misma acción de controlador invoca varias veces, se puede recuperar el contenido de la caché del explorador en lugar del servidor.

El controlador modificado en el listado 4 almacena en caché el resultado de la acción GetName(). Sin embargo, el contenido se almacena en caché únicamente en el explorador y no en el servidor. De este modo, cuando varios usuarios invocación el método GetName(), cada persona obtiene su propio nombre de usuario y no otro nombre del usuario.

**Listado 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Tenga en cuenta que el atributo [OutputCache] en el listado 4 incluye una propiedad de ubicación establecida en el valor OutputCacheLocation.Client. El atributo [OutputCache] también incluye una propiedad NoStore. La propiedad NoStore se utiliza para informar al explorador y los servidores proxy que no debería almacenar una copia permanente del contenido almacenado en caché.

## <a name="varying-the-output-cache"></a>Variar la caché de resultados

En algunas situaciones, podría interesar distintas versiones en caché del mismo contenido. Por ejemplo, imagine que está creando una página principal-detalle. La página maestra muestra una lista de los títulos de películas. Al hacer clic en un título, obtener detalles de la película seleccionada.

Si almacena en caché la página de detalles, a continuación, los detalles de la misma película se mostrará con independencia de qué película haga clic en. Se mostrará la primera película seleccionada por el primer usuario para todos los usuarios futuras.

Puede corregir este problema aprovechando las ventajas de la propiedad VaryByParam del atributo [OutputCache]. Esta propiedad le permite crear diferentes versiones en caché del mismo contenido cuando un parámetro de formulario o parámetro de cadena de consulta varía.

Por ejemplo, el controlador en el listado 5 expone dos acciones denominadas Master() y Details(). La acción Master() devuelve una lista de los títulos de películas y la acción Details() devuelve los detalles de la película seleccionada.

**Listado 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

La acción Master() incluye una propiedad VaryByParam con el valor "none". Se devuelve cuando se visualiza el Master() se invoca la acción, la misma versión en caché del maestro. Los parámetros del formulario o la cadena de consulta son parámetros omite (consulte la figura 2).

**Figura 2: la vista de /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Figura 3: la vista de detalles/películas /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

La acción de Details() incluye una propiedad VaryByParam con el valor "Id". Cuando distintos valores del parámetro de identificador se pasa a la acción de controlador, se generan diferentes versiones en caché de la vista de detalles.

Es importante entender que utilizando los resultados de la propiedad VaryByParam en más de almacenamiento en caché y no menor. Se crea una versión en caché diferente de la vista de detalles para cada versión del parámetro de identificador.

Puede establecer la propiedad VaryByParam en los siguientes valores:

> \* = Crear una versión en caché diferente cada vez que varía de un parámetro de cadena de consulta o formulario.
> 
> None = nunca crear distintas versiones en caché
> 
> Lista de puntos y coma de parámetros = crear distintas versiones en caché cada vez que alguno de los parámetros de cadena de consulta o formulario en la lista varía


## <a name="creating-a-cache-profile"></a>Crear un perfil de caché

Como alternativa a la configuración de las propiedades de la memoria caché de salida modificando las propiedades del atributo [OutputCache], puede crear un perfil de caché en el archivo de configuración (web.config) de la web. Crear un perfil de caché en el archivo de configuración de web ofrece dos ventajas importantes.

En primer lugar, mediante la configuración de caché de resultados en el archivo de configuración web, puede controlar cómo las acciones de controlador almacenar en caché contenido en una ubicación central. Puede crear un perfil de caché y aplicar el perfil a varios controladores o las acciones de controlador.

En segundo lugar, puede modificar el archivo de configuración web sin volver a compilar la aplicación. Si tiene que deshabilitar el almacenamiento en caché para una aplicación que ya se ha implementado en producción, simplemente puede modificar los perfiles de caché que se define en el archivo de configuración web. Se detectarán automáticamente y aplica los cambios realizados en el archivo de configuración web.

Por ejemplo, el &lt;almacenamiento en caché&gt; sección de configuración de web en el listado 6 define un perfil de caché con nombre Cache1Hour. El &lt;almacenamiento en caché&gt; sección debe aparecer dentro de la &lt;system.web&gt; sección del archivo de configuración web.

**Enumerar 6: almacenamiento en caché de sección de web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

El controlador en la lista 7 muestra cómo puede aplicar el perfil de Cache1Hour a una acción de controlador con el atributo [OutputCache].

**Listado 7: Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Si se invoca la acción de Index() expuesta por el controlador en la lista 7 se devolverá el mismo tiempo durante una hora.

## <a name="summary"></a>Resumen

Caché de resultados, proporciona un método muy fácil de mejorar drásticamente el rendimiento de las aplicaciones de ASP.NET MVC. En este tutorial, aprendió a utilizar el atributo [OutputCache] para almacenar en caché el resultado de las acciones de controlador. También ha aprendido cómo modificar las propiedades del atributo [OutputCache] como las propiedades Duration y VaryByParam para modificar cómo se almacena en caché de contenido. Por último, aprendió cómo definir perfiles de memoria caché en el archivo de configuración web.

> [!div class="step-by-step"]
> [Anterior](understanding-action-filters-cs.md)
> [Siguiente](adding-dynamic-content-to-a-cached-page-cs.md)
