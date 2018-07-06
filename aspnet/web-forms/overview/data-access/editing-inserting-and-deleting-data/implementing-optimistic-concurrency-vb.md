---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementar la simultaneidad optimista (VB) | Microsoft Docs
author: rick-anderson
description: Para una aplicación web que permite que varios usuarios editar datos, existe el riesgo que dos usuarios pueden estar editando los mismos datos al mismo tiempo. En este tutori...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b5a274103851b4b60c92d5fe46125cc4a1b0be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832971"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implementar la simultaneidad optimista (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) o [descargar PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Para una aplicación web que permite que varios usuarios editar datos, existe el riesgo que dos usuarios pueden estar editando los mismos datos al mismo tiempo. En este tutorial implementaremos el control de simultaneidad optimista para controlar este riesgo.


## <a name="introduction"></a>Introducción

Para las aplicaciones web que permiten sólo a los usuarios ver los datos, o para aquellos que incluyen un único usuario que puede modificar los datos, no hay ninguna amenaza de dos usuarios simultáneos que se sobrescriba accidentalmente los cambios del otro. Sin embargo, para las aplicaciones web que permiten que varios usuarios actualicen o eliminen datos, hay la posibilidad de que las modificaciones de un usuario entrar en conflicto con otro usuario simultáneo. Sin ninguna directiva de simultaneidad en su lugar, cuando dos usuarios simultáneamente están editando un registro único, el usuario que confirma los cambios de sus última invalidará los cambios realizados por la primera.

Por ejemplo, imagine que dos usuarios, Jisun y Sam, visitan una página en nuestra aplicación que permite a los visitantes de actualización y eliminación de los productos a través de un control GridView en ambos. Haga clic en el botón Editar en el control GridView aproximadamente al mismo tiempo. Jisun cambia el nombre del producto a "Té Chai" y hace clic en el botón Actualizar. El resultado neto es un `UPDATE` instrucción que se envía a la base de datos, que establece *todas* de los campos actualizables del producto (aunque Jisun solo actualiza un campo, `ProductName`). En este momento, la base de datos tiene los valores "Chai té," la categoría Bebidas, el proveedor líquidos exóticos, y así sucesivamente para este producto en particular. Sin embargo, el control GridView en pantalla de Sam sigue mostrando el nombre del producto en la fila GridView editable como "Chai". Unos pocos segundos después de que se han realizado los cambios del Jisun, Sam actualizaciones de la categoría condimentos y hace clic en actualizar. Esto da como resultado un `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto a "Chai", la `CategoryID` para el Id. de categoría de bebidas correspondiente y así sucesivamente. Se han sobrescrito los cambios realizados de Jisun en el nombre del producto. Figura 1 muestra gráficamente la esta serie de eventos.


[![Cuando dos usuarios actualizan simultáneamente un potencial s hay registros de cambios de un usuario sobrescribir el otro](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figura 1**: cuando dos usuarios simultáneamente actualización un registro existe s potencial para un usuario cambia a sobrescribir la otra ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image3.png))


De forma similar, cuando dos usuarios que visitan una página, puede ser un usuario en medio de actualizar un registro cuando se elimina por otro usuario. O bien, entre un usuario cuando carga una página y cuando haga clic en el botón Eliminar, otro usuario puede modificar el contenido de ese registro.

Hay tres [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) estrategias disponibles:

- **No hacer nada** -si usuarios simultáneos son modificar el mismo registro, permiten la última confirmación ganar (comportamiento predeterminado)
- [**Simultaneidad optimista** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -suponen que aunque puede haber conflictos de simultaneidad cada ahora y, a continuación, la mayoría de las veces no se produzcan dichos conflictos; por lo tanto, si surge un conflicto, simplemente informar al usuario que sus cambios no se puede guardar porque otro usuario modificó los mismos datos
- **Simultaneidad pesimista** -suponen que los conflictos de simultaneidad están algo habitual y que los usuarios no toleran la pérdida de ser dijo que no se han guardado los cambios debido a actividad simultánea de otro usuario; por lo tanto, cuando un usuario comienza a actualizar un registro, bloquearlo , lo que impide a otros usuarios editen o eliminen ese registro hasta que el usuario confirma sus modificaciones.

Todos nuestros tutoriales hasta ahora han usado la estrategia de resolución de simultaneidad predeterminado, es decir, hemos permitimos la última escritura gana. En este tutorial, examinaremos cómo implementar el control de simultaneidad optimista.

> [!NOTE]
> No veremos ejemplos de simultaneidad pesimista en esta serie de tutoriales. Simultaneidad pesimista no suele usarse porque bloquea por ejemplo, si no correctamente abandonar, puede evitar que otros usuarios actualicen los datos. Por ejemplo, si un usuario bloquea un registro para editarlo y, a continuación, se deja para el día anterior desbloquearla, ningún otro usuario será capaz de actualizar el registro hasta que el usuario original se devuelve y completa su actualización. Por lo tanto, en situaciones donde se utiliza simultaneidad pesimista, normalmente hay un tiempo de espera que, si se alcanza, cancela el bloqueo. Sitios Web ventas de vale, que bloquear una ubicación de asientos determinado durante un período breve mientras el usuario completa el proceso de pedido, es un ejemplo de control de simultaneidad pesimista.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Paso 1: Examina cómo la simultaneidad optimista se implementa

Control de simultaneidad optimista funciona asegurándose de que el registro que se va a actualizar o eliminar tiene los mismos valores que tenía cuando se inició la actualización o eliminación de proceso. Por ejemplo, al hacer clic en el botón Editar de un control GridView editable, los valores del registro se leen de la base de datos y muestran en cuadros de texto y otros controles Web. Estos valores originales se guardan por el control GridView. Más adelante, cuando el usuario hace que sus cambios y hace clic en el botón de actualización, los valores originales y los nuevos valores se envían a la capa de lógica empresarial y, a continuación, hacia abajo hasta la capa de acceso a datos. La capa de acceso a datos debe emitir una instrucción SQL que solo se actualizará el registro si los valores originales que el usuario comenzó a editar son idénticos a los valores en la base de datos. Figura 2 se ilustra esta secuencia de eventos.


[![Para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores actuales de la base de datos](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figura 2**: para la actualización o eliminación que sea un éxito, el Original valores debe ser igual que los valores actuales de la base de datos ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image6.png))


Hay varios enfoques para implementar la simultaneidad optimista (consulte [Peter A. Bromberg](http://peterbromberg.net/)del [Optmistic simultaneidad actualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para un breve vistazo a una serie de opciones). El conjunto de datos con tipo de ADO.NET proporciona una implementación que se puede configurar con solo la graduación de un control checkbox. Al habilitar la simultaneidad optimista para un TableAdapter en el conjunto de datos con tipo aumenta el TableAdapter `UPDATE` y `DELETE` instrucciones para incluir una comparación de todos los valores originales de la `WHERE` cláusula. La siguiente `UPDATE` instrucción, por ejemplo, actualiza el nombre y el precio de un producto sólo si los valores actuales de la base de datos son iguales a los valores que se recuperaron originalmente al actualizar el registro en el control GridView. El `@ProductName` y `@UnitPrice` parámetros contienen los nuevos valores especificados por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en el control GridView cuando se hace clic en el botón Editar:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Esto `UPDATE` instrucción se ha simplificado para mejorar la legibilidad. En la práctica, la `UnitPrice` Compruebe el `WHERE` cláusula sería más compleja desde `UnitPrice` puede contener `NULL` s y comprobar si `NULL = NULL` siempre devuelve False (en su lugar, debe usar `IS NULL`).


Además de utilizar un subyacente diferente `UPDATE` métodos directos de instrucción, la configuración de un TableAdapter para usar optimista de simultaneidad también modifica la firma de su base de datos. Recuerde que en nuestro primer tutorial, [ *crear una capa de acceso a datos*](../introduction/creating-a-data-access-layer-cs.md), que los métodos directos de base de datos eran las que acepta una lista de escalar los valores como parámetros de entrada (en lugar de una fila de datos fuertemente tipados o Instancia de DataTable). Cuando se utiliza simultaneidad optimista, la base de datos directa `Update()` y `Delete()` métodos incluyen parámetros de entrada para los valores originales. Además, el código en la capa BLL para usar el lote actualizar patrón (el `Update()` sobrecargas del método que acepten objetos DataRow y tablas de datos en lugar de valores escalares) debe cambiarse también.

En su lugar de ampliar nuestra existente TableAdapters de DAL para usar la simultaneidad optimista (lo que precisaría de cambiar el nivel de lógica empresarial para dar cabida a), vamos a en su lugar, crear nuevo DataSet con tipo denominado `NorthwindOptimisticConcurrency`, para lo que vamos a agregar un `Products` TableAdapter que utiliza simultaneidad optimista. A continuación, vamos a crear un `ProductsOptimisticConcurrencyBLL` clase de capa de lógica empresarial que tiene las modificaciones necesarias para admitir la simultaneidad optimista DAL. Una vez que se ha diseñado este marco de trabajo, se estará listos para crear la página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Paso 2: Creación de una capa de acceso a datos que admite la simultaneidad optimista

Para crear un nuevo conjunto de datos con tipo, haga doble clic en el `DAL` carpeta dentro de la `App_Code` carpeta y agregue un nuevo conjunto de datos denominado `NorthwindOptimisticConcurrency`. Como hemos visto en el primer tutorial, si lo hace así agregará un nuevo TableAdapter a DataSet con tipo, iniciar automáticamente el Asistente para configuración de TableAdapter. En la primera pantalla, nos estamos le solicitará que especifique la base de datos para conectarse a: conexión a la misma base de datos Northwind utilizando el `NORTHWNDConnectionString` de `Web.config`.


[![Conectarse a la misma base de datos Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figura 3**: conectarse a la misma base de datos de Northwind ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image9.png))


A continuación, se nos pide sobre cómo consultar los datos: a través de una instrucción SQL ad hoc, un nuevo procedimiento almacenado o procedimiento almacenado de una existente. Ya que hemos usado las consultas SQL ad hoc en nuestro DAL original, utilice esta opción aquí también.


[![Especifique los datos que se va a recuperar utilizando una instrucción de SQL Ad Hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figura 4**: especificar los datos para recuperar mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image12.png))


En la siguiente pantalla, escriba la consulta SQL se utiliza para recuperar la información del producto. Vamos a usar la consulta SQL misma exacta utilizada para la `Products` TableAdapter desde nuestro DAL original, que devuelve todos los `Product` columnas junto con los nombres de proveedor y categoría del producto:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Use la misma consulta SQL desde el TableAdapter de productos en la capa DAL Original](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figura 5**: usar la misma consulta SQL desde el `Products` TableAdapter en la capa DAL Original ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image15.png))


Antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. Para que este control de simultaneidad optimista emplean TableAdapter, simplemente marque la casilla "Usar simultaneidad optimista".


[![Habilitar Control de simultaneidad optimista mediante la comprobación de la &quot;usar simultaneidad optimista&quot; casilla de verificación](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figura 6**: habilitar el Control de simultaneidad optimista activando la casilla "Usar simultaneidad optimista" ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image18.png))


Por último, indicar que el TableAdapter debe usar los patrones de acceso de datos que devuelven un DataTable; y rellenar un DataTable También indican que se deben crear los métodos directos de la base de datos. Cambiar el nombre del método para el retorno de un patrón de DataTable de GetData a GetProducts, con el fin de reflejar las convenciones de nomenclatura usamos en nuestro DAL original.


[![Tiene lo TableAdapter utilizan todos los patrones de acceso a datos](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figura 7**: tiene los TableAdapter utilizan todos los datos de patrones de acceso ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image21.png))


Después de completar el asistente, el Diseñador de DataSet incluirá fuertemente tipadas `Products` DataTable y un TableAdapter. Tómese un momento para cambiar el nombre de la DataTable de `Products` a `ProductsOptimisticConcurrency`, lo que puede hacer con el botón secundario en la barra de título de la DataTable y eligiendo el cambio de nombre en el menú contextual.


[![Una tabla de datos y un TableAdapter se han agregado al conjunto de datos con tipo](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figura 8**: una DataTable y TableAdapter se han agregado al conjunto de datos con tipo ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image24.png))


Para ver las diferencias entre la `UPDATE` y `DELETE` consultas entre el `ProductsOptimisticConcurrency` TableAdapter (que utiliza simultaneidad optimista) y un TableAdapter de productos (que no), haga clic en el TableAdapter y vaya a la ventana Propiedades. En el `DeleteCommand` y `UpdateCommand` propiedades `CommandText` subpropiedades puede ver la sintaxis SQL que se envía a la base de datos cuando se invocan el DAL update o métodos relacionados con la eliminación. Para el `ProductsOptimisticConcurrency` TableAdapter el `DELETE` es la instrucción que se utiliza:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Mientras que el `DELETE` instrucción del TableAdapter de producto en nuestro DAL original es mucho más sencillo:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Como puede ver, el `WHERE` cláusula en el `DELETE` instrucción del TableAdapter que utiliza simultaneidad optimista incluye una comparación entre cada uno de los `Product` tabla existente del valores de columna y los valores originales en el momento en el GridView (o DetailsView o FormView) se rellenó por última vez. Desde todos los campos distinto `ProductID`, `ProductName`, y `Discontinued` puede tener `NULL` se incluyen para comparar correctamente los valores, los parámetros adicionales y comprobaciones `NULL` valores en el `WHERE` cláusula.

Que no van a agregar las tablas de datos adicionales para el conjunto de datos habilitadas para simultaneidad optimista para este tutorial, como nuestra página ASP.NET sólo proporcionará actualización y eliminación de información del producto. Sin embargo, aún necesitamos agregar el `GetProductByProductID(productID)` método a la `ProductsOptimisticConcurrency` TableAdapter.

Para ello, haga doble clic en la barra de título del TableAdapter (la derecha del área por encima del `Fill` y `GetProducts` los nombres de método) y elija Agregar consulta en el menú contextual. Esto iniciará al Asistente para configuración de consulta de TableAdapter. Como con la configuración inicial de nuestra TableAdapter, optar por crear la `GetProductByProductID(productID)` método mediante una instrucción de SQL ad hoc (consulte la figura 4). Puesto que la `GetProductByProductID(productID)` método devuelve información sobre un producto determinado, para indicar que esta consulta es un `SELECT` tipo que devuelve filas de consulta.


[![Marcar el tipo de consulta como un &quot;LECT que devuelve filas&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figura 9**: marcar el tipo de consulta como un "`SELECT` que devuelve filas" ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image27.png))


En la siguiente pantalla estamos solicitará la consulta SQL usar con la consulta del TableAdapter predeterminado previamente cargada. Aumentar la consulta existente para incluir la cláusula `WHERE ProductID = @ProductID`, como se muestra en la figura 10.


[![Agregar un WHERE cláusula a la consulta para devolver un producto específico de registro cargada previamente.](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figura 10**: agregar un `WHERE` cláusula a la consulta Pre-Loaded para devolver un registro específico del producto ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image30.png))


Por último, cambie los nombres de método generados para `FillByProductID` y `GetProductByProductID`.


[![Cambiar el nombre de los métodos FillByProductID y GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figura 11**: cambiar el nombre de los métodos para `FillByProductID` y `GetProductByProductID` ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image33.png))


Con este Asistente completado, lo TableAdapter ahora contiene dos métodos para recuperar datos: `GetProducts()`, que devuelve *todas* productos; y `GetProductByProductID(productID)`, que devuelve el producto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Paso 3: Creación de una capa de lógica empresarial para DAL habilitadas para simultaneidad optimista

Nuestro existente `ProductsBLL` clase tiene ejemplos del uso de la actualización por lotes y la directas patrones de base de datos. El `AddProduct` método y `UpdateProduct` sobrecargas ambos utilizan el modelo de actualización por lotes, pasando un `ProductRow` instancia al método Update del TableAdapter. El `DeleteProduct` método, por otro lado, usa el patrón de base de datos directo, una llamada del TableAdapter `Delete(productID)` método.

Con el nuevo `ProductsOptimisticConcurrency` TableAdapter, la base de datos directa ahora métodos requieren que también se pueden pasar los valores originales en. Por ejemplo, el `Delete` método espera ahora diez parámetros de entrada: original `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, y `Discontinued`. Usa los valores de estos parámetros de entrada adicional en `WHERE` cláusula de la `DELETE` instrucción enviada a la base de datos, solo eliminar el registro especificado si se asignan los valores actuales de la base de datos hasta los originales.

Mientras que la firma del método del TableAdapter `Update` no ha cambiado el método que se usa en el patrón de actualización por lotes, tiene el código necesario para registrar los valores originales y nuevos. Por lo tanto, en lugar de intentar usar DAL habilitadas para simultaneidad optimista con nuestras `ProductsBLL` class, vamos a crear una nueva clase de capa de lógica empresarial para trabajar con nuestra nueva DAL.

Agregue una clase denominada `ProductsOptimisticConcurrencyBLL` a la `BLL` carpeta dentro de la `App_Code` carpeta.


![Agregue la clase ProductsOptimisticConcurrencyBLL a la carpeta BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figura 12**: agregar el `ProductsOptimisticConcurrencyBLL` clase a la carpeta de nivel de lógica empresarial


A continuación, agregue el código siguiente a la `ProductsOptimisticConcurrencyBLL` clase:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Tenga en cuenta el uso de `NorthwindOptimisticConcurrencyTableAdapters` instrucción anteriormente el inicio de la declaración de clase. El `NorthwindOptimisticConcurrencyTableAdapters` espacio de nombres contiene la `ProductsOptimisticConcurrencyTableAdapter` (clase), que proporciona los métodos de la capa DAL. También antes de la declaración de clase, encontrará el `System.ComponentModel.DataObject` atributo, que indica a Visual Studio que incluya esta clase en la lista desplegable de ObjectDataSource del asistente.

El `ProductsOptimisticConcurrencyBLL`del `Adapter` propiedad proporciona acceso rápido a una instancia de la `ProductsOptimisticConcurrencyTableAdapter` clase y sigue el patrón utilizado en nuestras clases BLL originales (`ProductsBLL`, `CategoriesBLL`, y así sucesivamente). Por último, el `GetProducts()` método simplemente llama hacia abajo a la DAL `GetProducts()` método y devuelve un `ProductsOptimisticConcurrencyDataTable` rellena el objeto con un `ProductsOptimisticConcurrencyRow` instancia para cada registro del producto en la base de datos.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Eliminación de un producto con el patrón directo de la base de datos con la simultaneidad optimista

Cuando se usa el patrón de base de datos directo contra un DAL que utiliza simultaneidad optimista, los métodos deben pasar los valores nuevos y originales. Para eliminar, no existen nuevos valores, por lo que se necesitan pasar sólo los valores originales. En nuestro BLL, a continuación, debemos aceptamos todos los parámetros originales como parámetros de entrada. Vamos a tener la `DeleteProduct` método en el `ProductsOptimisticConcurrencyBLL` clase usa el método directo de la base de datos. Esto significa que este método debe tomar en todos los campos de datos de diez productos como parámetros de entrada y pasa a la capa DAL, tal como se muestra en el código siguiente:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Si los valores originales - aquellos valores que se cargaron por última vez en el control GridView (o DetailsView o FormView) - difieren de los valores de la base de datos cuando el usuario hace clic en el botón Eliminar el `WHERE` cláusula no coincide con cualquier registro de base de datos y no hay registros se verán afectadas. Por lo tanto, lo TableAdapter `Delete` método devolverá `0` y el BLL `DeleteProduct` método devolverá `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Actualizar un producto con el patrón de actualización por lotes con la simultaneidad optimista

Como se indicó anteriormente, lo TableAdapter `Update` método para el patrón de actualización por lotes tiene la misma firma de método, independientemente de si se emplea la simultaneidad optimista. Es decir, el `Update` método espera un objeto DataRow, una matriz de objetos DataRow, DataTable o un conjunto de datos con tipo. No hay ningún parámetro de entrada adicional para especificar los valores originales. Esto es posible porque la tabla de datos realiza un seguimiento de los valores originales y modificados para su DataRow(s). Cuando se emite la capa DAL su `UPDATE` instrucción, el `@original_ColumnName` parámetros se rellenan con los valores originales de DataRow, mientras que el `@ColumnName` parámetros se rellenan con los valores modificados de DataRow.

En el `ProductsBLL` (clase) (que usa nuestro DAL de simultaneidad original, que no es optimista), cuando se usa el patrón de actualización por lotes para actualizar la información de producto nuestro código realiza la siguiente secuencia de eventos:

1. Lea la información de producto de base de datos actual en un `ProductRow` instancia mediante el TableAdapter `GetProductByProductID(productID)` (método)
2. Asignar los nuevos valores para el `ProductRow` instancia del paso 1
3. Llamar a del TableAdapter `Update` método, pasando el `ProductRow` instancia

Esta secuencia de pasos, sin embargo, no admite correctamente la simultaneidad optimista porque los `ProductRow` rellena en el paso 1 se rellena directamente desde la base de datos, lo que significa que los valores originales utilizados por la DataRow son las que existen actualmente en el base de datos y no a los que se enlazaron a GridView al principio del proceso de edición. En su lugar, cuando uso un optimista habilitadas para simultaneidad DAL, necesitamos modificar la `UpdateProduct` sobrecargas del método que use los pasos siguientes:

1. Lea la información de producto de base de datos actual en un `ProductsOptimisticConcurrencyRow` instancia mediante el TableAdapter `GetProductByProductID(productID)` (método)
2. Asignar el *original* valores para el `ProductsOptimisticConcurrencyRow` instancia del paso 1
3. Llame a la `ProductsOptimisticConcurrencyRow` la instancia `AcceptChanges()` método, que indica la DataRow que sus valores actuales son los "originales"
4. Asignar el *nueva* valores para el `ProductsOptimisticConcurrencyRow` instancia
5. Llamar a del TableAdapter `Update` método, pasando el `ProductsOptimisticConcurrencyRow` instancia

Paso 1 lecturas en todos los valores de la base de datos actual para el registro del producto especificado. Este paso es superflua en el `UpdateProduct` sobrecarga que actualiza *todas* de las columnas de producto (como estos valores se sobrescriben en el paso 2), pero es esencial para dichas sobrecargas donde sólo un subconjunto de los valores de columna se pasan como parámetros de entrada. Una vez que se han asignado los valores originales para el `ProductsOptimisticConcurrencyRow` instancia, el `AcceptChanges()` se llama el método, que marca los valores actuales de DataRow como los valores originales que se usará en el `@original_ColumnName` parámetros en el `UPDATE` instrucción. A continuación, se asignan los nuevos valores de parámetro a la `ProductsOptimisticConcurrencyRow` y, finalmente, el `Update` se invoca el método, pasando la DataRow.

El código siguiente muestra el `UpdateProduct` sobrecarga que acepta datos de productos de todos los campos como parámetros de entrada. Mientras no se muestra aquí, el `ProductsOptimisticConcurrencyBLL` clase incluida en la descarga para este tutorial también contiene un `UpdateProduct` sobrecarga que acepta sólo el producto nombre y el precio como parámetros de entrada.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Paso 4: Pasando los valores originales y nuevos de la página ASP.NET a los métodos BLL

Con DAL y BLL completa, todo lo que queda es crear una página ASP.NET que puede utilizar la lógica de la simultaneidad optimista integrada el sistema. En concreto, los datos de control de Web (GridView, DetailsView o FormView) deben recordar que sus valores originales y ObjectDataSource deben pasar ambos conjuntos de valores a la capa de lógica empresarial. Además, la página ASP.NET debe configurarse para controlar correctamente las infracciones de simultaneidad.

Comience abriendo la `OptimisticConcurrency.aspx` página en el `EditInsertDelete` carpeta y agregar un control GridView al diseñador, establecer su `ID` propiedad `ProductsGrid`. Desde las etiquetas inteligentes de GridView, optar por crear un nuevo origen ObjectDataSource denominado `ProductsOptimisticConcurrencyDataSource`. Puesto que deseamos que este origen ObjectDataSource para usar la capa DAL que admite la simultaneidad optimista, configúrelo para utilizar el `ProductsOptimisticConcurrencyBLL` objeto.


[![Tiene el uso de ObjectDataSource el objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figura 13**: tiene el uso de ObjectDataSource el `ProductsOptimisticConcurrencyBLL` objeto ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image37.png))


Elija la `GetProducts`, `UpdateProduct`, y `DeleteProduct` métodos desde listas desplegables en el asistente. El método UpdateProduct por separado, utilice la sobrecarga que acepta todos los campos de datos del producto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configuración de las propiedades del Control ObjectDataSource

Después de completar al asistente, marcado declarativo de ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Como puede ver, el `DeleteParameters` colección contiene un `Parameter` instancia para cada uno de los parámetros de entrada diez en el `ProductsOptimisticConcurrencyBLL` la clase `DeleteProduct` método. Del mismo modo, el `UpdateParameters` colección contiene un `Parameter` instancia para cada uno de los parámetros de entrada en `UpdateProduct`.

Para los tutoriales anteriores que implica la modificación de datos, se recomienda eliminar el ObjectDataSource `OldValuesParameterFormatString` propiedad en este punto, ya que esta propiedad indica que el método BLL espera que los valores antiguos (u originales) que se pasarán en, así como los nuevos valores. Además, el valor de esta propiedad indica los nombres de parámetro de entrada para los valores originales. Puesto que estamos pasando los valores originales en el BLL, hacer *no* quite esta propiedad.

> [!NOTE]
> El valor de la `OldValuesParameterFormatString` propiedad se debe asignar a los nombres de parámetro de entrada en el nivel de lógica empresarial que esperan los valores originales. Puesto que hemos llamado a estos parámetros `original_productName`, `original_supplierID`, y así sucesivamente, puede dejar el `OldValuesParameterFormatString` como valor de propiedad `original_{0}`. Si, sin embargo, los parámetros de entrada de los métodos de la capa BLL tenían nombres como `old_productName`, `old_supplierID`, y así sucesivamente, deberá actualizar el `OldValuesParameterFormatString` propiedad `old_{0}`.


Hay una configuración de propiedad final que debe realizarse en orden para el origen ObjectDataSource pasar correctamente los valores originales para los métodos BLL. ObjectDataSource tiene un [propiedad ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que pueden asignarse a [uno de dos valores](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -el valor predeterminado; no envía los valores originales para los parámetros de entrada original de los métodos de la capa BLL
- `CompareAllValues` -enviar los valores originales para los métodos BLL; Elija esta opción cuando se utiliza simultaneidad optimista

Dedique un momento para establecer el `ConflictDetection` propiedad `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configuración de los campos y propiedades del control GridView

Con las propiedades de ObjectDataSource configuradas correctamente, dirijamos nuestra atención a la configuración de GridView. En primer lugar, puesto que deseamos que el control GridView para admitir la edición y eliminación, haga clic en las casillas de verificación Habilitar edición y habilitar la eliminación de etiquetas inteligentes de GridView. Esto agregará un CommandField cuyo `ShowEditButton` y `ShowDeleteButton` están establecidos en `true`.

Cuando se enlaza a la `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo para cada uno de los campos de datos del producto. Aunque se puede editar un GridView de este tipo, la experiencia del usuario es cualquier cosa menos aceptable. El `CategoryID` y `SupplierID` BoundFields se representarán como cuadros de texto, pedir al usuario que escriba la categoría adecuada y el proveedor como números de identificación. No habrá ningún formato para que los campos numéricos y no hay controles de validación para asegurarse de que se ha proporcionado el nombre del producto y que el precio unitario, unidades en existencias, unidades en orden y los valores de nivel de reordenación son ambos valores numéricos apropiados y son mayores que o igual a en cero.

Como se explicó en la *agregar controles de validación a las Interfaces de inserción y edición* y *personalizar la interfaz de modificación de datos* tutoriales, se puede personalizar la interfaz de usuario mediante reemplazando el BoundFields con TemplateFields. He modificado esta GridView y su interfaz de edición de las maneras siguientes:

- Quita el `ProductID`, `SupplierName`, y `CategoryName` BoundFields
- Convertir el `ProductName` BoundField a TemplateField y ha agregado un control RequiredFieldValidation.
- Convertir el `CategoryID` y `SupplierID` BoundFields a TemplateFields y ajustar la interfaz de edición para que use las listas desplegables en lugar de cuadros de texto. En estos TemplateFields' `ItemTemplates`, `CategoryName` y `SupplierName` se muestran los campos de datos.
- Convertir el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` BoundFields TemplateFields y controles CompareValidator agregados.

Puesto que ya hemos examinado cómo realizar estas tareas en los tutoriales anteriores, me simplemente la sintaxis declarativa final de la lista y dejar la implementación como práctica.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Estamos muy cerca de un ejemplo totalmente funcional. Sin embargo, hay algunos matices que surgen de y que nos puede provocar problemas. Además, se necesita alguna interfaz que alerta al usuario cuando se ha producido una infracción de simultaneidad.

> [!NOTE]
> Para un control Web de datos que puedan pasar correctamente los valores originales para el origen ObjectDataSource (que, a continuación, se pasan a la capa BLL), es fundamental que la GridView `EnableViewState` propiedad está establecida en `true` (valor predeterminado). Si deshabilita el estado de vista, los valores originales se pierden en el postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Pasar los valores originales correctos para el origen ObjectDataSource

Hay un par de problemas con la forma en que se ha configurado el control GridView. Si el ObjectDataSource `ConflictDetection` propiedad está establecida en `CompareAllValues` (tal como está nuestra), cuando el ObjectDataSource `Update()` o `Delete()` métodos se invocan mediante el control GridView (o DetailsView o FormView), ObjectDataSource intenta copiar el Los valores originales de GridView en su correspondiente `Parameter` instancias. Consulte la figura 2 para obtener una representación gráfica de este proceso.

En concreto, los valores originales de GridView se asignan los valores en las instrucciones de enlace de datos bidireccional, cada vez que los datos se enlazan a la GridView. Por lo tanto, es esencial que se capturan los valores originales requiere todas a través del enlace de datos bidireccional y que están disponibles en un formato convertible.

Para ver por qué esto es importante, dedique un momento a visite nuestra página en un explorador. Según lo esperado, el control GridView enumera todos los productos con un botón de edición y eliminación de la columna izquierda.


[![Los productos se muestran en un control GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figura 14**: se enumeran los productos en un control GridView ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image40.png))


Si hace clic en el botón Eliminar para cualquier producto, un `FormatException` se produce.


[![Intentando eliminar los resultados de cualquier producto en un FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figura 15**: intentando eliminar cualquier producto resultados en un `FormatException` ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image43.png))


El `FormatException` se genera cuando el origen ObjectDataSource intenta leer en el original `UnitPrice` valor. Puesto que la `ItemTemplate` tiene la `UnitPrice` con formato de moneda (`<%# Bind("UnitPrice", "{0:C}") %>`), incluye un símbolo de moneda, como $19,95. El `FormatException` se produce cuando el origen ObjectDataSource intenta convertir esta cadena en un `decimal`. Para prevenir este problema, tenemos varias opciones:

- Quitar la moneda en el formato de la `ItemTemplate`. Es decir, en lugar de usar `<%# Bind("UnitPrice", "{0:C}") %>`, basta con usar `<%# Bind("UnitPrice") %>`. La desventaja de esto es que el precio ya no tiene el formato.
- Mostrar el `UnitPrice` con formato como una moneda en la `ItemTemplate`, pero usar el `Eval` palabra clave para lograr esto. Recuerde que `Eval` realiza el enlace de datos unidireccional. Necesitamos proporcionar el `UnitPrice` valor para los valores originales, por lo que tendrá que una instrucción de enlace de datos bidireccional en el `ItemTemplate`, pero esto se puede colocar en un control Label Web cuya `Visible` propiedad está establecida en `false`. En la plantilla ItemTemplate podríamos usar el siguiente marcado:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Quitar la moneda en el formato de la `ItemTemplate`, usando `<%# Bind("UnitPrice") %>`. En el control de GridView `RowDataBound` controlador de eventos, mediante programación el acceso Web de la etiqueta de control dentro del cual el `UnitPrice` valor se muestra y establece su `Text` propiedad a la versión con formato.
- Deje el `UnitPrice` con formato de moneda. En el control de GridView `RowDeleting` controlador de eventos, reemplazar al original existente `UnitPrice` valor (US $19,95) con el uso de un valor decimal real `Decimal.Parse`. Hemos visto cómo lograr algo similar en el `RowUpdating` controlador de eventos en el [ *BLL - control y excepciones de nivel DAL en una página ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial.

Para mi ejemplo opté por el segundo enfoque, agregar una Web etiqueta oculta control cuya `Text` propiedad es bidireccional enlazado a datos con el formato `UnitPrice` valor.

Después de resolver este problema, intente hacer clic en el botón Eliminar para cualquier producto de nuevo. Esta vez obtendrá un `InvalidOperationException` cuando el origen ObjectDataSource intenta invocar el BLL `UpdateProduct` método.


[![ObjectDataSource no puede encontrar un método con los parámetros de entrada desea enviar](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figura 16**: ObjectDataSource no puede encontrar un método con los parámetros de entrada desea enviar ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image46.png))


Examinar el mensaje de excepción, está claro que ObjectDataSource desea invocar un BLL `DeleteProduct` método que incluye `original_CategoryName` y `original_SupplierName` parámetros de entrada. Esto es porque el `ItemTemplate` s para el `CategoryID` y `SupplierID` TemplateFields actualmente contienen instrucciones de enlace bidireccionales con el `CategoryName` y `SupplierName` campos de datos. En su lugar, debemos incluir `Bind` instrucciones con el `CategoryID` y `SupplierID` campos de datos. Para ello, sustituya las instrucciones de enlace existentes con `Eval` instrucciones y, a continuación, agregue la etiqueta oculta los controles cuya propiedad `Text` propiedades se enlazan a la `CategoryID` y `SupplierID` mediante enlace de datos bidireccional, como se muestra los campos de datos a continuación:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Con estos cambios, ahora es posible eliminar y editar la información del producto correctamente! En el paso 5 se echaremos un vistazo a cómo comprobar que se están detectando las infracciones de simultaneidad. Pero por ahora, tardar unos minutos para que intente actualizar y eliminar algunos registros para asegurarse de que la actualización y eliminación para un solo usuario funciona según lo previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Paso 5: Probar la compatibilidad de simultaneidad optimista

Para comprobar que las infracciones de simultaneidad se están detectado (en lugar de resultante en los datos que se sobrescriban ciegamente), es necesario abrir dos ventanas del explorador a esta página. En ambas instancias del explorador, haga clic en el botón Editar para Chai. A continuación, en solo uno de los exploradores, cambie el nombre a "Té Chai" y haga clic en actualizar. La actualización debe se ejecuta correctamente y devolver el control GridView a su estado de edición previamente, con "Chai té" como el nuevo nombre de producto.

Sin embargo, en la otra instancia de ventana Explorador, el nombre del producto del cuadro de texto sigue mostrando "Chai". En esta segunda ventana del explorador, actualice el `UnitPrice` a `25.00`. Sin compatibilidad con la simultaneidad optimista, haga clic en actualizar en la segunda instancia del explorador cambiaría el nombre del producto a "Chai", sobrescribiendo los cambios realizados por la primera instancia del explorador. Con la simultaneidad optimista empleada, sin embargo, al hacer clic en el botón de actualización de la segunda instancia del explorador da como resultado un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Cuando se detecta una infracción de simultaneidad, se produce una excepción DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figura 17**: se detecta cuando una infracción de simultaneidad, un `DBConcurrencyException` se produce ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image49.png))


El `DBConcurrencyException` sólo se produce cuando se utiliza el patrón de actualización por lotes de la capa DAL. El patrón directo de la base de datos no genera una excepción, simplemente indica que no hay filas afectadas. Para ilustrar esto, devolver GridView las instancias de ambos explorador a su estado de edición previamente. A continuación, en la primera instancia del explorador, haga clic en el botón Editar y cambiar el nombre del producto "Té Chai" a "Chai" y haga clic en actualizar. En la segunda ventana del explorador, haga clic en el botón Eliminar para Chai.

Al hacer clic en eliminar, devuelve la página, el control GridView invoca el ObjectDataSource `Delete()` método y el origen ObjectDataSource llama a la `ProductsOptimisticConcurrencyBLL` la clase `DeleteProduct` método, pasando a lo largo de los valores originales. La versión original `ProductName` valor de la segunda instancia del explorador es "Chai té", que no coincide con la actual `ProductName` valor en la base de datos. Por lo tanto, el `DELETE` cero filas porque no hay ningún registro en la base de datos afecta a la instrucción emitida a la base de datos que el `WHERE` cláusula satisface. El `DeleteProduct` devuelve del método `false` y se vuelve a enlazar datos de ObjectDataSource en GridView.

Desde la perspectiva del usuario final, al hacer clic en el botón Eliminar para té Chai en la segunda ventana del explorador ha provocado la pantalla de flash y, al volver, el producto aún está allí, aunque ahora aparece como "Chai" (el producto nombre cambio realizado por el primer explorador (instancia). Si el usuario hace clic el botón Eliminar de nuevo, la eliminación se realizará correctamente, como GridView original `ProductName` valor ("Chai") ahora coincide con el valor de la base de datos.

En ambos casos, la experiencia del usuario está lejos de ser ideal. Claramente no queremos mostrar al usuario los detalles esenciales de la `DBConcurrencyException` excepción cuando se usa el patrón de actualización por lotes. Y el comportamiento cuando se usa el patrón directo de la base de datos es un poco confuso como error del comando de los usuarios, pero no hay ninguna indicación precisa de por qué.

Para solucionar estos dos problemas, podemos crear controles Web de la etiqueta en la página que proporcionan una explicación a por qué una actualización o eliminación no se pudo. Para el patrón de actualización por lotes, podemos determinar si un `DBConcurrencyException` se ha producido una excepción en el controlador de eventos posteriores al nivel de GridView, mostrar la etiqueta de advertencia según sea necesario. Para el método directo de la base de datos, podemos examinar el valor devuelto del método BLL (que es `true` si una fila se ve afectada, `false` en caso contrario) y mostrar un mensaje informativo según sea necesario.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Paso 6: Agregar mensajes informativos y mostrarlos en el caso de una infracción de simultaneidad

Cuando se produce una infracción de simultaneidad, el comportamiento que mostró depende de si se usó la DAL actualización por lotes o patrón de base de datos directo. En este tutorial usa ambos patrones, con el patrón de actualización por lotes que se va a usar para la actualización y el patrón de base de datos directo que se usa para eliminar. Para empezar, vamos a agregar dos controles Web de la etiqueta a la página que se explica que se ha producido una infracción de simultaneidad cuando se intenta eliminar o actualizar los datos. Establecer el control de etiqueta `Visible` y `EnableViewState` propiedades a `false`; Esto hará que se oculta en cada visita de página, excepto para aquellos determinada página visita where sus `Visible` propiedad se establece mediante programación en `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Además de establecer su `Visible`, `EnabledViewState`, y `Text` propiedades, también establecí el `CssClass` propiedad `Warning`, lo que hace que la etiqueta de la que se mostrará en una fuente grande, rojo, cursiva, negrita. Este CSS `Warning` clase se definen y se agregó a Styles.css iniciarla el *examinando los eventos asociados con la inserción, actualización y eliminación* tutorial.

Después de agregar estas etiquetas, el Diseñador de Visual Studio debe ser similar a la figura 18.


[![Se han agregado dos controles de etiqueta a la página](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figura 18**: dos etiqueta los controles se han agregado a la página ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image52.png))


Con estos controles Web de la etiqueta en su lugar, estamos listos examinar cómo determinar cuándo una infracción de simultaneidad, momento en que la etiqueta adecuada `Visible` propiedad puede establecerse en `true`, mostrar el mensaje informativo.

## <a name="handling-concurrency-violations-when-updating"></a>Control de las infracciones de simultaneidad al actualizar

Veamos primero cómo tratar las infracciones de simultaneidad cuando se usa el patrón de actualización por lotes. Puesto que tales infracciones en el lote de actualización a causa del patrón un `DBConcurrencyException` excepción, tenemos que agregar código a nuestra página ASP.NET para determinar si un `DBConcurrencyException` se ha producido una excepción durante el proceso de actualización. Si por lo tanto, debemos mostramos un mensaje para el usuario que explica que sus cambios no se guardaron porque otro usuario había modificado los mismos datos entre cuando comenzaron a editar el registro, y cuando hace clic en el botón Actualizar.

Como hemos visto en el *BLL - control y excepciones de nivel DAL en una página ASP.NET* tutorial dichas excepciones se pueden detectar y suprimir en controladores de eventos posteriores al nivel del control de datos Web. Por lo tanto, es necesario crear un controlador de eventos para el control de GridView `RowUpdated` eventos que comprueba si un `DBConcurrencyException` se ha producido la excepción. Este controlador de eventos se pasa una referencia a cualquier excepción que se produjo durante el proceso de actualización, tal como se muestra en el caso de controlador de código a continuación:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

En el de un `DBConcurrencyException` muestra de este controlador de eventos de excepción, el `UpdateConflictMessage` control de etiqueta e indica que se ha controlado la excepción. Con este código en su lugar, cuando se produce una infracción de simultaneidad cuando se actualiza un registro, los cambios del usuario se pierden, puesto que habría sobrescribe las modificaciones de otro usuario al mismo tiempo. En concreto, GridView se vuelve a su estado de edición previamente y enlazado a la base de datos actual. La fila del control GridView se actualizará con los cambios del otro usuario, que estaban anteriormente no es visibles. Además, el `UpdateConflictMessage` explicará control de etiqueta para el usuario lo que ha sucedido. Esta secuencia de eventos se detalla en la figura 19.


[![Un usuario s se pierden las actualizaciones en la cara de una infracción de simultaneidad](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figura 19**: un usuario s se pierden las actualizaciones en la cara de una infracción de simultaneidad ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Como alternativa, en lugar de devolver el control GridView en el estado de edición previamente, podríamos deja el control GridView en su estado de edición estableciendo el `KeepInEditMode` propiedad del pasado de `GridViewUpdatedEventArgs` objeto en true. Si adopta este enfoque, sin embargo, asegúrese de volver a enlazar los datos en GridView (invocando su `DataBind()` método) para que se cargan los valores de otro usuario en la interfaz de edición. El código disponible para su descarga con este tutorial tiene estas dos líneas de código en el `RowUpdated` comentadas de controlador de eventos; simplemente quite estas líneas de código para tener el control GridView permanecen en modo de edición tras una infracción de simultaneidad.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Responder a las infracciones de simultaneidad al eliminar

Con el patrón de base de datos directo, no hay ninguna excepción en caso de una infracción de simultaneidad. En su lugar, simplemente la instrucción de base de datos afecta a ningún registro, como la cláusula WHERE no coincide con todos los registros. Todos los métodos de modificación de datos creados en el nivel de lógica empresarial se han diseñado tal que devuelvan un valor booleano que indica si las aplicaciones afectadas con precisión un registro. Por lo tanto, para determinar si se ha producido una infracción de simultaneidad cuando se elimina un registro, podemos examinar el valor devuelto de la capa de BLL `DeleteProduct` método.

Se puede examinar el valor devuelto para un método BLL en los controladores de eventos posteriores al nivel de ObjectDataSource a través de la `ReturnValue` propiedad de la `ObjectDataSourceStatusEventArgs` objeto pasado en el controlador de eventos. Puesto que estamos interesados en la determinación del valor devuelto desde el `DeleteProduct` método, debemos crear un controlador de eventos para el origen de ObjectDataSource `Deleted` eventos. El `ReturnValue` propiedad es de tipo `object` y puede ser `null` si se generó una excepción y el método se interrumpió antes de que pudo devolver un valor. Por lo tanto, nos debemos primero asegúrese de que el `ReturnValue` propiedad no es `null` y es un valor booleano. Suponiendo que supera esta comprobación, mostramos el `DeleteConflictMessage` etiqueta de control si el `ReturnValue` es `false`. Esto puede realizarse mediante el código siguiente:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

En caso de una infracción de simultaneidad, se cancela la solicitud de eliminación del usuario. Se actualiza el control GridView, que muestra los cambios efectuados para ese registro entre el momento en que el usuario cargan la página y cuando hace clic en el botón Eliminar. Cuando ocurre una infracción de este tipo, el `DeleteConflictMessage` etiqueta se muestra, que explica lo que ha sucedido (Véase la figura 20).


[![Un usuario s Delete se cancela en caso de una infracción de simultaneidad](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figura 20**: s de un usuario ha cancelado la eliminación en el caso de una infracción de simultaneidad ([haga clic aquí para ver imagen en tamaño completo](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Resumen

Existen oportunidades las infracciones de simultaneidad en todas las aplicaciones que permite que varios usuarios simultáneos para actualizar o eliminar datos. Si no se tienen en cuenta en este tipo de infracciones para cuando dos usuarios actualizan simultáneamente los mismos datos quienquiera que obtiene de la última escritura "gana", sobrescribiendo el otro usuario cambia los cambios. Como alternativa, los desarrolladores pueden implementar cualquier control de simultaneidad optimista o pesimista. Control de simultaneidad optimista supone que las infracciones de simultaneidad son poco frecuentes y simplemente no permite una actualización o eliminar el comando que supondría una infracción de simultaneidad. Control de concurrencia pesimista se da por supuesto que simultaneidad infracciones son frecuentes y simplemente rechazar un usuario, actualizar o eliminar comandos no es aceptable. Con el control de simultaneidad pesimista, actualizar un registro implica bloqueándola, lo que impide a otros usuarios modifiquen o eliminen el registro, aunque esté bloqueado.

El conjunto de datos con tipo en .NET proporciona funcionalidad para admitir el control de simultaneidad optimista. En concreto, el `UPDATE` y `DELETE` instrucciones emitidas para la base de datos incluyen todas las columnas de la tabla, lo que garantiza que la actualización o eliminación sólo se producirá si los datos del registro actual coinciden con los datos originales, el usuario tenía cuando realizar su actualización o eliminación. Una vez que se ha configurado la capa DAL para admitir la simultaneidad optimista, los métodos BLL deben actualizarse. Además, debe configurarse la página ASP.NET que llama hacia abajo en la capa BLL, que recupera los valores originales de su control Web de datos y los pasa hacia abajo en la capa BLL ObjectDataSource.

Como hemos visto en este tutorial, implementación del control de simultaneidad optimista en una aplicación web ASP.NET implica actualizar DAL y BLL y agregar compatibilidad en la página ASP.NET. Si es o no este trabajo se ha agregado una buena inversión de su tiempo y esfuerzo depende de la aplicación. Si tiene usuarios simultáneos, actualizar datos con poca frecuencia o los datos que va a actualizar están diferentes entre sí, a continuación, el control de simultaneidad no es un problema clave. Sin embargo, habitualmente si tiene varios usuarios en el sitio de trabajar con los mismos datos, control de simultaneidad puede ayudar a evitar eliminaciones o actualizaciones de un usuario sobrescriba accidentalmente de otra.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](customizing-the-data-modification-interface-vb.md)
> [Siguiente](adding-client-side-confirmation-when-deleting-vb.md)
