---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Crear procedimientos almacenados para los TableAdapters del conjunto de datos con tipo (C#) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores hemos creado instrucciones SQL en nuestro código y pasa las instrucciones a la base de datos que se ejecutará. Un enfoque alternativo es usar s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: c17cc0bd6cb97ddf21e2f8d6b177b7f97a28575d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368776"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Crear procedimientos almacenados para los TableAdapters del conjunto de datos con tipo (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) o [descargar PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> En los tutoriales anteriores hemos creado instrucciones SQL en nuestro código y pasa las instrucciones a la base de datos que se ejecutará. Un enfoque alternativo es usar los procedimientos almacenados, donde las instrucciones SQL están predefinidas en la base de datos. En este tutorial se aprenderá cómo hacer que el Asistente de TableAdapter para generar nuevos procedimientos almacenados para nosotros.


## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) para estos tutoriales usan conjuntos de datos con tipo. Como se describe en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial, los conjuntos de datos con tipo constan de tablas de datos fuertemente tipados y los TableAdapters. DataTables representan las entidades lógicas definidas en el sistema mientras la interfaz de los TableAdapters con la base de datos subyacente para realizar el trabajo de acceso de datos. Esto incluye rellenar las tablas de datos con datos, ejecutar consultas que devuelven datos escalares, insertar, actualizar y eliminación de registros de la base de datos.

Los comandos SQL ejecutados por los TableAdapters pueden ser cualquier instrucciones SQL ad hoc, como `SELECT columnList FROM TableName`, o los procedimientos almacenados. Los TableAdapters en nuestra arquitectura usar instrucciones SQL ad hoc. Muchos desarrolladores y administradores de base de datos, sin embargo, prefieren los procedimientos almacenados a través de las instrucciones SQL ad hoc por motivos de seguridad, mantenimiento y la capacidad de actualización. Ardently que otros prefieren las instrucciones SQL ad hoc para su flexibilidad. En mi propio trabajo me dé prioridad a los procedimientos almacenados a través de instrucciones SQL ad-hoc, pero eligió usar instrucciones SQL ad hoc para simplificar los tutoriales anteriores.

Al definir un TableAdapter o agregar nuevos métodos, el Asistente para la s de TableAdapter hace igual de fácil crear nuevos procedimientos almacenados o utilizar procedimientos almacenados existentes como lo hace para usar instrucciones SQL ad hoc. En este tutorial, examinaremos cómo hacer que el Asistente para la s de TableAdapter generación automática de procedimientos almacenados. En el siguiente tutorial veremos cómo configurar los métodos de TableAdapter s para utilizar procedimientos almacenados existentes o crear manualmente.

> [!NOTE]
> Consulte la entrada de blog de Rob Howard [no procedimientos de almacenados de uso aún?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) y [Frans Bouma](https://weblogs.asp.net/fbouma/) entrada de blog de s [procedimientos almacenados son malas, Kay M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para un debate sobre las ventajas y desventajas de animada procedimientos almacenados y SQL ad hoc.


## <a name="stored-procedure-basics"></a>Conceptos básicos de procedimiento almacenado

Las funciones son una construcción común a todos los lenguajes de programación. Una función es una colección de instrucciones que se ejecutan cuando se llama a la función. Funciones pueden aceptar parámetros de entrada y, opcionalmente, pueden devolver un valor. *[Procedimientos almacenados](http://en.wikipedia.org/wiki/Stored_procedure)*  son construcciones de base de datos que comparten muchas similitudes con las funciones en lenguajes de programación. Un procedimiento almacenado se compone de un conjunto de instrucciones de Transact-SQL que se ejecutan cuando se llama al procedimiento almacenado. Un procedimiento almacenado puede aceptar cero a muchos parámetros de entrada y puede devolver valores escalares, los parámetros de salida, o, más comúnmente, conjuntos de resultados de `SELECT` las consultas.

> [!NOTE]
> Los procedimientos almacenados a menudo se conocen como procedimientos almacenados o SPs.


Los procedimientos almacenados se crean mediante el [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instrucción T-SQL. Por ejemplo, el siguiente script T-SQL crea un procedimiento almacenado denominado `GetProductsByCategoryID` que toma un solo parámetro denominado `@CategoryID` y devuelve el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` campos de esas columnas en el `Products` tabla que tiene la correspondiente `CategoryID` valor:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Una vez creado este procedimiento almacenado, se puede llamar mediante la sintaxis siguiente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> En el siguiente tutorial, examinaremos la creación de procedimientos almacenados mediante el IDE de Visual Studio. Para este tutorial, sin embargo, vamos a dejar que el Asistente de TableAdapter a generar automáticamente los procedimientos almacenados para nosotros.


Además de simplemente devolver datos, procedimientos almacenados a menudo se utilizan para ejecutar varios comandos de base de datos dentro del ámbito de una sola transacción. Un procedimiento almacenado denominado `DeleteCategory`, por ejemplo, puede que tarde en un `@CategoryID` parámetro y llevar a cabo dos `DELETE` instrucciones: primero, uno para eliminar los productos relacionados y un segundo al eliminar la categoría especificada. Son varias instrucciones dentro de un procedimiento almacenado *no* ajustado automáticamente dentro de una transacción. Comandos de T-SQL adicionales deben emitirse para garantizar el procedimiento almacenado s que varios comandos se tratan como una operación atómica. Veremos cómo ajustar comandos un procedimiento almacenado dentro del ámbito de una transacción en el tutorial posterior.

Al utilizar procedimientos almacenados dentro de una arquitectura, los métodos de capa de acceso a datos s invocan un procedimiento almacenado específico en lugar de emitir una instrucción de SQL ad hoc. Este modo centraliza la ubicación de las instrucciones SQL ejecutadas (en la base de datos) en lugar de tener que definidos dentro de la arquitectura de s de la aplicación. Esta centralización podría decirse que hace que sea más fácil de buscar, analizar y optimizar las consultas y proporciona una imagen mucho más clara sobre dónde y cómo se está usando la base de datos.

Para obtener más información sobre los aspectos básicos del procedimiento almacenado, consulte los recursos en la sección Lecturas adicionales al final de este tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Paso 1: Creación de las páginas Web de escenarios de capa de datos avanzados acceso

Antes de comenzar nuestro debate sobre la creación de un DAL mediante procedimientos almacenados, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitamos para éste y el siguientes varios tutoriales. Empiece por agregar una nueva carpeta denominada `AdvancedDAL`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Agregar las páginas ASP.NET para los tutoriales de escenarios de capa de acceso avanzada de datos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales de escenarios de capa de acceso avanzada de datos


Al igual que en las demás carpetas `Default.aspx` en el `AdvancedDAL` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Por último, agregue estas páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de trabajar con datos por lotes `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para los tutoriales de escenarios avanzados de DAL.


![El mapa del sitio incluye ahora entradas para los tutoriales de escenarios avanzados de DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Figura 3**: el mapa del sitio incluye ahora entradas para los tutoriales de escenarios avanzados de DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Paso 2: Configuración de un TableAdapter para crear nuevos procedimientos almacenados

Para demostrar la creación de una capa de acceso a datos que utilice procedimientos almacenados en lugar de instrucciones SQL ad hoc, s permiten crear un conjunto de datos de tipo nuevo en el `~/App_Code/DAL` carpeta denominada `NorthwindWithSprocs.xsd`. Puesto que nos hemos avanzado a través de este proceso en detalle en los tutoriales anteriores, se continuará rápidamente a través de los pasos siguientes. Si se bloquea o se necesitan más instrucciones paso a paso para crear y configurar un conjunto de datos con tipo, hacer referencia a la [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial.

Agregar un nuevo conjunto de datos al proyecto con el botón secundario en el `DAL` carpeta, elija Agregar nuevo elemento y seleccionar la plantilla de conjunto de datos como se muestra en la figura 4.


[![Agregar un nuevo conjunto de datos con tipo al proyecto denominado NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Figura 4**: agregar un nuevo conjunto de datos con tipo para el proyecto denominado `NorthwindWithSprocs.xsd` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Esto se crear el nuevo conjunto de datos con tipo, abra su diseñador, crear un nuevo TableAdapter e inicie al Asistente para configuración de TableAdapter. Asistente para configuración de TableAdapter s primero nos pide que seleccione la base de datos para trabajar con. La cadena de conexión a la base de datos de Northwind debe aparecer en la lista desplegable. Seleccione esta opción y haga clic en siguiente.

Podemos elegir cómo TableAdapter debe tener acceso a la base de datos desde esta pantalla siguiente. En los tutoriales anteriores, hemos seleccionado la primera opción, usar instrucciones SQL. Para este tutorial, seleccione la segunda opción, crear nuevos procedimientos almacenados y haga clic en siguiente.


[![Indicar el TableAdpater para crear nuevos procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Figura 5**: indicar el TableAdpater para crear nuevos procedimientos almacenados ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Al igual que con el uso de instrucciones SQL ad hoc, en el siguiente paso se nos pide que proporcione la `SELECT` instrucción para la consulta principal de TableAdapter s. Pero en lugar de usar el `SELECT` instrucción insertada aquí para realizar una consulta ad hoc directamente, el Asistente para la s de TableAdapter creará un procedimiento almacenado que contiene este `SELECT` consulta.

Use el siguiente `SELECT` consulta para este TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Escriba la consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Figura 6**: escriba el `SELECT` consulta ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> La consulta anterior difiere ligeramente de la consulta principal de la `ProductsTableAdapter` en el `Northwind` DataSet con tipo. Recuerde que el `ProductsTableAdapter` en el `Northwind` DataSet con tipo incluye dos subconsultas correlativas para devolver el nombre de categoría y el nombre de la empresa para cada categoría de producto s y el proveedor. En la próxima [actualizar TableAdapter para usar combinaciones](updating-the-tableadapter-to-use-joins-cs.md) datos relacionados con el tutorial veremos agregue lo siguiente a este TableAdapter.


Dedique un momento para hacer clic en el botón Opciones avanzadas. Desde aquí podemos especificar si el asistente también debe generar insert, update y delete instrucciones para el TableAdapter, si se debe usar la simultaneidad optimista, y si se debe actualizar la tabla de datos después de inserciones y actualizaciones. La opción Generar Insert, Update y Delete de instrucciones está activada de forma predeterminada. Deje activada. Para este tutorial, deje desactivada la usar opciones de simultaneidad optimista.

Cuando se tiene los procedimientos almacenados creados automáticamente por el Asistente de TableAdapter, parece que la actualización de la opción de tabla de datos se omite. Independientemente de si se activa esta casilla, el resultante insert y update procedimientos almacenados recuperan el registro recién insertado o actualizado just, como veremos en el paso 3.


![Deje activada la opción de las instrucciones de generar Insert, Update y Delete](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Figura 7**: deje activada la opción de las instrucciones de generar Insert, Update y Delete


> [!NOTE]
> Si está activada la opción de usar simultaneidad optimista, el asistente agregará condiciones adicionales a la `WHERE` cláusula que impiden la actualización si se produjeron cambios en otros campos de datos. Vuelva a consultar el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial para obtener más información sobre el uso de la característica de control de simultaneidad optimista integradas de s TableAdapter.


Después de escribir el `SELECT` consultar y confirmar que esté marcada la opción de instrucciones generar Insert, Update y Delete, haga clic en siguiente. Esta pantalla siguiente, que se muestra en la figura 8, le pide los nombres de los procedimientos almacenados que se creará el Asistente para seleccionar, insertar, actualizar y eliminar datos. Cambiar estos almacenan los nombres de procedimientos para `Products_Select`, `Products_Insert`, `Products_Update`, y `Products_Delete`.


[![Cambiar el nombre de los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 8**: cambiar el nombre de los procedimientos almacenados ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Para ver la utilizará el Asistente de TableAdapter para crear los cuatro procedimientos almacenados de Transact-SQL, haga clic en el botón de vista previa de Script SQL. En el cuadro de diálogo Vista previa de Script de SQL puede guardar el script en un archivo o copiarlo al Portapapeles.


![Obtener una vista previa del Script SQL usado para generar los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 9**: vista previa de la secuencia de comandos SQL que se usa para generar los procedimientos almacenados


Después de asignar nombres a los procedimientos almacenados, haga clic junto a los métodos correspondientes del nombre de la s de TableAdapter. Al igual que cuando usa instrucciones SQL ad hoc, podemos crear métodos para rellenar una DataTable existente o devuelven una nueva. También podemos especificar si el objeto TableAdapter debe incluir el patrón directos de la base de datos de inserción, actualización y eliminación de registros. Deje todas las tres casillas de verificación activadas, pero cambie el nombre de la devolución a un método de DataTable `GetProducts` (como se muestra en la figura 10).


[![Nombre de los métodos Fill y GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Figura 10**: el nombre de los métodos `Fill` y `GetProducts` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Haga clic en siguiente para ver un resumen de los pasos que se llevará a cabo el asistente. Complete el asistente, haga clic en el botón Finalizar. Una vez completado el asistente, se le devolverá al conjunto de datos s diseñador, que ahora debe incluir el `ProductsDataTable`.


[![El Diseñador de DataSet s muestra la ProductsDataTable recién agregada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Figura 11**: el conjunto de datos s diseñador muestra recién agregados `ProductsDataTable` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Paso 3: Examen de los procedimientos almacenados recién creados

El Asistente de TableAdapter utilizado automáticamente en el paso 2 crea los procedimientos almacenados para seleccionar, insertar, actualizar y eliminar datos. Estos procedimientos almacenados se pueden ver o modificar a través de Visual Studio en el Explorador de servidores y profundizar en la carpeta de base de datos s procedimientos almacenados. Como se muestra en la figura 12, la base de datos Northwind contiene cuatro nuevos procedimientos almacenados: `Products_Delete`, `Products_Insert`, `Products_Select`, y `Products_Update`.


![Los cuatro procedimientos almacenados que creó en el paso 2 se pueden encontrar en la carpeta de base de datos s procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Figura 12**: los cuatro procedimientos almacenados que creó en el paso 2 se pueden encontrar en la carpeta de base de datos s procedimientos almacenados


> [!NOTE]
> Si no ve el Explorador de servidores, vaya al menú Ver y elija la opción del explorador de servidores. Si no ve los procedimientos almacenados relacionados con productos agregados desde el paso 2, pruebe con el botón secundario en la carpeta procedimientos almacenados y elija actualiza.


Para ver o modificar un procedimiento almacenado, haga doble clic en su nombre en el Explorador de servidores o, alternativamente, haga doble clic en el procedimiento almacenado y elija Abrir. Figura 13 se muestra el `Products_Delete` procedimiento almacenado, cuando se abre.


[![Procedimientos almacenados se pueden abrir y modificar desde dentro de Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Figura 13**: almacena los procedimientos se pueden abrir y modificar desde dentro de Visual Studio ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


El contenido de ambas la `Products_Delete` y `Products_Select` procedimientos almacenados son bastante sencillos. El `Products_Insert` y `Products_Update` los procedimientos almacenados, por otro lado, garantizan una inspección más de cerca mientras ambos llevan a cabo una `SELECT` instrucción después de su `INSERT` y `UPDATE` instrucciones. Por ejemplo, el siguiente código SQL constituye la `Products_Insert` procedimiento almacenado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

El procedimiento almacenado acepta como parámetros de entrada la `Products` columnas devueltas por la `SELECT` consulta especificada en el Asistente para la s de TableAdapter y estos valores se usan en un `INSERT` instrucción. Siguiendo el `INSERT` instrucción, una `SELECT` consulta se usa para devolver el `Products` valores de columna (incluido el `ProductID`) del registro recién agregado. Esta capacidad de actualización es útil al agregar un nuevo registro con el patrón de actualización por lotes tal y como automáticamente actualizaciones recién agregada `ProductRow` instancias `ProductID` propiedades con los valores de incremento automático asignados por la base de datos.

El código siguiente muestra esta característica. Contiene un `ProductsTableAdapter` y `ProductsDataTable` creado para el `NorthwindWithSprocs` DataSet con tipo. Un nuevo producto se agrega a la base de datos mediante la creación de un `ProductsRow` instancia, proporcionando sus valores y llamar a TableAdapters `Update` método, pasando el `ProductsDataTable`. Internamente, los TableAdapter s `Update` método enumera la `ProductsRow` las instancias de la DataTable pasada (en este ejemplo hay solo uno: uno que acabamos de agregar) y realiza adecuado Insertar, actualizar o eliminar el comando. En este caso, el `Products_Insert` se ejecuta el procedimiento almacenado, que agrega un nuevo registro a la `Products` de tabla y devuelve los detalles del registro recién agregado. El `ProductsRow` instancia s `ProductID` , a continuación, se actualiza el valor. Después de la `Update` método ha finalizado, se puede acceder al registro recién agregado s `ProductID` valor a través de la `ProductsRow` s `ProductID` propiedad.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

El `Products_Update` procedimiento almacenado de forma similar incluye un `SELECT` instrucción después de su `UPDATE` instrucción.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Tenga en cuenta que este procedimiento almacenado incluye dos parámetros de entrada para `ProductID`: `@Original_ProductID` y `@ProductID`. Esta funcionalidad permite escenarios donde es posible que se puede cambiar la clave principal. Por ejemplo, en una base de datos de empleados, cada registro de empleado podría usar la seguridad empleado s social como su clave principal. Para cambiar una existente empleado s número del seguro social, deben proporcionar tanto el nuevo valor de la seguridad social y la original. Para el `Products` tabla, esta funcionalidad no es necesaria porque el `ProductID` columna es una `IDENTITY` columna y nunca deben cambiarse. De hecho, el `UPDATE` instrucción en el `Products_Update` t de procedimiento almacenado incluyen el `ProductID` columna en su lista de columnas. Por lo tanto, mientras `@Original_ProductID` se utiliza en el `UPDATE` instrucción s `WHERE` cláusula, es superflua para el `Products` de tabla y se reemplazarán por los `@ProductID` parámetro. Cuando se modifica un parámetros de procedimiento almacenado s es importante que también se actualizan los métodos TableAdapter que utilice el procedimiento almacenado.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Paso 4: Modificar un parámetros de procedimiento almacenado s y actualización de TableAdapter

Puesto que la `@Original_ProductID` parámetro es superfluo, permiten s. quítela de la `Products_Update` procedimiento almacenado por completo. Abra el `Products_Update` procedimiento almacenado, eliminar el `@Original_ProductID` parámetro y, en el `WHERE` cláusula de la `UPDATE` instrucción, cambiar el nombre de parámetro usado de `@Original_ProductID` a `@ProductID`. Después de realizar estos cambios, el T-SQL dentro del procedimiento almacenado debe ser similar al siguiente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Para guardar estos cambios en la base de datos, haga clic en el icono Guardar en la barra de herramientas o presione CTRL+s. En este momento, el `Products_Update` procedimiento almacenado no espera una `@Original_ProductID` parámetro de entrada, pero el TableAdapter está configurado para pasar un parámetro de este tipo. Puede ver los parámetros de TableAdapter se enviará a la `Products_Update` procedimiento almacenado mediante la selección de TableAdapter en el Diseñador de DataSet, vaya a la ventana Propiedades y haciendo clic en el botón de puntos suspensivos en el `UpdateCommand` s `Parameters` colección. Se abrirá el cuadro de diálogo Editor de la colección de parámetros que se muestra en la figura 14.


![Las listas de Editor de la colección de parámetros los parámetros utilizados se pasa a la Products_Update procedimiento almacenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Figura 14**: las listas de Editor de la colección de parámetros los parámetros utilizados se pasa a la `Products_Update` procedimiento almacenado


Puede quitar este parámetro desde aquí simplemente seleccionando la `@Original_ProductID` parámetro en la lista de miembros y haga clic en el botón Quitar.

Como alternativa, puede actualizar los parámetros utilizados para todos los métodos, con el botón secundario en TableAdapter en el diseñador y elija Configurar. Se abrirá el Asistente para configuración de TableAdapter, enumerar los procedimientos almacenados utilizados para seleccionar, insertar, actualizar y eliminar, junto con los parámetros de los procedimientos almacenados esperan recibir. Si hace clic en la lista desplegable de actualización puede ver el `Products_Update` procedimientos almacenados espera parámetros de entrada, que ahora ya no incluye `@Original_ProductID` (consulte la figura 15). Simplemente haga clic en Finalizar para actualizar automáticamente la colección de parámetros utilizada por el TableAdapter.


[![Como alternativa, puede usar al Asistente para configuración de TableAdapter s para sus colecciones de parámetros de métodos de actualización](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 15**: como alternativa, puede usar el Asistente para configuración para actualizar sus colecciones de parámetros de métodos de TableAdapter s ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Paso 5: Agregar métodos adicionales de TableAdapter

Como paso 2 se muestra, al crear un nuevo TableAdapter es fácil tener los procedimientos almacenados correspondientes que se genera automáticamente. Lo mismo sirve al agregar métodos adicionales a un TableAdapter. Para ilustrar esto, el s permiten agregar un `GetProductByProductID(productID)` método a la `ProductsTableAdapter` creado en el paso 2. Este método llevará como entrada un `ProductID` de valor y devolver detalles sobre el producto especificado.

Comience con el botón secundario en TableAdapter y elegir Agregar consulta en el menú contextual.


![Agregar una nueva consulta al TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 16**: agregar una nueva consulta al TableAdapter


Esta acción iniciará al Asistente de configuración de la consulta de TableAdapter, que le pide primero cómo debe tener acceso a la base de datos del TableAdapter. Para que un nuevo procedimiento almacenado que creó, elija el crear una nueva opción de procedimiento almacenado y haga clic en siguiente.


[![Elija crear un nuevo procedimiento almacenado, opción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Figura 17**: elija el crear un nueva opción de procedimiento almacenado ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


La siguiente pantalla nos pide identificar el tipo de consulta que se ejecutará, independientemente de que se devuelva un conjunto de filas o un valor escalar único o realizar una `UPDATE`, `INSERT`, o `DELETE` instrucción. Puesto que el `GetProductByProductID(productID)` método se devuelva una fila, deje el LECT que devuelve la opción de la fila seleccionada y presione siguiente.


[![Elija la selección que devuelve la fila, opción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Figura 18**: elija la selección que devuelve la opción de fila ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


La siguiente pantalla muestra la consulta principal s del TableAdapter, que solo muestra el nombre del procedimiento almacenado (`dbo.Products_Select`). Reemplácelo por el nombre del procedimiento almacenado siguiente `SELECT` instrucción, que devuelve todos los campos de producto para un producto determinado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Reemplace el nombre del procedimiento almacenado con una consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Figura 19**: reemplazar el nombre del procedimiento almacenado con un `SELECT` consulta ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


La pantalla posteriores pide que asigne un nombre de procedimiento almacenado que se va a crear. Escriba el nombre `Products_SelectByProductID` y haga clic en siguiente.


[![Nombre de la nueva Products_SelectByProductID de procedimiento almacenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 20**: nombre del nuevo procedimiento almacenado `Products_SelectByProductID` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


El último paso del asistente nos permite cambiar el método de nombres generan, así como indican si se debe usar el relleno de un patrón de DataTable, devolver un DataTable de patrón, o ambos. Para este método, deje ambas opciones activadas, pero cambie el nombre de los métodos para `FillByProductID` y `GetProductByProductID`. Haga clic en siguiente para ver un resumen de los pasos, el asistente realizará y, a continuación, haga clic en Finalizar para completar al asistente.


[![Cambiar el nombre de los métodos de TableAdapter s FillByProductID y GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Figura 21**: cambiar el nombre de los métodos de TableAdapter s a `FillByProductID` y `GetProductByProductID` ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Después de completar el asistente, lo TableAdapter dispone de un método nuevo, `GetProductByProductID(productID)` que, cuando se invoca, se ejecutará el `Products_SelectByProductID` el procedimiento almacenado que se acaba de crear. Dedique unos minutos a ver este nuevo procedimiento almacenado desde el Explorador de servidores, obtención de detalles de la carpeta procedimientos almacenados y abra `Products_SelectByProductID` (si no lo ve, haga doble clic en la carpeta procedimientos almacenados y elija actualizar).

Tenga en cuenta que el `SelectByProductID` procedimiento almacenado toma `@ProductID` como un parámetro de entrada y se ejecuta el `SELECT` instrucción que se escriben en el asistente.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Paso 6: Creación de una clase de capa de lógica empresarial

A lo largo de la serie de tutoriales se han luchado por mantener una arquitectura por capas en el que la capa de presentación había realizado todas sus llamadas a la capa de lógica empresarial (BLL). Con el fin de cumplir esta decisión de diseño, necesitamos crear una clase BLL para el nuevo conjunto de datos con tipo antes de poder acceder a datos de productos desde la capa de presentación.

Crear un nuevo archivo de clase denominado `ProductsBLLWithSprocs.cs` en el `~/App_Code/BLL` carpeta y agregarle el código siguiente:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Esta clase imita el `ProductsBLL` semántica de los tutoriales anteriores, pero usa la clase el `ProductsTableAdapter` y `ProductsDataTable` objetos desde el `NorthwindWithSprocs` conjunto de datos. Por ejemplo, en lugar de tener un `using NorthwindTableAdapters` instrucción al principio del archivo de clase como `ProductsBLL` hace, el `ProductsBLLWithSprocs` clase utiliza `using NorthwindWithSprocsTableAdapters`. Del mismo modo, el `ProductsDataTable` y `ProductsRow` van precedidos de los objetos utilizados en esta clase la `NorthwindWithSprocs` espacio de nombres. El `ProductsBLLWithSprocs` clase proporciona acceso a datos de dos métodos, `GetProducts` y `GetProductByProductID`, y métodos para agregar, actualizar y eliminar una instancia única de producto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Paso 7: Trabajar con el`NorthwindWithSprocs`conjunto de datos de la capa de presentación

En este punto hemos creado un DAL que utilice procedimientos almacenados para tener acceso y modificar la base de datos subyacente. También hemos creado un BLL rudimentaria con métodos para recuperar todos los productos o un producto determinado junto con los métodos para agregar, actualizar y eliminar productos. Para redondear este tutorial, s permiten crear una página ASP.NET que usa la s BLL `ProductsBLLWithSprocs` clase para mostrar, actualizar y eliminar registros.

Abra el `NewSprocs.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, asígnele el nombre `Products`. Desde el control GridView elegir enlazarla a un nuevo origen ObjectDataSource denominada etiqueta inteligente de s `ProductsDataSource`. Configurar el origen ObjectDataSource para usar el `ProductsBLLWithSprocs` clase, como se muestra en la figura 22.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Figura 22**: configurar el origen ObjectDataSource que se usarán el `ProductsBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


La lista desplegable en la ficha Seleccionar tiene dos opciones, `GetProducts` y `GetProductByProductID`. Puesto que deseamos mostrar todos los productos en el control GridView, elija el `GetProducts` método. Las listas desplegables en las pestañas UPDATE, INSERT y DELETE tienen solo un método. Asegúrese de que cada una de estas listas desplegables tiene su correspondiente método seleccionado y, a continuación, haga clic en Finalizar.

Cuando haya finalizado el ObjectDataSource wizard, Visual Studio agregará BoundFields y un CampoCasillaVerificación en GridView para los campos de datos del producto. Activar el control GridView s integradas características de modificación y eliminación seleccionando las opciones Habilitar eliminación presentes en la etiqueta inteligente y Habilitar edición.


[![La página contiene un control GridView con la edición y eliminación habilitada la compatibilidad con](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Figura 23**: la página contiene un control GridView con la edición y eliminación de habilitada la compatibilidad con ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Como se ve descritos en los tutoriales anteriores, al finalizar el Asistente para la s de ObjectDataSource, conjuntos de Visual Studio la `OldValuesParameterFormatString` propiedad original\_{0}. Esto se debe revertirse a su valor predeterminado de {0} en orden para que funcione correctamente las características de modificación de datos según los parámetros esperados por los métodos de nuestro BLL. Por lo tanto, asegúrese de establecer el `OldValuesParameterFormatString` propiedad {0} o quite la propiedad completamente de la sintaxis declarativa.

Después de completar el Asistente para configurar origen de datos, activar la edición y eliminación de soporte técnico en el control GridView y devolver la s ObjectDataSource `OldValuesParameterFormatString` propiedad a su valor predeterminado, el marcado declarativo de la página s debe ser similar al siguiente:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

En este punto, podríamos limpiarla GridView personalizando la interfaz de edición para incluir la validación, tener el `CategoryID` y `SupplierID` columnas se representan como listas desplegables y así sucesivamente. También podríamos agregar una confirmación del cliente en el botón Eliminar, y le recomiendo que dedique un momento para implementar estas mejoras. Dado que estos temas se han tratado en tutoriales anteriores, sin embargo, no trataremos ellos nuevo aquí.

Independientemente de si el control GridView mejorar o no, pruebe las características principales de página s en un explorador. Como se muestra en la figura 24, la página muestra los productos en un control GridView que proporciona la edición y eliminación de funciones por fila.


[![Los productos se pueden ver, editar y eliminar de GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Figura 24**: los productos se pueden ver, editado y Deleted desde el control GridView ([haga clic aquí para ver imagen en tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Resumen

Los TableAdapters del conjunto de datos con tipo puede tener acceso a datos desde la base de datos mediante instrucciones SQL ad hoc o a través de procedimientos almacenados. Cuando se trabaja con procedimientos almacenados, pueden usar ambos procedimientos almacenados existentes o puede indicará el Asistente de TableAdapter para crear nuevos procedimientos según un `SELECT` consulta. En este tutorial, exploramos cómo hacer que los procedimientos almacenados que se crean automáticamente para nosotros.

Al disponer de los procedimientos almacenados generados automáticamente, le ayuda a ahorrar tiempo, hay algunos casos donde el procedimiento almacenado creado por el Asistente t alinear con lo que habría creado en nuestro propio. Un ejemplo es el `Products_Update` procedimiento almacenado, lo que esperaba ambos `@Original_ProductID` y `@ProductID` parámetros de entrada, aunque el `@Original_ProductID` parámetro era superfluo.

En muchos escenarios, es posible que ya se han creado los procedimientos almacenados o quizás deseamos compilarlos manualmente con el fin de tener un mayor grado de control sobre los comandos de s de procedimiento almacenado. En cualquier caso, querríamos indicar el TableAdapter para usar procedimientos almacenados existentes para sus métodos. Veremos cómo realizar esta tarea en el siguiente tutorial.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Crear y mantener procedimientos almacenados](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Recuperación de datos escalares de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Conceptos básicos de procedimiento almacenado de SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Los procedimientos almacenados: Información general](http://www.sqlteam.com/item.asp?ItemID=563)
- [Escribir un procedimiento almacenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Geisenow Hilton. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
