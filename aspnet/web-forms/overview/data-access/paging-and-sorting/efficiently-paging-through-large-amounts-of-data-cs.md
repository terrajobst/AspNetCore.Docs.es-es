---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: "Eficazmente paginar a través de grandes cantidades de datos (C#) | Documentos de Microsoft"
author: rick-anderson
description: "La opción de paginación predeterminada de un control de presentación de datos no es apropiada cuando se trabaja con grandes cantidades de datos, como su retriev de control de origen de datos subyacente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 5188c7119260e36eb61e9f57cadce29fefdd8016
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Eficazmente paginar a través de grandes cantidades de datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) o [descarga de PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> La opción de paginación predeterminada de un control de presentación de datos no es apropiada cuando se trabaja con grandes cantidades de datos, como el control de origen de datos subyacente recupera todos los registros, aunque se muestra solo un subconjunto de datos. En estas circunstancias, debemos activamos personalizados a la paginación.


## <a name="introduction"></a>Introducción

Como se explicó en el tutorial anterior, se puede implementar la paginación en uno de dos maneras:

- **Paginación predeterminada** se puede implementar simplemente comprobando la opción Habilitar paginación en el control de Web s de datos de etiqueta inteligente; sin embargo, cuando ve una página de datos, recupera el ObjectDataSource *todos los* de registros, incluso Aunque solo un subconjunto de ellos se muestran en la página
- **Paginación personalizada** mejora el rendimiento de manera predeterminada paginación al recuperar solo los registros de la base de datos que necesitan que se mostrará para la página concreta de los datos solicitados por el usuario; sin embargo, la paginación personalizada implica un poco más esfuerzo para implementar que la paginación de manera predeterminada

Debido a la facilidad de comprobación de implementación solo una casilla de verificación y que se van a terminado. paginación predeterminada es una opción atractiva. Su enfoque de tegorías na recuperar todos los registros, sin embargo, resulta una opción plausible al paginar a través de suficientemente grandes cantidades de datos o para sitios con muchos usuarios simultáneos. En estas circunstancias, debemos activamos en personalizada con el fin de proporcionar un sistema eficaz de paginación.

El desafío de paginación personalizada es poder escribir una consulta que devuelve el conjunto de registros necesarios para una determinada página de datos. Afortunadamente, Microsoft SQL Server 2005 proporciona una nueva palabra clave para los resultados de clasificación, que permite escribir una consulta que puede recuperar el subconjunto correcto de registros de forma eficaz. En este tutorial veremos cómo usar esta nueva palabra clave de SQL Server 2005 para implementar la paginación personalizada en un control GridView. Mientras que es idéntica a la de paginación de forma predeterminada, ejecución paso a paso de una página a la siguiente mediante la interfaz de usuario de paginación personalizada paginación personalizada puede ser varias órdenes de magnitud más rápido que la paginación de manera predeterminada.

> [!NOTE]
> La ganancia de rendimiento exacto mostrada por la paginación personalizada depende del número total de registros que se va a paginar a través de y la carga que se ubican en el servidor de base de datos. Al final de este tutorial analizaremos algunas métricas aproximadas que muestran las ventajas de rendimiento obtenido a través de la paginación personalizada.


## <a name="step-1-understanding-the-custom-paging-process"></a>Paso 1: Descripción del proceso de paginación personalizada

Al paginar a través de los datos, los registros precisos de una página dependen de la página de datos que se solicita y el número de registros mostrados por página. Por ejemplo, imagine que deseamos paginar a través de los 81 productos, mostrar los 10 productos más vendidos por página. Al ver la primera página, d queremos productos 1 a 10; al ver la segunda página se d esté interesado en 11 20 a través de productos y así sucesivamente.

Hay tres variables que obliguen a lo que deben recuperarse de los registros y cómo se debe representar la interfaz de paginación:

- **Índice de fila inicial** el índice de la primera fila en la página de datos que se va a mostrar; esto puede de índice se calcula multiplicando el índice de la página por los registros que se va a mostrar en cada página y agregar uno. Por ejemplo, al paginar a través de registros de 10 a la vez, para la primera página (cuyo índice de la página es 0), el índice de fila inicial es 0 \* 10 + 1 ó 1; para la segunda página (cuyo índice de la página es 1), el índice de fila inicial es 1 \* 10 + 1 , o 11.
- **Número máximo de filas** el número máximo de registros que se va a mostrar en cada página. Esta variable se conoce como el número máximo de filas ya durante la última página se puede ser menos registros devueltos que el tamaño de página. Por ejemplo, al paginar a través de los registros de 81 productos 10 por página, la página novena y final tendrá un solo registro. Sin embargo, no hay ninguna página mostrará más registros que el valor de número máximo de filas.
- **Número de registros total** el número total de registros que se va a paginar a través de. Aunque esta variable no es necesario para determinar qué registros para recuperar para una página determinada, indican la interfaz de paginación. Por ejemplo, si hay 81 productos que se va a paginar a través de, la interfaz de paginación sepa que puede para mostrar los nueve números de página en la interfaz de usuario de paginación.

Con la paginación de manera predeterminada, el índice de fila inicial se calcula como el producto del índice de la página y el tamaño de página más uno, mientras que el número máximo de filas es simplemente el tamaño de página. Puesto que la paginación predeterminada recupera todos los registros de la base de datos cuando se procesa cualquier página de datos, el índice para cada fila se conoce, haciéndolos móvil a la fila de índice de fila de iniciar una tarea trivial. Además, el número Total de registros está disponible fácilmente, tal y como s simplemente el número de registros en la tabla de datos (o cualquier objeto se usa para almacenar los resultados de la base de datos).

Dado que las variables de índice de fila inicial y el número máximo de filas, una implementación de paginación personalizada debe devolver solo el subconjunto de registros empezando por el índice de fila inicial y hasta el número máximo de filas de registros después de precisa. Paginación personalizada proporciona dos desafíos:

- Debemos ser capaces de forma eficaz asociar un índice de fila a cada fila de todos los datos que se va a paginar a través de para que podamos comenzar a devolver los registros en el índice de fila de inicio especificado
- Es necesario proporcionar el número total de registros que se va a paginar a través de

En los dos pasos siguientes examinaremos la secuencia de comandos SQL necesaria para responder a estas dos desafíos. Además de la secuencia de comandos SQL, se necesitará implementar métodos en la capa DAL y BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Paso 2: Devolver el número Total de registros que se va a paginar a través de

Antes de que se examina cómo recuperar el subconjunto de registros para que se va a mostrar la página preciso, permiten s primero observaremos cómo devolver el número total de registros que se va a paginar a través de. Esta información es necesaria para configurar correctamente la interfaz de usuario de paginación. El número total de registros devueltos por una consulta SQL determinada puede obtenerse mediante la [ `COUNT` función de agregado](https://msdn.microsoft.com/en-US/library/ms175997.aspx). Por ejemplo, para determinar el número total de registros en la `Products` tabla, podemos usar la consulta siguiente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Permitir s Agregar un método a nuestro DAL que devuelve esta información. En concreto, vamos a crear un método de la capa DAL llamado `TotalNumberOfProducts()` que se ejecuta el `SELECT` instrucción mostrada anteriormente.

Comience abriendo la `Northwind.xsd` archivo de conjunto de datos con tipo en el `App_Code/DAL` carpeta. A continuación, haga doble clic en el `ProductsTableAdapter` en el diseñador y elija Agregar consulta. Como se ha visto en tutoriales anteriores, esto nos permitirá agregar un nuevo método a la capa DAL que, cuando se invoca, se ejecutará una instrucción SQL determinada o un procedimiento almacenado. Al igual que con nuestros métodos de TableAdapter en tutoriales anteriores, para este optar por utilizar una instrucción de SQL ad hoc.


![Utilizar una instrucción de SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figura 1**: utilizar una instrucción de SQL Ad Hoc


En la siguiente pantalla se puede especificar qué tipo de consulta para crear. Puesto que esta consulta devolverá un único valor escalar el número total de registros en la `Products` Elegir tabla el `SELECT` que devuelve una opción de valor único.


![Configurar la consulta para utilizar una instrucción SELECT que devuelve un valor único](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figura 2**: configurar la consulta para utilizar una instrucción SELECT que devuelve un valor único


Después de que indica el tipo de consulta que se utilizará, debemos especificamos a continuación la consulta.


![Use el SELECT Count de consulta de productos](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figura 3**: usan el número de SELECT (\*) FROM productos consulta


Por último, especifique el nombre del método. Como s mencionado anteriormente, permiten usar `TotalNumberOfProducts`.


![Nombre del método de la capa DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figura 4**: nombre de la capa DAL método TotalNumberOfProducts


Después de hacer clic en Finalizar, el asistente agregará la `TotalNumberOfProducts` método a la capa DAL. Los métodos de devolución escalares en la capa DAL devuelven tipos que aceptan valores NULL, en caso de que el resultado de la consulta SQL es `NULL`. Nuestro `COUNT` consulta, sin embargo, siempre devolverá no`NULL` valor; en cualquier caso, el método de la capa DAL devuelve un entero que acepta valores NULL.

Además del método de la capa DAL, también se necesita un método en la capa BLL. Abra la `ProductsBLL` archivo de clase y agregue un `TotalNumberOfProducts` método que simplemente llama hacia abajo a la capa DAL s `TotalNumberOfProducts` método:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

Las operaciones de asignación DAL `TotalNumberOfProducts` método devuelve un entero que acepta valores NULL; sin embargo, se ha creado la `ProductsBLL` clase s. `TotalNumberOfProducts` método para que devuelva un entero estándar. Por lo tanto, es necesario tener la `ProductsBLL` clase s `TotalNumberOfProducts` método devuelve la parte del valor del entero que acepta valores NULL devuelto por las operaciones de asignación DAL `TotalNumberOfProducts` método. La llamada a `GetValueOrDefault()` devuelve el valor del entero que acepta valores NULL, si existe; si el entero que acepta valores NULL es `null`, sin embargo, devuelve el valor de entero del valor predeterminado, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Paso 3: Devolver el subconjunto preciso de registros

La siguiente tarea consiste en crear métodos en la capa DAL y BLL que aceptan el índice de fila inicial y las variables de número máximo de filas expuesto anteriormente y devuelvan los registros correspondientes. Antes de hacerlo, permiten s busque primero en la secuencia de comandos SQL necesario. El desafío nos encontramos es que debemos ser capaz de asignar eficazmente un índice para cada fila de todos los resultados que se va a paginar a través de para que podamos devolvemos aquellos registros empezando por el índice de fila inicial (y hasta el número máximo de registros de registros).

Esto no es un desafío si ya existe una columna en la tabla de base de datos que actúa como un índice de fila. A primera vista tengamos que creemos que la `Products` tabla s `ProductID` campo sería suficiente, ya que el primer producto tiene `ProductID` de 1, el segundo un 2, y así sucesivamente. Sin embargo, la eliminación de un producto deja un espacio en la secuencia, anulando este enfoque.

Hay dos técnicas generales que permiten asociar eficazmente un índice de fila a los datos a la página, lo que permite el subconjunto de registros que va a recuperar preciso:

- **Con SQL Server 2005 s `ROW_NUMBER()` palabra clave** familiarizado con SQL Server 2005, la `ROW_NUMBER()` palabra clave asocia una clasificación de cada registro devuelto basándose en el orden de algunos. Esta clasificación se puede usar como un índice de fila para cada fila.
- **Usar una Variable de tabla y `SET ROWCOUNT`**  s de SQL Server [ `SET ROWCOUNT` instrucción](https://msdn.microsoft.com/en-us/library/ms188774.aspx) puede utilizarse para especificar el número total de registros debe procesar una consulta antes de terminar; [las variables de tabla](http://www.sqlteam.com/item.asp?ItemID=9454) son variables locales de T-SQL que pueden contener datos tabulares, akin a [tablas temporales](http://www.sqlteam.com/item.asp?ItemID=2029). Este enfoque funciona igualmente bien con Microsoft SQL Server 2005 y SQL Server 2000 (mientras que el `ROW_NUMBER()` enfoque sólo funciona con SQL Server 2005).  
  
 La idea es crear una variable de tabla que tiene un `IDENTITY` y columnas para las claves principales de la tabla cuyos datos se envía un mensaje a través de. A continuación, se vuelca el contenido de la tabla cuyos datos se envía un mensaje a través de en la variable de tabla, con lo que se asocia un índice de fila secuenciales (a través de la `IDENTITY` columna) para cada registro en la tabla. Una vez que se ha rellenado la variable de tabla, un `SELECT` instrucción en la variable de tabla, se combina con la tabla subyacente, se puede ejecutar para extraer los registros determinados. El `SET ROWCOUNT` instrucción se utiliza para limitar el número de registros que necesita para realizar el volcado en la variable de tabla de forma inteligente.  
  
 Esta eficacia enfoque s se basa en el número de página que se solicita, como la `SET ROWCOUNT` valor se asigna el valor de índice de fila inicial más el número máximo de filas. Al paginar a través de páginas impares baja como la primera pocas páginas de datos de este enfoque es muy eficaz. Sin embargo, exhibe performance de paginación de forma predeterminada cuando se recuperan de una página cuando se acerque el final.

Este tutorial implementa la paginación personalizado con el `ROW_NUMBER()` palabra clave. Para obtener más información sobre el uso de la variable de tabla y `SET ROWCOUNT` técnica, consulte [A varios eficaz métodos para paginar a través de grandes conjuntos de resultados](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

El `ROW_NUMBER()` palabra clave asociada a una categoría cada registro devuelto en un orden concreto utilizando la sintaxis siguiente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()`Devuelve un valor numérico que especifica el rango de cada registro con respecto a que el orden indicado. Por ejemplo, para ver el rango de cada producto, ordenada de mayor cara a la menor, podríamos usar la siguiente consulta:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Figura 5 se muestra esta consulta resultados de s cuando se ejecuta a través de la ventana de consulta en Visual Studio. Tenga en cuenta que los productos se ordenan por precio, junto con un rango de precio para cada fila.


![Se incluye el rango de precio para cada registro devuelto](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figura 5**: incluido el rango de precio de cada registro devuelto


> [!NOTE]
> `ROW_NUMBER()`es simplemente una de las muchas nuevas funciones de clasificación disponibles en SQL Server 2005. Para obtener una explicación más exhaustiva de `ROW_NUMBER()`, junto con las demás funciones de clasificación, leer [devolver resultados clasificados con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Cuando los resultados de clasificación según los valores especificados `ORDER BY` columna en el `OVER` cláusula (`UnitPrice`, en el ejemplo anterior), SQL Server debe ordenar los resultados. Esto es una operación rápida si hay un índice clúster sobre las columnas que se va a se ordenan los resultados por, o si hay una cobertura de índice, pero puede ser más costoso en caso contrario. Con el fin de mejorar el rendimiento de las consultas lo suficientemente grandes, considere la posibilidad de agregar un índice no agrupado de la columna por la que los resultados se ordenan por. Vea [funciones de categoría y el rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obtener una visión más detallada en las consideraciones de rendimiento.

La información de clasificación devuelta por `ROW_NUMBER()` no se pueden usar directamente en el `WHERE` cláusula. Sin embargo, puede usarse una tabla derivada para devolver el `ROW_NUMBER()` resultado, que, a continuación, puede aparecer en el `WHERE` cláusula. Por ejemplo, la consulta siguiente utiliza una tabla derivada para devolver las columnas ProductName y UnitPrice, junto con el `ROW_NUMBER()` resultado y, a continuación, utiliza un `WHERE` cláusula para devolver únicamente los productos cuya clasificación precio es entre 11 y 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Extender un poco más este concepto, podemos utilizar este enfoque para recuperar una página específica de datos dados los valores de índice de fila inicial y el número máximo de filas deseados:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Como veremos más adelante en este tutorial, el  *`StartRowIndex`*  proporcionado por el ObjectDataSource se indiza empezando por cero, mientras que la `ROW_NUMBER()` valor devuelto por SQL Server 2005 se indiza empezando por 1. Por lo tanto, la `WHERE` cláusula devuelve los registros donde `PriceRank` es estrictamente mayor que  *`StartRowIndex`*  y menor o igual que  *`StartRowIndex`*   +  *`MaximumRows`*.


Ahora que nos ha explica cómo `ROW_NUMBER()` puede ser utilizado para recuperar una página determinada de dados los valores de índice de fila inicial y el número máximo de filas de datos, ahora debemos implementar esta lógica como métodos en la capa DAL y BLL.

Al crear esta consulta que debemos decidir la ordenación por el que los resultados se clasificará; permiten s ordenar los productos por su nombre en orden alfabético. Esto significa que con la implementación de paginación personalizada en este tutorial no se será capaz de crear un informe paginado personalizado que también se pueden ordenar. En el tutorial siguiente, sin embargo, veremos cómo se puede proporcionar esta funcionalidad.

En la sección anterior, creamos el método DAL instrucción SQL ad hoc. Desafortunadamente, el analizador de T-SQL en Visual Studio usa TableAdapter Asistente t como el `OVER` sintaxis utilizada por el `ROW_NUMBER()` (función). Por lo tanto, este método DAL que debemos crear como un procedimiento almacenado. Seleccione el Explorador de servidores desde el menú Ver (o el posicionamiento Ctrl + Alt + S) y expanda el `NORTHWND.MDF` nodo. Para agregar un nuevo procedimiento almacenado, haga doble clic en el nodo procedimientos almacenados y elija Agregar un nuevo procedimiento almacenado (consulte la figura 6).


![Agregar un nuevo procedimiento almacenado para la paginación a través de los productos](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figura 6**: agregar un nuevo procedimiento almacenado para la paginación a través de los productos


Este procedimiento almacenado debe aceptar dos parámetros de entrada enteros - `@startRowIndex` y `@maximumRows` y use la `ROW_NUMBER()` función ordenados por la `ProductName` campo, devolver sólo aquellas filas mayor que el especificado `@startRowIndex` y menor que o igual que `@startRowIndex`  +  `@maximumRow` s. Escriba el siguiente script en el nuevo procedimiento almacenado y, a continuación, haga clic en el icono de guardar para agregar el procedimiento almacenado a la base de datos.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Después de crear el procedimiento almacenado, tómese un momento para probarlo. Haga doble clic en el `GetProductsPaged` procedimiento almacenado un nombre en el Explorador de servidores y elija la opción de ejecución. Visual Studio le pedirá que, a continuación, para los parámetros de entrada, `@startRowIndex` y `@maximumRow` s (consulte la figura 7). Probar valores diferentes y examinar los resultados.


![Escriba un valor para el @startRowIndex y @maximumRows parámetros](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

**Figura 7**: escriba un valor para el @startRowIndex y @maximumRows parámetros


Después de elegir estos valores de parámetros de entrada, la ventana de salida muestra los resultados. La figura 8 muestra los resultados cuando se pasan en 10 tanto para el `@startRowIndex` y `@maximumRows` parámetros.


[![Se devuelven los registros que aparecería en la segunda página de datos](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figura 8**: los registros que aparecería en la segunda página de datos se devuelven ([haga clic aquí para ver la imagen a tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Con este procedimiento almacenado creado, se re listo para crear el `ProductsTableAdapter` método. Abra la `Northwind.xsd` Typed DataSet, menú contextual en el `ProductsTableAdapter`y elija la opción de Agregar consulta. En lugar de crear la consulta mediante una instrucción SQL ad hoc, crearlo mediante un procedimiento almacenado existente.


![Cree el método de la capa DAL mediante un procedimiento almacenado existente](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Figura 9**: crear el método de DAL mediante un procedimiento almacenado existente


A continuación, se nos pide que seleccione el procedimiento almacenado para invocar. Elegir el `GetProductsPaged` el procedimiento almacenado de la lista desplegable.


![Elija la GetProductsPaged el procedimiento almacenado de la lista desplegable](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figura 10**: elija la GetProductsPaged el procedimiento almacenado de la lista desplegable


La siguiente pantalla, a continuación, le pregunta qué tipo de datos devuelto por el procedimiento almacenado: datos tabulares, un valor único o ningún valor. Puesto que la `GetProductsPaged` procedimiento almacenado puede devolver varios registros, indique que devuelve datos tabulares.


![Indicar que el procedimiento almacenado devuelve datos tabulares](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figura 11**: indica que el procedimiento almacenado devuelve datos tabulares


Por último, se indican los nombres de los métodos que desea crear. Al igual que con nuestros tutoriales anteriores, continúe y crear un objeto DataTable de métodos que utilizan tanto el relleno y devolver un DataTable. El primer método el nombre `FillPaged` y el segundo `GetProductsPaged`.


![Nombre de lo métodos FillPaged y GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figura 12**: nombre de lo métodos FillPaged y GetProductsPaged


Por otra parte para crear un método de la capa DAL para devolver una página determinada de productos, también es necesario proporcionar esta funcionalidad en la capa BLL. Como el método de la capa DAL, la s BLL GetProductsPaged método debe aceptar dos entradas de entero para especificar el índice de fila inicial y el número máximo de filas y debe devolver solo los registros que se encuentran dentro del intervalo especificado. Cree este tipo de método BLL en la clase ProductsBLL que simplemente llamadas hacia abajo en la s DAL GetProductsPaged método, de este modo:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Puede utilizar cualquier nombre para los parámetros de entrada de método s BLL, pero, como verá en breve, decide usar `startRowIndex` y `maximumRows` nos evita adicional de un poco de trabajo al configurar un ObjectDataSource para usar este método.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Paso 4: Configurar el ObjectDataSource para utilizar la paginación personalizada

Con los métodos BLL y DAL para tener acceso a un subconjunto específico de registros completados, se re listo para crear un control GridView controlan esa páginas a través de sus registros subyacentes mediante la paginación personalizada. Comience abriendo la `EfficientPaging.aspx` página en el `PagingAndSorting` carpeta, agregar un control GridView a la página y configurarlo para usar un nuevo control ObjectDataSource. En los tutoriales anteriores, tuvimos a menudo ObjectDataSource configurado para usar el `ProductsBLL` clase s. `GetProducts` método. Esta vez, sin embargo, deseamos usar el `GetProductsPaged` método en su lugar, desde el `GetProducts` método devuelve *todos los* de los productos en la base de datos, mientras que `GetProductsPaged` devuelve solo un subconjunto específico de registros.


![Configurar el ObjectDataSource para utilizar el método de GetProductsPaged ProductsBLL clase s.](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figura 13**: configurar el ObjectDataSource para utilizar el método de GetProductsPaged ProductsBLL clase s.


Desde se re creando un GridView de solo lectura, tómese un momento para establecer la lista desplegable método en la instrucción INSERT, UPDATE y eliminar las fichas (ninguno).

A continuación, el Asistente ObjectDataSource nos solicita para los orígenes de la `GetProductsPaged` método s `startRowIndex` y `maximumRows` valores de parámetros de entrada. Estos parámetros de entrada realmente se establecerá de GridView automáticamente, por lo que simplemente deje el juego de origen en ninguno y haga clic en Finalizar.


![Deje los orígenes de parámetro de entrada como ninguno](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figura 14**: deje los orígenes de parámetro de entrada como ninguno


Después de completar al Asistente de ObjectDataSource GridView contendrá un BoundField o CampoCasillaVerificación para cada uno de los campos de datos de producto. Puede personalizar la apariencia de s GridView como considere oportuno. Se ha elegido para mostrar solamente el `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, y `UnitPrice` BoundFields. Además, configurar el control GridView para admitir paginación activando la casilla de verificación Habilitar paginación en su etiqueta inteligente. Después de estos cambios, el marcado declarativo GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Si visita la página a través de un explorador, sin embargo, el control GridView es no donde se encuentre.


![GridView no es aparece](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figura 15**: The GridView no es aparece


GridView es falta porque el ObjectDataSource está utilizando actualmente 0 como valores para ambas la `GetProductsPaged` `startRowIndex` y `maximumRows` parámetros de entrada. Por lo tanto, la consulta SQL resultante es que no devuelve ningún registro y, por tanto, no se muestra el control GridView.

Para solucionar esto, es necesario configurar el ObjectDataSource para utilizar la paginación personalizada. Esto puede realizarse en los pasos siguientes:

1. **Establecer la s ObjectDataSource `EnablePaging` propiedad `true`**  esto indica a ObjectDataSource que debe pasar a la `SelectMethod` dos parámetros adicionales: uno para especificar el índice de fila inicial ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) y otra para especificar el número máximo de filas ([`MaximumRowsParameterName`](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Establecer la s ObjectDataSource `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades según corresponda** el `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades indican los nombres de los parámetros de entrada que pasan a la `SelectMethod` para fines de paginación personalizada . De forma predeterminada, estos nombres de parámetro son `startIndexRow` y `maximumRows`, que es por este motivo, al crear el `GetProductsPaged` método en el BLL, usé estos valores para los parámetros de entrada. Si decide utilizar nombres de parámetro diferente para las operaciones de asignación BLL `GetProductsPaged` método como `startIndex` y `maxRows`, por ejemplo tendría que establecer la s ObjectDataSource `StartRowIndexParameterName` y `MaximumRowsParameterName` propiedades según corresponda (por ejemplo, startIndex para `StartRowIndexParameterName` y maxRows para `MaximumRowsParameterName`).
3. **Establecer la s ObjectDataSource [ `SelectCountMethod` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) en el nombre del método que devuelve el Total número de registros que se va a paginar a través de (`TotalNumberOfProducts`)** Recuerde que la `ProductsBLL` clase s `TotalNumberOfProducts`método devuelve el número total de registros que se va a paginar a través de un método de la capa DAL que ejecuta un `SELECT COUNT(*) FROM Products` consulta. Esta información es necesaria por el ObjectDataSource para representar correctamente la interfaz de paginación.
4. **Quitar el `startRowIndex` y `maximumRows` `<asp:Parameter>` elementos desde el marcado declarativo de ObjectDataSource s** al configurar el ObjectDataSource a través del asistente, Visual Studio agrega automáticamente dos `<asp:Parameter>` elementos de la `GetProductsPaged` método s parámetros de entrada. Estableciendo `EnablePaging` a `true`, estos parámetros se pasará automáticamente; si también aparecen en la sintaxis declarativa, ObjectDataSource intentar pasar *cuatro* parámetros para el `GetProductsPaged` (método) y dos parámetros para el `TotalNumberOfProducts` método. Si se olvida de quitar estos `<asp:Parameter>` elementos, al visitar la página a través de un explorador que se obtendrá un mensaje de error como: *ObjectDataSource 'ObjectDataSource1' no encontró un método no genérico 'TotalNumberOfProducts' que tiene parámetros: startRowIndex, maximumRows*.

Después de realizar estos cambios, la sintaxis declarativa de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Tenga en cuenta que la `EnablePaging` y `SelectCountMethod` se han establecido propiedades y `<asp:Parameter>` elementos se han quitado. La figura 16 muestra una captura de pantalla de la ventana Propiedades una vez realizados estos cambios.


![Para utilizar la paginación personalizada, configurar el Control ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figura 16**: para utilizar la paginación personalizada, configurar el Control ObjectDataSource


Después de realizar estos cambios, visite esta página a través de un explorador. Debería ver los 10 productos de la lista, ordenadas alfabéticamente. Tómese un momento para recorrer la datos página a la vez. Aunque no hay ninguna diferencia visual desde la perspectiva del usuario final s entre paginación predeterminada y la paginación personalizada, más eficazmente la paginación personalizada páginas a través de grandes cantidades de datos, ya que sólo recupera los registros que deben mostrarse para una página determinada.


[![Los datos, ordenados por el producto es nombre, está paginado utilizando paginación personalizada](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figura 17**: los datos, ordenados por el producto es nombre, está paginado utilizando paginación personalizada ([haga clic aquí para ver la imagen a tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Con la paginación personalizada, la página de recuento de valor devuelto por las operaciones de asignación ObjectDataSource `SelectCountMethod` se almacena en el estado de vista de s de GridView. Otras variables de GridView el `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` colección y así sucesivamente se almacenan en *el estado de control*, que se mantiene independientemente del valor de las operaciones de asignación GridView `EnableViewState` propiedad. Puesto que el `PageCount` valor se conserva entre las devoluciones de datos usando el estado de vista, cuando se usa una interfaz de paginación que incluye un vínculo para ir a la última página, es imperativo que esté habilitado el estado de vista de s de GridView. (Si la interfaz de paginación no incluye un vínculo directo a la última página, a continuación, puede deshabilitar el estado de vista).


Haga clic en el último vínculo de página provoca una devolución de datos e indica a la GridView para actualizar su `PageIndex` propiedad. Si se hace clic en el último vínculo de página, el control GridView asigna su `PageIndex` propiedad con un valor uno menor que su `PageCount` propiedad. Con estado de vista deshabilitado, la `PageCount` valor se pierde en las devoluciones y `PageIndex` se asigna el valor entero máximo en su lugar. A continuación, el control GridView intenta determinar el índice de fila inicial multiplicando el `PageSize` y `PageCount` propiedades. Esto da como resultado un `OverflowException` ya que el producto supera el tamaño máximo entero permitido.

## <a name="implement-custom-paging-and-sorting"></a>Implemente la paginación personalizada y la ordenación

Nuestra implementación de paginación personalizada actual requiere que se especifique el orden por el que se paginan los datos a través de forma estática al crear la `GetProductsPaged` procedimiento almacenado. Sin embargo, puede haber tomado nota de que la etiqueta inteligente de GridView s contiene una casilla de verificación Habilitar ordenación además de la opción de habilitar la paginación. Por desgracia, agregar compatibilidad con la ordenación a la GridView con nuestra implementación de paginación personalizada actual, solo se ordenarán los registros en la página de datos está viendo actualmente. Por ejemplo, si configura el control GridView para también admitir paginación y, a continuación, al ver la primera página de datos, ordenar por nombre de producto en orden descendente, invertirá el orden de los productos en la página 1. Como se muestra en la figura 18, por ejemplo, muestra tigre Carnarvon como el primer producto al ordenar en orden alfabético inverso, que omite 71 otros productos que venga después tigre Carnarvon, por orden alfabético; solo los registros en la primera página se consideran en la ordenación.


[![Solo los datos se muestran en la página actual está ordenada](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figura 18**: solo los datos se muestran en la página actual se ordena ([haga clic aquí para ver la imagen a tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


La ordenación sólo se aplica a la página actual de datos porque la ordenación se está produciendo una vez recuperados los datos de las operaciones de asignación BLL `GetProductsPaged` método y este método solo devuelve los registros de la página específica. Para implementar correctamente una ordenación, es necesario pasar la expresión de ordenación para el `GetProductsPaged` método para que los datos se pueden clasificar adecuadamente antes de devolver la página específica de los datos. Veremos cómo hacerlo en nuestro tutorial siguiente.

## <a name="implementing-custom-paging-and-deleting"></a>Implementación personalizada de paginación y eliminar

Si es habilitar la funcionalidad de eliminación en un control GridView cuyos datos se paginan mediante técnicas de paginación personalizada que encontrará al eliminar el último registro de la última página, GridView desaparece en lugar de disminuir de forma adecuada la s de GridView `PageIndex`. Para reproducir este error, habilitar la eliminación para el tutorial simplemente que acabamos de crear. Vaya a la última página (9), donde debería ver un único producto ya que nos estamos paginar a través de 81 productos, 10 productos a la vez. Eliminar este producto.

Cuando se elimina el último producto, GridView *debe* ir automáticamente a la página octava y esta funcionalidad se logran con paginación predeterminada. Con la paginación personalizada, sin embargo, después de eliminar ese producto en la última página, última GridView simplemente desaparece de la pantalla por completo. El motivo exacto *¿por qué* esto sucede es un poco más allá del ámbito de este tutorial, consulte [eliminar el último registro de la última página de un control GridView con paginación personalizada](http://scottonwriting.net/sowblog/posts/7326.aspx) para obtener los detalles de bajo nivel en cuanto a la fuente de Este problema. En resumen, s debido a la siguiente secuencia de pasos que se realizan mediante el control GridView cuando se hace clic en el botón Eliminar:

1. Eliminar el registro
2. Obtener los registros adecuados para mostrar para el elemento especificado `PageIndex` y`PageSize`
3. Asegúrese de que el `PageIndex` no supera el número de páginas de datos en el origen de datos; si lo automáticamente, disminuir la s GridView `PageIndex` propiedad
4. Enlazar la página apropiada de los datos en GridView con los registros obtenidos en el paso 2

El problema proviene del hecho de ese paso 2 en el `PageIndex` utiliza al arrastrar los registros que se va a mostrar sigue siendo el `PageIndex` de la última página cuyo registro único solo se ha eliminado. Por lo tanto, en el paso 2, *no* se devuelven registros desde esa última página de datos ya no contiene ningún registro. A continuación, en el paso 3, GridView es consciente de que su `PageIndex` propiedad es mayor que el número total de páginas en el origen de datos (desde que se ha eliminado el último registro de la última página) y, por tanto, disminuye su `PageIndex` propiedad. En el paso 4 GridView intenta enlazar a los datos recuperados en el paso 2; Sin embargo, en el paso 2 no se han devuelto registros, lo que un GridView vacía. Con la paginación de manera predeterminada, este problema tipo t superficie porque en el paso 2 *todos los* se recuperan los registros del origen de datos.

Para solucionar este problema, se tiene dos opciones. La primera consiste en crear un controlador de eventos para el s GridView `RowDeleted` controlador de eventos que determina el número de registros se muestra en la página que acaba de eliminar. Si se produjo un único registro, a continuación, el registro eliminado solo debe haber sido la última de ellas y necesitamos reducir las operaciones de asignación GridView `PageIndex`. Por supuesto, solamente queremos actualizar la `PageIndex` si la operación de eliminación fue realmente correcta, que se puede determinar asegurándose de que el `e.Exception` propiedad es `null`.

Este enfoque funciona porque actualiza el `PageIndex` después del paso 1, pero antes del paso 2. Por lo tanto, en el paso 2, se devuelve el conjunto de registros adecuado. Para ello, use código similar al siguiente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Una solución alternativa consiste en crear un controlador de eventos para el s ObjectDataSource `RowDeleted` eventos y para establecer el `AffectedRows` en un valor de 1. Después de eliminar el registro en el paso 1 (pero antes de volver a recuperar los datos en el paso 2), se actualiza el control GridView su `PageIndex` propiedad si una o varias filas afectadas por la operación. Sin embargo, la `AffectedRows` ObjectDataSource no establece la propiedad y, por tanto, se omite este paso. Una manera de que este paso ejecuta consiste en definir manualmente la `AffectedRows` propiedad si la operación de eliminación se realiza correctamente. Esto puede realizarse mediante código similar al siguiente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

El código para ambos de estos controladores de eventos puede encontrarse en la clase de código subyacente de la `EfficientPaging.aspx` ejemplo.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparar el rendimiento de manera predeterminada y la paginación personalizada

Puesto que la paginación personalizada solo recupera los registros necesarios, mientras que devuelve la paginación predeterminada *todos los* de los registros para cada página que se está viendo, s borrar que paginación personalizada es más eficaz que la paginación de manera predeterminada. ¿Pero simplemente cuánto más eficaz es paginación personalizada? ¿Qué tipo de mejoras de rendimiento puede verse moviendo paginación predeterminada a la paginación personalizada?

Desafortunadamente, hay s ningún talla única todos responder aquí. La ganancia de rendimiento depende de una serie de factores, más destacada dos está el número de registros que se va a paginar a través de y la carga se colocan en la base de datos servidor y canales de comunicación entre el servidor web y el servidor de base de datos. Para tablas pequeñas con unos pocos docenas registros, la diferencia de rendimiento puede ser insignificante. Sin embargo, para las tablas grandes, con miles de cientos de miles de filas, la diferencia de rendimiento es aguda.

Un artículo mío, [paginación personalizada en ASP.NET 2.0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene algunas pruebas de rendimiento ejecutó para presentar las diferencias de rendimiento entre estas dos técnicas de paginación al paginar a través de una tabla de base de datos con 50.000 registros. En estas pruebas examina tanto el tiempo para ejecutar la consulta en el nivel de SQL Server (mediante [SQL Profiler](https://msdn.microsoft.com/en-us/library/ms173757.aspx)) y en la página ASP.NET mediante [características de seguimiento de ASP.NET s](https://msdn.microsoft.com/en-US/library/y13fw6we.aspx). Tenga en cuenta que estas pruebas estaban ejecutar en el cuadro de desarrollo con un solo usuario activo, por lo tanto no científicos y son no imitar patrones de carga del sitio Web típica. En cualquier caso, los resultados de mostrar las diferencias relativas en tiempo de ejecución de forma predeterminada y la paginación personalizada cuando se trabaja con suficientemente grandes cantidades de datos.


|  | **AVG. Duración (seg.)** | **Lecturas** |
| --- | --- | --- |
| **Analizador de SQL de paginación de manera predeterminada** | 1.411 | 383 |
| **Analizador de SQL paginación personalizado** | 0.002 | 29 |
| **Seguimiento de ASP.NET de paginación predeterminado** | 2.379 | *N/D* |
| **Seguimiento de ASP.NET de paginación personalizada** | 0.029 | *N/D* |


Como puede ver, recuperar una página concreta de los datos necesarios 354 menos lecturas en promedio y completadas en una fracción del tiempo. En la página ASP.NET, personalizado la página era puede representar en el próximo a 1/100<sup>th</sup> del tiempo que tardó cuando use la paginación de manera predeterminada. Vea [mi artículo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) para obtener más información sobre estos resultados junto con el código y una base de datos puede descargar para reproducir estas pruebas en su propio entorno.

## <a name="summary"></a>Resumen

Paginación predeterminada es muy fácil de implementar la casilla de verificación Habilitar paginación de comprobación solo en la etiqueta inteligente de datos Web control s pero tal simplicidad a costa de rendimiento. Con la paginación de manera predeterminada, cuando un usuario solicita cualquier página de datos *todos los* se devuelven registros, aunque es posible que aparezcan solo una pequeña parte de ellos. Para hacer frente a esta sobrecarga de rendimiento, ObjectDataSource ofrece una alternativa paginación personalizada de opción de paginación.

Mientras la paginación personalizada mejora en los problemas de rendimiento de s de paginación mediante la recuperación de solo aquellos registros que deben mostrarse, de manera predeterminada, s más compleja implementar la paginación personalizada. En primer lugar, debe escribir una consulta que correcta (y eficaz) tenga acceso el subconjunto específico de registros solicitados. Esto puede realizarse de varias maneras; uno se examinan en este tutorial consiste en usar SQL Server 2005 s nuevo `ROW_NUMBER()` resultados de función para clasificar y, a continuación, para solamente los devolver resultados cuya clasificación se encuentra dentro de un intervalo especificado. Además, tenemos que agregar a fin de determinar el número total de registros que se va a paginar a través de. Después de crear estos métodos de la capa DAL y BLL, también es necesario configurar el ObjectDataSource para que pueda determinar el número total de registros son que se va a paginar a través y correctamente pueden pasar los valores de índice de fila inicial y el número máximo de filas a la capa BLL.

Mientras que implementar la paginación personalizada requiere una serie de pasos y es no casi tan sencillo como paginación de manera predeterminada, la paginación personalizada es una necesidad al paginar a través de suficientemente grandes cantidades de datos. A medida que examinan los resultados de la paginación personalizada, mostraron puede perder a segundos en el tiempo de procesamiento de la página ASP.NET y puede aclarar la carga en el servidor de base de datos por una o más órdenes de magnitud.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](paging-and-sorting-report-data-cs.md)
[Siguiente](sorting-custom-paged-data-cs.md)
