---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Crear nuevos procedimientos almacenados para TableAdapters del conjunto de datos con tipo (VB) | Documentos de Microsoft
author: rick-anderson
description: "En los tutoriales anteriores hemos creado instrucciones SQL en el código y pasa las instrucciones a la base de datos que se ejecute. Un enfoque alternativo es utilizar s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d250a7fb868d712e8039e65f7219f80ccaa780c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Crear nuevos procedimientos almacenados para TableAdapters del conjunto de datos con tipo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) o [descarga de PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> En los tutoriales anteriores hemos creado instrucciones SQL en el código y pasa las instrucciones a la base de datos que se ejecute. Un enfoque alternativo es utilizar los procedimientos almacenados, donde las instrucciones SQL son predefinidas en la base de datos. En este tutorial se muestra cómo para que el Asistente de TableAdapter generar nuevos procedimientos almacenados para nosotros.


## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) para estos tutoriales usa conjuntos de datos con tipo. Como se describe en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, los conjuntos de datos con tipo están compuestos de DataTables fuertemente tipadas y los TableAdapters. Las DataTables representan las entidades lógicas en el sistema mientras la interfaz de los TableAdapters con la base de datos subyacente para realizar el trabajo de acceso de datos. Esto incluye rellenar las tablas de datos con datos, ejecutar consultas que devuelven datos escalares, insertar, actualizar y eliminar registros de la base de datos.

Los comandos SQL ejecutados por los TableAdapters pueden ser cualquier instrucciones SQL ad hoc, como `SELECT columnList FROM TableName`, o procedimientos almacenados. Los TableAdapters en nuestra arquitectura usar instrucciones SQL ad hoc. Muchos desarrolladores y administradores de base de datos, sin embargo, prefieren los procedimientos almacenados sobre instrucciones de SQL ad hoc por motivos de seguridad, el mantenimiento y la capacidad de actualización. Otros prefieren ardently instrucciones de SQL ad hoc para su flexibilidad. En mi propio trabajo prefieren los procedimientos almacenados sobre instrucciones SQL ad hoc, pero decide usar instrucciones SQL ad hoc para simplificar los tutoriales anteriores.

Al definir un TableAdapter o agregar nuevos métodos, el Asistente para la s de TableAdapter hace así de fácil crear nuevos procedimientos almacenados o usar procedimientos almacenados existentes como lo hace para usar instrucciones SQL ad hoc. En este tutorial, examinaremos cómo hacer que el Asistente para la s de TableAdapter generación automática de procedimientos almacenados. En el siguiente tutorial, veremos cómo configurar los métodos de TableAdapter s para utilizar procedimientos almacenados existentes o creados manualmente.

> [!NOTE]
> Vea la entrada del blog de Rob Howard [Don t Use procedimientos almacenados todavía?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) y [Frans Bouma](https://weblogs.asp.net/fbouma/) entrada de blog de s [procedimientos almacenados son malas, Kay M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para un debate muy interesante sobre los pros y los contras de los procedimientos almacenados y SQL ad hoc.


## <a name="stored-procedure-basics"></a>Conceptos básicos de procedimiento almacenado

Las funciones son una construcción común a todos los lenguajes de programación. Una función es una colección de instrucciones que se ejecutan cuando se llama a la función. Funciones pueden aceptar parámetros de entrada y, opcionalmente, pueden devolver un valor. *[Procedimientos almacenados](http://en.wikipedia.org/wiki/Stored_procedure)*  son estructuras de base de datos que comparten muchas similitudes con las funciones de lenguajes de programación. Un procedimiento almacenado se compone de un conjunto de instrucciones de T-SQL que se ejecutan cuando se llama al procedimiento almacenado. Un procedimiento almacenado puede aceptar cero a muchos parámetros de entrada y puede devolver valores escalares, parámetros de salida, o bien, normalmente, conjuntos de resultados de `SELECT` las consultas.

> [!NOTE]
> Los procedimientos almacenados se conocen a menudo como procedimientos almacenados o SPs.


Los procedimientos almacenados se crean mediante el [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/en-us/library/aa258259(SQL.80).aspx) instrucción T-SQL. Por ejemplo, el siguiente script de T-SQL crea un procedimiento almacenado denominado `GetProductsByCategoryID` que toma un parámetro único denominado `@CategoryID` y devuelve el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` campos de las columnas de la `Products` tabla que tiene la correspondiente `CategoryID` valor:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Una vez que se ha creado este procedimiento almacenado, se puede llamar mediante la sintaxis siguiente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> En el siguiente tutorial examinaremos crear procedimientos almacenados mediante el IDE de Visual Studio. Para este tutorial, sin embargo, vamos a dejar que el Asistente de TableAdapter generar automáticamente los procedimientos almacenados para nosotros.


Además de devolver simplemente los datos, los procedimientos almacenados se utilizan a menudo para ejecutar varios comandos de base de datos dentro del ámbito de una sola transacción. Un procedimiento almacenado denominado `DeleteCategory`, por ejemplo, podría necesitar un `@CategoryID` parámetro y realizar dos `DELETE` instrucciones: primero, uno para eliminar los productos relacionados y un segundo al eliminar la categoría especificada. Son varias instrucciones dentro de un procedimiento almacenado *no* ajustado automáticamente dentro de una transacción. Comandos de T-SQL adicionales que debe emitir para garantizar el procedimiento almacenado s que varios comandos se tratan como una operación atómica. Veremos cómo ajustar un comandos s de procedimiento almacenado dentro del ámbito de una transacción en el tutorial posterior.

Cuando utilice procedimientos almacenados dentro de una arquitectura, los métodos de s de la capa de acceso a datos invocan un procedimiento almacenado específico en lugar de emitir una instrucción de SQL ad hoc. Con esto centraliza la ubicación de las instrucciones SQL ejecutadas (en la base de datos) en lugar de tener que definidos dentro de la arquitectura de s de la aplicación. Puede que esta centralización resulta más fácil buscar, analizar y optimizar las consultas y proporciona una imagen mucho más clara respecto a dónde y cómo es que se va a utilizar la base de datos.

Para obtener más información sobre aspectos básicos de procedimiento almacenado, consulte los recursos en la sección más información al final de este tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Paso 1: Crear las páginas Web de escenarios de capa de datos avanzados acceso

Antes de empezar la explicación sobre la creación de un DAL mediante procedimientos almacenados, permiten s primero Tómese un momento para crear las páginas ASP.NET en el proyecto de sitio Web que necesitamos para esto y los tutoriales de varias siguientes. Empiece por agregar una nueva carpeta denominada `AdvancedDAL`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Agregar las páginas ASP.NET para los tutoriales de escenarios de capa de acceso de datos avanzados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales de escenarios de capa de acceso de datos avanzados


Al igual que en las otras carpetas, `Default.aspx` en el `AdvancedDAL` carpeta enumerará los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Por último, agregue estas páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después el trabajo con datos por lotes `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. Ahora, el menú de la izquierda incluye elementos para los tutoriales de escenarios avanzados de DAL.


![El mapa del sitio ahora incluye las entradas para los tutoriales de escenarios avanzados de DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figura 3**: la asignación de sitio ahora incluye las entradas para los tutoriales de escenarios avanzados de DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Paso 2: Configurar un TableAdapter para crear nuevos procedimientos almacenados

Para demostrar la creación de una capa de acceso de datos que utiliza los procedimientos almacenados en lugar de instrucciones SQL ad hoc, s permiten crear un conjunto de datos de tipo nuevo en el `~/App_Code/DAL` carpeta denominada `NorthwindWithSprocs.xsd`. Puesto que nos hemos paso a paso el proceso detalladamente en tutoriales anteriores, se continuará rápidamente a través de los pasos siguientes. Si se bloquea o se necesitan más instrucciones paso a paso para crear y configurar un conjunto de datos con tipo, hacen referencia a la [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial.

Agregar un nuevo conjunto de datos al proyecto con el botón secundario en el `DAL` carpeta, elija Agregar nuevo elemento y seleccione la plantilla de conjunto de datos tal como se muestra en la figura 4.


[![Agregar un nuevo conjunto de datos con tipo al proyecto denominado NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figura 4**: agregar un nuevo conjunto de datos con tipo para el proyecto denominado `NorthwindWithSprocs.xsd` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Esto se crear el nuevo conjunto de datos con tipo, abra su diseñador, crear un nuevo TableAdapter e iniciar al Asistente para configuración de TableAdapter. Asistente para configuración de TableAdapter s primer paso le pide que seleccione la base de datos para trabajar con. La cadena de conexión a la base de datos de Northwind debe aparecer en la lista desplegable. Seleccione esta opción y haga clic en siguiente.

Desde esta pantalla siguiente podemos elegir cómo TableAdapter debe tener acceso a la base de datos. En los tutoriales anteriores, se selecciona la primera opción, usar instrucciones SQL. Para este tutorial, seleccione la segunda opción, crear nuevos procedimientos almacenados y haga clic en siguiente.


[![Indicar el TableAdpater para crear nuevos procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figura 5**: indicar el TableAdpater para crear nuevos procedimientos almacenados ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Al igual que con el uso de instrucciones SQL ad hoc, en el siguiente paso se nos pide que proporcione la `SELECT` instrucción de la consulta principal de TableAdapter s. Pero en lugar de utilizar el `SELECT` instrucción insertada aquí para realizar una consulta ad hoc directamente, el Asistente para la s de TableAdapter creará un procedimiento almacenado que contiene este `SELECT` consulta.

Utilice la siguiente `SELECT` consulta para este TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Escriba la consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figura 6**: escriba el `SELECT` consulta ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> La consulta anterior difiere ligeramente de la consulta principal de la `ProductsTableAdapter` en el `Northwind` conjunto de datos con tipo. Recuerde que el `ProductsTableAdapter` en el `Northwind` Typed DataSet incluye dos subconsultas correlativas para devolver el nombre de categoría y el nombre de la empresa para cada categoría de producto s y el proveedor. En el próximo seminario Web [actualizar TableAdapter a la utilice une](updating-the-tableadapter-to-use-joins-vb.md) datos relacionados con el tutorial, veremos agregar esto a este TableAdapter.


Tómese un momento para hacer clic en el botón Opciones avanzadas. Desde aquí podemos especificar si el asistente también debe generar insert, update y delete instrucciones de TableAdapter, si se debe usar simultaneidad optimista, y si se debe actualizar la tabla de datos después de las actualizaciones e inserciones. La opción de instrucciones generar Insert, Update y Delete está activada de forma predeterminada. Deja activada. Para este tutorial, deje desactivada la usar opciones de simultaneidad optimista.

Cuando es preciso los procedimientos almacenados creados automáticamente por el Asistente de TableAdapter, parece que la actualización de la opción de tabla de datos se omite. Independientemente de si se activa esta casilla, el resultante insert y update procedimientos almacenados recuperan el registro recién insertada o actualizada de just, tal y como se verá en el paso 3.


![Deje las instrucciones de generar Insert, Update y Delete activa opción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figura 7**: deje las instrucciones de generar Insert, Update y Delete activa opción


> [!NOTE]
> Si se activa el uso de la opción de simultaneidad optimista, el asistente agregará condiciones adicionales para la `WHERE` cláusula que impiden que los datos se actualicen si se produjeron cambios en otros campos. Hacen referencia a la [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial para obtener más información sobre el uso de la característica de control de simultaneidad optimista integrados de s de TableAdapter.


Después de escribir el `SELECT` consultar y confirmar que esté marcada la opción de instrucciones generar Insert, Update y Delete, haga clic en siguiente. Esta pantalla siguiente, que se muestra en la figura 8, solicita los nombres de los procedimientos almacenados que el asistente lo creará para seleccionar, insertar, actualizar y eliminar datos. Cambiar estos almacenan nombres de procedimientos para `Products_Select`, `Products_Insert`, `Products_Update`, y `Products_Delete`.


[![Cambiar el nombre de los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 8**: cambiar el nombre de los procedimientos almacenados ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Para ver el código T-SQL usará el Asistente de TableAdapter para crear los cuatro procedimientos almacenados, haga clic en el botón de vista previa de Script SQL. En el cuadro de diálogo de vista previa de Script de SQL puede guardar la secuencia de comandos en un archivo o copiarlo en el Portapapeles.


![Obtener una vista previa del Script SQL usado para generar los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 9**: vista previa de la secuencia de comandos SQL que se usa para generar los procedimientos almacenados


Después de asignar nombres a los procedimientos almacenados, haga clic en siguiente métodos correspondientes del nombre de la s de TableAdapter. Al igual que cuando utiliza instrucciones SQL ad hoc, podemos crear métodos para rellenar una DataTable existente o devuelven una nueva. También podemos especificar si el objeto TableAdapter debe incluir el patrón de DB-Direct de inserción, actualización y eliminación de registros. Deje todas las casillas de tres verificación activadas, pero cambie el nombre la devolución de un método de DataTable a `GetProducts` (como se muestra en la figura 10).


[![El nombre de los métodos de relleno y GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figura 10**: el nombre de los métodos `Fill` y `GetProducts` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Haga clic en siguiente para ver un resumen de los pasos que se llevará a cabo el asistente. Complete el asistente, haga clic en el botón Finalizar. Una vez completado el asistente, volverá al conjunto de datos s diseñador, que ahora debe incluir el `ProductsDataTable`.


[![El Diseñador de DataSet s muestra la ProductsDataTable recién agregada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figura 11**: s de conjunto de datos el diseñador muestra el agregado recientemente `ProductsDataTable` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Paso 3: Examinar los procedimientos almacenados recién creados

El Asistente de TableAdapter utilizado automáticamente en el paso 2 crea los procedimientos almacenados para seleccionar, insertar, actualizar y eliminar datos. Estos procedimientos almacenados se pueden ver o modificar a través de Visual Studio en el Explorador de servidores y explorar en profundidad en la carpeta procedimientos almacenados de s de base de datos. Como se muestra en la figura 12, la base de datos de Northwind contiene cuatro nuevos procedimientos almacenados: `Products_Delete`, `Products_Insert`, `Products_Select`, y `Products_Update`.


![Los cuatro procedimientos almacenados creados en el paso 2 se pueden encontrar en la carpeta de los procedimientos almacenados de la base de datos s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figura 12**: los cuatro procedimientos almacenados creados en el paso 2 se pueden encontrar en la carpeta de los procedimientos almacenados de la base de datos s


> [!NOTE]
> Si no ve el Explorador de servidores, vaya al menú Ver y elija la opción de explorador de servidores. Si no ve los procedimientos almacenados relacionados con productos agregados del paso 2, pruebe con el botón secundario en la carpeta procedimientos almacenados y elegir actualiza.


Para ver o modificar un procedimiento almacenado, haga doble clic en su nombre en el Explorador de servidores o, alternativamente, haga doble clic en el procedimiento almacenado y elija Abrir. La figura 13 muestra el `Products_Delete` procedimiento almacenado, cuando se abre.


[![Los procedimientos almacenados para poder abrir y modificar desde dentro de Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figura 13**: almacena los procedimientos se pueden abrir y modificar desde dentro de Visual Studio ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


El contenido de ambas la `Products_Delete` y `Products_Select` los procedimientos almacenados son bastante sencillos. El `Products_Insert` y `Products_Update` los procedimientos almacenados, por otro lado, garantizan detenimiento mientras ambos llevan a cabo una `SELECT` instrucción después de su `INSERT` y `UPDATE` las instrucciones. Por ejemplo, el siguiente código de SQL constituye la `Products_Insert` procedimiento almacenado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

El procedimiento almacenado acepta como parámetros de entrada del `Products` columnas que ha sido devuelto por la `SELECT` consulta especificada en el Asistente para la s de TableAdapter y estos valores se utilizan en un `INSERT` instrucción. Después de la `INSERT` (instrucción), un `SELECT` consulta se utiliza para devolver el `Products` valores de columna (incluido el `ProductID`) del registro recién agregado. Esta capacidad de actualización es útil al agregar un nuevo registro con el patrón de actualización por lotes tal y como automáticamente actualizaciones recién agregado `ProductRow` instancias `ProductID` propiedades con los valores de incremento automático asignados por la base de datos.

El código siguiente muestra esta característica. Contiene un `ProductsTableAdapter` y `ProductsDataTable` creado para el `NorthwindWithSprocs` conjunto de datos con tipo. Un nuevo producto se agrega a la base de datos mediante la creación de un `ProductsRow` instancia, proporcionando sus valores y llamar a las operaciones de asignación TableAdapter `Update` método, pasando el `ProductsDataTable`. Internamente, los TableAdapter s `Update` método enumera el `ProductsRow` instancias en la tabla de datos en el pasado (en este ejemplo solo hay un - aquel que acabamos de agregar) y realiza el apropiado insertar, actualizar o eliminar comandos. En este caso, el `Products_Insert` se ejecuta el procedimiento almacenado, que agrega un nuevo registro a la `Products` de tabla y devuelve los detalles del registro recién agregado. El `ProductsRow` instancia s `ProductID` , a continuación, se actualiza el valor. Después de la `Update` método se ha completado, podemos acceder al registro recién agregado s `ProductID` valor a través de la `ProductsRow` s `ProductID` propiedad.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

El `Products_Update` procedimiento almacenado de forma similar incluye un `SELECT` instrucción después de su `UPDATE` instrucción.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Tenga en cuenta que este procedimiento almacenado incluye dos parámetros de entrada para `ProductID`: `@Original_ProductID` y `@ProductID`. Esta funcionalidad permite escenarios donde se puede cambiar la clave principal. Por ejemplo, en una base de datos de empleados, cada registro de empleado puede usar el número de seguridad social de empleado s como su clave principal. Para cambiar un número de seguridad social de existente empleado s, deben proporcionarse tanto el nuevo número de seguridad social y la original. Para el `Products` tabla, esta funcionalidad no es necesaria porque el `ProductID` columna es una `IDENTITY` columna y nunca debe cambiarse. De hecho, el `UPDATE` instrucción en el `Products_Update` t de procedimiento almacenado incluyen la `ProductID` columna en su lista de columnas. Por lo tanto, mientras `@Original_ProductID` se utiliza en el `UPDATE` instrucción s `WHERE` cláusula, es superflua para la `Products` de tabla y podría ser sustituida por la `@ProductID` parámetro. Cuando se modifica un parámetros de procedimiento almacenado s es importante que los métodos de TableAdapter que utilice el procedimiento almacenado también se actualizan.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Paso 4: Modificar el procedimiento almacenado s parámetros y actualización de TableAdapter

Puesto que la `@Original_ProductID` parámetro es superfluo, permiten s. quítela de la `Products_Update` procedimiento almacenado por completo. Abrir la `Products_Update` procedimiento almacenado, eliminar el `@Original_ProductID` parámetro y, en la `WHERE` cláusula de la `UPDATE` instrucción, cambiar el nombre de parámetro usado de `@Original_ProductID` a `@ProductID`. Después de realizar estos cambios, el código T-SQL dentro del procedimiento almacenado debe ser similar al siguiente:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Para guardar estos cambios en la base de datos, haga clic en el icono Guardar en la barra de herramientas o presiona CTRL+s. En este momento, el `Products_Update` procedimiento almacenado no espera un `@Original_ProductID` parámetro de entrada, pero el TableAdapter está configurado para pasar un parámetro de este tipo. Puede ver los parámetros de TableAdapter se enviará a la `Products_Update` procedimiento almacenado seleccionando TableAdapter en el Diseñador de DataSet, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en el `UpdateCommand` s `Parameters` colección. Se abrirá el cuadro de diálogo Editor de la colección de parámetros que se muestra en la figura 14.


![Las listas de Editor de la colección de parámetros con los parámetros usados pasa a la Products_Update procedimiento almacenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figura 14**: las listas de Editor de la colección de parámetros con los parámetros usados pasa a la `Products_Update` procedimiento almacenado


Puede quitar este parámetro desde aquí, simplemente seleccione el `@Original_ProductID` parámetro en la lista de miembros y haga clic en el botón Quitar.

Como alternativa, puede actualizar los parámetros utilizados para todos los métodos, con el botón secundario en TableAdapter en el diseñador y elija Configurar. Se abrirá el Asistente para la configuración de TableAdapter, enumerar los procedimientos almacenados utilizados para seleccionar, insertar, actualizar, y eliminar, junto con los parámetros de los procedimientos almacenados esperan recibir. Si hace clic en la lista desplegable de actualización verá el `Products_Update` procedimientos almacenados espera parámetros de entrada, que ahora ya no incluye `@Original_ProductID` (consulte la figura 15). Simplemente haga clic en Finalizar para actualizar automáticamente la colección de parámetros utilizada por el objeto TableAdapter.


[![Como alternativa, puede usar al Asistente para la configuración de TableAdapter s para actualizar sus colecciones de parámetros de métodos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 15**: como alternativa, puede usar el Asistente para configuración para actualizar sus colecciones de parámetros de métodos de TableAdapter s ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Paso 5: Agregar métodos adicionales de TableAdapter

Como paso 2 se muestra, al crear un nuevo TableAdapter es muy fácil tener los procedimientos almacenados correspondientes que se genera automáticamente. El mismo es verdadero al agregar métodos adicionales a un TableAdapter. Para ilustrar esto, permiten s Agregar un `GetProductByProductID(productID)` método para el `ProductsTableAdapter` creado en el paso 2. Este método le llevará como entrada un `ProductID` valor así como devolver detalles sobre el producto especificado.

Iniciar, el botón secundario en TableAdapter y elija Agregar consulta en el menú contextual.


![Agregar una nueva consulta al TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 16**: agregar una nueva consulta al TableAdapter


Esto iniciará al Asistente para configuración de consultas de TableAdapter, que le pide primero sobre cómo el TableAdapter debe tener acceso a la base de datos. Para que un nuevo procedimiento almacenado creado, elija el crear una nueva opción de procedimiento almacenado y haga clic en siguiente.


[![Elija crear un nuevo procedimiento almacenado, opción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figura 17**: elegir crear un nuevo procedimiento almacenado opción ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


La siguiente pantalla le pide que identifique el tipo de consulta que se ejecutará, independientemente de que se devuelva un conjunto de filas o un valor escalar único o realizar una `UPDATE`, `INSERT`, o `DELETE` instrucción. Puesto que el  `GetProductByProductID(productID)` /método siguiente se devuelve una fila, deje la selección que devuelve la opción de filas seleccionado y presione siguiente.


[![Elija la selección que devuelve la fila opción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figura 18**: elija la selección que devuelve la fila opción ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


La siguiente pantalla muestra la consulta principal s del TableAdapter, que solo muestra el nombre del procedimiento almacenado (`dbo.Products_Select`). Reemplace el nombre del procedimiento almacenado con el siguiente `SELECT` instrucción, que devuelve todos los campos de producto para un producto determinado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Reemplace el nombre del procedimiento almacenado con una consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figura 19**: reemplazar el nombre del procedimiento almacenado con un `SELECT` consulta ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


La pantalla subsiguientes en el que se le pide que asigne un nombre de procedimiento almacenado que se va a crear. Escriba el nombre `Products_SelectByProductID` y haga clic en siguiente.


[![Nombre de la nueva Products_SelectByProductID de procedimiento almacenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 20**: nombre del nuevo procedimiento almacenado `Products_SelectByProductID` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


El último paso del asistente permite cambiar el método de nombres generan, así como indican si se debe usar el relleno de un patrón de DataTable, devolver un patrón de tabla de datos, o ambos. Para este método, deje las opciones activadas, pero cambie el nombre de los métodos para `FillByProductID` y `GetProductByProductID`. Haga clic en siguiente para ver un resumen de los pasos, el asistente realizará y, a continuación, haga clic en Finalizar para completar al asistente.


[![Cambiar el nombre de los métodos de TableAdapter s FillByProductID y GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figura 21**: cambiar el nombre de los métodos de TableAdapter s a `FillByProductID` y `GetProductByProductID` ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Después de completar el asistente, lo TableAdapter dispone de un método nuevo, `GetProductByProductID(productID)` que, cuando se invoca, se ejecutará el `Products_SelectByProductID` procedimiento almacenado que se acaba de crear. Tómese un momento para ver este nuevo procedimiento almacenado desde el Explorador de servidores, obtención de detalles de la carpeta procedimientos almacenados y abrir `Products_SelectByProductID` (si no lo ve, haga doble clic en la carpeta procedimientos almacenados y elija actualizar).

Tenga en cuenta que la `SelectByProductID` almacenados procedimiento toma `@ProductID` como un parámetro de entrada y se ejecuta el `SELECT` instrucción que se especificó en el asistente.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Paso 6: Crear una clase de capa de lógica de negocios

A lo largo de la serie de tutoriales se ha emprendido mantener una arquitectura en capas en el que la capa de presentación había realizado todas sus llamadas a la capa de lógica empresarial (BLL). Con el fin de cumplir esta decisión de diseño, primero se debe crear una clase BLL para el nuevo conjunto de datos con tipo antes de que podamos acceder a datos de productos de la capa de presentación.

Cree un nuevo archivo de clase denominado `ProductsBLLWithSprocs.vb` en el `~/App_Code/BLL` carpeta y agregarle el código siguiente:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Esta clase imita el `ProductsBLL` semántica de los tutoriales anteriores, pero usa la clase el `ProductsTableAdapter` y `ProductsDataTable` objetos de la `NorthwindWithSprocs` conjunto de datos. Por ejemplo, en lugar de tener un `Imports NorthwindTableAdapters` instrucción al principio del archivo de clase como `ProductsBLL` es así, el `ProductsBLLWithSprocs` clase utiliza `Imports NorthwindWithSprocsTableAdapters`. Del mismo modo, el `ProductsDataTable` y `ProductsRow` objetos utilizados en esta clase tienen el prefijo con el `NorthwindWithSprocs` espacio de nombres. El `ProductsBLLWithSprocs` clase proporciona acceso a datos de dos métodos, `GetProducts` y `GetProductByProductID`, y métodos para agregar, actualizar y eliminar una instancia de un único producto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Paso 7: Trabajar con el`NorthwindWithSprocs`conjunto de datos desde la capa de presentación

En este punto hemos creado un DAL que utiliza los procedimientos almacenados para tener acceso y modificar los datos de la base de datos subyacente. También hemos creado un BLL rudimentario con métodos para recuperar todos los productos o un producto determinado junto con los métodos para agregar, actualizar y eliminar productos. Para redondear este tutorial, s permiten crear una página ASP.NET que utiliza las operaciones de asignación BLL `ProductsBLLWithSprocs` clase para mostrar, actualizar y eliminar registros.

Abra la `NewSprocs.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView en el cuadro de herramientas hasta el diseñador, asígnele el nombre `Products`. De GridView etiqueta inteligente de s decide enlazar a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para usar el `ProductsBLLWithSprocs` de la clase, como se muestra en la figura 22.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figura 22**: configurar el ObjectDataSource que se utilizan los `ProductsBLLWithSprocs` clase ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


La lista desplegable en la ficha Seleccionar tiene dos opciones, `GetProducts` y `GetProductByProductID`. Puesto que deseamos mostrar todos los productos en GridView, elija la `GetProducts` método. Las listas desplegables en las pestañas UPDATE, INSERT y DELETE tienen solo un método. Asegúrese de que cada una de estas listas desplegables tiene su correspondiente método seleccionado y, a continuación, haga clic en Finalizar.

Una vez finalizado el Asistente ObjectDataSource, Visual Studio agregará BoundFields y un CampoCasillaVerificación a GridView para los campos de datos de producto. Activar la GridView s integradas características de modificación y eliminación mediante la comprobación de las opciones de Habilitar eliminación presentes en la etiqueta inteligente y de Habilitar edición.


[![La página contiene una GridView con Editar y eliminar habilitada la compatibilidad con](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figura 23**: la página contiene una GridView con Editar y eliminar la compatibilidad con habilitada ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Como se ha descrito en los tutoriales anteriores, al finalizar el Asistente para la s de ObjectDataSource, Visual Studio establece la `OldValuesParameterFormatString` propiedad original\_{0}. Es necesario revertir a su valor predeterminado de {0} en orden para las características de modificación de datos para que funcione correctamente dados los parámetros que se espera con los métodos de nuestro BLL. Por lo tanto, asegúrese de establecer el `OldValuesParameterFormatString` propiedad {0} o quite por completo la propiedad de la sintaxis declarativa.

Después de completar el asistente Configurar origen de datos, activar la edición y eliminación de soporte técnico en GridView y devolver la s ObjectDataSource `OldValuesParameterFormatString` propiedad en su valor predeterminado, el marcado declarativo de la página s debería ser similar al siguiente:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

En este momento se puede ordenar GridView personalizando la interfaz de edición para incluir la validación, tener la `CategoryID` y `SupplierID` las columnas se representan como listas desplegables y así sucesivamente. También podríamos agregar una confirmación de cliente para el botón Eliminar y le animo a dedicar tiempo a implementar estas mejoras. Puesto que estos temas se han tratado en tutoriales anteriores, sin embargo, no trataremos ellos nuevo aquí.

Independientemente de si mejorar GridView o no, pruebe las características principales de página s en un explorador. Como se muestra en figura 24, la página muestra los productos en un control GridView que proporciona la edición y capacidades de eliminación por fila.


[![Los productos se pueden ver, editar y eliminar de GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figura 24**: los productos se pueden ver, editada y eliminaciones de GridView ([haga clic aquí para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Resumen

Los TableAdapters en un conjunto de datos con tipo puede tener acceso a datos desde la base de datos utilizando instrucciones SQL ad hoc o a través de procedimientos almacenados. Cuando trabaja con procedimientos almacenados, pueden utilizar cualquier procedimientos almacenados existentes o el Asistente de TableAdapter se puede indicar al crear nuevos tomando como base de procedimientos almacenados una `SELECT` consulta. En este tutorial, analizamos cómo hacer que los procedimientos almacenados que se crean automáticamente.

Al disponer de los procedimientos almacenados generados automáticamente, le ayuda a ahorrar tiempo, hay algunos casos donde el procedimiento almacenado creado por el Asistente t alinear con lo que habría creado en nuestra propia. Un ejemplo es el `Products_Update` procedimiento almacenado, que se esperaba ambos `@Original_ProductID` y `@ProductID` parámetros de entrada, aunque el `@Original_ProductID` parámetro era superfluo.

En muchos escenarios, los procedimientos almacenados ya se han creado o que podríamos desear crearlos manualmente con el fin de tener un mayor grado de control sobre los comandos de s de procedimiento almacenado. En cualquier caso, queremos haría indicar el objeto TableAdapter para usar procedimientos almacenados existentes para sus métodos. Veremos cómo hacerlo en el siguiente tutorial.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Crear y mantener los procedimientos almacenados](https://msdn.microsoft.com/en-us/library/aa214299(SQL.80).aspx)
- [Recuperación de datos escalar de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Conceptos básicos de procedimiento almacenados de SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedimientos almacenados: Información general](http://www.sqlteam.com/item.asp?ItemID=563)
- [Escribir un procedimiento almacenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Geisenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
[Siguiente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
