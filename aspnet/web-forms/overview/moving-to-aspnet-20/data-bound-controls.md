---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles enlazados a datos | Microsoft Docs
author: microsoft
description: La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de w interactúan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: adaf8a40c1877db4181e1b1c7a74a2ecbaa373ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368264"
---
<a name="data-bound-controls"></a>Controles enlazados a datos
====================
por [Microsoft](https://github.com/microsoft)

> La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de la interacción con los datos en aplicaciones Web dinámicas. ASP.NET 2.0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y una sintaxis declarativa.


La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de la interacción con los datos en aplicaciones Web dinámicas. ASP.NET 2.0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y una sintaxis declarativa.

El BaseDataBoundControl actúa como clase base para la clase DataBoundControl y la clase HierarchicalDataBoundControl. En este módulo, trataremos las siguientes clases que derivan de DataBoundControl:

- AdRotator
- Controles de lista
- GridView
- FormView
- DetailsView

También se tratan las siguientes clases que derivan de la clase HierarchicalDataBoundControl:

- TreeView
- Menú
- SiteMapPath

## <a name="databoundcontrol-class"></a>Clase de DataBoundControl

La clase de DataBoundControl es una clase abstracta (marcada MustInherit en VB) utilizada para interactuar con tabulares o datos de estilo de lista. Los controles siguientes son algunos de los controles que derivan de DataBoundControl.

## <a name="adrotator"></a>AdRotator

El control AdRotator permite mostrar un banner de gráficos en una página Web que esté vinculada a una dirección URL específica. Se gira el gráfico que se muestra mediante las propiedades del control. Se puede configurar la frecuencia de un anuncio determinado se muestra en una página mediante el **impresiones** propiedad y los anuncios se pueden filtrar mediante el filtrado de palabras clave.

Controles AdRotator usar un archivo XML o una tabla en una base de datos para los datos. Los siguientes atributos se usan en los archivos XML para configurar el control AdRotator.

### <a name="imageurl"></a>ImageUrl
La dirección URL de una imagen para mostrar para el anuncio.

### <a name="navigateurl"></a>NavigateUrl
La dirección URL que el usuario debe realizarse para cuando se hace clic en el anuncio. Esto debe estar codificado como URL.

### <a name="alternatetext"></a>AlternateText
Texto alternativo que se muestra en una información sobre herramientas y leer los lectores de pantalla. También se muestra cuando la imagen especificada por la propiedad ImageUrl no está disponible.

### <a name="keyword"></a>Palabra clave
Define una palabra clave que se puede usar cuando se usa el filtrado de palabras clave. Si se especifica, se mostrará solo los anuncios con una palabra clave que coincida con el filtro de la palabra clave.

### <a name="impressions"></a>Impresiones
Número de ponderación que determina con qué frecuencia es probable que aparezca un anuncio determinado. Es en relación con la impresión de otros anuncios en el mismo archivo. El valor máximo de las impresiones colectivas para todos los anuncios en un archivo XML es 2,048,000,000 1.

### <a name="height"></a>Alto
El alto del anuncio en píxeles.

### <a name="width"></a>Ancho
El ancho del anuncio en píxeles.


> [!NOTE]
> Los atributos de alto y ancho invalidan el alto y ancho para el propio control AdRotator.


Un archivo XML típico podría ser similar al siguiente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

En el ejemplo anterior, dos veces es tan probable que aparezcan como el anuncio para el sitio Web de ASP.NET debido al valor del atributo de impresiones ad de Contoso.

Para mostrar anuncios en el archivo XML anterior, agregue un control AdRotator a una página y establezca el **AdvertisementFile** propiedad para que apunte al archivo XML, tal como se muestra a continuación:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si decide utilizar una tabla de base de datos como el origen de datos para el control AdRotator, primero deberá configurar una base de datos mediante el siguiente esquema:

| **Nombre de columna** | **Tipo de datos** | **Descripción** |
| --- | --- | --- |
| Id. | int | Clave principal. Esta columna puede tener cualquier nombre. |
| ImageUrl | nvarchar (*longitud*) | Dirección URL absoluta o relativa de la imagen para mostrar para el anuncio. |
| NavigateUrl | nvarchar (*longitud*) | La dirección URL de destino para el anuncio. Si no proporciona un valor, el anuncio no es un hipervínculo. |
| AlternateText | nvarchar (*longitud*) | Texto que se muestra si no se encuentra la imagen. En algunos exploradores, el texto se muestra como una información sobre herramientas. Texto alternativo que se usa también para mejorar la accesibilidad para que los usuarios que no se pueden ver el gráfico pueden escuchar su descripción. |
| Palabra clave | nvarchar (*longitud*) | Una categoría para el anuncio en el que puede filtrar la página. |
| Impresiones | int(4) | Un número que indica la probabilidad de que se muestre el anuncio. Cuanto mayor sea el número, más a menudo el anuncio se mostrará. El total de todos los valores de impresiones en el archivo XML no puede tener más superior a 2.048.000.000-1. |
| Ancho | int(4) | El ancho de la imagen en píxeles. |
| Alto | int(4) | El alto de la imagen en píxeles. |

En casos donde ya tiene una base de datos con un esquema diferente, puede usar el **AlternateTextField**, **ImageUrlField**, y **NavigateUrlField** propiedades para asignar el AdRotator atributos a la base de datos existente. Para mostrar los datos de la base de datos en el control AdRotator, agregue un control de origen de datos a la página de configuración de la cadena de conexión para el control de origen de datos para que apunte a la base de datos y establecer el control AdRotator **DataSourceID** propiedad para el identificador del control de origen de datos. En casos donde haya una necesidad de configurar AdRotator anuncios mediante programación, utilice el evento AdCreated. El evento AdCreated toma dos parámetros; uno de un objeto y el otro es una instancia de AdCreatedEventArgs. El AdCreatedEventArgs es una referencia al anuncio que se va a crear.

El siguiente fragmento de código establece la propiedad ImageUrl, NavigateUrl y AlternateText para una instancia de ad mediante programación:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Los controles de lista incluyen ListBox, DropDownList, CheckBoxList, RadioButtonList y BulletedList. Cada uno de estos controles puede ser datos enlazados a un origen de datos. Se usa un campo del origen de datos como texto para mostrar y, opcionalmente, puede usar un segundo campo como valor de un elemento. Los elementos también pueden agregarse estáticamente en tiempo de diseño, y puede mezclar elementos estáticos y dinámicos elementos agregados desde un origen de datos.

Para datos enlazar un control de lista, agregue un control de origen de datos a la página. Especifique un comando SELECT para el control de origen de datos y, a continuación, Establece la propiedad DataSourceID del control de lista en el identificador del control de origen de datos. Use la **DataTextField** y **DataValueField** las propiedades para definir el texto para mostrar y el valor para el control. Además, puede usar el **DataTextFormatString** propiedad para controlar la apariencia del texto de presentación como sigue:

| **Expresión** | **Descripción** |
| --- | --- |
| Precio: {0:C} | Para datos numéricos o decimales. Muestra el literal "precio:" seguido de números en formato de moneda. El formato de moneda depende de la configuración de la referencia cultural especificada en el atributo de referencia cultural en el **página** la directiva o en el archivo Web.config. |
| {0:D4} | Para datos enteros. No se puede usar con números decimales. Números enteros se muestran en un campo con ceros que tiene cuatro caracteres de ancho. |
| {0:N2}% | Para los datos numéricos. Muestra el número con 2 posiciones decimales de precisión seguido por el literal "%". |
| {0:000.0} | Para datos numéricos o decimales. Los números se redondean a una posición decimal. Los números de menos de tres dígitos se rellenan con ceros. |
| {0:D} | Para los datos de fecha y hora. Formato de fecha larga de muestra ("jueves, 06 de agosto de 1996"). El formato de fecha depende de la configuración de la referencia cultural de la página o del archivo Web.config. |
| {0:d} | Para los datos de fecha y hora. Formato de fecha corta de muestra ("12/31/99"). |
| {0:yy-MM-dd} | Para los datos de fecha y hora. Muestra la fecha en formato numérico de año-mes-día (96-08-06) |

## <a name="gridview"></a>GridView

El control GridView permite mostrar datos tabulares y la edición usando un enfoque declarativo y es el sucesor del control DataGrid. Las siguientes características están disponibles en el control GridView.

- Enlazar a datos controles de origen, como SqlDataSource.
- Funcionalidades integradas de ordenación.
- Actualización y eliminación de las capacidades integradas.
- Capacidades integradas de paginación.
- Funciones de selección de fila integrado.
- Acceso mediante programación al modelo de objetos de GridView para establecer propiedades dinámicamente, controlar eventos y así sucesivamente.
- Varios campos de clave.
- Varios campos de datos para las columnas de hipervínculo.
- Apariencia personalizable a través de temas y estilos.

**Campos de columna**

Cada columna en el control GridView se representa mediante un objeto DataControlField. De forma predeterminada, se establece la propiedad AutoGenerateColumns en **true**, que crea un objeto AutoGeneratedField para cada campo del origen de datos. Cada campo, a continuación, se representa como una columna en el control GridView en el orden de aparición de cada campo del origen de datos. Manualmente puede controlar qué campos de columna aparecen en la **GridView** control estableciendo el **AutoGenerateColumns** propiedad **false** y, a continuación, definir su propio colección de campos de columna. Tipos de campo de columna diferente determinan el comportamiento de las columnas en el control.

En la tabla siguiente se enumera los tipos de campo de columna diferente que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos. Este es el tipo de columna predeterminado del control GridView. |
| ButtonField | Muestra un botón de comando para cada elemento en el control GridView. Esto le permite crear una columna de controles de botón personalizado, como el botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla de verificación para cada elemento en el control GridView. Este tipo de campo de columna se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra los botones de comando para realizar la selección, edición o eliminación de operaciones predefinidos. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de columna permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen para cada elemento en el control GridView. |
| TemplateField | Muestra el contenido definido por el usuario para cada elemento en el control GridView según una plantilla especificada. Este tipo de campo de columna permite crear un campo de columna personalizado. |

Para definir una colección de campos de columna mediante declaración, primero agregue de apertura y cierre **&lt;columnas&gt;** etiquetas entre las etiquetas apertura y cierre del control GridView. A continuación, se enumeran los campos de columna que desea incluir entre la apertura y cierre **&lt;columnas&gt;** etiquetas. Las columnas especificadas se agregan a la colección de columnas en el orden mostrado. El **columnas** colección almacena todos los campos en el control de la columna y le permite administrar mediante programación los campos de columna en el control GridView.

Los campos de columna declarados explícitamente se pueden mostrar en combinación con los campos de columna generado automáticamente. Cuando se usan ambos, los campos de columna declarados explícitamente se representan en primer lugar, seguido de los campos de columna generado automáticamente.

## <a name="binding-to-data"></a>Enlazar a datos

El control GridView se puede enlazar a un control de origen de datos (como **SqlDataSource**, **ObjectDataSource**, y así sucesivamente), así como cualquier origen de datos que implementa el System.Collections.IEnumerable interfaz (por ejemplo, System.Data.DataView, System.Collections.ArrayList o System.Collections.Hashtable). Para enlazar el control GridView con el tipo de origen de datos adecuado, use uno de los métodos siguientes:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control GridView al valor de identificador del control de origen de datos. El control GridView se enlaza al control de origen de datos especificado y puede aprovechar el origen de datos las capacidades del control para realizar la ordenación, actualizar, eliminar y funcionalidad de paginación automáticamente. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa la interfaz System.Collections.IEnumerable, establecer mediante programación la propiedad DataSource del control GridView al origen de datos y, a continuación, llame al método DataBind. Cuando se usa este método, el control GridView no proporciona ordenación, actualizar, eliminar y funcionalidad de paginación integrada. Deberá proporcionar esta funcionalidad usted mismo.

## <a name="data-operations"></a>Operaciones de datos

El control GridView proporciona muchas funciones integradas que permiten al usuario ordenar, actualizar, eliminar, seleccione y desplazarse por los elementos del control. Cuando el control GridView se enlaza a un control de origen de datos, el control GridView puede aprovechar el origen de datos de las capacidades del control y proporcionar la ordenación y actualizaciones automáticas, eliminando la funcionalidad.

> [!NOTE]
> El control GridView puede proporcionar soporte técnico para ordenar, actualizar y eliminar con otros tipos de orígenes de datos; Sin embargo, deberá proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.


La ordenación permite al usuario ordenar los elementos en el control GridView con respecto a una columna específica, haga clic en el encabezado de columna. Para habilitar la ordenación, establezca la propiedad AllowSorting **true**.

Las funcionalidades de actualización, eliminación y selección automática se habilitan al seleccionar un botón en un **ButtonField** o **TemplateField** campo de columna con un nombre de comando de "Editar", "Delete" y "Select", respectivamente, hacer clic en. El control GridView puede agregar automáticamente una **CommandField** campo de columna con una edición, eliminación o botón Seleccionar si se establece la propiedad AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton en **true**, respectivamente.

> [!NOTE]
> Insertar registros en el origen de datos no es directamente compatible con el control GridView. Sin embargo, es posible insertar registros mediante el control GridView en conjunción con DetailsView o FormView control.


En lugar de mostrar todos los registros del origen de datos al mismo tiempo, el control GridView puede dividir automáticamente los registros de en las páginas. Para habilitar la paginación, establezca la propiedad AllowPaging **true**.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control GridView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control GridView. Cuando se establece esta propiedad, se muestran las filas de datos alternando entre la configuración RowStyle y **AlternatingRowStyle** configuración. |
| EditRowStyle | La configuración de estilo para la fila que se está editando en el control GridView. |
| EmptyDataRowStyle | La configuración de estilo de la fila de datos vacía mostrada en el control GridView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control GridView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control GridView. |
| PagerStyle | La configuración de estilo para la fila de paginación del control GridView. |
| RowStyle | La configuración de estilo para las filas de datos en el control GridView. Cuando el **AlternatingRowStyle** también se establece la propiedad, se muestran las filas de datos alternando el **RowStyle** configuración y el **AlternatingRowStyle** configuración. |
| SelectedRowStyle | La configuración de estilo para la fila seleccionada en el control GridView. |

También puede mostrar u ocultar diferentes partes del control. En la tabla siguiente se enumera las propiedades que controlan qué partes se mostrarse u ocultas.

| **Property** | **Descripción** |
| --- | --- |
| ShowFooter | Muestra u oculta la sección de pie de página del control GridView. |
| ShowHeader | Muestra u oculta la sección de encabezado del control GridView. |

### <a name="events"></a>Eventos

El control GridView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada siempre que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control GridView.

| **Event** | **Descripción** |
| --- | --- |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control GridView controla la operación de paginación. Este evento suele utilizarse cuando deba realizar una tarea una vez que el usuario navega a otra página en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de GridView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |
| RowCancelingEdit | Se produce cuando se hace clic en el botón de cancelación de una fila, pero antes de que el control GridView sale del modo de edición. Este evento se usa a menudo para detener la operación de cancelación. |
| RowCommand | Se produce cuando se hace clic en un botón en el control GridView. Este evento se usa a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| RowCreated | Se produce cuando se crea una nueva fila en el control GridView. Este evento se usa a menudo para modificar el contenido de una fila cuando se creó la fila. |
| RowDataBound | Se produce cuando una fila de datos está enlazada a datos en el control GridView. Este evento se usa a menudo para modificar el contenido de una fila cuando la fila está enlazada a datos. |
| RowDeleted | Se produce cuando se hace clic en el botón de eliminación de una fila, pero después de que el control GridView elimina el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| RowDeleting | Se produce cuando se hace clic en el botón de eliminación de una fila, pero antes de GridView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| RowEditing | Se produce cuando se hace clic en el botón de edición de una fila, pero antes de GridView control entra en modo de edición. Este evento se utiliza a menudo para cancelar la operación de edición. |
| RowUpdated | Se produce cuando se hace clic en el botón Actualizar de una fila, pero después de que el control GridView actualiza la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| RowUpdating | Se produce cuando se hace clic en el botón Actualizar de una fila, pero antes de GridView control actualiza la fila. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| Evento SelectedIndexChanged | Se produce cuando se hace clic en el botón Seleccionar de una fila, pero después de que el control GridView controla la operación de selección. Este evento se usa a menudo para realizar una tarea una vez que se selecciona una fila en el control. |
| SelectedIndexChanging | Se produce cuando se hace clic en el botón Seleccionar de una fila, pero antes de GridView control administra la operación de selección. Este evento se usa a menudo para cancelar la operación de selección. |
| Ordenar | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero después de que el control GridView controla la operación de ordenación. Este evento normalmente se usa para realizar una tarea una vez que el usuario hace clic en un hipervínculo para ordenar una columna. |
| Ordenar | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero antes de GridView control administra la operación de ordenación. Este evento suele utilizarse para cancelar la operación de ordenación o realizar una rutina de ordenación personalizada. |

## <a name="formview"></a>FormView

Se utiliza el control FormView para mostrar un único registro de un origen de datos. Es similar al control DetailsView, salvo que muestra las plantillas definidas por el usuario en lugar de campos de fila. Crear sus propias plantillas le ofrece mayor flexibilidad para controlar cómo se muestran los datos. El control FormView admite las siguientes características:

- Enlazar a datos controles de origen, como SqlDataSource y ObjectDataSource.
- Funciones integradas de inserción.
- Actualización y eliminación de las capacidades integradas.
- Capacidades integradas de paginación.
- Acceso mediante programación al modelo de objetos de FormView para establecer propiedades dinámicamente, controlar eventos y así sucesivamente.
- Personalización del aspecto mediante plantillas definidas por el usuario, temas y estilos.

## <a name="templates"></a>Plantillas

Para que el control FormView mostrar el contenido, debe crear plantillas para las distintas partes del control. La mayoría de las plantillas son opcionales; Sin embargo, debe crear una plantilla para el modo en que se configura el control. Por ejemplo, un control FormView que admite la inserción de registros debe tener definida una plantilla de elemento de inserción. En la tabla siguiente se enumera las diferentes plantillas que se pueden crear.

| **Tipo de plantilla** | **Descripción** |
| --- | --- |
| EditItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de edición. Esta plantilla normalmente contiene controles de entrada y botones de comando con el que el usuario puede editar un registro existente. |
| EmptyDataTemplate | Define el contenido de la fila de datos vacía mostrada cuando se enlaza el control FormView a un origen de datos que no contiene ningún registro. Normalmente, esta plantilla contiene contenido para avisar al usuario que el origen de datos no contiene ningún registro. |
| FooterTemplate | Define el contenido de la fila de pie de página. Esta plantilla normalmente contiene cualquier contenido adicional que le gustaría mostrar en la fila de pie de página. Como alternativa, puede especificar simplemente el texto para mostrar en la fila de pie de página estableciendo la propiedad FooterText. |
| HeaderTemplate | Define el contenido de la fila de encabezado. Esta plantilla normalmente contiene cualquier contenido adicional que le gustaría mostrar en la fila de encabezado. Como alternativa, puede especificar simplemente el texto para mostrar en la fila de encabezado estableciendo la propiedad HeaderText. |
| ItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de solo lectura. Normalmente, esta plantilla contiene contenido para mostrar los valores de un registro existente. |
| InsertItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de inserción. Esta plantilla normalmente contiene controles de entrada y botones de comando con el que el usuario puede agregar un nuevo registro. |
| PagerTemplate | Define el contenido de la fila de paginación mostrada cuando está habilitada la característica de paginación (cuando se establece la propiedad AllowPaging en **true**). Esta plantilla normalmente contiene controles con los que el usuario pueda ir a otro registro. |

Los controles de entrada en la plantilla de elemento de edición y la plantilla de elemento de inserción se pueden enlazar a los campos de un origen de datos mediante el uso de una expresión de enlace bidireccional. Esto permite que el control FormView extraer los valores del control de entrada para una actualización automática u operación de inserción. Las expresiones de enlace bidireccional también permiten los controles de entrada en una plantilla de elemento de edición para mostrar automáticamente los valores de campo originales.

### <a name="binding-to-data"></a>Enlazar a datos

Se puede enlazar el control FormView para un control de origen de datos (como **SqlDataSource**, AccessDataSource, **ObjectDataSource** y así sucesivamente), o a cualquier origen de datos que implementa el Interfaz System.Collections.IEnumerable (por ejemplo, System.Data.DataView System.Collections.ArrayList y System.Collections.Hashtable). Para enlazar el control FormView para el tipo de origen de datos adecuado, use uno de los métodos siguientes:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control FormView para el valor de identificador del control de origen de datos. El control FormView automáticamente enlaza el control de código fuente de datos especificado y puede aprovechar el origen de datos las capacidades del control para realizar Insertar, actualizar, eliminar y funcionalidad de paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** interfaz, establecer mediante programación la propiedad DataSource del control FormView para el origen de datos y, a continuación, llame al método DataBind. Cuando se usa este método, no proporciona el control FormView integradas de inserción, actualización, eliminación y paginación. Deberá proporcionar esta funcionalidad mediante el evento adecuado.

## <a name="data-operations"></a>Operaciones de datos

El control FormView proporciona muchas funciones integradas que permiten al usuario actualizar, eliminar, insertar y desplazarse por los elementos del control. Cuando se enlaza el control FormView para un control de origen de datos, el control FormView puede aprovechar el origen de datos de las capacidades del control y proporcionar actualizar, eliminar, insertar y funcionalidad de paginación automática. El control FormView puede proporcionar soporte técnico para update, delete, insert y las operaciones de paginación con otros tipos de orígenes de datos; Sin embargo, debe proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.

Dado que el control FormView utiliza plantillas, no proporciona una manera de generar automáticamente los botones de comando para llevar a cabo la actualización, eliminación o las operaciones de inserción. Debe incluir manualmente estos botones de comando en la plantilla adecuada. El control FormView reconoce ciertos botones que tienen sus **CommandName** propiedades establecidas en valores específicos. En la tabla siguiente se enumera los botones de comando que reconozca el control FormView.

| **Button** | **Valor de CommandName** | **Descripción** |
| --- | --- | --- |
| Cancelar | La opción "Cancelar" | Utilizado para actualizar o las operaciones de inserción para cancelar la operación y descartar los valores especificados por el usuario. A continuación, devuelve el control FormView para el modo especificado por la propiedad DefaultMode. |
| Eliminar | “Eliminar” | Se usa en las operaciones de eliminación para eliminar el registro mostrado del origen de datos. Provoca los eventos ItemDeleting y ItemDeleted. |
| Editar | "Edit" | Se utiliza en las operaciones de actualización para poner el control FormView en modo de edición. El contenido especificado en el **EditItemTemplate** propiedad se muestra para la fila de datos. |
| Insertar | "Insert" | Se utiliza en las operaciones de inserción para intentar insertar un nuevo registro en el origen de datos utilizando los valores proporcionados por el usuario. Provoca los eventos ItemInserting y ItemInserted. |
| Nuevo | "Nuevo" | Se utiliza en las operaciones de inserción para colocar el control FormView en modo de inserción. El contenido especificado en el **InsertItemTemplate** propiedad se muestra para la fila de datos. |
| Página | "Página" | Se usan en las operaciones de paginación para representar un botón en la fila de paginación que realiza la paginación. Para especificar la operación de paginación, establezca la **CommandArgument** propiedad del botón en "Siguiente", "Anterior", "First", "Last" o el índice de la página que se va a navegar. Provoca los eventos PageIndexChanging y PageIndexChanged. |
| Actualizar | "Actualización" | Utilizado en las operaciones de actualización para intentar actualizar el registro mostrado en el origen de datos con los valores proporcionados por el usuario. Provoca los eventos ItemUpdating y ItemUpdated. |

A diferencia de la eliminación button (lo que elimina el registro mostrado inmediatamente), cuando se hace clic en el botón Editar o nuevo, el control entra en la edición de FormView o modo de inserción, respectivamente. En modo de edición, el contenido incluido en el **EditItemTemplate** propiedad se muestra para el elemento de datos actual. Normalmente, la plantilla de elemento de edición se define tal que el botón de edición se reemplaza por una actualización y un botón Cancelar. Normalmente también se muestran los controles de entrada que son adecuados para el tipo de datos del campo (por ejemplo, un cuadro de texto o un control CheckBox) con el valor de un campo para el usuario modificar. Al hacer clic en el botón de actualización actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios.

Del mismo modo, el contenido incluido en el **InsertItemTemplate** propiedad se muestra para el elemento de datos cuando el control está en modo de inserción. Normalmente se define la plantilla de elemento de inserción tal que el botón nuevo se reemplaza por una instrucción Insert y un botón Cancelar, y se muestran los controles de entrada vacíos para que el usuario que escriba los valores para el nuevo registro. Al hacer clic en el botón Insertar inserta el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios.

El control FormView ofrece una característica de paginación, lo que permite al usuario navegar a otros registros del origen de datos. Cuando se habilita, se muestra una fila de paginación en el control FormView que contiene los controles de navegación de página. Para habilitar la paginación, establezca la **AllowPaging** propiedad **true**. Puede personalizar la fila de paginación estableciendo las propiedades de los objetos contenidos en el PagerStyle y la propiedad PagerSettings. En lugar de usar la interfaz de usuario de la fila de paginación integrada, puede crear su propia interfaz de usuario mediante el **PagerTemplate** propiedad.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control FormView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| EditRowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el control FormView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control FormView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control FormView. |
| InsertRowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila de paginación mostrada en el control FormView cuando está habilitada la característica de paginación. |
| RowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de solo lectura. |

## <a name="events"></a>Eventos

El control FormView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada siempre que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control FormView.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón dentro de un control FormView. Este evento se usa a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| ItemCreated | Se produce después de que todos los objetos FormViewRow se crean en el control FormView. Este evento se usa a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando un botón Eliminar (un botón con su **CommandName** propiedad establecida en "Delete") se hace clic en, pero después de que el control FormView elimina el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de FormView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando un botón Insertar (un botón con su **CommandName** propiedad establecida en "Insert") se hace clic en, pero después de que el control FormView inserta el registro. Este evento se usa a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de FormView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando un botón de actualización (un botón con su **CommandName** propiedad establecida en "Update") se hace clic en, pero después de que el control FormView actualiza la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de FormView control actualiza el registro. Este evento se usa a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control FormView cambie los modos de (a la edición, inserción o modo de solo lectura). Este evento se usa a menudo para realizar una tarea cuando el control FormView cambia los modos. |
| ModeChanging | Se produce antes de que el control FormView cambie los modos de (a la edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control FormView controla la operación de paginación. Este evento suele utilizarse cuando deba realizar una tarea una vez que el usuario navega a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de FormView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="detailsview"></a>DetailsView

El control DetailsView se usa para mostrar un único registro de un origen de datos en una tabla, donde cada campo del registro se muestra en una fila de la tabla. Se puede usar en combinación con un control GridView para escenarios de maestro y detalles. El control DetailsView admite las siguientes características:

- Enlazar a datos controles de origen, como SqlDataSource.
- Funciones integradas de inserción.
- Actualización y eliminación de las capacidades integradas.
- Capacidades integradas de paginación.
- Acceso mediante programación al modelo de objetos de DetailsView para establecer propiedades dinámicamente, controlar eventos y así sucesivamente.
- Apariencia personalizable a través de temas y estilos.

## <a name="row-fields"></a>Campos de fila

Cada fila de datos en el control DetailsView se crea mediante la declaración de un control de campo. Tipos de campos de fila diferente determinan el comportamiento de las filas del control. Los controles de campo se derivan de DataControlField. En la tabla siguiente se enumera los tipos de campos de fila diferentes que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos como texto. |
| ButtonField | Muestra un botón de comando en el control DetailsView. Esto le permite mostrar una fila con un control de botón personalizado, como una botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla de verificación en el control DetailsView. Este tipo de campo de fila se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra el comando integrado botones para realizar la edición, inserción o eliminación de las operaciones en el control DetailsView. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de fila permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen en el control DetailsView. |
| TemplateField | Muestra el contenido definido por el usuario para una fila en el control DetailsView según una plantilla especificada. Este tipo de campo de fila permite crear un campo de fila personalizado. |

De forma predeterminada, se establece la propiedad AutoGenerateRows en **true**, que genera automáticamente un objeto de campo de fila enlazados para cada campo de tipo enlazable en el origen de datos. Los tipos enlazables válidos son String, DateTime, Decimal, Guid y el conjunto de tipos primitivos. Cada campo se muestra a continuación, en una fila como texto, en el orden en que cada campo aparece en el origen de datos.

Generación automática de las filas proporciona una manera rápida y fácil para mostrar todos los campos en el registro. Sin embargo, para usar DetailsView control avanzados se deben declarar explícitamente los campos de fila para incluir en el control DetailsView de capacidades. Para declarar los campos de fila, establezca primero la **AutoGenerateRows** propiedad **false**. A continuación, agregue la apertura y cierre **&lt;campos&gt;** etiquetas entre las etiquetas apertura y cierre del control DetailsView. Por último, enumera los campos de fila que se van a incluir entre la apertura y cierre **&lt;campos&gt;** etiquetas. Los campos de fila especificados se agregan a la colección de campos en el orden mostrado. El **campos** colección le permite administrar mediante programación los campos de fila en el control DetailsView.

> [!NOTE]
> Los campos no se agregan a la colección de campos de fila generados automáticamente.


## <a name="binding-to-data"></a>Enlazar a datos

El control DetailsView se puede enlazar a un control de origen de datos, como **SqlDataSource** o AccessDataSource, o a cualquier origen de datos que implementa la interfaz System.Collections.IEnumerable, como System.Data.DataView, System.Collections.ArrayList y System.Collections.Hashtable.

Para enlazar el control DetailsView para el tipo de origen de datos adecuado, use uno de los métodos siguientes:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control DetailsView al valor de identificador del control de origen de datos. El control DetailsView se enlaza automáticamente al control de origen de datos especificado. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** interfaz, establecer la propiedad DataSource del control DetailsView mediante programación al origen de datos y, a continuación, llame al método DataBind.

## <a name="security"></a>Seguridad

Este control se puede utilizar para mostrar los datos proporcionados por el usuario, que pueden incluir scripts de cliente malintencionado. Compruebe que cualquier información que se envía desde un cliente para secuencias de comandos ejecutables, instrucciones SQL u otro código antes de mostrarla en la aplicación. ASP.NET proporciona una característica de validación de solicitud de entrada para el bloque script y HTML en la entrada del usuario.

## <a name="data-operations"></a>Operaciones de datos

El control DetailsView proporciona funciones integradas que permiten al usuario actualizar, eliminar, insertar y desplazarse por los elementos del control. Cuando el control DetailsView se enlaza a un control de origen de datos, el control DetailsView puede aprovechar el origen de datos de las capacidades del control y proporcionar actualizar, eliminar, insertar y funcionalidad de paginación automática.

El control DetailsView puede proporcionar soporte técnico para update, delete, insert y las operaciones de paginación con otros tipos de orígenes de datos; Sin embargo, debe proporcionar la implementación para estas operaciones en un controlador de eventos adecuado.

Puede agregar automáticamente el control DetailsView un **CommandField** campo de fila con el botón Editar, eliminar o nuevo estableciendo las propiedades AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton a **true**, respectivamente. A diferencia de la eliminación button (lo que elimina el registro seleccionado inmediatamente), cuando se hace clic en el botón Editar o nuevo, DetailsView control entra en la edición o inserción, respectivamente. En modo de edición, el botón de edición se reemplaza por una actualización y un botón Cancelar. Controles de entrada que son adecuados para el tipo de datos del campo (por ejemplo, un cuadro de texto o un control CheckBox) se muestran con el valor de un campo para el usuario modificar. Al hacer clic en el botón de actualización actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios. Del mismo modo, en modo de inserción, el botón nuevo se reemplaza por una instrucción Insert y un botón Cancelar, y se muestran los controles de entrada vacíos para que el usuario que escriba los valores para el nuevo registro.

El control DetailsView proporciona una característica de paginación, lo que permite al usuario navegar a otros registros del origen de datos. Cuando se habilita, los controles de navegación de página se muestran en una fila de paginación. Para habilitar la paginación, establezca la propiedad AllowPaging **true**. La fila de paginación puede personalizarse mediante las propiedades PagerStyle y PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control DetailsView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control DetailsView. Cuando se establece esta propiedad, se muestran las filas de datos alternando entre la configuración RowStyle y **AlternatingRowStyle** configuración. |
| CommandRowStyle | La configuración de estilo para la fila que contiene los botones de comando integrado en el control DetailsView. |
| EditRowStyle | La configuración de estilo para las filas de datos cuando el control DetailsView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo de la fila de datos vacía mostrada en el control DetailsView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control DetailsView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control DetailsView. |
| InsertRowStyle | La configuración de estilo para las filas de datos cuando el control DetailsView está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila de paginación del control DetailsView. |
| RowStyle | La configuración de estilo para las filas de datos en el control DetailsView. Cuando el **AlternatingRowStyle** también se establece la propiedad, se muestran las filas de datos alternando el **RowStyle** configuración y el **AlternatingRowStyle** configuración. |
| FieldHeaderStyle | La configuración de estilo para la columna de encabezado del control DetailsView. |

## <a name="events"></a>Eventos

El control DetailsView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada siempre que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control DetailsView. El control DetailsView también hereda estos eventos de sus clases base: enlace de datos, enlazados a datos, Disposed, Init, Load, PreRender y representación.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón en el control DetailsView. |
| ItemCreated | Se produce después de que todos los objetos DetailsViewRow se crean en el control DetailsView. Este evento se usa a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón Eliminar, pero después de que el control DetailsView elimina el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de DetailsView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón de inserción, pero una vez que el control DetailsView inserta el registro. Este evento se usa a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de DetailsView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón Actualizar, pero después de que el control DetailsView actualiza la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de DetailsView control actualiza el registro. Este evento se usa a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control DetailsView cambie los modos (edición, inserción o modo de solo lectura). Este evento se usa a menudo para realizar una tarea cuando el control DetailsView cambia los modos. |
| ModeChanging | Se produce antes de que el control DetailsView cambie los modos (edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control DetailsView controla la operación de paginación. Este evento suele utilizarse cuando deba realizar una tarea una vez que el usuario navega a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de DetailsView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="the-menu-control"></a>El Control de menú

El control Menu en ASP.NET 2.0 está diseñado para ser un sistema de exploración completa. Puede ser enlazado a datos fácilmente con orígenes de datos jerárquicos, como el SiteMapDataSource.

Una estructura de los controles de menú se puede definir de forma declarativa o dinámica y consta de un único nodo raíz y el número de nodos secundarios. El código siguiente define mediante declaración un menú para el control de menú.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

En el ejemplo anterior, el nodo Home.aspx es el nodo raíz. Todos los demás nodos están anidados dentro del nodo raíz de los distintos niveles.

Hay dos tipos de menús que puede representar el control de menú; los menús estáticos y menús dinámicos. Los menús estáticos se componen de elementos de menú siempre están visibles. Menús dinámicos constan de elementos de menú que solo están visibles cuando el usuario mantenga el mouse sobre ellos con el mouse. A menudo pueden confundir a los clientes los menús estáticos con los menús que se definen de forma declarativa y menús dinámicos con los menús que están enlazado a datos en tiempo de ejecución. De hecho, los menús estáticos y dinámicos están relacionadas con el método de población. Los términos *estático* y *dinámica* sólo hacen referencia a si es o no el menú estáticamente muestra de forma predeterminada o solo aparece cuando el usuario realiza alguna acción.

El **StaticDisplayLevels** propiedad se utiliza para configurar el número de niveles del menú es estáticos y, por tanto, se muestran de forma predeterminada. En el ejemplo anterior, establecer el **StaticDisplayLevels** propiedad con un valor de 2 produciría el menú para mostrar de forma estática el nodo principal, el nodo de música y el nodo de películas. Todos los demás nodos dinámicamente aparecería cuando el usuario mantiene el mouse sobre el nodo primario.

El **MaximumDynamicDisplayLevels** propiedad configura el número máximo de niveles dinámicos es capaz de mostrar el menú. Los menús dinámicos en un nivel superior al valor especificado por el **MaximumDynamicDisplayLevels** propiedad se descartan.

> [!NOTE]
> Es casi seguro que podría encontrarse con situaciones donde no aparecen los menús representar debido a la propiedad MaximumDynamicDisplayLevels. En esos casos, asegúrese de que la propiedad se establece lo suficiente para permitir la visualización de los menús de los clientes.


## <a name="data-binding-the-menu-control"></a>El Control de menú de enlace de datos

El control de menú se puede enlazar a cualquier origen de datos jerárquicos, como el SiteMapDataSource o XMLDataSource. El SiteMapDataSource es el método más usado para el enlace de datos a un control de menú porque las fuentes de fuera del archivo Web.sitemap y su esquema proporciona una API conocida para el control de menú. La siguiente lista muestra un simple archivo Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Tenga en cuenta que hay solo un elemento siteMapNode raíz, en este caso, el elemento principal. Varios atributos pueden configurarse para cada siteMapNode. Los atributos utilizados con más frecuencia son:

- **dirección URL** especifica la dirección URL que se muestra cuando un usuario hace clic en el elemento de menú. Si este atributo no está presente, el nodo simplemente registrará cuando hizo clic.
- **título** especifica el texto que se muestra en el elemento de menú.
- **descripción** utilizada como documentación para el nodo. También se muestra como una información sobre herramientas al mantener el mouse sobre el nodo.
- **siteMapFile** permite sitemap anidados. Este atributo debe apuntar a un archivo de mapa del sitio ASP.NET tiene el formato correcto.
- **roles** permite que la apariencia de un nodo se controla mediante el recorte de seguridad ASP.NET.

Tenga en cuenta que si bien estos atributos son opcionales, el comportamiento del menú es posible que no sea el esperado si no se especifican. Por ejemplo, si la *url* se especifica el atributo, pero el *descripción* atributo no es, el nodo no estarán visible y no habrá ninguna manera de navegar a la dirección URL especificada.

## <a name="controlling-a-menus-operation"></a>Controlar una operación de menús

Hay varias propiedades que afectan al funcionamiento de un control de menú de ASP.NET; el **orientación** propiedad, el **DisappearAfter** propiedad, el **StaticItemFormatString** propiedad y el **StaticPopoutImageUrl**propiedad son sólo algunas de ellas.

- El **orientación** puede establecerse en *horizontal* o *vertical* y controla si los elementos de menú estáticos se colocan horizontalmente en una fila o verticalmente y apilan entre sí. Esta propiedad no afecta a los menús dinámicos.
- El **DisappearAfter** propiedad configura cuánto tiempo que debe permanecer visible un menú dinámico después de que el mouse ha sido movido fuera de él. El valor se especifica en milisegundos y el valor predeterminado es 500. Establecer esta propiedad en un valor de -1 hará que el menú para nunca desaparecen automáticamente. En ese caso, el menú sólo desaparecerá cuando el usuario hace clic fuera del menú.
- El **StaticItemFormatString** propiedad facilita mantener coherente palabras en el sistema de menús. Al especificar esta propiedad, *{0}* debe escribirse en lugar de la descripción que aparece en el origen de datos. Por ejemplo, para tener el elemento de menú de ejercicio 1 say visite nuestra página de productos, etc., especificaría visita nuestro {0} página para el StaticItemFormatString. En tiempo de ejecución, ASP.NET reemplazará cualquier aparición de {0} con la descripción del elemento de menú correcta.
- El **StaticPopoutImageUrl** propiedad especifica la imagen que se usa para indicar que un nodo concreto de menú tiene nodos secundarios que pueden tener acceso al mantener el mouse sobre él. Menús dinámicos sigue usando la imagen predeterminada.

## <a name="templated-menu-controls"></a>Controles de menú con plantilla

El control de menú es un control con plantilla y permite dos ItemTemplates diferentes; el StaticItemTemplate y el DynamicItemTemplate. Mediante estas plantillas, puede agregar fácilmente controles de servidor o controles de usuario a los menús.

Para editar las plantillas de Visual Studio. NET, haga clic en el botón de etiqueta inteligente en el menú y elija Editar plantillas. A continuación, puede elegir entre modificar el StaticItemTemplate o el DynamicItemTemplate.

Los controles agregados a la StaticItemTemplate aparecerá en el menú estático cuando se carga la página. Los controles agregados a la DynamicItemTemplate aparecerán en todos los menús emergentes.

## <a name="menu-events"></a>Eventos de menú

El control de menú tiene dos eventos que son exclusivos de ella; el **MenuItemClicked** y **MenuItemDatabound** eventos.

El evento MenuItemClicked se genera cuando se hace clic en un elemento de menú. El evento MenuItemDatabound se genera cuando un elemento de menú está enlazado a datos. El **MenuEventArgs** que se pasa al evento controlador proporciona acceso al elemento de menú mediante la propiedad de elemento.

## <a name="controlling-a-menus-appearance"></a>Controlar una apariencia de los menús

También puede afectar a la apariencia de un control de menú mediante uno o varios de los muchos estilos disponibles para los menús de formato. Entre ellos se encuentran **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**y **DynamicHoverStyle**. Estas propiedades se configuran mediante una cadena de estilo HTML estándar. Por ejemplo, el siguiente afectará el estilo de menús dinámicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si usa cualquiera de los estilos de mantener el mouse, deberá agregar una &lt;head&gt; elemento en la página con el *runat* elemento establecido en *server*.


Controles de menú también admiten el uso de temas de ASP.NET 2.0.

## <a name="the-treeview-control"></a>El Control TreeView

El control TreeView muestra los datos en una estructura de árbol. Al igual que con el control de menú, puede ser fácilmente datos enlazados a cualquier origen de datos jerárquicos como el SiteMapDataSource.

La primera pregunta que los clientes suelen preguntar sobre el control TreeView de ASP.NET 2.0 es o no está relacionado con el WebControl de IE TreeView que estaba disponible para ASP.NET 1.x. No es así. El control TreeView de ASP.NET 2.0 se ha escrito desde el principio y ofrece mejora considerable sobre el TreeView WebControl de Internet Explorer que estaba disponible anteriormente.

No entraré en detalles sobre cómo enlazar un control TreeView a un mapa del sitio cuando se realice en exactamente la misma manera que el control de menú. Sin embargo, el control TreeView presenta algunas diferencias distinct en la forma en que funciona.

De forma predeterminada, un control TreeView aparece totalmente expandido. Para cambiar el nivel de expansión de carga inicial, modifique la **ExpandDepth** propiedad del control. Esto es especialmente importante en los casos donde la vista de árbol está enlazado a datos tras expandir nodos concretos.

## <a name="databinding-the-treeview-control"></a>Enlace de datos del Control TreeView

A diferencia del control de menú, la vista de árbol se presta bien para controlar grandes cantidades de datos. Por lo tanto, además de enlace de datos a un SiteMapDataSource o XMLDataSource, la vista de árbol suele ser datos enlazados a un conjunto de datos u otros datos relacionales. En casos donde se enlaza el control TreeView a grandes cantidades de datos, es mejor enlazar únicamente a los datos que es visibles en el control. Puede enlazar datos a datos adicionales como nodos de TreeView se expanden.

En estos casos, el **PopulateOnDemand** debe establecerse la propiedad de la vista de árbol en *true*. A continuación, deberá proporcionar una implementación para el **TreeNodePopulate** método.

## <a name="data-binding-without-postback"></a>Enlace de datos sin devolución de datos

Tenga en cuenta que cuando se expande un nodo en el ejemplo anterior por primera vez, la página devuelva los datos y se actualiza. Thats no es un problema en este ejemplo, pero se pueden imaginar que podría ser en un entorno de producción con una gran cantidad de datos. Un escenario de mejor sería aquel en el que la vista de árbol se sigue dinámicamente rellenar sus nodos, pero sin una entrada de blog, realizar una copia en el servidor.

Estableciendo el **PopulateNodesFromClient** y **PopulateOnDemand** propiedades en true, el control TreeView ASP.NET llenar de forma dinámica nodos sin una devolución de datos. Cuando se expande el nodo primario, se realiza una solicitud con XMLHttp desde el cliente y desencadena el evento OnTreeNodePopulate. El servidor responde con una isla de datos XML que, a continuación, se usa para datos enlaza los nodos secundarios.

ASP.NET crea dinámicamente el código de cliente que implementa esta funcionalidad. El &lt;script&gt; se generan las etiquetas que contienen la secuencia de comandos que señala a un archivo AXD. Por ejemplo, la siguiente lista muestra los vínculos de la secuencia de comandos para el código de script que genera la solicitud con XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si examinar el archivo AXD anteriormente en el explorador y abrirlo, verá el código que implementa la solicitud con XMLHttp. Este método evita que a los clientes modifiquen el archivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlar el funcionamiento del Control TreeView

El control de vista de árbol tiene varias propiedades que afectan al funcionamiento del control. Las propiedades más obvias son la **ShowCheckBoxes**, **ShowExpandCollapse**, y **ShowLines**.

El **ShowCheckBoxes** nodos muestran una casilla de verificación cuando se representan como si no afecta la propiedad. Los valores válidos para esta propiedad son **ninguno**, **raíz**, **primario**, **hoja**, y **todas**. Estos afectan a un control TreeView como sigue:

| **Valor de propiedad** | **Efecto** |
| --- | --- |
| Ninguna | Las casillas de verificación no se muestran en los nodos. Ésta es la configuración predeterminada. |
| Raíz | Solo se muestra una casilla en el nodo raíz. |
| Elemento primario | Solo se muestra una casilla en los nodos que tienen nodos secundarios. Los nodos secundarios pueden ser nodos primarios o nodos hoja. |
| Hoja | Se muestra una casilla de verificación sólo en aquellos nodos que no tienen nodos secundarios. |
| Todas | Se muestra una casilla en todos los nodos. |

Cuando se utilizan las casillas de verificación, la **CheckedNodes** propiedad devolverá una colección de nodos de la vista de árbol que se comprueban tras la devolución.

El **ShowExpandCollapse** propiedad controla la apariencia de la imagen de expandir o contraer junto a los nodos raíz y el elemento primario. Si esta propiedad se establece en **false**, nodos de la vista de árbol se representan como hipervínculos y son expandido/contraído, haga clic en el vínculo.

El **ShowLines** propiedad controla si se muestran las líneas conectando los nodos primarios a los nodos secundarios. Cuando **false** (valor predeterminado), no se muestran líneas. Cuando **true**, el control TreeView usará imágenes de líneas en la carpeta especificada por el **LineImagesFolder** propiedad.

Para personalizar la apariencia de las líneas de TreeView, Visual Studio .NET 2005 incluye una herramienta de diseñador de la línea. Puede tener acceso a esta herramienta con el botón de etiqueta inteligente en el control de vista de árbol como a continuación.


![](data-bound-controls/_static/image1.jpg)

**Figura 1**


Cuando selecciona el **personalizar imágenes de líneas** opción de menú, se iniciará la herramienta Diseñador de la línea que permite configurar la apariencia de las líneas de TreeView.

## <a name="treeview-events"></a>Eventos de TreeView

El control de vista de árbol tiene los siguientes eventos únicos:

- SelectedNodeChanged se produce cuando se selecciona un nodo según la **SelectAction** propiedad.
- TreeNodeCheckChanged se produce cuando se cambia un estado de casillas de los nodos.
- TreeNodeExpanded se produce cuando se expande un nodo según la **SelectAction** propiedad.
- TreeNodeCollapsed se produce cuando se contrae un nodo.
- TreeNodeDataBound se produce cuando un nodo de datos enlazado.
- TreeNodePopulate se produce cuando un nodo se rellena.

El **SelectAction** propiedad le permite configurar qué evento se desencadena cuando se selecciona un nodo. La propiedad SelectAction proporciona las siguientes acciones:

- TreeNodeSelectAction.Expand provoca TreeNodeExpanded cuando se selecciona el nodo.
- TreeNodeSelectAction.None no provoca ningún evento cuando se selecciona el nodo.
- TreeNodeSelectAction.Select provoca el evento SelectedNodeChanged cuando se selecciona el nodo.
- TreeNodeSelectAction.SelectExpand provoca el evento SelectedNodeChanged y el evento TreeNodeExpanded cuando se selecciona el nodo.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

El control TreeView ofrece muchas de las propiedades para controlar la apariencia del control con estilos. Las propiedades siguientes están disponibles.

| **Nombre de propiedad** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla el estilo de los nodos al mantener el mouse sobre ellos. |
| LeafNodeStyle | Controla el estilo de los nodos hoja. |
| NodeStyle | Controla el estilo de todos los nodos. Estilos de nodo específico (por ejemplo, LeafNodeStyle) reemplazan este estilo. |
| ParentNodeStyle | Controla el estilo de todos los nodos primarios. |
| RootNodeStyle | Controla el estilo para el nodo raíz. |
| SelectedNodeStyle | Controla el estilo del nodo seleccionado. |

Cada una de estas propiedades es de solo lectura. Sin embargo, lo harán cada devolución un **TreeNodeStyle** objeto y las propiedades de ese objeto se pueden modificar mediante la *subpropiedades de la propiedad* formato. Por ejemplo, para establecer el **ForeColor** propiedad de la **SelectedNodeStyle**, usaría la sintaxis siguiente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Tenga en cuenta que no se cierra la etiqueta a la anterior. Eso es porque cuando se usa la sintaxis declarativa que se muestra a continuación, deberá incluir los nodos de TreeView en el código HTML también.

También se pueden especificar las propiedades de estilo en el código mediante el *property.subproperty* formato. Por ejemplo, para establecer el **ForeColor** propiedad de la **RootNodeStyle** en código, usaría la sintaxis siguiente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obtener una lista completa de las propiedades de estilo diferente, consulte la documentación de MSDN en el objeto TreeNodeStyle.


## <a name="the-sitemappath-control"></a>El Control SiteMapPath

El control SiteMapPath proporciona un control de navegación de pan "crumb" para los desarrolladores de ASP.NET. Al igual que los controles de navegación, puede ser fácilmente datos enlazados a datos jerárquicos orígenes como XmlDataSource o SiteMapDataSource.

Un control SiteMapPath se compone de objetos SiteMapNodeItem. Hay tres tipos de nodos; el nodo raíz, los nodos primarios y el nodo actual. El nodo raíz es el nodo en la parte superior de la estructura jerárquica. El nodo actual representa la página actual. Todos los demás nodos son nodos primarios.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlar el funcionamiento del Control SiteMapPath

Las propiedades que controlan el funcionamiento del control SiteMapPath son como sigue:

| **Property** | **Descripción de propiedad** |
| --- | --- |
| ParentLevelsDisplayed | Controla cuántos nodos primarios se muestran. El valor predeterminado es -1 que no se impone ninguna restricción sobre el número de nodos primarios mostrados. |
| PathDirection | Controla la dirección de SiteMapPath. Los valores válidos son RootToCurrent (valor predeterminado) y CurrentToRoot. |
| PathSeparator | Una cadena que controla el carácter que separa los nodos de un control SiteMapPath. El valor predeterminado es:. |
| RenderCurrentNodeAsLink | Un valor booleano que controla si el nodo actual se representa como un vínculo. El valor predeterminado es False. |
| SkipLinkText | Ayuda con la accesibilidad cuando se ve la página de los lectores de pantalla. Esta propiedad permite que los lectores de pantalla omitan el control SiteMapPath. Para deshabilitar esta característica, establezca la propiedad String.Empty. |

## <a name="templated-sitemappath-controls"></a>Controles SiteMapPath con plantilla

El SiteMapControl es un control con plantilla y, por lo tanto, puede definir diferentes plantillas para su uso en Mostrar el control. Para editar las plantillas en un control SiteMapPath, haga clic en el botón de etiqueta inteligente del control y elija Editar plantillas en el menú. Esto muestra el menú de SiteMapTasks tal como se muestra a continuación donde puede elegir entre las diferentes plantillas disponibles.


![](data-bound-controls/_static/image2.jpg)

**Figura 2**


El **NodeTemplate** plantilla hace referencia a cualquier nodo de SiteMapPath. Si el nodo es un nodo raíz o el nodo actual y un **RootNodeTemplate** o **CurrentNodeTemplate** está configurado, se invalida el NodeTemplate.

## <a name="sitemappath-events"></a>Eventos de SiteMapPath

El control SiteMapPath tiene dos eventos que no se derivan de la clase del Control; el **ItemCreated** eventos y el **ItemDataBound** eventos. El evento ItemCreated se genera cuando se crea un elemento SiteMapPath. ItemDataBound se genera cuando se llama al método DataBind durante el enlace de datos de un nodo de SiteMapPath. Un **SiteMapNodeItemEventArgs** objeto proporciona acceso a la SiteMapNodeItem específico a través de la propiedad del elemento.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

Los estilos siguientes están disponibles para el formato de un control SiteMapPath.

| **Nombre de propiedad** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla el estilo del texto del nodo actual. |
| RootNodeStyle | Controla el estilo del texto para el nodo raíz. |
| NodeStyle | Controla el estilo del texto para todos los nodos, suponiendo que no se aplica un CurrentNodeStyle o RootNodeStyle. |

La propiedad NodeStyle se reemplaza por el CurrentNodeStyle o el RootNodeStyle. Cada una de estas propiedades es de solo lectura y devuelve un **estilo** objeto. Para modificar la apariencia de un nodo mediante una de estas propiedades, deberá establecer las propiedades del objeto Style que se devuelve. Por ejemplo, el código siguiente cambia la propiedad de color de primer plano del nodo actual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

También se puede aplicar la propiedad mediante programación como sigue:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si se aplica una plantilla, no se aplicará el estilo.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratorio 1: Configuración de un Control de menú de ASP.NET

1. Cree un nuevo sitio Web.
2. Agregue un archivo de mapa del sitio, seleccione archivo, nuevo, archivo y elija mapa del sitio en la lista de plantillas de archivos.
3. Abra el mapa del sitio (Web.sitemap de forma predeterminada) y modifíquela para que se parece a la lista siguiente. Las páginas a la que está vinculando en el archivo de asignación de sitio no existe realmente, pero que no es un problema para este ejercicio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra el formulario de Web predeterminado en la vista Diseño.
5. En la sección de navegación del cuadro de herramientas, agregue un nuevo control Menu a la página.
6. En la sección datos del cuadro de herramientas, agregue un SiteMapDataSource nuevo. El SiteMapDataSource utilizará automáticamente el archivo Web.sitemap en su sitio. (El archivo Web.sitemap *debe* estar en la carpeta raíz del sitio.)
7. Haga clic en el control de menú y, a continuación, haga clic en el botón de etiqueta inteligente para mostrar el cuadro de diálogo de tareas de menú.
8. En la lista desplegable Elegir origen de datos, seleccione SiteMapDataSource1.
9. Haga clic en el vínculo de Autoformato y elija un formato para el menú.
10. En el panel Propiedades, establezca la **StaticDisplayLevels** propiedad en 2. El control de menú ahora debe mostrar el nodo principal, productos y servicios en el diseñador.
11. Vaya a la página en el explorador para usar el menú. (Dado que las páginas que se ha agregado a la asignación de sitio no existen en realidad, verá un error cuando intente ir a ellas.)

Probar a cambiar el StaticDisplayLevels y las propiedades MaximumDynamicDisplayLevels y ver cómo afectan a cómo se representa el menú.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Práctica 2: Enlazar dinámicamente un Control TreeView

Este ejercicio se da por supuesto que tiene SQL Server que se ejecuta localmente y que la base de datos Northwind está presente en la instancia de SQL Server. Si no se cumplen estas condiciones, cambie la cadena de conexión en el ejemplo. Tenga en cuenta que también es posible que deba especificar la autenticación de SQL Server en lugar de una conexión de confianza.

1. Cree un nuevo sitio Web.
2. Cambiar a vista de código para Default.aspx y reemplace todo el código con el código que aparece a continuación. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Guarde la página como treeview.aspx.
4. Vaya a la página.
5. Cuando la página se muestra por primera vez, vea el origen de la página en el explorador. Tenga en cuenta que solo los nodos visibles se enviaron al cliente.
6. Haga clic en el signo más junto a los nodos.
7. Ver código fuente en la página de nuevo. Tenga en cuenta que los nodos recién mostrados están presentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratorio 3: Detalles ver y editar datos mediante un GridView y DetailsView

1. Cree un nuevo sitio Web.
2. Agregue un archivo web.config nuevo al sitio Web.
3. Agregue una cadena de conexión al archivo web.config como se muestra a continuación: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Es posible que deba cambiar la cadena de conexión en función de su entorno.
4. Guarde y cierre el archivo web.config.
5. Abra el archivo Default.aspx y agregue un nuevo control SqlDataSource.
6. Cambie el identificador del control SqlDataSource para **productos**.
7. En el **SqlDataSource tareas** menú, haga clic en **Configurar origen de datos**.
8. Seleccione **Northwind** en la lista desplegable de la conexión y haga clic en siguiente.
9. Seleccione **productos** desde el **nombre** lista desplegable y compruebe el **ProductID**, **ProductName**, **UnitPrice**, y **UnitsInStock** casillas, tal como se muestra a continuación. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Haga clic en **Siguiente**.
11. Haga clic en **Finalizar**.
12. Cambie a la vista de origen y examine el código que se generó. Tenga en cuenta la **SelectCommand**, **DeleteCommand**, **InsertCommand**, y **UpdateCommand** que se agregaron de SqlDataSource control. Tenga en cuenta también los parámetros que se han agregado.
13. Cambie a la vista Diseño y agregar un nuevo control GridView a la página.
14. Seleccione **productos** desde el **Elegir origen de datos** lista desplegable.
15. Comprobar **Habilitar paginación** y **Habilitar selección** tal como se muestra a continuación. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Haga clic en el **Edit Columns** vincular y asegúrese de que **generar campos automáticamente** está activada.
17. Haga clic en **Aceptar**.
18. Con el control GridView seleccionado, haga clic en el botón junto a la **DataKeyNames** propiedad en el panel Propiedades.
19. Seleccione **ProductID** desde el **campos de datos disponibles** lista y haga clic en el **&gt;** botón para agregarlo.
20. Haga clic en Aceptar.
21. Agregar un nuevo control SqlDataSource a la página.
22. Cambie el identificador del control SqlDataSource para **detalles**.
23. En el menú tareas de SqlDataSource, elija **Configurar origen de datos**.
24. Elija **Northwind** desde la lista desplegable y haga clic en **siguiente**.
25. Seleccione <strong>productos</strong> desde el <strong>nombre</strong> lista desplegable y compruebe el <strong> \</strong > * checkbox en el <strong>columnas</strong> listbox.
26. Haga clic en el **donde** botón.
27. Seleccione **ProductID** desde el **columna** lista desplegable.
28. Seleccione **=** en la lista desplegable operador.
29. Seleccione **Control** desde el **origen** lista desplegable.
30. Seleccione **GridView1** desde el **Id. de Control** lista desplegable.
31. Haga clic en el **agregar** para agregar la cláusula WHERE.
32. Haga clic en **Aceptar**.
33. Haga clic en el **avanzadas** botón y compruebe el **generar instrucciones INSERT, UPDATE y DELETE** casilla de verificación.
34. Haga clic en **Aceptar**.
35. Haga clic en **siguiente** y haga clic en **finalizar**.
36. Agregue un control DetailsView en la página.
37. En el **Elegir origen de datos** lista desplegable, elija **detalles**.
38. Compruebe el **Habilitar edición** casilla como se muestra a continuación. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Guarde la página y examinar Default.aspx.
40. Haga clic en el **seleccione** vínculo situado junto a diferentes registros para ver la actualización DetailsView automáticamente.
41. Haga clic en el **editar** vínculo en el control DetailsView.
42. Realice un cambio en el registro y haga clic en **actualización**.
