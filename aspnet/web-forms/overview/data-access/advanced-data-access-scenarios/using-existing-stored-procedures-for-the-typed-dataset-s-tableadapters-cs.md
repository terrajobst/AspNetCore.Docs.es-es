---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Usar existente los procedimientos almacenados para los TableAdapters del conjunto de datos con tipo (C#) | Documentos de Microsoft
author: rick-anderson
description: En el tutorial anterior, hemos visto cómo utilizar al Asistente de TableAdapter para generar nuevos procedimientos almacenados. En este tutorial se obtenga información acerca de cómo el mismo TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: df8a714325ce99db615eddc3d457da5c926919ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Usar existente los procedimientos almacenados para los TableAdapters del conjunto de datos con tipo (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) o [descarga de PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> En el tutorial anterior, hemos visto cómo utilizar al Asistente de TableAdapter para generar nuevos procedimientos almacenados. En este tutorial se obtenga información acerca de cómo el mismo asistente de TableAdapter puede trabajar con procedimientos almacenados existentes. También se aprenderá a agregar manualmente los nuevos procedimientos almacenados a nuestra base de datos.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) hemos visto cómo la s de conjunto de datos con tipo TableAdapters podría configurarse para utilizar procedimientos almacenados para el acceso a datos en lugar de "ad-hoc" instrucciones de SQL. En concreto, se examina cómo hacer que el Asistente de TableAdapter para crear automáticamente estos procedimientos almacenados. Al migrar una aplicación heredada para ASP.NET 2.0 o cuando se crea un sitio Web de ASP.NET 2.0 alrededor de un modelo de datos existente, lo más probable es que la base de datos ya contiene los procedimientos almacenados que necesitamos. O bien, quizás prefiera crear los procedimientos almacenados a mano o mediante alguna herramienta distinto del Asistente de TableAdapter que genera automáticamente los procedimientos almacenados.

En este tutorial, veremos cómo configurar el TableAdapter para usar procedimientos almacenados existentes. Puesto que la base de datos Northwind tiene solo un pequeño conjunto de procedimientos almacenados integrados, también veremos los pasos necesarios para agregar manualmente los nuevos procedimientos almacenados en la base de datos a través del entorno de Visual Studio. Permiten s empiece a trabajar.

> [!NOTE]
> En el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial agregamos métodos al TableAdapter para admitir transacciones (`BeginTransaction`, `CommitTransaction`, y así sucesivamente). Como alternativa, las transacciones se pueden administrar completamente dentro de un procedimiento almacenado, que no se requiere ninguna modificación en el código de capa de acceso a datos. En este tutorial se explorará los comandos de T-SQL que se usa para ejecutar un procedimiento almacenado s las instrucciones dentro del ámbito de una transacción.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Paso 1: Agregar procedimientos almacenados en la base de datos Northwind

Visual Studio resulta muy sencillo agregar nuevos procedimientos almacenados para una base de datos. S permiten agregar un nuevo procedimiento almacenado a la base de datos de Northwind que devuelve todas las columnas de la `Products` tabla para las que tienen un determinado `CategoryID` valor. En la ventana Explorador de servidores, expanda la base de datos Northwind para que se muestren las carpetas: diagramas de base de datos, tablas, vistas etc. Como vimos en el tutorial anterior, la carpeta procedimientos almacenados contiene la base de datos s los procedimientos almacenados existentes. Para agregar un nuevo procedimiento almacenado, haga clic en la carpeta procedimientos almacenados y elija la opción de agregar nuevo procedimiento almacenado en el menú contextual.


[![Haga clic en la carpeta procedimientos almacenados y agregar un nuevo procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: haga clic en la carpeta de procedimientos almacenados y agregar un nuevo procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Como se muestra en la figura 1, al seleccionar la opción de agregar nuevo procedimiento almacenado abre una ventana de script en Visual Studio con el esquema de la secuencia de comandos SQL necesario para crear el procedimiento almacenado. Es nuestro trabajo consiste en dar forma a esta secuencia de comandos y ejecutarlo, momento en que el procedimiento almacenado se agregará a la base de datos.

Escriba el siguiente script:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Este script, cuando se ejecuta, agregará un nuevo procedimiento almacenado a la base de datos de Northwind denominado `Products_SelectByCategoryID`. Este procedimiento almacenado acepta un parámetro de entrada (`@CategoryID`, del tipo `int`) y devuelve todos los campos para los productos que tengan una coincidencia `CategoryID` valor.

Para ejecutar este procedimiento `CREATE PROCEDURE` de secuencias de comandos y agregue el procedimiento almacenado a la base de datos, haga clic en el icono Guardar en la barra de herramientas o aciertos CTRL+s. Una vez hecho esto, las actualizaciones de la carpeta procedimientos almacenados, el procedimiento almacenado que muestra el recién creado. Además, la secuencia de comandos en la ventana cambiará detalle de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` a `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Agrega un nuevo procedimiento almacenado a la base de datos, mientras que `ALTER PROCEDURE` actualiza uno existente. Puesto que el inicio de la secuencia de comandos ha cambiado a `ALTER PROCEDURE`, cambiar los procedimientos almacenados de entrada de parámetros o instrucciones SQL y haga clic en el icono Guardar actualizará el procedimiento almacenado con estos cambios.

La figura 2 muestra Visual Studio después de la `Products_SelectByCategoryID` procedimiento almacenado se ha guardado.


[![El procedimiento almacenado Products_SelectByCategoryID se ha agregado a la base de datos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Figura 2**: el procedimiento almacenado `Products_SelectByCategoryID` se ha agregado a la base de datos ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Paso 2: Configuración de TableAdapter para utilizar un procedimiento almacenado existente

Ahora que el `Products_SelectByCategoryID` procedimiento almacenado se ha agregado a la base de datos, pero podemos concigure la capa de acceso a datos para utilizar este procedimiento almacenado cuando se llama a uno de sus métodos. En concreto, se agregará un `GetProducstByCategoryID(categoryID)` método a la `ProductsTableAdapter` en el `NorthwindWithSprocs` conjunto de datos con tipo que llama el `Products_SelectByCategoryID` que acabamos de crear el procedimiento almacenado.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos. Haga doble clic en el `ProductsTableAdapter` y elija Agregar consulta para iniciar el Asistente para configuración de consultas de TableAdapter. En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) hemos optado por hacer TableAdapter cree un nuevo procedimiento almacenado para que podamos. Para este tutorial, sin embargo, queremos conecte el nuevo método de TableAdapter para existente `Products_SelectByCategoryID` procedimiento almacenado. Por lo tanto, elija la opción de procedimiento almacenado existente de uso desde el paso del asistente s primero y, a continuación, haga clic en siguiente.


[![Elija el uso existente de la opción de procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Figura 3**: elija el uso existente almacenado procedimiento opción ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


La pantalla siguiente proporciona que una lista desplegable rellena con la base de datos s procedimientos almacenados. Al seleccionar un procedimiento almacenado, enumera sus parámetros de entrada a la izquierda y los campos de datos devueltos (si existe) de la derecha. Elija la `Products_SelectByCategoryID` el procedimiento almacenado de la lista y haga clic en siguiente.


[![Elegir el Products_SelectByCategoryID procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Figura 4**: elegir el `Products_SelectByCategoryID` procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


La siguiente pantalla nos pregunta qué tipo de datos devuelto por el procedimiento almacenado y nuestra respuesta aquí determina el tipo devuelto por el método de s de TableAdapter. Por ejemplo, si se indican que se devuelven datos tabulares, el método devolverá un `ProductsDataTable` instancia rellenado con los registros devueltos por el procedimiento almacenado. En cambio, si se indica que este procedimiento almacenado devuelve un valor único TableAdapter devolverá un `object` que se asigna el valor de la primera columna del primer registro devuelto por el procedimiento almacenado.

Puesto que la `Products_SelectByCategoryID` procedimiento almacenado devuelve todos los productos que pertenecen a una categoría determinada, elija la primera respuesta - TDS - y haga clic en siguiente.


[![Indicar que el procedimiento almacenado devuelve datos tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Figura 5**: indicar que el procedimiento almacenado devuelve datos tabulares ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Todo lo que queda es indicar qué patrones de método para su uso seguido de los nombres de estos métodos. Deje ambos el relleno una DataTable y retorno comprueban las opciones de una tabla de datos, pero cambiar el nombre de los métodos para `FillByCategoryID` y `GetProductsByCategoryID`. A continuación, haga clic en siguiente para ver un resumen de las tareas que realizará el asistente. Si todo parece correcto, haga clic en Finalizar.


[![Nombre de lo métodos FillByCategoryID y GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 6**: el nombre de los métodos `FillByCategoryID` y `GetProductsByCategoryID` ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Los métodos de TableAdapter que acabamos de crear, `FillByCategoryID` y `GetProductsByCategoryID`, espera un parámetro de entrada de tipo `int`. Este valor de parámetro de entrada se pasa al procedimiento almacenado a través de su `@CategoryID` parámetro. Si modifica el `Products_SelectByCategory` almacena los parámetros del procedimiento s, deberá actualizar los parámetros de estos métodos de TableAdapter. Como se describe en el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), esto puede hacerse de dos maneras: manualmente agregando o quitando los parámetros de la colección de parámetros o volviendo a ejecutar el Asistente de TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Paso 3: Agregar un`GetProductsByCategoryID(categoryID)`método a la capa BLL

Con el `GetProductsByCategoryID` DAL método completa el paso siguiente es proporcionar acceso a este método en la capa de lógica de negocios. Abra la `ProductsBLLWithSprocs` archivo de clase y agregue el método siguiente:


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Este método BLL simplemente devuelve el `ProductsDataTable` devuelto desde el `ProductsTableAdapter` s `GetProductsByCategoryID` método. El `DataObjectMethodAttribute` atributo proporciona metadatos usados por el Asistente para configurar origen de datos de ObjectDataSource s. En concreto, este método aparecerá en la lista de desplegable seleccione ficha s.

## <a name="step-4-displaying-products-by-category"></a>Paso 4: Mostrar productos por categoría

Para probar el recién agregado `Products_SelectByCategoryID` procedimiento almacenado y los métodos correspondientes de DAL y BLL, permiten s crear una página ASP.NET que contiene un DropDownList y GridView. DropDownList mostrará una lista de todas las categorías en la base de datos mientras el control GridView muestra los productos que pertenecen a la categoría seleccionada.

> [!NOTE]
> Se ha creado interfaces de maestro y detalles mediante las listas desplegables en los tutoriales anteriores. Para obtener información más detallada en la implementación de estos informes de maestro y detalles, consulte la [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial.


Abra la `ExistingSprocs.aspx` página en el `AdvancedDAL` carpeta y arrastre DropDownList desde el cuadro de herramientas hasta el diseñador. Establecer la s DropDownList `ID` propiedad `Categories` y su `AutoPostBack` propiedad `true`. A continuación, en la etiqueta inteligente, de enlazar DropDownList a un nuevo ObjectDataSource denominado `CategoriesDataSource`. Configurar el ObjectDataSource para que recupere sus datos de la `CategoriesBLL` clase s. `GetCategories` método. Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las fichas (ninguno).


[![Recuperar datos desde el método de GetCategories CategoriesBLL clase s.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 7**: recuperar datos de la `CategoriesBLL` clase s `GetCategories` método ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las pestañas en (ninguno)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Figura 8**: establecer las listas de lista desplegable en la actualización, INSERCIÓN y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


Después de completar el Asistente de ObjectDataSource configurar DropDownList para mostrar el `CategoryName` campo de datos y usar el `CategoryID` campo como el `Value` para cada `ListItem`.

En este momento, el marcado declarativo de s DropDownList y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

A continuación, arrastre un control GridView al diseñador, lo colocan debajo DropDownList. Establecer la s GridView `ID` a `ProductsByCategory` y, en su etiqueta inteligente, enlazarla a un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource`. Configurar la `ProductsByCategoryDataSource` ObjectDataSource para usar el `ProductsBLLWithSprocs` (clase), tiene recupera sus datos mediante la `GetProductsByCategoryID(categoryID)` método. Puesto que este GridView solo se usará para mostrar los datos, establezca las listas desplegables de la actualización, INSERCIÓN y eliminar tabulaciones en (ninguno) y haga clic en siguiente.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Figura 9**: configurar el ObjectDataSource que se utilizan los `ProductsBLLWithSprocs` clase ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Recuperar datos desde el método GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Figura 10**: recuperar datos de la `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


El método seleccionado en la ficha Seleccionar espera un parámetro, por lo que el último paso del asistente nos pide el origen del parámetro s. Establezca la lista desplegable de origen de parámetro para el Control y elija el `Categories` control de la lista desplegable de ControlID. Haga clic en Finalizar para completar al asistente.


[![Usar las categorías de DropDownList como el origen del parámetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 11**: Use la `Categories` DropDownList como el origen de la `categoryID` parámetro ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


Al finalizar al Asistente de ObjectDataSource, Visual Studio agregará BoundFields y un CampoCasillaVerificación para cada uno de los campos de datos de producto. No dude en personalizar estos campos como considere oportuno.

Visite la página a través de un explorador. Al visitar la página que seleccionar la categoría de bebidas y los productos correspondientes de la cuadrícula. Cambio de la lista desplegable a una categoría alternativa, como la figura 12 muestra, provoca una devolución de datos y vuelve a cargar la cuadrícula con los productos de la categoría recién seleccionada.


[![Se muestran los productos en la categoría generar](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 12**: se muestran los productos en la categoría generar ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Paso 5: Ajuste de un procedimiento almacenado de instrucciones de s dentro del ámbito de una transacción

En el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial analizamos las técnicas para realizar una serie de instrucciones de modificación de base de datos dentro del ámbito de una transacción. Recuerde que las modificaciones realizadas bajo el paraguas de una transacción, ya sea todas tengan éxito o con todos los errores, para garantizar la atomicidad. Técnicas para el uso de transacciones incluyen:

- Utilizar las clases en el `System.Transactions` espacio de nombres
- Con la capa de acceso a datos utilizar clases ADO.NET como `SqlTransaction`, y
- Agregar los comandos de transacción de T-SQL directamente dentro del procedimiento almacenado

El *ajuste modificaciones de base de datos dentro de una transacción* tutorial utiliza las clases de ADO.NET en la capa DAL. El resto de este tutorial, examina cómo administrar una transacción mediante comandos de T-SQL desde dentro de un procedimiento almacenado.

Los tres comandos SQL claves de inicio manual, confirmar y revertir una transacción son `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, y `ROLLBACK TRANSACTION`, respectivamente. Al igual que con el enfoque ADO.NET, cuando se usan transacciones desde dentro de un procedimiento almacenado que se deba aplicar el siguiente patrón:

1. Indicar el inicio de una transacción.
2. Ejecute las instrucciones SQL que componen la transacción.
3. Si se produce un error en cualquiera de las instrucciones del paso 2, revertir la transacción
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Este modelo puede implementarse en la sintaxis de T-SQL con la siguiente plantilla:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

La plantilla se inicia mediante la definición de un `TRY...CATCH` bloquear, una construcción de nueva a SQL Server 2005. Al igual que con `try...catch` bloques en C#, la instrucción SQL `TRY...CATCH` bloque ejecuta las instrucciones en el `TRY` bloque. Si cualquier instrucción produce un error, el control se transfiere inmediatamente a la `CATCH` bloque.

Si no hay ningún error de ejecución de las instrucciones SQL que utilizan la transacción, la `COMMIT TRANSACTION` instrucción confirma los cambios y finaliza la transacción. Si, sin embargo, una de las instrucciones provoca un error, el `ROLLBACK TRANSACTION` en el `CATCH` bloque devuelve la base de datos a su estado antes del inicio de la transacción. El procedimiento almacenado también genera un error mediante el [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), lo que hace que un `SqlException` que se genera en la aplicación.

> [!NOTE]
> Puesto que la `TRY...CATCH` bloque es una novedad de SQL Server 2005, la plantilla anterior no funcionará si usa versiones anteriores de Microsoft SQL Server. Si no se usa SQL Server 2005, consulte [administrar transacciones en procedimientos almacenados de SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para una plantilla que funciona con otras versiones de SQL Server.


Permiten s mirar un ejemplo concreto. Existe una restricción de clave externa entre la `Categories` y `Products` tablas, lo que significa que cada `CategoryID` campo el `Products` tabla debe asignarse a un `CategoryID` valor en el `Categories` tabla. Cualquier acción que infringe esta restricción, por ejemplo, si intenta eliminar una categoría asociada a productos, provoca una infracción de restricción de clave externa. Para comprobarlo, vuelve al ejemplo de actualización y eliminación de los datos binarios existentes en la sección trabajar con datos binarios (`~/BinaryData/UpdatingAndDeleting.aspx`). Esta página enumera cada categoría en el sistema junto con los botones Editar y eliminar (Véase la figura 13), pero si intenta eliminar una categoría de productos - como bebidas - asociada a la eliminación se produce un error debido a una infracción de restricción de clave externa (Véase la figura 14).


[![Cada categoría se muestra en un control GridView con Editar y eliminar botones](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Figura 13**: cada categoría se muestra en un control GridView con Editar y eliminar botones ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![No se puede eliminar una categoría que tiene los productos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Figura 14**: no se puede eliminar una categoría que tiene los productos existentes ([haga clic aquí para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


Sin embargo, imagine que quiere permitir categorías eliminarse independientemente de si tienen asociadas productos. ¿Debe eliminarse una categoría de productos, suponga que desea eliminar también los productos existentes (aunque otra opción sería basta con establecer sus productos `CategoryID` valores `NULL`). Esta funcionalidad podría implementarse a través de las reglas cascade de la restricción foreign key. Como alternativa, podríamos crear un procedimiento almacenado que acepta un `@CategoryID` parámetro de entrada y, cuando se invoca, elimina explícitamente todos los productos asociados y, a continuación, en la categoría especificada.

Nuestro primer intento de dicho procedimiento almacenado podría ser similar al siguiente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Mientras se eliminará definitivamente los productos asociados y la categoría, no lo bajo el paraguas de una transacción. Imagine que hay alguna otra restricción de clave externa en `Categories` que impediría la eliminación de una determinada `@CategoryID` valor. El problema es que en este caso todos los productos se eliminarán antes de que se intenta eliminar la categoría. El resultado neto es que dicha categoría, este procedimiento almacenado quitaría todos sus productos mientras seguía siendo la categoría porque aún tiene relacionados registros de otra tabla.

Si el procedimiento almacenado se ajusta dentro del ámbito de una transacción, sin embargo, las eliminaciones para la `Products` tabla se revertirá en el caso de un error al eliminar `Categories`. El siguiente script de procedimiento almacenado utiliza una transacción para garantizar la atomicidad entre los dos `DELETE` instrucciones:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Tómese un momento para agregar el `Categories_Delete` procedimiento almacenado para la base de datos Northwind. Hacen referencia al paso 1 para obtener instrucciones sobre cómo agregar procedimientos almacenados a una base de datos.

## <a name="step-6-updating-thecategoriestableadapter"></a>Paso 6: Actualizar el`CategoriesTableAdapter`

Mientras se ha agregado el `Categories_Delete` un procedimiento almacenado para la base de datos, la capa DAL está actualmente configurado para usar instrucciones SQL ad hoc para realizar la eliminación. Necesitamos actualizar la `CategoriesTableAdapter` e indicarle que se va a usar el `Categories_Delete` procedimiento almacenado en su lugar.

> [!NOTE]
> Anteriormente en este tutorial estuviéramos trabajando con el `NorthwindWithSprocs` conjunto de datos. Pero ese conjunto de datos solo tiene una sola entidad, `ProductsDataTable`, y es necesario para que funcione con categorías. Por lo tanto, para el resto de este tutorial cuando habla de la m datos acceso capa I que hace referencia a la `Northwind` conjunto de datos, que se crea por primera vez en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial.


Abra Northwind DataSet, seleccione la `CategoriesTableAdapter`y vaya a la ventana Propiedades. Las listas de la ventana de propiedades el `InsertCommand`, `UpdateCommand`, `DeleteCommand`, y `SelectCommand` utilizado por el TableAdapter, así como su información de conexión y nombre. Expanda el `DeleteCommand` propiedad para ver sus detalles. Como se muestra en la figura 15, el `DeleteCommand` s `ComamndType` propiedad está establecida en texto, lo que indica que se envíe el texto el `CommandText` propiedad como una consulta SQL ad hoc.


![Seleccione el CategoriesTableAdapter en el diseñador para ver sus propiedades en la ventana Propiedades](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Figura 15**: seleccione el `CategoriesTableAdapter` en el diseñador para ver sus propiedades en la ventana Propiedades


Para cambiar esta configuración, seleccione el texto (DeleteCommand) en la ventana Propiedades y elija (nuevo) en la lista desplegable. Esta acción borrará los valores para la `CommandText`, `CommandType`, y `Parameters` propiedades. A continuación, establezca el `CommandType` propiedad `StoredProcedure` y, a continuación, escriba el nombre del procedimiento almacenado para el `CommandText` (`dbo.Categories_Delete`). Si se asegure de que especifique las propiedades en este orden: primero la `CommandType` y, a continuación, el `CommandText` -Visual Studio rellenará automáticamente la colección de parámetros. Si no escribe estas propiedades en este orden, tendrá que agregar manualmente los parámetros a través del Editor de colección de parámetros. En cualquier caso, se s prudente hacer clic en el botón de puntos suspensivos en la propiedad de parámetros para mostrar el Editor de colección de parámetros para comprobar que se han realizado los cambios de configuración de parámetros correctos (Véase la figura 16). Si no ve ningún parámetro en el cuadro de diálogo, agregue el `@CategoryID` parámetro manualmente (no es necesario agregar el `@RETURN_VALUE` parámetro).


![Asegúrese de que la configuración de parámetros es correcto](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 16**: asegúrese de que la configuración de parámetros es correcto


Una vez que se ha actualizado la capa DAL, al eliminar una categoría automáticamente eliminará todos sus productos asociados y hacen esto bajo el paraguas de una transacción. Para comprobarlo, volver a la página de actualización y eliminación de los datos binarios existentes y haga clic en el botón Eliminar para una de las categorías. Con un solo clic del mouse, la categoría y todos sus productos asociados se eliminarán.

> [!NOTE]
> Antes de probar el `Categories_Delete` procedimiento almacenado, lo que eliminará un número de productos junto con la categoría seleccionada, podría ser conveniente realizar una copia de seguridad de la base de datos. Si usas el `NORTHWND.MDF` en la base de datos `App_Data`, simplemente cierre Visual Studio y copie los archivos MDF y LDF en `App_Data` a otra carpeta. Después de probar la funcionalidad, puede restaurar la base de datos, el cierre de Visual Studio y reemplazando el actual MDF y LDF archivos `App_Data` con las copias de seguridad.


## <a name="summary"></a>Resumen

Mientras el Asistente para la s de TableAdapter generará automáticamente los procedimientos almacenados para que podamos, hay ocasiones en que tengamos que ya tiene tales procedimientos almacenados creados o desea crearlos manualmente o con otras herramientas en su lugar. Para dar cabida a estos escenarios, lo TableAdapter también pueden configurarse para que apunte a un procedimiento almacenado existente. En este tutorial explicamos cómo agregar manualmente los procedimientos almacenados en una base de datos a través del entorno de Visual Studio y cómo conectar los métodos de TableAdapter s a estos procedimientos almacenados. También se examinan los comandos de T-SQL y un patrón de secuencia de comandos que se usa para iniciar, confirmar y revertir las transacciones desde dentro de un procedimiento almacenado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Hilton Geisenow, S ren Jacob Lauritsen y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Siguiente](updating-the-tableadapter-to-use-joins-cs.md)
