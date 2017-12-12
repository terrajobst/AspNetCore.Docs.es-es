---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: "Implementación de simultaneidad optimista (C#) | Documentos de Microsoft"
author: rick-anderson
description: "Para una aplicación web que permite que varios usuarios modifiquen los datos, existe el riesgo que dos usuarios pueden modificar los mismos datos al mismo tiempo. En esta tutori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 50d02e8da7b7ab489e662b42d8f08ad3a99e66eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="implementing-optimistic-concurrency-c"></a>Implementación de simultaneidad optimista (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) o [descarga de PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Para una aplicación web que permite que varios usuarios modifiquen los datos, existe el riesgo que dos usuarios pueden modificar los mismos datos al mismo tiempo. En este tutorial implementaremos el control de simultaneidad optimista para controlar este riesgo.


## <a name="introduction"></a>Introducción

Para aplicaciones web que solo permiten a los usuarios ver los datos, o para los que se incluyen un único usuario que puede modificar los datos, no hay ninguna amenaza de dos usuarios simultáneos que se sobrescriban accidentalmente los cambios de los demás. Para las aplicaciones web que permiten que varios usuarios actualizan o eliminan datos, sin embargo, hay una posible sobre las modificaciones de un usuario entren en conflicto con otro usuario simultáneo. Sin ninguna directiva de simultaneidad en su lugar, cuando dos usuarios simultáneamente están editando un registro único, el usuario que confirma los cambios de sus última invalidará los cambios realizados por la primera.

Por ejemplo, imagine que dos usuarios, Jisun y Sam, visitan una página en la aplicación que permite a los visitantes actualizar y eliminar los productos a través de un control GridView en ambos. Haga clic en el botón Editar en el control GridView aproximadamente al mismo tiempo. Jisun cambia el nombre del producto a "Té Chai" y hace clic en el botón de actualización. El resultado neto es un `UPDATE` instrucción que se envía a la base de datos, que establece *todos los* de campos actualizables del producto (aunque Jisun solo actualiza un campo, `ProductName`). En este momento, la base de datos tiene los valores "Té Chai", la categoría Bebidas, el proveedor Exotic Liquids, y así sucesivamente para este producto en particular. Sin embargo, el control GridView en pantalla de Sam sigue mostrando el nombre del producto en la fila de GridView editable como "Chai". Unos pocos segundos después de que se han realizado los cambios del Jisun, Sam actualizaciones de la categoría condimentos y hace clic en actualizar. Esto da como resultado un `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto a "Chai," la `CategoryID` para el Id. de categoría de bebidas correspondiente y así sucesivamente. Cambios del Jisun al nombre del producto se han sobrescrito. La figura 1 representa gráficamente la siguiente serie de eventos.


[![Cuando dos usuarios simultáneamente actualización un registro existe potencial s para un usuario s cambia para sobrescribir las otras operaciones de asignación](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Figura 1**: cuando dos usuarios al mismo tiempo la actualización un registro existe s potencial para un usuario s cambia a sobrescribir las otras operaciones de asignación ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image3.png))


De forma similar, cuando dos usuarios visitan una página, puede ser un usuario en el medio de actualizar un registro cuando se elimina por otro usuario. O bien, entre un usuario cuando carga una página y cuando hagan clic en el botón Eliminar, otro usuario puede modificar el contenido de ese registro.

Hay tres [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) estrategias disponibles:

- **No hacer nada** -si usuarios con acceso simultáneos son modificar el mismo registro, permiten la última confirmación win (comportamiento predeterminado)
- [**Simultaneidad optimista** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -suponga que aunque puede haber conflictos de simultaneidad cada ahora y, a continuación, la mayoría de las veces no se produzcan tales conflictos; por lo tanto, si surge un conflicto, simplemente informa al usuario de que sus cambios no se puede guardar porque otro usuario ha modificado los mismos datos
- **Simultaneidad pesimista** -se supone que los conflictos de simultaneidad son comunes y que los usuarios no permitirse estar informaba de sus cambios no se han guardado debido a actividad simultánea de otro usuario; por lo tanto, cuando un usuario comienza a actualizar un registro, bloquearlo , lo que evita que otros usuarios editen o eliminen dicho registro hasta que el usuario confirma sus modificaciones

Todos nuestros tutoriales hasta ahora, han utilizado la estrategia de resolución de simultaneidad predeterminado: es decir, hemos dejamos la última operación de escritura ganar. En este tutorial, examinaremos cómo implementar el control de simultaneidad optimista.

> [!NOTE]
> No se tratará en los ejemplos de simultaneidad pesimista en esta serie de tutoriales. Simultaneidad pesimista se usa con poca frecuencia debido a que bloquea por ejemplo, si no correctamente renuncia, puede impedir que otros usuarios actualicen datos. Por ejemplo, si un usuario bloquea un registro para su edición y, a continuación, se deja para el día anterior desbloquearla, ningún otro usuario podrá actualizar dicho registro hasta que el usuario original se devuelve y completa su actualización. Por lo tanto, en situaciones donde se utiliza simultaneidad pesimista, normalmente hay un tiempo de espera que, si se alcanza, cancela el bloqueo. Sitios Web ventas de vale, que bloquea una ubicación determinada y durante un período corto mientras el usuario completa el proceso de pedido, es un ejemplo de control de simultaneidad pesimista.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Paso 1: Examinar la simultaneidad optimista se implementa

Control de simultaneidad optimista funciona asegurándose de que el registro que se va a actualizar o eliminar tiene los mismos valores que tenía cuando se inició la actualización o eliminación de proceso. Por ejemplo, al hacer clic en el botón de edición en un control GridView editable, los valores del registro se leer desde la base de datos y se muestran en cuadros de texto y otros controles Web. Estos valores originales se guardan por GridView. Más adelante, cuando el usuario hace que sus cambios y haga clic en el botón de actualización, los valores originales y los nuevos valores se envían a la capa de lógica de negocios y, a continuación, hacia abajo hasta la capa de acceso a datos. La capa de acceso a datos debe emitir una instrucción SQL que sólo se actualizará el registro si los valores originales que el usuario inició la edición son idénticos a los valores en la base de datos. Figura 2 muestra esta secuencia de eventos.


[![Para que la actualización o eliminación sea correcta, los valores originales deben ser iguales a los valores actuales de la base de datos](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Figura 2**: para la actualización o eliminación como correcto, el Original valores debe ser igual a los valores actuales de la base de datos ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image6.png))


Hay varios enfoques para implementar la simultaneidad optimista (vea [Peter A. Bromberg](http://peterbromberg.net/)del [Optmistic simultaneidad actualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para una breve descripción de una serie de opciones). El conjunto de datos con tipo de ADO.NET proporciona una implementación que se pueden configurar con la marca de verificación de un control checkbox. Habilita la simultaneidad optimista de un TableAdapter en el conjunto de datos con tipo aumenta el TableAdapter `UPDATE` y `DELETE` instrucciones para incluir una comparación de todos los valores originales de la `WHERE` cláusula. El siguiente `UPDATE` instrucción, por ejemplo, actualiza el nombre y el precio de un producto sólo si los valores actuales de la base de datos son iguales a los valores que se recuperaron originalmente cuando se actualiza el registro en GridView. El `@ProductName` y `@UnitPrice` parámetros contienen los nuevos valores especificados por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en GridView cuando se hace clic en el botón de edición:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Esto `UPDATE` instrucción se ha simplificado para mejorar la legibilidad. En la práctica, la `UnitPrice` comprobar la `WHERE` cláusula sería más compleja desde `UnitPrice` puede contener `NULL` s y comprobando si `NULL = NULL` siempre devuelve False (en su lugar, debe usar `IS NULL`).


Además de usar un subyacente diferentes `UPDATE` instrucción, configurar un TableAdapter utilicen optimista de simultaneidad también modifica la firma de su base de datos directa métodos. Recuerde de nuestro primer tutorial, [ *crear una capa de acceso a datos*](../introduction/creating-a-data-access-layer-cs.md), que los métodos directos de base de datos son aquéllas que acepta una lista de escalar los valores como parámetros de entrada (en lugar de una fila de datos fuertemente tipados o Instancia de la tabla de datos). Cuando se utiliza simultaneidad optimista, la base de datos directa `Update()` y `Delete()` métodos incluyen parámetros de entrada para los valores originales así. Además, el código de la capa BLL para usar el lote actualizar patrón (el `Update()` sobrecargas del método que aceptan filas de datos y tablas de datos en lugar de valores escalares) también se debe cambiar.

En su lugar de ampliar nuestro existente TableAdapters del DAL usar simultaneidad optimista (que podría necesitar cambiar el BLL para dar cabida a), vamos a en su lugar, cree un nuevo DataSet con tipo denominado `NorthwindOptimisticConcurrency`, al que se agregará un `Products` TableAdapter que utiliza simultaneidad optimista. A continuación, vamos a crear un `ProductsOptimisticConcurrencyBLL` clase de capa de lógica empresarial que tiene las modificaciones necesarias para admitir la capa DAL de simultaneidad optimista. Una vez que se ha diseñado este marco de trabajo, se estará preparados crear la página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Paso 2: Crear una capa de acceso a datos que admite la simultaneidad optimista

Para crear un nuevo conjunto de datos con tipo, haga doble clic en el `DAL` carpeta dentro de la `App_Code` carpeta y agregue un nuevo conjunto de datos denominado `NorthwindOptimisticConcurrency`. Como vimos en el primer tutorial, al hacerlo así agregará un nuevo TableAdapter al DataSet con tipo, iniciar automáticamente el Asistente para configuración de TableAdapter. En la primera pantalla, nos estamos pedirá que especifique la base de datos para conectarse a, conectarse a la misma base de datos Northwind utilizando el `NORTHWNDConnectionString` de `Web.config`.


[![Conectarse a la misma base de datos de Northwind](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Figura 3**: conectarse a la misma base de datos de Northwind ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image9.png))


A continuación, se nos pide en cuanto a cómo consultar los datos: a través de una instrucción SQL ad hoc, un nuevo procedimiento almacenado o una existente de procedimiento almacenado. Puesto que se usan realizar consultas SQL ad hoc en nuestro DAL original, utilice esta opción aquí también.


[![Especificar los datos que se puede recuperar usando una instrucción de SQL Ad Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Figura 4**: especificar los datos que se recuperarán con una instrucción de SQL Ad Hoc ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image12.png))


En la pantalla siguiente, escriba la consulta SQL se utiliza para recuperar la información del producto. Vamos a usar la consulta SQL mismo exacta utilizada para la `Products` TableAdapter desde nuestro DAL original, que devuelve todos los `Product` columnas junto con los nombres de proveedor y categoría de producto:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Use la misma consulta SQL desde el TableAdapter de productos en la capa DAL Original](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Figura 5**: usar la misma consulta SQL desde el `Products` TableAdapter en la capa DAL Original ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image15.png))


Antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. Para que este control de simultaneidad optimista emplean TableAdapter, solo tiene que activar la casilla "Usar simultaneidad optimista".


[![Habilitar el Control de simultaneidad optimista por corriente el &quot;usar simultaneidad optimista&quot; casilla de verificación](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Figura 6**: habilitar el Control de simultaneidad optimista activando la casilla "Usar simultaneidad optimista" ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image18.png))


Por último, indicar que el objeto TableAdapter debe usar los patrones de acceso de datos que rellenar un DataTable y devuelven un DataTable; También indica que se deben crear los métodos directos de base de datos. Cambie el nombre del método para la devolución un patrón de DataTable de GetData a GetProducts, con el fin de reflejar las convenciones de nomenclatura usamos en nuestro DAL original.


[![Tiene lo TableAdapter utilizan todos los patrones de acceso a datos](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Figura 7**: tienen los TableAdapter utilizan todos los datos de patrones de acceso ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image21.png))


Después de completar el asistente, el Diseñador de DataSet incluirá fuertemente tipadas `Products` DataTable y un TableAdapter. Tómese un momento para cambiar el nombre de la tabla de datos de `Products` a `ProductsOptimisticConcurrency`, lo que puede hacer con el botón secundario en la barra de título de la tabla de datos y elegir cambiar el nombre en el menú contextual.


[![Una tabla de datos y un TableAdapter se agregaron al conjunto de datos con tipo](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Figura 8**: un DataTable y TableAdapter se han agregado al conjunto de datos con tipo ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image24.png))


Para ver las diferencias entre la `UPDATE` y `DELETE` consultas entre el `ProductsOptimisticConcurrency` TableAdapter (que utiliza simultaneidad optimista) y un TableAdapter de productos (que no), haga clic en el objeto TableAdapter y vaya a la ventana Propiedades. En el `DeleteCommand` y `UpdateCommand` propiedades `CommandText` subpropiedades puede ver la sintaxis SQL real que se envía a la base de datos cuando se invocan la DAL update o métodos relacionados con la eliminación. Para el `ProductsOptimisticConcurrency` TableAdapter el `DELETE` es la instrucción que se utiliza:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Mientras que la `DELETE` instrucción para el producto del TableAdapter en nuestro DAL original es mucho más fácil:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Como puede ver, el `WHERE` cláusula en la `DELETE` declaración de TableAdapter que utiliza simultaneidad optimista incluye una comparación entre cada uno de los `Product` tabla valores de columna y los valores originales del existentes en el momento el Se rellenó última GridView (o DetailsView o FormView). Desde todos los campos distintos de `ProductID`, `ProductName`, y `Discontinued` puede tener `NULL` valores, los parámetros adicionales y comprobaciones se incluyen para comparar correctamente `NULL` valores en el `WHERE` cláusula.

Se no puede agregar cualquier DataTables adicionales para el conjunto de datos habilitadas para simultaneidad optimista para este tutorial, como nuestra página de ASP.NET solo se proporcionarán actualizar y eliminar la información de producto. Sin embargo, todavía necesitamos agregar el `GetProductByProductID(productID)` método para el `ProductsOptimisticConcurrency` TableAdapter.

Para ello, haga doble clic en la barra de título del TableAdapter (el derecho de área por encima del `Fill` y `GetProducts` nombres de método) y elija Agregar consulta en el menú contextual. Se iniciará al Asistente para configuración de consultas de TableAdapter. Como con la configuración inicial de nuestra TableAdapter, optar por crear la `GetProductByProductID(productID)` método mediante una instrucción de SQL ad hoc (consulte la figura 4). Puesto que la `GetProductByProductID(productID)` método devuelve información sobre un producto determinado, indicar que esta consulta es un `SELECT` tipo que devuelve filas de consulta.


[![Marque el tipo de consulta como un &quot;SELECT que devuelve filas&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Figura 9**: marcar el tipo de consulta como un "`SELECT` que devuelve filas" ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image27.png))


En la siguiente pantalla estamos solicitará la consulta SQL utilizada con la consulta del TableAdapter predeterminado previamente cargada. Aumentar la consulta existente para incluir la cláusula `WHERE ProductID = @ProductID`, tal y como se muestra en la figura 10.


[![Agregar un WHERE cláusula a la consulta cargada previamente para devolver un registro de producto específico](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Figura 10**: agregar un `WHERE` cláusula a la consulta Pre-Loaded para devolver un registro de producto específico ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image30.png))


Por último, cambie los nombres de método generados para `FillByProductID` y `GetProductByProductID`.


[![Cambiar el nombre de los métodos FillByProductID y GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Figura 11**: cambiar el nombre de los métodos para `FillByProductID` y `GetProductByProductID` ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image33.png))


Con este asistente completando, lo TableAdapter ahora contiene dos métodos para recuperar datos: `GetProducts()`, que devuelve *todos los* productos; y `GetProductByProductID(productID)`, que devuelve el producto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Paso 3: Crear una capa de lógica de negocios para la capa DAL optimista con la simultaneidad habilitada

Nuestro existente `ProductsBLL` clase tiene ejemplos del uso de los patrones directa de base de datos y de actualización por lotes. El `AddProduct` método y `UpdateProduct` sobrecargas ambos usan el patrón de actualización por lotes, pasando una `ProductRow` instancia al método Update del TableAdapter. El `DeleteProduct` método, por otro lado, usa el patrón directa de base de datos, una llamada a del TableAdapter `Delete(productID)` método.

Con el nuevo `ProductsOptimisticConcurrency` TableAdapter, la base de datos directa métodos ahora requieren que también se pueden pasar los valores originales en. Por ejemplo, el `Delete` método espera ahora diez parámetros de entrada: original `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, y `Discontinued`. Utiliza los valores de estos parámetros de entrada adicionales en `WHERE` cláusula de la `DELETE` instrucción enviada a la base de datos, solo eliminando el registro especificado si se asignan los valores actuales de la base de datos hasta los originales.

Mientras que la firma del método para el TableAdapter `Update` no ha cambiado el método que se usa en el patrón de actualización por lotes, tiene el código necesario para registrar los valores originales y los nuevos. Por lo tanto, en lugar de intentar usar la DAL habilitadas para simultaneidad optimista con nuestras `ProductsBLL` de clases, vamos a crear una nueva clase de capa de lógica empresarial para trabajar con nuestro nuevo DAL.

Agregue una clase denominada `ProductsOptimisticConcurrencyBLL` a la `BLL` carpeta dentro de la `App_Code` carpeta.


![Agregar la clase ProductsOptimisticConcurrencyBLL a la carpeta BLL](implementing-optimistic-concurrency-cs/_static/image34.png)

**Figura 12**: agregar la `ProductsOptimisticConcurrencyBLL` clase a la carpeta BLL


A continuación, agregue el código siguiente a la `ProductsOptimisticConcurrencyBLL` clase:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Tenga en cuenta el uso de `NorthwindOptimisticConcurrencyTableAdapters` instrucción anteriormente el inicio de la declaración de clase. El `NorthwindOptimisticConcurrencyTableAdapters` espacio de nombres contiene la `ProductsOptimisticConcurrencyTableAdapter` (clase), que proporciona los métodos de la capa DAL. También antes de la declaración de clase, encontrará el `System.ComponentModel.DataObject` atributo, que indica a Visual Studio que incluya esta clase en la lista desplegable de ObjectDataSource del asistente.

El `ProductsOptimisticConcurrencyBLL`del `Adapter` propiedad proporciona acceso rápido a una instancia de la `ProductsOptimisticConcurrencyTableAdapter` clase y sigue el patrón que se usa en nuestro clases BLL originales (`ProductsBLL`, `CategoriesBLL`, y así sucesivamente). Por último, el `GetProducts()` método simplemente llama hacia abajo en la capa de DAL `GetProducts()` método y devuelve un `ProductsOptimisticConcurrencyDataTable` objeto rellena con un `ProductsOptimisticConcurrencyRow` instancia para cada registro de producto de la base de datos.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Eliminación de un producto con el patrón directa de la base de datos de simultaneidad optimista

Al utilizar el patrón directa de base de datos en un capa DAL que utiliza simultaneidad optimista, los métodos deben pasar los valores nuevos y originales. Para eliminar, no existen nuevos valores, por lo que se necesitan pasar solo los valores originales. En nuestro BLL, a continuación, debemos aceptamos todos los parámetros originales como parámetros de entrada. Vamos a tener la `DeleteProduct` método en la `ProductsOptimisticConcurrencyBLL` clase utilizar el método directo de la base de datos. Esto significa que este método debe tomar en todos los campos de datos de producto diez como parámetros de entrada y pasar a la capa DAL, tal como se muestra en el código siguiente:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Si los valores originales - los valores que se cargó por última vez en el control GridView (o DetailsView o FormView) - difieren de los valores de la base de datos cuando el usuario hace clic en el botón Eliminar la `WHERE` cláusula no coincide con ningún registro de base de datos y no hay ningún registro se verán afectados. Por lo tanto, lo TableAdapter `Delete` método devolverá `0` y el BLL `DeleteProduct` método devolverá `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Actualizar un producto con el patrón de actualización por lotes con la simultaneidad optimista

Como se indicó anteriormente, lo TableAdapter `Update` método para el patrón de actualización por lotes tiene la misma firma de método, independientemente de si se emplea simultaneidad optimista. Es decir, el `Update` método espera una fila de datos, una matriz de filas de datos, una tabla de datos o un conjunto de datos con tipo. No hay ningún parámetro de entrada adicional para especificar los valores originales. Esto es posible porque la tabla de datos realiza un seguimiento de los valores originales y modificados para su DataRow(s). Cuando se emite la capa DAL su `UPDATE` (instrucción), el `@original_ColumnName` parámetros se rellenan con los valores originales de DataRow, mientras que la `@ColumnName` parámetros se rellenan con los valores modificados de DataRow.

En el `ProductsBLL` (clase) (que utiliza la simultaneidad original, no optimista DAL), cuando se usa el patrón de actualización por lotes para actualizar la información de producto el código realiza la siguiente secuencia de eventos:

1. Leer la información de producto de base de datos actual en un `ProductRow` instancia mediante el TableAdapter `GetProductByProductID(productID)` (método)
2. Asignar los nuevos valores para el `ProductRow` instancia del paso 1
3. Llamar a del TableAdapter `Update` método, pasando el `ProductRow` instancia

Esta secuencia de pasos, sin embargo, no admite correctamente la simultaneidad optimista porque la `ProductRow` rellena en el paso 1 se rellena directamente desde la base de datos, lo que significa que los valores originales utilizados por el DataRow son aquellos que existen actualmente en el base de datos y no los que se enlazaron a GridView al principio del proceso de edición. En su lugar, al usar un optimista habilitadas para simultaneidad DAL, es necesario modificar el `UpdateProduct` sobrecargas de método debe usar los siguientes pasos:

1. Leer la información de producto de base de datos actual en un `ProductsOptimisticConcurrencyRow` instancia mediante el TableAdapter `GetProductByProductID(productID)` (método)
2. Asigne el *original* valores a la `ProductsOptimisticConcurrencyRow` instancia del paso 1
3. Llame a la `ProductsOptimisticConcurrencyRow` la instancia `AcceptChanges()` método, que indica el DataRow que sus valores actuales son los "originales"
4. Asigne el *nueva* valores a la `ProductsOptimisticConcurrencyRow` instancia
5. Llamar a del TableAdapter `Update` método, pasando el `ProductsOptimisticConcurrencyRow` instancia

Paso 1 lecturas en todos los valores actuales de la base de datos para el registro de producto especificado. Este paso es superfluo en el `UpdateProduct` sobrecarga que actualiza *todos los* de las columnas de producto (como estos valores se sobrescriben en el paso 2), pero es esencial para dichas sobrecargas que solo un subconjunto de los valores de columna se pasan como parámetros de entrada. Una vez que los valores originales se han asignado a la `ProductsOptimisticConcurrencyRow` instancia, el `AcceptChanges()` momento en que se llama al método, que marca los valores actuales de DataRow como los valores originales que se usará en el `@original_ColumnName` parámetros en la `UPDATE` instrucción. A continuación, se asignan los nuevos valores de parámetro a la `ProductsOptimisticConcurrencyRow` y, por último, el `Update` se invoca el método, pasando el DataRow.

El código siguiente muestra el `UpdateProduct` sobrecarga que acepta datos de producto de todos los campos como parámetros de entrada. Mientras no se muestra aquí, el `ProductsOptimisticConcurrencyBLL` clase incluida en la descarga de este tutorial también contiene un `UpdateProduct` sobrecarga que acepta únicamente el producto nombre y precio como parámetros de entrada.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Paso 4: Pasando los valores originales y los nuevos desde la página de ASP.NET a los métodos BLL

Con la capa DAL y BLL completa, todo lo que queda es crear una página ASP.NET que puede usar la lógica de simultaneidad optimista integrada en el sistema. En concreto, los datos de control de Web (GridView, DetailsView o FormView) deben recordar que sus valores originales y ObjectDataSource deben pasar ambos conjuntos de valores para la capa de lógica de negocios. Además, la página ASP.NET debe configurarse para controlar correctamente las infracciones de simultaneidad.

Comience abriendo la `OptimisticConcurrency.aspx` página en el `EditInsertDelete` carpeta y agregar un control GridView al diseñador, establecer su `ID` propiedad `ProductsGrid`. De las etiquetas inteligentes del control GridView, optar por crear un nuevo origen ObjectDataSource denominado `ProductsOptimisticConcurrencyDataSource`. Puesto que deseamos que este ObjectDataSource para usar la capa DAL que admite la simultaneidad optimista, configurarlo de modo que use la `ProductsOptimisticConcurrencyBLL` objeto.


[![Hacer que el uso de ObjectDataSource el objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Figura 13**: el uso de ObjectDataSource el `ProductsOptimisticConcurrencyBLL` objeto ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image37.png))


Elija la `GetProducts`, `UpdateProduct`, y `DeleteProduct` métodos de listas desplegables en el asistente. El método UpdateProduct, utilice la sobrecarga que acepta todos los campos de datos del producto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configuración de las propiedades del Control ObjectDataSource

Después de completar al asistente, marcado declarativo del ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Como puede ver, el `DeleteParameters` colección contiene un `Parameter` instancia para cada uno de los diez parámetros de entrada en el `ProductsOptimisticConcurrencyBLL` la clase `DeleteProduct` método. Del mismo modo, el `UpdateParameters` colección contiene un `Parameter` instancia para cada uno de los parámetros de entrada en `UpdateProduct`.

Para los tutoriales anteriores que participan la modificación de datos, se recomienda eliminar el ObjectDataSource `OldValuesParameterFormatString` propiedad en este punto, ya que esta propiedad indica que el método BLL espera los valores antiguos (u originales) que se pasará en, así como los nuevos valores. Además, el valor de esta propiedad indica los nombres de parámetro de entrada para los valores originales. Puesto que estamos pasando los valores originales en la capa BLL, hacer *no* quitar esta propiedad.

> [!NOTE]
> El valor de la `OldValuesParameterFormatString` propiedad debe asignar a los nombres de parámetro de entrada de la capa BLL que espera que los valores originales. Puesto que se denomina estos parámetros `original_productName`, `original_supplierID`, y así sucesivamente, puede dejar el `OldValuesParameterFormatString` como valor de propiedad `original_{0}`. Si, sin embargo, los parámetros de entrada de los métodos BLL tenían nombres como `old_productName`, `old_supplierID`, y así sucesivamente, deberá actualizar el `OldValuesParameterFormatString` propiedad `old_{0}`.


Hay una configuración de propiedad final que debe realizarse en orden para ObjectDataSource pasar correctamente los valores originales a los métodos BLL. ObjectDataSource tiene un [propiedad ConflictDetection](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que puede asignarse a [uno de dos valores](https://msdn.microsoft.com/en-US/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`-el valor predeterminado; no envía los valores originales para parámetros de entrada original de los métodos BLL
- `CompareAllValues`-enviar los valores originales a los métodos BLL; Elija esta opción cuando se utiliza simultaneidad optimista

Tómese un momento para establecer el `ConflictDetection` propiedad `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurar los campos y propiedades de GridView

Con las propiedades de ObjectDataSource configurados correctamente, centrémonos para configurar el control GridView. En primer lugar, puesto que deseamos GridView para admitir la edición y eliminación, haga clic en las casillas de verificación Habilitar edición y habilitar la eliminación de etiquetas inteligentes de GridView. Esto agregará una CommandField cuyo `ShowEditButton` y `ShowDeleteButton` están establecidos en `true`.

Cuando se enlaza a la `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo para cada uno de los campos de datos del producto. Aunque se puede editar un GridView de este tipo, la experiencia del usuario es aceptable distinto. El `CategoryID` y `SupplierID` BoundFields se representarán como cuadros de texto, exigir que el usuario especificar la categoría correspondiente y el proveedor como números de identificación. No habrá ningún formato de los campos numéricos y no hay controles de validación para asegurarse de que se ha proporcionado el nombre del producto y que el precio por unidad unidades en existencias y unidades en orden y los valores de nivel de nuevo pedido son ambos valores numéricos adecuados y son mayores que o igual a en cero.

Como se explicó en la *agregar controles de validación a las Interfaces de insertar y modificar* y *personalizar la interfaz de modificación de datos* tutoriales, la interfaz de usuario se puede personalizar reemplazar el BoundFields con TemplateFields. Modifiqué este GridView y su interfaz de edición de las maneras siguientes:

- Quita el `ProductID`, `SupplierName`, y `CategoryName` BoundFields
- Convertir el `ProductName` BoundField a TemplateField y agregar un control RequiredFieldValidation.
- Convertir el `CategoryID` y `SupplierID` BoundFields a TemplateFields y ajustar la interfaz de edición para usar listas desplegables en lugar de cuadros de texto. En estos TemplateFields `ItemTemplates`, `CategoryName` y `SupplierName` se muestran los campos de datos.
- Convertir el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` BoundFields TemplateFields y controles CompareValidator agregados.

Puesto que ya hemos examinado cómo realizar estas tareas en los tutoriales anteriores, sólo podrá enumerar la sintaxis declarativa final y dejar la implementación como práctica.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Estamos muy cerca de la necesidad de obtener un ejemplo totalmente funcional. Sin embargo, hay algunos matices que arrastre del seguridad y nos causar problemas. Además, se necesita alguna interfaz que avisa al usuario cuando se ha producido una infracción de simultaneidad.

> [!NOTE]
> En el orden de un control Web de datos para pasar correctamente los valores originales para el ObjectDataSource (que, a continuación, se pasan a la capa BLL), es fundamental que la GridView `EnableViewState` propiedad está establecida en `true` (valor predeterminado). Si deshabilita el estado de vista, se pierden los valores originales en el postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Pasar los valores originales correctos a ObjectDataSource

Hay un par de problemas con el modo en que se ha configurado la GridView. Si el ObjectDataSource `ConflictDetection` propiedad está establecida en `CompareAllValues` (tal como está nuestra), cuando el ObjectDataSource `Update()` o `Delete()` métodos se invocan la GridView (o DetailsView o FormView), ObjectDataSource intenta copiar el Valores originales de GridView en su correspondiente `Parameter` instancias. Hacen referencia a la figura 2 para ver una representación gráfica de este proceso.

En concreto, valores originales de GridView se asignan los valores de las instrucciones de enlace de datos bidireccional cada vez que los datos se enlazan a la GridView. Por lo tanto, es esencial que se capturan los valores originales necesarios todos a través del enlace de datos bidireccional y que se proporcionan en un formato que se puede convertir.

Para ver por qué esto es importante, tómese un momento para visite nuestra página en un explorador. Como se esperaba, GridView enumera todos los productos con un botón Editar y eliminar en la columna izquierda.


[![Los productos se muestran en un control GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Figura 14**: se enumeran los productos en un GridView ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image40.png))


Si hace clic en el botón Eliminar para cualquier producto, una `FormatException` se produce.


[![Intentando eliminar los resultados de cualquier producto en un FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Figura 15**: intento de eliminar cualquier producto resultados en un `FormatException` ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image43.png))


El `FormatException` se genera cuando el ObjectDataSource intenta leer en el original `UnitPrice` valor. Puesto que la `ItemTemplate` tiene la `UnitPrice` da formato como una moneda (`<%# Bind("UnitPrice", "{0:C}") %>`), incluye un símbolo de moneda, como $19.95. El `FormatException` se produce como ObjectDataSource intenta convertir la cadena en un `decimal`. Para prevenir este problema, tenemos una serie de opciones:

- Quitar la moneda del formato de la `ItemTemplate`. Es decir, en lugar de usar `<%# Bind("UnitPrice", "{0:C}") %>`, basta con usar `<%# Bind("UnitPrice") %>`. El inconveniente de esto es que el precio ya no tiene el formato.
- Mostrar la `UnitPrice` con formato de moneda en el `ItemTemplate`, pero usar el `Eval` palabra clave para lograr esto. Recuerde que `Eval` realiza el enlace de datos unidireccional. Necesitamos proporcionar el `UnitPrice` valor para los valores originales, por lo que todavía necesitaremos una instrucción de enlace de datos bidireccional en el `ItemTemplate`, pero esto puede colocarse en un control Web Label cuya `Visible` propiedad está establecida en `false`. En la plantilla ItemTemplate podríamos usar el siguiente marcado:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Quitar la moneda del formato de la `ItemTemplate`con `<%# Bind("UnitPrice") %>`. En la GridView `RowDataBound` controlador de eventos, mediante programación el acceso Web de la etiqueta de control dentro del cual el `UnitPrice` valor se muestran y establecer su `Text` propiedad a la versión con formato.
- Deje el `UnitPrice` da formato como una moneda. En la GridView `RowDeleting` controlador de eventos, sustituya el original existente `UnitPrice` valor (19,95 dólares estadounidenses) con un valor decimal real utilizando `Decimal.Parse`. Hemos visto cómo llevar a cabo algo similar en el `RowUpdating` controlador de eventos en el [ *BLL - control y las excepciones de nivel de la capa DAL de una página ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial.

Para el ejemplo que deseaba ir con el segundo enfoque, agregar una etiqueta de Web oculto control cuya `Text` propiedad está enlazado a la sin formato de datos bidireccionales `UnitPrice` valor.

Después de resolver este problema, intente hacer clic en el botón Eliminar para cualquier producto de nuevo. Esta vez obtendrá una `InvalidOperationException` cuando intente invocar el BLL ObjectDataSource `UpdateProduct` método.


[![ObjectDataSource no encuentra ningún método con los parámetros de entrada desea enviar](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Figura 16**: ObjectDataSource no encuentra ningún método con los parámetros de entrada desea enviar ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image46.png))


Examinando el mensaje de la excepción, resulta evidente que el ObjectDataSource desea invocar un BLL `DeleteProduct` método que incluye `original_CategoryName` y `original_SupplierName` parámetros de entrada. Esto es porque el `ItemTemplate` para ver si el `CategoryID` y `SupplierID` TemplateFields actualmente contienen instrucciones de enlace bidireccionales con el `CategoryName` y `SupplierName` campos de datos. En su lugar, debemos incluir `Bind` instrucciones con el `CategoryID` y `SupplierID` campos de datos. Para ello, reemplace las instrucciones de enlace existentes con `Eval` instrucciones y, a continuación, agregue controles Label oculto cuya `Text` propiedades se enlazan a la `CategoryID` y `SupplierID` campos de datos mediante el enlace de datos bidireccional, como se muestra a continuación:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Con estos cambios, se son capaces de eliminar y editar la información de producto correctamente. En el paso 5 se examinará cómo comprobar que se están detectando las infracciones de simultaneidad. Pero por ahora, tardar unos minutos en intente actualizar y eliminar unos registros para asegurarse de que la actualización y eliminación de un solo usuario funciona según lo previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Paso 5: Probar la compatibilidad de simultaneidad optimista

Para comprobar que se va infracciones de la simultaneidad detectado (en lugar de da lugar a que se sobrescriban seguirá a ciegas los datos), es necesario abrir dos ventanas de explorador a esta página. En ambas instancias del explorador, haga clic en el botón Editar para Chai. A continuación, en solo uno de los exploradores, cambie el nombre a "Té Chai" y haga clic en actualizar. La actualización debe tener éxito y devolver GridView a su estado de edición previamente, con "Té Chai" como el nuevo nombre de producto.

Sin embargo, en la otra instancia de ventana Explorador, el cuadro de texto Nombre del producto sigue mostrando "Chai". En esta segunda ventana del explorador, actualice el `UnitPrice` a `25.00`. Sin compatibilidad con la simultaneidad optimista, haga clic en actualizar en la segunda instancia del explorador cambiaría el nombre del producto a "Chai", sobrescribiendo los cambios realizados por la primera instancia del explorador. Con la simultaneidad optimista empleado, sin embargo, al hacer clic en el botón de actualización en la segunda instancia del explorador provoca un [DBConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.dbconcurrencyexception.aspx).


[![Cuando se detecta una infracción de simultaneidad, se produce una excepción DBConcurrencyException](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Figura 17**: se detecta cuando una infracción de simultaneidad, un `DBConcurrencyException` se produce ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image49.png))


El `DBConcurrencyException` solo se produce cuando se utiliza el patrón de actualización por lotes de la capa DAL. El patrón directa de la base de datos no provoca una excepción, simplemente indica que no hay filas afectadas. Para ilustrar esto, devolver GridView ambas instancias del explorador a su estado anterior de edición. A continuación, en la primera instancia del explorador, haga clic en el botón de edición y cambiar el nombre de producto de "Té Chai" a "Chai" y haga clic en actualizar. En la segunda ventana del explorador, haga clic en el botón Eliminar para Chai.

Al hacer clic en eliminar, la página devuelve datos, el control GridView, invoca el ObjectDataSource `Delete()` método y ObjectDataSource llama a la `ProductsOptimisticConcurrencyBLL` la clase `DeleteProduct` método, pasando a través de los valores originales. La versión original `ProductName` valor de la segunda instancia del explorador es "Chai té", que no coincide con la actual `ProductName` valor en la base de datos. Por lo tanto, la `DELETE` instrucción emitida a la base de datos afecta a cero filas porque no hay ningún registro en la base de datos que el `WHERE` cláusula satisface. El `DeleteProduct` método `false` y se vuelve a enlazar los datos de ObjectDataSource en GridView.

Desde la perspectiva del usuario final, al hacer clic en el botón Eliminar para té Chai en la segunda ventana del explorador ha provocado la pantalla para flash y, tras viniendo, el producto aún está allí, aunque ahora aparece como "Chai" (el producto nombre cambio realizado por el primer explorador instancia). Si el usuario hace clic en el botón Eliminar nuevo, la eliminación se realizará correctamente, como GridView original `ProductName` valor ("Chai") ahora coincide con el valor de la base de datos.

En ambos casos, la experiencia del usuario está muy alejado del ideal. Claramente no queremos mostrar al usuario los detalles esenciales de la `DBConcurrencyException` excepción al utilizar el patrón de actualización por lotes. Y el comportamiento cuando se usa el patrón directa de la base de datos es un poco confuso como error del comando de los usuarios, pero no había ninguna indicación precisa de por qué.

Para solucionar estos dos problemas, podemos crear controles Web de etiqueta en la página que proporcionan una explicación a por qué una actualización o no se pudo eliminar. El patrón de actualización por lotes, podemos determinar si un `DBConcurrencyException` se ha producido una excepción en el controlador de eventos posterior al nivel de GridView, mostrar la etiqueta de advertencia según sea necesario. Para el método directo de la base de datos, podemos examinar el valor devuelto del método BLL (que es `true` si una fila se ve afectada, `false` en caso contrario) y mostrar un mensaje informativo según sea necesario.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Paso 6: Agregar mensajes informativos y mostrarlos en el caso de una infracción de simultaneidad

Cuando se produce una infracción de simultaneidad, el comportamiento exhibe depende de si se usó la actualización por lotes o un patrón directa de base de datos de la capa DAL. Nuestro tutorial utiliza ambos patrones, con el patrón de actualización por lotes que se va a usar para la actualización y el patrón directa de base de datos utilizado para eliminar. Para empezar, vamos a agregar dos controles Web Label a la página que se explica que se ha producido una infracción de simultaneidad al intentar eliminar o actualizar los datos. Establecer el control de etiqueta `Visible` y `EnableViewState` propiedades `false`; Esto hará que se oculta en cada visita de página excepto aquellas determinada página visita where sus `Visible` propiedad se establece mediante programación en `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Además de establecer sus `Visible`, `EnabledViewState`, y `Text` propiedades, también establecí el `CssClass` propiedad `Warning`, lo que hace que la etiqueta de la que se mostrará en una fuente grande, roja, cursiva, negrita. Este CSS `Warning` clase se define y se agregó a Styles.css en el *examinar los eventos asociados con la inserción, actualización y eliminación* tutorial.

Después de agregar estas etiquetas, el Diseñador de Visual Studio debe ser similar a la figura 18.


[![Dos controles de etiqueta se agregaron a la página](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Figura 18**: dos etiqueta controles se agregaron a la página ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image52.png))


Con estos controles Web de etiqueta en su lugar, estamos listos examinar cómo determinar cuando se produjo una infracción de simultaneidad, el punto en la etiqueta adecuada `Visible` propiedad puede establecerse en `true`, mostrar el mensaje informativo.

## <a name="handling-concurrency-violations-when-updating"></a>Control de las infracciones de simultaneidad al actualizar

Veamos primero cómo controlar las infracciones de simultaneidad al utilizar el patrón de actualización por lotes. Desde este tipo de infracciones con el lote actualización causa patrón un `DBConcurrencyException` excepción, tenemos que agregar código a nuestra página ASP.NET para determinar si un `DBConcurrencyException` excepción se produjo durante el proceso de actualización. Si por lo tanto, se debe mostrar un mensaje para el usuario que explica que no se guardaron los cambios porque otro usuario había modificado los mismos datos entre cuando se iniciaron modificando el registro, y cuando hace clic en el botón de actualización.

Como vimos en el *BLL - control y las excepciones de nivel de la capa DAL de una página ASP.NET* tutorial dichas excepciones se pueden detectar y suprime en los controladores de eventos posterior al nivel del control de datos Web. Por lo tanto, es necesario crear un controlador de eventos del control de GridView `RowUpdated` eventos que comprueba si un `DBConcurrencyException` se ha producido la excepción. Este controlador de eventos se pasa una referencia a cualquier excepción que se produjo durante el proceso de actualización, tal como se muestra en el controlador de código debajo de eventos:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

En el face de un `DBConcurrencyException` muestra de este controlador de eventos de excepción, el `UpdateConflictMessage` control de etiqueta e indica que se ha controlado la excepción. Con este código en su lugar, cuando se produce una infracción de simultaneidad cuando se actualiza un registro, los cambios del usuario se pierden, ya que habría sobrescribe las modificaciones de otro usuario al mismo tiempo. En concreto, GridView se vuelve a su estado anterior de edición y enlaza a los datos de la base de datos actual. Esto actualizará la fila de GridView con cambios del otro usuario, que estaban previamente no visibles. Además, el `UpdateConflictMessage` control de etiqueta explicará al usuario ¿qué ha ocurrido. Esta secuencia de eventos se detalla en la figura 19.


[![Un usuario s se pierden las actualizaciones en la cara de una infracción de simultaneidad](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Figura 19**: s a un usuario en la cara de una infracción de simultaneidad se pierden las actualizaciones ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> O bien, en lugar de devolver el control GridView en el estado de edición previamente, pudimos deja GridView en su estado de edición estableciendo la `KeepInEditMode` propiedad del pasado `GridViewUpdatedEventArgs` objeto en true. Si adopta este enfoque, sin embargo, asegúrese de volver a enlazar los datos en GridView (invocando su `DataBind()` método) para que los otros valores del usuario se cargan en la interfaz de edición. El código disponible para su descarga con este tutorial tiene estas dos líneas de código en el `RowUpdated` controlador de eventos comentada; simplemente quite estas líneas de código para que el control GridView permanecen en modo de edición después de una infracción de simultaneidad.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Responde a las infracciones de simultaneidad al eliminar

Con el patrón directa de la base de datos, no hay ninguna excepción que se inicia en el caso de una infracción de simultaneidad. En su lugar, la instrucción de base de datos simplemente afecta a ningún registro, como la cláusula WHERE no coincide con ningún registro. Todos los métodos de modificación de datos creados en la capa BLL se han diseñado de forma que devuelven un valor booleano que indica si esto afecta exactamente un registro. Por lo tanto, para determinar si se ha producido una infracción de simultaneidad cuando se elimina un registro, podemos examinar el valor devuelto de la capa de BLL `DeleteProduct` método.

El valor devuelto para un método BLL se puede examinar posterior al nivel en de ObjectDataSource controladores de eventos a través de la `ReturnValue` propiedad de la `ObjectDataSourceStatusEventArgs` objeto pasado en el controlador de eventos. Puesto que estamos interesados en determinar el valor devuelto de la `DeleteProduct` método, es necesario crear un controlador de eventos para el ObjectDataSource `Deleted` eventos. El `ReturnValue` propiedad es de tipo `object` y puede ser `null` si se produce una excepción y el método se interrumpió antes de que podría devolver un valor. Por lo tanto, se deben primero asegúrese de que el `ReturnValue` propiedad no es `null` y es un valor booleano. Suponiendo que supera esta comprobación, se mostrará el `DeleteConflictMessage` etiqueta de control si el `ReturnValue` es `false`. Esto puede realizarse mediante el código siguiente:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

En el caso de una infracción de simultaneidad, se cancela la solicitud de eliminación del usuario. Se actualiza el control GridView, que muestra los cambios que se produjeron para ese registro entre el momento en que el usuario cargan la página y al hacer clic en el botón Eliminar. Cuando ocurre una infracción de este tipo, la `DeleteConflictMessage` etiqueta se muestra, que explica que pasó (Véase la figura 20).


[![Un usuario s Delete se cancela en el caso de una infracción de simultaneidad](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Figura 20**: s a un usuario se ha cancelado la eliminación en el caso de una infracción de simultaneidad ([haga clic aquí para ver la imagen a tamaño completo](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Resumen

Existen en todas las aplicaciones que permite que varias posibilidades las infracciones de simultaneidad de usuarios simultáneos para actualizar o eliminar datos. Si no se tienen en cuenta este tipo de infracciones de, cuando dos usuarios actualizan simultáneamente los mismos datos siempre obtiene la última escritura "gana", sobrescribiendo el otro usuario cambia cambia. Como alternativa, los desarrolladores pueden implementar cualquier control de simultaneidad optimista o pesimista. Control de simultaneidad optimista se da por supuesto que las infracciones de simultaneidad son poco frecuentes y simplemente no permite una actualización o eliminar comandos que sería una infracción de simultaneidad. Control de simultaneidad pesimista se da por supuesto que simultaneidad infracciones son frecuentes y simplemente rechazar un usuario se actualizan o eliminan comando no es aceptable. Con el control de simultaneidad pesimista, actualizar un registro implica bloquearlo, lo que evita que otros usuarios modifiquen o eliminen el registro aunque esté bloqueado.

El conjunto de datos con tipo en .NET proporciona funcionalidad para admitir el control de simultaneidad optimista. En concreto, el `UPDATE` y `DELETE` las instrucciones para la base de datos incluyen todas las columnas de la tabla, lo que garantiza que la actualización o eliminación sólo se producirá si los datos del registro actual coinciden con los datos originales, el usuario tenía cuando realizar su actualización o eliminación. Una vez que la capa DAL se ha configurado para admitir la simultaneidad optimista, los métodos BLL deben actualizarse. Además, la página ASP.NET que llama hacia abajo en la capa BLL debe configurarse de forma que ObjectDataSource recupera los valores originales de su control Web de datos y los pasa hacia abajo en la capa BLL.

Como hemos visto en este tutorial, implementar el control de simultaneidad optimista en una aplicación web ASP.NET implica la actualización de la capa DAL y BLL y agregar compatibilidad en la página ASP.NET. Si es o no este trabajo agregado de una buena inversión de su tiempo y esfuerzo depende de la aplicación. Si con poca frecuencia tendrá que actualizar los datos de usuarios simultáneos, o los datos que va a actualizar están diferentes entre sí, a continuación, el control de simultaneidad no es una cuestión clave. Si, sin embargo, habitualmente tiene varios usuarios en el sitio de trabajar con los mismos datos, control de simultaneidad puede ayudar a evitar eliminaciones o actualizaciones de un usuario se ha sobrescrito accidentalmente del otro.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](customizing-the-data-modification-interface-cs.md)
[Siguiente](adding-client-side-confirmation-when-deleting-cs.md)
