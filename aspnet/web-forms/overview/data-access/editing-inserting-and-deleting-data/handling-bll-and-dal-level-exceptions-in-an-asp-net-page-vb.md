---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Control de excepciones de nivel BLL y DAL en una página ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo mostrar un mensaje de error informativo y sencillo debe producir una excepción durante la operación de inserción, actualización o delete de...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: de181391a074ec837d2f9d98d55f912883d76be2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808997"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Control de excepciones de nivel BLL y DAL en una página ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) o [descargar PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> En este tutorial, veremos cómo mostrar un mensaje informativo y sencillo error se debe producir una excepción durante una inserción, la actualización o la operación de eliminación de un control Web de datos ASP.NET.


## <a name="introduction"></a>Introducción

Trabajar con datos desde una aplicación web ASP.NET con una arquitectura en capas de aplicación implica los siguientes tres pasos generales:

1. Determinar qué método de la capa de lógica empresarial debe invocarse y valores de parámetro what para pasarla. Los valores de parámetro pueden ser codificado, asignado mediante programación o escrito por el usuario.
2. Invoque al método.
3. Procesar los resultados. Al llamar a un método BLL que devuelve datos, esto puede implicar el enlace de datos a un control Web de datos. Para los métodos BLL que modifican datos, esto puede incluir realizar alguna acción según un valor devuelto o controlar correctamente cualquier excepción que se produjeron en el paso 2.

Como hemos visto en el [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), ObjectDataSource y los controles Web de datos proporcionan puntos de extensibilidad para los pasos 1 y 3. El control GridView, por ejemplo, se activa su `RowUpdating` eventos antes de asignar sus valores de campo a su ObjectDataSource `UpdateParameters` colección; su `RowUpdated` evento se desencadena después de que el origen ObjectDataSource ha finalizado la operación.

Ya hemos examinado los eventos que se activan durante el paso 1 y han visto cómo se puede usar para personalizar los parámetros de entrada o cancelar la operación. En este tutorial, centraremos nuestra atención a los eventos que activan una vez completada la operación. Con estos controladores de eventos posteriores al nivel que se puede, entre otras cosas, determinar si se produjo una excepción durante la operación y lo maneje debidamente, mostrar un mensaje de error informativo y sencillo en la pantalla, en lugar de con el estándar de ASP.NET predeterminado página de excepciones.

Para ilustrar cómo trabajar con estos eventos posteriores al niveles, vamos a crear una página que enumera los productos en un control GridView editable. Al actualizar un producto, si es una excepción provoca nuestro ASP.NET página mostrará un mensaje breve por encima del control GridView que explica que se ha producido un problema. Comencemos.

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Paso 1: Creación de un control GridView Editable de productos

En el tutorial anterior, creamos un control GridView editable con solo dos campos, `ProductName` y `UnitPrice`. Esto requiere la creación de una sobrecarga adicional para la `ProductsBLL` la clase `UpdateProduct` método, que solo acepta tres parámetros de entrada (nombre del producto, precio unitario e Id.) en oposición como un parámetro para cada campo de producto. Para este tutorial, vamos a práctica esta técnica de nuevo, crear un control GridView editable que muestra el nombre del producto, cantidad por unidad, el precio por unidad y las unidades en existencias, pero solo permite el nombre, precio unitario y las unidades en existencias para editarse.

Para dar cabida a este escenario se necesitará otra sobrecarga de la `UpdateProduct` método, que acepta cuatro parámetros: el nombre del producto, precio unitario, las unidades en existencias y el identificador. Agregue el método siguiente a la `ProductsBLL` clase:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Con este método completa, ya estamos listos para crear la página ASP.NET que permite la edición de estos cuatro campos de producto en particular. Abra el `ErrorHandling.aspx` página en el `EditInsertDelete` carpeta y agregue un control GridView a la página a través del diseñador. Enlazar el control GridView a un nuevo origen ObjectDataSource, asignación el `Select()` método a la `ProductsBLL` la clase `GetProducts()` método y el `Update()` método a la `UpdateProduct` sobrecarga que acaba de crear.


[![Utilice la sobrecarga del método UpdateProduct que acepta cuatro parámetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figura 1**: Use el `UpdateProduct` método de sobrecarga que acepta cuatro parámetros de entrada ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Esto creará un origen ObjectDataSource con una `UpdateParameters` colección con cuatro parámetros y un control GridView con un campo para cada uno de los campos de producto. Marcado declarativo de ObjectDataSource se asigna el `OldValuesParameterFormatString` el valor de propiedad `original_{0}`, lo que provocará una excepción dado que nuestra clase BLL no espera un parámetro de entrada denominado `original_productID` pasarse. No olvide quitar por completo esta configuración de la sintaxis declarativa (o se establece en el valor predeterminado, `{0}`).

A continuación, reducir el control GridView para incluir solo el `ProductName`, `QuantityPerUnit`, `UnitPrice`, y `UnitsInStock` BoundFields. También puede aplicar cualquier formato de nivel de campo que considere necesario (por ejemplo, cambiar el `HeaderText` propiedades).

En el tutorial anterior hemos visto cómo dar formato a la `UnitPrice` BoundField como una moneda en modo de solo lectura y en modo de edición. Vamos a hacer el mismo aquí. Recuerde que esta configuración requerida del BoundField `DataFormatString` propiedad `{0:c}`, sus `HtmlEncode` propiedad a `false`y su `ApplyFormatInEditMode` a `true`, como se muestra en la figura 2.


[![Configurar el UnitPrice BoundField a mostrar como moneda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figura 2**: configurar el `UnitPrice` BoundField para mostrar como una moneda ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Aplicar formato a la `UnitPrice` como una moneda en la interfaz de edición requiere la creación de un controlador de eventos para el control de GridView `RowUpdating` eventos que analiza la cadena con formato de moneda en un `decimal` valor. Recuerde que el `RowUpdating` controlador de eventos desde el último tutorial también se comprueba para asegurarse de que el usuario proporcionado un `UnitPrice` valor. Sin embargo, para este tutorial vamos a permitir que el usuario omitir el precio.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Nuestro GridView incluye un `QuantityPerUnit` BoundField, pero este BoundField debe ser solo para fines de presentación y no debe ser editable por el usuario. Para ello, basta con establecer 'BoundFields `ReadOnly` propiedad `true`.


[![Hacer el QuantityPerUnit BoundField de sólo lectura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figura 3**: realizar la `QuantityPerUnit` BoundField de solo lectura ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Por último, active la casilla Habilitar edición de etiquetas inteligentes de GridView. Después de completar estos pasos el `ErrorHandling.aspx` Diseñador de la página debe ser similar a la figura 4.


[![Quite todos menos necesario BoundFields y comprobación de activación de la casilla de verificación de edición](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figura 4**: quitar todos excepto los necesarios BoundFields y activar la casilla Habilitar de edición ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


En este momento tenemos una lista de todos los productos `ProductName`, `QuantityPerUnit`, `UnitPrice`, y `UnitsInStock` campos; sin embargo, solo el `ProductName`, `UnitPrice`, y `UnitsInStock` se pueden editar los campos.


[![Los usuarios ahora pueden editar fácilmente los nombres de los productos, precios y unidades en existencias campos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figura 5**: los nombres a los usuarios puede ahora fácilmente editar los productos, precios y campos de unidades en existencias ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Paso 2: Controla correctamente las excepciones de nivel de DAL

Aunque nuestro control GridView editable maravillosamente funciona cuando los usuarios escriben los valores válidos para el nombre del producto editado, precio y las unidades en existencias, escriba los valores no válidos produce una excepción. Por ejemplo, si se omite el `ProductName` valor hace que un [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) que se produzca desde el `ProductName` propiedad en el `ProdcutsRow` clase tiene su `AllowDBNull` propiedad establecida en `false`; si el base de datos no está activo, un `SqlException` producirá el TableAdapter al intentar conectarse a la base de datos. Sin realizar ninguna acción, estas excepciones se propagan desde la capa de acceso a datos a la capa de lógica empresarial, a continuación, en la página ASP.NET y, finalmente, al tiempo de ejecución ASP.NET.

Dependiendo de cómo se configura la aplicación web y el hecho de que está visitando la aplicación desde `localhost`, una excepción no controlada puede dar lugar a una página de error de servidor genérico, un informe de error detallados o una página web fácil de usar. Consulte [Web el control de los errores de aplicación en ASP.NET](http://www.15seconds.com/issue/030102.htm) y [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) para obtener más información sobre cómo el tiempo de ejecución ASP.NET responde a una excepción no detectada.

Figura 6 se muestra la pantalla al intentar actualizar un producto sin especificar el `ProductName` valor. Éste es el predeterminado que se muestra el informe de error detallados cuando llegan a través de `localhost`.


[![Si se omite el nombre mostrará excepción los detalles de producto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figura 6**: omisión nombre le mostrar detalles del producto de la excepción ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Aunque estos detalles de la excepción son útiles cuando se prueba una aplicación, presentar un usuario final con dicha pantalla en caso de una excepción es ideal. Un usuario final es probable que no sabe qué es un `NoNullAllowedException` es o por qué se produjo. Un mejor enfoque es presentar al usuario un mensaje de más fácil de usar que explique que hubo problemas al intentar actualizar el producto.

Si se produce una excepción al realizar la operación, los eventos posteriores al niveles de ObjectDataSource y el control de datos Web proporcionan un medio para detectarlo y cancelar la excepción que se propague hasta el tiempo de ejecución ASP.NET. En nuestro ejemplo, vamos a crear un controlador de eventos para el control de GridView `RowUpdated` eventos que determina si se ha desencadenado una excepción y, si es así, muestra los detalles de excepción en un control Web de la etiqueta.

Empiece por agregar una etiqueta a la página ASP.NET, establecer su `ID` propiedad `ExceptionDetails` y borrarlos su `Text` propiedad. Para dibujar los ojos del usuario a este mensaje, establecer su `CssClass` propiedad `Warning`, que es una clase CSS que se ha agregado a la `Styles.css` archivo en el tutorial anterior. Recuerde que esta clase CSS que hace que el texto de la etiqueta que se mostrará en una fuente roja, cursiva, negrita, extra grande.


[![Agregar un Control Web Label a la página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figura 7**: agregar un Control Web Label a la página ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Puesto que deseamos que este control sea visible solo inmediatamente después se ha producido una excepción, establezca su `Visible` propiedad en false en el `Page_Load` controlador de eventos:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Con este código, en la primera visita de página y los postbacks subsiguientes del `ExceptionDetails` control tendrá su `Visible` propiedad establecida en `false`. En caso de una excepción de nivel DAL o BLL, que nos podemos detectar en el control de GridView `RowUpdated` controlador de eventos, estableceremos la `ExceptionDetails` del control `Visible` propiedad en true. Puesto que los controladores de eventos de control Web que se producen después de la `Page_Load` controlador de eventos del ciclo de vida de la página, se mostrará la etiqueta. Sin embargo, en la siguiente devolución, el `Page_Load` se restablecerá el controlador de eventos el `Visible` propiedad nuevo a `false`, ocultarla a vista de nuevo.

> [!NOTE]
> Como alternativa, podríamos quitamos la necesidad de configuración de la `ExceptionDetails` del control `Visible` propiedad en `Page_Load` asignando su `Visible` propiedad `false` en la sintaxis declarativa y deshabilitar su estado de vista (estableciendo su `EnableViewState` propiedad `false`). En un futuro tutorial, vamos a usar este enfoque alternativo.


Agregado el control de etiqueta, el siguiente paso es crear el controlador de eventos para el control de GridView `RowUpdated` eventos. Seleccione el control GridView en el diseñador, vaya a la ventana Propiedades y haga clic en el icono de rayo, muestran los eventos de GridView. Ya debe haber una entrada no existe para el control de GridView `RowUpdating` eventos, como hemos creado un controlador de eventos para este evento anteriormente en este tutorial. Crear un controlador de eventos para el `RowUpdated` también el evento.


![Crear un controlador de eventos para RowUpdated (evento de GridView)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figura 8**: crear un controlador de eventos para el control de GridView `RowUpdated` eventos


> [!NOTE]
> También puede crear el controlador de eventos a través de las listas desplegables en la parte superior del archivo de clase de código subyacente. Seleccione el control GridView en la lista desplegable de la izquierda y la `RowUpdated` eventos de uno en la derecha.


Creación de este controlador de eventos se agrega el código siguiente a la clase de código subyacente de la página ASP.NET:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Segundo parámetro de entrada de este controlador de eventos es un objeto de tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tiene tres propiedades de interés para el control de excepciones:

- `Exception` una referencia a la excepción; Si no se ha producido ninguna excepción, esta propiedad tendrá un valor de `null`
- `ExceptionHandled` un valor booleano que indica si se controló la excepción en el `RowUpdated` controlador de eventos; si `false` (valor predeterminado), la excepción se vuelve a producir, filtre hasta el tiempo de ejecución ASP.NET
- `KeepInEditMode` Si establece en `true` la fila GridView editada permanece en modo de edición; si `false` (valor predeterminado), la fila GridView vuelve a su modo de solo lectura

A continuación, nuestro código, debe comprobar para ver si `Exception` no `null`, lo que significa que se generó una excepción al realizar la operación. Si esto es así, nos gustaría:

- Mostrar un mensaje descriptivo en el `ExceptionDetails` etiqueta
- Indicar que se ha controlado la excepción
- Mantener las filas del control GridView en modo de edición

Este código lleva a cabo estos objetivos:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Este controlador de eventos comienza comprobando si `e.Exception` es `null`. Si no lo es, el `ExceptionDetails` la etiqueta `Visible` propiedad está establecida en `true` y su `Text` propiedad a "Se produjo un problema al actualizar el producto." Los detalles de la excepción que se ha producido residen en el `e.Exception` del objeto `InnerException` propiedad. Se examina la excepción interna y, si es de un tipo determinado, se anexa un mensaje adicional y útil a la `ExceptionDetails` la etiqueta `Text` propiedad. Por último, el `ExceptionHandled` y `KeepInEditMode` propiedades se establecen en `true`.

Figura 9 se muestra una captura de pantalla de esta página cuando se omite el nombre del producto; Figura 10 se muestran los resultados al especificar un valor ilegal `UnitPrice` valor (de -50).


[![El ProductName BoundField debe contener un valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figura 9**: el `ProductName` BoundField debe contener un valor ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Los valores de UnitPrice negativos no son permitidos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figura 10**: negativo `UnitPrice` los valores no son permitido ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Estableciendo el `e.ExceptionHandled` propiedad `true`, el `RowUpdated` controlador de eventos ha indicado que ha controlado la excepción. Por lo tanto, no propaga la excepción hasta el tiempo de ejecución ASP.NET.

> [!NOTE]
> Las figuras 9 y 10 muestran una forma correcta para controlar las excepciones generadas debido a la entrada del usuario no válido. Lo ideal es que, sin embargo, este tipo de entrada no válido no superará nunca alcance la capa de lógica empresarial en primer lugar, como la página ASP.NET debe asegurarse de que las entradas del usuario son válidas antes de invocar el `ProductsBLL` la clase `UpdateProduct` método. En nuestro siguiente tutorial que veremos cómo agregar controles de validación a las interfaces de edición e inserción para asegurarse de que los datos enviados a la capa de lógica empresarial se ajusta a las reglas de negocios. Los controles de validación no solo impiden que la invocación de la `UpdateProduct` método hasta que los datos proporcionados por el usuario es válidos, pero también proporcionan una experiencia de usuario más informativa para identificar problemas de entrada de datos.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Paso 3: Administrar correctamente las excepciones de nivel BLL

Al insertar, actualizar o eliminar datos, la capa de acceso a datos puede producir una excepción en caso de un error relacionado con datos. La base de datos puede estar sin conexión, es posible que una columna de tabla de base de datos necesarios no haya tenido un valor especificado o han infringido una restricción de nivel de tabla. Además de las excepciones estrictamente relacionado con los datos, la capa de lógica empresarial puede usar excepciones para indicar cuándo se han infringido las reglas de negocios. En el [crear una capa de lógica empresarial](../introduction/creating-a-business-logic-layer-vb.md) tutorial, por ejemplo, hemos agregado una comprobación de reglas de negocio a la versión original `UpdateProduct` sobrecargar. En concreto, si el usuario ha marcado un producto como discontinuo, hemos necesitado que el producto no sea el único proporcionado por su proveedor. Si esta condición se ha infringido, un `ApplicationException` se ha producido.

Para el `UpdateProduct` sobrecarga creado en este tutorial, vamos a agregar una regla de negocios que prohíbe el `UnitPrice` arrastrándolo desde la que se establece en un valor nuevo que es más de dos veces el original `UnitPrice` valor. Para ello, ajuste el `UpdateProduct` sobrecarga para que se realiza esta comprobación y se produce un `ApplicationException` si se infringe la regla. El método actualizado se indica a continuación:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Con este cambio, cualquier actualización de precios que es más de dos veces el precio existente producirá un `ApplicationException` que se produzca. Al igual que la excepción generada desde la capa DAL, este se genera el BLL `ApplicationException` puede detectar y tratar en la GridView `RowUpdated` controlador de eventos. De hecho, el `RowUpdated` código del controlador de eventos, que escribe, correctamente detectará esta excepción y mostrar el `ApplicationException`del `Message` valor de propiedad. Figura 11 muestra una captura de pantalla cuando un usuario intenta actualizar el precio de Chai a 50,00 dólares, que es más del doble su precio actual de US $19,95.


[![Las reglas de negocios no permitir bonificaciones que más del doble de precio de un producto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figura 11**: las reglas de negocios bonificaciones de no permitir que más del doble de precio de un producto ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> Lo ideal es que se podrían refactorizar nuestras reglas de lógica de negocio fuera de la `UpdateProduct` sobrecargas del método y en un método común. Esto queda como ejercicio para el lector.


## <a name="summary"></a>Resumen

Durante la inserción, actualización y eliminación de las operaciones, el control de datos Web y el origen ObjectDataSource implicados activan eventos previos y posteriores al niveles que la operación real del delimitador. Como hemos visto en este tutorial y la anterior, cuando se trabaja con un control GridView editable de GridView `RowUpdating` desencadena el evento, seguido de ObjectDataSource `Updating` eventos, momento en que se realiza el comando update de ObjectDataSource objeto subyacente. Una vez completada la operación, de ObjectDataSource `Updated` desencadena el evento, seguido de GridView `RowUpdated` eventos.

Podemos crear controladores de eventos para los eventos de niveles previamente con el fin de personalizar los parámetros de entrada o para los eventos posteriores al niveles para inspeccionar y responder a los resultados de la operación. Controladores de eventos posteriores al nivel se usan normalmente para detectar si se produjo una excepción durante la operación. En caso de una excepción, estos controladores de eventos posteriores a la de nivel, opcionalmente, pueden controlar la excepción por sí solos. En este tutorial hemos visto cómo controlar la excepción mediante un mensaje de error descriptivo.

En el siguiente tutorial veremos cómo reducir la probabilidad de que las excepciones derivadas de los problemas de formato de datos (por ejemplo, escriba un valor negativo `UnitPrice`). En concreto, echemos un vistazo cómo agregar controles de validación a las interfaces de edición e inserción.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Liz Shulok. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Siguiente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
