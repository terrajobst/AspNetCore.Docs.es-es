---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Ordenar datos (C#) paginados personalizados | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior, aprendió a implementar la paginación personalizada al presentating datos en una página web. En este tutorial se muestra cómo extender la anterior...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 36080799c94b359a7e5b7ba6ccb47e359fc2161d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362802"
---
<a name="sorting-custom-paged-data-c"></a>Ordenar datos (C#) paginados personalizados
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) o [descargar PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> En el tutorial anterior, aprendió a implementar la paginación personalizada al presentating datos en una página web. En este tutorial veremos cómo ampliar el ejemplo anterior para incluir compatibilidad con para ordenar la paginación personalizada.


## <a name="introduction"></a>Introducción

En comparación con la paginación de forma predeterminada, la paginación personalizada puede mejorar el rendimiento de paginación de datos en varios órdenes de magnitud, realizar personalizado paginación la elección de implementación de paginación de hecho, al paginar a través de grandes cantidades de datos. Implementar la paginación personalizada es más complejo que implementar la paginación de forma predeterminada, sin embargo, especialmente cuando se agrega a la combinación de ordenación. En este tutorial se extenderá el ejemplo de la anterior para incluir compatibilidad con ordenación *y* paginación personalizada.

> [!NOTE]
> Puesto que este tutorial se basa en lo anterior, antes de principio dedique un momento para copiar la sintaxis declarativa en el `<asp:Content>` elemento desde la página web de tutorial s anterior (`EfficientPaging.aspx`) y péguelo entre el `<asp:Content>` elemento en el `SortParameter.aspx` página. Hacer referencia al paso 1 de la [agregar controles de validación a las Interfaces de inserción y edición](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para obtener una explicación más detallada sobre la replicación de la funcionalidad de una página ASP.NET a otro.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Paso 1: Volver a examinar la técnica de paginación personalizada

Para que funcione correctamente la paginación personalizada, debemos implementar alguna otra técnica que puede tomar eficazmente un subconjunto específico de registros dados los parámetros de índice de fila inicial y el número máximo de filas. Hay una serie de técnicas que puede usarse para lograr este objetivo. En el tutorial anterior, analizamos lograrlo con Microsoft SQL Server 2005 s nuevo `ROW_NUMBER()` función de categoría. En resumen, el `ROW_NUMBER()` función de categoría, asigna un número de fila para cada fila devuelta por una consulta que se clasifican por un criterio de ordenación especificado. Subconjunto adecuado de registros, a continuación, se obtiene mediante la devolución de una sección determinada de los resultados de números. La consulta siguiente muestra cómo utilizar esta técnica para devolver esos productos numerados 11 a 20 clasificar los resultados ordenados alfabéticamente según el `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Esta técnica funciona bien para paginación mediante un criterio de ordenación concreto (`ProductName` ordenados alfabéticamente, en este caso), pero la consulta debe modificarse para mostrar los resultados ordenados por una expresión de ordenación diferente. Idealmente, se podría reescribir la consulta anterior para usar un parámetro en el `OVER` cláusula, así:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Desafortunadamente, con parámetros `ORDER BY` cláusulas no se permiten. En su lugar, debemos crear un procedimiento almacenado que acepta un `@sortExpression` parámetro de entrada, pero usa una de las siguientes soluciones:

- Escribir consultas codificado de forma rígida para cada una de las expresiones de ordenación que se pueden usar; a continuación, utilice `IF/ELSE` instrucciones T-SQL para determinar qué consulta se ejecute.
- Use un `CASE` instrucción para proporcionar dinámica `ORDER BY` expresiones según el `@sortExpressio` n parámetro de entrada; vea la utilizada para la sección de resultados de la consulta de ordenación dinámicamente en [la potencia de SQL `CASE` instrucciones](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Para obtener más información.
- Diseñar la consulta adecuada como una cadena en el procedimiento almacenado y, a continuación, usar [el `sp_executesql` procedimiento almacenado del sistema](https://msdn.microsoft.com/library/ms188001.aspx) para ejecutar la consulta dinámica.

Cada una de estas soluciones tiene algunas desventajas. La primera opción no es tan fácil de mantener que las otras dos tal como lo requiere la creación de una consulta para cada expresión de ordenación posible. Por lo tanto, si más adelante decide agregar campos nuevos, que se puede ordenar en GridView también necesitará volver atrás y actualizar el procedimiento almacenado. El segundo enfoque tiene algunos matices que presentan problemas de rendimiento al ordenar por columnas de la base de datos que no son de cadena y también sufre los mismos problemas de mantenimiento como la primera. Y la tercera opción, que utiliza SQL dinámico, corre el riesgo de ataques de inyección SQL si un atacante es capaz de ejecutar el procedimiento almacenado pasando los valores de parámetro de entrada de su elección.

Aunque ninguno de estos enfoques es perfecto, creo que la tercera opción es lo mejor de los tres. Con el uso de SQL dinámico, ofrece un nivel de flexibilidad que los otros dos no lo hacen. Además, un ataque de inyección de SQL puede aprovecharse solo si un atacante es capaz de ejecutar el procedimiento almacenado pasando los parámetros de entrada de su elección. Puesto que la capa DAL utiliza consultas parametrizadas, ADO.NET protegerá los parámetros que se envían a la base de datos a través de la arquitectura, lo que significa que la vulnerabilidad de ataques de inyección de SQL solo existirá si el atacante puede ejecutar directamente el procedimiento almacenado.

Para implementar esta funcionalidad, cree un nuevo procedimiento almacenado en la base de datos Northwind denominado `GetProductsPagedAndSorted`. Este procedimiento almacenado debe aceptar tres parámetros de entrada: `@sortExpression`, un parámetro de entrada de tipo `nvarchar(100`) que especifica el modo en que los resultados se deben ordenar y se inserta directamente después de la `ORDER BY` texto en el `OVER` cláusula; y `@startRowIndex` y `@maximumRows`, los mismos parámetros de entrada de dos enteros desde el `GetProductsPaged` examinado en el tutorial anterior de procedimiento almacenado. Crear el `GetProductsPagedAndSorted` mediante el siguiente script de procedimiento almacenado:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

El procedimiento almacenado se inicia si se asegura de que un valor para el `@sortExpression` se ha especificado el parámetro. Si falta, los resultados se clasifican por `ProductID`. A continuación, se construye la consulta SQL dinámica. Tenga en cuenta que la consulta SQL dinámica aquí difiere ligeramente de nuestro consultas anteriores que se usa para recuperar todas las filas de la tabla Products. En los ejemplos anteriores, hemos recopilado cada categoría asociada del producto s s y el proveedor nombres s con una subconsulta. Se tomó esta decisión en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial y se realizó en vez de usar `JOIN` s porque el TableAdapter no puede crear automáticamente la inserción asociada, actualizar y eliminar los métodos de dicha consultas. El `GetProductsPagedAndSorted` el procedimiento almacenado, sin embargo, debe usar `JOIN` s que se ordenan por los nombres de categorías o proveedores de los resultados.

Esta consulta dinámica es generada por la concatenación de las partes de consulta estática y el `@sortExpression`, `@startRowIndex`, y `@maximumRows` parámetros. Puesto que `@startRowIndex` y `@maximumRows` son parámetros de número entero, deben convertirse en campos nvarchar para concatenarse correctamente. Una vez que se ha construido esta consulta SQL dinámica, que se ejecuta a través de `sp_executesql`.

Dedique un momento para probar este procedimiento almacenado con valores diferentes para el `@sortExpression`, `@startRowIndex`, y `@maximumRows` parámetros. Desde el Explorador de servidores, haga doble clic en el nombre del procedimiento almacenado y elija ejecutar. Se abrirá el cuadro de diálogo Ejecutar procedimiento almacenado en la que puede especificar los parámetros de entrada (consulte la figura 1). Para ordenar los resultados por el nombre de categoría, use CategoryName para el `@sortExpression` valor del parámetro; para ordenar por nombre de compañía s del proveedor, use CompanyName. Después de proporcionar los valores de parámetros, haga clic en Aceptar. Los resultados se muestran en la ventana de salida. Figura 2 muestra los resultados al devolver los productos clasificados 11 a 20 al ordenar por la `UnitPrice` en orden descendente.


![Pruebe diferentes valores para el procedimiento almacenado s tres parámetros de entrada](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: probar valores diferentes para el procedimiento almacenado s tres parámetros de entrada


[![El procedimiento almacenado de s resultados se muestran en la ventana de salida](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: The Stored Procedure s resultados se muestran en la ventana de salida ([haga clic aquí para ver imagen en tamaño completo](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Cuando los resultados de clasificación por especificado `ORDER BY` columna en el `OVER` cláusula, SQL Server debe ordenar los resultados. Esto es una operación rápida si hay un índice clúster a través de las columnas que se va a se ordenan los resultados por o si hay una cobertura de índice, pero puede ser más costosa de lo contrario. Para mejorar el rendimiento de las consultas lo suficientemente grandes, considere la posibilidad de agregar un índice no agrupado para la columna por la que los resultados se ordenan por. Consulte [funciones de categoría y el rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obtener más detalles.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Paso 2: Aumentar el acceso a datos y capas de lógica de negocios

Con el `GetProductsPagedAndSorted` crea un procedimiento almacenado, el siguiente paso es proporcionar un medio para ejecutar ese procedimiento almacenado a través de nuestra arquitectura de aplicación. Esto conlleva agregar un método adecuado para DAL y BLL. Permiten s empiece agregando un método a la capa DAL. Abra el `Northwind.xsd` DataSet con tipo, botón secundario en el `ProductsTableAdapter`y elija la opción de Agregar consulta en el menú contextual. Como hicimos en el tutorial anterior, desea configurar este nuevo método DAL para utilizar un procedimiento almacenado existente - `GetProductsPagedAndSorted`, en este caso. Empiece por indica que desea que el nuevo método de TableAdapter para usar un procedimiento almacenado existente.


![Optar por usar un procedimiento almacenado existente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: decide usar un procedimiento almacenado existente


Para especificar el procedimiento almacenado a utilizar, seleccione el `GetProductsPagedAndSorted` procedimiento almacenado desde la lista desplegable en la siguiente pantalla.


![Utilice el GetProductsPagedAndSorted procedimiento almacenado](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: usar el GetProductsPagedAndSorted procedimiento almacenado


Este procedimiento almacenado devuelve un conjunto de registros como sus resultados por lo tanto, en la pantalla siguiente, indican que devuelve datos tabulares.


![Indicar que el procedimiento almacenado devuelve datos tabulares](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indicar que el procedimiento almacenado devuelve datos tabulares


Por último, cree métodos DAL que usan tanto el relleno una DataTable y devolver un DataTable patrones, los métodos de nomenclatura `FillPagedAndSorted` y `GetProductsPagedAndSorted`, respectivamente.


![Elija los nombres de métodos](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: elija los nombres de métodos


Ahora que se ve ampliada la capa DAL, que está listo para convertir a la capa BLL. Abra el `ProductsBLL` archivo de clase y agregue un nuevo método `GetProductsPagedAndSorted`. Este método debe aceptar tres parámetros de entrada `sortExpression`, `startRowIndex`, y `maximumRows` y debe llamar simplemente hacia abajo a la s DAL `GetProductsPagedAndSorted` método, de este modo:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Paso 3: Configuración de ObjectDataSource a pasada en el parámetro SortExpression

Tener aumentada DAL y BLL para incluir métodos que usan el `GetProductsPagedAndSorted` procedimiento almacenado, todo lo que queda consiste en configurar el origen ObjectDataSource en el `SortParameter.aspx` página para utilizar el nuevo método BLL y pasar la `SortExpression` parámetro basado en la columna que el usuario ha solicitado que para ordenar los resultados.

Empiece por cambiar la s ObjectDataSource `SelectMethod` desde `GetProductsPaged` a `GetProductsPagedAndSorted`. Esto puede hacerse mediante el Asistente para configurar origen de datos, en la ventana Propiedades, o directamente a través de la sintaxis declarativa. A continuación, necesitamos proporcionar un valor para el s ObjectDataSource [ `SortParameterName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Si se establece esta propiedad, el origen ObjectDataSource intenta pasar la s GridView `SortExpression` propiedad a la `SelectMethod`. En concreto, el origen ObjectDataSource busca un parámetro de entrada cuyo nombre es igual al valor de la `SortParameterName` propiedad. Desde la s BLL `GetProductsPagedAndSorted` método tiene el parámetro de entrada de expresión de ordenación denominado `sortExpression`, establezca la s ObjectDataSource `SortExpression` propiedad sortExpression.

Después de realizar estos dos cambios, la sintaxis declarativa de ObjectDataSource s debe ser similar al siguiente:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Como con el tutorial anterior, asegúrese de que el origen ObjectDataSource no *no* incluyen los parámetros de entrada sortExpression, startRowIndex o maximumRows en su colección SelectParameters.


Para habilitar la ordenación en el control GridView, simplemente marque la casilla de verificación Habilitar ordenación en la etiqueta inteligente de s GridView, que establece la s GridView `AllowSorting` propiedad `true` y lo que hace que el texto del encabezado de cada columna se representan como un control LinkButton. Cuando el usuario final hace clic en uno de lo encabezado LinkButtons, una devolución de datos que habrá trastornos y tienen lugar los siguientes pasos:

1. Las actualizaciones de GridView su [ `SortExpression` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) al valor de la `SortExpression` del campo cuyo vínculo del encabezado se hizo clic
2. ObjectDataSource invoca la s BLL `GetProductsPagedAndSorted` método, pasando la s GridView `SortExpression` propiedad como valor para el método s `sortExpression` parámetro de entrada (junto con la correspondiente `startRowIndex` y `maximumRows` valores de parámetro de entrada)
3. La capa BLL invoca la s DAL `GetProductsPagedAndSorted` (método)
4. La capa DAL se ejecuta el `GetProductsPagedAndSorted` procedimiento almacenado pasando en el `@sortExpression` parámetro (junto con el `@startRowIndex` y `@maximumRows` valores de parámetro de entrada)
5. El procedimiento almacenado devuelve el subconjunto de datos correspondiente a la capa BLL, que lo devuelve a ObjectDataSource; estos datos, a continuación, están enlazados a GridView, representar en HTML y envía al usuario final

La figura 7 muestra la primera página de resultados cuando se ordenan por el `UnitPrice` en orden ascendente.


[![Los resultados se ordenan por el precio unitario](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: los resultados se ordenan por el precio unitario ([haga clic aquí para ver imagen en tamaño completo](sorting-custom-paged-data-cs/_static/image11.png))


Mientras que la implementación actual correctamente puede ordenar los resultados por nombre de producto, nombre de categoría, cantidad por unidad y el precio unitario, intenta ordenar los resultados por el proveedor de nombre tiene como resultado una excepción en tiempo de ejecución (consulte la figura 8).


![Al intentar ordenar los resultados por los resultados del proveedor en la siguiente excepción en tiempo de ejecución](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: intentar ordenar los resultados por los resultados del proveedor en la siguiente excepción en tiempo de ejecución


Esta excepción se produce porque el `SortExpression` la s GridView `SupplierName` BoundField está establecido en `SupplierName`. Sin embargo, el proveedor s nombre de la `Suppliers` realmente se denomina tabla `CompanyName` hemos sido un alias como nombre de la columna `SupplierName`. Sin embargo, el `OVER` cláusula utilizado por el `ROW_NUMBER()` no se puede usar el alias de función y debe usar el nombre de columna real. Por lo tanto, cambie el `SupplierName` BoundField s `SortExpression` desde NombreProveedor en CompanyName (consulte la figura 9). Como se muestra en la figura 10, después de este cambio que se pueden ordenar los resultados por el proveedor.


![Cambie la expresión de ordenación de s NombreProveedor BoundField a CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: cambiar la expresión de ordenación de s NombreProveedor BoundField a CompanyName


[![Ahora se pueden ordenar los resultados por proveedor](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: los resultados ahora se pueden ordenar por proveedor ([haga clic aquí para ver imagen en tamaño completo](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Resumen

La implementación de paginación personalizada que hemos visto en el tutorial anterior requiere que se especifica el orden en que fueron los resultados se ordenen en tiempo de diseño. En resumen, esto significaba que la implementación de paginación personalizada que implementamos no pudo, al mismo tiempo, proporcionar las funcionalidades de ordenación. En este tutorial hemos superado esta limitación mediante la extensión del procedimiento almacenado desde la primera para incluir un `@sortExpression` parámetro de entrada mediante el cual se puede ordenar los resultados.

Después de crear este procedimiento almacenado y crear nuevos métodos en la capa DAL y BLL, estábamos capaz de implementar un control GridView que ofrece tanto la ordenación y personalizados de paginación mediante la configuración de ObjectDataSource para pasar la s GridView actual `SortExpression` propiedad a la capa BLL `SelectMethod`.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Santos Carlos. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Siguiente](creating-a-customized-sorting-user-interface-cs.md)
