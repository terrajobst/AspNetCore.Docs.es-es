---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Crear una capa de lógica empresarial (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo centralizar las reglas de negocio en una capa de lógica empresarial (BLL) que actúa como intermediario para intercambiar datos entre t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 06799205ca180ca504083a6e2e99faceb79b22fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375320"
---
<a name="creating-a-business-logic-layer-vb"></a>Crear una capa de lógica empresarial (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) o [descargar PDF](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> En este tutorial, veremos cómo centralizar las reglas de negocio en una capa de lógica empresarial (BLL) que actúa como intermediario para intercambiar datos entre la capa de presentación y la capa DAL.


## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) creado en el [primer tutorial](creating-a-data-access-layer-vb.md) separa claramente los datos de acceso a la lógica de la lógica de presentación. Sin embargo, mientras que la capa DAL separa claramente los detalles de acceso a datos de la capa de presentación, no aplica reglas de negocio que se pueden aplicar. Por ejemplo, para nuestra aplicación nos conviene no permitir la `CategoryID` o `SupplierID` campos de la `Products` tabla que se puede modificar cuando la `Discontinued` campo se establece en 1, o que podemos desear aplicar reglas de antigüedad, lo que prohíbe situaciones en las que un empleado está administrado por alguien que ha sido contratado tras ellas. Otro escenario común es autorización quizás solo los usuarios de un rol determinado, pueden eliminar los productos o pueden cambiar el `UnitPrice` valor.

En este tutorial, veremos cómo centralizar estas reglas de negocios en una capa de lógica empresarial (BLL) que actúa como intermediario para intercambiar datos entre la capa de presentación y la capa DAL. En una aplicación real, la capa BLL debe implementarse como un proyecto de biblioteca de clases independiente; Sin embargo, para estos tutoriales implementaremos el nivel de lógica empresarial como una serie de clases en nuestra `App_Code` carpeta con el fin de simplificar la estructura del proyecto. Figura 1 ilustra las relaciones arquitectura entre la capa de presentación, BLL y DAL.


![La capa BLL separa la capa de presentación de la capa de acceso a datos e impone las reglas de negocios](creating-a-business-logic-layer-vb/_static/image1.png)

**Figura 1**: BLL separa la capa de presentación de la capa de acceso a datos e impone las reglas de negocios


En lugar de crear clases independientes para implementar nuestra [lógica de negocios](http://en.wikipedia.org/wiki/Business_logic), o bien, podríamos colocar esta lógica directamente en el conjunto de datos con tipo con clases parciales. Para obtener un ejemplo de creación y extensión de un conjunto de datos con tipo, consulte el primer tutorial.

## <a name="step-1-creating-the-bll-classes"></a>Paso 1: Creación de clases de BLL

Nuestro nivel de lógica empresarial se compone de cuatro clases, una para cada TableAdapter en la capa DAL; cada una de estas clases BLL tendrá métodos para recuperar, insertar, actualizar y eliminar del TableAdapter respectivo en la capa DAL, aplicando las reglas de negocios adecuados.

Para más claramente separadas las clases relacionadas con DAL y BLL, vamos a crear dos subcarpetas en la `App_Code` carpeta, `DAL` y `BLL`. Simplemente haga doble clic en el `App_Code` carpeta en el Explorador de soluciones y elija la nueva carpeta. Después de crear estas dos carpetas, mover el conjunto de datos con tipo creado en el primer tutorial en el `DAL` subcarpeta.

A continuación, cree los cuatro archivos de clase BLL en los `BLL` subcarpeta. Para ello, haga doble clic en el `BLL` subcarpeta, elija Agregar un nuevo elemento y elija la plantilla de clase. Nombre de las cuatro clases `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, y `EmployeesBLL`.


![Agregar cuatro nuevas clases a la carpeta App_Code](creating-a-business-logic-layer-vb/_static/image2.png)

**Figura 2**: agregar cuatro nuevas clases a la `App_Code` carpeta


A continuación, vamos a agregar métodos a cada una de las clases que encapsulan sencillamente los métodos definidos para los TableAdapters desde el primer tutorial. Por ahora, estos métodos solo llamará directamente en la capa DAL; Volveremos más adelante para agregar cualquier lógica de negocios necesaria.

> [!NOTE]
> Si usa Visual Studio Standard Edition o versiones posteriores (es decir, está *no* mediante Visual Web Developer), opcionalmente, puede diseñar sus clases visualmente con la [Diseñador de clases](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Hacer referencia a la [Blog del Diseñador de clase](https://blogs.msdn.com/classdesigner/default.aspx) para obtener más información sobre esta nueva característica en Visual Studio.


Para el `ProductsBLL` que necesitamos agregar un total de siete métodos de clase:

- `GetProducts()` Devuelve todos los productos
- `GetProductByProductID(productID)` Devuelve el producto con el identificador de producto especificado
- `GetProductsByCategoryID(categoryID)` Devuelve todos los productos de la categoría especificada
- `GetProductsBySupplier(supplierID)` Devuelve todos los productos del proveedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Inserta un nuevo producto en la base de datos con los valores pasados de; Devuelve el `ProductID` valor del registro recién insertado
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` actualiza un producto existente en la base de datos con los valores en el pasado; Devuelve `True` si se ha actualizado con precisión una fila, `False` en caso contrario
- `DeleteProduct(productID)` Elimina el producto especificado de la base de datos

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Los métodos que simplemente devuelven datos `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, y `GetProductBySuppliersID` son bastante sencillas, ya que simplemente llaman a hacia abajo en la capa DAL. Aunque en algunos escenarios puede ser reglas de negocio que deben implementarse en este nivel (por ejemplo, las reglas de autorización basada en el usuario ha iniciado sesión actualmente o el rol al que pertenece el usuario), simplemente dejaremos estos métodos como-es. Para estos métodos, a continuación, BLL sirve únicamente como un proxy a través del cual la capa de presentación, tiene acceso a los datos subyacentes de la capa de acceso a datos.

El `AddProduct` y `UpdateProduct` métodos tanto toman como parámetros de los valores para los distintos campos de producto y agregar un nuevo producto o actualiza uno existente, respectivamente. Dado que muchas de las `Product` pueden aceptar las columnas de tabla `NULL` valores (`CategoryID`, `SupplierID`, y `UnitPrice`, por nombrar algunos), los parámetros de entrada `AddProduct` y `UpdateProduct` que se asignan a este uso de las columnas [tipos que aceptan valores NULL](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Tipos que aceptan valores NULL son nuevos en .NET 2.0 y proporcionan una técnica que indica si un tipo de valor se debe, en su lugar, ser `Nothing`. Hacer referencia a la [Paul Vick](http://www.panopticoncentral.net/)de entrada de blog [la verdad acerca de tipos que aceptan valores NULL y VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) y la documentación técnica de la [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) estructura para obtener más información.

Los tres métodos devuelven un valor booleano que indica si una fila se ha insertado, actualizada o eliminado no puede dar lugar a la operación en una fila afectada. Por ejemplo, si el desarrollador de páginas llama `DeleteProduct` pasando un `ProductID` para un producto que no existe, el `DELETE` instrucción emitida a la base de datos tendrán ningún efecto y, por tanto, el `DeleteProduct` método devolverá `False`.

Tenga en cuenta que al agregar un nuevo producto o actualizar uno existente que tomamos los valores de campo nuevos o modificados del producto como una lista de valores escalares en lugar de aceptar un `ProductsRow` instancia. Se ha elegido este enfoque porque la `ProductsRow` clase se deriva de la versión de ADO.NET `DataRow` (clase), que no tiene un constructor sin parámetros predeterminado. Para crear un nuevo `ProductsRow` instancia, debemos crear primero un `ProductsDataTable` de instancia y, a continuación, invocar su `NewProductRow()` método (lo que hacemos en `AddProduct`). Este inconveniente rears la cabeza cuando nos concentramos en la inserción y actualización de productos a través de ObjectDataSource. En resumen, el origen ObjectDataSource intentará crear una instancia de los parámetros de entrada. Si el método BLL espera un `ProductsRow` instancia, el origen ObjectDataSource intentará crear uno, pero se producirá un error debido a la falta de un constructor sin parámetros predeterminado. Para obtener más información sobre este problema, consulte las siguientes dos entradas de foros de ASP.NET: [actualizar ObjectDataSource con conjuntos de datos Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), y [problema con ObjectDataSource y conjunto de datos Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

A continuación, en ambos `AddProduct` y `UpdateProduct`, el código crea un `ProductsRow` de instancia y la rellena con los valores pasados simplemente. Al asignar valores a los objetos DataColumn de una fila de datos pueden ocurrir varias comprobaciones de validación de nivel de campo. Por lo tanto, pasar manualmente los valores pasados en una fila de datos le ayuda a garantizar la validez de los datos pasados al método BLL. Por desgracia las clases de DataRow fuertemente tipado generadas por Visual Studio no utilizan tipos que aceptan valores NULL. En su lugar, para indicar que un objeto DataColumn determinado en un objeto DataRow debe corresponder a un `NULL` debemos usar el valor de la base de datos la `SetColumnNameNull()` método.

En `UpdateProduct` cargamos en primer lugar en el producto se actualiza con `GetProductByProductID(productID)`. Aunque esto puede parecer visitas innecesarias a la base de datos, este viaje adicional demostrará valga la pena en próximos tutoriales que exploran la simultaneidad optimista. Simultaneidad optimista es una técnica para asegurarse de que dos usuarios que trabajan simultáneamente en los mismos datos no sobrescriban accidentalmente los cambios del otro. Tomar todo el registro también resulta más fácil crear métodos de actualización en la capa BLL que modifique solo un subconjunto de columnas de DataRow. Cuando se explorará el `SuppliersBLL` veremos un ejemplo de clase.

Por último, tenga en cuenta que el `ProductsBLL` clase tiene el [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) aplicado a él (el `[System.ComponentModel.DataObject]` sintaxis justo antes de la instrucción de clase en la parte superior del archivo) y los métodos tienen [ Atributos DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). El `DataObject` atributo marca la clase como un objeto adecuado para enlazarlo a un [control ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), mientras que el `DataObjectMethodAttribute` indica el propósito del método. Como veremos en el futuro tutoriales, ObjectDataSource de ASP.NET 2.0 facilita la forma declarativa tener acceso a datos desde una clase. Para ayudar a filtrar la lista de las clases posibles que enlazar en el Asistente de ObjectDataSource, de forma predeterminada solo esas clases marcadas como `DataObjects` se muestran en la lista desplegable del asistente. La `ProductsBLL` clase funcionará perfectamente sin estos atributos, pero agregarlos resulta más fácil trabajar con Asistente de ObjectDataSource.

## <a name="adding-the-other-classes"></a>Adición de las otras clases

Con el `ProductsBLL` clase completa, todavía tenemos que agregar las clases para trabajar con categorías, proveedores y empleados. Dedique unos minutos para crear las siguientes clases y métodos mediante los conceptos del ejemplo anterior:

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

El método que vale la pena mencionar es el `SuppliersBLL` la clase `UpdateSupplierAddress` método. Este método proporciona una interfaz para actualizar solo la información de dirección del proveedor. Internamente, este método lee la `SupplierDataRow` objeto para el elemento especificado `supplierID` (mediante `GetSupplierBySupplierID`), establece sus propiedades relacionadas con la dirección y, a continuación, llama a hacia abajo en la `SupplierDataTable`del `Update` método. El `UpdateSupplierAddress` método se indica a continuación:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Consulte la descarga de este artículo para mi implementación completa de las clases BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Paso 2: Obtener acceso a los conjuntos de datos con tipo a través de las clases BLL

En el primer tutorial hemos visto algunos ejemplos de trabajar directamente con el conjunto de datos con tipo mediante programación, pero con la adición de nuestras clases BLL, el nivel de presentación debe funcionar en el nivel de lógica empresarial en su lugar. En el `AllProducts.aspx` ejemplo desde el primer tutorial, el `ProductsTableAdapter` se usó para enlazar la lista de productos en un control GridView, tal como se muestra en el código siguiente:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Para usar la capa BLL nuevas clases, todo lo que debe cambiarse es simplemente reemplace la primera línea de código la `ProductsTableAdapter` objeto con un `ProductBLL` objeto:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Las clases de nivel de lógica empresarial también pueden obtenerse mediante declaración (como el conjunto de datos con tipo) mediante el uso de ObjectDataSource. Trataremos ObjectDataSource en detalle en los tutoriales siguientes.


[![Se muestra la lista de productos en un control GridView](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Figura 3**: se muestra la lista de productos en un control GridView ([haga clic aquí para ver imagen en tamaño completo](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Paso 3: Agregar validación de nivel de campo a las clases de DataRow

Validación de nivel de campo son los cheques que pertenece a los valores de propiedad de los objetos de negocio al insertar o actualizar. Algunas reglas de validación de nivel de campo para los productos incluyen:

- El `ProductName` campo debe ser la longitud máxima de 40 caracteres
- El `QuantityPerUnit` campo debe ser la longitud máxima de 20 caracteres
- El `ProductID`, `ProductName`, y `Discontinued` campos son obligatorios, pero todos los demás campos son opcionales
- El `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` campos deben ser mayor o igual a cero.

Estas reglas pueden y deben expresarse en el nivel de base de datos. El límite de caracteres en el `ProductName` y `QuantityPerUnit` se capturan los campos por los tipos de datos de esas columnas en el `Products` tabla (`nvarchar(40)` y `nvarchar(20)`, respectivamente). Si los campos son obligatorios y opcionales se expresan por si la columna de tabla de base de datos permite `NULL` s. Cuatro [restricciones check](https://msdn.microsoft.com/library/ms188258.aspx) existe que asegurarse de que solo los valores iguales a cero o mayores que pueden realizar en el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` columnas.

Además de aplicar estas reglas en la base de datos también se deben aplicar en el nivel de conjunto de datos. De hecho, la longitud de campo y si un valor es obligatorio u opcional ya se capturan para el conjunto de cada DataTable de DataColumn. Para ver la validación de nivel de campo existente proporciona automáticamente, vaya al diseñador de DataSet, seleccione un campo de una de las tablas de datos y, a continuación, vaya a la ventana Propiedades. Como se muestra en la figura 4, el `QuantityPerUnit` DataColumn en el `ProductsDataTable` tiene una longitud máxima de 20 caracteres y permitir `NULL` valores. Si se intenta establecer el `ProductsDataRow`del `QuantityPerUnit` propiedad con un valor de cadena de más de 20 caracteres un `ArgumentException` se iniciará.


[![DataColumn proporciona validación básica de nivel de campo](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Figura 4**: la DataColumn proporciona nivel de campo de validación básicas ([haga clic aquí para ver imagen en tamaño completo](creating-a-business-logic-layer-vb/_static/image8.png))


Lamentablemente, no podemos especificamos las comprobaciones de límites, como el `UnitPrice` valor debe ser mayor o igual a cero, a través de la ventana Propiedades. Para proporcionar este tipo de validación de nivel de campo es necesario crear un controlador de eventos de la DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) eventos. Como se mencionó en el [tutorial anterior](creating-a-data-access-layer-vb.md), los objetos DataRow, DataTable y DataSet creados por el conjunto de datos con tipo se pueden extender mediante el uso de clases parciales. Con esta técnica se puede crear un `ColumnChanging` controlador de eventos para el `ProductsDataTable` clase. Empiece por crear una clase en el `App_Code` carpeta denominada `ProductsDataTable.ColumnChanging.vb`.


[![Agregue una nueva clase a la carpeta App_Code](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Figura 5**: agregue una nueva clase a la `App_Code` carpeta ([haga clic aquí para ver imagen en tamaño completo](creating-a-business-logic-layer-vb/_static/image11.png))


A continuación, cree un controlador de eventos para el `ColumnChanging` eventos que se asegura de que el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` valores de columna (si no `NULL`) son mayores o iguales a cero. Si cualquier columna de este tipo está fuera del intervalo, se produce un `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Paso 4: Agregar reglas de negocios personalizada a las clases del nivel de lógica empresarial

Además de la validación de nivel de campo, puede haber reglas de alto nivel de negocios personalizada que implican entidades diferentes o no se puede expresar conceptos en el nivel de columna única, como:

- Si un producto no está disponible, su `UnitPrice` no se puede actualizar
- País de un empleado de residencia debe coincidir con el país de su administrador de residencia
- Un producto no puede interrumpirse si es el único producto proporcionado por el proveedor

Las clases BLL deben contener comprobaciones para garantizar el cumplimiento de las reglas de negocios de la aplicación. Estas comprobaciones pueden agregarse directamente a los métodos a los que se aplican.

Imagine que nuestras reglas de negocio dictan que un producto no se pudo marcar discontinuo si era el único producto de un proveedor determinado. Es decir, si producto *X* era el único producto comprado a proveedor *Y*, no nos pudimos marcar *X* como discontinuo; si, sin embargo, proveedor *Y*nos suministrados con tres productos *A*, *B*, y *C*, a continuación, nos podríamos marcar cualquier y todos ellos como obsoletos. Una regla de negocio extraño, pero las reglas de negocios y sentido común no están alineadas siempre!

Para aplicar esta regla de negocios en el `UpdateProducts` comenzamos comprobando si el método `Discontinued` se estableció en `True` y, si es así, se llamaría a `GetProductsBySupplierID` para determinar cuántos productos se adquiridas a través de proveedores de este producto. Si solo se adquiere un producto de este proveedor, generaremos una `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Responder a errores de validación en el nivel de presentación

Al llamar a la capa BLL desde el nivel de presentación se puede decidir si se intenta controlar las excepciones que puedan generarse o bien permitirles ascienden a ASP.NET (que se producirá la `HttpApplication`del `Error` eventos). Para controlar una excepción cuando se trabaja mediante programación con la capa BLL, podemos usar un [intente... Detectar](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) bloque, como se muestra en el ejemplo siguiente:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Como veremos en tutoriales futuros, controlar las excepciones que surjan BLL al usar los datos de un control Web para insertar, actualizar, o eliminar los datos puede controlarse directamente en un controlador de eventos en lugar de tener que ajustar el código en `Try...Catch` bloques.

## <a name="summary"></a>Resumen

Una aplicación con buena arquitectura se cimienta en distintas capas, cada una de las cuales encapsula una función concreta. En el primer tutorial de esta serie de artículos, creamos una capa de acceso a datos mediante conjuntos de datos con tipo; en este tutorial hemos creado una capa de lógica empresarial como una serie de clases en nuestra aplicación `App_Code` carpeta que llama a nuestro DAL. La capa BLL implementa la lógica de nivel de campo y el nivel de negocio para nuestra aplicación. Además de crear un nivel de lógica empresarial independiente, como hicimos en este tutorial, otra opción es ampliar los métodos de los TableAdapters mediante el uso de clases parciales. Sin embargo, esta técnica no nos permite invalidar los métodos existentes ni lo separar nuestro DAL y nuestro BLL lo más limpiamente el enfoque que hemos tomado en este artículo.

Con la DAL y BLL completa, ya estamos listos para iniciar en la capa de presentación. En el [siguiente tutorial](master-pages-and-site-navigation-vb.md) echaremos un breve desvío de temas de acceso de datos y definir un diseño de página coherente para su uso a lo largo de los tutoriales.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok, Dennis Patterson, Carlos Santos y Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-vb.md)
> [Siguiente](master-pages-and-site-navigation-vb.md)
