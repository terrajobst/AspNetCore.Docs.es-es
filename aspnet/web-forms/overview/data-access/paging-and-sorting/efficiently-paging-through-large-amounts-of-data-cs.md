---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Página de forma eficaz a través de grandes cantidades de datos (C#) | Microsoft Docs
author: rick-anderson
description: La opción de paginación predeterminada de un control de presentación de datos es adecuada cuando se trabaja con grandes cantidades de datos, como su retriev subyacente de control de origen de datos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a6d023f299d3c36e0b9f0d00f2b531d73657135c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367886"
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Página de forma eficaz a través de grandes cantidades de datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) o [descargar PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> La opción de paginación predeterminada de un control de presentación de datos es adecuada cuando se trabaja con grandes cantidades de datos, como su control de origen de datos subyacente recupera todos los registros, incluso si se muestra solo un subconjunto de datos. En tales circunstancias, se debe activar en personalizada paginación.


## <a name="introduction"></a>Introducción

Como se explicó en el tutorial anterior, se puede implementar la paginación en uno de dos maneras:

- **Paginación predeterminada** puede implementarse con sólo marcar la opción de habilitar la paginación en el control Web s de datos de etiqueta inteligente; sin embargo, cada vez que ve una página de datos, recupera el origen ObjectDataSource *todas* de los registros, incluso Aunque sólo un subconjunto de ellos se muestran en la página
- **Paginación personalizada** mejora el rendimiento de forma predeterminada paginación por recuperar solo los registros de la base de datos que se deben mostrar para la página de datos solicitados por el usuario; sin embargo, la paginación personalizada implica un mayor esfuerzo para implementar que la paginación predeterminada

Debido a la facilidad de comprobación de implementación solo una casilla y que se van a realizar. paginación predeterminada es una opción atractiva. Su enfoque de ve na en recuperar todos los registros, sin embargo, resulta una opción inverosímil al paginar a través de lo suficientemente grandes cantidades de datos o para sitios con muchos usuarios simultáneos. En tales circunstancias, nos debemos convertir en personalizada con el fin de proporcionar un sistema con capacidad de respuesta de paginación.

El desafío de la paginación personalizada es poder escribir una consulta que devuelve el conjunto de registros necesarios para una determinada página de datos. Afortunadamente, Microsoft SQL Server 2005 proporciona una nueva palabra clave para resultados de la clasificación, lo que nos permite escribir una consulta que puede recuperar eficazmente el subconjunto correcto de registros. En este tutorial, veremos cómo usar esta nueva palabra clave de SQL Server 2005 para implementar la paginación personalizada en un control GridView. Mientras que la interfaz de usuario para la paginación personalizada es idéntica a la paginación de forma predeterminada, ejecución paso a paso de una página a la siguiente mediante paginación personalizada puede ser varios órdenes de magnitud más rápido que la paginación predeterminada.

> [!NOTE]
> El número total de registros que se va a paginar a través y la carga que se va a colocar en el servidor de base de datos depende de la ganancia de rendimiento exacto mostrada por la paginación personalizada. Al final de este tutorial veremos algunas métricas aproximadas que presentan las ventajas de rendimiento obtenido a través de la paginación personalizada.


## <a name="step-1-understanding-the-custom-paging-process"></a>Paso 1: Descripción del proceso de paginación personalizada

Cuando la paginación de datos, los registros precisos mostrados en una página dependen de la página de datos que se solicita y el número de registros mostrados por página. Por ejemplo, imagine que queríamos paginar a través de los 81 productos, mostrar los 10 productos por página. Al ver la primera página, d queremos productos 1 a 10; al ver la segunda página se d que le interese 11 20 a través de productos y así sucesivamente.

Hay tres variables que determinen qué registros deben recuperarse y cómo se debe representar la interfaz de paginación:

- **Índice de fila inicial** el índice de la primera fila de la página de datos que se va a mostrar; este índice se puede calcularse multiplicando el índice de página por los registros que se va a mostrar por página y agregando uno. Por ejemplo, al paginar a través de los registros de 10 a la vez, para la primera página (cuyo índice de página es 0), el índice de fila inicial es 0 \* 10 + 1 o 1; la segunda página (cuyo índice de página es 1), el índice de fila inicial es 1 \* 10 + 1 , o 11.
- **Número máximo de filas** el número máximo de registros que se muestran por página. Esta variable se conoce como el número máximo de filas ya para la última página hay puede ser menos registros devueltos que el tamaño de página. Por ejemplo, al paginar a través de los registros de 81 productos 10 por página, la página final y novena tendrá sólo un registro. Sin embargo, no hay ninguna página, mostrará más registros que el valor de número máximo de filas.
- **Número de registros total** el número total de registros que se va a paginar a través. Aunque esta variable no es necesario para determinar qué registros para recuperar de una página determinada, indican la interfaz de paginación. Por ejemplo, si hay 81 productos que se va a paginar a través, sabe la interfaz de paginación para mostrar los números de página nueve en el interfaz de usuario de paginación.

Con la paginación de forma predeterminada, el índice de fila inicial se calcula como el producto del índice de la página y el tamaño de página más uno, mientras que el número máximo de filas es simplemente el tamaño de página. Puesto que la paginación predeterminada recupera todos los registros de la base de datos al representar cualquier página de datos, el índice para cada fila se conoce, por lo que el cambio a la fila de índice de fila de iniciar una tarea trivial. Además, el número Total de registros está disponible, tal y como s simplemente el número de registros en la tabla de datos (o cualquier objeto se utiliza para almacenar los resultados de la base de datos).

Dadas las variables de índice de fila inicial y el número máximo de filas, una implementación personalizada de paginación debe devolver solo el preciso subconjunto de registros empezando por el índice de fila inicial y hasta el número máximo de filas de registros después de eso. Paginación personalizada proporciona dos desafíos:

- Debemos poder asociar eficazmente un índice de fila con cada fila en todos los datos que se va a paginar a través de por lo que podemos comenzar a devolver los registros en el índice de fila de inicio especificado
- Se debe proporcionar el número total de registros que se va a paginar a través de

En los dos pasos siguientes, examinaremos el script SQL necesario para responder a estas dos desafíos. Además de la secuencia de comandos SQL, también se necesitará implementar métodos en la capa DAL y BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Paso 2: Devolver el número Total de registros que se va a paginar a través de

Antes de examinar cómo recuperar el subconjunto preciso de los registros de la página que se muestran, permiten s primer vistazo a cómo se devuelve el número total de registros que se va a paginar a través. Esta información es necesaria para configurar correctamente la interfaz de usuario de paginación. El número total de registros devueltos por una consulta SQL determinada puede obtenerse utilizando el [ `COUNT` función de agregado](https://msdn.microsoft.com/library/ms175997.aspx). Por ejemplo, para determinar el número total de registros en el `Products` tabla, podemos usar la siguiente consulta:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Permitir s Agregar un método a nuestro DAL que devuelve esta información. En concreto, vamos a crear un método DAL llamado `TotalNumberOfProducts()` que se ejecuta el `SELECT` instrucción mostrado anteriormente.

Comience abriendo la `Northwind.xsd` DataSet con tipo de archivo en el `App_Code/DAL` carpeta. A continuación, haga doble clic en el `ProductsTableAdapter` en el diseñador y seleccione Add Query. Como se ve visto en los tutoriales anteriores, esto nos permitirá agregar un nuevo método a la capa DAL que, cuando se invoca, se ejecutará una determinada instrucción SQL o procedimiento almacenado. Al igual que con nuestros métodos de TableAdapter en los tutoriales anteriores, en esta ocasión optar por usar una instrucción de SQL ad hoc.


![Utilice una instrucción de SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figura 1**: utilice una instrucción de SQL Ad Hoc


Podemos especificar qué tipo de consulta para crear en la pantalla siguiente. Puesto que esta consulta devolverá un único valor escalar el número total de registros en el `Products` tabla elegir el `SELECT` que devuelve una opción de valor único.


![Configurar la consulta para utilizar una instrucción SELECT que devuelve un valor único](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figura 2**: configuración de la consulta para utilizar una instrucción SELECT que devuelve un valor único


Después de que indica el tipo de consulta que se utilizará, a continuación se debe especificar la consulta.


![Utilice la seleccione COUNT(*) de consulta de productos](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figura 3**: Use el número de SELECT (\*) FROM productos consulta


Por último, especifique el nombre del método. Como s mencionados anteriormente, permiten usar `TotalNumberOfProducts`.


![Nombre de la TotalNumberOfProducts DAL (método)](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figura 4**: el nombre de la TotalNumberOfProducts DAL (método)


Después de hacer clic en Finalizar, el asistente agregará la `TotalNumberOfProducts` método a la capa DAL. Los métodos devuelven escalares en la capa DAL devuelven tipos que aceptan valores NULL, en caso de que el resultado de la consulta SQL es `NULL`. Nuestro `COUNT` consulta, sin embargo, siempre devolverá una que no sean de`NULL` valor; en cualquier caso, el método DAL devuelve un entero que acepta valores NULL.

Además del método DAL, también se necesita un método en la capa BLL. Abra el `ProductsBLL` archivo de clase y agregue un `TotalNumberOfProducts` método que llama simplemente hacia abajo a la s DAL `TotalNumberOfProducts` método:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

La s DAL `TotalNumberOfProducts` método devuelve un entero que acepta valores NULL; sin embargo, se ve que se creó el `ProductsBLL` clase s `TotalNumberOfProducts` método para que devuelva un entero estándar. Por lo tanto, necesitamos tener la `ProductsBLL` clase s `TotalNumberOfProducts` método devuelve la parte del valor del entero que acepta valores NULL devuelto por la s DAL `TotalNumberOfProducts` método. La llamada a `GetValueOrDefault()` devuelve el valor del entero que acepta valores NULL, si existe; si el entero que acepta valores NULL es `null`, sin embargo, devuelve el valor del entero de forma predeterminada, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Paso 3: Devolver el preciso subconjunto de registros

La siguiente tarea consiste en crear métodos de DAL y BLL que aceptan el índice de fila inicial y las variables de número máximo de filas describen anteriormente y devuelvan los registros correspondientes. Antes de hacerlo, permiten s primer vistazo a la secuencia de comandos SQL necesario. El reto de nosotros es que debemos ser capaz de asignar eficazmente un índice para cada fila en todos los resultados que se va a paginar a través de poder, podemos devolver esos registros empezando en el índice de fila inicial (y hasta el número máximo de registros de registros).

Esto no es un desafío si ya existe una columna en la tabla de base de datos que actúa como un índice de fila. A primera vista podríamos pensar que el `Products` tabla s `ProductID` campo sería suficiente, ya que tiene el primer producto `ProductID` de 1, el segundo un 2, y así sucesivamente. Sin embargo, la eliminación de un producto deja una discontinuidad en la secuencia, anulando este enfoque.

Hay dos técnicas generales que usa para asociar eficazmente un índice de fila con los datos en la página a través de lo que permite el subconjunto de registros que deben recuperarse preciso:

- **Con SQL Server 2005 s `ROW_NUMBER()` palabra clave** es nuevo en SQL Server 2005, el `ROW_NUMBER()` palabra clave asocia una clasificación de cada registro devuelto en función de algunos pedidos. Esta clasificación puede usarse como un índice de fila para cada fila.
- **Uso de una Variable de tabla y `SET ROWCOUNT`**  s de SQL Server [ `SET ROWCOUNT` instrucción](https://msdn.microsoft.com/library/ms188774.aspx) puede utilizarse para especificar el número total de registros debe procesar una consulta antes de finalizar; [variables de tabla](http://www.sqlteam.com/item.asp?ItemID=9454) son variables locales de Transact-SQL que pueden contener datos tabulares, akin [tablas temporales](http://www.sqlteam.com/item.asp?ItemID=2029). Este enfoque funciona igualmente bien con Microsoft SQL Server 2005 y SQL Server 2000 (mientras que el `ROW_NUMBER()` enfoque solo funciona con SQL Server 2005).  
  
  La idea aquí es crear una variable de tabla que tiene un `IDENTITY` y columnas para las claves principales de la tabla cuyos datos se envía un mensaje a través. A continuación, se vuelca el contenido de la tabla cuyos datos se envía un mensaje a través en la variable de tabla, con lo que se asocia un índice de fila secuenciales (a través de la `IDENTITY` columna) para cada registro en la tabla. Una vez que se ha rellenado la variable de tabla, un `SELECT` instrucción en la variable de tabla, se combina con la tabla subyacente, se puede ejecutar para extraer los registros determinados. El `SET ROWCOUNT` instrucción se utiliza para limitar el número de registros que se deban volcarse en la variable de tabla de forma inteligente.  
  
  Esta eficacia del enfoque s se basa en el número de página que se solicita, como el `SET ROWCOUNT` valor se asigna el valor de índice de fila inicial más el número máximo de filas. Al paginar a través de páginas numeradas de baja, como la primera algunas páginas de datos de este enfoque es muy eficaz. Sin embargo, exhibe performance de paginación de forma predeterminada cuando se recuperan de una página cerca del final.

En este tutorial implementa paginación personalizado mediante la `ROW_NUMBER()` palabra clave. Para obtener más información sobre el uso de la variable de tabla y `SET ROWCOUNT` técnica, consulte [A varios métodos eficaces para paginar a través de grandes conjuntos de resultados](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

El `ROW_NUMBER()` palabra clave asociada una clasificación de cada registro devuelto a través de un orden concreto utilizando la sintaxis siguiente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` Devuelve un valor numérico que especifica el rango para cada registro con respecto a que el orden indicado. Por ejemplo, para ver el rango para cada producto, ordenada de mayor costoso al menos, podríamos usar la siguiente consulta:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Figura 5 muestra esta consulta los resultados de s cuando se ejecuta a través de la ventana de consulta en Visual Studio. Tenga en cuenta que los productos se ordenan por precio, junto con un rango de precios para cada fila.


![Se incluye el rango de precios para cada registro devuelto](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figura 5**: se incluye el rango de precios para cada registro devuelto


> [!NOTE]
> `ROW_NUMBER()` es solo una de las muchas nuevas funciones de categoría disponibles en SQL Server 2005. Para obtener una explicación más exhaustiva de `ROW_NUMBER()`, junto con las funciones de categoría, leer [devolver resultados de rango con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Cuando los resultados de clasificación por especificado `ORDER BY` columna en el `OVER` cláusula (`UnitPrice`, en el ejemplo anterior), SQL Server debe ordenar los resultados. Esto es una operación rápida si hay un índice clúster a través de las columnas que se va a se ordenan los resultados por, o si hay una cobertura de índice, pero puede ser más costosa de lo contrario. Para ayudar a mejorar el rendimiento de consultas lo suficientemente grandes, considere la posibilidad de agregar un índice no agrupado para la columna por la que los resultados se ordenan por. Consulte [funciones de categoría y el rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para una visión más detallada de las consideraciones de rendimiento.

La información de clasificación devuelta por `ROW_NUMBER()` no se puede usar directamente en el `WHERE` cláusula. Sin embargo, se puede usar una tabla derivada para devolver el `ROW_NUMBER()` resultado, que, a continuación, puede aparecer en el `WHERE` cláusula. Por ejemplo, la consulta siguiente utiliza una tabla derivada para devolver las columnas ProductName y UnitPrice, junto con el `ROW_NUMBER()` resultado y, a continuación, utiliza un `WHERE` cláusula para devolver únicamente aquellos productos cuyo rango precio es entre 11 y 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Ampliar un poco más este concepto, podemos utilizar este enfoque para recuperar una página específica de datos dados los valores de índice de fila inicial y el número máximo de filas deseados:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Como veremos más adelante en este tutorial, el *`StartRowIndex`* proporcionado por el origen ObjectDataSource se indiza empezando por cero, mientras que la `ROW_NUMBER()` valor devuelto por SQL Server 2005 se indiza empezando por 1. Por lo tanto, el `WHERE` cláusula devuelve aquellos registros donde `PriceRank` es estrictamente mayor que *`StartRowIndex`* y menor o igual a *`StartRowIndex`*  +  *`MaximumRows`*.


Ahora que se ve cómo tratan `ROW_NUMBER()` puede ser utilizado para recuperar una página de datos dados los valores de índice de fila inicial y el número máximo de filas determinada, ahora es necesario implementar esta lógica como métodos en la capa DAL y BLL.

Al crear esta consulta que debemos decidir el orden por el que los resultados tendrán una clasificación; permiten s ordenar los productos por su nombre en orden alfabético. Esto significa que con la implementación de paginación personalizada en este tutorial, no será capaz de crear un informe paginado personalizado que también se pueden ordenar. En el siguiente tutorial, sin embargo, veremos cómo se puede proporcionar esta funcionalidad.

En la sección anterior, creamos el método DAL como instrucción SQL ad hoc. Lamentablemente, el analizador de Transact-SQL en Visual Studio usando el asistente TableAdapter t como el `OVER` sintaxis utilizada por el `ROW_NUMBER()` función. Por lo tanto, debemos crear este método DAL como un procedimiento almacenado. Seleccione el Explorador de servidores en el menú de vista (o Ctrl + Alt + S visitas) y expanda el `NORTHWND.MDF` nodo. Para agregar un nuevo procedimiento almacenado, haga doble clic en el nodo procedimientos almacenados y elija Agregar un nuevo procedimiento almacenado (consulte la figura 6).


![Agregar un nuevo procedimiento almacenado para la paginación a través de los productos](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figura 6**: agregar un nuevo procedimiento almacenado para la paginación a través de los productos


Este procedimiento almacenado debe aceptar dos parámetros de entrada enteros - `@startRowIndex` y `@maximumRows` y usar el `ROW_NUMBER()` función ordenados por la `ProductName` campo, y devuelve solo las filas mayor que el especificado `@startRowIndex` y menor que o igual que `@startRowIndex`  +  `@maximumRow` s. Escriba el siguiente script en el nuevo procedimiento almacenado y, a continuación, haga clic en el icono de guardar para agregar el procedimiento almacenado a la base de datos.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Después de crear el procedimiento almacenado, dedique un momento para probarlo. Haga doble clic en el `GetProductsPaged` procedimiento almacenado nombre en el Explorador de servidores y elija la opción de ejecución. Visual Studio le pedirá que, a continuación, para los parámetros de entrada, `@startRowIndex` y `@maximumRow` s (consulte la figura 7). Probar valores diferentes y examinar los resultados.


![Escriba un valor para el @startRowIndex y @maximumRows parámetros](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Figura 7</strong>: escriba un valor para el @startRowIndex y @maximumRows parámetros


Después de elegir estos valores de parámetros de entrada, la ventana de salida mostrará los resultados. Figura 8 se muestran los resultados al pasar de 10 para ambos el `@startRowIndex` y `@maximumRows` parámetros.


[![Los registros que podrían aparecer en la segunda página de datos se devuelven.](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figura 8**: los registros que podrían aparecer en la segunda página de datos se devuelven ([haga clic aquí para ver imagen en tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Con este procedimiento almacenado creado, que está listo para crear el `ProductsTableAdapter` método. Abra el `Northwind.xsd` DataSet con tipo, botón secundario en el `ProductsTableAdapter`y elija la opción Add Query. En lugar de crear la consulta mediante una instrucción SQL ad hoc, créelo mediante un procedimiento almacenado existente.


![Crear el método de DAL mediante un procedimiento almacenado existente](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Figura 9**: crear el método de DAL mediante un procedimiento almacenado existente


A continuación, se nos pide que seleccione el procedimiento almacenado para invocar. Elegir el `GetProductsPaged` procedimiento almacenado desde la lista desplegable.


![Elija la GetProductsPaged procedimiento almacenado desde la lista desplegable](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figura 10**: elija la GetProductsPaged procedimiento almacenado desde la lista desplegable


La siguiente pantalla, a continuación, le pregunta qué tipo de datos devuelto por el procedimiento almacenado: datos tabulares, un valor único o ningún valor. Puesto que el `GetProductsPaged` procedimiento almacenado puede devolver varios registros, indique que devuelve datos tabulares.


![Indicar que el procedimiento almacenado devuelve datos tabulares](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figura 11**: indicar que el procedimiento almacenado devuelve datos tabulares


Por último, se indican los nombres de los métodos que desee que haya creado. Al igual que con nuestros tutoriales anteriores, siga adelante y crear métodos que utilizan tanto el relleno una DataTable y devolver un DataTable. Nombre del primer método `FillPaged` y el segundo `GetProductsPaged`.


![Nombre de lo métodos de FillPaged y GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figura 12**: nombre de lo métodos de FillPaged y GetProductsPaged


Por otra parte que va a crear un método DAL para devolver una página determinada de productos, también es necesario ofrecer esta funcionalidad en la capa BLL. Como el método DAL, la s BLL GetProductsPaged método debe aceptar dos entradas de entero para especificar el índice de fila inicial y el número máximo de filas y debe devolver solo aquellos registros que se encuentran dentro del intervalo especificado. Cree este tipo de método BLL en la clase ProductsBLL que simplemente llamadas hacia abajo en la s DAL método GetProductsPaged, así:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Puede utilizar cualquier nombre para los parámetros de entrada del método s BLL, pero, como veremos en breve, decide usar `startRowIndex` y `maximumRows` nos evita adicional de un poco de trabajo al configurar un origen ObjectDataSource para usar este método.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Paso 4: Configurar el origen ObjectDataSource para utilizar la paginación personalizada

Con los métodos para tener acceso a un subconjunto específico de registros completados BLL y DAL, está listo para crear un control GridView controlamos ese páginas a través de sus registros subyacentes mediante la paginación personalizada. Comience abriendo la `EfficientPaging.aspx` página en el `PagingAndSorting` carpeta, agregue un control GridView a la página y configurarlo para usar un nuevo control ObjectDataSource. En los tutoriales anteriores, a menudo teníamos ObjectDataSource configurado para usar el `ProductsBLL` clase s `GetProducts` método. Esta vez, sin embargo, deseamos usar el `GetProductsPaged` método en su lugar, desde el `GetProducts` devuelve del método *todas* de los productos de la base de datos, mientras que `GetProductsPaged` devuelve solo un subconjunto específico de registros.


![Configurar el origen ObjectDataSource para usar el método de clase ProductsBLL s GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figura 13**: configurar el origen ObjectDataSource para usar el método de clase ProductsBLL s GetProductsPaged


Desde que creamos re de la creación de un control GridView de solo lectura, dedique un momento para establecer la lista desplegable de método en la instrucción INSERT, UPDATE y elimine las pestañas en (None).

A continuación, el ObjectDataSource wizard nos pide los orígenes de la `GetProductsPaged` método s `startRowIndex` y `maximumRows` valores de parámetros de entrada. Estos parámetros de entrada realmente que establecerá la GridView automáticamente, así, simplemente deje el código fuente como Ninguno y haga clic en Finalizar.


![Deje los orígenes de parámetro de entrada como ninguno](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figura 14**: deja los orígenes de parámetro de entrada como ninguno


Después de completar al Asistente de ObjectDataSource GridView contendrá un BoundField o CampoCasillaVerificación para cada uno de los campos de datos del producto. No dude en adaptar la apariencia GridView como considere oportuno. Se ve optó por mostrar solamente el `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, y `UnitPrice` BoundFields. Además, configurar el control GridView para admitir la paginación activando la casilla Habilitar paginación en su etiqueta inteligente. Después de estos cambios, el marcado declarativo de GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Si visita la página a través de un explorador, sin embargo, el control GridView es ningún donde va a calcular.


![El control GridView no es aparece](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figura 15**: GridView no es aparece


Falta el control GridView porque el origen ObjectDataSource utiliza actualmente 0 como los valores para ambas la `GetProductsPaged` `startRowIndex` y `maximumRows` parámetros de entrada. Por lo tanto, la consulta SQL resultante devuelve ningún registro y, por tanto, no se muestra el control GridView.

Para solucionar este problema, es necesario configurar el origen ObjectDataSource para utilizar la paginación personalizada. Esto puede realizarse en los pasos siguientes:

1. **Establezca la s ObjectDataSource `EnablePaging` propiedad `true`**  esto indica al origen ObjectDataSource que debe pasar a la `SelectMethod` dos parámetros adicionales: uno para especificar el índice de fila inicial ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) y otra para especificar el número máximo de filas ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Establezca la s ObjectDataSource `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades según corresponda** el `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades indican los nombres de los parámetros de entrada que se pasó el `SelectMethod` para fines de paginación personalizada. De forma predeterminada, estos nombres de parámetro son `startIndexRow` y `maximumRows`, que es por eso, al crear el `GetProductsPaged` método en el BLL, utiliza estos valores para los parámetros de entrada. Si decide utilizar los nombres de parámetro diferente para la s BLL `GetProductsPaged` método como `startIndex` y `maxRows`, por ejemplo deberá establecer la s ObjectDataSource `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades según corresponda (por ejemplo, startIndex para `StartRowIndexParameterName` y maxRows para `MaximumRowsParameterName`).
3. **Establezca la s ObjectDataSource [ `SelectCountMethod` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) en el nombre del método que devuelve el Total número de registros que se va a paginar a través de (`TotalNumberOfProducts`)** Recuerde que el `ProductsBLL` clase s `TotalNumberOfProducts`método devuelve el número total de registros que se va a paginar a través de un método DAL que se ejecuta un `SELECT COUNT(*) FROM Products` consulta. Esta información es necesaria por el origen ObjectDataSource para representar correctamente la interfaz de paginación.
4. **Quitar el `startRowIndex` y `maximumRows` `<asp:Parameter>` elementos desde el marcado declarativo de ObjectDataSource s** al configurar el origen ObjectDataSource a través del asistente, Visual Studio agrega automáticamente dos `<asp:Parameter>` elementos para el `GetProductsPaged` método s parámetros de entrada. Estableciendo `EnablePaging` a `true`, estos parámetros se pasarán automáticamente; si también aparecen en la sintaxis declarativa, ObjectDataSource intenta pasar *cuatro* parámetros para el `GetProductsPaged` (método) y dos parámetros para el `TotalNumberOfProducts` método. Si se olvida de quitar estos `<asp:Parameter>` elementos, al visitar la página a través de un explorador que se obtendrá un mensaje de error como: *ObjectDataSource 'ObjectDataSource1' no encontró un método no genérico 'TotalNumberOfProducts' que tiene parámetros: startRowIndex, maximumRows*.

Después de realizar estos cambios, la sintaxis declarativa de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Tenga en cuenta que el `EnablePaging` y `SelectCountMethod` se han establecido las propiedades y los `<asp:Parameter>` se han quitado elementos. Figura 16 se muestra una captura de pantalla de la ventana Propiedades de una vez realizados estos cambios.


![Para usar la paginación personalizada, Configure el Control ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figura 16**: para usar la paginación personalizada, Configure el Control ObjectDataSource


Después de realizar estos cambios, visite esta página a través de un explorador. Debería ver 10 productos en la lista, ordenadas alfabéticamente. Tómese un momento para paso a través de la página datos a la vez. Aunque no hay ninguna diferencia visual desde la perspectiva del usuario final s entre la paginación predeterminada y la paginación personalizada, más eficazmente la paginación personalizada páginas a través de grandes cantidades de datos, ya que recupera solo aquellos registros que deben mostrarse de una página determinada.


[![Los datos, ordenado por el producto, nombre de s es paginado usando paginación personalizada](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figura 17**: los datos, ordenado por el producto, nombre de es paginado usando paginación personalizada ([haga clic aquí para ver imagen en tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Con la paginación personalizada, la página de recuento de valor devuelto por la s ObjectDataSource `SelectCountMethod` se almacena en el estado de vista GridView s. Otras variables GridView el `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` colección etc. se almacenan en *controlar el estado de*, que se conserva independientemente del valor de la s GridView `EnableViewState` propiedad. Puesto que la `PageCount` valor se conserva entre las devoluciones de datos usando el estado de vista, cuando se usa una interfaz de paginación que incluye un vínculo para ir a la última página, es imperativo que esté habilitado el estado de vista GridView s. (Si la interfaz de paginación no incluye un vínculo directo a la última página, a continuación, puede deshabilitar el estado de vista).


Al hacer clic en el último vínculo de página hace que una devolución de datos e indica a la GridView para actualizar su `PageIndex` propiedad. Si se hace clic en el último vínculo de página, el control GridView asigna su `PageIndex` propiedad con un valor uno menor que su `PageCount` propiedad. Con el estado de vista deshabilitado, el `PageCount` valor se pierde durante las devoluciones y los `PageIndex` se asigna el valor entero máximo en su lugar. A continuación, el control GridView intenta determinar el índice de fila inicial multiplicando el `PageSize` y `PageCount` propiedades. Esto da como resultado un `OverflowException` puesto que el producto supera el tamaño máximo de enteros permitidos.

## <a name="implement-custom-paging-and-sorting"></a>Implemente la paginación personalizada y ordenación

Nuestra implementación de paginación personalizada actual requiere que se especifique el orden por el que los datos están paginados a través de forma estática al crear el `GetProductsPaged` procedimiento almacenado. Sin embargo, es posible que haya anotado que la etiqueta inteligente de GridView s contiene una casilla de verificación Habilitar ordenación además de la opción de habilitar la paginación. Por desgracia, agregar compatibilidad con la ordenación en el control GridView con nuestra implementación de paginación personalizada actual, solo se ordenarán los registros en la página de datos está viendo actualmente. Por ejemplo, si configura el control GridView para admitir la paginación y, a continuación, al ver la primera página de datos, ordenar por nombre de producto en orden descendente, invertirá el orden de los productos en la página 1. Como se muestra en la figura 18, como muestra tigre Carnarvon como el primer producto al ordenar en orden alfabético inverso, que omite el 71 otros productos que venga después tigre Carnarvon, por orden alfabético; solo aquellos registros en la primera página se consideran en la ordenación.


[![Solo los datos que se muestra en la página actual se ordena.](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figura 18**: solo los datos que se muestra en la página actual está ordenada ([haga clic aquí para ver imagen en tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


La ordenación sólo se aplica a la página de datos actual porque la ordenación se está produciendo una vez recuperados los datos del s BLL `GetProductsPaged` , este método y devuelve solo aquellos registros para la página específica. Para implementar correctamente una ordenación, necesitamos pasar la expresión de ordenación para el `GetProductsPaged` método para que los datos pueden clasificarse adecuadamente antes de devolver la página específica de datos. Veremos cómo hacerlo en nuestro tutorial siguiente.

## <a name="implementing-custom-paging-and-deleting"></a>Implementación personalizada de paginación y eliminar

Si puede habilitar la funcionalidad de eliminación en un control GridView se paginan cuyos datos mediante técnicas de paginación personalizada que encontrará al eliminar el último registro de la última página, GridView desaparece en lugar de disminuir de forma adecuada los s GridView `PageIndex`. Para reproducir este error, habilitar la eliminación para el tutorial simplemente que acabamos de crear. Vaya a la última página (9), donde debería ver un único producto ya que nos estamos paginar 81 productos, 10 productos a la vez. Eliminar este producto.

Tras eliminar el último producto, el control GridView *debe* automáticamente, vaya a la página octava y esta funcionalidad se logran con la paginación predeterminada. Con la paginación personalizada, sin embargo, después de eliminar el último producto en la última página, el control GridView simplemente desaparece de la pantalla por completo. El motivo exacto *¿por qué* esto sucede es un poco más allá del ámbito de este tutorial; vea [eliminar el último registro en la última página de un control GridView con paginación personalizada](http://scottonwriting.net/sowblog/posts/7326.aspx) para obtener los detalles de bajo nivel sobre el origen de Este problema. En resumen, s debido a la siguiente secuencia de pasos que se realizan mediante el control GridView cuando se hace clic en el botón Eliminar:

1. Eliminar el registro
2. Obtener los registros apropiados para mostrar para el elemento especificado `PageIndex` y `PageSize`
3. Compruebe que la `PageIndex` no supera el número de páginas de datos del origen de datos; si lo automáticamente, disminuir la s GridView `PageIndex` propiedad
4. Enlazar la página adecuada de los datos en GridView con los registros obtenidos en el paso 2

El problema proviene del hecho ese paso 2 en el `PageIndex` usa al tomar los registros que se va a mostrar sigue siendo el `PageIndex` de la última página cuyo registro único solo se ha eliminado. Por lo tanto, en el paso 2, *ningún* se devuelven registros desde esa última página de datos ya no contiene ningún registro. A continuación, en el paso 3, el control GridView es consciente de que su `PageIndex` propiedad es mayor que el número total de páginas en el origen de datos (desde que se ve eliminado el último registro de la última página) y, por tanto, disminuye su `PageIndex` propiedad. En el paso 4 GridView intenta enlazar a los datos recuperados en el paso 2. Sin embargo, en el paso 2 se han devuelto ningún registro, por lo tanto, lo que en un GridView vacío. Con la paginación de forma predeterminada, este problema tipo t superficie porque en el paso 2 *todas* se recuperan los registros del origen de datos.

Para solucionar este problema, tiene dos opciones. La primera consiste en crear un controlador de eventos para el s GridView `RowDeleted` controlador de eventos que determina cuántos registros se muestran en la página que acaba de eliminar. Si se ha producido un único registro, a continuación, el registro que acaba de eliminar debe haber sido la última de ellas y necesitamos reducir la s GridView `PageIndex`. Por supuesto, sólo vamos a actualizar el `PageIndex` si la operación de eliminación fue realmente correcta, que se puede determinar si se asegura que el `e.Exception` propiedad es `null`.

Este enfoque funciona porque actualiza el `PageIndex` después del paso 1, pero antes del paso 2. Por lo tanto, en el paso 2, se devuelve el conjunto de registros adecuado. Para ello, use código similar al siguiente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Una solución alternativa es crear un controlador de eventos para el s ObjectDataSource `RowDeleted` eventos y establecer el `AffectedRows` propiedad con un valor de 1. Después de eliminar el registro en el paso 1 (pero antes de volver a recuperar los datos en el paso 2), el control GridView actualiza su `PageIndex` propiedad si uno o más filas afectadas por la operación. Sin embargo, el `AffectedRows` ObjectDataSource no establece la propiedad y, por tanto, se omite este paso. Es una manera de tener este paso ejecuta establecer manualmente el `AffectedRows` propiedad si la operación de eliminación se completa correctamente. Esto puede lograrse mediante código similar al siguiente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

El código para ambos de estos controladores de eventos puede encontrarse en la clase de código subyacente de la `EfficientPaging.aspx` ejemplo.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparar el rendimiento de forma predeterminada y la paginación personalizada

Puesto que la paginación personalizada solo recupera los registros necesarios, mientras que la paginación predeterminada devuelve *todas* de los registros de cada página que se está viendo, lo s borrar que es más eficaz que la paginación predeterminada paginación personalizada. ¿Pero el solo hecho mucho más eficaz es la paginación personalizada? ¿Qué tipo de mejoras de rendimiento puede verse moviendo la paginación predeterminada a la paginación personalizada?

Por desgracia, s hay no hay un tamaño se ajusta a todo responder aquí. La ganancia de rendimiento depende de una serie de factores, más destacada dos el número de registros que se va a paginar a través y la carga se colocan en la base de datos servidor y canales de comunicación entre el servidor web y el servidor de base de datos. Para tablas pequeñas con unos pocos docenas registros, la diferencia de rendimiento puede ser insignificante. Sin embargo, para tablas grandes, con miles de cientos de miles de filas, la diferencia de rendimiento es aguda.

Un artículo mío, [paginación personalizada en ASP.NET 2.0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene algunas pruebas de rendimiento se ejecutó para presentar las diferencias de rendimiento entre estas dos técnicas de paginación al paginar a través de una tabla de base de datos con 50.000 registros. En estas pruebas examinar tanto el tiempo para ejecutar la consulta en el nivel de SQL Server (mediante [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) y en la página ASP.NET mediante [características de seguimiento de ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenga en cuenta que estas pruebas eran ejecutar en mi cuadro de desarrollo con un solo usuario activo y por lo tanto, son poco científicos y no imitar los patrones de carga del sitio Web típico. No obstante, los resultados muestran las diferencias relativas en tiempo de ejecución de forma predeterminada y la paginación personalizada al trabajar con lo suficientemente grandes cantidades de datos.


|  | **Promedio Duración (s)** | **Lecturas** |
| --- | --- | --- |
| **Paginación a SQL Profiler predeterminada** | 1.411 | 383 |
| **Profiler SQL de paginación personalizada** | 0.002 | 29 |
| **Seguimiento de ASP.NET de paginación predeterminado** | 2.379 | *N/D* |
| **Seguimiento de ASP.NET de paginación personalizada** | 0.029 | *N/D* |


Como puede ver, recuperar una determinada página de datos requerido por término medio 354 menos lecturas y finalizar en una fracción del tiempo. En la página ASP.NET personalizado la página fue capaz de representar en cerca de 1/100<sup>th</sup> del tiempo que tardó la paginación de forma predeterminada. Consulte [mi artículo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) para obtener más información sobre estos resultados junto con una base de datos y el código se puede descargar para reproducir estas pruebas en su propio entorno.

## <a name="summary"></a>Resumen

Paginación predeterminada es muy fácil de implementar sólo comprobación de la casilla de verificación Habilitar paginación en la etiqueta inteligente de datos Web control s pero tal simplicidad proviene a costa del rendimiento. Con la paginación de forma predeterminada, cuando un usuario solicita cualquier página de datos *todas* se devuelven registros, aunque se muestren solamente una pequeña fracción de ellos. Para combatir esta sobrecarga de rendimiento, el origen ObjectDataSource ofrece una alternativa paginación personalizada de opción de paginación.

Mientras mejora la paginación personalizada predeterminada mediante la recuperación solo aquellos registros que deben mostrarse, problemas de rendimiento de s de paginación s más complicada implementar la paginación personalizada. En primer lugar, debe escribir una consulta que correctamente (y eficaz) tenga acceso el subconjunto específico de registros solicitados. Esto puede realizarse de varias maneras; lo hemos visto en este tutorial es usar SQL Server 2005 s nuevo `ROW_NUMBER()` da como resultado de la función de rango y, a continuación, para sólo los devolver resultados cuya clasificación se encuentra dentro de un intervalo especificado. Además, tenemos que agregar una forma de determinar el número total de registros que se va a paginar a través. Después de crear estos métodos DAL y BLL, también es necesario configurar el origen ObjectDataSource para que pueda determinar el número total de registros son que se va a paginar a través y correctamente pueden pasar los valores de índice de fila inicial y el número máximo de filas a la capa BLL.

Mientras que implementar la paginación personalizada requieren una serie de pasos y es casi no es tan sencillo como la paginación predeterminada, la paginación personalizada es una necesidad al paginar a través de lo suficientemente grandes cantidades de datos. A medida que examinan los resultados de la paginación personalizada, se demostró puede arrojar a segundos fuera el tiempo de procesamiento de la página ASP.NET y puede aclarar la carga en el servidor de base de datos en uno o más órdenes de magnitud.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](paging-and-sorting-report-data-cs.md)
> [Siguiente](sorting-custom-paged-data-cs.md)
