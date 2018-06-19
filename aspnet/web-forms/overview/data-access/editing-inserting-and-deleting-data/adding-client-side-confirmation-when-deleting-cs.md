---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Adición de confirmación del cliente cuando se elimina (C#) | Documentos de Microsoft
author: rick-anderson
description: En las interfaces que hemos creado hasta ahora, un usuario puede eliminar accidentalmente datos haciendo clic en el botón Eliminar cuando realmente deseaba hacer clic en el botón Editar. En este t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 72b15d498e45cc519a14ecfe39111b224db88c30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880276"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Adición de confirmación del cliente cuando se elimina (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) o [descarga de PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> En las interfaces que hemos creado hasta ahora, un usuario puede eliminar accidentalmente datos haciendo clic en el botón Eliminar cuando realmente deseaba hacer clic en el botón Editar. En este tutorial vamos a agregar un cuadro de diálogo de confirmación del cliente que aparece cuando se hace clic en el botón Eliminar.


## <a name="introduction"></a>Introducción

En los últimos varios tutoriales se ha visto cómo usar la arquitectura de la aplicación, ObjectDataSource y los controles Web de datos conjuntamente para proporcionar Insertar, modificar y eliminar las capacidades. La eliminación de las interfaces se ha examinado hasta ahora se han formada por una eliminación botón que, cuando hace clic en, provoca una devolución de datos e invoca las operaciones de asignación ObjectDataSource `Delete()` método. El `Delete()` método, a continuación, invoca el método configurado de la capa de lógica empresarial, lo que propaga la llamada bajando a la capa de acceso a datos, emisión real `DELETE` instrucción a la base de datos.

Aunque esta interfaz de usuario permite a los visitantes eliminar registros a través de los controles GridView, DetailsView o FormView, no tiene a ningún tipo de confirmación cuando el usuario hace clic en el botón Eliminar. Si un usuario accidentalmente hace clic en el botón Eliminar cuando realmente deseaba hacer clic en Editar, en su lugar, se eliminará el registro que pretenden actualizar. Para ayudar a evitar este problema, en este tutorial vamos a agregar un cuadro de diálogo de confirmación del cliente que aparece cuando se hace clic en el botón Eliminar.

El código JavaScript `confirm(string)` función muestra su parámetro de entrada de cadena como el texto dentro de un cuadro de diálogo modal que está equipado con dos botones: Aceptar y cancela (consulte la figura 1). El `confirm(string)` función devuelve un valor booleano dependiendo de qué botón se hizo clic (`true`, si el usuario hace clic en Aceptar, y `false` si hacen clic en Cancelar).


![El método de JavaScript confirm(string) muestra un estado Modal, cuadro de mensajes de cliente](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figura 1**: el código JavaScript `confirm(string)` método muestra un cuadro de mensaje Modal, de cliente


Durante el envío de un formulario, si un valor de `false` se devuelve desde un controlador de eventos de cliente, a continuación, se cancela el envío del formulario. Con esta característica, podemos hacer que el lado de cliente de botón s Delete `onclick` controlador de eventos devuelve el valor de una llamada a `confirm("Are you sure you want to delete this product?")`. Si el usuario hace clic en Cancelar, `confirm(string)` devolverá false, lo que produce el envío del formulario Cancelar. Con ninguna devolución de datos, puede eliminar el producto se ha hecho clic cuyo botón Eliminar won t. Si, sin embargo, el usuario hace clic en Aceptar en el cuadro de diálogo de confirmación, la devolución de datos seguirá sin cesar y se eliminará el producto. Consulte [de usar JavaScript s `confirm()` método de envío del formulario Control](http://www.webreference.com/programming/javascript/confirm/) para obtener más información sobre esta técnica.

Agregar el script de cliente necesario varía ligeramente si utilizan plantillas que cuando se usan un CommandField. Por lo tanto, en este tutorial, veremos un FormView y GridView ejemplo.

> [!NOTE]
> Uso de técnicas de confirmación del cliente, como las que se tratan en este tutorial, se da por supuesto que están visitando los usuarios con exploradores que admiten JavaScript y que tengan JavaScript habilitado. Si alguno de estos supuestos no son VERDADERO para un usuario determinado, haga clic en el botón Eliminar inmediatamente provocará una devolución de datos (no se muestra un cuadro de mensajes de confirmación).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Paso 1: Crear un FormView que admite la eliminación

Empiece agregando un FormView para la `ConfirmationOnDelete.aspx` página en el `EditInsertDelete` carpeta, enlazarlo a un nuevo ObjectDataSource que extrae la información de producto a través de nuevo el `ProductsBLL` clase s. `GetProducts()` método. Configurar el ObjectDataSource para que la `ProductsBLL` clase s `DeleteProduct(productID)` método se asigna a las operaciones de asignación ObjectDataSource `Delete()` método; Asegúrese de que las pestañas INSERT y UPDATE listas desplegables se establecen en (ninguno). Por último, active la casilla Habilitar paginación en la etiqueta inteligente de s de FormView.

Después de realizar estos pasos, el marcado declarativo de nuevo ObjectDataSource s tendrá un aspecto similar al siguiente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Como en nuestros ejemplos anteriores que no utilizan la simultaneidad optimista, tómese un momento para borrar la s ObjectDataSource `OldValuesParameterFormatString` propiedad.

Puesto que se ha enlazado a un control ObjectDataSource que solo admite la eliminación, la s FormView `ItemTemplate` ofrece sólo el botón de eliminación, que carecen de los botones de New y Update. El marcado declarativo de FormView s, sin embargo, incluye un superfluas `EditItemTemplate` y `InsertItemTemplate`, que se pueden quitar. Tómese un momento para personalizar el `ItemTemplate` por lo que es solo un subconjunto del producto se muestran los campos de datos. Se ha configurado extraiga para mostrar el nombre del producto s en un `<h3>` encabezado por encima de sus nombres de proveedor y categoría (junto con el botón Eliminar).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Con estos cambios, tenemos una página web totalmente funcional que permite a un usuario alternar a través de los productos de uno en uno, con la posibilidad de eliminar un producto, simplemente haga clic en el botón Eliminar. La figura 2 muestra una captura de pantalla de nuestro progreso hasta el momento cuando se ven a través de un explorador.


[![FormView muestra información acerca de un único producto](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figura 2**: The FormView muestra información acerca de un único producto ([haga clic aquí para ver la imagen a tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Paso 2: Llamar a la función confirm(string) desde el lado de cliente botones Eliminar OnClick (evento)

Con FormView creado, el último paso es configurar el botón Eliminar este tipo que, cuando se hizo clic s el visitante, el código JavaScript `confirm(string)` función se invoca. Agregar script del lado cliente a un botón, LinkButton o ImageButton s de cliente `onclick` evento puede realizarse mediante el uso de la `OnClientClick property`, que es una novedad de ASP.NET 2.0. Puesto que deseamos que tengan el valor de la `confirm(string)` simplemente devuelve la función, establezca esta propiedad en: `return confirm('Are you certain that you want to delete this product?');`

Después de este cambio debe ser la sintaxis declarativa de eliminar LinkButton s similar:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

S todo es a él. La figura 3 muestra una captura de pantalla de esta confirmación en acción. Haga clic en el botón Eliminar, se abrirá el cuadro de diálogo de confirmación. Si el usuario hace clic en Cancelar, se cancela la devolución de datos y no se elimina el producto. Si, sin embargo, el usuario hace clic en Aceptar, continúa la devolución de datos y las operaciones de asignación ObjectDataSource `Delete()` se invoca el método, hasta la preinstalación en el registro de base de datos que se va a eliminar.

> [!NOTE]
> La cadena que se pasa en el `confirm(string)` función de JavaScript se delimita con apóstrofos (en lugar de las comillas). En JavaScript, las cadenas se pueden delimitar con cualquier carácter. Apóstrofos aquí utilizamos para que los delimitadores de la cadena pasan `confirm(string)` no se introducen ambigüedad con los delimitadores que se utilizan para la `OnClientClick` valor de propiedad.


[![Una confirmación es ahora aparece cuando hace clic en el botón Eliminar.](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figura 3**: una confirmación es ahora aparece cuando hace clic en el botón Eliminar ([haga clic aquí para ver la imagen a tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Paso 3: Configurar la propiedad OnClientClick para el botón Eliminar en un CommandField

Cuando se trabaja con un botón, LinkButton o ImageButton directamente en una plantilla, un cuadro de diálogo de confirmación puede asociarse con él mediante la configuración de simplemente su `OnClientClick` propiedad para devolver los resultados de JavaScript `confirm(string)` función. Sin embargo, no tiene el CommandField - que agrega un campo de botones de eliminación a un GridView o DetailsView - un `OnClientClick` propiedad que se pueden establecer mediante declaración. En su lugar, se debemos hacer referencia mediante programación el botón Eliminar en la s DetailsView o GridView adecuado `DataBound` controlador de eventos y, después, establezca su `OnClientClick` propiedad no existe.

> [!NOTE]
> Al establecer el botón Eliminar s `OnClientClick` propiedad en los correspondientes `DataBound` controlador de eventos, se tiene acceso a los datos se enlazarán al registro actual. Esto significa que podemos extender el mensaje de confirmación para incluir detalles sobre el registro concreto, como por ejemplo, "¿está seguro de que desea eliminar el producto Chai?" Dicha personalización también es posible en las plantillas mediante la sintaxis de enlace de datos.


Para practicar la configuración de la `OnClientClick` propiedad del botón de eliminación en un CommandField, s permiten agregar un control GridView a la página. Configure este GridView para utilizar el mismo control ObjectDataSource que usa el FormView. Limitar las operaciones de asignación GridView BoundFields para incluir sólo el nombre del producto s, la categoría y el proveedor. Por último, active la casilla Habilitar eliminación de la etiqueta inteligente de s de GridView. Esto agregará una CommandField a las operaciones de asignación GridView `Columns` colección con su `ShowDeleteButton` propiedad establecida en `true`.

Después de realizar estos cambios, el código de marcado declarativo de GridView s debería ser similar al siguiente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

El CommandField contiene una sola instancia de eliminar LinkButton que puede tener acceso mediante programación desde la s GridView `RowDataBound` controlador de eventos. Una vez que se hace referencia a, podemos establecer su `OnClientClick` propiedad según corresponda. Crear un controlador de eventos para el `RowDataBound` eventos utilizando el código siguiente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Este controlador de eventos funciona con filas de datos (los que tendrá el botón Eliminar) y comienza por referencia mediante programación el botón Eliminar. En general, utilice el modelo siguiente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* es el tipo de botón que se usa el CommandField - Button, LinkButton o ImageButton. De forma predeterminada, el CommandField usa LinkButton, pero esto se puede personalizar a través de las operaciones de asignación CommandField `ButtonType property`. El *commandFieldIndex* es el índice ordinal de la CommandField dentro de las operaciones de asignación GridView `Columns` colección, mientras que la *controlIndex* es el índice del botón Delete dentro de las operaciones de asignación CommandField `Controls` colección. El *controlIndex* valor depende de la posición del botón s en relación con otros botones en la CommandField. Por ejemplo, si el botón solo aparece en el CommandField es el botón Eliminar, utilice un índice de 0. Si, sin embargo, s hay un botón Editar que precede el botón Eliminar, utilice un índice de 2. La razón es utilizar un índice de 2 es que se agregan dos controles mediante el CommandField antes el botón Eliminar: el botón Editar y LiteralControl s utilizado para agregar espacio entre los botones Editar y eliminar.

En nuestro ejemplo concreto, la CommandField utiliza LinkButton y, el campo del extremo izquierdo, ofrece un *commandFieldIndex* de 0. Dado que hay ningún otro botón pero el botón Eliminar en el CommandField, usamos un *controlIndex* de 0.

Después de hacer referencia al botón Eliminar en el CommandField, a continuación se obtenga información sobre el producto enlazado a la fila actual de GridView. Por último, configuramos el botón Eliminar s `OnClientClick` propiedad en el código de JavaScript adecuado, que incluye el nombre de producto s. Puesto que la cadena de JavaScript que se pasa en el `confirm(string)` función está delimitada con apóstrofos se deben escape apóstrofos que aparecen en el nombre del producto s. En concreto, cualquier apóstrofos en el nombre del producto s se escapan con "`\'`".

Con estos cambios completado, haga clic en un botón Eliminar en la muestra de GridView un cuadro de diálogo de confirmación personalizado cuadro (Véase la figura 4). Como con el cuadro de mensajes de confirmación de FormView, si el usuario hace clic en Cancelar la devolución de datos se cancela, lo que evita que la eliminación se produzcan.

> [!NOTE]
> Esta técnica también puede usarse para acceder mediante programación el botón Eliminar en el CommandField en DetailsView. Para DetailsView, sin embargo, d creas un controlador de eventos para el `DataBound` eventos, como el DetailsView no tiene un `RowDataBound` eventos.


[![Haga clic en el botón de eliminación de GridView s muestra un cuadro de diálogo de confirmación personalizado](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figura 4**: haga clic en el botón Eliminar de GridView s muestra un cuadro de diálogo de confirmación personalizado ([haga clic aquí para ver la imagen a tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Uso de TemplateFields

Uno de los inconvenientes de la CommandField es que se deben tener acceso a sus botones mediante el índice y que el objeto resultante debe convertirse al tipo adecuado de botón (Button, LinkButton o ImageButton). Usar "números de magia" y tipos codificados de forma rígida invita a problemas que no se puede detectar hasta el tiempo de ejecución. Por ejemplo, si usted, u otro programador, agrega nuevos botones a la CommandField en algún momento en el futuro (por ejemplo, un botón Editar) o cambios el `ButtonType` propiedad, el código existente sigue se compilará sin errores, pero visitar la página puede provocar una excepción o un comportamiento inesperado, dependiendo de cómo se escribe el código y los cambios realizados.

Un método alternativo consiste en convertir la s GridView y DetailsView CommandFields al TemplateFields. Esto generará un TemplateField con un `ItemTemplate` que tiene un LinkButton (o botón o ImageButton) para cada botón de la CommandField. Estos botones `OnClientClick` propiedades se pueden asignar mediante declaración, tal y como se vio con FormView o puede accederse mediante programación en los correspondientes `DataBound` controlador de eventos utilizando el modelo siguiente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Donde *controlID* es el valor del botón s `ID` propiedad. Aunque este patrón todavía requiere un tipo codificado de forma rígida para la conversión, elimina la necesidad de indización, que permite que el diseño cambiar sin que produce un error en tiempo de ejecución.

## <a name="summary"></a>Resumen

El código JavaScript `confirm(string)` función es una técnica ampliamente usada para controlar flujo de trabajo de envío de formulario. Cuando se ejecuta, la función muestra un cuadro de diálogo modal, de cliente que incluye dos botones, Aceptar y Cancelar. Si el usuario hace clic en Aceptar, el `confirm(string)` función devuelve `true`; al hacer clic en Cancelar devuelve `false`. Esta funcionalidad, junto con un comportamiento del explorador s para cancelar el envío de un formulario si devuelve un controlador de eventos durante el proceso de envío `false`, puede utilizarse para mostrar un cuadro de mensajes de confirmación al eliminar un registro.

El `confirm(string)` función puede asociarse a un lado del cliente Web Button control s `onclick` controlador de eventos a través del control s `OnClientClick` propiedad. Cuando se trabaja con un botón Eliminar en una plantilla - en una de las plantillas de FormView s o en TemplateField en el DetailsView o GridView - esta propiedad puede establecerse mediante declaración o mediante programación, en tal y como hemos visto en este tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-cs.md)
> [Siguiente](limiting-data-modification-functionality-based-on-the-user-cs.md)
