---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Almacenamiento en caché de datos en la arquitectura (C#) | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior hemos aprendido a aplicar el almacenamiento en caché en la capa de presentación. En este tutorial se aprenderá cómo aprovechar las ventajas de nuestro architectu capas...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 80805bae14654d6817328232453031384ceadad6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820463"
---
<a name="caching-data-in-the-architecture-c"></a>Datos en caché en la arquitectura (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) o [descargar PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> En el tutorial anterior hemos aprendido a aplicar el almacenamiento en caché en la capa de presentación. En este tutorial, aprenderá cómo aprovechar las ventajas de nuestra arquitectura por capas para almacenar datos en caché de la capa de lógica empresarial. Para hacerlo mediante la extensión de la arquitectura para incluir una capa de almacenamiento en caché.


## <a name="introduction"></a>Introducción

Como hemos visto en el tutorial anterior, el almacenamiento en caché los datos de origen ObjectDataSource s es tan sencillo como configurar un par de propiedades. Por desgracia, ObjectDataSource aplica el almacenamiento en caché en la capa de presentación, que se acopla estrechamente con las directivas de almacenamiento en caché con la página ASP.NET. Uno de los motivos para la creación de una arquitectura por capas es permitirle tales acoplamientos interrumpir. La capa de lógica empresarial, por ejemplo, separa la lógica de negocios desde las páginas ASP.NET, mientras que la capa de acceso a datos desacopla los detalles de acceso a datos. Esta separación de los detalles de acceso de datos y la lógica empresarial es preferida, en parte, porque hace que el sistema sea más legible, más fácil de mantener y más flexible para cambiar. También se permite para la división de mano de obra de un desarrollador que trabaja en la capa de presentación t debe estar familiarizado con los detalles de s de la base de datos con el fin de hacer su trabajo y conocimiento del dominio. Desacoplamiento de la directiva de caché de la capa de presentación ofrece ventajas similares.

En este tutorial aumentaremos nuestra arquitectura debe incluir un *capa de almacenamiento en caché* (o CL para abreviar) que emplea nuestra directiva de almacenamiento en caché. La capa de almacenamiento en caché incluirá un `ProductsCL` clase que proporciona acceso a la información de producto con métodos como `GetProducts()`, `GetProductsByCategoryID(categoryID)`, y así sucesivamente, que, cuando se invoca, le primer intento para recuperar los datos de la memoria caché. Si la memoria caché está vacía, estos métodos se invocarán adecuado `ProductsBLL` método BLL, que se obtendría a su vez los datos de la capa DAL. El `ProductsCL` métodos almacenar en caché los datos recuperados de BLL antes de devolverlo.

Como se muestra en la figura 1, la CL se encuentra entre la presentación y capas de lógica de negocios.


![El almacenamiento en caché de capa (CL) es otra capa en nuestra arquitectura](caching-data-in-the-architecture-cs/_static/image1.png)

**Figura 1**: capa de almacenamiento en caché el (CL) es otra capa en nuestra arquitectura


## <a name="step-1-creating-the-caching-layer-classes"></a>Paso 1: Creación de las clases de capa de almacenamiento en caché

En este tutorial crearemos un CL muy simple con una sola clase `ProductsCL` que tiene solo un puñado de métodos. Creación de una capa de almacenamiento en caché completa para toda la aplicación necesitaría crear `CategoriesCL`, `EmployeesCL`, y `SuppliersCL` clases y proporcionar un método en estas clases de la capa de almacenamiento en caché para cada método de acceso o la modificación de datos en la capa BLL. Al igual que con las BLL y DAL, la capa de almacenamiento en caché lo ideal es que debe implementarse como un proyecto de biblioteca de clases independiente; Sin embargo, se implementará como una clase en el `App_Code` carpeta.

Para clases independientes claramente la CL más de las clases de DAL y BLL, s permiten crear una nueva subcarpeta en el `App_Code` carpeta. Haga doble clic en el `App_Code` carpeta en el Explorador de soluciones, elija la nueva carpeta y el nombre de la nueva carpeta `CL`. Después de crear esta carpeta, agregar a ella, una nueva clase denominada `ProductsCL.cs`.


![Agregar una carpeta nueva denominada CL y una clase denominada ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figura 2**: agregar una carpeta nueva denominada `CL` y una clase denominada `ProductsCL.cs`


El `ProductsCL` clase debe incluir el mismo conjunto de métodos de acceso y de modificación de datos que se encuentra en su clase de capa de lógica empresarial correspondiente (`ProductsBLL`). En lugar de crear todos estos métodos, intente crear de let s aquí un par para hacerse una idea de los patrones utilizados por la CL. En concreto, vamos a agregar el `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos en el paso 3 y un `UpdateProduct` sobrecarga en el paso 4. Puede agregar el resto `ProductsCL` métodos y `CategoriesCL`, `EmployeesCL`, y `SuppliersCL` clases en su tiempo libre.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Paso 2: Lectura y escritura en la caché de datos

ObjectDataSource característica explorado internamente en el tutorial anterior, usa la caché de datos ASP.NET para almacenar los datos recuperados de BLL. La caché de datos también se puede acceder mediante programación desde las clases de código subyacente de las páginas ASP.NET o desde las clases en la arquitectura de s aplicación web. Para leer y escribir en la caché de datos de una clase de código subyacente de s de página ASP.NET, utilice el siguiente patrón:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

El [ `Cache` clase](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` método](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) tiene un número de sobrecargas. `Cache["key"] = value` y `Cache.Insert(key, value)` son sinónimos y ambos agregan un elemento a la memoria caché utilizando la clave especificada sin una expiración definido. Normalmente, desea especificar una fecha de expiración al agregar un elemento a la caché, ya sea como una dependencia, una basada en tiempo de expiración o ambos. Utilice uno de los otros `Insert` sobrecargas del método s para proporcionar información de dependencia o basado en tiempo de caducidad.

La capa de almacenamiento en caché s métodos deben comprobar primero si los datos solicitados se están en la memoria caché y, si es así, vuelve a partir de ahí. Si los datos solicitados no están en la memoria caché, debe invocarse el método BLL adecuado. Su valor devuelto se debe almacenar en caché y, a continuación, se devuelve, como se muestra en el siguiente diagrama de secuencia.


![Los métodos de capa de almacenamiento en caché s devuelven los datos de la memoria caché si lo s disponible](caching-data-in-the-architecture-cs/_static/image3.png)

**Figura 3**: métodos de la capa de almacenamiento en caché s devuelven los datos de la memoria caché si lo s disponible


La secuencia que se muestra en la figura 3 se realiza en las clases de CL con el siguiente patrón:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

En este caso, *tipo* es el tipo de datos que se almacenan en la memoria caché `Northwind.ProductsDataTable`, por ejemplo mientras *clave* es la clave que identifica el elemento de caché. Si el elemento con los valores especificados *clave* no está en la memoria caché, a continuación, *instancia* será `null` y los datos se recuperan desde el método BLL adecuado y se agregará a la memoria caché. En el momento `return instance` se alcanza, *instancia* contiene una referencia a los datos, ya sea desde la memoria caché o extraído de la capa BLL.

Asegúrese de usar el patrón anterior al obtener acceso a datos de la memoria caché. El siguiente patrón, que, a primera vista, parece equivalente, contiene una diferencia sutil que presenta una condición de carrera. Las condiciones de carrera son difíciles de depurar porque revelarse a sí mismos esporádicamente y son difíciles de reproducir.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

La diferencia en este segundo fragmento de código incorrecto es que en lugar de almacenar una referencia al elemento almacenado en caché en una variable local, la caché de datos se accede directamente en la instrucción condicional *y* en el `return`. Imagine que cuando se alcanza este código, `Cache["key"]` no es`null`, pero antes la `return` se alcanza la instrucción, el sistema desaloja *clave* desde la memoria caché. En este caso excepcional, el código devolverá una `null` valor en lugar de un objeto del tipo esperado.

> [!NOTE]
> La caché de datos es segura para subprocesos, para que no la necesidad de t para sincronizar el acceso de subproceso para simple lecturas o escrituras. Sin embargo, si necesita realizar varias operaciones en los datos en la memoria caché que deben ser atómicas, es responsable de implementar un bloqueo o algún otro mecanismo para garantizar la seguridad para subprocesos. Consulte [sincronizar el acceso a la memoria caché de ASP.NET](http://www.ddj.com/184406369) para obtener más información.


Puede expulsar mediante programación un elemento de la caché de datos mediante el [ `Remove` método](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) así:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Paso 3: Devolver información del producto desde la`ProductsCL`clase

Para este tutorial permite s implementar dos métodos para devolver información del producto desde la `ProductsCL` clase: `GetProducts()` y `GetProductsByCategoryID(categoryID)`. Al igual que con el `ProductsBL` clase en la capa de lógica empresarial, el `GetProducts()` método en la CL devuelve información acerca de todos los productos como un `Northwind.ProductsDataTable` objeto, mientras que `GetProductsByCategoryID(categoryID)` devuelve todos los productos de una categoría especificada.

El código siguiente muestra una parte de los métodos de la `ProductsCL` clase:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

En primer lugar, tenga en cuenta la `DataObject` y `DataObjectMethodAttribute` atributos aplicados a las clases y métodos. Estos atributos proporcionan información para el ObjectDataSource wizard s, que indica qué clases y métodos deben aparecer en los pasos del asistente s. Puesto que los métodos y clases de CL se tendrá acceso desde un origen ObjectDataSource en la capa de presentación, he agregado estos atributos para mejorar la experiencia en tiempo de diseño. Vuelva a consultar el [crear una capa de lógica empresarial](../introduction/creating-a-business-logic-layer-cs.md) tutorial para obtener una descripción más exhaustiva sobre estos atributos y sus efectos.

En el `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos, los datos devueltos por la `GetCacheItem(key)` método se asigna a una variable local. El `GetCacheItem(key)` método, que se examinarán en breve, devuelve un elemento determinado de la memoria caché según lo especificado *clave*. Si no se encuentran ningún dichos datos en memoria caché, se recupera de la correspondiente `ProductsBLL` método de clase y, a continuación, se agrega a la caché mediante el `AddCacheItem(key, value)` método.

El `GetCacheItem(key)` y `AddCacheItem(key, value)` métodos de interfaz con la memoria caché de datos, leer y escribir valores, respectivamente. El `GetCacheItem(key)` método es más sencillo de los dos. Simplemente devuelve el valor de la clase de caché con el pasar de *clave*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` no utiliza *clave* valor tal como lo suministró, sino más bien llamadas la `GetCacheKey(key)` método, que devuelve el *clave* van precedidas de ProductsCache-. El `MasterCacheKeyArray`, que contiene la cadena ProductsCache, también se usa por la `AddCacheItem(key, value)` método, como veremos en breve.

De una clase de código subyacente ASP.NET página s, se puede acceder la caché de datos mediante el `Page` clase s [ `Cache` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)y permite una sintaxis similar a `Cache["key"] = value`, como se describe en el paso 2. Desde una clase dentro de la arquitectura, la caché de datos puede obtenerse mediante `HttpRuntime.Cache` o `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)de entrada de blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) notas de la ventaja de ligeramente el rendimiento de utilizar `HttpRuntime` en lugar de `HttpContext.Current`; por lo tanto, `ProductsCL` usa `HttpRuntime`.

> [!NOTE]
> Si la arquitectura se implementa mediante proyectos de biblioteca de clases, a continuación, deberá agregar una referencia a la `System.Web` ensamblado para poder usar el [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) y [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) clases.


Si el elemento no se encuentra en la memoria caché, el `ProductsCL` métodos de la clase s obtener los datos de la capa BLL y agréguelo a la caché mediante el `AddCacheItem(key, value)` método. Para agregar *valor* podríamos usar el código siguiente, que utiliza una expiración de tiempo de 60 segundos a la memoria caché:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Especifica la expiración basada en tiempo de 60 segundos en el futuro mientras [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) no indica que existen s ninguna fecha de expiración variable. Mientras este `Insert` sobrecarga del método tiene parámetros para ambos absoluta de entrada y el plazo de expiración, solo puede proporcionar uno de los dos. Si intenta especificar un tiempo absoluto y un intervalo de tiempo, el `Insert` método producirá una `ArgumentException` excepción.

> [!NOTE]
> Esta implementación de la `AddCacheItem(key, value)` método actualmente tiene algunas limitaciones. Comenzaremos abordar y solucionar estos problemas en el paso 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Paso 4: Invalidar la memoria caché cuando los datos es modificado a través de la arquitectura

Junto con los métodos de recuperación de datos, debe proporcionar los mismos métodos que BLL para insertar, actualizar y eliminar datos de la capa de almacenamiento en caché. Los métodos de modificación de datos de CL s no modifican los datos en caché, pero en su lugar se llame al método de modificación de datos correspondiente de BLL s y, a continuación, invalidar la memoria caché. Como hemos visto en el tutorial anterior, este comportamiento es el mismo que el origen ObjectDataSource se aplica cuando se habilitan las características de almacenamiento en caché y su `Insert`, `Update`, o `Delete` los métodos se invocan.

La siguiente `UpdateProduct` sobrecarga ilustra cómo implementar los métodos de modificación de datos en la CL:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Se invoca la modificación de datos correspondiente método de la capa de lógica empresarial, pero antes de devolver su respuesta es necesario invalidar la memoria caché. Desafortunadamente, lo que invalida la memoria caché no es sencillo porque la `ProductsCL` clase s `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos cada agregan elementos a la memoria caché con claves diferentes y el `GetProductsByCategoryID(categoryID)` método agrega un elemento de caché diferente para cada uno único *categoryID*.

Al invalidar la memoria caché, es necesario quitar *todas* de los elementos que se han agregado por el `ProductsCL` clase. Esto puede realizarse mediante la asociación de un *la dependencia de caché* con cada elemento agregado a la memoria caché en el `AddCacheItem(key, value)` método. En general, una dependencia de caché puede ser otro elemento en la memoria caché, un archivo en el sistema de archivos o datos de una base de datos de Microsoft SQL Server. Cuando la dependencia cambia o se quita de la memoria caché, los elementos de la memoria caché está asociado automáticamente se expulsan de la memoria caché. Para este tutorial, queremos crear un elemento adicional en la memoria caché que actúa como una dependencia de caché para todos los elementos agregados a través del `ProductsCL` clase. De este modo, todos estos elementos pueden quitarse de la memoria caché quitando la dependencia de caché.

Actualización de s Let el `AddCacheItem(key, value)` está asociado con una dependencia de caché único método para que cada elemento se agrega a la memoria caché a través de este método:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` es una matriz de cadenas que contiene un valor único, ProductsCache. En primer lugar, un elemento de caché se agrega a la memoria caché y asigna la fecha y hora actuales. Si ya existe un elemento de la caché, se actualiza. A continuación, se crea una dependencia de caché. El [ `CacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) constructor s tiene un número de sobrecargas, pero lo que se usan aquí espera dos `string` entradas de la matriz. La primera de ellas especifica el conjunto de archivos que se usará como dependencias. Puesto que no queremos t desea utilizar las dependencias basadas en archivos, un valor de `null` se usa para el primer parámetro de entrada. El segundo parámetro de entrada especifica el conjunto de claves de caché que se usarán como dependencias. Aquí especificamos nuestra dependencia única, `MasterCacheKeyArray`. El `CacheDependency` , a continuación, se pasa a la `Insert` método.

Con esta modificación `AddCacheItem(key, value)`, invaliding la memoria caché es tan sencilla como eliminar la dependencia.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Paso 5: Llamar a la capa de almacenamiento en caché desde la capa de presentación

Los métodos y clases de s de capa de almacenamiento en caché pueden usarse para trabajar con datos mediante las técnicas se ve examina a lo largo de estos tutoriales. Para ilustrar cómo trabajar con datos almacenados en caché, guardar los cambios realizados en el `ProductsCL` clase y, a continuación, abra el `FromTheArchitecture.aspx` página en el `Caching` carpeta y agregue un control GridView. En la etiqueta inteligente s GridView, cree un nuevo origen ObjectDataSource. En el paso del asistente s primero debería ver el `ProductsCL` de clase como una de las opciones de la lista desplegable.


[![La clase ProductsCL se incluye en la lista desplegable de objeto de negocio](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figura 4**: el `ProductsCL` clase se incluye en la lista desplegable de objeto de negocio ([haga clic aquí para ver imagen en tamaño completo](caching-data-in-the-architecture-cs/_static/image6.png))


Después de seleccionar `ProductsCL`, haga clic en siguiente. La lista desplegable en la ficha Seleccionar tiene dos elementos - `GetProducts()` y `GetProductsByCategoryID(categoryID)` y la pestaña de actualización tiene el mero `UpdateProduct` sobrecargar. Elija la `GetProducts()` método desde la ficha Seleccionar y `UpdateProducts` método desde la pestaña de la actualización y haga clic en Finalizar.


[![Se enumeran los métodos de la clase ProductsCL s en el cuadro desplegable enumera](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figura 5**: el `ProductsCL` en las listas de la lista desplegable se enumeran los métodos de clase s ([haga clic aquí para ver imagen en tamaño completo](caching-data-in-the-architecture-cs/_static/image9.png))


Después de completar el asistente, Visual Studio establecerá la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}` y agregue los campos correspondientes en el control GridView. Cambiar el `OldValuesParameterFormatString` propiedad a su valor predeterminado, `{0}`y configurar el control GridView para admitir la paginación, ordenación y edición. Puesto que el `UploadProducts` sobrecarga utilizada por el CL acepta únicamente el nombre de producto modificada s y precio, limitar el control GridView para que solo estos campos son editables.

En el tutorial anterior, hemos definido una clase GridView para incluir campos para el `ProductName`, `CategoryName`, y `UnitPrice` campos. No dude en replicar este formato y la estructura, en cuyo caso la s GridView y ObjectDataSource declarativa marcado debe ser similar al siguiente:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

En este momento tenemos una página que usa la capa de almacenamiento en caché. Para ver la memoria caché en acción, establecer puntos de interrupción el `ProductsCL` clase s `GetProducts()` y `UpdateProduct` métodos. Visite la página en un explorador y recorrer el código cuando la ordenación y paginación con el fin de ver los datos extraen de la memoria caché. A continuación, actualizar un registro y observe que invalida la memoria caché y, por lo tanto, se recupera de la capa BLL cuando se vuelve a enlazar los datos en GridView.

> [!NOTE]
> La capa de almacenamiento en caché proporcionada en la descarga que acompaña este artículo no está completa. Contiene solo una clase `ProductsCL`, que sólo dispone de una serie de métodos. Además, una sola página ASP.NET usa el CL (`~/Caching/FromTheArchitecture.aspx`) todos los demás aún hacen referencia a la capa BLL directamente. Si planea usar un CL en la aplicación, todas las llamadas desde la capa de presentación deben ir a la CL, lo que requeriría que las clases de CL s y los métodos que tratan esas clases y métodos en la capa BLL utilizado actualmente por la capa de presentación.


## <a name="summary"></a>Resumen

Aunque el almacenamiento en caché se puede aplicar en la capa de presentación con ASP.NET 2.0 s SqlDataSource y ObjectDataSource controles, lo ideal es que las responsabilidades de almacenamiento en caché podría delegarse a un nivel independiente en la arquitectura. En este tutorial, creamos una capa de almacenamiento en caché que se encuentra entre la capa de presentación y la capa de lógica empresarial. La capa de almacenamiento en caché debe proporcionar el mismo conjunto de clases y métodos que existen en el nivel de lógica empresarial y se llaman desde la capa de presentación.

Los ejemplos de la capa de almacenamiento en caché, analizamos en este y los tutoriales anteriores se mostró *carga reactiva*. La carga reactiva, los datos se cargan en la memoria caché solo cuando se realiza una solicitud para los datos y falta que los datos de la memoria caché. También pueden ser datos *proactivamente cargado* en la memoria caché, una técnica que carga los datos en la memoria caché antes de que realmente es necesario. En el siguiente tutorial, veremos un ejemplo de carga automático cuando nos centramos en cómo almacenar valores estáticos en la memoria caché al iniciarse la aplicación.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Teresa Murph. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-with-the-objectdatasource-cs.md)
> [Siguiente](caching-data-at-application-startup-cs.md)
