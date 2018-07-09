---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Usar procedimientos almacenados existentes para los TableAdapters del conjunto de datos con tipo (VB) | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior, aprendió a usar al Asistente de TableAdapter para generar nuevos procedimientos almacenados. En este tutorial, aprenderá cómo el mismo TableAdapter...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 586b46acf1df19cb1bdd56c4242e92d2019c7b8a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805631"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Usar procedimientos almacenados existentes para los TableAdapters del conjunto de datos con tipo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) o [descargar PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> En el tutorial anterior, aprendió a usar al Asistente de TableAdapter para generar nuevos procedimientos almacenados. En este tutorial, aprenderá cómo el mismo asistente de TableAdapter puede trabajar con procedimientos almacenados existentes. También se obtenga información sobre cómo agregar manualmente los nuevos procedimientos almacenados a nuestra base de datos.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) hemos visto cómo la s de conjunto de datos con tipo TableAdapters podría configurarse para utilizar procedimientos almacenados para instrucciones de acceso a datos en lugar de ad hoc SQL. En concreto, hemos visto cómo hacer que el Asistente de TableAdapter para crear automáticamente estos procedimientos almacenados. Al migrar una aplicación heredada a ASP.NET 2.0 o cuando se crea un sitio Web de ASP.NET 2.0 en torno a un modelo de datos existente, lo más probable es que la base de datos ya contiene los procedimientos almacenados que necesitamos. Como alternativa, quizás prefiera crear los procedimientos almacenados a mano o mediante alguna herramienta que no sea el Asistente de TableAdapter que genera automáticamente los procedimientos almacenados.

En este tutorial veremos cómo configurar el TableAdapter para usar los procedimientos almacenados existentes. Puesto que la base de datos Northwind tiene solo un pequeño conjunto de procedimientos almacenados integrados, también veremos los pasos necesarios para agregar manualmente los nuevos procedimientos almacenados a la base de datos a través del entorno de Visual Studio. Introducción s Let!

> [!NOTE]
> En el [ajuste modificaciones de base de datos dentro de una transacción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial agregamos métodos a TableAdapter para admitir las transacciones (`BeginTransaction`, `CommitTransaction`, y así sucesivamente). Como alternativa, las transacciones se pueden administrar completamente dentro de un procedimiento almacenado, que no se requiere ninguna modificación en el código de capa de acceso a datos. En este tutorial exploraremos los comandos de T-SQL usados para ejecutar un procedimiento almacenado s las instrucciones dentro del ámbito de una transacción.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Paso 1: Agregar procedimientos almacenados para la base de datos Northwind

Visual Studio facilita agregar nuevos procedimientos almacenados para una base de datos. S permiten agregar un nuevo procedimiento almacenado a la base de datos de Northwind que devuelve todas las columnas de la `Products` tabla aquellas personas que tienen un determinado `CategoryID` valor. En la ventana Explorador de servidores, expanda la base de datos Northwind para que se muestren sus carpetas: diagramas de base de datos, tablas, vistas etc. Como hemos visto en el tutorial anterior, la carpeta procedimientos almacenados contiene la base de datos s los procedimientos almacenados existentes. Para agregar un nuevo procedimiento almacenado, haga clic en la carpeta procedimientos almacenados y elija la opción Agregar nuevo procedimiento almacenado en el menú contextual.


[![Haga clic en la carpeta procedimientos almacenados y agregue un nuevo procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: haga clic en la carpeta de procedimientos almacenados y agregar un nuevo procedimiento almacenado ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Como se muestra en la figura 1, seleccionando la opción Agregar nuevo procedimiento almacenado abre una ventana de script en Visual Studio con el contorno de la secuencia de comandos SQL necesario para crear el procedimiento almacenado. Es nuestro trabajo consiste en dar forma a este script y ejecutarlo, momento en que el procedimiento almacenado se agregarán a la base de datos.

Escriba el siguiente script:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Este script, cuando se ejecuta, agregará un nuevo procedimiento almacenado a la base de datos Northwind denominado `Products_SelectByCategoryID`. Este procedimiento almacenado acepta un parámetro de entrada (`@CategoryID`, del tipo `int`) y devuelve todos los campos para los productos que tengan una coincidencia `CategoryID` valor.

Para ejecutar este `CREATE PROCEDURE` de secuencias de comandos y agregue el procedimiento almacenado a la base de datos, haga clic en el icono Guardar en la barra de herramientas o presione CTRL+s. Una vez hecho esto, las actualizaciones de la carpeta procedimientos almacenados, el procedimiento almacenado que muestra el recién creado. Además, la secuencia de comandos en la ventana cambiará sutileza de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` a `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Agrega un nuevo procedimiento almacenado a la base de datos, mientras que `ALTER PROCEDURE` actualiza una ya existente. Puesto que el inicio de la secuencia de comandos ha cambiado a `ALTER PROCEDURE`, cambiar los procedimientos almacenados de entrada parámetros o instrucciones SQL y haga clic en el icono Guardar se actualizará el procedimiento almacenado con estos cambios.

Figura 2 se muestra Visual Studio después de la `Products_SelectByCategoryID` procedimiento almacenado se ha guardado.


[![El Products_SelectByCategoryID del procedimiento almacenado se ha agregado a la base de datos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: The Stored Procedure `Products_SelectByCategoryID` se ha agregado a la base de datos ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Paso 2: Configurar el TableAdapter para usar un procedimiento almacenado existente

Ahora que el `Products_SelectByCategoryID` procedimiento almacenado se ha agregado a la base de datos, podemos configurar la capa de acceso a datos para usar este procedimiento almacenado cuando se llama a uno de sus métodos. En concreto, se agregará un `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` método a la `ProductsTableAdapter` en el `NorthwindWithSprocs` DataSet con tipo que llama a la `Products_SelectByCategoryID` que acabamos de crear el procedimiento almacenado.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos. Haga doble clic en el `ProductsTableAdapter` y elija Agregar consulta para iniciar el Asistente para configuración de la consulta de TableAdapter. En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) decidimos que TableAdapter cree un nuevo procedimiento almacenado para nosotros. Para este tutorial, sin embargo, desea conectar el nuevo método TableAdapter existente `Products_SelectByCategoryID` procedimiento almacenado. Por lo tanto, elija la opción de procedimiento almacenado existente del uso del Asistente para la s primer paso y, a continuación, haga clic en siguiente.


[![Elija el uso existente de la opción de procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: elegir el uso existente procedimiento opción almacenado ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


La pantalla siguiente se proporciona que una lista desplegable rellena con la base de datos s procedimientos almacenados. Seleccionar un procedimiento almacenado, enumera sus parámetros de entrada a la izquierda y los campos de datos devueltos (si existe) de la derecha. Elija la `Products_SelectByCategoryID` procedimiento almacenado desde la lista y haga clic en siguiente.


[![Elegir el Products_SelectByCategoryID procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: elegir el `Products_SelectByCategoryID` Stored Procedure ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


La siguiente pantalla nos pregunta qué tipo de datos devuelto por el procedimiento almacenado y aquí nuestra respuesta determina el tipo devuelto por el método s de TableAdapter. Por ejemplo, si nos indican que se devuelven datos tabulares, el método devolverá un `ProductsDataTable` instancia rellenado con los registros devueltos por el procedimiento almacenado. En cambio, si nos indican que este procedimiento almacenado devuelve un valor único TableAdapter devolverá un `Object` que se asigna el valor de la primera columna del primer registro devuelto por el procedimiento almacenado.

Puesto que el `Products_SelectByCategoryID` procedimiento almacenado devuelve todos los productos que pertenecen a una categoría determinada, elija la primera respuesta - datos tabulares y haga clic en siguiente.


[![Indicar que el procedimiento almacenado devuelve datos tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicar que el procedimiento almacenado devuelve datos tabulares ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Todo lo que queda es indicar qué patrones de método para usar seguido por los nombres de estos métodos. Deje ambas rellenar una DataTable y devolverla comprueban las opciones de una DataTable, pero cambie el nombre de los métodos para `FillByCategoryID` y `GetProductsByCategoryID`. A continuación, haga clic en siguiente para revisar un resumen de las tareas que realizará el asistente. Si todo parece correcto, haga clic en Finalizar.


[![Nombre de lo métodos de FillByCategoryID y GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: el nombre de los métodos `FillByCategoryID` y `GetProductsByCategoryID` ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Los métodos de TableAdapter que acabamos de crear, `FillByCategoryID` y `GetProductsByCategoryID`, espera un parámetro de entrada de tipo `Integer`. Este valor de parámetro de entrada se pasa al procedimiento almacenado a través de su `@CategoryID` parámetro. Si modifica el `Products_SelectByCategory` almacena los parámetros de procedimiento s, deberá actualizar también los parámetros de estos métodos de TableAdapter. Como se describe en el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), esto puede hacerse de dos maneras: manualmente agregando o quitando los parámetros de la colección de parámetros o volviendo a ejecutar el Asistente de TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Paso 3: Agregar un`GetProductsByCategoryID(categoryID)`a la capa BLL (método)

Con el `GetProductsByCategoryID` completa el método DAL, el siguiente paso es proporcionar acceso a este método en la capa de lógica empresarial. Abra el `ProductsBLLWithSprocs` archivo de clase y agregue el siguiente método:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Este método BLL devuelve simplemente el `ProductsDataTable` devuelto desde el `ProductsTableAdapter` s `GetProductsByCategoryID` método. El `DataObjectMethodAttribute` atributo proporciona metadatos usados por el Asistente para configurar origen de datos de origen ObjectDataSource s. En concreto, este método aparecerá en la lista de desplegable seleccione ficha s.

## <a name="step-4-displaying-products-by-category"></a>Paso 4: Mostrar productos por categoría

Para probar la recién agregada `Products_SelectByCategoryID` procedimiento almacenado y los métodos correspondientes de DAL y BLL, permiten s crear una página ASP.NET que contiene un DropDownList y GridView. DropDownList enumerará todas las categorías en la base de datos mientras el control GridView mostrará los productos que pertenecen a la categoría seleccionada.

> [!NOTE]
> Se ve crear interfaces de maestro y detalles mediante las listas desplegables en los tutoriales anteriores. Para una información más detallada sobre la implementación de un informe de maestro y detalles, consulte el [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial.


Abra el `ExistingSprocs.aspx` página en el `AdvancedDAL` carpeta y arrastre un DropDownList desde el cuadro de herramientas hasta el diseñador. Establezca la s DropDownList `ID` propiedad `Categories` y su `AutoPostBack` propiedad `True`. A continuación, en la etiqueta inteligente, de enlazar la DropDownList a un nuevo origen ObjectDataSource denominado `CategoriesDataSource`. Configurar el origen ObjectDataSource para que recupera sus datos desde el `CategoriesBLL` clase s `GetCategories` método. Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Recuperar datos desde el método de clase CategoriesBLL s GetCategories](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperar datos desde el `CategoriesBLL` clase s `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Después de completar el ObjectDataSource wizard, configure la DropDownList para mostrar el `CategoryName` campo de datos y usar el `CategoryID` campo como el `Value` para cada `ListItem`.

En este momento, el marcado declarativo de s DropDownList y ObjectDataSource debe similar al siguiente:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

A continuación, arrastre un control GridView en el diseñador, colocarlo bajo la DropDownList. Establezca la s GridView `ID` a `ProductsByCategory` y, en la etiqueta inteligente, de enlazarla a un nuevo origen ObjectDataSource denominado `ProductsByCategoryDataSource`. Configurar la `ProductsByCategoryDataSource` ObjectDataSource para usar el `ProductsBLLWithSprocs` (clase), que se puede recuperar sus datos usando el `GetProductsByCategoryID(categoryID)` método. Puesto que este GridView sólo se usará para mostrar los datos, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None) y haga clic en siguiente.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurar el origen ObjectDataSource que se usarán el `ProductsBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Recuperar datos desde el método GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperar datos desde el `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


El método seleccionado en la ficha Seleccionar espera un parámetro, por lo que el último paso del asistente nos pide el origen del parámetro s. Establece la lista desplegable de origen de parámetro en el Control y elija el `Categories` control en la lista desplegable de ControlID. Haga clic en Finalizar para completar al asistente.


[![Usar categorías DropDownList como el origen del parámetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: Use el `Categories` DropDownList como origen de la `categoryID` parámetro ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Al finalizar al Asistente de ObjectDataSource, Visual Studio agregará BoundFields y un CampoCasillaVerificación para cada uno de los campos de datos del producto. No dude en personalizar estos campos como considere oportuno.

Visite la página a través de un explorador. Cuando se visita la página que seleccionar la categoría bebidas y los productos correspondientes de la cuadrícula. Cambio de la lista desplegable a una categoría alternativa, como la figura 12 se muestra, hace que una devolución de datos y vuelve a cargar la cuadrícula con los productos de la categoría recién seleccionada.


[![Se muestran los productos en la categoría generar](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: se muestran los productos de la categoría generar ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Paso 5: Ajuste de un procedimiento almacenado de instrucciones s dentro del ámbito de una transacción

En el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial analizamos las técnicas para realizar una serie de instrucciones de modificación de base de datos dentro del ámbito de una transacción. Recuerde que las modificaciones realizadas bajo el paraguas de una transacción, ya sea todas tengan éxito o con todos los errores, garantizar la atomicidad. Técnicas para usar transacciones incluyen:

- Mediante las clases en el `System.Transactions` espacio de nombres
- Tener la capa de acceso a datos usa clases ADO.NET como `SqlTransaction`, y
- Agregar los comandos de transacción de Transact-SQL directamente en el procedimiento almacenado

El *ajuste modificaciones de base de datos dentro de una transacción* tutorial utiliza las clases de ADO.NET en la capa DAL. El resto de este tutorial examina cómo administrar una transacción mediante comandos de T-SQL desde dentro de un procedimiento almacenado.

Los tres comandos SQL claves para iniciar manualmente, confirmar y revertir una transacción son `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, y `ROLLBACK TRANSACTION`, respectivamente. Al igual que con el enfoque ADO.NET, cuando se usan transacciones desde dentro de un procedimiento almacenado se debe aplicar el siguiente patrón:

1. Indicar el inicio de una transacción.
2. Ejecute las instrucciones SQL que componen la transacción.
3. Si se produce un error en cualquiera de las instrucciones del paso 2, revierta la transacción.
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Este patrón puede implementarse en la sintaxis de Transact-SQL con la siguiente plantilla:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

La plantilla empieza definiendo una `TRY...CATCH` bloquear una construcción de nueva en SQL Server 2005. Al igual que con `Try...Catch` bloquea en Visual Basic, la instrucción SQL `TRY...CATCH` bloque ejecuta las instrucciones en el `TRY` bloque. Si cualquier instrucción genera un error, el control se transfiere inmediatamente a la `CATCH` bloque.

Si no hay ningún error de ejecución de las instrucciones SQL que utilizan la transacción, el `COMMIT TRANSACTION` instrucción confirma los cambios y se completa la transacción. Si, sin embargo, una de las instrucciones produce un error, el `ROLLBACK TRANSACTION` en el `CATCH` bloque devuelve la base de datos a su estado antes del comienzo de la transacción. Además, el procedimiento almacenado genera un error mediante el [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), lo que hace que un `SqlException` que se genera en la aplicación.

> [!NOTE]
> Puesto que el `TRY...CATCH` bloque es una novedad de SQL Server 2005, la plantilla anterior no funcionará si está utilizando versiones anteriores de Microsoft SQL Server. Si no utiliza SQL Server 2005, consulte [administrar transacciones en procedimientos almacenados de SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para una plantilla que funcione con otras versiones de SQL Server.


Permiten s observar un ejemplo concreto. Existe una restricción foreign key entre el `Categories` y `Products` tablas, lo que significa que cada `CategoryID` campo el `Products` tabla debe asignarse a un `CategoryID` valor en el `Categories` tabla. Cualquier acción que infringe esta restricción, por ejemplo, si se intenta eliminar una categoría que tiene asociados los productos, provoca una infracción de restricción de clave externa. Para comprobarlo, vuelve al ejemplo de actualización y eliminación de los datos binarios existentes en la sección trabajar con datos binarios (`~/BinaryData/UpdatingAndDeleting.aspx`). Esta página enumera cada categoría en el sistema, junto con los botones Editar y eliminar (consulte la figura 13), pero si intenta eliminar una categoría que tiene asociados productos - como bebidas: la eliminación se produce un error debido a una infracción de restricción de clave externa (vea la figura 14).


[![Cada categoría se muestra en un control GridView con Editar y eliminar botones](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: cada categoría se muestra en un control GridView con botones Editar y eliminar ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![No se puede eliminar una categoría de productos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: no se puede eliminar una categoría de productos existentes ([haga clic aquí para ver imagen en tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Sin embargo, imagine que deseamos permitir categorías para eliminarse, independientemente de si han asociado los productos. ¿Debe eliminarse una categoría de productos, imagine que desea eliminar también sus productos existentes (aunque otra opción sería simplemente establecer sus productos `CategoryID` valores `NULL`). Esta funcionalidad podría implementarse a través de las reglas en cascada de la restricción foreign key. Como alternativa, podríamos crear un procedimiento almacenado que acepta un `@CategoryID` parámetro de entrada y, cuando se invoca, elimina explícitamente todos los productos asociados y, a continuación, en la categoría especificada.

Nuestro primer intento de dicho procedimiento almacenado podría parecerse a lo siguiente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Aunque esta acción eliminará definitivamente la categoría y los productos asociados, no lo bajo el paraguas de una transacción. Imagine que hay alguna otra restricción de clave externa en `Categories` que impediría la eliminación de una determinada `@CategoryID` valor. El problema es que en este caso todos los productos se eliminarán antes de intentar eliminar la categoría. El resultado neto es que dicha categoría, este procedimiento almacenado quitarían todos sus productos mientras seguía siendo la categoría de puesto que todavía tiene relacionados registros de otra tabla.

Si el procedimiento almacenado se ajusta dentro del ámbito de una transacción, sin embargo, las eliminaciones para la `Products` tabla se revertirán ante un error al eliminar `Categories`. El siguiente script de procedimiento almacenado utiliza una transacción para garantizar la atomicidad entre los dos `DELETE` instrucciones:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Dedique un momento para agregar el `Categories_Delete` procedimiento almacenado a la base de datos Northwind. Consulte paso 1 para obtener instrucciones sobre cómo agregar procedimientos almacenados a una base de datos.

## <a name="step-6-updating-thecategoriestableadapter"></a>Paso 6: Actualización el`CategoriesTableAdapter`

Mientras se ha agregado el `Categories_Delete` procedimiento almacenado a la base de datos, la capa DAL actualmente está configurado para usar instrucciones SQL ad hoc para realizar la eliminación. Es necesario actualizar el `CategoriesTableAdapter` e indicarle que use el `Categories_Delete` procedimiento almacenado en su lugar.

> [!NOTE]
> Anteriormente en este tutorial que estábamos trabajando el `NorthwindWithSprocs` conjunto de datos. Pero ese conjunto de datos solo tiene una sola entidad, `ProductsDataTable`, y tenemos que trabajar con categorías. Por lo tanto, para el resto de este tutorial cuando hable Data Access Layer I m que hace referencia a la `Northwind` conjunto de datos, lo que se crea por primera vez en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial.


Abra Northwind DataSet, seleccione el `CategoriesTableAdapter`y vaya a la ventana Propiedades. Las listas de la ventana de propiedades el `InsertCommand`, `UpdateCommand`, `DeleteCommand`, y `SelectCommand` utilizado por el TableAdapter, así como su información de conexión y nombre. Expanda el `DeleteCommand` propiedad para ver sus detalles. Como se muestra en la figura 15, el `DeleteCommand` s `ComamndType` propiedad está establecida en texto, que le indica que enviar el mensaje de texto el `CommandText` propiedad como una consulta SQL ad hoc.


![Seleccione el CategoriesTableAdapter en el diseñador para ver sus propiedades en la ventana Propiedades](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: seleccione el `CategoriesTableAdapter` en el diseñador para ver sus propiedades en la ventana Propiedades


Para cambiar esta configuración, seleccione el texto (DeleteCommand) en la ventana Propiedades y elija (nuevo) en la lista desplegable. Esto borrará la configuración correspondiente a la `CommandText`, `CommandType`, y `Parameters` propiedades. A continuación, establezca el `CommandType` propiedad `StoredProcedure` y, a continuación, escriba el nombre de procedimiento almacenado para el `CommandText` (`dbo.Categories_Delete`). Si se asegura que escriba las propiedades en este orden: primero el `CommandType` y, a continuación, el `CommandText` -Visual Studio se rellenará automáticamente la colección de parámetros. Si no escribe estas propiedades en este orden, tendrá que agregar manualmente los parámetros a través del Editor de colección de parámetros. En cualquier caso, lo s prudente hacer clic en el botón de puntos suspensivos en la propiedad de los parámetros para mostrar el Editor de colección de parámetros para comprobar que se han realizado los cambios de configuración de parámetro correctos (vea la figura 16). Si no ve ningún parámetro en el cuadro de diálogo, agregue el `@CategoryID` parámetro manualmente (no es necesario agregar el `@RETURN_VALUE` parámetro).


![Asegúrese de que la configuración de parámetros son correctas](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: asegúrese de que la configuración de parámetros son correctas


Una vez que se ha actualizado la capa DAL, al eliminar una categoría automáticamente eliminará todos sus productos asociados y hacerlo bajo el paraguas de una transacción. Para comprobarlo, volver a la página de actualización y eliminación de los datos binarios existentes y haga clic en el botón Eliminar para una de las categorías. Con un solo clic del mouse, la categoría y todos sus productos asociados se eliminarán.

> [!NOTE]
> Antes de probar la `Categories_Delete` procedimiento almacenado, lo que eliminará un número de productos junto con la categoría seleccionada, puede resultar prudente hacer una copia de seguridad de la base de datos. Si usas el `NORTHWND.MDF` en la base de datos `App_Data`, simplemente cierre Visual Studio y copie los archivos MDF y LDF en `App_Data` a otra carpeta. Después de probar la funcionalidad, puede restaurar la base de datos al cerrar Visual Studio y reemplazando el actual MDF y LDF archivos en `App_Data` con las copias de seguridad.


## <a name="summary"></a>Resumen

Mientras el Asistente para la s de TableAdapter generará automáticamente los procedimientos almacenados para nosotros, hay ocasiones en que podríamos ya tienen estos procedimientos almacenados creados o desea crearlos manualmente o con otras herramientas en su lugar. Para dar cabida a estos escenarios, el TableAdapter también puede configurarse para que apunte a un procedimiento almacenado existente. En este tutorial explicamos cómo agregar manualmente los procedimientos almacenados a una base de datos a través del entorno de Visual Studio y cómo conectar los métodos de TableAdapter s a estos procedimientos almacenados. También examinamos los comandos de T-SQL y el patrón de secuencia de comandos que se usa para iniciar, confirmar y revertir las transacciones desde dentro de un procedimiento almacenado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Hilton Geisenow, S ren Jacob Lauritsen y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Siguiente](updating-the-tableadapter-to-use-joins-vb.md)
