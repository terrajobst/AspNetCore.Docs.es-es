---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Realizar actualizaciones por lotes (C#) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo crear una completamente editable DataList donde todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en un botón "Actualizar todo" en los...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 3528444269a3595681696251d3906a204090410c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807810"
---
<a name="performing-batch-updates-c"></a>Realizar actualizaciones por lotes (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) o [descargar PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Obtenga información sobre cómo crear una completamente editable DataList donde todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en un botón "Actualizar todo" en la página.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) hemos visto cómo crear un control DataList de nivel de elemento. Como GridView editable estándar, cada elemento en el control DataList incluye una edición de botón que, al hacer clic en, haría que el elemento editable. Aunque este nivel de elemento edición funciona bien con datos que solo se actualizan ocasionalmente, determinados escenarios de casos de uso requieren que el usuario editar el número de registros. Si un usuario necesita editar docenas de registros y se ve obligado a haga clic en Editar, realice los cambios y haga clic en Actualizar para cada uno de ellos, la cantidad de hacer clic en, puede reducir su productividad. En tales situaciones, una mejor opción es proporcionar un control DataList puede modificar por completo, donde *todas* de sus elementos están en modo de edición y cuyos valores se pueden editar, haga clic en un botón Actualizar todo en la página (consulte la figura 1).


[![Se puede modificar cada elemento en un control DataList Editable totalmente](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Figura 1**: se puede modificar cada elemento en un control DataList Editable totalmente ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image3.png))


En este tutorial, examinaremos cómo habilitar usuarios actualizar la información de direcciones de proveedores mediante un control DataList puede modificar por completo.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Paso 1: Crear la interfaz de usuario Editable en DataList s ItemTemplate

En el tutorial anterior, donde se crea a un DataList editable estándar, el nivel de elemento, usamos dos plantillas:

- `ItemTemplate` contiene la interfaz de usuario de solo lectura (los controles Web Label para mostrar cada nombre de producto s y precio).
- `EditItemTemplate` contiene la interfaz de usuario del modo de edición (los dos controles TextBox Web).

El control DataList s `EditItemIndex` propiedad determina qué `DataListItem` (si existe) se representa utilizando el `EditItemTemplate`. En concreto, el `DataListItem` cuyo `ItemIndex` valor coincide con el control DataList s `EditItemIndex` propiedad se representa utilizando el `EditItemTemplate`. Este modelo funciona bien cuando sólo un elemento puede modificarse en un momento, pero se encuentra una distancia al crear a un control DataList puede modificar por completo.

Para un control DataList totalmente editable, queremos *todas* de la `DataListItem` s para representar mediante la interfaz editable. La manera más sencilla de lograrlo es definir la interfaz editable en el `ItemTemplate`. Para modificar la información de dirección de proveedores, la interfaz editable contiene el nombre del proveedor como texto y, a continuación, cuadros de texto para la dirección, ciudad y los valores de país.

Comience abriendo la `BatchUpdate.aspx` página, agregue un control DataList y establezca su `ID` propiedad `Suppliers`. En la etiqueta inteligente de DataList s, optar por agregar un nuevo control ObjectDataSource denominado `SuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Figura 2**: crear un nuevo origen ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image6.png))


Configurar el origen ObjectDataSource para recuperar datos mediante el `SuppliersBLL` clase s `GetSuppliers()` (método) (consulte la figura 3). Al igual que con el tutorial anterior, en lugar de actualizar la información del proveedor a través de ObjectDataSource, vamos a trabajar directamente con la capa de lógica empresarial. Por lo tanto, establecer la lista desplegable en (None) en la pestaña de actualización (consulte la figura 4).


[![Recuperar la información de proveedor mediante el método GetSuppliers()](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Figura 3**: recuperar información de proveedor mediante el `GetSuppliers()` método ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image9.png))


[![Establecer la lista desplegable en (None) en la pestaña de la actualización](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Figura 4**: establecer la lista desplegable en (None) en la pestaña de actualización ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image12.png))


Después de completar el asistente, Visual Studio genera automáticamente el control DataList s `ItemTemplate` para mostrar cada campo de datos devuelto por el origen de datos en un control Web de la etiqueta. Es necesario modificar esta plantilla para que proporcione la interfaz de edición en su lugar. El `ItemTemplate` se pueden personalizar mediante el diseñador mediante la opción Editar plantillas de la etiqueta inteligente de DataList s o directamente a través de la sintaxis declarativa.

Dedique un momento para crear una interfaz de edición que muestra el nombre del proveedor s como texto, pero incluye cuadros de texto para la dirección de proveedor, ciudad y los valores de país. Después de realizar estos cambios, la sintaxis declarativa de s de página debe ser similar al siguiente:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Como con el tutorial anterior, el control DataList en este tutorial debe tener habilitado el estado de su vista.


En el `ItemTemplate` me m con dos nuevas clases CSS, `SupplierPropertyLabel` y `SupplierPropertyValue`, que se han agregado a la `Styles.css` clase y configurado para usar la misma configuración de estilo que el `ProductPropertyLabel` y `ProductPropertyValue` clases CSS.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Después de realizar estos cambios, visite esta página a través de un explorador. Como se muestra en la figura 5, cada elemento DataList muestra el nombre del proveedor como texto y cuadros de texto utiliza para mostrar la dirección, ciudad y país.


[![Cada proveedor en el control DataList es modificable](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Figura 5**: cada distribuidor en el control DataList es Editable ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Paso 2: Agregar una actualización botón todo

Aunque cada proveedor en la figura 5 tiene su dirección, ciudad y campos de país que se muestran en un control TextBox, actualmente no hay ningún botón de actualización disponible. En lugar de tener un botón de actualización por cada elemento, con DataList totalmente editables suele haber un único botón Actualizar todo en la página que, al hacer clic, actualiza *todas* de los registros en el control DataList. Para este tutorial, permiten s Agregue dos botones Actualizar todo: uno en la parte superior de la página y otro en la parte inferior (aunque al hacer clic en cualquiera de los botones tendrán el mismo efecto).

Comience por agregar un control de botón Web por encima de los controles DataList y conjunto su `ID` propiedad `UpdateAll1`. A continuación, agregue el segundo control de botón Web bajo el control DataList, establecer su `ID` a `UpdateAll2`. Establecer el `Text` las propiedades de los dos botones para actualizar todo. Por último, crear controladores de eventos de ambos botones `Click` eventos. En lugar de duplicar la lógica de actualización en cada uno de los controladores de eventos, s permiten refactorizar esa lógica a un tercer método `UpdateAllSupplierAddresses`, tener los controladores de eventos, basta con invocar este método terceros.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Figura 6 muestra la página después de han agregado los botones Actualizar todo.


[![Dos botones de todas las actualizaciones se han agregado a la página](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Figura 6**: dos botones de todas las actualizaciones se han agregado a la página ([haga clic aquí para ver imagen en tamaño completo](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Paso 3: Actualización de toda la información de dirección de proveedores

Con todos los elementos de s DataList mostrar la interfaz de edición y la adición de la actualización de todos los botones, todo lo que queda es escribir el código para realizar la actualización por lotes. En concreto, es necesario recorrer los elementos de DataList s y la llamada la `SuppliersBLL` clase s `UpdateSupplierAddress` método para cada uno de ellos.

La colección de `DataListItem` instancias que utilizan el control DataList puede obtenerse mediante el control DataList s [ `Items` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Con una referencia a un `DataListItem`, podemos tomamos correspondiente `SupplierID` desde el `DataKeys` referencia controla la Web de cuadro de texto dentro de colección y mediante programación el `ItemTemplate` como se muestra en el código siguiente:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Cuando el usuario hace clic en uno de los botones Actualizar todo, el `UpdateAllSupplierAddresses` método recorre en iteración cada `DataListItem` en el `Suppliers` DataList y llama a la `SuppliersBLL` clase s `UpdateSupplierAddress` método, pasando los valores correspondientes. Un valor no especificado para la dirección, ciudad o país pasa es un valor de `Nothing` a `UpdateSupplierAddress` (en lugar de una cadena vacía), que da como resultado una base de datos `NULL` para los campos de registro s subyacente.

> [!NOTE]
> Como una mejora, puede agregar un estado de control Web de la etiqueta a la página que proporciona algún mensaje de confirmación una vez realizada la actualización por lotes.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Actualizar solo las direcciones que se han modificado

El algoritmo de actualización por lotes utilizado para este tutorial se llama el `UpdateSupplierAddress` método para *cada* proveedor en el control DataList, independientemente de si ha cambiado su información de dirección. Mientras estas blind actualiza no son normalmente un problema de rendimiento, puede provocar registros superfluo si se van a auditar los cambios en la tabla de base de datos. Por ejemplo, si utiliza desencadenadores para registrar todas `UPDATE` s para el `Suppliers` tabla a una tabla de auditoría cada vez que un usuario hace clic en el botón Actualizar todo, se creará un nuevo registro de auditoría para cada proveedor en el sistema, independientemente de si el usuario realiza cada cambios.

Las clases DataTable de ADO.NET y DataAdapter están diseñadas para admitir las actualizaciones por lotes donde se produce solo modificados, eliminados y nuevos registros en cualquier comunicación de la base de datos. Cada fila de la tabla de datos tiene un [ `RowState` propiedad](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica si la fila se ha agregado a la DataTable, se eliminan de él, puede modificar, o permanece sin cambios. Cuando inicialmente se rellena una DataTable, todas las filas se marcan sin cambios. Cambiar el valor de cualquiera de las columnas de fila s marca la fila como modificado.

En el `SuppliersBLL` clase actualizamos la información de dirección de proveedor especificado s por leer primero en el registro de proveedor solo en un `SuppliersDataTable` y, a continuación, establezca el `Address`, `City`, y `Country` valores de columna mediante el código siguiente:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Este código asigna otra vez los pasa en la dirección, ciudad y valores de país para el `SuppliersRow` en el `SuppliersDataTable` independientemente de si los valores han cambiado. Estas modificaciones causan la `SuppliersRow` s `RowState` propiedad marcarse como modificada. Cuando la capa de acceso a datos de s `Update` se llama al método, lo que ve el `SupplierRow` se ha modificado y, por tanto, se envía un `UPDATE` comando a la base de datos.

Sin embargo, imagine que hemos agregado código a este método para asignar solo los pasa en la dirección, ciudad y valores de país si difieren de los `SuppliersRow` s valores existentes. En el caso donde la dirección, ciudad y país son los mismos que los datos existentes, no se realizará ningún cambio y la `SupplierRow` s `RowState` izquierda marcado como permanece invariable. El resultado neto es que cuando la s DAL `Update` se llama al método, no se realizará ninguna llamada de base de datos porque el `SuppliersRow` no ha sido modificado.

Para establecer este cambio, reemplace las instrucciones que asignan ciegamente el pasado en la dirección, ciudad y valores de país con el código siguiente:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Con este código añadido, la s DAL `Update` método envía un `UPDATE` instrucción a solo aquellos registros que han cambiado cuyos valores relacionados con la dirección de la base de datos.

Como alternativa, podríamos realizar un seguimiento de si hay alguna diferencia entre los campos de dirección en el pasado y la base de datos y, si no hay ninguno, basta con omitir la llamada a la s DAL `Update` método. Este enfoque funciona bien si el método, que se van a usar la base de datos directo ya que la base de datos directa método no se pasa un `SuppliersRow` cuya instancia `RowState` puede comprobarse para determinar si realmente se necesita una llamada de la base de datos.

> [!NOTE]
> Cada vez que el `UpdateSupplierAddress` se invoca el método, se realiza una llamada a la base de datos para recuperar información sobre el registro actualizado. A continuación, si hay algún cambio en los datos, se realiza otra llamada a la base de datos para actualizar la fila de tabla. Este flujo de trabajo se podría optimizar mediante la creación de un `UpdateSupplierAddress` sobrecarga del método que acepta un `EmployeesDataTable` instancia que tenga *todas* de los cambios de la `BatchUpdate.aspx` página. A continuación, podría realizar una llamada a la base de datos para obtener todos los registros de la `Suppliers` tabla. A continuación, se han podrían enumerar los dos conjuntos de resultados y solo aquellos registros donde se han producido cambios podrían actualizarse.


## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo crear a un control DataList puede modificar por completo, lo que permite a un usuario modificar rápidamente la información de dirección para varios proveedores. Comenzamos definiendo la interfaz de edición un control de cuadro de texto Web para la dirección de proveedor, ciudad y los valores de país en el control DataList s `ItemTemplate`. A continuación, agregamos botones Actualizar todo por encima y debajo el control DataList. Después de un usuario haya realizado sus cambios y hace clic en uno de los botones Actualizar todo, el `DataListItem` se enumeran s y una llamada a la `SuppliersBLL` clase s `UpdateSupplierAddress` se realiza el método.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones y Ken Pespisa. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-cs.md)
