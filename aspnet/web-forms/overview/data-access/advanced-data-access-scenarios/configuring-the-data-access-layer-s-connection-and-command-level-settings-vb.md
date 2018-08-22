---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Configuración de las opciones de nivel de conexión y comando de la capa de acceso a datos (VB) | Microsoft Docs
author: rick-anderson
description: Los TableAdapters dentro de un conjunto de datos con tipo se ocupan automáticamente de conectarse a la base de datos, emisión de comandos y rellenar un DataTable con los resultados...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: d44372ef3eaf7634d3bf3a82bd2c1eb1d710f786
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835962"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Configuración de las opciones de nivel de conexión y comando de la capa de acceso a datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) o [descargar PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> Los TableAdapters dentro de un conjunto de datos con tipo automáticamente se encargan de conectarse a la base de datos, emisión de comandos y rellenar un DataTable con los resultados. Sin embargo, cuando deseemos a cargo de estos detalles nosotros mismos y, en este tutorial, obtenga información sobre cómo acceder a la configuración de nivel de conexión y comando de base de datos en el TableAdapter, hay ocasiones.


## <a name="introduction"></a>Introducción

A lo largo de la serie de tutoriales hemos usado los conjuntos de datos con tipo para implementar la capa de acceso a datos y objetos de negocios de nuestra arquitectura por capas. Como se describe en el [primer tutorial](../introduction/creating-a-data-access-layer-vb.md), la s de conjunto de datos con tipo DataTables sirven como repositorios de datos, mientras que los TableAdapters actúan como contenedores para comunicarse con la base de datos para recuperar y modificar los datos subyacentes. Los TableAdapters encapsulan la complejidad que implica trabajar con la base de datos y nos evita tener que escribir código para conectarse a la base de datos, emitir un comando o rellenar los resultados en un objeto DataTable.

Veces, sin embargo, cuando necesitemos madriguera las profundidades del TableAdapter y escribir código que funciona directamente con los objetos de ADO.NET. En el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial, por ejemplo, agregamos métodos a TableAdapter para el principio, confirmar y revertir las transacciones de ADO.NET. Estos métodos usan una instancia interna, creado manualmente `SqlTransaction` objeto que se asignó a TableAdapters `SqlCommand` objetos.

En este tutorial, examinaremos cómo tener acceso a la configuración de nivel de conexión y comando de base de datos en el TableAdapter. En concreto, vamos a agregar funcionalidad a la `ProductsTableAdapter` que habilita el acceso a la cadena de conexión subyacente y configuración de tiempo de espera del comando.

## <a name="working-with-data-using-adonet"></a>Trabajar con datos mediante ADO.NET

Microsoft .NET Framework contiene una amplia gama de clases diseñadas específicamente para trabajar con datos. Estas clases, que se encuentra dentro de la [ `System.Data` espacio de nombres](https://msdn.microsoft.com/library/system.data.aspx), se conocen como el *ADO.NET* clases. Algunas de las clases bajo el paraguas de ADO.NET están vinculados a un determinado *proveedor de datos*. Un proveedor de datos se puede considerar como un canal de comunicación que permite que la información fluya entre las clases de ADO.NET y el almacén de datos subyacente. Existen proveedores generalizados, como OleDb y ODBC, así como los proveedores que están especialmente diseñados para un sistema de base de datos determinada. Por ejemplo, aunque es posible conectarse a una base de datos de Microsoft SQL Server mediante el proveedor OleDb, el proveedor SqlClient es mucho más eficaz, tal como se ha diseñado y optimizado específicamente para SQL Server.

Al tener acceso a datos, normalmente se usa el siguiente patrón:

1. Establecer una conexión a la base de datos.
2. Emitir un comando.
3. Para `SELECT` consultas, trabajar con los registros resultantes.

Hay clases ADO.NET independientes para realizar cada uno de estos pasos. Para conectarse a una base de datos con el proveedor SqlClient, por ejemplo, use el [ `SqlConnection` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Va a emitir un `INSERT`, `UPDATE`, `DELETE`, o `SELECT` comando a la base de datos, use el [ `SqlCommand` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Excepto para el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial, no tuvimos que escribir ni código de ADO.NET de bajo nivel nosotros mismos, ya que el código generado automáticamente de los TableAdapters incluye la funcionalidad necesaria para conectarse a la base de datos, emiten comandos, recuperar los datos y rellenar los datos en tablas de datos. Sin embargo, puede haber ocasiones en que deba personalizar esta configuración de bajo nivel. A través de los pasos siguientes, examinaremos cómo acceder a los objetos ADO.NET lo utilizados internamente los TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Paso 1: Examinar con la propiedad de conexión

Cada clase de TableAdapter tiene un `Connection` propiedad que especifica la información de conexión de base de datos. Este tipo de datos de la propiedad s y `ConnectionString` valor viene determinado por las selecciones realizadas en el Asistente para configuración de TableAdapter. Recuerde que cuando se agrega primero un TableAdapter a un DataSet con tipo este asistente nos pide la base de datos de origen (consulte la figura 1). La lista desplegable en este primer paso incluye esas bases de datos especificadas en el archivo de configuración, así como otras bases de datos en las conexiones de datos de s Explorador de servidores. Si la base de datos que desea usar no existe en la lista desplegable, se puede especificar una nueva conexión de base de datos haciendo clic en el botón nueva conexión y proporcionar la información de conexión necesaria.


[![El primer paso del Asistente para la configuración de TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Figura 1**: el primer paso del Asistente para la configuración de TableAdapter ([haga clic aquí para ver imagen en tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Let s dedique un momento para inspeccionar el código para los TableAdapters `Connection` propiedad. Como se indicó en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, podemos ver el código del TableAdapter generadas automáticamente, vaya a la ventana de vista de clases, cómo llegar a la clase adecuada y, a continuación, haga doble clic en el nombre del miembro.

Vaya a la ventana de vista de clases, vaya al menú Ver y elegir la vista de clases (o escriba Ctrl + Mayús + C). Desde la mitad superior de la ventana de vista de clases, profundizar hasta el `NorthwindTableAdapters` espacio de nombres y seleccione el `ProductsTableAdapter` clase. Esto mostrará la `ProductsTableAdapter` miembros s en la parte inferior la mitad de la vista de clases, tal como se muestra en la figura 2. Haga doble clic en el `Connection` propiedad para ver su código.


![Haga doble clic en la propiedad de conexión en la vista de clases para ver el código generado automáticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Figura 2**: haga doble clic en la propiedad de conexión en la vista de clases para ver el código generado automáticamente


Los TableAdapter s `Connection` propiedad y otro código relacionado con la conexión se indica a continuación:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Cuando se crea una instancia de la clase de TableAdapter, la variable de miembro `_connection` es igual a `Nothing`. Cuando el `Connection` se accede a la propiedad, comprueba primero si el `_connection` se ha creado una instancia de la variable de miembro. Si no, tiene la `InitConnection` se invoca el método, que crea una instancia `_connection` y establece su `ConnectionString` propiedad al valor de cadena de conexión especificada a partir de la configuración de TableAdapter asistente s primero.

El `Connection` propiedad también se puede asignar a un `SqlConnection` objeto. Si lo hace, asocia el nuevo `SqlConnection` objeto con cada uno de los TableAdapters `SqlCommand` objetos.

## <a name="step-2-exposing-connection-level-settings"></a>Paso 2: Exponer la configuración de nivel de conexión

La información de conexión debe permanecer encapsulada dentro del TableAdapter y no estar accesible para el resto de niveles en la arquitectura de aplicación. Sin embargo, puede haber escenarios cuando la información de nivel de conexión de TableAdapter s debe ser accesibles o personalizables para una consulta, un usuario o una página ASP.NET.

Permiten s ampliar el `ProductsTableAdapter` en el `Northwind` conjunto de datos para incluir un `ConnectionString` propiedad que puede usarse por la capa de lógica de negocios para leer o cambiar la cadena de conexión utilizada por el TableAdapter.

> [!NOTE]
> Un *cadena de conexión* es una cadena que especifica la información de conexión de base de datos, como el proveedor debe usar la ubicación de la base de datos, las credenciales de autenticación y otra configuración relacionada con la base de datos. Para obtener una lista de patrones de cadena de conexión utilizado por una variedad de almacenes de datos y proveedores, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


Como se describe en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, se pueden extender las clases de conjunto de datos con tipo s generados automáticamente mediante el uso de clases parciales. En primer lugar, cree una nueva subcarpeta en el proyecto denominado `ConnectionAndCommandSettings` debajo el `~/App_Code/DAL` carpeta.


![Agregar una subcarpeta denominada ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Figura 3**: agregar una subcarpeta denominada `ConnectionAndCommandSettings`


Agregue un nuevo archivo de clase denominado `ProductsTableAdapter.ConnectionAndCommandSettings.vb` y escriba el código siguiente:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Esta clase parcial, se agrega un `Public` propiedad denominada `ConnectionString` a la `ProductsTableAdapter` clase que permite que cualquier capa leer o actualizar la cadena de conexión para el s TableAdapter conexión subyacente.

Con esta clase parcial crear (y guardar), abra el `ProductsBLL` clase. Vaya a uno de los métodos existentes y escriba en `Adapter` y a continuación, presione la tecla de punto para que aparezca en IntelliSense. Debería ver el nuevo `ConnectionString` propiedad disponible en IntelliSense, lo que significa que puede leer mediante programación o ajustar este valor de la capa BLL.

## <a name="exposing-the-entire-connection-object"></a>Exponer el objeto de conexión completa

Esta clase parcial expone sólo una propiedad del objeto de conexión subyacente: `ConnectionString`. Si desea que el objeto toda conexión esté disponible más allá de los confines del TableAdapter, o bien puede cambiar el `Connection` nivel de protección de la propiedad s. El código generado automáticamente que hemos visto en el paso 1 se ha mostrado que TableAdapters `Connection` propiedad está marcada como `Friend`, lo que significa que solo se accesible por clases en el mismo ensamblado. Esto se puede cambiar, sin embargo, a través de los TableAdapters `ConnectionModifier` propiedad.

Abra el `Northwind` conjunto de datos, haga clic en el `ProductsTableAdatper` en el diseñador y vaya a la ventana Propiedades. Allí verá el `ConnectionModifier` establecido en su valor predeterminado, `Assembly`. Para realizar la `Connection` disponible fuera del ensamblado del conjunto de datos con tipo s, cambio de propiedad el `ConnectionModifier` propiedad `Public`.


[![El nivel de accesibilidad de s de propiedad de conexión puede configurarse a través de la propiedad ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Figura 4**: el `Connection` propiedad s accesibilidad nivel pueden configurarse a través de la `ConnectionModifier` propiedad ([haga clic aquí para ver imagen en tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Guarde el conjunto de datos y, a continuación, vuelva a la `ProductsBLL` clase. Como antes, vaya a uno de los métodos existentes y escriba en `Adapter` y a continuación, presione la tecla de punto para que aparezca en IntelliSense. La lista debe incluir un `Connection` propiedad, lo que significa que ahora puede mediante programación leer o asignar cualquier configuración de nivel de conexión de la capa BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Paso 3: Examen de las propiedades relacionadas con el comando

Un TableAdapter consta de una consulta principal que, de forma predeterminada, se autogenerado `INSERT`, `UPDATE`, y `DELETE` instrucciones. Esta consulta principal s `INSERT`, `UPDATE`, y `DELETE` instrucciones se implementan en el código del TableAdapter s como un objeto de adaptador de datos ADO.NET mediante el `Adapter` propiedad. Al igual que con su `Connection` propiedad, el `Adapter` tipo de datos de la propiedad s viene determinada por el proveedor de datos usado. Dado que estos tutoriales usan el proveedor SqlClient, el `Adapter` propiedad es de tipo [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapters `Adapter` propiedad tiene tres propiedades de tipo `SqlCommand` que usa al problema el `INSERT`, `UPDATE`, y `DELETE` instrucciones:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Un `SqlCommand` objeto es responsable de enviar una consulta determinada a la base de datos y tiene propiedades, como: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), que contiene la instrucción de SQL ad hoc o procedimiento almacenado para ejecutar; y [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), que es una colección de `SqlParameter` objetos. Como vimos en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, estos comandos se pueden personalizar los objetos a través de la ventana Propiedades.

Además de la consulta principal, lo TableAdapter puede incluir un número variable de métodos que, cuando se invoca, enviar un comando especificado para la base de datos. El objeto de comando de la consulta principal s y los objetos de comando para todos los métodos adicionales se almacenan en los TableAdapter s `CommandCollection` propiedad.

Let s dedique un momento a examinar el código generado por el `ProductsTableAdapter` en el `Northwind` conjunto de datos para estas dos propiedades y sus variables miembro auxiliares y métodos auxiliares:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

El código para el `Adapter` y `CommandCollection` propiedades de imita mejor el `Connection` propiedad. Hay variables de miembro que contienen los objetos utilizados por las propiedades. Las propiedades `Get` descriptores de acceso de inicio y comprueba si la variable de miembro correspondiente es `Nothing`. Si es así, se llama un método de inicialización que crea una instancia de la variable de miembro y asigna el núcleo de las propiedades relacionadas con el comando.

## <a name="step-4-exposing-command-level-settings"></a>Paso 4: Exponer la configuración de nivel de comando

Idealmente, la información de nivel de comando debe permanecer encapsulada dentro de la capa de acceso a datos. Debería ser necesario esta información en otras capas de la arquitectura, sin embargo, se puede exponer a través de una clase parcial, al igual que con la configuración de nivel de conexión.

Puesto que el TableAdapter sólo tiene una sola `Connection` propiedad, el código para exponer la configuración de nivel de conexión es bastante sencillo. Las cosas son un poco más complicadas cuando se modifica la configuración de nivel de comando porque el TableAdapter puede tener varios objetos de comando: un `InsertCommand`, `UpdateCommand`, y `DeleteCommand`, junto con un número variable de objetos de comando en el `CommandCollection` propiedad. Al actualizar la configuración de nivel de comando, necesitará estas configuraciones se propaguen a todos los objetos de comando.

Por ejemplo, imagine que había algunas consultas de TableAdapter que tardó un extraordinario mucho tiempo en ejecutarse. Al utilizar el TableAdapter para ejecutar una de las consultas, que podría desear aumentar el objeto de comando s [ `CommandTimeout` propiedad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Esta propiedad especifica el número de segundos que deben transcurrir para que ejecute el comando y el valor predeterminado es 30.

Para permitir la `CommandTimeout` propiedad va a ajustar el BLL, agregue el siguiente `Public` método a la `ProductsDataTable` mediante el archivo de clase parcial creado en el paso 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Este método podría invocarse desde el nivel de lógica empresarial o capa de presentación para establecer el tiempo de espera de comando para todos los problemas de comandos por dicha instancia de TableAdapter.

> [!NOTE]
> El `Adapter` y `CommandCollection` propiedades se marcan como `Private`, lo que significa que solo sea accesible desde el código dentro del TableAdapter. A diferencia de la `Connection` propiedad, estos modificadores de acceso no son configurables. Por lo tanto, si tiene que exponga las propiedades de nivel de comandos para otras capas de la arquitectura debe usar el enfoque de la clase parcial descrito anteriormente para proporcionar un `Public` método o propiedad que se lee o escribe en el `Private` objetos de comando.


## <a name="summary"></a>Resumen

Los TableAdapters dentro de un conjunto de datos con tipo sirven para encapsular los detalles de acceso a datos y la complejidad. Uso de TableAdapters, no tenemos que preocuparse sobre cómo escribir código ADO.NET para conectarse a la base de datos, emitir un comando o rellenar los resultados en un objeto DataTable. Se administra completamente automáticamente para nosotros.

Sin embargo, puede haber ocasiones cuando es necesario personalizar los detalles de bajo nivel ADO.NET, como cambiar la cadena de conexión o los valores de tiempo de espera de conexión o un comando predeterminado. El TableAdapter se ha generado mediante automática `Connection`, `Adapter`, y `CommandCollection` propiedades, pero estos son `Friend` o `Private`, de forma predeterminada. Esta información interna puede exponerse mediante la ampliación del TableAdapter utilizando clases parciales para incluir `Public` métodos o propiedades. Como alternativa, los TableAdapter s `Connection` modificador de acceso de propiedad puede configurarse a través de los TableAdapters `ConnectionModifier` propiedad.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy y Hilton Geisenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](working-with-computed-columns-vb.md)
> [Siguiente](protecting-connection-strings-and-other-configuration-information-vb.md)
