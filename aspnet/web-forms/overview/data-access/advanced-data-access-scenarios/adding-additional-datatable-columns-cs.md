---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Agregar columnas adicionales de DataTable (C#) | Microsoft Docs
author: rick-anderson
description: Al usar al Asistente de TableAdapter para crear un conjunto de datos con tipo, la correspondiente DataTable contiene las columnas devueltas por la consulta de base de datos principal. Sin embargo, existen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 373ba6d3ec86ea2ab26bfecf39647776694960de
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393988"
---
<a name="adding-additional-datatable-columns-c"></a>Agregar columnas adicionales de DataTable (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) o [descargar PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Al usar al Asistente de TableAdapter para crear un conjunto de datos con tipo, la correspondiente DataTable contiene las columnas devueltas por la consulta de base de datos principal. Pero hay ocasiones cuando la tabla de datos debe incluir columnas adicionales. En este tutorial se aprenderá por qué se recomiendan los procedimientos almacenados cuando sea necesario columnas adicionales de DataTable.


## <a name="introduction"></a>Introducción

Al agregar un TableAdapter a un conjunto de datos con tipo, el esquema de s correspondiente DataTable viene determinada por la consulta principal de TableAdapter s. Por ejemplo, si la consulta principal devuelve los campos de datos *A*, *B*, y *C*, la DataTable tendrá tres columnas correspondientes denominadas *A*, *B*, y *C*. Además de la consulta principal, un TableAdapter puede incluir consultas adicionales que devuelven, quizás, un subconjunto de los datos en función de algún parámetro. Por ejemplo, en además con el `ProductsTableAdapter` consulta principal s, que devuelve información acerca de todos los productos, también contiene métodos como `GetProductsByCategoryID(categoryID)` y `GetProductByProductID(productID)`, que devuelve información de producto específico en función de un parámetro proporcionado.

El modelo de tener el esquema de DataTable s reflejan la consulta principal de TableAdapter s funciona bien si todos los métodos de TableAdapter s devuelven el mismo o menos campos de datos de aquéllos especificados en la consulta principal. Si necesita un método del TableAdapter devolver los campos de datos adicionales, a continuación, nos debemos expanda el esquema de DataTable s en consecuencia. En el [maestro/detalle con una lista con viñetas de registros maestros con un control DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial se ha agregado un método para el `CategoriesTableAdapter` que devuelve el `CategoryID`, `CategoryName`, y `Description` definidos en los campos de datos la consulta principal más `NumberOfProducts`, un campo de datos adicionales que notifica el número de productos asociados con cada categoría. Se agrega manualmente una nueva columna a la `CategoriesDataTable` con el fin de capturar la `NumberOfProducts` valor de este nuevo método de campo de datos.

Como se describe en el [cargar archivos](../working-with-binary-files/uploading-files-cs.md) excelente, tutorial debe tener cuidado con los TableAdapters que utilizan instrucciones SQL ad hoc y tienen métodos cuyos campos de datos no coincide exactamente con la consulta principal. Si ya se vuelve a ejecutar el Asistente para configuración de TableAdapter, actualizará todos los métodos de TableAdapter s para que su lista de campos de datos coincida con la consulta principal. Por lo tanto, cualquier método con listas de columnas personalizadas se revertir a la lista de columnas de la consulta principal s y no devolverá los datos esperados. Este problema no ocurre cuando se utilizan procedimientos almacenados.

En este tutorial veremos cómo ampliar un esquema de DataTable s para incluir columnas adicionales. Debido a la fragilidad del TableAdapter al usar instrucciones SQL ad hoc, en este tutorial usaremos los procedimientos almacenados. Hacer referencia a la [crear procedimientos almacenados nuevo para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) y [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) tutoriales para obtener más información sobre configuración de un TableAdapter para usar los procedimientos almacenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Paso 1: Agregar un`PriceQuartile`columna a la`ProductsDataTable`

En el *crear procedimientos almacenados nuevo para la s de conjunto de datos con tipo TableAdapters* tutorial hemos creado un conjunto de datos con tipo denominado `NorthwindWithSprocs`. Actualmente, este conjunto de datos contiene dos tablas de datos: `ProductsDataTable` y `EmployeesDataTable`. El `ProductsTableAdapter` tiene los tres métodos siguientes:

- `GetProducts` -la consulta principal, que devuelve todos los registros de la `Products` tabla
- `GetProductsByCategoryID(categoryID)` -Devuelve todos los productos con los valores especificados *categoryID*.
- `GetProductByProductID(productID)` -Devuelve el producto específico con los valores especificados *productID*.

La consulta principal y los dos métodos adicionales devuelven el mismo conjunto de campos de datos, es decir, todas las columnas de la `Products` tabla. No hay ningún subconsultas correlacionadas o `JOIN` s extracción de datos relacionados desde el `Categories` o `Suppliers` tablas. Por lo tanto, el `ProductsDataTable` tiene una columna correspondiente para cada campo en el `Products` tabla.

Para este tutorial, s permiten agregar un método para el `ProductsTableAdapter` denominado `GetProductsWithPriceQuartile` que devuelve todos los productos. Además de los campos de datos estándar del producto, `GetProductsWithPriceQuartile` también incluirá un `PriceQuartile` campo de datos que se indica en qué cuartil cae el precio del producto s. Por ejemplo, aquellos productos cuyos precios se encuentran en el 25% más costoso tendrá un `PriceQuartile` valor 1, mientras que aquellos cuyos precios se encuentran en la parte inferior 25% tendrá un valor de 4. Antes de que preocuparse por crear el procedimiento almacenado para devolver esta información, sin embargo, primero es necesario actualizar el `ProductsDataTable` para incluir una columna que contenga el `PriceQuartile` resultados cuando el `GetProductsWithPriceQuartile` se usa el método.

Abra el `NorthwindWithSprocs` conjunto de datos y haga doble clic en el `ProductsDataTable`. Elija Agregar en el menú contextual y, a continuación, seleccione la columna.


[![Agregar una nueva columna a la ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: agregar una nueva columna a la `ProductsDataTable` ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image3.png))


Esto agregará una nueva columna a la DataTable denominada Column1 de tipo `System.String`. Es necesario actualizar el nombre de la columna s PriceQuartile y su tipo a `System.Int32` desde que se usará para contener un número entre 1 y 4. Seleccione la columna recién agregada en el `ProductsDataTable` y, en la ventana Propiedades, establezca la `Name` propiedad PriceQuartile y el `DataType` propiedad `System.Int32`.


[![Establecer las propiedades de tipo de datos y el nuevo nombre de columna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: establecer la nueva columna de s `Name` y `DataType` propiedades ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image6.png))


Como se muestra en la figura 2, hay propiedades adicionales que se pueden establecer, por ejemplo, si los valores de la columna deben ser únicos, si la columna es una columna de incremento automático, si la base de datos `NULL` se permiten valores y así sucesivamente. Deje estos valores con sus valores predeterminados.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Paso 2: Crear el`GetProductsWithPriceQuartile`(método)

Ahora que la `ProductsDataTable` se ha actualizado para incluir la `PriceQuartile` columna, estamos preparados para crear el `GetProductsWithPriceQuartile` método. Comience con el botón secundario en TableAdapter y elegir Agregar consulta en el menú contextual. Se abrirá el Asistente de configuración de la consulta de TableAdapter, que primero nos preguntará si desea usar instrucciones SQL ad hoc o un procedimiento almacenado nuevo o existente. Puesto que nos don t aún tiene un procedimiento almacenado que devuelve los datos de precios cuartil, permitir s TableAdapter crear este procedimiento almacenado para nosotros. Seleccione la opción de crear nuevo procedimiento almacenado y haga clic en siguiente.


[![Indique al Asistente de TableAdapter para crear el procedimiento almacenado para nosotros](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: indicar el Asistente de TableAdapter para crear el almacenado procedimiento para nosotros ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image9.png))


En la pantalla posteriores, que se muestra en la figura 4, el Asistente nos pregunta a qué tipo de consulta para agregar. Puesto que la `GetProductsWithPriceQuartile` método devolverá todas las columnas y los registros desde el `Products` tabla, seleccione el LECT que devuelve filas opción y haga clic en siguiente.


[![Nuestra consulta será una instrucción SELECT que devuelve varias filas](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: nuestra consulta será un `SELECT` instrucción que devuelve varias filas ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image12.png))


A continuación se nos pide la `SELECT` consulta. En el asistente, escriba la siguiente consulta:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La consulta anterior utiliza SQL Server 2005 s nuevo [ `NTILE` función](https://msdn.microsoft.com/library/ms175126.aspx) para dividir los resultados en cuatro grupos donde los grupos están determinados por la `UnitPrice` valores ordenados en orden descendente.

Lamentablemente, el generador de consultas no sabe cómo analizar el `OVER` palabra clave y se mostrará un error al analizar la consulta anterior. Por lo tanto, escribir la consulta anterior directamente en el cuadro de texto en el asistente sin usar el generador de consultas.

> [!NOTE]
> Para obtener más información sobre s NTILE y SQL Server 2005 otras funciones de categoría, vea [devolver resultados de rango con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) y [sección de funciones de categoría](https://msdn.microsoft.com/library/ms189798.aspx) desde el [SQL Libros en pantalla de Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).


Después de escribir el `SELECT` consulta y haga clic en siguiente, el Asistente nos pide que proporcione un nombre para el procedimiento almacenado que se va a crear. Nombre del nuevo procedimiento almacenado `Products_SelectWithPriceQuartile` y haga clic en siguiente.


[![Nombre del procedimiento almacenado de Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nombre del procedimiento almacenado `Products_SelectWithPriceQuartile` ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image15.png))


Por último, se nos pide un nombre de los métodos de TableAdapter. Deje ambas rellenar un DataTable y devolver un DataTable casillas de verificación activadas y el nombre de los métodos `FillWithPriceQuartile` y `GetProductsWithPriceQuartile`.


[![Los métodos nombres TableAdapters y haga clic en Finalizar](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: el nombre de los métodos de TableAdapter s y haga clic en Finalizar ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image18.png))


Con el `SELECT` especificado de la consulta y el procedimiento almacenado y los métodos de TableAdapter con nombre, haga clic en Finalizar para completar el asistente. En este momento puede aparecer una advertencia o dos desde el Asistente para la que indica que el `OVER` no se admite la instrucción o construcción de SQL. Se pueden omitir estas advertencias.

Después de completar el asistente, debe incluir el TableAdapter los `FillWithPriceQuartile` y `GetProductsWithPriceQuartile` métodos y la base de datos deben incluir un procedimiento almacenado denominado `Products_SelectWithPriceQuartile`. Dedique un momento para comprobar que el TableAdapter contiene realmente este nuevo método y que el procedimiento almacenado se ha agregado correctamente a la base de datos. Al comprobar la base de datos, si no ve el procedimiento almacenado try con los que el botón secundario en la carpeta procedimientos almacenados y elegir la opción actualizar.


![Compruebe que se ha agregado un nuevo método a TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: Compruebe que se ha agregado un nuevo método a TableAdapter


[![Asegúrese de que la base de datos contiene el Products_SelectWithPriceQuartile procedimiento almacenado](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: asegúrese de que la base de datos que contiene el `Products_SelectWithPriceQuartile` Stored Procedure ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Una de las ventajas de utilizar procedimientos almacenados en lugar de instrucciones SQL ad hoc es que volver a ejecutar al Asistente para configuración de TableAdapter no modificará las listas de columnas de los procedimientos almacenados. Puede comprobarlo mediante el botón secundario en TableAdapter, eligiendo la opción de configurar en el menú contextual para iniciar al asistente y, a continuación, haga clic en Finalizar para completarlo. A continuación, vaya a la base de datos y ver el `Products_SelectWithPriceQuartile` procedimiento almacenado. Tenga en cuenta que no se ha modificado su lista de columnas. Habíamos estado usando instrucciones SQL ad hoc, volver a ejecutar el Asistente para configuración de TableAdapter habría revertir esta lista de columnas de la consulta s para que coincida con la lista de columnas de la consulta principal, y lo quita de la instrucción NTILE desde la consulta usada por el `GetProductsWithPriceQuartile` método.


Cuando la s de la capa de acceso a datos `GetProductsWithPriceQuartile` se invoca el método, lo TableAdapter ejecuta el `Products_SelectWithPriceQuartile` procedimiento almacenado y agrega una fila a la `ProductsDataTable` de cada registro devuelto. Los campos de datos devueltos por el procedimiento almacenado se asignan a la `ProductsDataTable` columnas s. Dado que no hay un `PriceQuartile` campo de datos devuelto por el procedimiento almacenado, su valor se asigna a la `ProductsDataTable` s `PriceQuartile` columna.

Caso de los métodos de TableAdapter cuyas consultas no devuelven un `PriceQuartile` campo de datos, el `PriceQuartile` valor de columna s es el valor especificado por su `DefaultValue` propiedad. Como se muestra en la figura 2, este valor se establece en `DBNull`, el valor predeterminado. Si prefiere un valor predeterminado diferente, basta con establecer la `DefaultValue` propiedad según corresponda. Asegúrese de que el `DefaultValue` valor es válido según la columna s `DataType` (es decir, `System.Int32` para el `PriceQuartile` columna).

En este punto hemos realizado los pasos necesarios para agregar una columna adicional a un objeto DataTable. Para comprobar que esta columna adicional funciona según lo previsto, permiten crear una página ASP.NET que muestra cada nombre del producto, precio y cuartil precio de s. Antes de hacerlo, sin embargo, primero es necesario actualizar la capa de lógica empresarial para que incluya un método que llama hacia abajo a la s DAL `GetProductsWithPriceQuartile` método. Se actualizará la capa BLL a continuación, en el paso 3 y, a continuación, cree la página ASP.NET en el paso 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Paso 3: Aumento de la capa de lógica de negocios

Antes de usar el nuevo `GetProductsWithPriceQuartile` método desde la capa de presentación, primero debemos agregar un método correspondiente a la capa BLL. Abra el `ProductsBLLWithSprocs` archivo de clase y agregue el código siguiente:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Al igual que los otros métodos de recuperación de datos en `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` método simplemente llama a la capa DAL s correspondiente `GetProductsWithPriceQuartile` método y devuelve sus resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Paso 4: Mostrar la información de precios cuartil en una página Web ASP.NET

Con la adición de BLL completar, está listo para crear una página ASP.NET que muestra el cuartil precio de cada producto. Abra el `AddingColumns.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad a `Products`. En la etiqueta inteligente s GridView, enlazarlo a un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para usar el `ProductsBLLWithSprocs` clase s `GetProductsWithPriceQuartile` método. Puesto que se trata de una cuadrícula de solo lectura, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurar el origen ObjectDataSource que se usarán el `ProductsBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperar información de producto desde el método GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperar información del producto desde la `GetProductsWithPriceQuartile` método ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image28.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un BoundField o CampoCasillaVerificación en GridView para cada uno de los campos de datos devueltos por el método. Uno de estos campos de datos es `PriceQuartile`, que es la columna se agrega a la `ProductsDataTable` en el paso 1.

Edite los campos de s GridView, quitando todos excepto el `ProductName`, `UnitPrice`, y `PriceQuartile` BoundFields. Configurar la `UnitPrice` BoundField para dar formato a su valor como moneda y tener la `UnitPrice` y `PriceQuartile` BoundFields y centro-alineado a la derecha, respectivamente. Por último, actualice los restantes BoundFields `HeaderText` propiedades para el producto, precio y precio cuartil, respectivamente. Además, active la casilla Habilitar ordenación de la etiqueta inteligente de s GridView.

Después de estas modificaciones, el marcado declarativo de s GridView y ObjectDataSource debería ser similar al siguiente:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Figura 11 muestra esta página cuando visita a través de un explorador. Tenga en cuenta que, inicialmente, los productos se ordenan por su precio en orden descendente con cada producto asignado un adecuado `PriceQuartile` valor. Por supuesto estos datos pueden ordenarse por otros criterios con el valor de columna precio cuartil todavía que refleja la clasificación de producto s con respecto al precio (consulte la figura 12).


[![Los productos se ordenan por sus precios](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: los productos se ordenan por sus precios ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image31.png))


[![Los productos se ordenan por sus nombres](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: los productos se ordenan por sus nombres ([haga clic aquí para ver imagen en tamaño completo](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Con unas pocas líneas de código nos podríamos aumentar GridView para que lo colorea las filas del producto basándose en sus `PriceQuartile` valor. Nos podríamos color esos productos en el primer cuartil verde claro, los de la segunda cuartil un color amarillo y así sucesivamente. Le sugiero que dedique un momento para agregar esta funcionalidad. Si necesita un actualizador cómo dar formato a un control GridView, consulte el [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un enfoque alternativo - crear otro objeto de TableAdapter

Como hemos visto en este tutorial, al agregar un método a TableAdapter que devuelva los campos de datos distintos de los especificados por la consulta principal, podemos agregar las columnas correspondientes a la DataTable. Este enfoque, sin embargo, funciona bien solo si hay un pequeño número de métodos de TableAdapter que devuelva los campos de datos diferentes, y si esos campos de datos alternativo no varían mucho de la consulta principal.

En lugar de agregar columnas a la DataTable, en su lugar, puede agregar otro TableAdapter al conjunto de datos que contiene los métodos de la primera TableAdapter que devuelven los campos de datos diferentes. Para este tutorial, en lugar de agregar el `PriceQuartile` columna a la `ProductsDataTable` (que solo se usa por la `GetProductsWithPriceQuartile` método), podríamos ha agregado un TableAdapter adicional al conjunto de datos denominado `ProductsWithPriceQuartileTableAdapter` que usa el `Products_SelectWithPriceQuartile` almacenados procedimiento como su consulta principal. Las páginas ASP.NET que es necesario para obtener información de producto con el cuartil precio utilizaría el `ProductsWithPriceQuartileTableAdapter`, mientras que aquellos que no se pueden seguir usando el `ProductsTableAdapter`.

Al agregar un nuevo TableAdapter, DataTables permanecen untarnished y sus columnas reflejan con precisión los campos de datos devueltos por sus métodos de TableAdapter s. Sin embargo, los TableAdapters adicionales puede introducir la funcionalidad y las tareas repetitivas. Por ejemplo, si esas páginas ASP.NET que muestra el `PriceQuartile` columna también es necesario para proporcionar la inserción, actualización y eliminación de soporte técnico, el `ProductsWithPriceQuartileTableAdapter` necesitaría su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades correctamente configurado. Aunque estas propiedades se reflejará el `ProductsTableAdapter` s, esta configuración presenta un paso adicional. Además, ahora hay dos maneras de actualizar, eliminar o agregar un producto a la base de datos - a través de la `ProductsTableAdapter` y `ProductsWithPriceQuartileTableAdapter` clases.

La descarga de este tutorial incluye una `ProductsWithPriceQuartileTableAdapter` clase en el `NorthwindWithSprocs` conjunto de datos que se muestra este enfoque alternativo.

## <a name="summary"></a>Resumen

En la mayoría de los escenarios, todos los métodos en un TableAdapter devolución el mismo conjunto de campos de datos, pero hay ocasiones es posible que necesite un método determinado o dos para devolver un campo adicional. Por ejemplo, en el [maestro/detalle con una lista con viñetas de registros maestros con un control DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial se ha agregado un método para el `CategoriesTableAdapter` que, además de los campos de datos de la consulta principal s devueltos un `NumberOfProducts` campo que notifica el número de productos asociados con cada categoría. En este tutorial explicamos la adición de un método en el `ProductsTableAdapter` que devuelve un `PriceQuartile` campo además de los campos de datos de la consulta principal s. Para capturar datos adicionales campos devueltos por los métodos de TableAdapter s que tenemos que agregar las columnas correspondientes a la DataTable.

Si tiene previsto agregar manualmente las columnas a la DataTable, se recomienda que los TableAdapter utilizan procedimientos almacenados. Si el TableAdapter utiliza instrucciones SQL ad hoc, siempre que el Asistente para configuración de TableAdapter se ejecuta todos los métodos de reversión las listas de campos de datos a los campos de datos devueltos por la consulta principal. Este problema no se extiende a los procedimientos almacenados, que es la razón por la que se recomiendan y se usaron en este tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Randy Schmidt, Goor Jacky, Bernadette Leigh y Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-the-tableadapter-to-use-joins-cs.md)
> [Siguiente](working-with-computed-columns-cs.md)
