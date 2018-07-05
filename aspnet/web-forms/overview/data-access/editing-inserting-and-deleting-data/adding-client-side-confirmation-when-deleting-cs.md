---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Agregar la confirmación del cliente al eliminar (C#) | Microsoft Docs
author: rick-anderson
description: En las interfaces creadas hasta ahora, un usuario puede eliminar accidentalmente los datos haciendo clic en el botón Eliminar cuando realmente deseaba hacer clic en el botón Editar. En este tipo t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ad66e24198a83d32edeb9fddf7a6b648b993fcc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386567"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Agregar la confirmación del cliente al eliminar (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) o [descargar PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> En las interfaces creadas hasta ahora, un usuario puede eliminar accidentalmente los datos haciendo clic en el botón Eliminar cuando realmente deseaba hacer clic en el botón Editar. En este tutorial vamos a agregar un cuadro de diálogo de confirmación del cliente que aparece cuando se hace clic en el botón Eliminar.


## <a name="introduction"></a>Introducción

A través de los diversos tutoriales anteriores se ve sabe cómo utilizar nuestra arquitectura de aplicación, ObjectDataSource y los controles Web de datos conjuntamente para proporcionar la inserción, edición y eliminación de recursos. La eliminación de interfaces se ve examinar hasta ahora han formado una eliminación botón que, cuando hace clic en, hace que una devolución de datos e invoca la s ObjectDataSource `Delete()` método. El `Delete()` método, a continuación, invoca el método configurado desde la capa de lógica empresarial, lo que propaga la llamada hasta la capa de acceso a datos, emisión real `DELETE` instrucción a la base de datos.

Si bien esta interfaz de usuario permite a los visitantes a eliminar registros mediante los controles GridView, DetailsView o FormView, no tiene a ningún tipo de confirmación cuando el usuario hace clic en el botón Eliminar. Si un usuario accidentalmente hace clic en el botón Eliminar cuando realmente deseaba hacer clic en Editar, en su lugar, se eliminará el registro que pretenden actualizar. Para ayudar a evitar esto, en este tutorial vamos a agregar un cuadro de diálogo de confirmación del cliente que aparece cuando se hace clic en el botón Eliminar.

El código JavaScript `confirm(string)` función muestra su parámetro de entrada de cadena como el texto dentro de un cuadro de diálogo modal que viene equipado con dos botones: Aceptar y cancela (consulte la figura 1). El `confirm(string)` función devuelve un valor booleano dependiendo de qué botón se hizo clic (`true`, si el usuario hace clic en Aceptar, y `false` si hacen clic en Cancelar).


![El método de JavaScript confirm(string) muestra un estado Modal, cuadro de mensajes del lado cliente](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figura 1**: el código JavaScript `confirm(string)` método muestra un cuadro de mensaje Modal, del lado cliente


Durante un envío de formulario, si un valor de `false` se devuelve desde un controlador de eventos del lado cliente, a continuación, se ha cancelado el envío del formulario. Con esta característica, podemos tener la eliminación botón s del lado cliente `onclick` controlador de eventos devuelve el valor de una llamada a `confirm("Are you sure you want to delete this product?")`. Si el usuario hace clic en Cancelar, `confirm(string)` devolverá false, lo que causa el envío del formulario Cancelar. Con ninguna devolución de datos, se puede eliminar el producto cuyo botón Delete se hizo clic ganado t. Si, sin embargo, el usuario hace clic en Aceptar en el cuadro de diálogo de confirmación, la devolución de datos continuará sin alteraciones y se eliminará el producto. Consulte [s de usando JavaScript `confirm()` método de envío del Control formulario](http://www.webreference.com/programming/javascript/confirm/) para obtener más información sobre esta técnica.

Agregar el script de cliente necesario varía ligeramente si usa las plantillas que cuando se utiliza un CommandField. Por lo tanto, en este tutorial veremos un FormView y GridView de ejemplo.

> [!NOTE]
> Uso de técnicas de confirmación del cliente, como los descritos en este tutorial, se da por supuesto que visitan los usuarios con exploradores que admiten JavaScript y que tengan que JavaScript está habilitado. Si cualquiera de estas suposiciones no se cumplen para un usuario determinado, haga clic en el botón Eliminar inmediatamente provocará una devolución de datos (no mostrar un cuadro de mensajes de confirmación).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Paso 1: Crear un FormView que admite la eliminación

Empiece agregando un FormView para el `ConfirmationOnDelete.aspx` página en el `EditInsertDelete` carpeta, enlazarlo a un nuevo origen ObjectDataSource que extrae de nuevo la información del producto a través de la `ProductsBLL` clase s `GetProducts()` método. También configurar el origen ObjectDataSource para que la `ProductsBLL` clase s `DeleteProduct(productID)` método está asignado al origen ObjectDataSource s `Delete()` método; Asegúrese de que las pestañas INSERT y UPDATE listas desplegables se establecen en (None). Por último, active la casilla Habilitar paginación en la etiqueta inteligente de s de FormView.

Después de estos pasos, el marcado declarativo de nuevo origen ObjectDataSource s tendrá un aspecto similar al siguiente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Como en los ejemplos anteriores que no utilizan la simultaneidad optimista, dedique un momento para borrar la s ObjectDataSource `OldValuesParameterFormatString` propiedad.

Puesto que se ha enlazado a un control ObjectDataSource que solo admite la eliminación, la s FormView `ItemTemplate` ofrece sólo el botón de eliminación, que carecen de los botones New y Update. Sin embargo, el marcado declarativo s FormView, incluye un superfluo `EditItemTemplate` y `InsertItemTemplate`, que se pueden quitar. Dedique un momento para personalizar el `ItemTemplate` por lo que es solo un subconjunto del producto se muestran los campos de datos. Se ve configurado mío para mostrar el nombre del producto s en un `<h3>` encabezado por encima de sus nombres de categoría y proveedor (junto con el botón Eliminar).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Con estos cambios, tenemos una página web totalmente funcional que permite que un usuario activar o desactivar a través de los productos de uno en uno, con la capacidad de eliminar un producto simplemente haciendo clic en el botón Eliminar. Figura 2 muestra una captura de pantalla de nuestro progreso hasta ahora, cuando se ve mediante un explorador.


[![FormView muestra información acerca de un solo producto](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figura 2**: The FormView muestra información acerca de un solo producto ([haga clic aquí para ver imagen en tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Paso 2: Llamar a la función confirm(string) desde el evento onclick eliminar botones de cliente

Con FormView creado, el último paso es configurar el botón de eliminar este tipo que, cuando lo s hace clic en el visitante, el código JavaScript `confirm(string)` se invoca la función. Agregar script de cliente a un botón, LinkButton o ImageButton s del lado cliente `onclick` eventos pueden realizarse mediante el uso de la `OnClientClick property`, que es una novedad de ASP.NET 2.0. Puesto que deseamos que el valor de la `confirm(string)` simplemente devuelve la función, establezca esta propiedad en: `return confirm('Are you certain that you want to delete this product?');`

Después de este cambio debe ser la sintaxis declarativa de eliminar LinkButton s similar:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

¡Y ya está todo s! Figura 3 muestra una captura de pantalla de esta confirmación en acción. Al hacer clic en el botón Eliminar, se abrirá el cuadro de diálogo de confirmación. Si el usuario hace clic en Cancelar, se cancela la devolución de datos y no se elimina el producto. Si, sin embargo, el usuario hace clic en Aceptar, continúa la devolución de datos y la s ObjectDataSource `Delete()` se invoca el método, y se culmina en el registro de base de datos que se va a eliminar.

> [!NOTE]
> La cadena pasó la `confirm(string)` función de JavaScript está delimitada por apóstrofos (en lugar de las comillas). En JavaScript, las cadenas se pueden delimitar con cualquier carácter. Usamos apóstrofos aquí para que los delimitadores de la cadena pasan `confirm(string)` no introducen una ambigüedad con los delimitadores que se utilizan para la `OnClientClick` valor de propiedad.


[![Una confirmación es ahora muestra al hacer clic en el botón Eliminar.](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figura 3**: una confirmación es ahora muestra al hacer clic en el botón Eliminar ([haga clic aquí para ver imagen en tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Paso 3: Configuración de la propiedad OnClientClick del botón de eliminación en un CommandField

Cuando se trabaja con un botón, LinkButton o ImageButton directamente en una plantilla, se puede asociar con ella un cuadro de diálogo de confirmación simplemente configurando su `OnClientClick` propiedad para devolver los resultados de JavaScript `confirm(string)` función. Sin embargo, no tiene el CommandField, que agrega un campo de los botones eliminar a un control GridView o DetailsView - un `OnClientClick` propiedad que se puede establecer mediante declaración. En su lugar, nos debemos hacer referencia mediante programación el botón Eliminar en el control GridView o DetailsView s adecuado `DataBound` controlador de eventos y, después, establezca su `OnClientClick` propiedad no existe.

> [!NOTE]
> Al establecer el botón Eliminar s `OnClientClick` propiedad en los correspondientes `DataBound` controlador de eventos, tenemos acceso a los datos se enlazan con el registro actual. Esto significa que ahora podemos ampliar el mensaje de confirmación para incluir detalles sobre el registro determinado, como por ejemplo, "¿está seguro que quiere eliminar el producto Chai?" Dicha personalización también es posible en las plantillas mediante la sintaxis de enlace de datos.


Para practicar la configuración de la `OnClientClick` propiedad del botón de eliminación en un CommandField, s permiten agregar un control GridView a la página. Configure este GridView para usar el mismo control ObjectDataSource que usa FormView. También limitan la s GridView BoundFields para incluir solo el nombre del producto, la categoría y el proveedor. Por último, active la casilla Habilitar eliminación de la etiqueta inteligente de s GridView. Esto agregará un CommandField al s GridView `Columns` colección con su `ShowDeleteButton` propiedad establecida en `true`.

Después de realizar estos cambios, el marcado declarativo de GridView s debe ser similar al siguiente:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

El CommandField contiene una sola instancia de eliminar LinkButton que puede tener acceso mediante programación desde la s GridView `RowDataBound` controlador de eventos. Una vez que se hace referencia a, podemos establecer su `OnClientClick` propiedad según corresponda. Crear un controlador de eventos para el `RowDataBound` eventos utilizando el código siguiente:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Este controlador de eventos funciona con las filas de datos (los que tendrá el botón Eliminar) y comienza haciendo mediante programación el botón Eliminar. En general, utilice el siguiente patrón:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* es el tipo de botón que usa el CommandField - Button, LinkButton o ImageButton. De forma predeterminada, el CommandField usa LinkButtons, pero esto se puede personalizar mediante la s CommandField `ButtonType property`. El *commandFieldIndex* es el índice ordinal de la CommandField dentro de las operaciones de asignación GridView `Columns` colección, mientras que el *controlIndex* es el índice del botón Delete dentro de las operaciones de asignación CommandField `Controls` colección. El *controlIndex* valor depende de la posición del botón s en relación con otros botones en la CommandField. Por ejemplo, si el botón solo aparece en el CommandField es el botón Eliminar, utilice un índice de 0. Si, sin embargo, hay un botón Editar que precede el botón Eliminar, usar un índice de 2. La razón se usa un índice de 2 es que dos controles se agregan mediante el CommandField delante del botón Delete: el botón Editar y así un LiteralControl s utilizado para agregar algo de espacio entre los botones Editar y eliminar.

En nuestro ejemplo concreto, el CommandField usa LinkButtons y, el campo más a la izquierda, tiene un *commandFieldIndex* 0. Puesto que no hay ningún otro botón, pero el botón Eliminar en el CommandField, usamos un *controlIndex* 0.

Después de hacer referencia al botón Eliminar en el CommandField, a continuación obtenemos información sobre el producto enlazado a la fila actual de GridView. Por último, establecemos el botón Eliminar s `OnClientClick` propiedad en el código de JavaScript adecuado, que incluye el nombre del producto s. Puesto que la cadena de JavaScript pasa a la `confirm(string)` función está delimitada con apóstrofes nos debemos escape que aparecen en el nombre del producto s apóstrofos. En concreto, el nombre del producto s apóstrofos usan secuencias de escape con "`\'`".

Con estos cambios completa, al hacer clic en un botón Eliminar GridView muestra un cuadro de diálogo de confirmación personalizado cuadro (consulte la figura 4). Como con el cuadro de mensajes de confirmación de FormView, si el usuario hace clic en Cancelar la devolución de datos se cancela, lo que impide la eliminación se produzca.

> [!NOTE]
> Esta técnica también puede usarse para acceder mediante programación en el CommandField en DetailsView el botón Eliminar. Para DetailsView, pero d. crear un controlador de eventos para el `DataBound` eventos, ya que no tiene DetailsView un `RowDataBound` eventos.


[![Al hacer clic en el botón de eliminar s GridView muestra un cuadro de diálogo de confirmación personalizado](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figura 4**: al hacer clic en el botón Eliminar de GridView s muestra un cuadro de diálogo de confirmación personalizado ([haga clic aquí para ver imagen en tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Uso de TemplateFields

Una de las desventajas de la CommandField es que deben tener acceso a sus botones mediante el índice y que el objeto resultante debe convertirse al tipo adecuado de botón (Button, LinkButton o ImageButton). Usar "números mágicos" y tipos rígida invita a los problemas que no se puede detectar hasta el tiempo de ejecución. Por ejemplo, si usted o a otro desarrollador, agrega nuevos botones a la CommandField en algún momento en el futuro (por ejemplo, un botón de edición) o los cambios del `ButtonType` propiedad, el código existente seguirá compilándose sin errores, pero visitar la página puede provocar una excepción o un comportamiento inesperado, dependiendo de cómo se escribe el código y los cambios realizados.

Un enfoque alternativo es convertir la s GridView y DetailsView CommandFields TemplateFields. Esto generará un TemplateField con un `ItemTemplate` que tiene un control LinkButton (o botón o ImageButton) para cada botón en la CommandField. Estos botones `OnClientClick` propiedades se pueden asignar mediante declaración, como se vio con FormView, o puede tener acceso mediante programación en los correspondientes `DataBound` controlador de eventos mediante el siguiente patrón:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Donde *controlID* es el valor del botón s `ID` propiedad. Mientras este patrón todavía requiere un tipo codificado de forma rígida para la conversión, elimina la necesidad para la indexación, lo que permite el diseño cambiar sin que lo que produce un error en tiempo de ejecución.

## <a name="summary"></a>Resumen

El código JavaScript `confirm(string)` función es una técnica frecuente para controlar flujo de trabajo de envío de formulario. Cuando se ejecuta, la función muestra un cuadro de diálogo modal, del lado cliente que incluye dos botones, Aceptar y Cancelar. Si el usuario hace clic en Aceptar, el `confirm(string)` función devuelve `true`; al hacer clic en Cancelar devuelve `false`. Esta funcionalidad, junto con un comportamiento del explorador s para cancelar el envío de un formulario si devuelve un controlador de eventos durante el proceso de envío `false`, se puede usar para mostrar un cuadro de mensajes de confirmación al eliminar un registro.

El `confirm(string)` función puede asociarse con un botón Web control s del lado cliente `onclick` controlador de eventos a través del control s `OnClientClick` propiedad. Cuando se trabaja con un botón Eliminar en una plantilla: en una de las plantillas de FormView s o en TemplateField en el DetailsView o GridView - esta propiedad puede establecerse mediante declaración o mediante programación, en como hemos visto en este tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-cs.md)
> [Siguiente](limiting-data-modification-functionality-based-on-the-user-cs.md)
