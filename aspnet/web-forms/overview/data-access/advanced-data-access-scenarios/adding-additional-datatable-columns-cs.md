---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Agregar columnas adicionales de DataTable (C#) | Documentos de Microsoft
author: rick-anderson
description: Al utilizar al Asistente de TableAdapter para crear un conjunto de datos con tipo, la tabla de datos correspondiente contiene las columnas devueltas por la consulta de base de datos principal. Sin embargo, existen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b1fe8d2e376065aed8d94b1267910bd1f7e5bd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-additional-datatable-columns-c"></a>Agregar columnas adicionales de DataTable (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) o [descarga de PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Al utilizar al Asistente de TableAdapter para crear un conjunto de datos con tipo, la tabla de datos correspondiente contiene las columnas devueltas por la consulta de base de datos principal. Pero hay ocasiones cuando la tabla de datos debe incluir columnas adicionales. En este tutorial se información sobre por qué se recomiendan los procedimientos almacenados cuando necesitemos columnas adicionales de DataTable.


## <a name="introduction"></a>Introducción

Al agregar un TableAdapter a un conjunto de datos con tipo, el esquema de s DataTable correspondiente se determina por la consulta principal de TableAdapter s. Por ejemplo, si la consulta principal devuelve campos de datos *A*, *B*, y *C*, la tabla de datos tendrá tres columnas correspondientes denominadas *A*, *B*, y *C*. Además de su consulta principal, un TableAdapter puede incluir consultas adicionales que devuelven, quizás, un subconjunto de los datos en función de algún parámetro. Por ejemplo, en además con el `ProductsTableAdapter` consulta principal s, que devuelve información acerca de todos los productos, también contiene métodos como `GetProductsByCategoryID(categoryID)` y `GetProductByProductID(productID)`, que devuelven información de producto específico basado en un parámetro proporcionado.

El modelo de tener el esquema de DataTable s reflejan la consulta principal de TableAdapter s funciona bien si todos los métodos de TableAdapter s devuelven los mismos o menos campos de datos de las especificadas en la consulta principal. Si se necesita un método de TableAdapter devolver los campos de datos adicionales, a continuación, que deberíamos ampliamos el esquema de DataTable s según corresponda. En el [principal/detalle con una lista con viñetas de registros maestros DataList detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial hemos agregado un método para el `CategoriesTableAdapter` que devuelve el `CategoryID`, `CategoryName`, y `Description` campos de datos definidos en la consulta principal más `NumberOfProducts`, un campo de datos adicionales que notifica el número de productos asociados con cada categoría. Hemos agregado manualmente una nueva columna a la `CategoriesDataTable` para capturar la `NumberOfProducts` valor de este nuevo método de campo de datos.

Como se describe en el [cargar archivos](../working-with-binary-files/uploading-files-cs.md) tutorial, excelente debe tener cuidado con los TableAdapter utilizan instrucciones SQL ad hoc y tienen métodos cuyos campos de datos no coincide exactamente con la consulta principal. Si ya se vuelve a ejecutar el Asistente para configuración de TableAdapter, se actualizan todos los métodos de TableAdapter s para que su lista de campos de datos coincida con la consulta principal. Por lo tanto, los métodos con listas de columnas personalizadas se vuelve a la lista de columnas de la consulta principal s y no devuelve los datos esperados. Este problema no se da cuando utilice procedimientos almacenados.

En este tutorial, veremos cómo ampliar un esquema de DataTable s para incluir columnas adicionales. Debido a la fragilidad del TableAdapter al usar instrucciones SQL ad hoc, en este tutorial utilizaremos los procedimientos almacenados. Hacer referencia a la [crear procedimientos almacenados nuevos para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) y [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) tutoriales para obtener más información sobre configuración de un TableAdapter para utilizar procedimientos almacenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Paso 1: Agregar un`PriceQuartile`columna a la`ProductsDataTable`

En el *crear procedimientos almacenados nuevos para la s de conjunto de datos con tipo TableAdapters* tutorial hemos creado un conjunto de datos con tipo denominado `NorthwindWithSprocs`. Actualmente, este conjunto de datos contiene dos tablas de datos: `ProductsDataTable` y `EmployeesDataTable`. El `ProductsTableAdapter` tiene los tres métodos siguientes:

- `GetProducts`-la consulta principal, que devuelve todos los registros de la `Products` tabla
- `GetProductsByCategoryID(categoryID)`-Devuelve todos los productos con los valores especificados *categoryID*.
- `GetProductByProductID(productID)`-Devuelve el producto concreto con los valores especificados *productID*.

La consulta principal y los dos métodos adicionales devuelven el mismo conjunto de campos de datos, es decir, todas las columnas de la `Products` tabla. No hay ningún subconsultas correlacionadas o `JOIN` s extrayendo datos relacionados de la `Categories` o `Suppliers` tablas. Por lo tanto, la `ProductsDataTable` no tiene una columna correspondiente para cada campo en el `Products` tabla.

Para este tutorial, s permiten agregar un método a la `ProductsTableAdapter` denominado `GetProductsWithPriceQuartile` que devuelve todos los productos. Además de los campos de datos de producto estándar, `GetProductsWithPriceQuartile` también incluirá un `PriceQuartile` campo de datos que se indica en qué cuartil cae el precio del producto s. Por ejemplo, los productos cuyos precios están en el 25% más caro tendrá un `PriceQuartile` valor 1, mientras que aquellos cuyos precios que se encuentran en la parte inferior 25% tendrá un valor de 4. Antes de que nos preocupamos sobre cómo crear el procedimiento almacenado para devolver esta información, sin embargo, en primer lugar deberá actualizar el `ProductsDataTable` para incluir una columna para almacenar el `PriceQuartile` resultados cuando el `GetProductsWithPriceQuartile` se utiliza el método.

Abra la `NorthwindWithSprocs` conjunto de datos y haga doble clic en el `ProductsDataTable`. Elija Agregar en el menú contextual y, a continuación, elija la columna.


[![Agregar una nueva columna a la ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: agregar una nueva columna a la `ProductsDataTable` ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image3.png))


Esto agregará una nueva columna a la tabla de datos denominada Column1 de tipo `System.String`. Deberá actualizar el nombre de la columna s PriceQuartile y su tipo a `System.Int32` ya que se usarán para almacenar un número entre 1 y 4. Seleccione la columna recién agregado en el `ProductsDataTable` y, en la ventana Propiedades, establezca el `Name` propiedad PriceQuartile y `DataType` propiedad `System.Int32`.


[![Establecer las propiedades de tipo de datos y el nuevo nombre de columna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: establecer la nueva columna s `Name` y `DataType` propiedades ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image6.png))


Como se muestra en la figura 2, hay propiedades adicionales que se pueden establecer, por ejemplo, si los valores de la columna deben ser únicos, si la columna es una columna de incremento automático, si la base de datos `NULL` se permiten valores y así sucesivamente. Deje estos valores establecidos en sus valores predeterminados.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Paso 2: Crear el`GetProductsWithPriceQuartile`(método)

Ahora que el `ProductsDataTable` se ha actualizado para incluir la `PriceQuartile` columna, estamos preparados para crear el `GetProductsWithPriceQuartile` método. Iniciar, el botón secundario en TableAdapter y elija Agregar consulta en el menú contextual. Se abrirá el Asistente para configuración de consultas de TableAdapter, que primero nos preguntará si desea usar instrucciones SQL ad hoc o un procedimiento almacenado nuevo o existente. Puesto que se don t aún tiene un procedimiento almacenado que devuelve los datos de cuartil precio, permiten s permitir TableAdapter crear este procedimiento almacenado para que podamos. Seleccione la opción de crear nuevo procedimiento almacenado y haga clic en siguiente.


[![Indique al Asistente de TableAdapter para crear el procedimiento almacenado para que podamos](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: indicar al Asistente de TableAdapter que cree el almacenado procedimiento para nosotros ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image9.png))


En la pantalla posterior, que se muestra en la figura 4, el Asistente para nosotros preguntará a qué tipo de consulta para agregar. Puesto que la `GetProductsWithPriceQuartile` método devolverá todas las columnas y los registros de la `Products` tabla, seleccione la selección que devuelve filas opción y haga clic en siguiente.


[![Nuestra consulta será una instrucción SELECT que devuelve varias filas](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: consulta nuestra será un `SELECT` instrucción que devuelve varias filas ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image12.png))


A continuación se nos pide la `SELECT` consulta. Escriba la siguiente consulta en el asistente:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La consulta anterior usa SQL Server 2005 s nuevo [ `NTILE` función](https://msdn.microsoft.com/en-us/library/ms175126.aspx) para dividir los resultados en cuatro grupos donde los grupos se determinan por el `UnitPrice` valores en orden descendente.

Desafortunadamente, el generador de consultas no sabe cómo analizar la `OVER` palabra clave y mostrará un error al analizar la consulta anterior. Por lo tanto, escriba la consulta anterior directamente en el cuadro de texto en el asistente sin utilizar el generador de consultas.

> [!NOTE]
> Para obtener más información sobre s NTILE y SQL Server 2005 otras funciones de categoría, vea [devolver resultados clasificados con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) y [sección funciones de categoría](https://msdn.microsoft.com/en-us/library/ms189798.aspx) desde el [SQL Libros en pantalla de Server 2005](https://msdn.microsoft.com/en-us/library/ms189798.aspx).


Después de escribir el `SELECT` consulta y haga clic en siguiente, el asistente le pide que proporcione un nombre para el procedimiento almacenado que se va a crear. Nombre del nuevo procedimiento almacenado `Products_SelectWithPriceQuartile` y haga clic en siguiente.


[![Nombre del procedimiento almacenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nombre del procedimiento almacenado `Products_SelectWithPriceQuartile` ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image15.png))


Por último, se nos pide el nombre de los métodos de TableAdapter. Deje el relleno de ambas una tabla de datos y devolver un DataTable casillas de verificación activadas y el nombre de los métodos `FillWithPriceQuartile` y `GetProductsWithPriceQuartile`.


[![Métodos de nombres TableAdapters y haga clic en Finalizar](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: nombre de los métodos de TableAdapter s y haga clic en Finalizar ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image18.png))


Con el `SELECT` especificado de la consulta y el procedimiento almacenado y los métodos de TableAdapter con el nombre, haga clic en Finalizar para completar el asistente. En este momento es posible que obtenga una advertencia o dos desde el Asistente para que indique que la `OVER` no se admite la construcción de SQL o instrucción. Pueden omitir estas advertencias.

Después de completar el asistente, debe incluir el TableAdapter el `FillWithPriceQuartile` y `GetProductsWithPriceQuartile` métodos y la base de datos deben incluir un procedimiento almacenado denominado `Products_SelectWithPriceQuartile`. Tómese un momento para comprobar que el objeto TableAdapter realmente contiene este nuevo método y que el procedimiento almacenado se ha agregado correctamente a la base de datos. Cuando la comprobación de la base de datos, si no ve el procedimiento almacenado try con los que el botón secundario en la carpeta procedimientos almacenados y elegir la actualización.


![Compruebe que se ha agregado un nuevo método al objeto TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: Compruebe que se ha agregado un nuevo método al objeto TableAdapter


[![Asegúrese de que la base de datos contiene el Products_SelectWithPriceQuartile procedimiento almacenado](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: asegúrese de que la base de datos contiene el `Products_SelectWithPriceQuartile` procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Una de las ventajas de utilizar procedimientos almacenados en lugar de instrucciones SQL ad hoc es que volver a ejecutar al Asistente para configuración de TableAdapter no modificará la columna se enumeran los procedimientos almacenados. Compruebe esto con el botón secundario en TableAdapter, elegir la opción de configurar en el menú contextual para iniciar al asistente y, a continuación, haga clic en Finalizar para completarla. A continuación, vaya a la base de datos, vea el `Products_SelectWithPriceQuartile` procedimiento almacenado. Tenga en cuenta que no se ha modificado su lista de columnas. Habíamos estado usando instrucciones SQL ad hoc, volver a ejecutar el Asistente para configuración de TableAdapter habría revertir esta lista de columnas de la consulta s para que coincida con la lista de columnas de la consulta principal, con lo que se quita la instrucción NTILE de la consulta usa el `GetProductsWithPriceQuartile` método.


Cuando la s de capa de acceso a datos `GetProductsWithPriceQuartile` se invoca el método, lo TableAdapter se ejecuta el `Products_SelectWithPriceQuartile` procedimiento almacenado y agrega una fila a la `ProductsDataTable` de cada registro devuelto. Los campos de datos devueltos por el procedimiento almacenado se asignan a la `ProductsDataTable` columnas s. Dado que no hay un `PriceQuartile` campo de datos devueltos por el procedimiento almacenado, su valor se asigna a la `ProductsDataTable` s `PriceQuartile` columna.

Caso de los métodos de TableAdapter cuyas consultas no devuelven un `PriceQuartile` campo de datos, el `PriceQuartile` valor de columna s es el valor especificado por su `DefaultValue` propiedad. Como se muestra en la figura 2, este valor se establece en `DBNull`, el valor predeterminado. Si prefiere un valor predeterminado diferente, basta con establecer la `DefaultValue` propiedad según corresponda. Asegúrese de que el `DefaultValue` valor es válido, dada la columna s `DataType` (es decir, `System.Int32` para la `PriceQuartile` columna).

En este punto hemos realizado los pasos necesarios para agregar una columna adicional a una tabla de datos. Para comprobar que esta columna adicional funciona según lo previsto, permiten crear una página ASP.NET que muestra cada nombre s del producto, precio y cuartil precio de s. Antes de hacerlo, sin embargo, en primer lugar deberá actualizar la capa de lógica de negocios para que incluya un método que llama hacia abajo a la capa DAL s `GetProductsWithPriceQuartile` método. Se actualizará el BLL a continuación, en el paso 3 y, a continuación, crear la página ASP.NET en el paso 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Paso 3: Aumentar el nivel de lógica de negocios

Antes de que se use la nueva `GetProductsWithPriceQuartile` método de la capa de presentación, se debe agregar un método correspondiente a la capa BLL. Abra la `ProductsBLLWithSprocs` archivo de clase y agregue el código siguiente:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Al igual que los otros métodos de recuperación de datos en `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` método simplemente llama a la capa DAL s correspondiente `GetProductsWithPriceQuartile` método y devuelve sus resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Paso 4: Mostrar la información de precios cuartil en una página Web ASP.NET

Con la adición de BLL completar se re listo para crear una página ASP.NET que muestra el cuartil de precio para cada producto. Abra el `AddingColumns.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Products`. Desde la etiqueta inteligente de GridView s, enlazarla a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para usar el `ProductsBLLWithSprocs` clase s. `GetProductsWithPriceQuartile` método. Puesto que se trata de una cuadrícula de solo lectura, Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar tabulaciones en (ninguno).


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurar el ObjectDataSource que se utilizan los `ProductsBLLWithSprocs` clase ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperar la información de producto desde el método GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperar información del producto desde la `GetProductsWithPriceQuartile` método ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image28.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un BoundField o CampoCasillaVerificación a GridView para cada uno de los campos de datos devueltos por el método. Uno de estos campos de datos es `PriceQuartile`, que es la columna se agrega a la `ProductsDataTable` en el paso 1.

Modifique los campos de s GridView, quitar todos menos el `ProductName`, `UnitPrice`, y `PriceQuartile` BoundFields. Configurar la `UnitPrice` BoundField para dar formato a su valor como moneda y tener la `UnitPrice` y `PriceQuartile` BoundFields y center-alineado a la derecha, respectivamente. Por último, actualizar los restantes BoundFields `HeaderText` propiedades de producto, precio y precio cuartil, respectivamente. Además, active la casilla Habilitar ordenación de la etiqueta inteligente de s de GridView.

Después de estas modificaciones, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

La figura 11 muestra esta página cuando visita a través de un explorador. Tenga en cuenta que, en principio, los productos se ordenan por su precio en orden descendente con cada producto que se asigna un adecuado `PriceQuartile` valor. Por supuesto estos datos se pueden ordenar por otros criterios con el valor de la columna de precio cuartil todavía reflejar la clasificación de producto s con respecto a los precios (consulte la figura 12).


[![Los productos se ordenan por sus precios](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: los productos se ordenan por sus precios ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image31.png))


[![Los productos se ordenan por sus nombres](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: los productos se ordenan por sus nombres ([haga clic aquí para ver la imagen a tamaño completo](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Con unas pocas líneas de código se puede aumentar GridView para que lo colorea las filas del producto basándose en sus `PriceQuartile` valor. Nos tengamos que dichos productos en el primer cuartil verde claro, los de la segundo cuartil un amarillo claro de color y así sucesivamente. Recomendamos que dedique unos minutos a agregar esta funcionalidad. Si necesita un actualizador de dar formato a un control GridView, consulte el [formato basado en datos personalizados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un enfoque alternativo - crear otro objeto TableAdapter

Como hemos visto en este tutorial, al agregar un método a un TableAdapter que devuelve los campos de datos distintos de los especificados por la consulta principal, podemos agregar las columnas correspondientes a la tabla de datos. Este enfoque, sin embargo, da buenos resultados solo si hay un pequeño número de métodos de TableAdapter que devuelven los campos de datos diferentes, y si los campos de datos alternativo no varía mucho de la consulta principal.

En lugar de agregar columnas a la tabla de datos, en su lugar, puede agregar otro TableAdapter al conjunto de datos que contiene los métodos de la primera TableAdapter que devuelven los campos de datos diferentes. Para este tutorial, en lugar de agregar el `PriceQuartile` columna a la `ProductsDataTable` (donde solo se usa por la `GetProductsWithPriceQuartile` (método)), podríamos ha agregado un TableAdapter adicional al conjunto de datos denominado `ProductsWithPriceQuartileTableAdapter` que usa el `Products_SelectWithPriceQuartile` almacenados procedimiento como su consulta principal. Páginas ASP.NET que necesita para obtener información del producto con el cuartil precio utilizaría el `ProductsWithPriceQuartileTableAdapter`, mientras que los que no lo hicieron podrían seguir usando el `ProductsTableAdapter`.

Al agregar un nuevo TableAdapter, DataTables permanecen untarnished y sus columnas reflejan con precisión los campos de datos devueltos por los métodos de TableAdapter s. Sin embargo, los TableAdapters adicionales pueden introducir funcionalidad y las tareas repetitivas. Por ejemplo, si las páginas de ASP.NET que muestran la `PriceQuartile` columna también sea necesario para proporcionar la inserción, actualización y eliminación de soporte técnico, la `ProductsWithPriceQuartileTableAdapter` es necesario que su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades correctamente configurar. Aunque estas propiedades debe reflejar el `ProductsTableAdapter` s, esta configuración presenta un paso adicional. Además, hay ahora dos formas de actualizar, eliminar o agregar un producto a la base de datos - a través de la `ProductsTableAdapter` y `ProductsWithPriceQuartileTableAdapter` clases.

La descarga de este tutorial incluye un `ProductsWithPriceQuartileTableAdapter` clase en el `NorthwindWithSprocs` conjunto de datos que muestra este enfoque alternativo.

## <a name="summary"></a>Resumen

En la mayoría de los escenarios, todos los métodos en un TableAdapter devolverá el mismo conjunto de campos de datos, pero hay ocasiones cuando necesite un método concreto o dos para devolver un campo adicional. Por ejemplo, en la [principal/detalle con una lista con viñetas de registros maestros DataList detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial hemos agregado un método para el `CategoriesTableAdapter` que, además de los campos de datos de la consulta principal s, devueltos un `NumberOfProducts` campo que notifica el número de productos asociados con cada categoría. En este tutorial, observamos agregar un método en el `ProductsTableAdapter` que devuelve un `PriceQuartile` campo además de los campos de datos de la consulta principal s. Para capturar datos adicionales campos devueltos por los métodos de TableAdapter s que necesitamos agregar las columnas correspondientes a la tabla de datos.

Si tiene previsto agregar manualmente las columnas a la tabla de datos, se recomienda que los TableAdapter utilizan procedimientos almacenados. Si el objeto TableAdapter utiliza instrucciones SQL ad hoc, siempre que el Asistente para configuración de TableAdapter se ejecuta todos los métodos revertir las listas de campos de datos a los campos de datos devueltos por la consulta principal. Este problema no se extiende a los procedimientos almacenados, que es el motivo por el que se recomiendan y se usan en este tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisores para este tutorial eran Randy Schmidt, Goor Jacky, Bernadette Leigh y Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](updating-the-tableadapter-to-use-joins-cs.md)
[Siguiente](working-with-computed-columns-cs.md)
