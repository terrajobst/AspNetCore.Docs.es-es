---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Creación de una capa de lógica de negocios (VB) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial veremos cómo centralizar sus reglas de negocios en una capa de lógica empresarial (BLL) que actúa como intermediario para el intercambio de datos entre t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 150862decbbb69747f3e957a941b71b118b7231c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-business-logic-layer-vb"></a>Creación de una capa de lógica de negocios (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) o [descarga de PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> En este tutorial veremos cómo centralizar sus reglas de negocios en una capa de lógica empresarial (BLL) que actúa como intermediario para el intercambio de datos entre la capa de presentación y la capa DAL.


## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) creado en el [primer tutorial](creating-a-data-access-layer-vb.md) correctamente separa los datos de acceso lógica de la lógica de presentación. Sin embargo, mientras que la capa DAL separa claramente los detalles de acceso a datos de la capa de presentación, no aplica las reglas de negocio que se pueden aplicar. Por ejemplo, para la aplicación que podríamos desear no permitir la `CategoryID` o `SupplierID` campos de la `Products` tabla modificarse cuando el `Discontinued` campo se establece en 1 o que queramos aplicar reglas de antigüedad, prohibición de situaciones en las que un empleado está administrado por alguien que se contrató a continuación de ellos. Otro escenario común es autorización quizás solo los usuarios en un rol determinado, pueden eliminar los productos o pueden cambiar la `UnitPrice` valor.

En este tutorial veremos cómo centralizar estas reglas de negocios en una capa de lógica empresarial (BLL) que actúa como intermediario para el intercambio de datos entre la capa de presentación y la capa DAL. En una aplicación del mundo real, la capa BLL se debería implementar como un proyecto de biblioteca de clases independiente; Sin embargo, para estos tutoriales implementaremos el BLL como una serie de clases en nuestra `App_Code` carpeta con el fin de simplificar la estructura del proyecto. Figura 1 muestra las relaciones de arquitectura entre la capa de presentación, BLL y DAL.


![La capa BLL separa el nivel de presentación de la capa de acceso a datos e impone las reglas de negocios](creating-a-business-logic-layer-vb/_static/image1.png)

**Figura 1**: BLL separa el nivel de presentación de la capa de acceso a datos e impone las reglas de negocios


En lugar de crear clases independientes para implementar nuestro [lógica de negocios](http://en.wikipedia.org/wiki/Business_logic), o bien se puede colocar esta lógica directamente en el conjunto de datos con tipo con clases parciales. Para obtener un ejemplo de crear y ampliar un conjunto de datos con tipo, consulte el primer tutorial.

## <a name="step-1-creating-the-bll-classes"></a>Paso 1: Crear las clases BLL

Nuestro BLL podría estar compuesta por cuatro clases, uno para cada TableAdapter en la capa DAL; cada una de estas clases BLL tendrá métodos para recuperar, insertar, actualizar y eliminar del TableAdapter respectivo en la capa DAL, aplicar las reglas de negocios adecuada.

Para más correctamente separar las clases relacionadas con DAL y BLL, vamos a crear dos subcarpetas en la `App_Code` carpeta, `DAL` y `BLL`. Simplemente haga doble clic en el `App_Code` carpeta en el Explorador de soluciones y elija la nueva carpeta. Después de crear estas dos carpetas, mover el conjunto de datos con tipo creados en el primer tutorial en el `DAL` subcarpeta.

A continuación, cree los cuatro archivos de clase BLL en el `BLL` subcarpeta. Para ello, haga doble clic en el `BLL` subcarpeta, elija Agregar un nuevo elemento y elija la plantilla de clase. El nombre de las cuatro clases `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, y `EmployeesBLL`.


![Agregar cuatro nuevas clases a la carpeta App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Figura 2**: agregar cuatro nuevas clases para el `App_Code` carpeta


A continuación, vamos a agregar métodos a cada una de las clases que simplemente encapsulan los métodos definidos para los TableAdapters del primer tutorial. Por ahora, estos métodos simplemente llamará directamente en la capa DAL; se tendrá que volver más tarde para agregar cualquier lógica de negocios necesaria.

> [!NOTE]
> Si usa Visual Studio Standard Edition o una versión posterior (es decir, está *no* con Visual Web Developer), si lo desea puede diseñar sus clases visualmente usando el [Diseñador de clases](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Hacer referencia a la [clase diseñador Blog](https://blogs.msdn.com/classdesigner/default.aspx) para obtener más información sobre esta nueva característica en Visual Studio.


Para el `ProductsBLL` que necesitamos agregar un total de siete métodos de clase:

- `GetProducts()` Devuelve todos los productos
- `GetProductByProductID(productID)` Devuelve el producto con el identificador de producto especificado
- `GetProductsByCategoryID(categoryID)` Devuelve todos los productos de la categoría especificada
- `GetProductsBySupplier(supplierID)` Devuelve todos los productos del proveedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Inserta un nuevo producto en la base de datos con los valores pasados de; Devuelve el `ProductID` valor del registro recién insertado
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` actualiza un producto existente en la base de datos utilizando los valores en el pasado; Devuelve `True` si se ha actualizado con precisión una fila, `False` en caso contrario
- `DeleteProduct(productID)` Elimina el producto especificado de la base de datos

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Los métodos que simplemente devuelven datos `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, y `GetProductBySuppliersID` son bastante sencilla basta con llamar a hacia abajo en la capa DAL. Aunque en algunos escenarios pueden existir reglas de negocios que necesitan para implementarse en este nivel (por ejemplo, las reglas de autorización basada en el usuario ha iniciado sesión actualmente o el rol al que pertenece el usuario), simplemente dejaremos estos métodos como-es. De estos métodos, a continuación, la capa BLL sirve simplemente como un proxy a través del cual el nivel de presentación, tiene acceso a los datos subyacentes de la capa de acceso a datos.

El `AddProduct` y `UpdateProduct` métodos tanto toman como parámetros de los valores para los distintos campos de producto y agregar un nuevo producto o actualizar una ya existente, respectivamente. Dado que muchas de las `Product` pueden aceptar las columnas de tabla `NULL` valores (`CategoryID`, `SupplierID`, y `UnitPrice`, por nombrar algunos), los parámetros de entrada `AddProduct` y `UpdateProduct` que se asignan a dicho uso de columnas [tipos que aceptan valores NULL](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Tipos que aceptan valores NULL son nuevos para .NET 2.0 y proporcionar una técnica para que indica si un tipo de valor debe, en su lugar, ser `Nothing`. Hacer referencia a la [Paul Vick](http://www.panopticoncentral.net/)de entrada de blog [la verdad acerca de tipos que aceptan valores NULL y VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) y la documentación técnica de la [acepta valores NULL](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) estructura para obtener más información.

Los tres métodos devuelven un valor booleano que indica si una fila se ha insertado, actualizada o eliminado porque no está penada por la operación en una fila afectada. Por ejemplo, si el desarrollador de páginas llama `DeleteProduct` pasando un `ProductID` para un producto inexistente, el `DELETE` instrucción emitida a la base de datos no tendrá efecto alguno y, por tanto, la `DeleteProduct` método devolverá `False`.

Tenga en cuenta que al agregar un nuevo producto o actualizar uno existente se toman en los valores de campo nuevos o modificados del producto como una lista de valores escalares en lugar de aceptar un `ProductsRow` instancia. Este enfoque se ha elegido porque la `ProductsRow` clase se deriva de ADO.NET `DataRow` (clase), que no tiene un constructor sin parámetros predeterminado. Para crear un nuevo `ProductsRow` instancia, debemos crear primero un `ProductsDataTable` de instancia y, a continuación, invocar su `NewProductRow()` (método) (lo que hacemos en `AddProduct`). Este inconveniente rears su cabeza cuando es necesario para insertar y actualizar los productos a través de ObjectDataSource. En resumen, ObjectDataSource intentará crear una instancia de los parámetros de entrada. Si se espera que el método BLL un `ProductsRow` instancia, ObjectDataSource intentará crear uno, pero producirá un error debido a la falta de un constructor sin parámetros predeterminado. Para obtener más información sobre este problema, consulte las siguientes dos entradas de foros de ASP.NET: [ObjectDataSources actualizar con conjuntos de datos de Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), y [problema con ObjectDataSource y conjunto de datos de Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

Después, tanto en `AddProduct` y `UpdateProduct`, el código crea un `ProductsRow` de instancia y la rellena con los valores que pasa simplemente en. Al asignar valores a los objetos DataColumn de una fila de datos pueden producirse varias comprobaciones de validación de nivel de campo. Por lo tanto, pasar manualmente los valores pasados en una fila de datos le ayuda a garantizar la validez de los datos que se pasan al método BLL. Lamentablemente las clases de DataRow fuertemente tipado generadas por Visual Studio no utilizan tipos que aceptan valores NULL. En su lugar, para indicar que un objeto DataColumn determinado en un objeto DataRow debe corresponder a un `NULL` valor debemos usar la base de datos la `SetColumnNameNull()` método.

En `UpdateProduct` cargamos en primer lugar en el producto que desea actualizar con `GetProductByProductID(productID)`. Aunque puede parecer un viaje innecesario a la base de datos, este viaje adicional demostrará merece la pena en tutoriales futuros que exploran la simultaneidad optimista. Simultaneidad optimista es una técnica para asegurarse de que dos usuarios que trabajan simultáneamente en los mismos datos no sobrescriban accidentalmente cambios de los demás. Arrastrar todo el registro también resulta más fácil crear métodos de actualización en la capa BLL que modifican solo un subconjunto de columnas de DataRow. Cuando se explora la `SuppliersBLL` , veremos un ejemplo de este tipo de clase.

Por último, tenga en cuenta que la `ProductsBLL` clase tiene la [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) aplicado (la `[System.ComponentModel.DataObject]` sintaxis inmediatamente anterior a la instrucción class en la parte superior del archivo) y los métodos tienen [ Atributos de DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). El `DataObject` atributo marca la clase como un objeto adecuado para enlazarlo a un [control ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), mientras que la `DataObjectMethodAttribute` indica el propósito del método. Como en el futuro, veremos tutoriales, ObjectDataSource de ASP.NET 2.0 facilita la forma declarativa tener acceso a datos de una clase. Con el fin de filtrar la lista de posibles clases para enlazar en el Asistente de ObjectDataSource, de forma predeterminada sólo esas clases marcadas como `DataObjects` se muestran en la lista desplegable del asistente. La `ProductsBLL` clase funcionará igual de bien sin estos atributos, pero agrega resulta más fácil trabajar en el Asistente de ObjectDataSource.

## <a name="adding-the-other-classes"></a>Adición de las otras clases

Con el `ProductsBLL` clase completa, todavía tenemos que agregar las clases para trabajar con categorías, proveedores y empleados. Dedique unos minutos para crear las siguientes clases y métodos con los conceptos descritos en el ejemplo anterior:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

El método debe tener en cuenta es el `SuppliersBLL` la clase `UpdateSupplierAddress` método. Este método proporciona una interfaz para actualizar solo la información de dirección del proveedor. Internamente, este método lee de la `SupplierDataRow` objeto para el elemento especificado `supplierID` (mediante `GetSupplierBySupplierID`), establece sus propiedades relacionadas con la dirección y, a continuación, llama a hacia abajo en la `SupplierDataTable`del `Update` método. El `UpdateSupplierAddress` método se indica a continuación:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Hacer referencia a la descarga de este artículo para mi implementación completa de las clases BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Paso 2: Obtener acceso a los conjuntos de datos con tipo a través de las clases BLL

En el primer tutorial hemos visto algunos ejemplos de trabajar directamente con el conjunto de datos con tipo mediante programación, pero con la adición de nuestras clases BLL, el nivel de presentación debe funcionar con la capa BLL en su lugar. En el `AllProducts.aspx` ejemplo desde el primer tutorial, el `ProductsTableAdapter` se usa para enlazar la lista de productos para un control GridView, tal como se muestra en el código siguiente:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Para usar el nuevo BLL clases, todo lo que debe cambiarse es la primera línea de código simplemente reemplace el `ProductsTableAdapter` objeto con un `ProductBLL` objeto:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Las clases BLL también son accesibles mediante declaración (como el conjunto de datos con tipo) mediante el uso de ObjectDataSource. Se explicará el ObjectDataSource con más detalle en los siguientes tutoriales.


[![Se muestra la lista de productos en un control GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Figura 3**: se muestra la lista de productos en un GridView ([haga clic aquí para ver la imagen a tamaño completo](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Paso 3: Agregar validación de nivel de campo a las clases de DataRow

Validación de nivel de campo son los cheques que pertenece a los valores de propiedad de los objetos de negocio al insertar o actualizar. Algunas reglas de validación de nivel de campo para los productos incluyen:

- El `ProductName` campo debe ser una longitud máxima de 40 caracteres
- El `QuantityPerUnit` campo debe ser una longitud máxima de 20 caracteres
- El `ProductID`, `ProductName`, y `Discontinued` campos son obligatorios, pero todos los demás campos son opcionales
- El `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` campos deben ser mayor o igual a cero.

Estas reglas pueden y se deben expresar en el nivel de base de datos. El límite de caracteres en el `ProductName` y `QuantityPerUnit` campos se capturan en los tipos de datos de las columnas de la `Products` tabla (`nvarchar(40)` y `nvarchar(20)`, respectivamente). Si los campos son obligatorios y opcionales se expresan por si la columna de tabla de base de datos permite `NULL` s. Cuatro [restricciones check](https://msdn.microsoft.com/library/ms188258.aspx) existe que asegurarse de que solo los valores iguales a cero o mayores que pueden realizar en el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` columnas.

Además de aplicar estas reglas en la base de datos debe aplicarse también en el nivel de conjunto de datos. De hecho, conjunto de cada DataTable de objetos DataColumn ya capturan la longitud de campo y si un valor es obligatorio u opcional. Para ver la validación de nivel de campo existente proporciona automáticamente, vaya al diseñador de DataSet, seleccione un campo de una de las tablas de datos y, a continuación, vaya a la ventana Propiedades. Como se muestra en la figura 4, el `QuantityPerUnit` DataColumn en el `ProductsDataTable` tiene una longitud máxima de 20 caracteres y admite `NULL` valores. Si se intenta establecer el `ProductsDataRow`del `QuantityPerUnit` propiedad con un valor de cadena más de 20 caracteres un `ArgumentException` se iniciará.


[![DataColumn proporciona validación básica de nivel de campo](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Figura 4**: el objeto DataColumn proporciona nivel de campo una validación básica ([haga clic aquí para ver la imagen a tamaño completo](creating-a-business-logic-layer-vb/_static/image8.png))


Por desgracia, no podemos especificamos comprobaciones de los límites, como el `UnitPrice` valor debe ser mayor que o igual a cero, a través de la ventana Propiedades. Para proporcionar este tipo de validación de nivel de campo es necesario crear un controlador de eventos para la DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) eventos. Como se mencionó en la [tutorial anterior](creating-a-data-access-layer-vb.md), los objetos DataSet, DataTable y DataRow creados por el conjunto de datos con tipo se pueden extender mediante el uso de clases parciales. Con esta técnica podemos crear un `ColumnChanging` controlador de eventos para el `ProductsDataTable` clase. Empiece por crear una clase en el `App_Code` carpeta denominada `ProductsDataTable.ColumnChanging.vb`.


[![Agregue una nueva clase a la carpeta App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Figura 5**: agregar una nueva clase para la `App_Code` carpeta ([haga clic aquí para ver la imagen a tamaño completo](creating-a-business-logic-layer-vb/_static/image11.png))


A continuación, cree un controlador de eventos para el `ColumnChanging` eventos que garantiza que la `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` valores de columna (si no `NULL`) son mayores o iguales a cero. Si cualquier columna de este tipo está fuera del intervalo, inicie un `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Paso 4: Agregar reglas de negocios a las clases de la capa BLL

Además de la validación de nivel de campo, puede haber reglas de negocios alto nivel que implican distintas entidades o conceptos no se puede expresar en el nivel de columna única, como:

- Si un producto ya no está disponible, su `UnitPrice` no se puede actualizar
- País de un empleado de residencia debe coincidir con el país de su administrador de residencia
- Un producto no se suspenderá si es el único producto proporcionado por el proveedor

Las clases BLL deben contener comprobaciones para garantizar su conformidad con las reglas de negocios de la aplicación. Estas comprobaciones pueden agregarse directamente a los métodos a los que aplicará.

Imagine que nuestro reglas de negocios dictan que un producto no se pudieron marcar discontinuo si era el único producto de un proveedor determinado. Es decir, si producto *X* fue el producto solo se adquirió de proveedor *Y*, no se pudo marcar *X* como discontinuo; si, sin embargo, proveedor *Y*nos suministrados con tres productos *A*, *B*, y *C*, a continuación, se puede marcar cualquier y todos ellos como desparecido. Una regla de negocios impar, pero las reglas de negocios y el sentido común siempre no están alineadas.

Para aplicar esta regla de negocios en el `UpdateProducts` método comenzamos comprobando si `Discontinued` se estableció en `True` y, si es así, se llamaría a `GetProductsBySupplierID` para determinar cuántos productos se adquirió de proveedor de este producto. Si solo se adquiere un producto de este proveedor, iniciamos una `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Responder a errores de validación en el nivel de presentación

Al llamar a la capa BLL desde el nivel de presentación se puede decidir si se intenta controlar las excepciones que puedan generarse o de permitir que ascienden a ASP.NET (que se producirá la `HttpApplication`del `Error` evento). Para controlar una excepción cuando se trabaja con la capa BLL mediante programación, podemos usar un [intente... Detectar](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) bloque, como se muestra en el ejemplo siguiente:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Tal y como veremos en tutoriales futuros, controlar las excepciones que surjan de BLL al usar datos de un control Web para insertar, actualizar, o eliminar los datos puede controlarse directamente en un controlador de eventos en lugar de tener que ajustar el código en `Try...Catch` bloques.

## <a name="summary"></a>Resumen

Una aplicación bien diseñada está diseñada en niveles distintos, cada uno de los cuales encapsula una función concreta. En el primer tutorial de esta serie de artículos, creamos una capa de acceso a datos mediante conjuntos de datos con tipo; en este tutorial se compila una capa de lógica empresarial como una serie de clases en nuestra aplicación `App_Code` carpeta que llama a nuestro DAL. La capa BLL implementa la lógica de nivel de campo y de nivel de negocio para nuestra aplicación. Además de crear un BLL independiente, como hicimos en este tutorial, otra opción es ampliar los métodos de los TableAdapters mediante el uso de clases parciales. Sin embargo, con esta técnica no nos permiten invalidar los métodos existentes ni lo separar nuestro DAL y nuestro BLL más limpio el enfoque que hemos llevado en este artículo.

Con la capa DAL y BLL completa, se está listos para iniciar en el nivel de presentación. En el [siguiente tutorial](master-pages-and-site-navigation-vb.md) comenzaremos tomar un desvío breve de los temas de acceso a datos y definir un diseño de página coherente para su uso a lo largo de los tutoriales.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Liz Shulok, Dennis Patterson, Carlos Santos y Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-vb.md)
> [Siguiente](master-pages-and-site-navigation-vb.md)
