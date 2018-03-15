---
uid: web-pages/overview/data/5-working-with-data
title: "Introducción a trabajar con una base de datos en ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft"
author: tfitzmac
description: "En este capítulo se describe cómo tener acceso a datos desde una base de datos y mostrarlo mediante ASP.NET Web Pages."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 460af471a1b0650f8d782d582ce6cd9a06664d5c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introducción a trabajar con una base de datos en ASP.NET Web Pages (Razor) sitios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo utilizar las herramientas de Microsoft WebMatrix para crear una base de datos en un sitio Web de ASP.NET Web Pages (Razor) y cómo crear páginas que pueden mostrar, agregar, editar y eliminar datos.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear una base de datos.
> - Cómo conectarse a una base de datos.
> - Cómo mostrar los datos en una página web.
> - Cómo insertar, actualizar y eliminar registros de base de datos.
> 
> Estas son las características introducidas en el artículo:
> 
> - Trabajar con una base de datos de Microsoft SQL Server Compact Edition.
> - Trabajar con consultas SQL.
> - La clase `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con 3 de WebMatrix. Puede utilizar ASP.NET Web Pages 3 y Visual Studio 2013 (o Visual Studio Express 2013 para Web); Sin embargo, la interfaz de usuario será diferente.


## <a name="introduction-to-databases"></a>Introducción a las bases de datos

Imagine una libreta de direcciones típico. Para cada entrada de la libreta de direcciones (es decir, para cada persona) tienen varios elementos de información como el nombre, apellido, dirección, dirección de correo electrónico y número de teléfono.

Una forma habitual a los datos de imagen así es como una tabla con filas y columnas. En términos de base de datos, cada fila se conoce a menudo como un registro. Cada columna (a veces denominada campos) contiene un valor para cada tipo de datos: nombre, nombre de último y así sucesivamente.

| **ID** | **FirstName** | **LastName** | **Dirección** | **Correo electrónico** | **Teléfono** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Para la mayoría de las tablas de base de datos, la tabla debe tener una columna que contiene un identificador único, como un número de cliente, número de cuenta, etcetera. Esto se conoce como la tabla *clave principal*, y usarlo para identificar cada fila de la tabla. En el ejemplo, la columna Id. es la clave principal de la libreta de direcciones.

Esta descripción básica de las bases de datos, está listo para aprender a crear una base de datos simple y realizar operaciones como agregar, modificar y eliminar datos.

> [!TIP] 
> 
> **Bases de datos relacionales**
> 
> Puede almacenar datos de muchas maneras, incluidas las hojas de cálculo y archivos de texto. Sin embargo, para la mayoría de los usos de negocio, datos se almacenan en una base de datos relacional.
> 
> En este artículo no vaya muy profundamente en bases de datos. Sin embargo, le resultará útil para comprender un poco acerca de ellos. En una base de datos relacional, la información se divide lógicamente en tablas independientes. Por ejemplo, una base de datos para una escuela podría contener tablas independientes para estudiantes y para las ofertas de la clase. Los comandos eficaces de es compatible con software (por ejemplo, SQL Server) de base de datos que permiten dinámicamente establecen relaciones entre las tablas. Por ejemplo, puede usar la base de datos relacional para establecer una relación lógica entre alumnos y clases para crear una programación. Almacenar datos en tablas independientes reduce la complejidad de la estructura de tabla y reduce la necesidad de mantener datos redundantes de tablas.


## <a name="creating-a-database"></a>Crear una base de datos

Este procedimiento muestra cómo crear una base de datos denominada SmallBakery mediante la herramienta de diseño de base de datos de SQL Server Compact que se incluye en WebMatrix. Aunque puede crear una base de datos mediante código, es más habitual para crear la base de datos y tablas de base de datos mediante una herramienta de diseño como WebMatrix.

1. Iniciar WebMatrix y en la página Inicio rápido, haga clic en **plantilla de sitio**.
2. Seleccione **sitio vacío**y en el **nombre del sitio** cuadro, escriba "SmallBakery" y, a continuación, haga clic en **Aceptar**. El sitio se crea y se muestra en WebMatrix.
3. En el panel izquierdo, haga clic en el **bases de datos** área de trabajo.
4. En la cinta de opciones, haga clic en **nueva base de datos**. Se crea una base de datos vacío con el mismo nombre que el sitio.
5. En el panel izquierdo, expanda el **SmallBakery.sdf** nodo y, a continuación, haga clic en **tablas**.
6. En la cinta de opciones, haga clic en **nueva tabla**. WebMatrix abre el Diseñador de tablas.

    ![[image]](5-working-with-data/_static/image1.jpg)
7. Haga clic en el **nombre** columna y escriba &quot;identificador&quot;.
8. En el **tipo de datos** columna, seleccione **int**.
9. Establecer el **es la clave principal?** y **es identificar?** opciones para **Sí**.

    Como sugiere su nombre, **es la clave principal** indica que la base de datos que será clave principal de la tabla. **Identidad** indica a la base de datos que se va a crear automáticamente un número de identificación para cada nuevo registro como asignar el siguiente número secuencial (comenzando en 1).
10. Haga clic en la siguiente fila. Iniciará una nueva definición de columna en el editor.
11. Para el valor de nombre, escriba &quot;nombre&quot;.
12. Para **tipo de datos**, elija &quot;nvarchar&quot; y establecer la longitud en 50. El *var* forma parte de `nvarchar` indica que la base de datos que los datos de esta columna será una cadena cuyo tamaño puede variar entre los registros. (El *n* prefijo representa *national*, que indica que el campo puede contener datos de caracteres que representa cualquier alfabeto o sistema de escritura &#8212; es decir, que el campo contiene datos Unicode.)
13. Establecer el **permitir valores NULL** opción **No**. Esto exigirá la *nombre* columna no se deja en blanco.
14. Con el mismo proceso, cree una columna denominada *descripción*. Establecer **tipo de datos** "nvarchar" y 50 para la longitud y el conjunto **permitir valores NULL** en false.
15. Cree una columna denominada *precio*. Establecer **tipo de datos "money"** y establecer **permitir valores NULL** en false.
16. En el cuadro en la parte superior, llame a la tabla &quot;producto&quot;.

    Cuando haya terminado, la definición tendrá un aspecto similar al siguiente:

    ![[image]](5-working-with-data/_static/image2.jpg)
17. Presione CTRL+s para guardar la tabla.

## <a name="adding-data-to-the-database"></a>Agregar datos a la base de datos

Ahora puede agregar algunos datos de ejemplo a la base de datos que trabajará con más adelante en el artículo.

1. En el panel izquierdo, expanda el **SmallBakery.sdf** nodo y, a continuación, haga clic en **tablas**.
2. Haga clic en la tabla Product y, a continuación, haga clic en **datos**.
3. En el panel de edición, escriba los siguientes registros:

    | **Name** | **Descripción** | **Price** |
    | --- | --- | --- |
    | Pan | Preparado reciente cada día. | 2.99 |
    | Shortcake fresa | Se realiza con fresas orgánicas desde nuestro multiproceso. | 9.99 |
    | Gráfico circular de Apple | Segundo únicamente a circular de su madre. | 12.99 |
    | Pacanas circular | Si le gusta pacanas, esto es para usted. | 10.99 |
    | Tarta de limón | Realizar con el mejor limones en todo el mundo. | 11.99 |
    | Bizcochos | Sus hijos y la kid en el se le encanta estos. | 7.99 |

    Recuerde que no tiene que escribir nada para el *identificador* columna. Al crear el *identificador* columna, establezca su **identidad** propiedad en true, lo que hace que se rellena automáticamente.

    Cuando haya terminado de escribir los datos, el Diseñador de tablas tendrá un aspecto similar al siguiente:

    ![[image]](5-working-with-data/_static/image3.jpg)
4. Cerrar la pestaña que contiene los datos de la base de datos.

## <a name="displaying-data-from-a-database"></a>Mostrar datos de una base de datos

Una vez que tenga una base de datos con datos en ella, puede mostrar los datos en una página web ASP.NET. Para seleccionar las filas de tabla para mostrar, se utiliza una instrucción SQL, que es un comando que se pasa a la base de datos.

1. En el panel izquierdo, haga clic en el **archivos** área de trabajo.
2. En la raíz del sitio Web, cree una nueva página CSHTML denominada *ListProducts.cshtml*.
3. Reemplace el marcado existente con lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    En el primer bloque de código, se abrirá la *SmallBakery.sdf* archivo (base de datos) que creó anteriormente. El `Database.Open` método supone que la *.sdf* archivo se encuentra en su sitio Web *aplicación\_datos* carpeta. (Tenga en cuenta que no es necesario especificar el *.sdf* extensión &#8212; de hecho, si lo hace, el `Open` método no funcionará.)

    > [!NOTE]
    > El *aplicación\_datos* carpeta es una carpeta especial en ASP.NET que se usa para almacenar archivos de datos. Para obtener más información, consulte [conectarse a una base de datos](#SB_ConnectingToADatabase) más adelante en este artículo.

    A continuación, se realiza una solicitud para consultar la base de datos mediante el siguiente código SQL `Select` instrucción:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    En la instrucción, `Product` identifica la tabla a la consulta. El `*` carácter especifica que la consulta debe devolver todas las columnas de la tabla. (También puede hacer una lista las columnas individualmente, separados por comas, si deseara ver sólo algunas de las columnas.) El `Order By` cláusula indica cómo se deben ordenar los datos &#8212; en este caso, por la *nombre* columna. Esto significa que los datos se ordenan alfabéticamente según el valor de la *nombre* columna para cada fila.

    En el cuerpo de la página, el marcado crea una tabla HTML que se usará para mostrar los datos. Dentro de la `<tbody>` elemento, use un `foreach` bucle obtener individualmente cada fila de datos devuelto por la consulta. Para cada fila de datos, crear una fila de tabla HTML (`<tr>` elemento). A continuación, cree las celdas de tabla HTML (`<td>` elementos) para cada columna. Cada vez que vaya a través del bucle, la siguiente fila disponible de la base de datos está en el `row` variable (Esto se configura el `foreach` instrucción). Para obtener una sola columna de la fila, puede usar `row.Name` o `row.Description` o el nombre de cualquier es de la columna desee.
4. Ejecute la página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) La página muestra una lista similar al siguiente:

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Lenguaje de consulta estructurado (SQL)**
> 
> SQL es un lenguaje que se utiliza en la mayoría de bases de datos relacionales para administrar los datos en una base de datos. Incluye comandos que permiten recuperar datos y actualizarlo y que le permiten crear, modificar y administrar las tablas de base de datos. SQL es diferente de un lenguaje de programación (por ejemplo, uno que está usando en WebMatrix) porque con SQL, la idea es que puede indicar a la base de datos de la acción que realizará y es trabajo de la base de datos para averiguar cómo obtener los datos o realizar la tarea. Estos son algunos ejemplos de algunos comandos SQL y qué hacer:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Esto captura el *identificador*, *nombre*, y *precio* columnas de registros en la *producto* tabla si el valor de *precio* es superior a 10 y devuelve los resultados en orden alfabético según los valores de la *nombre* columna. Este comando devolverá un conjunto de resultados que contiene los registros que cumplen los criterios, o un conjunto vacío si no hay registros que coincidan con.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Inserta un nuevo registro en el *producto* tabla, al establecer el *nombre* columna &quot;"croissant"&quot;, el *descripción* columna &quot; Una maravilla inestable&quot;y el precio para 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Este comando elimina los registros de la *producto* tabla cuya columna de fecha de expiración es anterior al 1 de enero de 2008. (Se supone que la *producto* tabla tiene una columna de este tipo, por supuesto.) Se especifica la fecha en formato MM/DD/AAAA, pero debe especificarse en el formato que se usa para la configuración regional.
> 
> El `Insert Into` y `Delete` comandos no devuelven conjuntos de resultados. En su lugar, devuelven un número que indica el número de registros se vieron afectado por el comando.
> 
> Para algunas de estas operaciones (por ejemplo, insertar y eliminar registros), el proceso que está solicitando la operación debe tener los permisos adecuados en la base de datos. Se trata de por qué las bases de datos de producción a menudo tendrá que proporcionar un nombre de usuario y una contraseña cuando se conecta a la base de datos.
> 
> Existen docenas de comandos SQL, pero siguen un patrón similar al siguiente. Puede utilizar comandos SQL para crear tablas de base de datos, contar el número de registros de una tabla, calcular precios y realizar muchas más operaciones.


## <a name="inserting-data-in-a-database"></a>Insertar datos en una base de datos

Esta sección muestra cómo crear una página que permite a los usuarios agregar un nuevo producto hasta el *producto* tabla de base de datos. Después de inserta un nuevo registro de producto, la página muestra la tabla actualizada mediante la *ListProducts.cshtml* página que creó en la sección anterior.

La página incluye la validación para asegurarse de que los datos introducidos por el usuario son válidos para la base de datos. Por ejemplo, código en la página se asegura que se ha especificado un valor para todas las columnas necesarias.

1. En el sitio Web, cree un nuevo archivo CSHTML denominado *InsertProducts.cshtml*.
2. Reemplace el marcado existente con lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    El cuerpo de la página contiene un formulario HTML con tres cuadros de texto que permiten a los usuarios escribir un nombre, la descripción y el precio. Cuando el usuario hace clic en el **insertar** botón, el código en la parte superior de la página abre una conexión a la *SmallBakery.sdf* base de datos. A continuación, obtener los valores que el usuario ha enviado mediante el uso de la `Request` del objeto y asignar dichos valores a variables locales.

    Para validar que el usuario especificó un valor para cada columna necesaria, registre cada `<input>` elemento que se desea validar:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    El `Validation` auxiliares comprueba que hay un valor en cada uno de los campos que ha registrado. Puede probar si todos los campos pasan la validación mediante la comprobación de `Validation.IsValid()`, que lo haría normalmente antes de procesar la información obtenida del usuario:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (El `&&` medio del operador AND: esta prueba es *si se trata de un envío de formulario y todos los campos han pasado la validación*.)

    Si se validan todas las columnas (ninguno están vacío), voy a crear una instrucción SQL para insertar los datos y, a continuación, ejecutarla tal y como se muestra a continuación:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Para que los valores Insertar, incluye marcadores de posición de parámetro (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Como precaución de seguridad, siempre pasar valores a una instrucción SQL con parámetros, como se ve en el ejemplo anterior. Esto le da una oportunidad para validar los datos del usuario, además de ayuda a protegerse frente a intentos para enviar comandos malintencionados a la base de datos (denominado a veces como ataques de inyección de SQL).

    Para ejecutar la consulta, use esta instrucción, pasa a la las variables que contienen los valores para sustituir los marcadores de posición:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Después de la `Insert Into` instrucción ejecutada, enviar al usuario a la página que contiene los productos con esta línea:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Si no se realizó correctamente la validación, omita la inserción. En su lugar, tiene una aplicación auxiliar en la página que se puede mostrar los mensajes de error acumulado (si existe):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Observe que el bloque de estilos en el marcado incluye una definición de clase CSS denominada `.validation-summary-errors`. Este es el nombre de la clase CSS que se usa de forma predeterminada para el `<div>` elemento que contiene los errores de validación. En este caso, la clase CSS que especifica que los errores de resumen de validación se muestran en rojo y en negrita, pero es posible definir la `.validation-summary-errors` clase para mostrar cualquier formato que desee.

### <a name="testing-the-insert-page"></a>Probar la página de inserción

1. Ver la página en un explorador. La página muestra una forma que sea similar a la que se muestra en la siguiente ilustración.

    ![[image]](5-working-with-data/_static/image5.jpg)
2. Especifique valores para todas las columnas, pero debe asegurarse de que deje el *precio* columna en blanco.
3. Haga clic en **insertar**. La página muestra un mensaje de error, tal como se muestra en la siguiente ilustración. (No se crea ningún registro nuevo).

    ![[image]](5-working-with-data/_static/image6.jpg)
4. Rellene el formulario por completo y, a continuación, haga clic en **insertar**. En esta ocasión, el *ListProducts.cshtml* página aparece y muestra el nuevo registro.

## <a name="updating-data-in-a-database"></a>Actualizar datos en una base de datos

Después de que se han especificado datos en una tabla, debe actualizarla. Este procedimiento muestra cómo crear dos páginas que son similares a las que ha creado para la inserción de datos anterior. La primera página muestra los productos y permite a los usuarios seleccionar uno y cambiar. La segunda página permite a los usuarios realmente realice las modificaciones y guardarlas.

> [!NOTE] 
> 
> **Importante** en un sitio Web de producción, normalmente restringir quién se puede realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y formas de autorizar a los usuarios para realizar tareas en el sitio, consulte [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. En el sitio Web, cree un nuevo archivo CSHTML denominado *EditProducts.cshtml*.
2. Reemplace el marcado existente en el archivo con lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    La única diferencia entre esta página y la *ListProducts.cshtml* página desde versiones anteriores es que la tabla HTML de esta página incluye una columna adicional que muestra un **editar** vínculo. Al hacer clic en este vínculo, le lleva a la *UpdateProducts.cshtml* página (que se creará a continuación) donde puede editar el registro seleccionado.

    Examine el código que crea el **editar** vínculo:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Esto crea un elemento HTML `<a>` elemento cuyo `href` atributo está establecido de forma dinámica. El `href` atributo especifica la página que se mostrará cuando el usuario hace clic en el vínculo. También pasa el `Id` valor de la fila actual para el vínculo. Cuando se ejecuta la página, el origen de la página podría contener vínculos como estas:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Tenga en cuenta que la `href` atributo está establecido en `UpdateProducts/n`, donde *n* es un número de producto. Cuando un usuario hace clic en uno de estos vínculos, la dirección URL resultante tendrá un aspecto similar al siguiente:

    `http://localhost:18816/UpdateProducts/6`

    En otras palabras, el número de producto que se pueda editar se pasarán en la dirección URL.
3. Ver la página en un explorador. La página muestra los datos en un formato similar al siguiente:

    ![[image]](5-working-with-data/_static/image7.jpg)

    A continuación, podrá crear la página que permite a los usuarios actualizar los datos. La página de actualización incluye la validación para validar los datos introducidos por el usuario. Por ejemplo, código en la página se asegura que se ha especificado un valor para todas las columnas necesarias.
4. En el sitio Web, cree un nuevo archivo CSHTML denominado *UpdateProducts.cshtml*.
5. Reemplace el marcado existente en el archivo por el siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    El cuerpo de la página contiene un formulario HTML que se muestra un producto y donde los usuarios pueden editar. Para obtener el producto que desea mostrar, utilice esta instrucción SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Se seleccionarán el producto cuyo identificador coincide con el valor que se pasa en el `@0` parámetro. (Dado que *identificador* es la clave principal y, por tanto, debe ser único, el registro de un único producto nunca se puede seleccionar este modo.) Para obtener el valor de identificador para pasar a este `Select` instrucción, puede leer el valor que se pasa a la página como parte de la dirección URL, utilizando la sintaxis siguiente:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Para capturar realmente el registro de producto, use la `QuerySingle` método, que devolverá únicamente un solo registro:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Se devuelve la fila única en la `row` variable. Puede obtener datos de cada columna y asígnela a las variables locales como el siguiente:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    En el marcado para el formulario, estos valores se muestran automáticamente en los cuadros de texto individuales mediante el uso de código incrustado similar al siguiente:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Esa parte del código muestra el registro de producto se actualice. Una vez que se ha mostrado el registro, el usuario puede editar las columnas individuales.

    Cuando el usuario envía el formulario, haga clic en el **actualización** botón, el código en el `if(IsPost)` bloquear ejecuciones. De esta forma obtiene los valores del usuario desde el `Request` objetos, almacena los valores de variables y valida que cada columna se ha rellenado. Si la validación es correcta, el código crea la siguiente instrucción Update de SQL:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    En una instancia de SQL `Update` instrucción, especifica cada columna en la actualización y el valor que establezca. En este código, los valores se especifican utilizando los marcadores de posición de parámetro `@0`, `@1`, `@2`, y así sucesivamente. (Como se indicó anteriormente, para la seguridad, también debe siempre pasar valores a una instrucción SQL con parámetros.)

    Cuando se llama a la `db.Execute` (método), pasa las variables que contienen los valores en el orden en que se corresponde con los parámetros en la instrucción SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Después de la `Update` se ha ejecutado la instrucción, llame al método siguiente con el fin de redirigir al usuario a la página de edición:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    El efecto es que el usuario verá una lista actualizada de los datos de la base de datos y puede editar otro producto.
6. Guarde la página.
7. Ejecute el *EditProducts.cshtml* página (no en la página de actualización) y, a continuación, haga clic en **editar** para seleccionar un producto para editar. El *UpdateProducts.cshtml* página se muestra, que muestra el registro seleccionado.

    ![[image]](5-working-with-data/_static/image8.jpg)
8. Realiza un cambio y haga clic en **actualización**. La lista de productos se muestre de nuevo con los datos actualizados.

## <a name="deleting-data-in-a-database"></a>Eliminar datos en una base de datos

Esta sección muestra cómo permitir a los usuarios eliminar un producto de la *producto* tabla de base de datos. El ejemplo consta de dos páginas. En la primera página, los usuarios seleccionar un registro para eliminar. Que se puede eliminar el registro, a continuación, se muestra en una segunda página que les permite confirmar que desea eliminar el registro.

> [!NOTE] 
> 
> **Importante** en un sitio Web de producción, normalmente restringir quién se puede realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y sobre los modos para autorizar el usuario para realizar tareas en el sitio, consulte [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. En el sitio Web, cree un nuevo archivo CSHTML denominado *ListProductsForDelete.cshtml*.
2. Reemplace el marcado existente con lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Esta página es similar a la *EditProducts.cshtml* página desde versiones anteriores. Sin embargo, en lugar de mostrar una **editar** vínculo para cada producto, muestra un **eliminar** vínculo. El **eliminar** vínculo se crea mediante el siguiente código incrustado en el marcado:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Esto crea una dirección URL que aspecto cuando los usuarios, haga clic en el vínculo:

    `http://<server>/DeleteProduct/4`

    Llama a la dirección URL de una página denominada *DeleteProduct.cshtml* (que se creará a continuación) y pasa el identificador del producto que se va a eliminar (aquí, 4).
3. Guarde el archivo, pero dejarla abierta.
4. Cree otro archivo CHTML denominado *DeleteProduct.cshtml*. Reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Llama a esta página *ListProductsForDelete.cshtml* y permite a los usuarios confirmar que desea eliminar un producto. Para obtener una lista del producto que se va a eliminar, obtendrá el identificador del producto que se va a eliminar de la dirección URL usando el siguiente código:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    La página, a continuación, pide al usuario que haga clic en un botón para eliminar realmente el registro. Se trata de una medida de seguridad importante: cuando se realizan operaciones confidenciales de su sitio Web como actualizar o eliminar datos, siempre se deben realizar estas operaciones mediante una operación POST, no una operación GET. Si el sitio está configurado para que se puede realizar una operación de eliminación con una operación GET, cualquier usuario puede pasar una dirección URL como `http://<server>/DeleteProduct/4` y todo lo que desea eliminar de la base de datos. Agregando la confirmación y la página de codificación para que la eliminación puede realizarse únicamente mediante una entrada de blog, agrega una medida de seguridad a su sitio.

    La operación de eliminación real se realiza con el código siguiente, que confirma primero que se trata de una operación post y que el identificador no está vacío:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    El código ejecuta una instrucción SQL que elimina el registro especificado y, a continuación, redirige al usuario a la página de lista.
5. Ejecutar *ListProductsForDelete.cshtml* en un explorador.

    ![[image]](5-working-with-data/_static/image9.jpg)
6. Haga clic en el **eliminar** vínculo de uno de los productos. El *DeleteProduct.cshtml* página se muestra para confirmar que desea eliminar este registro.
7. Haga clic en el **eliminar** botón. Se elimina el registro del producto y la página se actualiza con una lista actualizada del producto.

> [!TIP] 
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Conectarse a una base de datos
> 
> Puede conectarse a una base de datos de dos maneras. La primera consiste en utilizar el `Database.Open` método y para especificar el nombre del archivo de base de datos (menos la *.sdf* extensión):
> 
> `var db = Database.Open("SmallBakery");`
> 
> El `Open` método supone que el. *sdf* archivo se encuentra en el sitio de Web *aplicación\_datos* carpeta. Esta carpeta está diseñada específicamente para almacenar datos. Por ejemplo, tiene los permisos adecuados para permitir que el sitio Web leer y escribir datos, y como medida de seguridad, WebMatrix no permite el acceso a los archivos de esta carpeta.
> 
> La segunda manera es utilizar una cadena de conexión. Una cadena de conexión contiene información sobre cómo conectarse a una base de datos. Esto puede incluir una ruta de acceso de archivo, o puede incluir el nombre de una base de datos de SQL Server en un servidor local o remoto, junto con un nombre de usuario y una contraseña para conectarse a ese servidor. (Si conservar los datos en una versión de SQL Server administrada centralmente, como en el sitio de un proveedor de hospedaje, siempre utilizar una cadena de conexión para especificar la información de conexión de base de datos.)
> 
> En WebMatrix, las cadenas de conexión se almacenan normalmente en un archivo XML denominado *Web.config*. Como su nombre indica, puede usar un *Web.config* archivo en la raíz del sitio Web para almacenar información de configuración del sitio, incluidas las cadenas de conexión que puede requerir su sitio. Un ejemplo de una cadena de conexión en un *Web.config* archivo podría ser similar al siguiente:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> En el ejemplo, la cadena de conexión apunta a una base de datos en una instancia de SQL Server que se ejecuta en un servidor en algún lugar (en lugar de una variable local *.sdf* archivo). Debe sustituir los nombres correspondientes de `myServer` y `myDatabase`y especifique los valores de inicio de sesión de SQL Server para `username` y `password`. (Los valores de nombre de usuario y la contraseña no son necesariamente el mismo como sus credenciales de Windows o como los valores que el proveedor de hospedaje ha proporcionado para iniciar una sesión en sus servidores. Póngase en contacto con el administrador para los valores exactos que necesita.)
> 
> El `Database.Open` método es flexible, ya que le permite pasar el nombre de una base de datos *.sdf* archivo o el nombre de una cadena de conexión que se almacena en la *Web.config* archivo. En el ejemplo siguiente se muestra cómo conectarse a la base de datos mediante la cadena de conexión se muestra en el ejemplo anterior:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Como se mencionó, la `Database.Open` método le permite pasar un nombre de base de datos o una cadena de conexión y podrá averiguar que se va a usar. Esto resulta muy útil al implementar (publicar) el sitio Web. Puede usar un *.sdf* un archivo en el *aplicación\_datos* carpeta cuando está desarrollar y probar su sitio. Cuando se mueve el sitio a un servidor de producción, puede utilizar una cadena de conexión en el *Web.config* archivo que tiene el mismo nombre que su *.sdf* archivo pero que &#8212;sin tener que cambiar el código.
> 
> Por último, si desea trabajar directamente con una cadena de conexión, puede llamar a la `Database.OpenConnectionString` método y pase, la conexión real de cadena en lugar de simplemente el nombre de un elemento de la *Web.config* archivo. Esto puede resultar útil en situaciones donde por algún motivo no tiene acceso a la cadena de conexión (o valores en él, como el *.sdf* nombre de archivo) hasta que la página se está ejecutando. Sin embargo, para la mayoría de los escenarios, puede usar `Database.Open` tal como se describe en este artículo.


## <a name="additional-resources"></a>Recursos adicionales

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Conectarse a SQL Server o de base de datos MySQL en WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validar la entrada del usuario en los sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
