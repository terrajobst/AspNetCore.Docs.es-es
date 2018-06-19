---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Almacenar en caché datos de la arquitectura (C#) | Documentos de Microsoft
author: rick-anderson
description: En el tutorial anterior, hemos visto cómo aplicar el almacenamiento en caché en el nivel de presentación. En este tutorial es aprender a aprovechar las ventajas de nuestro architectu superpuesto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 9ca91ecdaed536fe69196e0f726138590d7a9b77
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879223"
---
<a name="caching-data-in-the-architecture-c"></a>Almacenar en caché datos de la arquitectura (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) o [descarga de PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> En el tutorial anterior, hemos visto cómo aplicar el almacenamiento en caché en el nivel de presentación. En este tutorial se Aprenda a sacar partido de nuestra arquitectura por niveles en caché los datos en la capa de lógica de negocios. Para hacerlo mediante la extensión de la arquitectura para incluir una capa de almacenamiento en caché.


## <a name="introduction"></a>Introducción

Como vimos en el tutorial anterior, almacenamiento en caché los datos de s ObjectDataSource es tan sencillo como establecer un par de propiedades. Por desgracia, ObjectDataSource aplica el almacenamiento en caché en el nivel de presentación, que se acopla estrechamente con las directivas de almacenamiento en caché con la página ASP.NET. Uno de los motivos para crear una arquitectura en capas es permitir tales acoplamientos dejar de funcionar. La capa de lógica empresarial, por ejemplo, separa la lógica de negocios desde las páginas ASP.NET, mientras que la capa de acceso a datos separa los detalles de acceso a datos. En parte, este desacoplamiento de detalles de acceso lógica y los datos de negocio es preferible, porque hace que el sistema sea más legible, más fácil de mantener y más flexible para cambiar. También permite el conocimiento de dominios y la división del trabajo que un desarrollador que trabaja en el t de nivel de presentación necesita estar familiarizado con los detalles de s de la base de datos para hacer su trabajo. Desacoplar la directiva de caché de la capa de presentación ofrece ventajas similares.

En este tutorial aumentaremos nuestra arquitectura debe incluir un *capa de almacenamiento en caché* (o CL abreviado) que emplea la directiva de caché. La capa de almacenamiento en caché incluirá un `ProductsCL` clase que proporciona acceso a la información de producto con métodos como `GetProducts()`, `GetProductsByCategoryID(categoryID)`, etc., que, cuando se invoca, agotará el primer intento para recuperar los datos de la memoria caché. Si la caché está vacía, estos métodos se invocará la correspondiente `ProductsBLL` método en la capa BLL, que a su vez, obtendría los datos de la capa DAL. El `ProductsCL` métodos almacenar en caché los datos recuperados de BLL antes de devolverlo.

Como se muestra en la figura 1, la CL se encuentra entre la capa de lógica de negocios y presentación.


![El almacenamiento en caché de capa (CL) es otra capa de nuestra arquitectura](caching-data-in-the-architecture-cs/_static/image1.png)

**Figura 1**: capa de almacenamiento en caché el (CL) es otra capa de nuestra arquitectura


## <a name="step-1-creating-the-caching-layer-classes"></a>Paso 1: Crear las clases de capa de almacenamiento en caché

En este tutorial crearemos una CL muy simple con una sola clase `ProductsCL` que tiene solo un conjunto de métodos. Creación de una capa de almacenamiento en caché completa para toda la aplicación necesitaría crear `CategoriesCL`, `EmployeesCL`, y `SuppliersCL` clases y proporcionar un método en estas clases de la capa de almacenamiento en caché para cada método de acceso o la modificación de datos de la capa BLL. Al igual que con las capas BLL y DAL, lo ideal es que se debería implementar la capa de almacenamiento en caché como un proyecto de biblioteca de clases independiente; Sin embargo, se implementará como una clase en el `App_Code` carpeta.

Para clases independientes correctamente la CL más de las clases de la capa DAL y BLL, s permiten crear una nueva subcarpeta en la `App_Code` carpeta. Haga doble clic en el `App_Code` carpeta en el Explorador de soluciones, elija la nueva carpeta y el nombre de la nueva carpeta `CL`. Después de crear esta carpeta, agregar a ella, una nueva clase denominada `ProductsCL.cs`.


![Agregar una nueva carpeta denominada CL y una clase denominada ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figura 2**: agregar una nueva carpeta denominada `CL` y una clase denominada `ProductsCL.cs`


El `ProductsCL` clase debe incluir el mismo conjunto de métodos de acceso y modificación de datos que se encuentran en su clase correspondiente de la capa de lógica empresarial (`ProductsBLL`). En lugar de todos estos métodos, intente crear de s permiten crear un par aquí para hacerse una idea de los patrones utilizados por la CL. En concreto, vamos a agregar la `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos en el paso 3 y un `UpdateProduct` sobrecarga en el paso 4. Puede agregar el resto `ProductsCL` métodos y `CategoriesCL`, `EmployeesCL`, y `SuppliersCL` clases en su tiempo libre.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Paso 2: Leer y escribir en la caché de datos

La característica explorada en el tutorial anterior internamente el almacenamiento en caché de ObjectDataSource utiliza la caché de datos ASP.NET para almacenar los datos recuperados de BLL. La caché de datos también son accesibles mediante programación de las clases de código subyacente de páginas ASP.NET o de las clases de la arquitectura de s de aplicación web. Para leer y escribir en la caché de datos de una clase de código subyacente ASP.NET página s, utilice el modelo siguiente:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

El [ `Cache` clase](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` método](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) tiene una serie de sobrecargas. `Cache["key"] = value` y `Cache.Insert(key, value)` son sinónimos y ambos agregan un elemento a la memoria caché utilizando la clave especificada sin una expiración definido. Por lo general, debe especificar una fecha de expiración al agregar un elemento a la caché, ya sea como una dependencia, una expiración basada en la hora o ambas. Utilice uno de los otros `Insert` sobrecargas del método s para proporcionar información de dependencia o basado en tiempo de caducidad.

La capa de almacenamiento en caché métodos s que deba comprobar primero si los datos solicitados que se están en la memoria caché y, si es así, vuelve a partir de ahí. Si los datos solicitados no están en la memoria caché, el método adecuado de BLL debe invocarse. Su valor devuelto debe estar almacenado en memoria caché y, a continuación, devuelve, como se muestra en el siguiente diagrama de secuencia.


![Los métodos de s de la capa de almacenamiento en caché devuelven datos de la memoria caché si lo s disponible](caching-data-in-the-architecture-cs/_static/image3.png)

**Figura 3**: métodos de s de la capa de almacenamiento en caché devuelven datos de la memoria caché si lo s disponible


La secuencia que se muestra en la figura 3 se realiza en las clases de CL utilizando el modelo siguiente:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

En este caso, *tipo* es el tipo de datos que se almacena en la memoria caché `Northwind.ProductsDataTable`, por ejemplo mientras *clave* es la clave que identifica de forma única el elemento en caché. Si el elemento con los valores especificados *clave* no está en la memoria caché, a continuación, *instancia* será `null` y los datos se recuperan desde el método BLL adecuado y agregados a la caché. En el momento `return instance` se alcanza, *instancia* contiene una referencia a los datos, ya sea desde la memoria caché o extraerse de la capa BLL.

Asegúrese de usar el patrón anterior al obtener acceso a datos de la memoria caché. El patrón siguiente, que, a primera vista, parece equivalente, contiene una ligera diferencia que presenta una condición de carrera. Condiciones de carrera son difíciles de depurar, puesto que se revelan a sí mismos de manera esporádica y son difíciles de reproducir.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

La diferencia en este segundo, el fragmento de código incorrecto es que en lugar de almacenar una referencia al elemento almacenado en caché en una variable local, la caché de datos se obtiene acceso directamente en la instrucción condicional *y* en el `return`. Imagine que cuando se alcanza este código, `Cache["key"]` no es`null`, pero antes del `return` se alcanza la instrucción, el sistema expulsa *clave* de la memoria caché. En este caso excepcional, el código devolverá un `null` valor en lugar de un objeto del tipo esperado.

> [!NOTE]
> La caché de datos es segura para subprocesos, por lo que no necesita t para sincronizar el acceso a un subproceso para simples lecturas o escrituras. Sin embargo, si tiene que realizar varias operaciones en los datos en la memoria caché que tienen que ser atómicos, es responsable de la implementación de un bloqueo o algún otro mecanismo para garantizar la seguridad de subprocesos. Vea [sincronizar el acceso a la caché de ASP.NET](http://www.ddj.com/184406369) para obtener más información.


Un elemento se pueda expulsar mediante programación desde la memoria caché de datos mediante la [ `Remove` método](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) así:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Paso 3: Devolver información de producto desde la`ProductsCL`(clase)

Para este tutorial permite s implementar dos métodos para devolver información del producto desde la `ProductsCL` clase: `GetProducts()` y `GetProductsByCategoryID(categoryID)`. Al igual que con la `ProductsBL` clase en la capa de lógica empresarial, el `GetProducts()` método en la CL devuelve información acerca de todos los productos como un `Northwind.ProductsDataTable` objeto, mientras que `GetProductsByCategoryID(categoryID)` devuelve todos los productos de una categoría especificada.

El código siguiente muestra una parte de los métodos de la `ProductsCL` clase:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

En primer lugar, tenga en cuenta el `DataObject` y `DataObjectMethodAttribute` atributos que se aplican a las clases y métodos. Estos atributos proporcionan información para el Asistente para la s ObjectDataSource, que indica qué clases y métodos deben aparecer en los pasos del asistente s. Como los métodos y clases de CL se obtendrá acceso desde un origen ObjectDataSource en el nivel de presentación, agregan estos atributos para mejorar la experiencia en tiempo de diseño. Hacen referencia a la [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md) tutorial para obtener una descripción más detallada sobre estos atributos y sus efectos.

En el `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos, los datos devueltos por la `GetCacheItem(key)` método se asigna a una variable local. El `GetCacheItem(key)` método, que examinaremos en breve, devuelve un elemento determinado de la memoria caché según lo especificado *clave*. Si no se encuentran ningún dichos datos en memoria caché, se recupera de la correspondiente `ProductsBLL` método de la clase y, a continuación, se agrega a la caché mediante el `AddCacheItem(key, value)` método.

El `GetCacheItem(key)` y `AddCacheItem(key, value)` métodos de interfaz con la memoria caché de datos, leer y escribir valores, respectivamente. El `GetCacheItem(key)` método es la opción más sencilla de los dos. Simplemente devuelve el valor de la clase de caché mediante la pasa en *clave*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` No usar *clave* valor tal como lo suministró, sino más bien llamadas el `GetCacheKey(key)` método, que devuelve el *clave* con ProductsCache-. El `MasterCacheKeyArray`, que contiene la cadena ProductsCache, también se usa por la `AddCacheItem(key, value)` método, como veremos en breve.

De una clase de código subyacente ASP.NET página s, la caché de datos puede tener acceso mediante el `Page` clase s [ `Cache` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)y permite una sintaxis similar a `Cache["key"] = value`, como se describe en el paso 2. De una clase dentro de la arquitectura, la caché de datos son accesibles mediante `HttpRuntime.Cache` o `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)de entrada de blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) notas de la ventaja de rendimiento ligeras de utilizar `HttpRuntime` en lugar de `HttpContext.Current`; por lo tanto, `ProductsCL` utiliza `HttpRuntime`.

> [!NOTE]
> Si la arquitectura se implementa mediante proyectos de biblioteca de clases, a continuación, debe agregar una referencia a la `System.Web` ensamblado para poder usar el [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) y [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) clases.


Si el elemento no se encuentra en la memoria caché, el `ProductsCL` métodos de clase s. obtener los datos de la capa BLL y lo agrega a la caché mediante el `AddCacheItem(key, value)` método. Para agregar *valor* a la memoria caché podríamos usar el código siguiente, que utiliza una expiración de tiempo de 60 segundos:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Especifica la expiración basada en tiempo de 60 segundos en el futuro mientras [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) no indica que existen s ninguna fecha de expiración variable. Si bien esto `Insert` sobrecarga del método tiene parámetros para ambos absoluto de entrada y deslizante expiración, solo puede proporcionar uno de los dos. Si intenta especificar un tiempo absoluto y un intervalo de tiempo, el `Insert` método producirá una `ArgumentException` excepción.

> [!NOTE]
> Esta implementación de la `AddCacheItem(key, value)` método actualmente tiene algunas limitaciones. Comenzaremos direcciones y solucionar estos problemas en el paso 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Paso 4: Invalidando la memoria caché cuando los datos es modificado a través de la arquitectura

Junto con los métodos de recuperación de datos, debe proporcionar los mismos métodos que la capa BLL para insertar, actualizar y eliminar datos de la capa de almacenamiento en caché. Los métodos de modificación de datos de CL s no modifican los datos en caché, pero en su lugar llame al método de modificación de datos correspondiente de BLL s y, a continuación, invalidar la memoria caché. Como vimos en el tutorial anterior, este comportamiento es el mismo que el ObjectDataSource se aplica cuando se habilitan las características de sus almacenamiento en caché y su `Insert`, `Update`, o `Delete` los métodos se invocan.

El siguiente `UpdateProduct` sobrecarga muestra cómo implementar los métodos de modificación de datos en la CL:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Se invoca la modificación de datos correspondiente método de la capa de lógica empresarial, pero antes de que se devuelve la respuesta es necesario invalidar la memoria caché. Desafortunadamente, invalidar la memoria caché no es sencillo porque la `ProductsCL` clase s `GetProducts()` y `GetProductsByCategoryID(categoryID)` métodos entre agregan elementos a la caché con claves diferentes y el `GetProductsByCategoryID(categoryID)` método agrega un elemento de caché diferente para cada único *categoryID*.

Al invalidar la memoria caché, es necesario quitar *todos los* de los elementos que se han agregado por la `ProductsCL` clase. Esto puede realizarse mediante la asociación de un *la dependencia de caché* con cada elemento agregado a la memoria caché en el `AddCacheItem(key, value)` método. En general, una dependencia de caché puede ser otro elemento en la memoria caché, un archivo en el sistema de archivos o datos de una base de datos de Microsoft SQL Server. Cuando la dependencia cambia o se quita de la memoria caché, los elementos de caché que se asocia automáticamente se desalojan de la memoria caché. Para este tutorial, desea crear un elemento adicional en la memoria caché que actúa como una dependencia de caché para todos los elementos agregados a través del `ProductsCL` clase. De este modo, todos estos elementos pueden quitarse de la memoria caché con solo quitar la dependencia de caché.

Actualización de s permiten la `AddCacheItem(key, value)` está asociado con una dependencia de caché único método para que cada elemento se agrega a la memoria caché a través de este método:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` es una matriz de cadena que contiene un valor único, ProductsCache. En primer lugar, un elemento de caché se agrega a la memoria caché y asigna la fecha y hora actuales. Si ya existe el elemento en caché, se actualiza. A continuación, se crea una dependencia de caché. El [ `CacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s constructor tiene un número de sobrecargas, pero que se utiliza aquí espera dos `string` entradas de la matriz. La primera de ellas especifica el conjunto de archivos que se usará como dependencias. Puesto que no queremos t desea usar las dependencias basadas en archivos, un valor de `null` se usa para el primer parámetro de entrada. El segundo parámetro de entrada especifica el conjunto de claves de caché que se usarán como dependencias. Aquí se especifica la dependencia única, `MasterCacheKeyArray`. El `CacheDependency` , a continuación, se pasa a la `Insert` método.

Con esta modificación a `AddCacheItem(key, value)`, invaliding la memoria caché es tan sencilla como si quita la dependencia.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Paso 5: Llamando a la capa de almacenamiento en caché de la capa de presentación

Los métodos y clases de s de capa de almacenamiento en caché se pueden usar para trabajar con datos mediante las técnicas se ha examinado a lo largo de estos tutoriales. Para ilustrar trabajar con datos almacenados en caché, guarde los cambios en el `ProductsCL` clase y, a continuación, abra el `FromTheArchitecture.aspx` página en el `Caching` carpeta y agregar un control GridView. Desde la etiqueta inteligente de GridView s, cree un nuevo origen ObjectDataSource. En el paso del primer asistente s debería ver la `ProductsCL` de clase como una de las opciones de la lista desplegable.


[![La clase ProductsCL se incluye en la lista desplegable de objeto de negocios](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figura 4**: el `ProductsCL` clase se incluye en la lista desplegable de objeto de negocios ([haga clic aquí para ver la imagen a tamaño completo](caching-data-in-the-architecture-cs/_static/image6.png))


Después de seleccionar `ProductsCL`, haga clic en siguiente. La lista desplegable en la ficha Seleccionar tiene dos elementos - `GetProducts()` y `GetProductsByCategoryID(categoryID)` y la pestaña de actualización tiene el mero `UpdateProduct` sobrecarga. Elija la `GetProducts()` método desde la ficha Seleccionar y `UpdateProducts` método desde la pestaña de actualización y haga clic en Finalizar.


[![Se enumeran los métodos de la clase ProductsCL s en el cuadro desplegable enumera](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figura 5**: el `ProductsCL` en el cuadro desplegable enumera se enumeran los métodos de clase s. ([haga clic aquí para ver la imagen a tamaño completo](caching-data-in-the-architecture-cs/_static/image9.png))


Después de completar el asistente, Visual Studio establecerá la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}` y agregue los campos correspondientes a la GridView. Cambiar el `OldValuesParameterFormatString` propiedad en su valor predeterminado, `{0}`y configurar el control GridView para admitir la paginación, ordenar y editar. Puesto que la `UploadProducts` sobrecarga utilizada por la CL acepta solo el nombre de producto editado s y el precio, limitar GridView para que solo estos campos son editables.

En el tutorial anterior se ha definido un GridView para que incluya campos para la `ProductName`, `CategoryName`, y `UnitPrice` campos. No dude en replicar este formato y la estructura, en cuyo caso la s GridView y ObjectDataSource declarativa marcado debe ser similar al siguiente:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

En este momento tenemos una página que usa la capa de almacenamiento en caché. Para ver la memoria caché en acción, establecer puntos de interrupción la `ProductsCL` clase s `GetProducts()` y `UpdateProduct` métodos. Visite la página en un explorador y recorrer el código al ordenar y paginar para ver los datos extraen de la memoria caché. A continuación, actualizar un registro y observe que se invalida la caché y, por lo tanto, se recupera de la capa BLL cuando se vuelve a enlazar los datos en GridView.

> [!NOTE]
> La capa de almacenamiento en caché proporcionado en la descarga que acompaña a este artículo no está completa. Contiene solo una clase `ProductsCL`, que solo deportivos una serie de métodos. Además, una sola página ASP.NET usa la CL (`~/Caching/FromTheArchitecture.aspx`) todos los demás aún hacen referencia a la capa BLL directamente. Si tiene previsto utilizar una CL en la aplicación, todas las llamadas de la capa de presentación deben ir a la CL, lo que requeriría que las clases de CL s y métodos tratan esas clases y métodos en la capa BLL utilizado actualmente por la capa de presentación.


## <a name="summary"></a>Resumen

Mientras que el almacenamiento en caché se puede aplicar en el nivel de presentación con ASP.NET 2.0 s SqlDataSource y ObjectDataSource controles, lo ideal es que el almacenamiento en caché responsabilidades debería delegarse a una capa independiente de la arquitectura. En este tutorial, creamos una capa de almacenamiento en caché que se encuentra entre la capa de presentación y la capa de lógica de negocios. La capa de almacenamiento en caché debe proporcionar el mismo conjunto de clases y métodos que existen en la capa BLL y se les llama desde la capa de presentación.

Los ejemplos de capa de almacenamiento en caché, analizamos en este y los tutoriales anteriores exhibe *carga reactiva*. Con carga reactiva, los datos se cargan en la memoria caché solo cuando se realiza una solicitud para los datos y que los datos que faltan de la memoria caché. También pueden ser datos *proactivamente cargado* en la memoria caché, una técnica que carga los datos en la memoria caché antes de que sea estrictamente necesario. En el siguiente tutorial veremos un ejemplo de carga automático cuando veremos cómo almacenar valores estáticos en la memoria caché al iniciar la aplicación.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Teresa Murph. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-with-the-objectdatasource-cs.md)
> [Siguiente](caching-data-at-application-startup-cs.md)
