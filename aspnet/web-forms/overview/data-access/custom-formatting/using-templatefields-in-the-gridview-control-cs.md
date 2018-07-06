---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Uso de TemplateFields en el Control GridView (C#) | Microsoft Docs
author: rick-anderson
description: Para proporcionar flexibilidad, el control GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles Web, y...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b01d2018d4164f8db7c86226f7f1d5999743e6c2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826707"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Uso de TemplateFields en el Control GridView (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) o [descargar PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Para proporcionar flexibilidad, el control GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles y sintaxis de enlace de datos Web. En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView.


## <a name="introduction"></a>Introducción

El control GridView se compone de un conjunto de campos que indican qué propiedades desde el `DataSource` deben incluirse en la salida representada junto con cómo se mostrará los datos. El tipo de campo más sencillo es el BoundField, que muestra un valor de datos como texto. Otros tipos de campo muestran los datos utilizando los elementos HTML alternativos. CheckBoxField, por ejemplo, se representa como una casilla de verificación cuyo estado activado depende del valor de un campo de datos especificado; ImageField presenta una imagen cuyo origen de la imagen se basa en un campo de datos especificado. Los hipervínculos y botones cuyo estado depende de un valor de campo de datos subyacente se pueden representar mediante los tipos de campo del campo Hyperlink y ButtonField.

Aunque los tipos de campo CampoCasillaVerificación, ImageField, campo Hyperlink y ButtonField permiten una vista alternativa de los datos, todavía son bastante limitados con respecto al formato. Un CampoCasillaVerificación solo puede mostrar una casilla, mientras que un ImageField solo puede mostrar una sola imagen. ¿Qué ocurre si necesita mostrar algo de texto, una casilla, un campo determinado *y* una imagen, todas basada en valores de campo de datos diferentes? O bien, ¿qué sucede si deseamos mostrar los datos mediante un control Web que no sea la casilla de verificación, imagen, hipervínculo o botón? Además, el BoundField limita su presentación en un único campo de datos. ¿Qué sucede si deseamos mostrar dos o más valores de campo de datos en una sola columna de GridView?

Para dar cabida a este nivel de flexibilidad GridView ofrece TemplateField, que se representa con un *plantilla*. Una plantilla puede incluir una combinación de HTML estático, controles y sintaxis de enlace de datos Web. Además, TemplateField tiene una variedad de plantillas que puede usarse para personalizar la representación para diferentes situaciones. Por ejemplo, el `ItemTemplate` se usa de forma predeterminada para representar la celda para cada fila, pero la `EditItemTemplate` plantilla se puede usar para personalizar la interfaz al editar los datos.

En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView. En el [tutorial anterior](custom-formatting-based-upon-data-cs.md) vimos cómo personalizar el formato en función de los datos subyacentes mediante la `DataBound` y `RowDataBound` controladores de eventos. Otra manera de personalizar el formato en función de los datos subyacentes es mediante una llamada a métodos de formato desde dentro de una plantilla. Veremos esta técnica en este tutorial también.

En este tutorial usaremos TemplateFields para personalizar la apariencia de una lista de empleados. En concreto, se deberá mostrar todos los empleados, sino que mostrará el empleado nombres y apellidos de una columna, su fecha de contratación de un control de calendario y una columna de estado que indica cuántos días ha sido trabajan en la empresa.


[![Tres TemplateFields sirven para personalizar la presentación](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figura 1**: tres TemplateFields sirven para personalizar la presentación ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Paso 1: Enlace de datos a la GridView

En escenarios donde es necesario usar TemplateFields para personalizar la apariencia de la creación de informes, le resulte más sencillo empezar creando un control GridView que contiene solo BoundFields primero y, a continuación, para agregar nuevas TemplateFields o convertir el BoundFields existente para TemplateFields según sea necesario. Por lo tanto, vamos a empezar este tutorial agregando un control GridView a la página a través del diseñador y enlazarlo a un origen ObjectDataSource que devuelve la lista de empleados. Estos pasos crearán un control GridView con BoundFields para cada uno de los campos de employee.

Abra el `GridViewTemplateField.aspx` página y arrastre un control GridView del cuadro de herramientas hasta el diseñador. Desde la etiqueta inteligente de GridView optar por agregar un nuevo control ObjectDataSource que invoca la `EmployeesBLL` la clase `GetEmployees()` método.


[![Agregar un nuevo Control ObjectDataSource que invoca el método GetEmployees()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo ObjectDataSource Control ese Invokes el `GetEmployees()` método ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Enlace el control GridView de esta manera se agregará automáticamente un BoundField para cada una de las propiedades de empleado: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, y `Country`. Para este informe vamos a no preocuparse por mostrar los `EmployeeID`, `ReportsTo`, o `Country` propiedades. Para quitar estos BoundFields puede:

- Utilice el cuadro de diálogo campos haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView para que aparezca este cuadro de diálogo. A continuación, seleccione el BoundFields desde la parte inferior izquierda y haga clic en la X roja botón para quitar el BoundField.
- Editar la sintaxis declarativa de GridView manualmente desde la vista del origen, elimine el `<asp:BoundField>` (elemento) para el BoundField que desea quitar.

Después de haber quitado el `EmployeeID`, `ReportsTo`, y `Country` BoundFields, marcado de GridView en su aspecto:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Dedique un momento para ver nuestro progreso en un explorador. En este momento debería ver una tabla con un registro para cada empleado y cuatro columnas: una para el empleado apellido, uno por su nombre, una para su puesto y uno para su fecha de contratación.


[![El LastName, FirstName, Title y HireDate campos se muestran para cada empleado](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figura 3**: el `LastName`, `FirstName`, `Title`, y `HireDate` se muestran los campos de cada empleado ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Paso 2: Mostrar los nombres y apellidos en una sola columna

Actualmente, cada empleado de la primera y última nombres se muestran en una columna independiente. Podría ser interesante combinar en una sola columna. Necesitamos usar TemplateField para lograr esto. Nos podemos ya sea agregar un nuevo TemplateField, agregarle el marcado necesario y la sintaxis de enlace de datos y, a continuación, eliminar el `FirstName` y `LastName` BoundFields o se puede convertir el `FirstName` BoundField en TemplateField, editar TemplateField para incluir el `LastName` valor y, a continuación, quite el `LastName` BoundField.

Ambos enfoques net el mismo resultado, pero personalmente me gusta convertir BoundFields a TemplateFields cuando sea posible porque la conversión se agrega automáticamente un `ItemTemplate` y `EditItemTemplate` con controles Web y la sintaxis de enlace de datos para imitar el aspecto y la funcionalidad de la BoundField. La ventaja es que se deberá realizar menos trabajo con TemplateField ya que el proceso de conversión habrá realiza parte del trabajo por nosotros.

Para convertir un BoundField existente en TemplateField, haga clic en el vínculo Editar columnas de etiqueta inteligente de GridView, muestre el cuadro de diálogo campos. Seleccione el BoundField para convertir en la lista en la esquina inferior izquierda y, a continuación, haga clic en el vínculo "Convert this field into a TemplateField" en la esquina inferior derecha.


[![Convertir un BoundField en TemplateField en el cuadro de diálogo campos](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figura 4**: convertir una BoundField en TemplateField en el cuadro de diálogo campos ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Siga adelante y convertir la `FirstName` BoundField en TemplateField. Después de este cambio, no hay ninguna diferencia en el Diseñador de visión. Esto es porque convierte el BoundField en TemplateField crea TemplateField que mantiene la apariencia y comportamiento de la BoundField. A pesar de que exista ninguna diferencia visual en este momento en el diseñador, este proceso de conversión reemplazó la sintaxis declarativa del BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` : con la siguiente sintaxis TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Como puede ver, TemplateField consta de dos plantillas un `ItemTemplate` que tiene una etiqueta cuyo `Text` propiedad está establecida en el valor de la `FirstName` campo de datos y un `EditItemTemplate` con un control TextBox control cuya `Text` también se establece la propiedad para el `FirstName` campo de datos. La sintaxis de enlace de datos - `<%# Bind("fieldName") %>` -indica que el campo de datos *`fieldName`* está enlazado a la propiedad de control Web especificada.

Para agregar la `LastName` valor para este TemplateField que necesitamos agregar otro control Web de la etiqueta de campo de datos el `ItemTemplate` y enlazar su `Text` propiedad `LastName`. Esto puede realizarse manualmente o a través del diseñador. Para hacerlo manualmente, basta con agregar la sintaxis declarativa adecuada para el `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Para agregarlo a través del diseñador, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView. Se mostrará la interfaz de edición de plantilla de GridView. En esta etiqueta inteligente de la interfaz es una lista de las plantillas en el control GridView. Puesto que sólo tenemos una TemplateField en este momento, las plantillas sola aparece en la lista desplegable son las plantillas para el `FirstName` TemplateField junto con el `EmptyDataTemplate` y `PagerTemplate`. El `EmptyDataTemplate` plantilla, si se especifica, se usa para representar la salida de GridView si no hay ningún resultado en los datos enlazados a la GridView; el `PagerTemplate`, si se especifica, se usa para representar la interfaz de paginación para un control GridView que admita la paginación.


[![Se pueden editar plantillas de GridView a través del diseñador](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figura 5**: plantillas puede ser editado a través del Diseñador de GridView ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Para mostrar también el `LastName` en el `FirstName` TemplateField arrastrar el control de etiqueta del cuadro de herramientas en el `FirstName` del TemplateField `ItemTemplate` en el control GridView de la edición de plantillas interfaz.


[![Agregar un Control Web Label a ItemTemplate de FirstName TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figura 6**: agregar un Control Web de etiqueta a la `FirstName` ItemTemplate del TemplateField ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


En este punto tiene el control de etiqueta Web agregado a TemplateField su `Text` propiedad establecida en "Label". Es necesario cambiar esto para que esta propiedad se enlaza al valor de la `LastName` del campo de datos en su lugar. Para lograr este haga clic en la etiqueta inteligente del control de etiqueta y elija la opción Editar DataBindings.


[![Elija la opción de Editar DataBindings de etiqueta inteligente de la etiqueta](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figura 7**: elija la opción de edición de DataBindings de etiqueta inteligente de la etiqueta ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Se abrirá el cuadro de diálogo enlaces de datos. Desde aquí puede seleccionar la propiedad para participar en el enlace de datos en la lista de la izquierda y elija el campo para enlazar los datos de la lista desplegable de la derecha. Elija la `Text` propiedad desde la izquierda y la `LastName` el campo de la derecha y haga clic en Aceptar.


[![Enlazar la propiedad de texto al campo de datos LastName](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figura 8**: enlazar el `Text` propiedad a la `LastName` campo de datos ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> El cuadro de diálogo DataBindings permite indicar si se debe realizar el enlace de datos bidireccional. Si deja esta casilla desactivada, la sintaxis de enlace de datos `<%# Eval("LastName")%>` se usará en lugar de `<%# Bind("LastName")%>`. Cualquier enfoque es adecuado para este tutorial. Enlace de datos bidireccional se vuelve importante al insertar y editar datos. Para mostrar datos, sin embargo, cualquiera de los enfoques funcionará bien. Trataremos el enlace de datos bidireccional en detalle en próximos tutoriales.


Dedique un momento para ver esta página a través de un explorador. Como puede ver, el control GridView todavía incluye cuatro columnas; Sin embargo, el `FirstName` columna muestra ahora *ambos* el `FirstName` y `LastName` valores de campo de datos.


[![Se muestran los valores LastName y FirstName en una sola columna](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figura 9**: tanto el `FirstName` y `LastName` se muestran los valores en una sola columna ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Para completar este primer paso, quite el `LastName` BoundField y cambiar el nombre de la `FirstName` del TemplateField `HeaderText` propiedad en "Name". Después de estos cambios marcado declarativo de GridView debe ser similar al siguiente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Nombre y apellidos de cada empleado se muestran en una columna](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figura 10**: nombre y apellidos de cada empleado se muestran en una columna ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Paso 3: Usar el Control de calendario para mostrar el`HiredDate`campo

Mostrar un valor de campo de datos como texto en un control GridView es tan simple como utilizar un BoundField. Para determinados escenarios, sin embargo, los datos se mejor expresa mediante un control Web determinado en lugar de sólo texto. Es posible con TemplateFields dicha personalización de la visualización de datos. Por ejemplo, en lugar de mostrar la fecha de contratación del empleado como texto, podríamos mostramos un calendario (mediante [el control de calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con su fecha de contratación resaltado.

Para ello, inicie convirtiendo el `HiredDate` BoundField en TemplateField. Simplemente vaya a la etiqueta inteligente de GridView y haga clic en el vínculo Edit Columns, muestre el cuadro de diálogo campos. Seleccione el `HiredDate` BoundField y haga clic en "conversión este campo TemplateField."


[![Convertir el HiredDate BoundField en TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figura 11**: convertir el `HiredDate` BoundField en TemplateField unas ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Como hemos visto en el paso 2, Esto reemplazará el BoundField con TemplateField que contiene un `ItemTemplate` y `EditItemTemplate` con una etiqueta y un cuadro de texto cuyo `Text` propiedades se enlazan a la `HiredDate` valor utilizando la sintaxis de enlace de datos `<%# Bind("HiredDate")%>`.

Para reemplazar el texto con un control de calendario, edite la plantilla al quitar la etiqueta y agregar un control de calendario. En el diseñador, seleccione Editar plantillas de etiqueta inteligente de GridView y elija el `HireDate` del TemplateField `ItemTemplate` en la lista desplegable. A continuación, elimine el control de etiqueta y arrastre un control de calendario en el cuadro de herramientas a la interfaz de edición de plantilla.


[![Agregar un Control de calendario para el HireDate ItemTemplate del TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figura 12**: agregar un Control de calendario para el `HireDate` del TemplateField `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


En este momento, cada fila en el control GridView contendrá un control de calendario en su `HiredDate` TemplateField. Sin embargo, el empleado del real `HiredDate` valor no está establecido en cualquier lugar en el control de calendario, causando cada control de calendario al valor predeterminado para que se muestra la fecha y el mes actual. Para solucionar este problema, necesitamos asignar cada empleado `HiredDate` para el control de calendario [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) y [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) propiedades.

En la etiqueta inteligente del control de calendario, elija Editar DataBindings. A continuación, enlazar ambos `SelectedDate` y `VisibleDate` propiedades para el `HiredDate` campo de datos.


[![Enlazar la propiedad SelectedDate y VisibleDate propiedades para el campo de datos HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figura 13**: enlazar el `SelectedDate` y `VisibleDate` propiedades para el `HiredDate` campo de datos ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> La fecha seleccionada del control de calendario no necesita ser visible necesariamente. Por ejemplo, un calendario puede tener el 1 de agosto<sup>st</sup>, 1999 como la fecha seleccionada, pero que se muestra el mes y año actual. La fecha seleccionada y la fecha visible se especifican mediante el control de calendario `SelectedDate` y `VisibleDate` propiedades. Puesto que deseamos realizar ambas una selección del empleado `HiredDate` y asegúrese de que se muestra, debemos enlazar ambas propiedades a la `HireDate` campo de datos.


Al ver la página en un explorador, el calendario ahora muestra el mes de la fecha de contratación del empleado y selecciona esa fecha en particular.


[![HiredDate del empleado se muestra en el Control de calendario](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figura 14**: el empleado `HiredDate` se muestra en el Control de calendario ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Al contrario que en todos los ejemplos que hemos visto hasta ahora, para este tutorial hemos hecho *no* establecer `EnableViewState` propiedad `false` para este control GridView. La razón para tomar esta decisión es que al hacer clic en las fechas del control Calendar produce un postback, establecer la fecha del calendario seleccionada a la fecha que acaba de hacer clic. Si se deshabilita el estado de vista de GridView, sin embargo, en cada devolución de datos de GridView se vuelve a enlazar al origen de datos subyacente, lo que hace que la fecha del calendario seleccionada debe establecerse *Atrás* al empleado `HireDate`, sobrescritura la fecha elegida por el usuario.


En este tutorial se trata de una discusión discutible debido a que el usuario no es capaz de actualizar el empleado `HireDate`. Probablemente sería mejor configurar el control de calendario para que las fechas no son seleccionables. No obstante, en este tutorial se muestra que en algunas circunstancias el estado de vista debe habilitarse con el fin de proporcionar cierta funcionalidad.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Paso 4: Que muestra al número de días que el empleado ha trabajado para la empresa

Hasta ahora hemos visto dos aplicaciones de TemplateFields:

- Combinar dos o más valores de campo de datos en una columna, y
- Expresar un valor de campo de datos mediante un control Web en lugar de texto

Un tercer uso de TemplateFields es mostrar los metadatos sobre la GridView datos subyacentes. Además de mostrar las fechas de contratación de los empleados, por ejemplo, podríamos también queremos tener una columna que muestra el número total de días han estado en el trabajo.

Todavía se produce otro uso de TemplateFields en escenarios cuando los datos subyacentes deben mostrarse de manera diferente en el informe de la página web que en el formato se almacena en la base de datos. Imagine que el `Employees` tabla tenía un `Gender` campo que almacena el carácter `M` o `F` para indicar el sexo del empleado. Al mostrar esta información en una página web que podemos desear mostrar el sexo como "Masculino" o "Female", en lugar de simplemente "M" o "F".

Ambos escenarios se pueden controlar mediante la creación de un *método de formato* en la clase de código subyacente de la página ASP.NET (o en una biblioteca de clases independiente, que se implementa como un `static` método) que se invoca desde la plantilla. Este método de formato se invoca desde la plantilla mediante la misma sintaxis de enlace de datos vista anteriormente. El método de formato puede tardar en cualquier número de parámetros, pero debe devolver una cadena. Esta cadena devuelta es el código HTML que se inserta en la plantilla.

Para ilustrar este concepto, vamos a aumentar nuestro tutorial para mostrar una columna que muestra el número total de días que un empleado ha sido en el trabajo. Este método de formato tardará un `Northwind.EmployeesRow` de objetos y devuelve el número de días que el empleado ha ejercido como una cadena. Este método se puede agregar a la clase de código subyacente de la página ASP.NET, pero *debe* marcarse como `protected` o `public` con el fin de ser accesibles desde la plantilla.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Puesto que la `HiredDate` campo puede contener `NULL` valores primero debemos asegurarnos de que el valor no es la base de datos `NULL` antes de continuar con el cálculo. Si el `HiredDate` valor es `NULL`, devolvemos simplemente la cadena "Unknown"; Si no es `NULL`, se calcula la diferencia entre la hora actual y el `HiredDate` de valor y devuelve el número de días.

Para utilizar este método es necesario invocarlo desde un TemplateField en el control GridView que usa la sintaxis de enlace de datos. Empiece agregando un nuevo TemplateField en el control GridView haciendo clic en el vínculo Editar columnas en la etiqueta inteligente de GridView y agregar un nuevo TemplateField.


[![Agregar un nuevo TemplateField en el control GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figura 15**: agregar un nuevo TemplateField en el control GridView ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Establecer este TemplateField nuevo `HeaderText` propiedad en "Días en el trabajo" y su `ItemStyle`del `HorizontalAlign` propiedad `Center`. Para llamar a la `DisplayDaysOnJob` método desde la plantilla, agregue un `ItemTemplate` y use la sintaxis de enlace de datos siguientes:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Devuelve un `DataRowView` objeto correspondiente a la `DataSource` registro enlazado a la `GridViewRow`. Su `Row` propiedad devuelve fuertemente tipadas `Northwind.EmployeesRow`, que se pasa a la `DisplayDaysOnJob` método. Esta sintaxis de enlace de datos puede aparecer directamente en el `ItemTemplate` (como se muestra en la sintaxis declarativa siguiente) o se pueden asignar a la `Text` propiedad de un control Web Label.

> [!NOTE]
> Como alternativa, en lugar de pasar un `EmployeesRow` instancia, podemos pasar simplemente en el `HireDate` valor mediante `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Sin embargo, el `Eval` método devuelve un `object`, por lo que tenemos que cambiar nuestra `DisplayDaysOnJob` firma del método que acepte un parámetro de entrada de tipo `object`en su lugar. No podemos convertir ciegamente el `Eval("HireDate")` la llamada a un `DateTime` porque el `HireDate` columna en el `Employees` tabla puede contener `NULL` valores. Por lo tanto, se debe aceptar un `object` como parámetro de entrada para el `DisplayDaysOnJob` método, compruebe si tenía una base de datos `NULL` valor (que puede realizarse mediante `Convert.IsDBNull(objectToCheck)`) y, a continuación, proceda según corresponda.


Debido a estos matices, he optado por pasar toda la `EmployeesRow` instancia. En el siguiente tutorial, vamos a ver un ejemplo de conexión más para usar el `Eval("columnName")` sintaxis para pasar un parámetro de entrada a un método de formato.

Lo siguiente muestra la sintaxis declarativa para nuestro GridView una vez agregada la TemplateField y `DisplayDaysOnJob` método se llama desde el `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Figura 16 muestra el tutorial completo, cuando se ve mediante un explorador.


[![Se muestra el número de días, que el empleado ha estado en el trabajo](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figura 16**: el número de días el empleado ha estado en el trabajo se muestra ([haga clic aquí para ver imagen en tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Resumen

TemplateField en el control GridView permite un mayor grado de flexibilidad al mostrar los datos que está disponible con los otros controles de campo. TemplateFields son ideales para las situaciones donde:

- Necesitan varios campos de datos que se mostrará en una columna de GridView
- Los datos se expresan mejor mediante un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como mostrar los metadatos o en volver a formatear los datos

Además de personalizar la visualización de datos, TemplateFields también se usan para personalizar las interfaces de usuario que se usa para editar e insertar datos, como veremos en el futuro tutoriales.

Los siguientes dos tutoriales con explorando las plantillas, a partir de un vistazo al uso de TemplateFields en DetailsView. A continuación, centraremos a FormView, que usa las plantillas en lugar de campos para proporcionar mayor flexibilidad en el diseño y la estructura de los datos.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Dan Jagers. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-cs.md)
> [Siguiente](using-templatefields-in-the-detailsview-control-cs.md)
