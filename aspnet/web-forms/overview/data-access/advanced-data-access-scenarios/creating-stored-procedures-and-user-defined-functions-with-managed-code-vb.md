---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Crear procedimientos almacenados y funciones definidas por el usuario con código (VB) administrado | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 se integra con .NET Common Language Runtime para permitir a los desarrolladores crear objetos de base de datos a través de código administrado. En este tutorial...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a2a4042303fe507af449e83e36f67f4624f579cc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371848"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Crear procedimientos almacenados y funciones definidas por el usuario con código administrado (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) o [descargar PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 se integra con .NET Common Language Runtime para permitir a los desarrolladores crear objetos de base de datos a través de código administrado. Este tutorial muestra cómo crear los procedimientos almacenados administrados y administrados de funciones definidas por el usuario con el código de Visual Basic o C#. También vemos cómo estas ediciones de Visual Studio le permiten depurar estos objetos de base de datos administrados.


## <a name="introduction"></a>Introducción

Usar bases de datos como s de Microsoft SQL Server 2005 el [Transact-Structured lenguaje de consulta (Transact-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) para insertar, modificar y recuperar datos. La mayoría de los sistemas de base de datos incluyen construcciones para agrupar una serie de instrucciones SQL que, a continuación, se puede ejecutar como una sola unidad reutilizable. Los procedimientos almacenados son un ejemplo. Otro ejemplo es *User-Defined Functions*(UDF), una construcción que examinaremos en detalle en el paso 9.

En esencia, SQL está diseñado para trabajar con conjuntos de datos. El `SELECT`, `UPDATE`, y `DELETE` instrucciones intrínsecamente se aplican a todos los registros de la tabla correspondiente y solo están limitadas por sus `WHERE` cláusulas. Todavía hay muchas características de lenguaje diseñadas para trabajar con un registro a la vez y la manipulación de datos escalar. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) permitir para un conjunto de registros de un bucle a través de uno a la vez. Al igual que las funciones de manipulación de cadenas `LEFT`, `CHARINDEX`, y `PATINDEX` funcionan con datos escalares. SQL también incluye instrucciones de flujo de control como `IF` y `WHILE`.

Antes de Microsoft SQL Server 2005, procedimientos almacenados y UDF podrían solo se define como una colección de instrucciones de Transact-SQL. Sin embargo, SQL Server 2005, se diseñó para proporcionar integración con el [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), que es el tiempo de ejecución utilizado por todos los ensamblados. NET. Por lo tanto, los procedimientos almacenados y UDF en una base de datos de SQL Server 2005 se pueden crear mediante código administrado. Es decir, puede crear un procedimiento almacenado o UDF como un método en una clase de Visual Basic. Esto permite que estos procedimientos almacenados y UDF para utilizar la funcionalidad de .NET Framework y de sus propias clases personalizadas.

En este tutorial, examinaremos cómo crear administrada procedimientos almacenados y funciones definidas por el usuario y cómo integrarlos en nuestra base de datos Northwind. Introducción s Let!

> [!NOTE]
> Los objetos de base de datos administrados ofrecen algunas ventajas con respecto a sus homólogos SQL. Riqueza del lenguaje y la familiaridad y la capacidad de volver a usar la lógica y el código existente son las principales ventajas. Pero los objetos de base de datos administrados suelen ser menos eficaz cuando se trabaja con conjuntos de datos que no impliquen mucha lógica de procedimientos. Para obtener una explicación más completa sobre las ventajas del uso de código administrado en comparación con Transact-SQL, consulte el [ventajas de utilizar código administrado para crear objetos de base de datos](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Paso 1: Mover fuera de la base de datos de Northwind`App_Data`

Todos nuestros tutoriales hasta ahora han usado un archivo de base de datos de Microsoft SQL Server 2005 Express Edition en la s de la aplicación web `App_Data` carpeta. Colocación de la base de datos en `App_Data` simplifica la distribución y ejecución de estos tutoriales como todos los archivos se encuentran en un directorio y no requiere ningún paso de configuración adicionales para probar el tutorial.

Sin embargo, para este tutorial, s permiten mover la base de datos Northwind de `App_Data` y registrarlo explícitamente con la instancia de base de datos de SQL Server 2005 Express Edition. Aunque se puede realizar los pasos de este tutorial con la base de datos en el `App_Data` carpeta, un número de los pasos se realiza mucho más sencillo registrando explícitamente la base de datos con la instancia de base de datos de SQL Server 2005 Express Edition.

La descarga de este tutorial contiene los archivos de dos bases de datos - `NORTHWND.MDF` y `NORTHWND_log.LDF` : coloca en una carpeta denominada `DataFiles`. Si está trabajando con su propia implementación de los tutoriales, cierre Visual Studio y mover el `NORTHWND.MDF` y `NORTHWND_log.LDF` archivos desde el sitio Web s `App_Data` a una carpeta fuera del sitio Web. Una vez que se han movido los archivos de base de datos a otra carpeta que se debe registrar la base de datos Northwind con la instancia de base de datos de SQL Server 2005 Express Edition. Esto puede hacerse desde SQL Server Management Studio. Si tiene un no - Express Edition de SQL Server 2005 instalado en el equipo, a continuación, es probable que ya tiene instalado Management Studio. Si tiene SQL Server 2005 Express Edition en el equipo, dedique un momento para descargar e instalar sólo [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Inicie SQL Server Management Studio. Como se muestra en la figura 1, Management Studio comienza preguntando qué servidor al que conectarse. Escriba localhost\SQLExpress para el nombre del servidor, elija autenticación de Windows en la lista desplegable de autenticación y haga clic en conectar.


![Conéctese a la instancia de base de datos adecuada](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figura 1**: conéctese a la instancia de base de datos adecuada


Una vez que se ha conectado, la ventana del explorador de objetos enumerará información acerca de la instancia de base de datos de SQL Server 2005 Express Edition, incluidos sus bases de datos, información de seguridad, las opciones de administración y así sucesivamente.

Se debe adjuntar la base de datos Northwind en el `DataFiles` carpeta (o donde es posible que ha realizado) a la instancia de base de datos de SQL Server 2005 Express Edition. Haga doble clic en la carpeta de bases de datos y elija la opción para adjuntar en el menú contextual. Se abrirá el cuadro de diálogo Adjuntar bases de datos. Haga clic en el botón Agregar, explorar en profundidad adecuado `NORTHWND.MDF` de archivo y haga clic en Aceptar. En este momento, la pantalla debe ser similar a la figura 2.


[![Conéctese a la instancia de base de datos adecuada](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figura 2**: conectarse a la instancia adecuada de la base de datos ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Al conectarse a la instancia de SQL Server 2005 Express Edition a través de Management Studio del cuadro de diálogo Adjuntar bases de datos no permite profundizar en los directorios de perfil de usuario, como Mis documentos. Por lo tanto, asegúrese de colocar el `NORTHWND.MDF` y `NORTHWND_log.LDF` archivos en un directorio del perfil que no es de usuario.


Haga clic en el botón Aceptar para adjuntar la base de datos. Se cerrará el cuadro de diálogo Adjuntar bases de datos y el Explorador de objetos debe enumerar ahora la base de datos recién conectado. Lo más probable es que la base de datos tiene un nombre como de Northwind `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Cambiar el nombre de la base de datos Northwind, con el botón secundario en la base de datos y elija el cambio de nombre.


![Cambiar el nombre de la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figura 3**: cambiar el nombre de la base de datos Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Paso 2: Crear una nueva solución y proyecto de SQL Server en Visual Studio

Para crear los procedimientos almacenados administrados o las UDF en SQL Server 2005 se escribirá el procedimiento almacenado y la lógica UDF como código de Visual Basic en una clase. Una vez que se ha escrito el código, se deberá compilar esta clase en un ensamblado (un `.dll` archivo), registrar el ensamblado con la base de datos de SQL Server y, a continuación, crear un procedimiento almacenado o UDF en la base de datos que señala al método correspondiente en el ensamblado. Estos pasos pueden llevarse a cabo manualmente. Podemos crear el código en cualquier texto editor, compilarlo desde la línea de comandos mediante el compilador de Visual Basic (`vbc.exe`), regístrelo con la base de datos mediante el [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) comando o desde Management Studio y agregue almacenado procedimiento u objeto de UDF a través de medios similar. Afortunadamente, las versiones Professional y sistemas de equipo de Visual Studio incluyen un tipo de proyecto de SQL Server que automatiza estas tareas. En este tutorial le guiará a través con el tipo de proyecto de SQL Server para crear un procedimiento almacenado administrado o una UDF.

> [!NOTE]
> Si usas Visual Web Developer o la edición estándar de Visual Studio, deberá usar el enfoque manual en su lugar. Paso 13 proporciona instrucciones detalladas para llevar a cabo estos pasos manualmente. Le animo a leer los pasos del 2 al 12 antes de leer paso 13, ya que estos pasos incluyen instrucciones de configuración de SQL Server importantes que deben aplicarse independientemente de qué versión de Visual Studio que esté usando.


Comience abriendo Visual Studio. En el menú archivo, elija Nuevo proyecto para mostrar el cuadro de diálogo nuevo proyecto cuadro (consulte la figura 4). Explorar en profundidad el tipo de proyecto de base de datos y, a continuación, en las plantillas indicadas a la derecha, elija crear un nuevo proyecto de SQL Server. Se ha elegido un nombre de este proyecto `ManagedDatabaseConstructs` y lo ha colocado dentro de una solución denominada `Tutorial75`.


[![Crear un nuevo proyecto SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figura 4**: crear un nuevo proyecto de SQL Server ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


Haga clic en el botón Aceptar en el cuadro de diálogo nuevo proyecto para crear la solución y proyecto de SQL Server.

Un proyecto de SQL Server está asociado a una base de datos determinada. Por lo tanto, después de crear el nuevo proyecto de SQL Server se inmediatamente nos pide que especifique esta información. Figura 5 muestra el cuadro de diálogo nueva referencia de base de datos que se ha rellenado para que apunte a la base de datos de Northwind que se registró en la instancia de base de datos de SQL Server 2005 Express Edition en el paso 1.


![Asociar el proyecto de SQL Server con la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figura 5**: asociar el proyecto de SQL Server con la base de datos Northwind


Para depurar los procedimientos almacenados administrados y UDF se creará dentro de este proyecto, es necesario habilitar la compatibilidad para la conexión de depuración SQL/CLR. Cada vez que la asociación de un proyecto de SQL Server con una base de datos (como hicimos en la figura 5), Visual Studio nos le preguntará si desea habilitar la depuración SQL/CLR en la conexión (consulte la figura 6). Haga clic en Sí.


![Habilitar la depuración SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figura 6**: habilitar la depuración SQL/CLR


En este momento, el nuevo proyecto de SQL Server se agregó a la solución. Contiene una carpeta denominada `Test Scripts` con un archivo denominado `Test.sql`, que se usa para depurar los objetos de base de datos administrados creados en el proyecto. Veremos la depuración en el paso 12.

Ahora podemos agregar nuevos procedimientos almacenados administrados y las UDF para este proyecto, pero antes de que nos permiten s primero incluir nuestra aplicación web existente en la solución. En el menú Archivo seleccione la opción de agregar y elija el sitio Web existente. Vaya a la carpeta del sitio Web correspondiente y haga clic en Aceptar. Como se muestra en la figura 7, esto actualizará la solución para incluir dos proyectos: el sitio Web y el `ManagedDatabaseConstructs` proyecto de SQL Server.


![El Explorador de soluciones ahora incluye dos proyectos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figura 7**: el Explorador de soluciones ahora incluye dos proyectos


El `NORTHWNDConnectionString` valor en `Web.config` actualmente hace referencia a la `NORTHWND.MDF` de archivos en el `App_Data` carpeta. Dado que quitamos esta base de datos de `App_Data` y registrar de forma explícita en la instancia de base de datos de SQL Server 2005 Express Edition, es necesario actualizar en consecuencia el `NORTHWNDConnectionString` valor. Abra el `Web.config` archivo en el sitio Web y cambie el `NORTHWNDConnectionString` valor para que quede de la cadena de conexión: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Después de este cambio, su `<connectionStrings>` sección `Web.config` debe ser similar al siguiente:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Como se describe en el [tutorial anterior](debugging-stored-procedures-vb.md), al depurar un objeto de SQL Server desde una aplicación cliente, como un sitio Web ASP.NET, es necesario deshabilitar la agrupación de conexiones. La cadena de conexión anterior deshabilita la agrupación de conexiones ( `Pooling=false` ). Si no tiene previsto en los procedimientos almacenados administrados y las UDF de depuración desde el sitio Web ASP.NET, habilitar la agrupación de conexiones.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Paso 3: Creación un procedimiento almacenado administrado

Para agregar un procedimiento almacenado administrado a la base de datos Northwind, primero debe crear el procedimiento almacenado como un método en el proyecto de SQL Server. En el Explorador de soluciones, haga doble clic en el `ManagedDatabaseConstructs` nombre del proyecto y elija Agregar un nuevo elemento. Se mostrará el cuadro de diálogo Agregar nuevo elemento, que se enumera los tipos de objetos de base de datos administrado que se pueden agregar al proyecto. Como se muestra en la figura 8, esto incluye procedimientos almacenados y funciones definidas por el usuario, entre otros.

Permiten s empiece agregando un procedimiento almacenado que simplemente devuelve todos los productos que se han suspendido. Asigne al nuevo archivo de procedimiento almacenado `GetDiscontinuedProducts.vb`.


[![Agregar un nuevo procedimiento almacenado denominado GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figura 8**: agregar un nuevo procedimiento denominado de almacenados `GetDiscontinuedProducts.vb` ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Esto creará un nuevo archivo de clase de Visual Basic con el siguiente contenido:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Tenga en cuenta que el procedimiento almacenado se implementa como un `Shared` método dentro de un `Partial` archivo de clase denominado `StoredProcedures`. Además, el `GetDiscontinuedProducts` método está decorado con el [ `SqlProcedure` atributo](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), que marca el método como un procedimiento almacenado.

El código siguiente crea un `SqlCommand` objeto y establece su `CommandText` a un `SELECT` consulta que devuelve todas las columnas de la `Products` tabla productos cuyos `Discontinued` campo es igual a 1. A continuación, ejecuta el comando y envía los resultados a la aplicación cliente. Agregue este código a la `GetDiscontinuedProducts` método.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Todos los objetos de base de datos administrados tienen acceso a un [ `SqlContext` objeto](https://msdn.microsoft.com/library/ms131108.aspx) que representa el contexto del llamador. El `SqlContext` proporciona acceso a un [ `SqlPipe` objeto](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) a través de su [ `Pipe` propiedad](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Esto `SqlPipe` objeto se utiliza para transportar información entre la base de datos de SQL Server y la aplicación que realiza la llamada. Como su nombre implica, el [ `ExecuteAndSend` método](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) se ejecuta en un pasado `SqlCommand` objeto y lo envía de nuevo los resultados a la aplicación cliente.

> [!NOTE]
> Objetos de base de datos administrados son más adecuados para los procedimientos almacenados y UDF que utilice lógica de procedimientos en lugar de la lógica basada en conjunto. Lógica de procedimiento implica trabajar con conjuntos de datos según una fila por fila o trabajar con datos escalares. El `GetDiscontinuedProducts` que acabamos de crear, sin embargo, el método no implica ninguna lógica de procedimientos. Por lo tanto, lo ideal es que podría implementarse como un procedimiento almacenado de T-SQL. Se implementa como un procedimiento almacenado administrado para demostrar los pasos necesarios para crear e implementar administrado procedimientos almacenados.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Paso 4: Implementar los recursos administrados de procedimiento almacenado

Con este código completo, estamos preparados para implementarla en la base de datos Northwind. Implementar un proyecto de SQL Server compila el código en un ensamblado, el ensamblado registra con la base de datos y crea los objetos correspondientes en la base de datos, vincularlos a los métodos adecuados en el ensamblado. El conjunto exacto de las tareas realizadas por la opción de implementar con más precisión está escrito en el paso 13. Haga doble clic en el `ManagedDatabaseConstructs` nombre en el Explorador de soluciones del proyecto y elija la opción de implementar. Sin embargo, no la implementación con el siguiente error: sintaxis incorrecta cerca de 'Externo'. Es posible que deba establecer el nivel de compatibilidad de la base de datos actual en un valor superior para habilitar esta característica. Consulte la ayuda para el procedimiento almacenado `sp_dbcmptlevel`.

Este mensaje de error se produce cuando se intenta registrar el ensamblado con la base de datos Northwind. Con el fin de registrar un ensamblado con una base de datos de SQL Server 2005, el nivel de compatibilidad de base de datos s debe establecerse en 90. De forma predeterminada, nuevas bases de datos de SQL Server 2005 tienen un nivel de compatibilidad 90. Sin embargo, las bases de datos creadas con Microsoft SQL Server 2000 tienen un nivel de compatibilidad predeterminado de 80. Puesto que la base de datos Northwind inicialmente era una base de datos de Microsoft SQL Server 2000, su nivel de compatibilidad actualmente está establecido en 80 y, por tanto, debe aumentarse en 90, con el fin de registrar los objetos de base de datos administrados.

Para actualizar el nivel de compatibilidad de base de datos s, abra una ventana nueva consulta en Management Studio y escriba:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Haga clic en el icono ejecutar en la barra de herramientas para ejecutar la consulta anterior.


[![Actualizar el nivel de compatibilidad de base de datos Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figura 9**: actualizar la base de datos Northwind s nivel de compatibilidad ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Después de actualizar el nivel de compatibilidad, vuelva a implementar el proyecto de SQL Server. Esta vez la implementación debe completarse sin errores.

Volver a SQL Server Management Studio, haga doble clic en la base de datos Northwind en el Explorador de objetos y elija actualizar. A continuación, profundizar en la carpeta Programmability y, a continuación, expanda la carpeta de ensamblados. Como se muestra en la figura 10, la base de datos Northwind ahora incluye el ensamblado generado por el `ManagedDatabaseConstructs` proyecto.


![El ensamblado ManagedDatabaseConstructs está registrado con la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figura 10**: el `ManagedDatabaseConstructs` ensamblado está registrado con la base de datos Northwind


También, expanda la carpeta procedimientos almacenados. Allí verá un procedimiento almacenado denominado `GetDiscontinuedProducts`. Este procedimiento almacenado creado por el proceso de implementación y apunta a la `GetDiscontinuedProducts` método en el `ManagedDatabaseConstructs` ensamblado. Cuando el `GetDiscontinuedProducts` se ejecuta el procedimiento almacenado, que, a su vez, ejecuta el `GetDiscontinuedProducts` método. Puesto que se trata de un procedimiento almacenado administrado no se puede editar a través de Management Studio (por lo tanto, el icono de candado junto al nombre del procedimiento almacenado).


![El procedimiento almacenado GetDiscontinuedProducts aparece en la carpeta de procedimientos almacenados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figura 11**: el `GetDiscontinuedProducts` Stored Procedure se muestra en la carpeta de procedimientos almacenados


Sigue habiendo un obstáculo más que tenemos que superar antes de que podemos llamar al procedimiento almacenado administrado: la base de datos está configurada para impedir la ejecución de código administrado. Para comprobarlo, abra una nueva ventana de consulta y ejecutar la `GetDiscontinuedProducts` procedimiento almacenado. Recibirá el siguiente mensaje de error: la ejecución del código de usuario en .NET Framework está deshabilitada. Habilitar la opción de configuración clr habilitado.

Para examinar la información de configuración de la base de datos s de Northwind, escriba y ejecute el comando `exec sp_configure` en la ventana de consulta. Esto muestra que el clr habilitado establecer actualmente está establecido en 0.


[![Clr habilitado configuración está actualmente establecido en 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figura 12**: clr habilitado configuración está actualmente establecido en 0 ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Tenga en cuenta que cada opción de configuración en la figura 12 tiene cuatro valores que se muestran con él: la y los valores máximos y la configuración de valores mínimos y ejecución. Para actualizar el valor de configuración para la configuración de clr habilitado, ejecute el siguiente comando:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Si vuelve a ejecutar el `exec sp_configure` verá que la instrucción anterior actualiza el valor de configuración de la configuración de clr habilitado s a 1, pero que el valor de ejecución sigue establecido en 0. Para que este cambio de configuración surta efecto, es necesario ejecutar el [ `RECONFIGURE` comando](https://msdn.microsoft.com/library/ms176069.aspx), que establecerá el valor de ejecución en el valor de configuración actual. Simplemente escriba `RECONFIGURE` en la ventana de consulta y haga clic en el icono ejecutar en la barra de herramientas. Si ejecuta `exec sp_configure` ahora debería ver un valor de 1 para la configuración de la configuración del clr habilitado s y ejecutar los valores.

Haya completado la configuración habilitada para clr, estamos preparados ejecutar los recursos administrados `GetDiscontinuedProducts` procedimiento almacenado. En la ventana de consulta, escriba y ejecute el comando `exec` `GetDiscontinuedProducts`. Invoca el procedimiento almacenado, el código administrado correspondiente en el `GetDiscontinuedProducts` ejecución del método. Este código emite un `SELECT` consulta para devolver todos los productos que se han suspendido y devuelve estos datos a la aplicación que realiza la llamada, que es de SQL Server Management Studio en esta instancia. Management Studio recibe estos resultados y muestra en la ventana de resultados.


[![El GetDiscontinuedProducts procedimiento almacenado devuelve todos los productos discontinuados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figura 13**: el `GetDiscontinuedProducts` almacenados procedimiento devuelve todos los productos discontinuados ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Paso 5: Crear administrado procedimientos almacenados que aceptan parámetros de entrada

Muchas de las consultas y procedimientos almacenados que hemos creado a lo largo de estos tutoriales se han usado *parámetros*. Por ejemplo, en el [crear procedimientos almacenados nuevo para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial hemos creado un procedimiento almacenado denominado `GetProductsByCategoryID` que acepta un parámetro de entrada denominado `@CategoryID`. El procedimiento almacenado, a continuación, devuelve todos los productos cuyo `CategoryID` campo coincide con el valor de la `@CategoryID` parámetro.

Para crear un procedimiento almacenado administrado que acepta parámetros de entrada, basta con especificar esos parámetros en la definición del método s. Para ilustrar esto, s permiten agregar otro procedimiento almacenado administrado para el `ManagedDatabaseConstructs` proyecto denominado `GetProductsWithPriceLessThan`. Este procedimiento almacenado administrado va a aceptar un parámetro de entrada especifica un precio y devolverá todos los productos cuyo `UnitPrice` campo es menor que el valor del parámetro.

Para agregar un nuevo procedimiento almacenado para el proyecto, haga doble clic en el `ManagedDatabaseConstructs` nombre del proyecto y elija Agregar un nuevo procedimiento almacenado. Asigne al archivo el nombre `GetProductsWithPriceLessThan.vb`. Como hemos visto en el paso 3, esto creará un nuevo archivo de clase de Visual Basic con un método denominado `GetProductsWithPriceLessThan` colocado dentro el `Partial` clase `StoredProcedures`.

Actualización de la `GetProductsWithPriceLessThan` definición del método s para que acepte un [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) parámetro de entrada denominado `price` y escribir el código para ejecutar y devolver los resultados de consulta:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

El `GetProductsWithPriceLessThan` definición del método s y el código se parece bastante a la definición y código de la `GetDiscontinuedProducts` método creado en el paso 3. Las únicas diferencias son que el `GetProductsWithPriceLessThan` método acepta como parámetro de entrada (`price`), el `SqlCommand` s consulta incluye un parámetro (`@MaxPrice`), y se agrega un parámetro a la `SqlCommand` s `Parameters` es la colección y asigna el valor de la `price` variable.

Después de agregar este código, volver a implementar el proyecto de SQL Server. A continuación, vuelva a SQL Server Management Studio y actualice la carpeta procedimientos almacenados. Debería ver una nueva entrada, `GetProductsWithPriceLessThan`. Desde una ventana de consulta, escriba y ejecute el comando `exec GetProductsWithPriceLessThan 25`, que enumerará todos los productos de menor que 25 USD, como se muestra en la figura 14.


[![Se muestran los productos en 25 USD](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figura 14**: se muestran los productos en 25 USD ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Paso 6: Llamar al procedimiento almacenado administrado desde la capa de acceso a datos

En este punto hemos agregado la `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan` administrados a los procedimientos almacenados del `ManagedDatabaseConstructs` del proyecto y se registró con la base de datos Northwind de SQL Server. También se invocan estos procedimientos almacenados administrados desde SQL Server Management Studio (consulte las figuras 13 y 14). En orden para nuestro ASP.NET aplicación para usar estos administra los procedimientos almacenados, sin embargo, es necesario agregarlos al acceso a datos y capas de lógica de negocios en la arquitectura. En este paso se agregará dos nuevos métodos para la `ProductsTableAdapter` en el `NorthwindWithSprocs` DataSet con tipo, que se creó inicialmente en el [crear procedimientos almacenados nuevo para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. En el paso 7 agregaremos métodos correspondientes a la capa BLL.

Abra el `NorthwindWithSprocs` Typed DataSet en Visual Studio y comience por agregar un nuevo método a la `ProductsTableAdapter` denominado `GetDiscontinuedProducts`. Para agregar un nuevo método a un TableAdapter, haga doble clic en el nombre de s TableAdapter en el diseñador y elija la opción de Agregar consulta en el menú contextual.

> [!NOTE]
> Puesto que se mueve la base de datos Northwind desde el `App_Data` carpeta a la instancia de base de datos de SQL Server 2005 Express Edition, es imprescindible que la cadena de conexión correspondiente en el archivo Web.config se actualiza para reflejar este cambio. En el paso 2 se explicó actualizando el `NORTHWNDConnectionString` valor en `Web.config`. Si ha olvidado realizar esta actualización, verá el mensaje de error no se pudo agregar la consulta. No se puede encontrar la conexión `NORTHWNDConnectionString` objeto `Web.config` en un cuadro de diálogo cuando se intenta agregar un nuevo método al TableAdapter. Para resolver este error, haga clic en Aceptar y, a continuación, vaya a `Web.config` y actualizar la `NORTHWNDConnectionString` valor tal como se describe en el paso 2. A continuación, intente volver a agregar el método a TableAdapter. Esta vez debería funcionar sin errores.


Agregar un nuevo método inicia al Asistente de configuración de la consulta de TableAdapter, que hemos usado muchas veces en los tutoriales anteriores. El primer paso nos pide para especificar cómo debe tener acceso a la base de datos del TableAdapter: mediante una instrucción de SQL ad hoc o a través de un procedimiento almacenado nuevo o existente. Puesto que ya hemos creado y registrado el `GetDiscontinuedProducts` un procedimiento almacenado administrado con la base de datos, elija existente utilice opción procedimiento almacenados y presione siguiente.


[![Elija el uso existente de la opción de procedimiento almacenado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figura 15**: elegir el uso existente procedimiento opción almacenado ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


La siguiente pantalla nos pide el procedimiento almacenado que se invocará el método. Elija la `GetDiscontinuedProducts` procedimiento almacenado administrado en la lista desplegable y presione siguiente.


[![Seleccione el GetDiscontinuedProducts procedimiento almacenado administrado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figura 16**: seleccione el `GetDiscontinuedProducts` administrados Stored Procedure ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


A continuación, se nos pide para especificar si el procedimiento almacenado devuelve filas, un único valor o nada. Puesto que `GetDiscontinuedProducts` devuelve el conjunto de filas de productos discontinuados, elija la primera opción (TDS) y haga clic en siguiente.


[![Seleccione la opción de datos tabulares](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figura 17**: seleccione la opción de datos Tabular ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


La última pantalla del asistente nos permite especificar los patrones de acceso de datos utilizados y los nombres de los métodos resultantes. Deje las casillas de verificación activadas y el nombre los métodos `FillByDiscontinued` y `GetDiscontinuedProducts`. Haga clic en Finalizar para completar al asistente.


[![Nombre de lo métodos de FillByDiscontinued y GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figura 18**: el nombre de los métodos `FillByDiscontinued` y `GetDiscontinuedProducts` ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Repita estos pasos para crear métodos denominados `FillByPriceLessThan` y `GetProductsWithPriceLessThan` en el `ProductsTableAdapter` para el `GetProductsWithPriceLessThan` procedimiento almacenado administrado.

Figura 19 se muestra una captura de pantalla del Diseñador de DataSet después de agregar los métodos para la `ProductsTableAdapter` para el `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan` administra los procedimientos almacenados.


[![El ProductsTableAdapter incluye los nuevos métodos de agregado en este paso](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figura 19**: el `ProductsTableAdapter` incluye el nuevo métodos de agregado en este paso ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Paso 7: Agregar métodos correspondientes a la capa de lógica de negocios

Ahora que hemos actualizado la capa de acceso a datos para incluir métodos para llamar a los procedimientos almacenados administrados que agregó en los pasos 4 y 5, necesitamos agregar métodos correspondientes a la capa de lógica empresarial. Agregue los dos métodos siguientes a la `ProductsBLLWithSprocs` clase:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Ambos métodos, simplemente llame al método correspondiente de la capa DAL y devolver el `ProductsDataTable` instancia. El `DataObjectMethodAttribute` marcado encima de cada método hace que estos métodos que se incluirán en la lista desplegable en la pestaña del Asistente para configurar origen de datos s ObjectDataSource SELECT.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Paso 8: Invocar administrado procedimientos almacenados desde la capa de presentación

Con la lógica de negocios y los niveles de acceso a datos ampliado para incluir compatibilidad para llamar a la `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan` administra los procedimientos almacenados, ahora podemos mostrar estos almacena los resultados de los procedimientos a través de una página ASP.NET.

Abra el `ManagedFunctionsAndSprocs.aspx` página en el `AdvancedDAL` carpeta y, en el cuadro de herramientas, arrastre un control GridView en el diseñador. Establezca la s GridView `ID` propiedad `DiscontinuedProducts` y, en la etiqueta inteligente, de enlazarla a un nuevo origen ObjectDataSource denominado `DiscontinuedProductsDataSource`. Configurar el origen ObjectDataSource para extraer sus datos de la `ProductsBLLWithSprocs` clase s `GetDiscontinuedProducts` método.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figura 20**: configurar el origen ObjectDataSource que se usarán el `ProductsBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Elija el método GetDiscontinuedProducts en la lista desplegable en la ficha Seleccionar](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figura 21**: elija la `GetDiscontinuedProducts` método en la lista desplegable en la ficha Seleccione ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Puesto que esta cuadrícula se usará para mostrar solo la información de producto, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None) y, a continuación, haga clic en Finalizar.

Al finalizar el asistente, Visual Studio agregará automáticamente un BoundField o CampoCasillaVerificación para cada campo de datos en el `ProductsDataTable`. Dedique un momento a todos estos campos, excepto para quitar `ProductName` y `Discontinued`, momento en que el control GridView y marcado declarativo de ObjectDataSource s debe ser similar al siguiente:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Dedique un momento para ver esta página a través de un explorador. Cuando se visita la página, las llamadas de ObjectDataSource el `ProductsBLLWithSprocs` clase s `GetDiscontinuedProducts` método. Como hemos visto en el paso 7, este método llama a hacia abajo a la s DAL `ProductsDataTable` clase s `GetDiscontinuedProducts` método, que invoca la `GetDiscontinuedProducts` procedimiento almacenado. Este procedimiento almacenado es un procedimiento almacenado y se ejecuta el código que hemos creado en el paso 3, devolver los productos discontinuados.

Los resultados devueltos por el procedimiento almacenado administrado están empaquetados en un `ProductsDataTable` la capa DAL y, a continuación, se devuelve a la capa BLL, que, a continuación, los devuelve a la capa de presentación que se enlaza a la GridView y se muestran. Según lo previsto, la cuadrícula enumera los productos que se han suspendido.


[![Se enumeran los productos discontinuados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Figura 22**: se enumeran los productos discontinuados ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Para obtener más práctica, agregue un cuadro de texto y GridView otra a la página. Hacer este GridView muestre los productos de menor que la cantidad especificada en el cuadro de texto mediante una llamada a la `ProductsBLLWithSprocs` clase s `GetProductsWithPriceLessThan` método.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Paso 9: Crear y llamar a las UDF de Transact-SQL

Las funciones definidas por el usuario o UDF, son objetos imitar estrechamente la base de datos de la semántica de funciones en lenguajes de programación. Como una función en Visual Basic, las UDF pueden incluir un número variable de parámetros de entrada y devuelven un valor de un tipo determinado. Una UDF puede devolver datos escalares - una cadena, un entero y así sucesivamente - o datos tabulares. Permiten s Eche un vistazo rápido a ambos tipos de UDF, a partir de una UDF que devuelve un tipo de datos escalar.

La UDF siguiente calcula el valor estimado del inventario para un producto determinado. Para ello, toma tres parámetros de entrada - la `UnitPrice`, `UnitsInStock`, y `Discontinued` valores para un determinado producto - y devuelve un valor de tipo `money`. Calcula el valor estimado del inventario multiplicando el `UnitPrice` por la `UnitsInStock`. Para artículos discontinuos, este valor se reduce a la mitad.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Una vez que se ha agregado esta UDF en la base de datos, puede encontrarse a través de Management Studio, expanda la carpeta Programmability, a continuación, funciones y, a continuación, funciones con valores escalares. Se puede usar en un `SELECT` consulta así:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

He agregado el `udf_ComputeInventoryValue` UDF a la base de datos Northwind Figura 23 se muestra la salida de los anteriores `SELECT` de consulta cuando se ven a través de Management Studio. Tenga en cuenta también que la UDF se encuentra bajo la carpeta de funciones con valores escalares en el Explorador de objetos.


[![Se muestra cada producto s valores de inventario](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figura 23**: s de cada producto que se muestran los valores de inventario ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


Las UDF también pueden devolver datos tabulares. Por ejemplo, podemos crear una UDF que devuelve los productos que pertenecen a una categoría determinada:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

El `udf_GetProductsByCategoryID` UDF acepta un `@CategoryID` parámetro de entrada y devuelve los resultados del elemento especificado `SELECT` consulta. Una vez creado, puede hacer referencia a esta UDF en el `FROM` (o `JOIN`) cláusula de una `SELECT` consulta. En el ejemplo siguiente, se devolvería la `ProductID`, `ProductName`, y `CategoryID` valores para cada una de las bebidas.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

He agregado el `udf_GetProductsByCategoryID` UDF a la base de datos Northwind Figura 24 se muestra la salida de los anteriores `SELECT` de consulta cuando se ven a través de Management Studio. Las UDF que devuelven datos tabulares se pueden encontrar en la carpeta de funciones con valores de tabla s Explorador de objetos.


[![El ProductID, ProductName y CategoryID aparecen para cada bebida](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figura 24**: el `ProductID`, `ProductName`, y `CategoryID` aparecen para cada bebida ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Para obtener más información sobre la creación y uso de UDF, consulte [Introducción a las funciones definidas por el usuario](http://www.sqlteam.com/item.asp?ItemID=1955). Consulte también [ventajas y funciones Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Paso 10: Creación de UDF administradas

El `udf_ComputeInventoryValue` y `udf_GetProductsByCategoryID` UDF creadas en los ejemplos anteriores son objetos de base de datos de Transact-SQL. SQL Server 2005 también es compatible con UDF administradas, que se pueden agregar a la `ManagedDatabaseConstructs` proyecto tal como el administrado procedimientos almacenados de los pasos 3 y 5. Este paso, permiten s implementar el `udf_ComputeInventoryValue` UDF en código administrado.

Para agregar una UDF administradas para el `ManagedDatabaseConstructs` del proyecto, haga doble clic en el nombre del proyecto en el Explorador de soluciones y elija Agregar un nuevo elemento. Seleccione la plantilla definidas por el usuario en el cuadro de diálogo Agregar nuevo elemento y asigne al nuevo archivo UDF `udf_ComputeInventoryValue_Managed.vb`.


[![Agregar nuevo UDF administradas para el proyecto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figura 25**: agregar un nuevo UDF administradas para la `ManagedDatabaseConstructs` proyecto ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


La plantilla de función definida por el usuario crea un `Partial` clase denominada `UserDefinedFunctions` con un método cuyo nombre es el mismo que el nombre de archivo s clase (`udf_ComputeInventoryValue_Managed`, en este caso). Este método está decorada con el [ `SqlFunction` atributo](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), que marca el método como UDF administradas.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

El `udf_ComputeInventoryValue` método actualmente devuelve un [ `SqlString` objeto](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) y no acepta cualquier parámetro de entrada. Es necesario actualizar la definición de método para que acepte tres parámetros: de entrada `UnitPrice`, `UnitsInStock`, y `Discontinued` - y devuelve un `SqlMoney` objeto. La lógica para calcular el valor de inventario es idéntica de T-SQL `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Tenga en cuenta que los parámetros de entrada del método s UDF de sus tipos correspondientes de SQL: `SqlMoney` para el `UnitPrice` campo, [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) para `UnitsInStock`, y [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) para `Discontinued`. Estos tipos de datos reflejan los tipos definidos en el `Products` tabla: la `UnitPrice` columna es de tipo `money`, el `UnitsInStock` columna de tipo `smallint`y el `Discontinued` columna de tipo `bit`.

El código comienza creando una `SqlMoney` instancia denominada `inventoryValue` que se asigna un valor de 0. El `Products` permite a tabla de base de datos `NULL` valores en el `UnitsInPrice` y `UnitsInStock` columnas. Por lo tanto, es necesario para comprobar primero para ver si estos valores contienen `NULL` s, lo que hacemos a través de la `SqlMoney` objeto s [ `IsNull` propiedad](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Si ambos `UnitPrice` y `UnitsInStock` contienen que no sean de`NULL` valores, a continuación, se calcula el `inventoryValue` para el producto de los dos. A continuación, si `Discontinued` es true, el valor a la mitad.

> [!NOTE]
> El `SqlMoney` objeto sólo permite dos `SqlMoney` las instancias se multiplican juntos. No se permite un `SqlMoney` instancia va a multiplicar mediante un número de punto flotante literal. Por lo tanto, a la mitad `inventoryValue` multiplíquelo por un nuevo `SqlMoney` instancia que tiene el valor 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Paso 11: Implementación de la UDF administradas

Ahora que se ha creado la UDF administradas, estamos preparados para implementarla en la base de datos Northwind. Como hemos visto en el paso 4, los objetos administrados en un proyecto de SQL Server se implementan con el botón secundario en el nombre del proyecto en el Explorador de soluciones y eligiendo la opción implementar en el menú contextual.

Una vez que haya implementado el proyecto, vuelva a SQL Server Management Studio y actualice la carpeta de las funciones escalares. Ahora debería ver dos entradas:

- `dbo.udf_ComputeInventoryValue` -la UDF T-SQL creado en el paso 9, y
- `dbo.udf ComputeInventoryValue_Managed` -la UDF administrada creado en el paso 10 que acaba de implementar.

Para probar esta UDF administradas, ejecute la siguiente consulta en Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Este comando usa los recursos administrados `udf ComputeInventoryValue_Managed` UDF en lugar de T-SQL `udf_ComputeInventoryValue` UDF, pero el resultado es el mismo. Consulte la figura 23 para ver una captura de pantalla de la salida de s UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Paso 12: Depuración de los objetos de base de datos administrados

En el [depurar procedimientos almacenados](debugging-stored-procedures-vb.md) tutorial analizamos las tres opciones para la depuración de SQL Server a través de Visual Studio: depuración directa de la base de datos, la depuración de aplicaciones y depuración de un proyecto de SQL Server. Administra la base de datos de objetos no se puede depurar a través de depuración directa de la base de datos, pero se pueden depurar desde una aplicación cliente y directamente desde el proyecto de SQL Server. Sin embargo, en orden para que funcione la depuración, la base de datos de SQL Server 2005 debe permitir depuración SQL/CLR. Recuerde que cuando se crea por primera vez el `ManagedDatabaseConstructs` proyecto de Visual Studio nos preguntó si deseamos habilitar la depuración (consulte la figura 6 en el paso 2) de SQL/CLR. Esta configuración se puede modificar con el botón secundario en la base de datos desde la ventana del explorador de servidores.


![Asegúrese de que la base de datos permite la depuración SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figura 26**: asegúrese de que la base de datos permite la depuración SQL/CLR


Imaginemos que deseamos depurar el `GetProductsWithPriceLessThan` procedimiento almacenado administrado. Comenzamos estableciendo un punto de interrupción en el código de la `GetProductsWithPriceLessThan` método.


[![Establezca un punto de interrupción en el método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figura 27**: establecer un punto de interrupción en el `GetProductsWithPriceLessThan` método ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Permiten s primer vistazo al depurar los objetos de base de datos administrado desde el proyecto de SQL Server. Dado que nuestra solución incluye dos proyectos: el `ManagedDatabaseConstructs` proyecto de SQL Server junto con nuestro sitio Web - con el fin de depurar desde el proyecto de SQL Server es necesario indicar a Visual Studio para iniciar el `ManagedDatabaseConstructs` proyecto de SQL Server cuando se inicie la depuración. Haga clic en el `ManagedDatabaseConstructs` proyecto en el Explorador de soluciones y elija el conjunto como opción de proyecto de inicio en el menú contextual.

Cuando el `ManagedDatabaseConstructs` proyecto se inicia desde el depurador ejecuta las instrucciones SQL en el `Test.sql` archivo, que se encuentra en la `Test Scripts` carpeta. Por ejemplo, para probar la `GetProductsWithPriceLessThan` administra el procedimiento almacenado, reemplazar el existente `Test.sql` archivos de contenido con la instrucción siguiente, que invoca la `GetProductsWithPriceLessThan` pasando de procedimiento almacenado administrado la `@CategoryID` valor 14,95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Una vez que ha escrito el script anterior en `Test.sql`, iniciar la depuración, vaya al menú Depurar y elija Iniciar depuración o presionando F5 o el verde del icono de reproducción en la barra de herramientas. Esto se compile los proyectos dentro de la solución, implementar los objetos de base de datos administrada en la base de datos Northwind y, a continuación, ejecutará el `Test.sql` secuencia de comandos. En este momento, el punto de interrupción se alcanzarán y podemos recorrer el `GetProductsWithPriceLessThan` método, examine los valores de los parámetros de entrada y así sucesivamente.


[![Se alcanzó el punto de interrupción en el método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figura 28**: el punto de interrupción en el `GetProductsWithPriceLessThan` se ha llegado a método ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Para un objeto de base de datos SQL que se desea depurar a través de una aplicación cliente, es imperativo que la base de datos se ha configurado para admitir la depuración de la aplicación. Haga doble clic en la base de datos en el Explorador de servidores y asegúrese de que esté marcada la opción de depuración de la aplicación. Además, se necesita configurar la aplicación de ASP.NET para integrar con el depurador de SQL y para deshabilitar la agrupación de conexiones. Estos pasos se describen en detalle en el paso 2 de la [depurar procedimientos almacenados](debugging-stored-procedures-vb.md) tutorial.

Una vez haya configurado la aplicación ASP.NET y la base de datos, establezca el sitio Web de ASP.NET como proyecto de inicio e inicie la depuración. Si visita una página que llama a uno de los objetos administrados que tiene un punto de interrupción, la aplicación se detendrá y el control se derivará al depurador, donde puede recorrer el código tal como se muestra en la figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Paso 13: Compilar e implementar administran de manera manual los objetos de base de datos

Proyectos de SQL Server fácil crear, compilar e implementar objetos de base de datos administrados. Lamentablemente, los proyectos de SQL Server solo están disponibles en las ediciones Professional y sistemas de equipo de Visual Studio. Si está usando Visual Web Developer o el de Visual Studio Standard Edition y quiere usar objetos de base de datos administrado, deberá crear manualmente e implementarlas. Esto implica cuatro pasos:

1. Cree un archivo que contiene el código fuente para el objeto de base de datos administrados
2. Compilar el objeto en un ensamblado,
3. Registrar el ensamblado con la base de datos de SQL Server 2005, y
4. Cree un objeto de base de datos en SQL Server que señala al método apropiado en el ensamblado.

Para ilustrar estas tareas, permiten s crear un nuevo administrado de procedimiento almacenado que devuelve los productos cuyo `UnitPrice` es mayor que un valor especificado. Cree un nuevo archivo en el equipo denominado `GetProductsWithPriceGreaterThan.vb` y escriba el código siguiente en el archivo (puede usar Visual Studio, el Bloc de notas o cualquier editor de texto para realizar esta acción):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Este código es casi idéntico de la `GetProductsWithPriceLessThan` método creado en el paso 5. Las únicas diferencias son los nombres de método, el `WHERE` cláusula y el nombre del parámetro usado en la consulta. En el `GetProductsWithPriceLessThan` método, el `WHERE` cláusula leer: `WHERE UnitPrice < @MaxPrice`. A continuación, en `GetProductsWithPriceGreaterThan`, usamos: `WHERE UnitPrice > @MinPrice` .

Ahora es necesario compilar esta clase en un ensamblado. Desde la línea de comandos, desplácese al directorio donde guardó el `GetProductsWithPriceGreaterThan.vb` de archivo y usar el compilador de C# (`csc.exe`) para compilar el archivo de clase en un ensamblado:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Si la carpeta que contiene v `bc.exe` en no en el sistema s `PATH`, tendrá que hacer referencia a su ruta de acceso completamente `%WINDOWS%\Microsoft.NET\Framework\version\`, así:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Compilar GetProductsWithPriceGreaterThan.vb en un ensamblado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figura 29**: Compile `GetProductsWithPriceGreaterThan.vb` en un ensamblado ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


El `/t` marcador especifica que se debe compilar el archivo de clase de Visual Basic en un archivo DLL (en lugar de un archivo ejecutable). El `/out` marca especifica el nombre del ensamblado resultante.

> [!NOTE]
> En lugar de compilar el `GetProductsWithPriceGreaterThan.vb` archivo de clase desde la línea de comandos como alternativa, podría usar [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) o crear un proyecto de biblioteca de clases independiente en Visual Studio Standard Edition. S ren Jacob Lauritsen póngase ha proporcionado a dicho proyecto de Visual Basic Express Edition con el código para el `GetProductsWithPriceGreaterThan` procedimiento almacenado y las dos administran los procedimientos almacenados y UDF creados en los pasos 3, 5 y 10. Proyecto de S ren s también incluye los comandos de Transact-SQL necesarios para agregar los objetos de base de datos correspondientes.


Con el código compilado en un ensamblado, estamos preparados para registrar el ensamblado en la base de datos de SQL Server 2005. Esto puede realizarse a través de Transact-SQL, con el comando `CREATE ASSEMBLY`, o a través de SQL Server Management Studio. Permitir que el foco s mediante Management Studio.

En Management Studio, expanda la carpeta de la programación en la base de datos Northwind. Uno de sus subcarpetas es los ensamblados. Para agregar manualmente un nuevo ensamblado a la base de datos, haga doble clic en la carpeta de ensamblados y elija a nuevo ensamblado en el menú contextual. Esta muestra el cuadro de diálogo nuevo ensamblado cuadro (consulte la figura 30). Haga clic en el botón Examinar, seleccione el `ManuallyCreatedDBObjects.dll` nos acaba de compilar y, a continuación, haga clic en Aceptar para agregar el ensamblado a la base de datos de ensamblado. No debería ver la `ManuallyCreatedDBObjects.dll` ensamblado en el Explorador de objetos.


[![Agregar el ensamblado ManuallyCreatedDBObjects.dll a la base de datos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figura 30**: agregar el `ManuallyCreatedDBObjects.dll` ensamblado a la base de datos ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![El ManuallyCreatedDBObjects.dll aparece en el Explorador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figura 31**: el `ManuallyCreatedDBObjects.dll` aparece en el Explorador de objetos


Aunque hemos agregado el ensamblado a la base de datos Northwind, aún es necesario asociar un procedimiento almacenado con el `GetProductsWithPriceGreaterThan` método del ensamblado. Para ello, abra una nueva ventana de consulta y ejecute el siguiente script:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Esto crea un nuevo procedimiento almacenado en la base de datos Northwind denominado `GetProductsWithPriceGreaterThan` y lo asocia con el método administrado `GetProductsWithPriceGreaterThan` (que se encuentra en la clase `StoredProcedures`, que está en el ensamblado `ManuallyCreatedDBObjects`).

Después de ejecutar el script anterior, actualice la carpeta procedimientos almacenados en el Explorador de objetos. Debería ver una nueva entrada de procedimiento almacenado - `GetProductsWithPriceGreaterThan` -que tiene un icono de candado junto a él. Para probar este procedimiento almacenado, escriba y ejecute el siguiente script en la ventana de consulta:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Como se muestra en la figura 32, el comando anterior muestra la información de los productos que tengan un `UnitPrice` mayor US$ 24,95.


[![El ManuallyCreatedDBObjects.dll aparece en el Explorador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Figura 32**: el `ManuallyCreatedDBObjects.dll` aparece en el Explorador de objetos ([haga clic aquí para ver imagen en tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Resumen

Microsoft SQL Server 2005 proporciona integración con el Common Language Runtime (CLR), que permite que los objetos de base de datos se deben crear con código administrado. Anteriormente, sólo se pueden crear estos objetos de base de datos mediante T-SQL, pero ahora podemos crear estos objetos mediante programación en lenguajes como Visual Basic .NET. En este tutorial que hemos creado dos administran los procedimientos almacenados y una función definida por el usuario administrado.

S el tipo de proyecto de SQL Server de Visual Studio facilita crear, compilar e implementar objetos de base de datos administrados. Además, ofrece compatibilidad con la depuración enriquecida. Sin embargo, los tipos de proyecto de SQL Server solo están disponibles en las ediciones Professional y sistemas de equipo de Visual Studio. Para aquellos usando Visual Web Developer o la de Visual Studio Standard Edition, la creación, compilación y los pasos de implementación debe realizarse manualmente, como vimos en el paso 13.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Ventajas y desventajas de las funciones definidas por el usuario](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Crear objetos de SQL Server 2005 en código administrado](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Creación de desencadenadores mediante código administrado en SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Cómo: Crear y ejecutar una instancia de CLR de SQL Server de procedimiento almacenado](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Cómo: Crear y ejecutar una función definida por el usuario de CLR de SQL Server](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Cómo: Editar el `Test.sql` secuencia de comandos para ejecutar los objetos SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Las funciones definidas por la introducción a usuario](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Código administrado y SQL Server 2005 (vídeo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referencia de Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Tutorial: Crear un procedimiento almacenado en código administrado](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era S ren Jacob Lauritsen. Además de revisar este artículo, S ren también crea el proyecto de Visual C# Express Edition incluido en esta descarga artículo s para compilar manualmente los objetos de base de datos administrados. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](debugging-stored-procedures-vb.md)
