---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Control de excepciones de nivel de DAL y BLL (VB) | Documentos de Microsoft
author: rick-anderson
description: "En este tutorial, veremos cómo controlar las excepciones generadas durante el flujo de trabajo de actualización de un DataList editable de manera discreta."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 245e381eca2aca61be5f860d1ec9994b482a9863
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Control de excepciones de nivel de DAL y BLL (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) o [descarga de PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> En este tutorial, veremos cómo controlar las excepciones generadas durante el flujo de trabajo de actualización de un DataList editable de manera discreta.


## <a name="introduction"></a>Introducción

En el [información general de edición y eliminación de datos en el control DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) tutorial, creamos un DataList que ofrecen simple de edición y eliminación de las capacidades. Aunque es totalmente funcional, es muy fácil de usar, como cualquier error que se produjo durante la edición o eliminar proceso generó una excepción no controlada. Por ejemplo, si se omite el nombre del producto s o, al editar un producto, escriba un valor del precio de muy asequible!, produce una excepción. Puesto que no se detecta esta excepción en el código, propaga hasta el tiempo de ejecución ASP.NET, que, a continuación, muestra los detalles de excepción s en la página web.

Como vimos en el [BLL - control y las excepciones de nivel de la capa DAL de una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial, si se produce una excepción desde las profundidades de la lógica de negocios o los niveles de acceso a datos, los detalles de la excepción se devuelven a ObjectDataSource y a continuación, en GridView. Hemos visto cómo controlar estas excepciones correctamente mediante la creación de `Updated` o `RowUpdated` controladores de eventos para el ObjectDataSource o GridView, comprobar si una excepción y, a continuación, que indica que se ha controlado la excepción.

Nuestros DataList tutoriales, sin embargo, t a través de ObjectDataSource para actualizar y eliminar datos. En su lugar, estamos trabajando directamente en la capa BLL. Con el fin de detectar las excepciones que se origina en el BLL o la capa DAL, es necesario implementar el código en el código subyacente de la página ASP.NET de control de excepciones. En este tutorial, veremos cómo controlar las excepciones producidas durante una s DataList editable actualizar el flujo de trabajo de manera más discreta.

> [!NOTE]
> En el *una visión general de edición y eliminación de datos en el control DataList* tutorial analizamos técnicas diferentes para modificar y eliminar datos de DataList, algunas técnicas conlleva el uso de un origen ObjectDataSource para actualizar y eliminar. Si utiliza estas técnicas, puede controlar las excepciones de la capa BLL o la capa DAL a través de las operaciones de asignación ObjectDataSource `Updated` o `Deleted` controladores de eventos.


## <a name="step-1-creating-an-editable-datalist"></a>Paso 1: Crear a un DataList Editable

Antes de que nos preocupamos sobre el control de excepciones que se producen durante el flujo de trabajo de actualización, permiten s crear primero un DataList editable. Abrir la `ErrorHandling.aspx` página en el `EditDeleteDataList` carpeta, agregar un control DataList hasta el diseñador, establezca su `ID` propiedad a `Products`, y agregue un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para usar el `ProductsBLL` clase s. `GetProducts()` registra el método para la selección; establece las listas de lista desplegable en la instrucción INSERT, UPDATE y eliminar las fichas (ninguno).


[![Devolver la información de producto mediante el método GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: devolver la información de producto mediante el `GetProducts()` método ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Después de completar el Asistente de ObjectDataSource, Visual Studio creará automáticamente un `ItemTemplate` para el control DataList. Reemplácelo con un `ItemTemplate` que muestra cada nombre de producto s y precio e incluye un botón Editar. A continuación, cree una `EditItemTemplate` con un control de cuadro de texto Web para el nombre y el precio y botones Actualizar y Cancelar. Por último, establezca la DataList s `RepeatColumns` propiedad en 2.

Después de estos cambios, el marcado declarativo de s de página debe ser similar al siguiente. Asegúrese de que la edición, Cancelar, y los botones de actualización tienen sus `CommandName` establecer propiedades para editar, Cancelar y actualizar, respectivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Para este tutorial, el control DataList debe estar habilitado el estado de vista de s.


Tómese un momento para ver el progreso a través de un explorador (consulte la figura 2).


[![Cada producto incluye un botón Editar](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: cada producto incluye un botón Editar ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Actualmente, el botón Editar sólo provoca una devolución de datos, t aún hacen que el producto editable. Para habilitar la edición, necesitamos crear controladores de eventos para el control DataList s `EditCommand`, `CancelCommand`, y `UpdateCommand` eventos. El `EditCommand` y `CancelCommand` eventos simplemente actualización DataList s `EditItemIndex` propiedad y vuelva a enlazar los datos al control DataList:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

El `UpdateCommand` controlador de eventos es un poco más complicada. Tiene que leerse en el producto editado s `ProductID` desde el `DataKeys` colección junto con el nombre de producto s y el precio de los cuadros de texto en el `EditItemTemplate`y, a continuación, llame a la `ProductsBLL` clase s. `UpdateProduct` método antes de devolver el control DataList a su estado anterior de edición.

Por ahora, s permiten usar simplemente el mismo código exacto de la `UpdateCommand` controlador de eventos en el *información general de edición y eliminación de datos en el control DataList* tutorial. Vamos a agregar el código para controlar correctamente las excepciones en el paso 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

En el caso de entrada no válida que puede ser en forma de un precio por unidad con formato incorrecto, un valor del precio unitario no válido como - 5,00 USD o la omisión del nombre del producto de s que se producirá una excepción. Puesto que la `UpdateCommand` controlador de eventos no incluye ninguna en este momento, el código de control de excepciones, la excepción se llegan hasta el tiempo de ejecución ASP.NET, donde se mostrará al usuario final (consulte la figura 3).


![Cuando se produce una excepción no controlada, el usuario final ve una página de Error](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: cuando se produce una excepción no controlada, el usuario final ve una página de Error


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Paso 2: Administrar correctamente las excepciones en el controlador de eventos de UpdateCommand

Durante el flujo de trabajo de actualización, las excepciones pueden producirse en el `UpdateCommand` controlador de eventos, la capa BLL o la capa DAL. Por ejemplo, si un usuario escribe un precio de demasiados costosas, la `Decimal.Parse` instrucción en el `UpdateCommand` controlador de eventos, se producirá un `FormatException` excepción. Si el usuario la omita el nombre del producto s o si el precio tiene un valor negativo, la capa DAL producirá una excepción.

Cuando se produce una excepción, deseamos mostrar un mensaje informativo en la propia página. Agregar un sitio Web de la etiqueta de control a la página cuya `ID` está establecido en `ExceptionDetails`. Configurar el texto del rótulo s para mostrar en una red, muy grande, fuente negrita y cursiva asignando su `CssClass` propiedad a la `Warning` clase CSS, que se define en el `Styles.css` archivo.

Cuando se produce un error, solamente queremos la etiqueta que se mostrará una vez. Es decir, en postbacks posteriores, el mensaje de advertencia de etiqueta s debería desaparecer. Esto puede realizarse borrando la etiqueta s `Text` propiedad o configuración de su `Visible` propiedad `False` en el `Page_Load` controlador de eventos (como hicimos en el [BLL - control y las excepciones de nivel de la capa DAL en un ASP Página .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial) o al deshabilitar la compatibilidad de estado de vista de etiqueta s. Permitir que s use esta última opción.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Cuando se produce una excepción, asignaremos los detalles de la excepción para la `ExceptionDetails` control s etiqueta `Text` propiedad. Puesto que su estado de vista está deshabilitado, en postbacks posteriores el `Text` propiedad s mediante programación cambios se perderán, vuelve a establecerse en el texto predeterminado (una cadena vacía), ocultando de esta manera el mensaje de advertencia.

Para determinar cuándo se ha producido un error con el fin de mostrar un mensaje útil en la página, tenemos que agregar una `Try ... Catch` bloquear para el `UpdateCommand` controlador de eventos. El `Try` parte contiene código que puede provocar una excepción, mientras que la `Catch` bloque contiene código que se ejecuta en el caso de una excepción. Extraer del repositorio el [Fundamentos del control de excepciones](https://msdn.microsoft.com/en-us/library/2w8f0bss.aspx) sección en la documentación de .NET Framework para obtener más información sobre la `Try ... Catch` bloque.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Cuando se produce una excepción de cualquier tipo de código dentro de la `Try` bloque, la `Catch` código del bloque s iniciará la ejecución. El tipo de excepción que se produce `DbException`, `NoNullAllowedException`, `ArgumentException`y así sucesivamente dependerá de qué, exactamente, provocado el error en primer lugar. Si hay un problema de s en el nivel de base de datos, un `DbException` se iniciará. Si se especifica un valor no válido para la `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` campos, un `ArgumentException` se producirá, tal y como se agrega código para validar estos valores de campo en el `ProductsDataTable` clase (vea la [ Creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-vb.md) tutorial).

Podemos proporcionar una explicación más relevantes para el usuario final al basar el texto del mensaje en el tipo de excepción detectada. El código siguiente que se usó de forma casi idéntica en el [BLL - control y las excepciones de nivel de la capa DAL de una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial proporciona este nivel de detalle:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Para completar este tutorial, basta con llamar a la `DisplayExceptionDetails` método desde el `Catch` bloque pasando detectado `Exception` instancia (`ex`).

Con el `Try ... Catch` bloquear en su lugar, los usuarios recibirán un mensaje de error más informativo, como las figuras 4 y 5 mostrar. Tenga en cuenta que en el caso de una excepción DataList permanece en modo de edición. Esto es porque una vez que se produce la excepción, el flujo de control se redirigirá inmediatamente a la `Catch` bloque, omitiendo el código que devuelve el control DataList a su estado anterior de edición.


[![Se muestra un mensaje de Error si un usuario la omita un campo necesario](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: aparece un mensaje de Error si un usuario la omita un campo necesario ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Es de un mensaje de Error aparece cuando especifica un precio negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: un mensaje de Error es aparece cuando especifica un precio negativo ([haga clic aquí para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Resumen

El control GridView y ObjectDataSource proporcionan controladores de eventos posterior al nivel que incluyen información sobre las excepciones que se produjeron durante el flujo de trabajo de actualización y eliminación, así como propiedades que se pueden establecer para indicar si la excepción se ha administra. Estas características, sin embargo, no están disponibles al trabajar con el control DataList y con la capa BLL directamente. En su lugar, somos responsables de implementar el control de excepciones.

En este tutorial se ha explicado cómo agregar el control de excepciones para una s DataList editable actualiza el flujo de trabajo mediante la adición de un `Try ... Catch` bloquear para el `UpdateCommand` controlador de eventos. Si se produce una excepción durante el flujo de trabajo de actualización, el `Catch` ejecuta código del bloque s, mostrar información útil en la `ExceptionDetails` etiqueta.

En este punto, el control DataList realiza ningún esfuerzo para evitar excepciones suceda en primer lugar. Aunque se sabe que un precio negativo se producirá una excepción, conocemos t ha agregado funcionalidad para evitar de forma proactiva un usuario entraran en estas entradas no válidas. En el siguiente tutorial veremos cómo ayudar a reducir las excepciones producidas por proporcionados por el usuario no válido mediante la adición de controles de validación en el `EditItemTemplate`.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Instrucciones de diseño para excepciones](https://msdn.microsoft.com/en-us/library/ms298399.aspx)
- [Módulos de registro de error y controladores (ELMAH)](http://workspaces.gotdotnet.com/elmah) (una biblioteca de código abierto para el registro de errores)
- [Enterprise Library para .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (incluye el bloque de aplicaciones de administración de excepciones)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Ken Pespisa. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](performing-batch-updates-vb.md)
[Siguiente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
