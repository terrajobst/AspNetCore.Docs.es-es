---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Pasar datos a páginas maestras de vista (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una vista m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e70f15d98101336dbef31b4f9d8b958632e46c01
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388524"
---
<a name="passing-data-to-view-master-pages-vb"></a>Pasar datos a páginas maestras de vista (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una página maestra de la vista. En primer lugar, se describe una solución sencilla que da como resultado una aplicación que es difícil de mantener. A continuación, examinaremos una solución mucho mejor que requiere un poco más trabajo inicial, pero los resultados en una aplicación de mucho más fácil de mantener.


## <a name="passing-data-to-view-master-pages"></a>Pasar datos a las páginas maestras de vista

El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de la vista. Se examinan dos estrategias para pasar datos a una página maestra de la vista. En primer lugar, se describe una solución sencilla que da como resultado una aplicación que es difícil de mantener. A continuación, examinaremos una solución mucho mejor que requiere un poco más trabajo inicial, pero los resultados en una aplicación de mucho más fácil de mantener.

### <a name="the-problem"></a>El problema

Imagine que está creando una aplicación de base de datos de películas y desea mostrar la lista de las categorías de películas en todas las páginas en la aplicación (consulte la figura 1). Además, imagine que la lista de las categorías de películas se almacena en una tabla de base de datos. En ese caso tendría sentido para recuperar las categorías de la base de datos y presentar la lista de categorías de película dentro de una página maestra de la vista.


[![Mostrar las categorías de películas en una página maestra de la vista](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: mostrar las categorías de películas en una página de vista maestra ([haga clic aquí para ver imagen en tamaño completo](passing-data-to-view-master-pages-vb/_static/image3.png))


Éste es el problema. ¿Cómo se recuperar la lista de categorías de la película en la página principal? Es tentador para llamar a métodos de las clases de modelo en la página maestra directamente. En otras palabras, es tentador para incluir el código para recuperar los datos de la derecha de la base de datos en la página maestra. Sin embargo, omitiendo los controladores MVC para tener acceso a la base de datos infringiría la separación clara de intereses es una de las principales ventajas de la creación de una aplicación MVC.

En una aplicación MVC, desea que todas las interacciones entre las vistas MVC y el modelo MVC que va a administrar los controladores MVC. Esta separación de preocupaciones da lugar a una aplicación más fácil de mantener, adaptable y fácil de probar.

En una aplicación MVC, todos los datos que se pasa a una vista, incluida una página maestra de la vista, deben pasarse a una vista por una acción de controlador. Además, se deben pasar los datos aprovechando las ventajas de los datos de vista. En el resto de este tutorial, examinar dos métodos para pasar datos de vista a una página maestra de la vista.

### <a name="the-simple-solution"></a>La solución más sencilla

Comencemos con la solución más sencilla para pasar datos de vista de un controlador a una página maestra de la vista. La solución más sencilla es pasar los datos de vista de la página maestra en cada acción del controlador.

Tenga en cuenta el controlador en el listado 1. Expone dos acciones denominadas `Index()` y `Details()`. El `Index()` método de acción devuelve cada película en la tabla de base de datos de películas. El `Details()` método de acción devuelve cada película en una categoría determinada de película.

**Listado 1: `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Tenga en cuenta que tanto el `Index()` y `Details()` acciones de agregan dos elementos para ver los datos. El `Index()` acción agrega dos claves: categorías y películas. La lista de categorías de película aparece en la página principal de vista representado por la clave de categorías. La clave de películas representa la lista de películas que se muestra en la página de vista de índice.

El `Details()` acción también agrega dos claves denominada categorías y películas. La clave de categorías, una vez más, representa la lista de las categorías de películas que se muestra en la página principal de la vista. La clave de películas representa la lista de películas en una categoría determinada que se muestra en la página de vista de detalles (consulte la figura 2).


[![La vista de detalles](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: vista de detalles de la ([haga clic aquí para ver imagen en tamaño completo](passing-data-to-view-master-pages-vb/_static/image6.png))


La vista de índice se encuentra en el listado 2. Simplemente recorre en iteración la lista de películas representada por el elemento de películas en datos de la vista.

**Listado 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La vista de página principal se encuentra en el listado 3. La página principal de la vista recorre en iteración y representa todas las categorías de película representadas por el elemento de las categorías de datos de la vista.

**Listado 3: `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos los datos se pasa a la vista y la página principal de la vista de datos de la vista. Que es la forma correcta para pasar datos a la página maestra.

Por lo tanto, ¿qué es el problema con esta solución? El problema es que esta solución infringe el principio DRY (no repita usted mismo). Cada acción del controlador debe agregar la misma muy lista de las categorías de películas para ver los datos. Tener código duplicado en la aplicación hace que su aplicación mucho más difícil de mantener, adaptar y modificar.

### <a name="the-good-solution"></a>La mejor solución

En esta sección, examinaremos una solución alternativa y mejor, para pasar datos de una acción de controlador a una página maestra de la vista. En lugar de agregar las categorías de película de la página maestra en cada acción de controlador, agregamos las categorías de películas para ver los datos de una sola vez. Todos los datos de vista utilizados por la página principal de la vista se agrega en un controlador de aplicaciones.

La clase ApplicationController está contenida en el listado 4.

La clase ApplicationController está contenida en el listado 4.

**Listado 4: `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Hay tres cosas que debe tener en cuenta sobre el controlador de la aplicación en el listado 4. En primer lugar, tenga en cuenta que la clase hereda de la clase System.Web.Mvc.Controller base. El controlador de la aplicación es una clase de controlador.

En segundo lugar, observe que la clase de controlador de la aplicación es una clase MustInherit. Una clase MustInherit es una clase que debe implementarse mediante una clase concreta. Dado que el controlador de la aplicación es una clase MustInherit, no no se puede invocar cualquier método definido en la clase directamente. Si se intenta invocar directamente la clase de la aplicación obtendrá un mensaje de error no se encuentra el recurso.

En tercer lugar, tenga en cuenta que el controlador de la aplicación contiene un constructor que agrega la lista de las categorías de películas para ver los datos. Cada clase de controlador que se hereda desde el controlador de la aplicación llama al constructor del controlador de la aplicación automáticamente. Siempre que llame a cualquier acción en cualquier controlador que hereda desde el controlador de la aplicación, las categorías de películas se incluye automáticamente en los datos de vista.

El controlador de películas en el listado 5 se hereda desde el controlador de la aplicación.

**Listado 5: `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

El controlador de películas, igual que el controlador Home descrito en la sección anterior, expone dos métodos de acción denominados `Index()` y `Details()`. Tenga en cuenta que la lista de categorías de la película aparece en la página principal de la vista no es agregado a ver datos en el `Index()` o `Details()` método. Dado que el controlador Movies hereda desde el controlador de la aplicación, se agrega la lista de las categorías de películas para ver los datos automáticamente.

Tenga en cuenta que esta solución para agregar datos de vista de una página maestra de la vista no infringe el principio DRY (no repita usted mismo). El código para agregar la lista de las categorías de películas para ver los datos se encuentra en una única ubicación: el constructor para el controlador de la aplicación.

### <a name="summary"></a>Resumen

En este tutorial, hemos tratado dos enfoques para pasar datos de vista de un controlador a una página maestra de la vista. En primer lugar, hemos visto una sencilla, pero es difícil mantener el enfoque. En la primera sección, explicamos cómo puede agregar datos de vista de una página maestra de la vista en cada acción de cada controlador en la aplicación. Se concluye que esto era un enfoque incorrecto porque infringe el principio DRY (no repita usted mismo).

A continuación, hemos visto una mejor estrategia para agregar los datos requeridos por una página maestra de la vista para ver los datos. En lugar de agregar los datos de vista en cada acción de controlador, agregamos los datos de vista solo una vez dentro de un controlador de aplicaciones. De este modo, puede evitar código duplicado al pasar datos a una página maestra de la vista en una aplicación ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
