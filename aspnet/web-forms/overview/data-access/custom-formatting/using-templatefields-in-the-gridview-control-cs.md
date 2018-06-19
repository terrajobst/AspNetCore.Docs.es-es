---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Uso de TemplateFields en el Control GridView (C#) | Documentos de Microsoft
author: rick-anderson
description: Para proporcionar flexibilidad, GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles Web, y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6485cbda50912920808fc0caf41c888493f210dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877881"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Uso de TemplateFields en el Control GridView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) o [descarga de PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Para proporcionar flexibilidad, GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles y sintaxis de enlace de datos Web. En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView.


## <a name="introduction"></a>Introducción

GridView se compone de un conjunto de campos que indican qué propiedades de la `DataSource` deben incluirse en la salida representada junto con cómo se mostrarán los datos. El tipo de campo más sencillo es el BoundField, que muestra un valor de datos como texto. Otros tipos de campo muestran los datos mediante los elementos HTML alternativos. CampoCasillaVerificación, por ejemplo, se representa como una casilla de verificación cuyo estado activado depende del valor de un campo de datos especificado; el ImageField presenta una imagen cuyo origen de la imagen se basa en un campo de datos especificado. Los hipervínculos y los botones cuyo estado depende de un valor de campo de datos subyacente se pueden representar mediante los tipos de campo campo Hyperlink y ButtonField.

Aunque los tipos de campo CampoCasillaVerificación, ImageField, campo Hyperlink y ButtonField permiten una vista alternativa de los datos, todavía son bastante limitados con respecto al formato. Un CampoCasillaVerificación solo puede mostrar una casilla, mientras que un ImageField solo puede mostrar una imagen. ¿Qué ocurre si se necesita un campo concreto para mostrar texto, una casilla de verificación, *y* una imagen, todas basada en valores de campo de datos diferente? O bien, ¿qué ocurre si deseamos mostrar los datos mediante un control Web que no sea la casilla de verificación, una imagen, un hipervínculo o un botón? Además, el BoundField limita su presentación en un único campo de datos. ¿Qué ocurre si deseamos mostrar dos o más valores de campo de datos en una sola columna de GridView?

Para dar cabida a este nivel de flexibilidad GridView ofrece TemplateField, que se representa con un *plantilla*. Una plantilla puede incluir una combinación de HTML estático, controles y sintaxis de enlace de datos Web. Además, el TemplateField tiene una variedad de plantillas que puede usarse para personalizar la representación en distintas situaciones. Por ejemplo, el `ItemTemplate` se usa de forma predeterminada para representar la celda para cada fila, pero la `EditItemTemplate` plantilla se puede usar para personalizar la interfaz durante la edición de datos.

En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView. En el [tutorial anterior](custom-formatting-based-upon-data-cs.md) hemos visto cómo personalizar el formato en función de los datos subyacentes mediante la `DataBound` y `RowDataBound` controladores de eventos. Otra manera de personalizar el formato en función de los datos subyacentes se mediante una llamada a métodos de formato desde dentro de una plantilla. Iremos viendo esta técnica en este tutorial también.

Para este tutorial, usaremos TemplateFields para personalizar la apariencia de una lista de empleados. En concreto, se podrá mostrar todos los empleados, sino que mostrará el empleado nombres y apellidos en una columna, su fecha de contratación de un control de calendario y una columna de estado que indica el número de días hayan se haya utilizado en la empresa.


[![Tres TemplateFields sirven para personalizar la presentación](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figura 1**: tres TemplateFields sirven para personalizar la presentación ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Paso 1: Enlace de datos en GridView

En escenarios donde es necesario utilizar TemplateFields para personalizar la apariencia de los informes, resultarle más fácil empezar creando un control GridView que contiene solo BoundFields primero y, a continuación, agregar nuevos TemplateFields o convertir el BoundFields existente para TemplateFields según sea necesario. Por lo tanto, puede empezar este tutorial agregando un control GridView a la página a través del diseñador y enlazarlo a un origen ObjectDataSource que devuelve la lista de empleados. Estos pasos crearán un control GridView con BoundFields para cada uno de los campos de employee.

Abra la `GridViewTemplateField.aspx` página y arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Desde la etiqueta inteligente de GridView optar por agregar un nuevo control ObjectDataSource que invoca la `EmployeesBLL` la clase `GetEmployees()` método.


[![Agregar un nuevo Control ObjectDataSource que invoca el método GetEmployees()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo ObjectDataSource Control ese Invokes el `GetEmployees()` método ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Enlazar el control GridView de esta manera se agregará automáticamente una BoundField para cada una de las propiedades de empleado: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, y `Country`. Para este informe vamos a no pierda tiempo mostrar la `EmployeeID`, `ReportsTo`, o `Country` propiedades. Para quitar estos BoundFields hacer lo siguiente:

- Utilice el cuadro de diálogo campos haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView para que aparezca este cuadro de diálogo. A continuación, seleccione la BoundFields desde la parte inferior izquierda y haga clic en la X roja botón para quitar el BoundField.
- Editar la sintaxis declarativa de GridView manualmente desde la vista del origen, elimine la `<asp:BoundField>` (elemento) para el BoundField que se va a quitar.

Después de haber quitado el `EmployeeID`, `ReportsTo`, y `Country` BoundFields, el marcado de la GridView debería ser similar:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Tómese un momento para ver el progreso en un explorador. En este momento debería ver una tabla con un registro de cada empleado y cuatro columnas: uno para el empleado apellido, una para su nombre, uno para su título y otro para su fecha de contratación.


[![El LastName, FirstName, título y los campos HireDate se muestran para cada empleado](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figura 3**: el `LastName`, `FirstName`, `Title`, y `HireDate` campos se muestran para cada empleado ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Paso 2: Mostrar los nombres y apellidos en una sola columna

Actualmente, cada empleado de la primera y última nombres se muestran en una columna independiente. Sería estupendo combinarlas en una sola columna. Para ello es necesario usar TemplateField. Podemos ya sea agregar TemplateField nuevos, agregarle el marcado necesario y la sintaxis de enlace de datos y, a continuación, elimine la `FirstName` y `LastName` BoundFields, o se puede convertir el `FirstName` BoundField en TemplateField, edite el TemplateField para incluir el `LastName` valor y, a continuación, quite el `LastName` BoundField.

Ambos enfoques net el mismo resultado, pero personalmente me gusta convertir BoundFields a TemplateFields siempre que sea posible, ya que la conversión se agrega automáticamente una `ItemTemplate` y `EditItemTemplate` con controles Web y la sintaxis de enlace de datos para imitar la apariencia y la funcionalidad de la BoundField. La ventaja es que necesitamos realizar menos trabajo con TemplateField como el proceso de conversión se habrá realizado algunas de las tareas para que podamos.

Para convertir un BoundField existente en TemplateField, haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView, muestre el cuadro de diálogo de campos. Seleccione el BoundField para convertir en la lista en la esquina inferior izquierda y, a continuación, haga clic en el vínculo "Convertir este campo en TemplateField" en la esquina inferior derecha.


[![Convertir un BoundField TemplateField desde el cuadro de diálogo campos](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figura 4**: convertir una BoundField en TemplateField desde el cuadro de diálogo campos ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Voy a convertir el `FirstName` BoundField en TemplateField. Después de este cambio, no hay ninguna diferencia perceptive en el diseñador. Esto es porque convirtiendo el BoundField en TemplateField crea TemplateField que mantiene la apariencia y funcionamiento de la BoundField. Aunque haya ninguna diferencia visual en este momento en el diseñador, este proceso de conversión ha reemplazado la sintaxis declarativa de BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` : con la siguiente sintaxis TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Como puede ver, TemplateField consta de dos plantillas un `ItemTemplate` que tiene una etiqueta cuyo `Text` propiedad está establecida en el valor de la `FirstName` campo de datos y un `EditItemTemplate` con un cuadro de texto control cuya `Text` también se establece la propiedad para el `FirstName` campo de datos. La sintaxis de enlace de datos - `<%# Bind("fieldName") %>` -indica que el campo de datos *`fieldName`* está enlazado a la propiedad de control Web especificada.

Para agregar el `LastName` valor para este TemplateField tenemos que agregar otro control Web de la etiqueta del campo de datos el `ItemTemplate` y enlazar sus `Text` propiedad `LastName`. Esto puede realizarse manualmente o mediante el diseñador. Para hacerlo manualmente, basta con agregar la sintaxis declarativa adecuada para el `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Para agregarlo a través del diseñador, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView. Se mostrará la interfaz de edición de plantilla de GridView. En esta etiqueta inteligente de la interfaz es una lista de las plantillas en GridView. Dado que solo tenemos un TemplateField en este momento, las plantillas sola aparecen en la lista desplegable son las plantillas para el `FirstName` TemplateField junto con el `EmptyDataTemplate` y `PagerTemplate`. El `EmptyDataTemplate` plantilla, si se especifica, se usa para representar el resultado de GridView si no hay ningún resultado en los datos enlazados a la GridView; el `PagerTemplate`, si se especifica, se usa para representar la interfaz de paginación para un control GridView que admite la paginación.


[![Plantillas de GridView pueden modificarse mediante el diseñador](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figura 5**: puede ser editado a través del Diseñador de GridView el de plantillas de ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Para mostrar la `LastName` en el `FirstName` TemplateField arrastrar el control de etiqueta desde el cuadro de herramientas en el `FirstName` del TemplateField `ItemTemplate` en GridView de la edición de plantillas (interfaz).


[![Agregar un Control Web Label a ItemTemplate de FirstName TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figura 6**: agregar un Control Label Web para la `FirstName` ItemTemplate del TemplateField ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


En este momento tiene el control de etiqueta Web agregado a la TemplateField su `Text` propiedad establecida en "Etiqueta". Es necesario cambiar esto para que esta propiedad se enlaza al valor de la `LastName` del campo de datos en su lugar. Para lograr este haga clic en la etiqueta inteligente de la etiqueta del control y elija la opción de Editar DataBindings.


[![Elija la opción de Editar DataBindings de etiqueta inteligente de la etiqueta](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figura 7**: elija la opción edición de DataBindings de etiqueta inteligente de la etiqueta ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Se abrirá el cuadro de diálogo DataBindings. Desde aquí puede seleccionar la propiedad para participar en el enlace de datos en la lista de la izquierda y elija el campo para enlazar los datos de la lista desplegable de la derecha. Elija la `Text` propiedad desde la izquierda y el `LastName` del campo de la derecha y haga clic en Aceptar.


[![Enlazar la propiedad de texto al campo de datos LastName](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figura 8**: enlazar el `Text` propiedad a la `LastName` campo de datos ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> El cuadro de diálogo DataBindings permite indicar si se debe realizar el enlace de datos bidireccional. Si se deja desactivada, la sintaxis de enlace de datos `<%# Eval("LastName")%>` se usará en lugar de `<%# Bind("LastName")%>`. Cualquier enfoque es correcto para este tutorial. Enlace de datos bidireccional es importante al insertar y modificar datos. Para mostrar simplemente los datos, sin embargo, cualquier enfoque funciona igualmente bien. Analizaremos en detalle el enlace de datos bidireccional en tutoriales futuros.


Tómese un momento para ver esta página a través de un explorador. Como puede ver, GridView todavía incluye cuatro columnas; Sin embargo, el `FirstName` columna muestra ahora *ambos* el `FirstName` y `LastName` valores de campo de datos.


[![Se muestran los valores de LastName y FirstName en una sola columna](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figura 9**: tanto el `FirstName` y `LastName` valores se muestran en una sola columna ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Para completar este primer paso, quite el `LastName` BoundField y cambiar el nombre de la `FirstName` del TemplateField `HeaderText` propiedad "Name". Después de estos cambios marcado declarativo del GridView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Nombre y apellidos de cada empleado se muestran en una columna](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figura 10**: nombre y apellidos de cada empleado se muestran en una columna ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Paso 3: Usar el Control de calendario que se muestran los`HiredDate`campo

Mostrar un valor de campo de datos como texto en un control GridView es tan simple como utilizar un BoundField. En determinados escenarios, sin embargo, los datos se mejor expresa mediante un control Web determinado en lugar de solo texto. Es posible con TemplateFields dicha personalización de la visualización de datos. Por ejemplo, en lugar de mostrar la fecha de contratación del empleado como texto, podríamos mostramos un calendario (mediante [el control de calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con su fecha de contratación resaltado.

Para ello, inicie convirtiendo el `HiredDate` BoundField en TemplateField. Simplemente vaya a la etiqueta inteligente de GridView y haga clic en el vínculo Editar columnas, muestre el cuadro de diálogo de campos. Seleccione el `HiredDate` BoundField y haga clic en "convertir este campo TemplateField."


[![Convertir el HiredDate BoundField en TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figura 11**: convertir la `HiredDate` BoundField en unas TemplateField ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Como vimos en el paso 2, se reemplazará la BoundField con TemplateField que contiene un `ItemTemplate` y `EditItemTemplate` con una etiqueta y un cuadro de texto cuyo `Text` propiedades se enlazan a la `HiredDate` valor utilizando la sintaxis de enlace de datos `<%# Bind("HiredDate")%>`.

Para reemplazar el texto con un control de calendario, edite la plantilla quitando la etiqueta y agregar un control de calendario. En el diseñador, seleccione Editar plantillas de etiqueta inteligente de GridView y elija la `HireDate` del TemplateField `ItemTemplate` en la lista desplegable. A continuación, elimine el control de etiqueta y arrastre un control de calendario en el cuadro de herramientas en la interfaz de edición de plantilla.


[![Agregar un Control de calendario para el HireDate ItemTemplate del TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figura 12**: agregar un Control de calendario para el `HireDate` del TemplateField `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


En este momento, cada fila de GridView contendrá un control de calendario en su `HiredDate` TemplateField. Sin embargo, el empleado del real `HiredDate` valor no está establecido en cualquier lugar en el control de calendario, haciendo que cada control de calendario en que muestra el mes actual y la fecha de forma predeterminada. Para solucionar esto, debemos asignar cada empleado `HiredDate` para el control de calendario [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) y [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) propiedades.

En las etiquetas inteligentes del control de calendario, elija Editar DataBindings. A continuación, enlazar ambos `SelectedDate` y `VisibleDate` propiedades para el `HiredDate` campo de datos.


[![Enlazar las propiedades de VisibleDate y SelectedDate al campo de datos de HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figura 13**: enlazar el `SelectedDate` y `VisibleDate` propiedades para la `HiredDate` campo de datos ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Fecha seleccionada del control de calendario no debe ser visible necesariamente. Por ejemplo, un calendario puede tener el 1 de agosto<sup>st</sup>, 1999 como la fecha seleccionada, pero puede que muestra el mes actual y el año. La fecha seleccionada y la fecha visible se especifican mediante el control de calendario `SelectedDate` y `VisibleDate` propiedades. Puesto que deseamos a ambos seleccione del empleado `HiredDate` y asegúrese de que se muestra es necesario enlazar dos de estas propiedades en el `HireDate` campo de datos.


Al ver la página en un explorador, el calendario ahora muestra el mes de la fecha de contratación del empleado y selecciona esa fecha en particular.


[![HiredDate del empleado se muestra en el Control de calendario](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figura 14**: el empleado `HiredDate` se muestra en el Control de calendario ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Al contrario que en todos los ejemplos que hemos visto hasta ahora, para este tutorial se realizó *no* establecer `EnableViewState` propiedad `false` para este GridView. La razón para tomar esta decisión es que al hacer clic en las fechas del control Calendar provoca una devolución de datos, establecer la fecha del calendario seleccionada en la fecha que se acaba de hacer clic. Si se deshabilita el estado de vista de GridView, sin embargo, en cada postback datos de GridView se vuelve a enlazar a su origen de datos subyacente, lo que hace que la fecha del calendario seleccionada se establezca *Atrás* del empleado `HireDate`, sobrescritura la fecha seleccionada por el usuario.


Para este tutorial trata una discusión discutible puesto que el usuario no es capaz de actualizar el empleado `HireDate`. Probablemente sería mejor configurar el control de calendario para que sus fechas no son seleccionables. A pesar de todo, en este tutorial se muestra que en algunas circunstancias el estado de vista debe estar habilitado para proporcionar una funcionalidad determinada.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Paso 4: Muestra al número de días que el empleado ha funcionado de la empresa

Hasta ahora hemos visto dos aplicaciones de TemplateFields:

- Combinar dos o más valores de campo de datos en una columna, y
- Expresar un valor de campo de datos mediante un control Web en lugar de texto

Un tercer uso de TemplateFields es mostrar metadatos sobre la GridView datos subyacentes. Además de mostrar las fechas de contratación de los empleados, por ejemplo, nos también convenga tener una columna que muestra cuántos días en total que han estado en el trabajo.

Aún otro uso de TemplateFields surge en escenarios cuando los datos subyacentes deben mostrarse de manera diferente en el informe de página web que en el formato se almacena en la base de datos. Imagine que el `Employees` tabla tenía un `Gender` campo que almacena el carácter `M` o `F` para indicar el sexo del empleado. Cuando se muestra esta información en una página web que tengamos que deseamos mostrar el sexo como "Hombre" o "Mujer", en lugar de simplemente "M" o "F".

Ambos escenarios pueden controlarse mediante la creación de un *método de formato* en la clase de código subyacente de la página ASP.NET (o en una biblioteca de clases independiente, que se implementa como un `static` método) que se invoca desde la plantilla. Este tipo de método formato se invoca desde la plantilla con la misma sintaxis de enlace de datos vista anteriormente. El método de formato puede tardar en cualquier número de parámetros, pero debe devolver una cadena. Esta cadena devuelta es el código HTML que se inserta en la plantilla.

Para ilustrar este concepto, vamos a aumentar nuestro tutorial para mostrar una columna que muestra el número total de días que un empleado ha estado en el trabajo. Este método de formato tardará un `Northwind.EmployeesRow` de objetos y devuelve el número de días que el empleado ha sido contratado como una cadena. Este método puede agregarse a la clase de código subyacente de la página ASP.NET, pero *debe* marcarse como `protected` o `public` para que sea accesible desde la plantilla.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Puesto que la `HiredDate` campo puede contener `NULL` valores primero debemos asegurarnos de que el valor no es la base de datos `NULL` antes de continuar con el cálculo. Si el `HiredDate` valor es `NULL`, devolvemos simplemente la cadena "Unknown"; Si no es `NULL`, se calcula la diferencia entre la hora actual y la `HiredDate` valor y devuelve el número de días.

Para utilizar este método que se debe invocar desde TemplateField en GridView utilizando la sintaxis de enlace de datos. Empiece agregando un nuevo TemplateField a GridView haciendo clic en el vínculo Editar columnas de etiqueta inteligente de GridView y agregar un nuevo TemplateField.


[![Agregar un nuevo TemplateField a GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figura 15**: agregar un nuevo TemplateField a GridView ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Establecer este TemplateField nueva `HeaderText` propiedad en "Días en el trabajo" y su `ItemStyle`del `HorizontalAlign` propiedad `Center`. Para llamar a la `DisplayDaysOnJob` método de la plantilla, agregar un `ItemTemplate` y use la siguiente sintaxis de enlace de datos:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Devuelve un `DataRowView` objeto correspondiente a la `DataSource` registro enlazado a la `GridViewRow`. Su `Row` propiedad devuelve fuertemente tipadas `Northwind.EmployeesRow`, que se pasa a la `DisplayDaysOnJob` método. Esta sintaxis de enlace de datos puede aparecer directamente en el `ItemTemplate` (como se muestra en la sintaxis declarativa siguiente) o se pueden asignar a la `Text` propiedad de un control Web Label.

> [!NOTE]
> O bien, en lugar de pasar una `EmployeesRow` instancia, simplemente podríamos pasar en el `HireDate` valor usando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Sin embargo, el `Eval` método devuelve un `object`, por lo que tenemos que cambiar nuestro `DisplayDaysOnJob` firma del método que acepte un parámetro de entrada de tipo `object`, en su lugar. Seguirá a ciegas, no podemos convertir el `Eval("HireDate")` la llamada a un `DateTime` porque el `HireDate` columna en el `Employees` tabla puede contener `NULL` valores. Por lo tanto, se necesita aceptar un `object` como parámetro de entrada para el `DisplayDaysOnJob` /método siguiente, compruebe si tuviera una base de datos `NULL` valor (que puede realizarse mediante `Convert.IsDBNull(objectToCheck)`) y, a continuación, proceda según corresponda.


Debido a estos matices, he optado por pasar en toda la matriz `EmployeesRow` instancia. En el siguiente tutorial veremos un ejemplo de ajuste más para usar el `Eval("columnName")` sintaxis para pasar un parámetro de entrada a un método de formato.

La siguiente muestra la sintaxis declarativa para nuestro GridView una vez agregada la TemplateField y `DisplayDaysOnJob` método invocado desde el `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

La figura 16 muestra el tutorial completado, cuando se ven a través de un explorador.


[![Se muestra el número de días, que el empleado ha estado en el trabajo](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figura 16**: el número de días el empleado ha estado en el trabajo se muestra ([haga clic aquí para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Resumen

TemplateField en el control GridView permite un mayor grado de flexibilidad para mostrar los datos que está disponible con los demás controles de campo. TemplateFields son ideales para situaciones donde:

- Varios campos de datos es necesario que se muestren en una columna de GridView
- La fecha se expresa mejor usando un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como mostrar metadatos, o en volver a formatear los datos

Además de personalizar la visualización de datos, TemplateFields también se usan para personalizar las interfaces de usuario que se usa para editar e insertar datos, como veremos en el futuro tutoriales.

Los siguientes dos tutoriales continuar explorando plantillas, a partir de un vistazo al uso de TemplateFields en DetailsView. A continuación, se podrá optar por FormView, que usa las plantillas en lugar de campos para proporcionar mayor flexibilidad en el diseño y la estructura de los datos.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Dan Jagers. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-cs.md)
> [Siguiente](using-templatefields-in-the-detailsview-control-cs.md)
