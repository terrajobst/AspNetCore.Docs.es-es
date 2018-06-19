---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Uso de consultas parametrizadas con SqlDataSource (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial, se continuar nuestro aspecto en el control SqlDataSource y obtenga información acerca de cómo definir consultas con parámetros. Los parámetros se pueden especificar ambos decla...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 87abf3029919eb9e3c2c931abfb4beb0b2f92fdb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878196"
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Uso de consultas parametrizadas con SqlDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) o [descarga de PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> En este tutorial, se continuar nuestro aspecto en el control SqlDataSource y obtenga información acerca de cómo definir consultas con parámetros. Los parámetros se pueden especificar mediante declaración y mediante programación y pueden extraerse de un número de ubicaciones, como la cadena de consulta, estado Session, otros controles y mucho más.


## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos visto cómo usar el control SqlDataSource para recuperar los datos directamente desde una base de datos. Mediante el Asistente para configurar orígenes de datos, podríamos seleccionar la base de datos y, a continuación,: elegir las columnas que se va a devolver desde una tabla o vista; Escriba una instrucción SQL personalizada; o bien, utilice un procedimiento almacenado. Si selecciona las columnas de una tabla o vista, o escribir una instrucción SQL personalizada, SqlDataSource control s `SelectCommand` propiedad se asigna el código SQL ad hoc resultante `SELECT` instrucción y es esto `SELECT` instrucción que se ejecuta cuando el SqlDataSource s `Select()` se invoca el método (ya sea mediante programación o automáticamente desde un control Web de datos).

La instrucción SQL `SELECT` instrucciones que se utilizan en las demostraciones anteriores de tutorial s carecían `WHERE` cláusulas. En un `SELECT` (instrucción), el `WHERE` cláusula puede utilizarse para limitar los resultados devueltos. Por ejemplo, para mostrar los nombres de productos que cuestan más de 50,00 $, podríamos usar la siguiente consulta:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Normalmente, los valores utilizados en un `WHERE` cláusula se determinan por algún origen externo, como un valor de cadena de consulta, una variable de sesión o proporcionados por el usuario de un control Web en la página. Idealmente, estas entradas se especifican mediante el uso de *parámetros*. Con Microsoft SQL Server, los parámetros se denotan con `@parameterName`, como en:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource admite consultas con parámetros, tanto para `SELECT` instrucciones y `INSERT`, `UPDATE`, y `DELETE` las instrucciones. Además, los valores de parámetro pueden automáticamente extraerse de diversos orígenes de la cadena de consulta, estado de sesión, los controles en la página y así sucesivamente o se pueden asignar mediante programación. En este tutorial, veremos cómo definir las consultas con parámetros, así como el modo para especificar el parámetro de valores mediante declaración y mediante programación.

> [!NOTE]
> En el tutorial anterior se compara el ObjectDataSource que ha sido nuestra herramienta de elección durante los primeros 46 tutoriales con SqlDataSource, teniendo en cuenta sus semejanzas conceptuales. Estas similitudes también se extienden a parámetros. Los parámetros de s ObjectDataSource asignados a los parámetros de entrada para los métodos en la capa de lógica de negocios. Con SqlDataSource, los parámetros se definen directamente dentro de la consulta SQL. Ambos controles tienen las colecciones de parámetros para su `Select()`, `Insert()`, `Update()`, y `Delete()` métodos y ambos pueden tener estos valores de parámetro que se rellena a partir de orígenes predefinidos (valores de cadena de consulta, variables de sesión etc. ) o tener asignados mediante programación.


## <a name="creating-a-parameterized-query"></a>Crear una consulta parametrizada

El Asistente para configurar origen de datos de SqlDataSource control s ofrece tres modos para definir el comando ejecutar para recuperar registros de la base de datos:

- Al seleccionar las columnas de una tabla o vista existente,
- Si escribe una instrucción SQL personalizada, o
- Si elige un procedimiento almacenado

Al seleccionar las columnas de una tabla existente o vista, los parámetros para la `WHERE` cláusula debe especificarse mediante el complemento `WHERE` cuadro de diálogo de la cláusula. Al crear una instrucción SQL personalizada, sin embargo, puede especificar los parámetros directamente en el `WHERE` cláusula (mediante `@parameterName` para indicar cada parámetro). A [procedimiento almacenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) consta de una o varias instrucciones SQL, y se pueden parametrizar estas instrucciones. Los parámetros utilizados en las instrucciones SQL, sin embargo, deben pasarse en como parámetros de entrada para el procedimiento almacenado.

Puesto que la creación de una consulta parametrizada depende de cómo las operaciones de asignación SqlDataSource `SelectCommand` es s especificado, deje que eche un vistazo a las tres enfoques. Para empezar, abra el `ParameterizedQueries.aspx` página en el `SqlDataSource` carpeta, arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador y establezca su `ID` a `Products25BucksAndUnderDataSource`. A continuación, haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente de control s. Seleccione la base de datos (`NORTHWINDConnectionString`) y haga clic en siguiente.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Paso 1: Agregar un`WHERE`cláusula al seleccionar las columnas de una tabla o vista

Al seleccionar los datos que se va a devolver de la base de datos con el control SqlDataSource, el Asistente para configurar orígenes de datos nos permite seleccionar simplemente las columnas que se devuelven de una tabla existente o ver (consulte la figura 1). Al hacerlo, automáticamente crea una instancia de SQL `SELECT` instrucción, que es lo que se envía a la base de datos cuando las operaciones de asignación SqlDataSource `Select()` se invoca el método. Como hicimos en el tutorial anterior, en la lista desplegable, seleccione la tabla Products y compruebe el `ProductID`, `ProductName`, y `UnitPrice` columnas.


[![Seleccionar las columnas que se va a devolver desde una tabla o vista](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: elegir las columnas para la devolución de una tabla o vista ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Para incluir un `WHERE` cláusula en la `SELECT` (instrucción), haga clic en el `WHERE` botón, que abrirá el complemento `WHERE` cuadro de diálogo de la cláusula (consulte la figura 2). Para agregar un parámetro para limitar los resultados devueltos por la `SELECT` de consulta, en primer lugar, elija la columna que se va a filtrar los datos por. A continuación, elija el operador que se utiliza para filtrar (=, &lt;, &lt;=, &gt;, y así sucesivamente). Por último, elija el origen del valor del parámetro, como desde el estado de sesión o de cadena de consulta. Después de configurar el parámetro, haga clic en el botón Agregar para incluirlo en la `SELECT` consulta.

En este ejemplo, s permiten devolver sólo esos resultados donde la `UnitPrice` valor es menor o igual que 25,00 $. Por lo tanto, elegir `UnitPrice` desde la lista desplegable de columnas y &lt;= desde la lista desplegable operador. Cuando se usa un valor de parámetro codificado de forma rígida (por ejemplo, $25,00) o si el valor del parámetro debe especificarse mediante programación, seleccione Ninguno en la lista desplegable de origen. A continuación, escriba el valor del parámetro codificado de forma rígida en el cuadro de texto valor 25,00 y completar el proceso, haga clic en el botón Agregar.


[![Limitar los resultados devueltos por el agregar WHERE cláusula cuadro de diálogo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figura 2**: limitar los resultados se devuelven desde el complemento `WHERE` cuadro de diálogo de la cláusula ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Después de agregar el parámetro, haga clic en Aceptar para volver al Asistente para configurar origen de datos. El `SELECT` ahora debe incluir la instrucción en la parte inferior del Asistente para un `WHERE` cláusula con un parámetro denominado `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Si se especifican varias condiciones en el `WHERE` cláusula desde el complemento `WHERE` cuadro de diálogo de cláusula, el asistente los combina con la `AND` operador. Si tiene que incluir un `OR` en el `WHERE` cláusula (como `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), a continuación, tiene que generar el `SELECT` instrucción a través de la pantalla de instrucción SQL personalizada.


Completar la configuración SqlDataSource (haga clic en siguiente, a continuación, finalizar) y, a continuación, inspeccione el marcado declarativo de SqlDataSource s. El marcado incluye ahora un `<SelectParameters>` recolección, lo que pone de relieve los orígenes de los parámetros de la `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Cuando las operaciones de asignación SqlDataSource `Select()` se invoca el método, el `UnitPrice` (25,00) el valor del parámetro se aplica a la `@UnitPrice` parámetro en la `SelectCommand` antes de enviarse a la base de datos. El resultado neto es que solo los productos menor o igual que 25,00 $ se devuelven desde el `Products` tabla. Para confirmar esto, agregue un control GridView a la página, enlazar a este origen de datos y, a continuación, ver la página a través de un explorador. Sólo debería ver los productos de la lista que tengan menor o igual que 25,00 $, confirma la figura 3.


[![Se muestran solo los productos es menor o igual que 25,00 $](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figura 3**: se muestran solo los productos es menor o igual que 25,00 $ ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Paso 2: Agregar parámetros a una instrucción SQL personalizada

Al agregar una instrucción SQL personalizada puede escribir el `WHERE` cláusula explícitamente o especifique un valor en la celda de filtro del generador de consultas. Para demostrar esto, permiten s Mostrar solo los productos en un GridView cuyos precios son inferiores a un umbral determinado. Empiece agregando un cuadro de texto a la `ParameterizedQueries.aspx` página para recopilar este valor de umbral del usuario. Establezca el cuadro de texto s `ID` propiedad `MaxPrice`. Agregue un control Web de botón y establezca su `Text` propiedad para mostrar productos de búsqueda de coincidencias.

A continuación, arrastre un control GridView a la página y en la etiqueta inteligente de optar por crear un nuevo SqlDataSource denominado `ProductsFilteredByPriceDataSource`. Desde el Asistente para configurar origen de datos, continúe para especificar una instrucción SQL personalizada o la pantalla de procedimiento almacenado (consulte la figura 4) y escriba la siguiente consulta:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Después de escribir la consulta (ya sea manualmente o mediante el generador de consultas), haga clic en siguiente.


[![Devolver solo los productos menor o igual que un valor de parámetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figura 4**: devolver solo los productos es menor o igual que un valor de parámetro ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Dado que la consulta incluye parámetros, la siguiente pantalla del asistente nos solicita el origen de los valores de parámetros. Elija el Control de la lista desplegable de origen de parámetro y `MaxPrice` (el control de cuadro de texto s `ID` valor) en la lista desplegable de ControlID. También puede especificar un valor predeterminado opcional para usarlo en el caso donde el usuario no ha escrito ningún texto en la `MaxPrice` cuadro de texto. Por el momento, no escriba un valor predeterminado.


[![Las operaciones de asignación MaxPrice TextBox que Text (propiedad) se utiliza como origen del parámetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figura 5**: el `MaxPrice` s de cuadro de texto `Text` propiedad se utiliza como origen del parámetro ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Complete el Asistente para configurar orígenes de datos haciendo clic en siguiente y, a continuación, finalice. A continuación se muestra el marcado declarativo de GridView, cuadro de texto, botón y SqlDataSource:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Tenga en cuenta que el parámetro dentro de las operaciones de asignación SqlDataSource `<SelectParameters>` sección es un `ControlParameter`, lo que incluye propiedades adicionales como `ControlID` y `PropertyName`. Cuando las operaciones de asignación SqlDataSource `Select()` se invoca el método, el `ControlParameter` toma el valor de la propiedad de control Web especificado y lo asigna al parámetro correspondiente en el `SelectCommand`. En este ejemplo, el `MaxPrice` Text (propiedad) se usa como el `@MaxPrice` el valor del parámetro.

Dedique un minuto a ver esta página a través de un explorador. Cuando visita primero a la página o cada vez que la `MaxPrice` cuadro de texto no tiene un valor no se muestran registros en GridView.


[![No hay registros son que muestra cuando el MaxPrice cuadro de texto está vacío](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figura 6**: sin registros son aparece cuando la `MaxPrice` cuadro de texto está vacío ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


La razón no productos se muestran es que, de forma predeterminada, una cadena vacía para un valor de parámetro se convierte en una base de datos `NULL` valor. Desde la comparación de `[UnitPrice] <= NULL` siempre se evalúa como False, se devuelve ningún resultado.

Escriba un valor en el cuadro de texto, como 5.00 y haga clic en el botón Mostrar productos de búsqueda de coincidencias. En la devolución, SqlDataSource informa a que GridView que uno de sus orígenes de parámetro ha cambiado. Por lo tanto, el control GridView vuelve a enlazar el SqlDataSource, mostrar los productos menor o igual a 5,00 USD.


[![Se muestran los productos es menor o igual a 5,00 USD](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figura 7**: se muestran los productos es menor o igual a 5,00 USD ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Mostrar inicialmente todos los productos

En lugar de no mostrar ningún producto cuando se carga la página por primera vez, que podríamos desear mostrar *todos los* productos. Una manera de mostrar todos los productos cada vez que la `MaxPrice` cuadro de texto está vacío es establecer el valor del parámetro s predeterminado en algún valor alucinantemente alta, como 1000000, porque s improbable que Northwind Traders nunca habrá inventario cuyo precio supera a 1.000.000 dólares. Sin embargo, este enfoque es limitada y podría no funcionar en otras situaciones.

En los tutoriales anteriores - [parámetros declarativos](../basic-reporting/declarative-parameters-cs.md) y [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) que nos enfrentamos con un problema similar. Nuestra solución existe era colocar esta lógica en la capa de lógica de negocios. En concreto, la capa BLL examina el valor entrante y, si fue `NULL` o algunos valor reservado, la llamada se enruta al método DAL que devuelve todos los registros. Si el valor de entrada es un valor de filtrado normal, se realiza una llamada al método DAL que ejecuta una instrucción SQL que usa un con parámetros `WHERE` cláusula con el valor proporcionado.

Desafortunadamente, la arquitectura se omiten cuando se usa el SqlDataSource. En su lugar, es necesario personalizar la instrucción SQL que cambien agarre todos los registros si la `@MaximumPrice` parámetro es `NULL` o algún valor reservado. Para este ejercicio, s permiten tener, por lo que ese if el `@MaximumPrice` es igual al parámetro `-1.0`, a continuación, *todos los* de los registros que se van a devolverse (`-1.0` funciona como un valor reservado, ya que ningún producto puede tener un negativo `UnitPrice`valor). Para lograr esto podemos utilizar la siguiente instrucción SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Esto `WHERE` cláusula devuelve *todos los* registra si la `@MaximumPrice` parámetro es igual a `-1.0`. Si el valor del parámetro no es `-1.0`, solo los productos cuyo `UnitPrice` es menor o igual que el `@MaximumPrice` se devuelven el valor del parámetro. Estableciendo el valor predeterminado de la `@MaximumPrice` parámetro `-1.0`, en la primera carga de página (o cada vez que la `MaxPrice` cuadro de texto está vacío), `@MaximumPrice` tendrá un valor de `-1.0` y se mostrarán todos los productos.


[![Ahora todos los productos están muestra cuando el MaxPrice cuadro de texto está vacío](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figura 8**: ahora todos los productos están aparece cuando la `MaxPrice` cuadro de texto está vacío ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Hay un par de avisos que hay tener en cuenta con este enfoque. En primer lugar, tenga en cuenta que se deduce el tipo de datos de parámetro s uso s en la consulta SQL. Si cambia la `WHERE` cláusula de `@MaximumPrice = -1.0` a `@MaximumPrice = -1`, el tiempo de ejecución trata el parámetro como un entero. Si intenta asignar el `MaxPrice` cuadro de texto en un valor decimal (por ejemplo, 5,00), se producirá un error porque no se puede convertir 5.00 en un entero. Para solucionar esto, asegúrese de que usas `@MaximumPrice = -1.0` en el `WHERE` cláusula o, mejor aún, establezca el `ControlParameter` objeto s `Type` propiedad en un valor Decimal.

En segundo lugar, agregando la `OR @MaximumPrice = -1.0` a la `WHERE` cláusula, el motor de consultas no puede utilizar un índice en `UnitPrice` (suponiendo que exista uno), lo que resulta en un recorrido de tabla. Puede afectar al rendimiento si hay un número suficiente de registros en la `Products` tabla. Un enfoque más adecuado sería mover esta lógica a un procedimiento almacenado donde un `IF` instrucción podría realizar un `SELECT` consultar en el `Products` tabla sin un `WHERE` cláusula cuando necesitan devolver todos los registros o uno cuyo `WHERE` cláusula contiene solo el `UnitPrice` criterios, por lo que puede usarse un índice.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Paso 3: Crear y utilizar procedimientos almacenados parametrizados

Los procedimientos almacenados pueden incluir un conjunto de parámetros de entrada que se pueden usar en las instrucciones de SQL definidas dentro del procedimiento almacenado. Al configurar el SqlDataSource para utilizar un procedimiento almacenado que acepta parámetros de entrada, pueden especificar estos valores de parámetro con las mismas técnicas como instrucciones SQL ad hoc.

Para ilustrar el uso de procedimientos almacenados en SqlDataSource, s permiten crear un nuevo procedimiento almacenado en la base de datos de Northwind denominado `GetProductsByCategory`, que acepta un parámetro denominado `@CategoryID` y devuelve todas las columnas de los productos cuyos `CategoryID` coincide con la columna `@CategoryID`. Para crear un procedimiento almacenado, vaya al explorador de servidores y explorar en profundidad el `NORTHWND.MDF` base de datos. (Si don t ve el Explorador de servidores, mostrar, ir al menú Ver y seleccionar la opción de explorador de servidores.)

Desde el `NORTHWND.MDF` la base de datos, haga doble clic en la carpeta procedimientos almacenados, elija Agregar nuevo procedimiento almacenado y escriba la siguiente sintaxis:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Haga clic en el icono de guardar (o Ctrl + S) para guardar el procedimiento almacenado. Puede probar el procedimiento almacenado, con el botón secundario en la carpeta procedimientos almacenados y elija ejecutar. Se le solicitará los parámetros de procedimiento almacenado s (`@CategoryID`, en este caso), después de que los resultados se mostrarán en la ventana de salida.


[![El GetProductsByCategory almacenados procedimiento cuando se ejecuta un @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figura 9**: el `GetProductsByCategory` procedimiento almacenado cuando se ejecuta con una `@CategoryID` de 1 ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Permiten s usar este procedimiento almacenado para mostrar todos los productos de la categoría de bebidas en un control GridView. Agregar un control GridView nuevo a la página y enlazarla a un nuevo SqlDataSource denominado `BeverageProductsDataSource`. Continuar para especificar una instrucción SQL personalizada o la pantalla de procedimiento almacenado, seleccionar el botón de opción de procedimiento almacenado y seleccione el `GetProductsByCategory` el procedimiento almacenado de la lista desplegable.


[![Seleccione el GetProductsByCategory el procedimiento almacenado de la lista desplegable](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figura 10**: seleccione la `GetProductsByCategory` procedimiento almacenado en la lista desplegable ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Puesto que el procedimiento almacenado acepta un parámetro de entrada (`@CategoryID`), haga clic en siguiente le pide que especifique el origen para este valor de parámetro s. Las bebidas `CategoryID` es 1, por lo tanto, deje la lista desplegable de origen de parámetro en ninguno y escriba 1 en el cuadro de texto DefaultValue.


[![Utilice un valor codificado de forma rígida de 1 para devolver los productos de la categoría de bebidas](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figura 11**: utilice un valor de Hard-Coded de 1 para devolver los productos de la categoría bebidas ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Como muestra el siguiente marcado declarativo, cuando se utiliza un procedimiento almacenado, las operaciones de asignación SqlDataSource `SelectCommand` propiedad se establece en el nombre del procedimiento almacenado y el [ `SelectCommandType` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) se establece en `StoredProcedure`, que indica que que el `SelectCommand` es el nombre de un procedimiento almacenado en lugar de una instrucción de SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Probar la página en un explorador. Se muestran solo los productos que pertenecen a la categoría Bebidas, aunque *todos los* del producto se muestran los campos desde el `GetProductsByCategory` procedimiento almacenado devuelve todas las columnas de la `Products` tabla. Por supuesto, se puede limitar o personalizar los campos que aparecen en el control GridView desde el cuadro de diálogo Editar columnas GridView.


[![Se mostrarán todas las bebidas](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figura 12**: se mostrarán todas las bebidas ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Paso 4: Invocar mediante programación un s SqlDataSource`Select()`instrucción

Los ejemplos se ha visto hasta ahora en el tutorial anterior y en este tutorial enlazar los controles SqlDataSource directamente a un control GridView. Los datos del control s SqlDataSource, sin embargo, pueden obtener mediante programación acceso y enumerados en el código. Esto puede ser especialmente útil cuando necesite consultar los datos para inspeccionar, pero necesita don t para mostrarla. En lugar de tener que escribir todo el código de ADO.NET para conectarse a la base de datos, especifique el comando y recuperar los resultados de reutilizable, puede dejar que el SqlDataSource gestione este código más monótonas.

Para ilustrar el trabajo con la opción s SqlDataSource datos mediante programación, imagine que su jefe ha abordado con una solicitud para crear una página web que muestra el nombre de una categoría seleccionada de forma aleatoria y sus productos asociados. Es decir, cuando un usuario visita esta página, queremos que elija aleatoriamente una categoría de la `Categories` de la tabla, mostrar el nombre de categoría y, a continuación, enumera los productos que pertenecen a esa categoría.

Para ello se necesitan dos controles de SqlDataSource uno para tomar una categoría aleatoria de la `Categories` tabla y otra para obtener la categoría de productos de s. Vamos a crear la SqlDataSource que recupera un registro de la categoría aleatorio en este paso; Paso 5 se examina elaborar SqlDataSource que recupera los productos de s de categoría.

Empiece agregando un SqlDataSource a `ParameterizedQueries.aspx` y establecer su `ID` a `RandomCategoryDataSource`. Configúrelo para que utilice la siguiente consulta SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` Devuelve los registros ordenados en orden aleatorio (vea [mediante `NEWID()` para ordenar los registros de forma aleatoria](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Devuelve el primer registro del conjunto de resultados. Reunir, esta consulta devuelve el `CategoryID` y `CategoryName` valores de columna de una categoría única, seleccionado aleatoriamente.

Para mostrar la categoría s `CategoryName` valor, agregue un control Web Label a la página, establezca su `ID` propiedad `CategoryNameLabel`y borrar su `Text` propiedad. Para recuperar los datos de un control SqlDataSource mediante programación, se debe invocar su `Select()` método. El [ `Select()` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) espera un parámetro de entrada de tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), que especifica cómo se deberían mensajes los datos antes de devolverse. Esto puede incluir instrucciones en Ordenar y filtrar los datos y se utiliza por los datos de que controles Web al ordenar o paginar a través de los datos desde un control SqlDataSource. En nuestro ejemplo, sin embargo, se don necesidad de t los datos pueden modificar antes de que se devuelven y pasará por lo tanto, en la `DataSourceSelectArguments.Empty` objeto.

El `Select()` método devuelve un objeto que implementa `IEnumerable`. El tipo preciso devuelto depende del valor del control SqlDataSource s [ `DataSourceMode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Como se describe en el tutorial anterior, esta propiedad puede establecerse en un valor de `DataSet` o `DataReader`. Si establece en `DataSet`, `Select()` método devuelve un [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) objeto; si se establece en `DataReader`, devuelve un objeto que implementa [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Puesto que la `RandomCategoryDataSource` SqlDataSource tiene su `DataSourceMode` propiedad establecida en `DataSet` (valor predeterminado), se va a trabajar con un objeto DataView.

El código siguiente muestra cómo recuperar los registros de la `RandomCategoryDataSource` SqlDataSource como un objeto DataView, así como cómo leer la `CategoryName` valor de la columna de la primera fila de DataView:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` Devuelve el primer `DataRowView` en DataView. `randomCategoryView[0]["CategoryName"]` Devuelve el valor de la `CategoryName` columna en esta primera fila. Tenga en cuenta que la propiedad DataView es imprecisa. Para hacer referencia a un valor de columna en particular, es necesario pasar el nombre de la columna como una cadena (CategoryName, en este caso). Figura 13 muestra el mensaje mostrado en el `CategoryNameLabel` al ver la página. Por supuesto, el nombre de categoría real mostrado se selecciona aleatoriamente por el `RandomCategoryDataSource` SqlDataSource en cada visita a la página (incluidas las devoluciones de datos).


[![Se muestra el nombre de las operaciones de asignación seleccionado aleatoriamente de categoría](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figura 13**: s el aleatoriamente categoría seleccionada se muestra el nombre ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Si el SqlDataSource control s `DataSourceMode` propiedad hubiera establecido en `DataReader`, el valor devuelto de la `Select()` método hubiera necesitado convertirse al `IDataReader`. Para leer la `CategoryName` valor de la columna de la primera fila, se d use código como:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Con SqlDataSource aleatoriamente si selecciona una categoría, se re listo para agregar el control GridView que muestra los productos de la categoría s.

> [!NOTE]
> En lugar de usar un control Web Label para mostrar el nombre de categoría s, se pudo agregar una FormView o DetailsView a la página, enlazarlo a SqlDataSource. Sin embargo, con la etiqueta, permite que explorar cómo invocar mediante programación la s SqlDataSource `Select()` instrucción y trabajar con sus datos resultantes en el código.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Paso 5: Asignar valores de parámetro mediante programación

Todos los ejemplos se ha visto hasta ahora en este tutorial ha utilizado un valor de parámetro codificado de forma rígida o uno procedente de uno de los orígenes de parámetro predefinidos (un valor de cadena de consulta, un control Web en la página y así sucesivamente). Sin embargo, los parámetros de s control SqlDataSource también pueden establecerse mediante programación. Para completar nuestro ejemplo actual, se necesita un SqlDataSource que devuelve todos los productos que pertenecen a una categoría especificada. Tendrá este SqlDataSource un `CategoryID` parámetro cuyo valor debe establecerse basado en la `CategoryID` valor de la columna devuelta por la `RandomCategoryDataSource` SqlDataSource en el `Page_Load` controlador de eventos.

Empiece agregando un control GridView a la página y enlazarla a un nuevo SqlDataSource denominado `ProductsByCategoryDataSource`. Mucho como hicimos en el paso 3, configure el SqlDataSource para que invoca el `GetProductsByCategory` procedimiento almacenado. Deje el parámetro establecido de lista desplegable de origen en None, pero no se especifique un valor predeterminado, tal y como se establecerá el valor predeterminado mediante programación.


[![No se especifica un origen del parámetro o valor predeterminado](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Figura 14**: no se especifica un origen del parámetro o el valor predeterminado ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Después de completar al Asistente SqlDataSource el marcado declarativo resultante debe ser similar al siguiente:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Se puede asignar la `DefaultValue` de la `CategoryID` parámetro mediante programación en el `Page_Load` controlador de eventos:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Con esta adición, la página incluye un control GridView que muestra los productos asociados con la categoría seleccionada al azar.


[![No se especifica un origen del parámetro o valor predeterminado](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figura 15**: no se especifica un origen del parámetro o el valor predeterminado ([haga clic aquí para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Resumen

SqlDataSource permite a los programadores de la página Definir consultas con parámetros cuyos valores de parámetros pueden codificado de forma rígida, extraídos de orígenes de parámetros predefinidos o asignados mediante programación. En este tutorial, hemos visto cómo crear una consulta con parámetros desde el asistente Configurar origen de datos para realizar consultas SQL ad hoc y procedimientos almacenados. También explicamos uso de orígenes de parámetro codificado de forma rígida, un control Web como un origen del parámetro y especificar mediante programación el valor del parámetro.

Al igual que con el ObjectDataSource SqlDataSource también proporciona capacidades para modificar los datos subyacentes. En el siguiente tutorial analizaremos cómo definir `INSERT`, `UPDATE`, y `DELETE` instrucciones con SqlDataSource. Una vez que se han agregado estas instrucciones, se puede aprovechar la integrada Insertar, modificar y eliminar funciones inherentes a los controles GridView, DetailsView y FormView.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Scott Clyde, Randell Schmidt y Ken Pespisa. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](querying-data-with-the-sqldatasource-control-cs.md)
> [Siguiente](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
