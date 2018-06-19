---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Ordenación personalizada paginado datos (C#) | Documentos de Microsoft
author: rick-anderson
description: En el tutorial anterior, hemos visto cómo implementar paginación personalizada cuando presentating datos en una página web. En este tutorial se muestra cómo ampliar los anteriores...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: c30f13703ef20cd764785b00cd812ef4486e0f16
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882421"
---
<a name="sorting-custom-paged-data-c"></a>Ordenación personalizada paginado datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) o [descarga de PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> En el tutorial anterior, hemos visto cómo implementar paginación personalizada cuando presentating datos en una página web. En este tutorial, vemos cómo ampliar el ejemplo anterior para incluir la compatibilidad para la ordenación paginación personalizada.


## <a name="introduction"></a>Introducción

En comparación con la paginación de manera predeterminada, la paginación personalizada puede mejorar el rendimiento de paginación de datos mediante varias órdenes de magnitud, realizar la paginación de la opción de implementación de paginación de hecho al paginar a través de grandes cantidades de datos personalizado. Implementar la paginación personalizada es más complejo que implementar la paginación de forma predeterminada, sin embargo, especialmente al agregar ordenación a la combinación. En este tutorial ampliaremos el ejemplo de la anterior para incluir compatibilidad con la ordenación *y* paginación personalizada.

> [!NOTE]
> Puesto que este tutorial se basa en el caso anterior, antes de principio Tómese un momento para copiar la sintaxis declarativa en el `<asp:Content>` elemento de la página web de tutorial s anterior (`EfficientPaging.aspx`) y pegar entre el `<asp:Content>` elemento en el `SortParameter.aspx` página. Hacen referencia al paso 1 de la [agregar controles de validación a las Interfaces de insertar y modificar](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para obtener una explicación más detallada sobre la replicación de la funcionalidad de una página ASP.NET a otro.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Paso 1: Volver a examinar la técnica de paginación personalizada

Para que funcione correctamente la paginación personalizada, debemos implementar alguna otra técnica que puede tomar eficazmente un subconjunto específico de registros dados los parámetros de índice de fila inicial y el número máximo de filas. Hay una serie de técnicas que puede usar para lograr este objetivo. En el tutorial anterior, observamos para realizar esto con Microsoft SQL Server 2005 s nuevo `ROW_NUMBER()` función de categoría. En resumen, la `ROW_NUMBER()` función de categoría, asigna un número de fila para cada fila devuelta por una consulta que se clasifican por un criterio de ordenación especificado. El subconjunto apropiado de registros, a continuación, se obtiene mediante la devolución de una sección concreta de los resultados numerados. La consulta siguiente muestra cómo utilizar esta técnica para devolver los productos numerados 11 a 20 clasificar los resultados ordenados alfabéticamente según la `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Esta técnica funciona bien para paginación mediante un criterio de ordenación concreto (`ProductName` ordenadas alfabéticamente, en este caso), pero la consulta tiene que modificarse para mostrar los resultados ordenados por una expresión de ordenación diferente. Idealmente, la consulta anterior podría modificarse para usar un parámetro en el `OVER` cláusula, así:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Por desgracia, con parámetros `ORDER BY` cláusulas no están permitidas. En su lugar, debemos crear un procedimiento almacenado que acepta un `@sortExpression` parámetro de entrada, pero usa una de las siguientes acciones:

- Escribir consultas codificado de forma rígida para cada una de las expresiones de ordenación que se pueden usar; a continuación, utilice `IF/ELSE` instrucciones T-SQL para determinar qué consulta se debe ejecutar.
- Use un `CASE` instrucción para proporcionar dinámica `ORDER BY` las expresiones se basan en el `@sortExpressio` n parámetro de entrada, consulte la utilizada en la sección de resultados de la consulta de ordenación dinámicamente en [Power de SQL `CASE` instrucciones](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Para obtener más información.
- Diseñar la consulta adecuada como una cadena en el procedimiento almacenado y, a continuación, usar [el `sp_executesql` procedimiento almacenado del sistema](https://msdn.microsoft.com/library/ms188001.aspx) para ejecutar la consulta dinámica.

Cada una de estas soluciones tiene algunas desventajas. La primera opción no es tan fácil de mantener que las otras dos tal y como lo requiere la creación de una consulta para cada expresión de ordenación posible. Por lo tanto, si más adelante decide agregar campos nuevos, que se puede ordenar a GridView también necesitará volver atrás y actualizar el procedimiento almacenado. El segundo enfoque tiene algunos matices que presentan problemas de rendimiento al ordenar por columnas de la base de datos no es una cadena y también se resiente de los mismos problemas de mantenimiento como la primera. Y la tercera opción, que utiliza SQL dinámico, corre el riesgo de un ataque de inyección de SQL si un atacante es capaz de ejecutar el procedimiento almacenado pasando los valores de parámetro de entrada de su elección.

Aunque ninguno de estos enfoques es perfecto, creo que la tercera opción es lo mejor de los tres. Con el uso de SQL dinámico, ofrece un nivel de flexibilidad que los otros dos no. Además, solo se puede aprovechar un ataque de inyección de SQL si un atacante es capaz de ejecutar el procedimiento almacenado pasando los parámetros de entrada de su elección. Puesto que la capa DAL utiliza las consultas parametrizadas, ADO.NET protegerá los parámetros que se envían a la base de datos a través de la arquitectura, lo que significa que la vulnerabilidad de ataques de inyección de SQL solo existe si el atacante puede ejecutar directamente el procedimiento almacenado.

Para implementar esta funcionalidad, cree un nuevo procedimiento almacenado en la base de datos de Northwind denominado `GetProductsPagedAndSorted`. Este procedimiento almacenado debe aceptar tres parámetros de entrada: `@sortExpression`, un parámetro de entrada de tipo `nvarchar(100`) que especifica el modo en que los resultados se deben ordenar y se aplica directamente después de la `ORDER BY` texto en la `OVER` cláusula; y `@startRowIndex` y `@maximumRows`, los mismos parámetros de entrada de dos enteros de la `GetProductsPaged` examinado en el tutorial anterior del procedimiento almacenado. Crear el `GetProductsPagedAndSorted` mediante el siguiente script de procedimiento almacenado:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

El procedimiento almacenado inicia asegurándose de que un valor para el `@sortExpression` se ha especificado el parámetro. Si está presente, los resultados se clasifican por `ProductID`. A continuación, se crea la consulta SQL dinámica. Tenga en cuenta que la consulta SQL dinámica aquí difiere ligeramente de nuestro consultas anteriores que se usa para recuperar todas las filas de la tabla Products. En los ejemplos anteriores, hemos recopilado a cada categoría de asociado de producto s s y proveedor nombres s utilizando una subconsulta. Esta decisión se toma en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial y se realizó en vez de usar `JOIN` s como TableAdapter no puede crear automáticamente la inserción asociada, actualizar y eliminar los métodos de dicha consultas. El `GetProductsPagedAndSorted` procedimiento almacenado, sin embargo, debe usar `JOIN` para ver si los resultados se ordenen por los nombres de categoría o proveedor.

Esta consulta dinámica se componen mediante la concatenación de las partes de consulta estática y la `@sortExpression`, `@startRowIndex`, y `@maximumRows` parámetros. Puesto que `@startRowIndex` y `@maximumRows` son parámetros de enteros, se debe convertir en nvarchars para concatenar correctamente. Una vez que se ha construido esta consulta SQL dinámica, se ejecuta a través de `sp_executesql`.

Tómese un momento para probar este procedimiento almacenado con valores diferentes para la `@sortExpression`, `@startRowIndex`, y `@maximumRows` parámetros. En el Explorador de servidores, haga doble clic en el nombre del procedimiento almacenado y elija ejecutar. Se abrirá el cuadro de diálogo Ejecutar procedimiento almacenado en la que puede especificar los parámetros de entrada (consulte la figura 1). Para ordenar los resultados por el nombre de categoría, use CategoryName para el `@sortExpression` valor del parámetro; para ordenar por nombre de compañía proveedor s, use CompanyName. Después de proporcionar los valores de parámetros, haga clic en Aceptar. Los resultados se muestran en la ventana de salida. La figura 2 muestra los resultados al devolver productos clasificados 11 a 20 al ordenar por el `UnitPrice` en orden descendente.


![Probar valores diferentes para el procedimiento almacenado s tres parámetros de entrada](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: probar distintos valores para el procedimiento almacenado s tres parámetros de entrada


[![El procedimiento almacenado s resultados se muestran en la ventana de salida](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: el procedimiento almacenado s resultados se muestran en la ventana de salida ([haga clic aquí para ver la imagen a tamaño completo](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Cuando los resultados de clasificación según los valores especificados `ORDER BY` columna en el `OVER` cláusula, SQL Server debe ordenar los resultados. Esto es una operación rápida si hay un índice clúster sobre las columnas que se va a se ordenan los resultados por o si hay una cobertura de índice, pero puede ser más costoso en caso contrario. Para mejorar el rendimiento de las consultas lo suficientemente grandes, considere la posibilidad de agregar un índice no agrupado de la columna por la que los resultados se ordenan por. Hacer referencia a [funciones de categoría y el rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obtener más detalles.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Paso 2: Aumentar el acceso a datos y las capas de lógica de negocios

Con el `GetProductsPagedAndSorted` crea un procedimiento almacenado, el siguiente paso es proporcionar un medio para ejecutar ese procedimiento almacenado a través de la arquitectura de la aplicación. Esto implica agregar un método adecuado para la capa DAL y BLL. Permiten s empiece agregando un método a la capa DAL. Abra la `Northwind.xsd` Typed DataSet, menú contextual en el `ProductsTableAdapter`y elija la opción de Agregar consulta en el menú contextual. Igual que hicimos en el tutorial anterior, desea configurar este nuevo método de la capa DAL para utilizar un procedimiento almacenado existente - `GetProductsPagedAndSorted`, en este caso. Empiece por indica que desea que utilice un procedimiento almacenado existente, el nuevo método de TableAdapter.


![Decide usar un procedimiento almacenado existente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: decide usar un procedimiento almacenado existente


Para especificar el procedimiento almacenado para usar, seleccione el `GetProductsPagedAndSorted` el procedimiento almacenado de la lista desplegable en la pantalla siguiente.


![Utilice la GetProductsPagedAndSorted procedimiento almacenado](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: usar el GetProductsPagedAndSorted procedimiento almacenado


Este procedimiento almacenado devuelve un conjunto de registros, como los resultados por lo tanto, en la siguiente pantalla, indican que devuelve datos tabulares.


![Indicar que el procedimiento almacenado devuelve datos tabulares](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indica que el procedimiento almacenado devuelve datos tabulares


Por último, crear métodos DAL que utilizan tanto el relleno una tabla de datos y devolver un patrones DataTable, los métodos de nomenclatura `FillPagedAndSorted` y `GetProductsPagedAndSorted`, respectivamente.


![Elija los nombres de métodos](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: elija los nombres de métodos


Ahora que se ha ampliado la capa DAL, nos re listo para volver a la capa BLL. Abra la `ProductsBLL` archivo de clase y agregue un nuevo método, `GetProductsPagedAndSorted`. Este método debe aceptar tres parámetros de entrada `sortExpression`, `startRowIndex`, y `maximumRows` y debería llamar simplemente hacia abajo en la capa DAL s `GetProductsPagedAndSorted` método, de este modo:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Paso 3: Configurar el ObjectDataSource a pasada en el parámetro SortExpression

Tener ampliada DAL y BLL para incluir métodos que utilizan el `GetProductsPagedAndSorted` procedimiento almacenado, todo lo que queda es configurar el ObjectDataSource en el `SortParameter.aspx` página que se va a utilizar el nuevo método BLL como pasar el `SortExpression` parámetro basado en el columna para ordenar los resultados por solicitada por el usuario.

Empiece por cambiar las operaciones de asignación ObjectDataSource `SelectMethod` de `GetProductsPaged` a `GetProductsPagedAndSorted`. Esto puede hacerse mediante el asistente Configurar origen de datos, en la ventana Propiedades, o directamente a través de la sintaxis declarativa. A continuación, es necesario proporcionar un valor para las operaciones de asignación ObjectDataSource [ `SortParameterName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Si se establece esta propiedad, el ObjectDataSource intenta pasar en las operaciones de asignación GridView `SortExpression` propiedad a la `SelectMethod`. En concreto, el ObjectDataSource busca un parámetro de entrada cuyo nombre es igual al valor de la `SortParameterName` propiedad. Desde el s BLL `GetProductsPagedAndSorted` método tiene el con el nombre del parámetro de entrada de la expresión de ordenación `sortExpression`, establezca la s ObjectDataSource `SortExpression` propiedad sortExpression.

Después de realizar estos dos cambios, la sintaxis declarativa de ObjectDataSource s debe ser similar al siguiente:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Como con el tutorial anterior, asegúrese de que el ObjectDataSource no *no* incluir los parámetros de entrada sortExpression, startRowIndex o maximumRows en su colección SelectParameters.


Para habilitar la ordenación en el control GridView, solo tiene que activar la casilla de verificación Habilitar ordenación en la etiqueta inteligente de s GridView, que establece las operaciones de asignación GridView `AllowSorting` propiedad `true` de archivo y provoquen el texto del encabezado de cada columna se representan como un control LinkButton. Cuando el usuario final hace clic en uno de los encabezados LinkButton de, seguidamente, tiene lugar una devolución de datos y transcurrir los pasos siguientes:

1. Las actualizaciones de GridView su [ `SortExpression` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) en el valor de la `SortExpression` del campo cuyo vínculo de encabezado se hizo clic
2. ObjectDataSource invoca las operaciones de asignación BLL `GetProductsPagedAndSorted` método, pasando el s GridView `SortExpression` propiedad como el valor para el método s `sortExpression` parámetro de entrada (junto con la correspondiente `startRowIndex` y `maximumRows` valores de parámetro de entrada)
3. La capa BLL invoca las operaciones de asignación DAL `GetProductsPagedAndSorted` (método)
4. La capa DAL se ejecuta el `GetProductsPagedAndSorted` procedimiento almacenado, pasar en el `@sortExpression` parámetro (junto con la `@startRowIndex` y `@maximumRows` valores de parámetro de entrada)
5. El procedimiento almacenado devuelve el subconjunto de datos adecuado a la capa BLL, que devuelve a ObjectDataSource; estos datos, a continuación, se enlaza a GridView, representar en HTML y enviar al usuario final

La figura 7 muestra la primera página de resultados cuando se ordenan por el `UnitPrice` en orden ascendente.


[![Los resultados se ordenan por el precio unitario](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: los resultados se ordenan por el precio unitario ([haga clic aquí para ver la imagen a tamaño completo](sorting-custom-paged-data-cs/_static/image11.png))


Mientras que la implementación actual correctamente puede ordenar los resultados por nombre de producto, nombre de categoría, cantidad por unidad y el precio unitario, intenta ordenar los resultados por el proveedor nombre tiene como resultado una excepción en tiempo de ejecución (consulte la figura 8).


![Intentar ordenar los resultados por los resultados de proveedor en la siguiente excepción en tiempo de ejecución](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: intentar ordenar los resultados por los resultados de proveedor en la siguiente excepción en tiempo de ejecución


Esta excepción se produce porque el `SortExpression` de las operaciones de asignación GridView `SupplierName` BoundField está establecido en `SupplierName`. Sin embargo, el nombre del proveedor s en el `Suppliers` realmente se denomina tabla `CompanyName` hemos estado tiene un alias como nombre de la columna `SupplierName`. Sin embargo, el `OVER` cláusula utilizada por el `ROW_NUMBER()` función no se puede utilizar el alias y debe utilizar el nombre real de la columna. Por lo tanto, cambie la `SupplierName` BoundField s `SortExpression` desde NombreProveedor en CompanyName (consulte la figura 9). Como se muestra en la figura 10, después de este cambio que se pueden ordenar los resultados por el proveedor.


![Cambie el SortExpression s NombreProveedor BoundField a CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: cambiar el SortExpression s NombreProveedor BoundField a CompanyName


[![Ahora se pueden ordenar los resultados por proveedor](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: los resultados ahora se pueden ordenar por proveedor ([haga clic aquí para ver la imagen a tamaño completo](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Resumen

La implementación de paginación personalizada que se examina en el tutorial anterior requiere que se especifique el orden por el que fueron los resultados se ordenen en tiempo de diseño. En resumen, esto significaba que la implementación de paginación personalizada que se implementa no pudo, al mismo tiempo, proporcionar capacidades de ordenación. En este tutorial se superaron esta limitación mediante el procedimiento almacenado que se extiende desde el primero que se va a incluir un `@sortExpression` parámetro de entrada que se puede ordenar los resultados.

Después de crear este procedimiento almacenado y crear nuevos métodos en la capa DAL y BLL, hemos capaz de implementar un control GridView que ofrece tanto la ordenación y personalizados de paginación mediante la configuración de ObjectDataSource para pasar de las operaciones de asignación GridView actual `SortExpression` propiedad a la capa BLL `SelectMethod`.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Carlos Santos. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Siguiente](creating-a-customized-sorting-user-interface-cs.md)
