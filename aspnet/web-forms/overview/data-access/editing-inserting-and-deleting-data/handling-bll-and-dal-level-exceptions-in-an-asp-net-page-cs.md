---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Control de excepciones de nivel de DAL y BLL en una página ASP.NET (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial veremos cómo se muestra un mensaje de error informativo y sencillo debe producir una excepción durante la operación de inserción, actualización o delete de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: e7589584f3b0773a739b785c433ec45eeac3607e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Control de excepciones de nivel de DAL y BLL en una página ASP.NET (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) o [descarga de PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> En este tutorial veremos cómo se muestra un mensaje de error informativo y sencillo debe producir una excepción durante una inserción, actualización o la operación de eliminación de un control Web de datos ASP.NET.


## <a name="introduction"></a>Introducción

Trabajar con datos de una aplicación web ASP.NET con una arquitectura de aplicación en niveles implica los siguientes tres pasos generales:

1. Determinar qué método de la capa de lógica de negocios se necesita invocar y valores de los parámetros para pasarla. Los valores de parámetro pueden ser codificado, asignado mediante programación o entradas especificado por el usuario.
2. Invoque al método.
3. Procesar los resultados. Al llamar a un método BLL que devuelve datos, esto puede implicar el enlace de datos a un control Web de datos. Para los métodos BLL que modifican datos, esto puede incluir realizar alguna acción en función de un valor devuelto o manipular correctamente cualquier excepción que se produjo en el paso 2.

Como vimos en el [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), ObjectDataSource y los controles Web de datos proporcionan puntos de extensibilidad para los pasos 1 y 3. El control GridView, por ejemplo, se activa su `RowUpdating` eventos antes de asignar sus valores de campo a su ObjectDataSource `UpdateParameters` colección; su `RowUpdated` evento se desencadena después de que el ObjectDataSource ha finalizado la operación.

Ya hemos examinado los eventos que se activan durante el paso 1 y han visto cómo puede utilizar para personalizar los parámetros de entrada o cancelar la operación. En este tutorial se podrá nos centraremos en los eventos que activan una vez completada la operación. Con estos controladores de eventos posterior al nivel que se puede, entre otras cosas, determinar si se produjo una excepción durante la operación y maneje debidamente, mostrar un mensaje de error informativo y sencillo en la pantalla, en lugar de con el estándar de ASP.NET predeterminado página de excepción.

Para ilustrar trabajar con estos eventos posteriores al niveles, vamos a crear una página que enumera los productos en un control GridView editable. Al actualizar un producto, si es una excepción provoca nuestro ASP.NET página mostrará un mensaje breve anteriormente GridView que explica que se ha producido un problema. Comencemos.

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Paso 1: Crear un control GridView Editable de productos

En el tutorial anterior, creamos un control GridView editable con dos campos, `ProductName` y `UnitPrice`. Esto es necesario crear una sobrecarga adicional para la `ProductsBLL` la clase  `UpdateProduct` /método siguiente, que sólo acepta tres parámetros de entrada (nombre del producto, precio unitario e Id.) de diferencia un parámetro para cada campo de producto. Para este tutorial, vamos a practicar esta técnica de nuevo, crear un control GridView editable que muestra el nombre del producto, cantidad por unidad, precio unitario y unidades en existencias, pero solo se permite el nombre, precio unitario y las unidades en existencias para editarse.

Para admitir este escenario se necesitará otra sobrecarga de la  `UpdateProduct` /método siguiente, que acepta cuatro parámetros: el nombre del producto, precio unitario, unidades en existencias y el identificador. Agregue el método siguiente a la `ProductsBLL` clase:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Con este método completa, se está listos para crear la página ASP.NET que permite modificar estos cuatro campos de producto en particular. Abra la `ErrorHandling.aspx` página en el `EditInsertDelete` carpeta y agregar un control GridView a la página a través del diseñador. Enlazar el control GridView a un nuevo origen ObjectDataSource, asignación el `Select()` método para el `ProductsBLL` la clase `GetProducts()` (método) y la `Update()` método para el `UpdateProduct` sobrecarga que acaba de crear.


[![Utilice la sobrecarga del método de UpdateProduct que acepta cuatro parámetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Figura 1**: Use la `UpdateProduct` método de sobrecarga que acepta cuatro parámetros de entrada ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Esto creará un ObjectDataSource con un `UpdateParameters` colección con cuatro parámetros y un control GridView con un campo para cada uno de los campos de producto. Marcado declarativo del ObjectDataSource asigna el `OldValuesParameterFormatString` el valor de la propiedad `original_{0}`, que producirá una excepción desde nuestra clase BLL no espera un parámetro de entrada denominado `original_productID` deben pasarse en. No olvide quitar por completo esta configuración de la sintaxis declarativa (o establezca el valor predeterminado, `{0}`).

A continuación, reducir la GridView sólo incluirá el `ProductName`, `QuantityPerUnit`, `UnitPrice`, y `UnitsInStock` BoundFields. También puede aplicar cualquier formato de nivel de campo que considere necesario (por ejemplo, cambiar el `HeaderText` propiedades).

En el tutorial anterior, observamos cómo dar formato a la `UnitPrice` BoundField como una moneda en modo de solo lectura y en modo de edición. Vamos a hacer el mismo aquí. Recuerde esto necesario establecer la BoundField `DataFormatString` propiedad `{0:c}`, sus `HtmlEncode` propiedad `false`y su `ApplyFormatInEditMode` a `true`, tal y como se muestra en la figura 2.


[![Configurar el UnitPrice BoundField que se muestran como moneda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Figura 2**: configurar la `UnitPrice` BoundField que se muestran como una moneda ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Aplicar formato a la `UnitPrice` como una moneda en la interfaz de edición requiere la creación de un controlador de eventos del control de GridView `RowUpdating` eventos que analiza la cadena con formato de moneda en un `decimal` valor. Recuerde que el `RowUpdating` controlador de eventos desde el último tutorial también se comprueba para asegurarse de que el usuario proporciona un `UnitPrice` valor. Sin embargo, para este tutorial vamos a permite al usuario omitir el precio.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Nuestro GridView incluye un `QuantityPerUnit` BoundField, pero este BoundField debe ser solo con fines de visualización y no debe ser editable por el usuario. Para ello, basta con establecer la BoundFields `ReadOnly` propiedad `true`.


[![Hacer el QuantityPerUnit BoundField de solo lectura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Figura 3**: realizar la `QuantityPerUnit` BoundField de solo lectura ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Por último, active la casilla Habilitar edición de etiquetas inteligentes de GridView. Después de completar estos pasos la `ErrorHandling.aspx` Diseñador de la página debe ser similar a la figura 4.


[![Quitar todos excepto el cliente necesita BoundFields y comprobación de activación de la casilla de verificación de edición](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Figura 4**: quitar todos menos el necesario BoundFields y activar la casilla Habilitar de edición ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


En este momento tenemos una lista de todos los productos `ProductName`, `QuantityPerUnit`, `UnitPrice`, y `UnitsInStock` campos; sin embargo, solo la `ProductName`, `UnitPrice`, y `UnitsInStock` se pueden editar los campos.


[![Los usuarios ahora pueden editar fácilmente los nombres de productos, precios y unidades en existencias campos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Figura 5**: nombres de los usuarios pueden ahora fácilmente editar productos, precios y campos de unidades en existencias ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Paso 2: Correctamente el control de excepciones de nivel de la capa DAL

Aunque nuestro GridView editable muy funciona cuando los usuarios escribir valores válidos para el nombre del producto editado, precio y unidades en existencias, introducir valores no válidos produce una excepción. Por ejemplo, si se omite la `ProductName` valor causas un [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) que se produzca desde el `ProductName` propiedad en el `ProdcutsRow` clase tiene su `AllowDBNull` propiedad establecida en `false`; si el base de datos no está activo, un `SqlException` producirá el TableAdapter al intentar conectarse a la base de datos. Sin realizar ninguna acción, estas excepciones se propagan desde la capa de acceso a datos a la capa de lógica de negocios, y después a la página ASP.NET y, finalmente, para el tiempo de ejecución ASP.NET.

Dependiendo de cómo se configura la aplicación web y el hecho de que está visitando la aplicación desde `localhost`, puede dar lugar a una excepción no controlada en una página de error de servidor genérico, un informe de error detallados o una página web fácil de usar. Vea [Web el control de los errores de aplicación en ASP.NET](http://www.15seconds.com/issue/030102.htm) y [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) para obtener más información sobre cómo el tiempo de ejecución ASP.NET responde a una excepción no detectada.

Figura 6 se muestra la pantalla al intentar actualizar un producto sin especificar el `ProductName` valor. Este es el valor predeterminado muestra el informe de error detallados cuando llegan a través de `localhost`.


[![Omisión de detalles de la excepción del producto nombre Will presentación](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Figura 6**: si se omite nombre le mostrar detalles del producto de la excepción ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Aunque estos detalles de excepción son útiles cuando se prueba una aplicación, presentar un usuario final con una pantalla de este tipo en el caso de una excepción es menor que ideal. Un usuario final es probable que no sabe qué es un `NoNullAllowedException` es o por qué se produjo. Es un enfoque más adecuado presentar al usuario un mensaje más descriptivo que explica que no hubo problemas al intentar actualizar el producto.

Si se produce una excepción al realizar la operación, los eventos posteriores al niveles en ObjectDataSource y el control de datos Web proporcionan un medio para detectarlo y cancelar la excepción que se propague hasta el tiempo de ejecución ASP.NET. En nuestro ejemplo, vamos a crear un controlador de eventos del control de GridView `RowUpdated` evento que determina si se ha desencadenado una excepción y, si es así, muestra los detalles de la excepción en un control Web Label.

Empiece por agregar una etiqueta a la página ASP.NET, establecer su `ID` propiedad `ExceptionDetails` y borrando su `Text` propiedad. Para dibujar los ojos del usuario a este mensaje, establecer su `CssClass` propiedad `Warning`, que es una clase CSS que hemos agregado a la `Styles.css` archivo en el tutorial anterior. Recuerde que esta clase CSS hace que el texto de la etiqueta que se mostrará en una fuente de color rojo, cursiva, negrita, extra grande.


[![Agregar un Control Web Label a la página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Figura 7**: agregar un Control Web Label a la página ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Puesto que deseamos que este control Web Label esté visible solo inmediatamente después se ha producido una excepción, establezca su `Visible` propiedad en false en el `Page_Load` controlador de eventos:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Con este código, en la primera página visita y postbacks posteriores el `ExceptionDetails` control tendrá su `Visible` propiedad establecida en `false`. En el caso de una excepción en el nivel de DAL o BLL, que se puede detectar en la GridView `RowUpdated` controlador de eventos, se establecerá el `ExceptionDetails` del control `Visible` propiedad en true. Debido a que los controladores de eventos de control Web aparecen después de la `Page_Load` el controlador de eventos en el ciclo de vida de la página, se mostrará la etiqueta. Sin embargo, en la devolución de datos siguiente, la `Page_Load` controlador de eventos, se restablecerá la `Visible` propiedad volver a `false`, ocultar de la vista de nuevo.

> [!NOTE]
> Como alternativa, podríamos quitamos la necesidad de configuración de la `ExceptionDetails` del control `Visible` propiedad en `Page_Load` asignando su `Visible` propiedad `false` en la sintaxis declarativa y deshabilitar su estado de vista (estableciendo su `EnableViewState` propiedad `false`). Vamos a usar este enfoque alternativo en un tutorial posterior.


Con el control de etiqueta que se agregan, el siguiente paso es crear el controlador de eventos para el GridView `RowUpdated` eventos. Seleccione el control GridView en el diseñador, vaya a la ventana Propiedades y haga clic en el icono de rayo, lista de eventos de GridView. Ya debe haber una entrada no existe para la GridView `RowUpdating` eventos, como un controlador de eventos para este evento se creó anteriormente en este tutorial. Crear un controlador de eventos para el `RowUpdated` así el evento.


![Crear un controlador de eventos para el evento RowUpdated de GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Figura 8**: crear un controlador de eventos del control de GridView `RowUpdated` eventos


> [!NOTE]
> También puede crear el controlador de eventos a través de las listas desplegables en la parte superior del archivo de clase de código subyacente. Seleccione el control GridView en la lista desplegable de la izquierda y la `RowUpdated` eventos de la de la derecha.


Creación de este controlador de eventos agregará el código siguiente a la clase de código subyacente de la página ASP.NET:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Segundo parámetro de entrada de este controlador de eventos es un objeto de tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tiene tres propiedades de interés para controlar las excepciones:

- `Exception` una referencia a la excepción; Si no se ha producido ninguna excepción, esta propiedad tendrá un valor de `null`
- `ExceptionHandled` un valor booleano que indica si se controló la excepción en el `RowUpdated` controlador de eventos; si `false` (valor predeterminado), la excepción se vuelve a producir, filtre hasta el tiempo de ejecución ASP.NET
- `KeepInEditMode` Si establece en `true` la fila editada de GridView permanece en modo de edición; si `false` (valor predeterminado), la fila de GridView vuelve a su modo de solo lectura

A continuación, el código, debe comprobar para ver si `Exception` no es `null`, lo que significa que se generó una excepción al realizar la operación. Si este es el caso, queremos que:

- Mostrar un mensaje descriptivo en la `ExceptionDetails` etiqueta
- Indicar que se controló la excepción
- Mantener la fila de GridView en modo de edición

El código siguiente lleva a cabo con estos objetivos:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Este controlador de eventos se inicia y comprueba si `e.Exception` es `null`. Si no lo está, el `ExceptionDetails` etiqueta `Visible` propiedad está establecida en `true` y su `Text` propiedad a "Se produjo un problema al actualizar el producto". Los detalles de la excepción lanzada residen en el `e.Exception` del objeto `InnerException` propiedad. Se examina la excepción interna y, si es de un tipo determinado, se anexa un mensaje adicional, útil para la `ExceptionDetails` etiqueta `Text` propiedad. Por último, el `ExceptionHandled` y `KeepInEditMode` propiedades se establecen como `true`.

Ilustración 9 se muestra una captura de pantalla de esta página al usar el nombre del producto; La figura 10 muestra los resultados al especificar un valor ilegal `UnitPrice` valor (de -50).


[![El ProductName BoundField debe contener un valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Figura 9**: el `ProductName` BoundField debe contener un valor ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Los valores negativos UnitPrice no son permitidos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Figura 10**: negativo `UnitPrice` valores no son permitido ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Estableciendo la `e.ExceptionHandled` propiedad `true`, el `RowUpdated` controlador de eventos ha indicado que controló la excepción. Por lo tanto, no propaga la excepción hasta el tiempo de ejecución ASP.NET.

> [!NOTE]
> Las figuras 9 y 10 se muestra una manera estable para controlar las excepciones generadas por proporcionados por el usuario no válido. Lo ideal es que, sin embargo, este tipo de entrada no válidos no superará nunca el alcance de la capa de lógica de negocios en primer lugar, como la página ASP.NET debe asegurarse de que las entradas del usuario son válidas antes de invocar el `ProductsBLL` la clase `UpdateProduct` método. En el siguiente tutorial que veremos cómo agregar controles de validación a las interfaces de edición e inserción para asegurarse de que los datos que se envían a la capa de lógica de negocios se ajusta a las reglas de negocios. Los controles de validación no solo impiden que la invocación de la `UpdateProduct` método hasta que los datos proporcionados por el usuario es válidos, pero también proporcionan una experiencia de usuario más informativa para identificar los problemas de entrada de datos.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Paso 3: Correctamente el control de excepciones de nivel de BLL

Al insertar, actualizar o eliminar datos, la capa de acceso a datos puede producir una excepción en el caso de un error relacionado con datos. La base de datos puede estar sin conexión, puede que una columna de tabla de base de datos requerida no hayan tenido un valor especificado o han infringido una restricción de nivel de tabla. Además de las excepciones estrictamente relacionado con los datos, la capa de lógica empresarial puede usar excepciones para indicar cuándo se han infringido las reglas de negocios. En el [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md) tutorial, por ejemplo, hemos agregado una comprobación de la regla de negocios a la versión original `UpdateProduct` sobrecarga. En concreto, si el usuario marca un producto como discontinuo, hemos necesitado que el producto no sea la única persona proporcionada por su proveedor. Si esta condición se ha infringido, un `ApplicationException` se inició.

Para el `UpdateProduct` sobrecarga creada en este tutorial, vamos a agregar una regla de negocios que prohíbe el `UnitPrice` arrastrándolo desde la que se va a establecer en un valor nuevo que es más del doble original `UnitPrice` valor. Para lograr esto, ajustar el `UpdateProduct` sobrecarga para que se realiza esta comprobación y emite una `ApplicationException` si se infringe la regla. El método actualizado que se indica a continuación:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Con este cambio, cualquier actualización de precio que es más del doble el precio existente provocarán una `ApplicationException` que se produzca. Al igual que la excepción que se generan a partir de la capa DAL, este genera de BLL `ApplicationException` se pueda detectar y controlar en la GridView `RowUpdated` controlador de eventos. De hecho, el `RowUpdated` código del controlador de eventos, tal y como se escribe, correctamente detectará esta excepción y mostrar el `ApplicationException`del `Message` valor de propiedad. La figura 11 muestra una captura de pantalla cuando un usuario intenta actualizar el precio de Chai a 50,00 $, que es más del doble su precio actual de 19,95 $.


[![Las reglas de negocios no permitir bonificaciones que más del doble de precio de un producto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Figura 11**: no permitir bonificaciones que más del doble de precio de un producto de las reglas de negocios ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> Lo ideal es que nuestro reglas de lógica de negocios se puede refactorizar fuera de la `UpdateProduct` sobrecargas de método y en un método común. Esto queda como un ejercicio para el lector.


## <a name="summary"></a>Resumen

Durante la inserción, actualización y las operaciones de eliminación, tanto lo datos Web y el control ObjectDataSource implicados activan eventos previos y posteriores al niveles delimitador de la operación real. Como hemos visto en este tutorial y la anterior, cuando se trabaja con un control GridView editable la GridView `RowUpdating` desencadena el evento, seguido por el ObjectDataSource `Updating` eventos, momento en que el comando de actualización se realiza en el ObjectDataSource objeto subyacente. Una vez haya completado la operación, el ObjectDataSource `Updated` desencadena el evento, seguido por la GridView `RowUpdated` eventos.

Podemos crear controladores de eventos para los eventos de niveles previamente con el fin de personalizar los parámetros de entrada o para los eventos posteriores al niveles para inspeccionar y responder a los resultados de la operación. Controladores de eventos posterior al nivel suelen usarse para detectar si se produjo una excepción durante la operación. En el caso de una excepción, estos controladores de eventos posterior al nivel, opcionalmente, pueden controlar la excepción por sí solas. En este tutorial, hemos visto cómo controlar esta excepción mediante un mensaje de error descriptivo.

En el siguiente tutorial veremos cómo reducir la probabilidad de que las excepciones resultantes de los problemas de formato de los datos (por ejemplo, especificar negativo `UnitPrice`). En concreto, analizaremos cómo agregar controles de validación a las interfaces de edición e inserción.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Liz Shulok. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [Siguiente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
