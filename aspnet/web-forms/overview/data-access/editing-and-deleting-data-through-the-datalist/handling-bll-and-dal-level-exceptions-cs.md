---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Control de excepciones de nivel BLL y DAL (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo controlar las excepciones producidas durante el flujo de trabajo actualización de un DataList editable de manera discreta.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: ad48b5d2dcb9748e6c2b5a08a9f09f594b9c8374
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368277"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>Control de excepciones de nivel BLL y DAL (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) o [descargar PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> En este tutorial, veremos cómo controlar las excepciones producidas durante el flujo de trabajo actualización de un DataList editable de manera discreta.


## <a name="introduction"></a>Introducción

En el [información general de la edición y eliminación de datos en el control DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) tutorial, creamos un control DataList que ofrecen simple de edición y eliminación de recursos. Aunque es totalmente funcional, estaba apenas fáciles de usar, como cualquier error producido durante la edición o eliminar proceso dieron lugar a una excepción no controlada. Por ejemplo, si se omite el nombre del producto s o, al editar un producto, escriba un valor del precio de muy asequible!, produce una excepción. Puesto que no se detecta esta excepción en el código, propaga hasta el tiempo de ejecución ASP.NET, que, a continuación, muestra los detalles de excepción s en la página web.

Como hemos visto en el [BLL - control y excepciones de nivel DAL en una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial, si se produce una excepción desde las profundidades de la lógica de negocios o los niveles de acceso a datos, los detalles de la excepción se devuelven a ObjectDataSource y a continuación, en GridView. Hemos visto cómo controlar correctamente estas excepciones mediante la creación de `Updated` o `RowUpdated` controladores de eventos para el origen ObjectDataSource o GridView, comprobar si hay una excepción y, a continuación, que indica que se ha controlado la excepción.

Nuestros DataList tutoriales, sin embargo, t a través de ObjectDataSource para actualizar y eliminar datos. En su lugar, estamos trabajando directamente en la capa BLL. Con el fin de detectar las excepciones que se origina en el nivel de lógica empresarial o DAL, debemos implementar código en el código subyacente de nuestra página ASP.NET de control de excepciones. En este tutorial, veremos cómo controlar las excepciones iniciadas durante una s DataList editable actualizar el flujo de trabajo de manera más discreta.

> [!NOTE]
> En el *una visión general de edición y eliminación de datos en el control DataList* tutorial analizamos las distintas técnicas para editar y eliminar datos desde el control DataList, algunas técnicas conlleva el uso de un origen ObjectDataSource para actualizar y Eliminando. Si emplea estas técnicas, puede controlar las excepciones de la capa BLL o DAL a través de las operaciones de asignación ObjectDataSource `Updated` o `Deleted` controladores de eventos.


## <a name="step-1-creating-an-editable-datalist"></a>Paso 1: Creación de un control DataList Editable

Antes de que nos preocupamos controlar las excepciones que se producen durante el flujo de trabajo de actualización, permiten s en primer lugar cree a un DataList editable. Abrir el `ErrorHandling.aspx` página en el `EditDeleteDataList` carpeta, agregue un control DataList hasta el diseñador, establece su `ID` propiedad a `Products`, y agregue un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase s `GetProducts()` registra el método para seleccionar; establece las listas desplegables en la instrucción INSERT, UPDATE y eliminar pestañas en (None).


[![Devolver la información de producto mediante el método GetProducts()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Figura 1**: devolver la información de producto mediante la `GetProducts()` método ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


Después de completar el ObjectDataSource wizard, Visual Studio creará automáticamente un `ItemTemplate` para el control DataList. Reemplácelo con un `ItemTemplate` que muestra cada nombre de producto s y el precio y se incluye un botón de edición. A continuación, cree un `EditItemTemplate` con un control de cuadro de texto Web para el nombre y el precio y botones Actualizar y Cancelar. Por último, establezca el control DataList s `RepeatColumns` propiedad en 2.

Después de estos cambios, el marcado declarativo s de página debe ser similar al siguiente. Vuelva a comprobar para la edición, Cancelar, asegúrese de que y los botones de actualización tienen sus `CommandName` establecer propiedades para editar, Cancelar y actualizar, respectivamente.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> En este tutorial, el control DataList debe habilitarse el estado de vista s.


Dedique unos minutos a ver nuestro progreso a través de un explorador (consulte la figura 2).


[![Cada producto incluye un botón Editar](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Figura 2**: cada producto incluye un botón Editar ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


Actualmente, el botón Editar solo produce un postback, t aún hacen que el producto editable. Para habilitar la edición, necesitamos crear controladores de eventos para el control DataList s `EditCommand`, `CancelCommand`, y `UpdateCommand` eventos. El `EditCommand` y `CancelCommand` eventos solo tiene que actualizar el control DataList s `EditItemIndex` propiedad y volver a vincular los datos para el control DataList:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

El `UpdateCommand` controlador de eventos es un poco más complicado. Tiene que leerse en el producto editado s `ProductID` desde el `DataKeys` colección junto con el nombre de producto s y el precio de los cuadros de texto en el `EditItemTemplate`y, a continuación, llame a la `ProductsBLL` clase s `UpdateProduct` antes de devolver el control DataList (método) a su estado de edición previamente.

Por ahora, s permiten usar simplemente exactamente el mismo código de la `UpdateCommand` controlador de eventos en el *información general de la edición y eliminación de datos en el control DataList* tutorial. Vamos a agregar el código para controlar correctamente las excepciones en el paso 2.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

En caso de entrada no válida que puede ser en forma de un precio por unidad con formato incorrecto, un valor del precio de unidad no válida, como-$5.00 o la omisión del nombre de s del producto, se generará una excepción. Puesto que el `UpdateCommand` controlador de eventos no incluye ninguna en este momento, el código de control de excepciones, la excepción que se elevará hasta el tiempo de ejecución ASP.NET, donde se mostrarán al usuario final (consulte la figura 3).


![Cuando se produce una excepción no controlada, el usuario final ve una página de Error](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Figura 3**: cuando se produce una excepción no controlada, el usuario final ve una página de Error


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Paso 2: Administrar correctamente las excepciones en el controlador de eventos UpdateCommand

Durante el flujo de trabajo de actualización, pueden producirse excepciones en el `UpdateCommand` la capa DAL, BLL o controlador de eventos. Por ejemplo, si un usuario escribe un precio de demasiadas costoso, el `Decimal.Parse` instrucción en el `UpdateCommand` controlador de eventos se iniciará un `FormatException` excepción. Si el usuario la omita el nombre del producto s o si el precio tiene un valor negativo, la capa DAL, producirá una excepción.

Cuando se produce una excepción, queremos mostrar un mensaje informativo en la propia página. Agregar un sitio Web de la etiqueta de control a la página cuya `ID` está establecido en `ExceptionDetails`. Configurar el texto del rótulo s para mostrar en una color rojo, muy grande, la fuente en negrita y cursiva mediante la asignación de su `CssClass` propiedad a la `Warning` clase CSS que se define en el `Styles.css` archivo.

Cuando se produce un error, solamente queremos la etiqueta que se mostrará una vez. Es decir, en los postbacks subsiguientes, el mensaje de advertencia de etiqueta s debería desaparecer. Esto puede realizarse borrando la etiqueta s `Text` propiedad o la configuración de su `Visible` propiedad `False` en el `Page_Load` controlador de eventos (como hicimos en el [BLL - control y excepciones de nivel DAL en ASP Página .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial) o si se deshabilita la compatibilidad de estado de vista de la etiqueta s. Permitir que s use la última opción.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Cuando se produce una excepción, asignaremos los detalles de la excepción para el `ExceptionDetails` control s etiqueta `Text` propiedad. Puesto que su estado de vista está deshabilitado, en los postbacks subsiguientes del `Text` cambios mediante programación la propiedad s se perderán, vuelve a establecerse en el texto predeterminado (una cadena vacía), ocultando de esta manera el mensaje de advertencia.

Para determinar cuándo se han se produce un error con el fin de mostrar un mensaje útil en la página, necesitamos agregar un `Try ... Catch` bloquear a los `UpdateCommand` controlador de eventos. El `Try` parte contiene el código que puede provocar una excepción, mientras que el `Catch` bloque contiene código que se ejecuta en el caso de una excepción. Consulte la [Fundamentos del control de excepciones](https://msdn.microsoft.com/library/2w8f0bss.aspx) sección en la documentación de .NET Framework para obtener más información sobre la `Try ... Catch` bloque.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Cuando se produce una excepción de cualquier tipo de código dentro de la `Try` bloque, el `Catch` código del bloque s inicia la ejecución. El tipo de excepción que se produce `DbException`, `NoNullAllowedException`, `ArgumentException`y así sucesivamente depende de qué, exactamente, en primer lugar a provocado el error. Si hay un problema de s en el nivel de base de datos, un `DbException` se iniciará. Si se especifica un valor no válido para el `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` campos, un `ArgumentException` se iniciará, tal como se ha agregado código para validar los valores de estos campos en el `ProductsDataTable` clase (vea la [ Crear una capa de lógica empresarial](../introduction/creating-a-business-logic-layer-cs.md) tutorial).

Podemos proporcionar una explicación más útil para el usuario final al basar el texto del mensaje en el tipo de excepción detectada. El siguiente código que se usó de forma casi idéntica en el [BLL - control y excepciones de nivel DAL en una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial proporciona este nivel de detalle:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Para completar este tutorial, simplemente llame a la `DisplayExceptionDetails` método desde el `Catch` bloque pasando detectado `Exception` instancia (`ex`).

Con el `Try ... Catch` bloquear en su lugar, los usuarios recibirán un mensaje de error más informativo, como las figuras 4 y 5 show. Tenga en cuenta que, en caso de una excepción, el control DataList, permanece en modo de edición. Esto es porque una vez que se produce la excepción, el flujo de control se redirigirá inmediatamente a la `Catch` bloque, omitiendo el código que devuelve el control DataList a su estado de edición previamente.


[![Se muestra un mensaje de Error si un usuario la omita un campo necesario](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Figura 4**: se muestra un mensaje de Error si un usuario la omita los campos opcionales ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Es de un mensaje de Error aparece cuando escribe un precio negativo](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Figura 5**: es de un mensaje de Error aparece cuando escribe un precio negativo ([haga clic aquí para ver imagen en tamaño completo](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Resumen

El control GridView y ObjectDataSource proporcionan controladores de eventos posteriores al nivel que incluyen información sobre las excepciones que se han producido durante el flujo de trabajo de actualización y eliminación, así como las propiedades que se pueden establecer para indicar si la excepción se ha controla. Estas características, sin embargo, no están disponibles al trabajar con el control DataList y uso de la capa BLL directamente. En su lugar, somos responsables de implementar el control de excepciones.

En este tutorial hemos visto cómo agregar el control de excepciones para una s DataList editable actualizar el flujo de trabajo mediante la adición de un `Try ... Catch` bloquear a los `UpdateCommand` controlador de eventos. Si se produce una excepción durante el flujo de trabajo de actualización, el `Catch` ejecuta código del bloque s, mostrar información útil en la `ExceptionDetails` etiqueta.

En este momento, el control DataList realiza ningún esfuerzo para evitar excepciones tengan lugar en primer lugar. Aunque se sabe que un precio negativo se producirá una excepción, conocemos t ha agregado funcionalidad para evitar de forma proactiva un usuario de escribir una entrada no válida. En nuestro siguiente tutorial veremos cómo ayudar a reducir las excepciones causadas por la entrada de usuario no válido mediante la adición de controles de validación en el `EditItemTemplate`.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Instrucciones de diseño de excepciones](https://msdn.microsoft.com/library/ms298399.aspx)
- [Módulos de registro de errores y controladores (ELMAH)](http://workspaces.gotdotnet.com/elmah) (una biblioteca de código abierto para el registro de errores)
- [Enterprise Library para .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (incluye Exception Management Application Block)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Ken Pespisa. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](performing-batch-updates-cs.md)
> [Siguiente](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
