---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Realizar actualizaciones por lotes (VB) | Documentos de Microsoft
author: rick-anderson
description: Información sobre cómo crear un totalmente editable DataList donde todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en un botón "Actualizar todo" en los...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d28a431c2b09de8c46079e888aa191017de4e30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="performing-batch-updates-vb"></a>Realizar actualizaciones por lotes (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) o [descarga de PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Información sobre cómo crear un totalmente editable DataList donde todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en un botón "Actualizar todo" en la página.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) se examina cómo crear un nivel de elemento DataList. Como GridView editable estándar, cada elemento en el control DataList incluye una operación de edición botón que, al hacer clic en, haría que el elemento editable. Aunque este nivel de elemento edición funciona bien con datos que solo se actualizan ocasionalmente, ciertos escenarios de casos de uso requieren que el usuario editar el número de registros. Si un usuario necesita editar docenas de registros y se ve obligado a haga clic en Editar, realice los cambios y haga clic en Actualizar para cada una de ellas, la cantidad de hacer clic en puede dificultar su productividad. En estos casos, es una mejor opción proporcionar un control DataList completamente editable, donde *todos los* de sus elementos se encuentran en modo de edición y cuyos valores se pueden editar haciendo clic en un botón Actualizar todo en la página (consulte la figura 1).


[![Se puede modificar cada elemento en un control DataList Editable totalmente](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: se puede modificar cada elemento en un control DataList Editable totalmente ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image3.png))


En este tutorial, examinaremos cómo permitir a los usuarios actualizar información de direcciones de proveedores mediante un control DataList puede modificar por completo.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Paso 1: Crear la interfaz de usuario puede editar en DataList s ItemTemplate

En el tutorial anterior, donde se crea a un DataList editable estándar, el nivel de elemento, utilizamos dos plantillas:

- `ItemTemplate` contiene la interfaz de usuario de solo lectura (los controles Web de etiqueta para mostrar cada nombre de producto s y precio).
- `EditItemTemplate` contiene la interfaz de usuario de modo de edición (los dos controles de cuadro de texto Web).

El control DataList s `EditItemIndex` propiedad determina qué `DataListItem` (si existe) se representa mediante el `EditItemTemplate`. En concreto, el `DataListItem` cuyo `ItemIndex` valor coincide con el control DataList s `EditItemIndex` propiedad se represente usando el `EditItemTemplate`. Este modelo funciona bien cuando se puede editar un único elemento en un momento, pero la diferencia se encuentra al crear a un control DataList totalmente editables.

Para un control DataList puede modificar por completo, queremos *todos los* de la `DataListItem` s para representar mediante la interfaz editable. La manera más sencilla de lograrlo es definir la interfaz editable en el `ItemTemplate`. Para modificar la información de dirección de proveedores, la interfaz editable contiene el nombre del proveedor como texto y, a continuación, cuadros de texto para la dirección, ciudad y los valores de país.

Comience abriendo la `BatchUpdate.aspx` página, agregue un control DataList y establecer su `ID` propiedad `Suppliers`. Desde la etiqueta inteligente de DataList s, optar por agregar un nuevo control ObjectDataSource denominado `SuppliersDataSource`.


[![Crear un nuevo origen ObjectDataSource denominado SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: crear un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image6.png))


Configurar el ObjectDataSource para recuperar datos mediante el `SuppliersBLL` clase s. `GetSuppliers()` (método) (consulte la figura 3). Al igual que con el tutorial anterior, en lugar de actualizar la información de proveedor a través de ObjectDataSource, trabajamos directamente con la capa de lógica de negocios. Por tanto, establecer la lista desplegable en (ninguno) en la pestaña de actualización (consulte la figura 4).


[![Recuperar información de proveedor mediante el método GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperar información de proveedor mediante el `GetSuppliers()` método ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image9.png))


[![Establecer la lista desplegable en (ninguno) en la pestaña de actualización](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: establecer la lista desplegable en (ninguno) en la pestaña de actualización ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image12.png))


Después de completar el asistente, Visual Studio genera automáticamente el control DataList s `ItemTemplate` para mostrar cada campo de datos devuelto por el origen de datos en un control Web Label. Es necesario modificar esta plantilla para que proporcione la interfaz de edición en su lugar. El `ItemTemplate` se pueden personalizar mediante el diseñador utilizando la opción de editar plantillas de la etiqueta inteligente de DataList s o directamente a través de la sintaxis declarativa.

Tómese un momento para crear una interfaz de edición que muestra el nombre del proveedor s como texto, pero incluye cuadros de texto para el proveedor s dirección, ciudad y los valores de país. Después de realizar estos cambios, la sintaxis declarativa de s de página debe ser similar al siguiente:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Como con el tutorial anterior, el control DataList en este tutorial debe tener su estado de vista habilitado.


En el `ItemTemplate` I m con dos nuevas clases CSS, `SupplierPropertyLabel` y `SupplierPropertyValue`, que se han agregado a la `Styles.css` clase y configurado para usar la misma configuración de estilo que el `ProductPropertyLabel` y `ProductPropertyValue` clases CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Después de realizar estos cambios, visite esta página a través de un explorador. Como se muestra en la figura 5, cada elemento DataList muestra el nombre del proveedor como texto y cuadros de texto utiliza para mostrar la dirección, ciudad y país.


[![Cada proveedor en el control DataList está en modo Editable](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: cada proveedor en el control DataList es Editable ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Paso 2: Agregar una actualización botón todo

Aunque cada proveedor en la figura 5 tiene su dirección, ciudad y campos de país que se muestran en un cuadro de texto, actualmente no hay ningún botón de actualización disponible. En lugar de tener un botón de actualización por cada elemento, con completamente editable DataList suele haber un solo botón Actualizar todo en la página que, al hacer clic, actualiza *todos los* de los registros en el control DataList. Para este tutorial, permiten s agregar dos botones Actualizar todo: uno en la parte superior de la página y otro en la parte inferior (aunque al hacer clic en cualquiera de los botones, tendrá el mismo efecto).

Comience por agregar un control de botón Web anteriormente la DataList y establezca su `ID` propiedad `UpdateAll1`. A continuación, agregar el segundo control de botón Web bajo el control DataList, establecer su `ID` a `UpdateAll2`. Establecer el `Text` propiedades de los dos botones para actualizar todo. Por último, crear controladores de eventos de ambos botones `Click` eventos. En lugar de duplicar la lógica de actualización en cada uno de los controladores de eventos, permiten s refactorizar esa lógica a un tercer método, `UpdateAllSupplierAddresses`, con los controladores de eventos, basta con invocar este método terceros.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Figura 6 muestra la página después de han agregado los botones Actualizar todo.


[![Se han agregado dos botones de todas las actualizaciones a la página](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: se han agregado dos botones de todas las actualizaciones a la página ([haga clic aquí para ver la imagen a tamaño completo](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Paso 3: Actualizar toda la información de direcciones de proveedores

Con todos los elementos de s DataList mostrar la interfaz de edición y la adición de los botones Actualizar todo, todo lo que sigue siendo está escribiendo el código para realizar la actualización por lotes. En concreto, es necesario recorrer los elementos de DataList s y la llamada la `SuppliersBLL` clase s. `UpdateSupplierAddress` método para cada uno de ellos.

La colección de `DataListItem` instancias que utilizan el control DataList son accesibles a través de DataList s [ `Items` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Con una referencia a un `DataListItem`, podemos obtenemos correspondiente `SupplierID` desde el `DataKeys` referencia controla la Web de cuadro de texto dentro de colección y mediante programación el `ItemTemplate` como se muestra en el código siguiente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Cuando el usuario hace clic en uno de los botones Actualizar todo, el `UpdateAllSupplierAddresses` método recorre en iteración cada `DataListItem` en el `Suppliers` DataList y llamadas a la `SuppliersBLL` clase s. `UpdateSupplierAddress` método, pasando los valores correspondientes. Un valor no especificado para la dirección, ciudad o país pasa es un valor de `Nothing` a `UpdateSupplierAddress` (en lugar de una cadena en blanco), que da como resultado una base de datos `NULL` para los campos de registro s subyacente.

> [!NOTE]
> Como una mejora, puede que desee agregar un estado de control Web Label a la página que ofrece algunos mensajes de confirmación una vez realizada la actualización por lotes.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Actualizar solo las direcciones que se han modificado

El algoritmo de actualización de lote utilizado para este tutorial se llama el `UpdateSupplierAddress` método para *cada* proveedor en el control DataList, independientemente de si ha cambiado su información de dirección. Mientras estos blind actualiza t normalmente un problema de rendimiento, puede generar registros superfluos si se van a auditar cambios en la tabla de base de datos. Por ejemplo, si utiliza desencadenadores para registrar todos los `UPDATE` s para el `Suppliers` tabla a una tabla de auditoría cada vez que un usuario hace clic en el botón Actualizar todo se creará un nuevo registro de auditoría para cada proveedor en el sistema, independientemente de si el usuario efectúa cualquiera cambios.

Las clases DataTable de ADO.NET y DataAdapter están diseñadas para admitir las actualizaciones por lotes que da como resultado solo modificados, eliminados y nuevos registros en cualquier comunicación de base de datos. Cada fila de la tabla de datos tiene un [ `RowState` propiedad](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica si la fila se ha agregado a la tabla de datos, eliminado contenido, modificar, o se mantiene sin cambios. Cuando inicialmente se rellena una tabla de datos, todas las filas se marcan sin cambios. Cambiar el valor de cualquiera de las columnas de fila s marca la fila modificada.

En el `SuppliersBLL` clase actualizamos la información de dirección de proveedor especificado s por primera lectura en el registro de proveedor único en una `SuppliersDataTable` y, a continuación, establezca el `Address`, `City`, y `Country` valores de columna mediante el código siguiente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Este código naively asigna las pasa la dirección, ciudad y los valores de país para el `SuppliersRow` en el `SuppliersDataTable` independientemente de si han cambiado los valores. Estas modificaciones provocarán la `SuppliersRow` s `RowState` propiedad se marca como modificada. Cuando la s de capa de acceso a datos `Update` se llama al método, lo que ve el `SupplierRow` se ha modificado y, por tanto, se envía una `UPDATE` comando a la base de datos.

Sin embargo, imagine que agregamos código a este método para asignar solo la dirección pasada, city y valores de país si difieren de las `SuppliersRow` s valores existentes. En el caso donde la dirección, ciudad y país son los mismos que los datos existentes, no se realizará ningún cambio y la `SupplierRow` s `RowState` izquierda marcada como permanece invariable. El resultado neto es que cuando la capa DAL s `Update` se llama al método, no se realizará ninguna llamada de la base de datos porque el `SuppliersRow` no se ha modificado.

Para establecer este cambio, reemplace las instrucciones que se asignan a ciegas los pasa la dirección, ciudad y valores de país con el código siguiente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Con este código añadido, s de la capa DAL `Update` método envía un `UPDATE` instrucción a solo aquellos registros que se han cambiado cuyos valores relacionados con la dirección en la base de datos.

Como alternativa, podríamos hacer un seguimiento de si hay alguna diferencia entre los campos de dirección en el pasado y la base de datos y, si no hay ninguno, basta con omitir la llamada a la capa DAL s `Update` método. Este enfoque funciona bien si el método, que se van a usar la base de datos directo ya que el método directo DB ejecutando t pasa un `SuppliersRow` instancia cuya `RowState` se puede comprobar para determinar si realmente se necesita una llamada de la base de datos.

> [!NOTE]
> Cada vez que la `UpdateSupplierAddress` se invoca el método, se realiza una llamada a la base de datos para recuperar información sobre el registro actualizado. A continuación, si hay algún cambio en los datos, se realiza otra llamada a la base de datos para actualizar la fila de la tabla. Este flujo de trabajo podría optimizarse mediante la creación de un `UpdateSupplierAddress` sobrecarga del método que acepta un `EmployeesDataTable` instancia cuya *todos los* de los cambios desde la `BatchUpdate.aspx` página. A continuación, podría realizar una llamada a la base de datos para obtener todos los registros de la `Suppliers` tabla. A continuación, se han pudieron enumerar los dos conjuntos de resultados y solo aquellos registros que se han producido cambios pudieron actualizarse.


## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo crear a un control DataList puede modificar por completo, que permite a un usuario modificar rápidamente la información de dirección de varios proveedores. Hemos iniciado mediante la definición de la interfaz de modificación de un control de cuadro de texto Web para el proveedor s dirección, ciudad y los valores de país en el control DataList s `ItemTemplate`. A continuación, agregamos botones Actualizar todo encima y debajo el control DataList. Después de que un usuario ha realizado los cambios y hacer clic en uno de los botones Actualizar todo, el `DataListItem` se enumeran s y una llamada a la `SuppliersBLL` clase s `UpdateSupplierAddress` se realiza el método.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Zack Jones y Ken Pespisa. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-vb.md)
