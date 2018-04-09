---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles enlazados a datos | Documentos de Microsoft
author: microsoft
description: La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de interactivas w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 5c3f6aad4b87450149189352e86106f46c765fb8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="data-bound-controls"></a>Controles enlazados a datos
====================
por [Microsoft](https://github.com/microsoft)

> La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de la interacción con los datos de aplicaciones Web dinámicas. ASP.NET 2.0 presenta algunas mejoras sustanciales en controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y la sintaxis declarativa.


La mayoría de las aplicaciones de ASP.NET se basan en cierto grado de presentación de los datos de un origen de datos back-end. Controles enlazados a datos han sido una parte fundamental de la interacción con los datos de aplicaciones Web dinámicas. ASP.NET 2.0 presenta algunas mejoras sustanciales en controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y la sintaxis declarativa.

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

La clase DataBoundControl es una clase abstracta (marcada como MustInherit en VB) utilizada para interactuar con tabulares o datos de estilo de lista. Los controles siguientes son algunos de los controles que derivan de DataBoundControl.

## <a name="adrotator"></a>AdRotator

El control AdRotator permite mostrar una pancarta de gráficos en una página Web que está vinculada a una dirección URL específica. Gira el gráfico que se muestra mediante propiedades para el control. Se puede configurar la frecuencia de un anuncio determinado que se muestra en una página con el **impresiones** propiedad y los anuncios se pueden filtrar mediante el filtrado de palabras clave.

Controles AdRotator utilizan un archivo XML o una tabla en una base de datos para los datos. Los siguientes atributos se usan en archivos XML para configurar el control AdRotator.

### <a name="imageurl"></a>ImageUrl
La dirección URL de una imagen para mostrar para el anuncio.

### <a name="navigateurl"></a>NavigateUrl
La dirección URL que el usuario debe realizarse para cuando se hace clic en el anuncio. Esto debe ser la dirección URL codificada.

### <a name="alternatetext"></a>AlternateText
El texto alternativo que se muestra en una información sobre herramientas y leído por lectores de pantalla. También se muestra cuando la imagen especificada por ImageUrl no está disponible.

### <a name="keyword"></a>Palabra clave
Define una palabra clave que se puede usar cuando se usa el filtrado de palabras clave. Si se especifica, se mostrará sólo los anuncios con una palabra clave que coincida con el filtro de palabra clave.

### <a name="impressions"></a>Impresiones
Número de ponderación que determina la frecuencia con la que es probable que aparezca un anuncio determinado. Es en relación con la impresión de otros anuncios en el mismo archivo. El valor máximo de las impresiones colectivos de todos los anuncios en un archivo XML es 2,048,000,000 1.

### <a name="height"></a>Alto
El alto del anuncio en píxeles.

### <a name="width"></a>Ancho
El ancho del anuncio en píxeles.


> [!NOTE]
> Los atributos de alto y ancho invalidan el alto y ancho para el control AdRotator en Sí.


Un archivo XML típico podría ser similar al siguiente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

En el ejemplo anterior, el anuncio de Contoso es dos veces como probable que aparezcan como ad para el sitio Web de ASP.NET debido al valor del atributo de impresiones.

Para mostrar anuncios del archivo XML anterior, agregar un control AdRotator a una página y establezca el **AdvertisementFile** propiedad para que apunte al archivo XML, tal y como se muestra a continuación:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si decide usar una tabla de base de datos como origen de datos para el control AdRotator, primero deberá configurar una base de datos mediante el siguiente esquema:

| **Nombre de columna** | **Tipo de datos** | **Descripción** |
| --- | --- | --- |
| Id. | int | Clave principal. Esta columna puede tener cualquier nombre. |
| ImageUrl | nvarchar (*longitud*) | Dirección URL absoluta o relativa de la imagen para mostrar para el anuncio. |
| NavigateUrl | nvarchar (*longitud*) | La dirección URL de destino para el anuncio. Si no proporciona un valor, el anuncio no será un hipervínculo. |
| AlternateText | nvarchar (*longitud*) | El texto que se muestra si no se encuentra la imagen. En algunos exploradores, el texto se muestra como una información sobre herramientas. Texto alternativo también se usa para mejorar la accesibilidad para que los usuarios que no se pueden ver el gráfico puedan escuchar su descripción. |
| Palabra clave | nvarchar (*longitud*) | Una categoría para el anuncio en el que puede filtrar la página. |
| Impresiones | int(4) | Un número que indica la probabilidad de que la frecuencia con la que se muestra el anuncio. Cuanto mayor sea el número, más a menudo el anuncio se mostrará. El total de todos los valores de impresiones en el archivo XML no puede superar los superior a 2.048.000.000-1. |
| Ancho | int(4) | El ancho de la imagen en píxeles. |
| Alto | int(4) | El alto de la imagen en píxeles. |

En casos donde ya tiene una base de datos con un esquema diferente, puede usar el **AlternateTextField**, **ImageUrlField**, y **NavigateUrlField** propiedades para asignar el Atributos de AdRotator a la base de datos existente. Para mostrar los datos de la base de datos en el control AdRotator, agregue un control de origen de datos a la página, configurar la cadena de conexión para el control de origen de datos para que apunte a la base de datos y establecer el control AdRotator **DataSourceID** propiedad para el identificador del control de origen de datos. En situaciones en las que hay una necesidad de configurar AdRotator anuncios mediante programación, utilice el evento AdCreated. El evento AdCreated toma dos parámetros; un objeto de uno y el otro es una instancia de AdCreatedEventArgs. El AdCreatedEventArgs es una referencia al anuncio que se va a crear.

El siguiente fragmento de código establece la propiedad ImageUrl, NavigateUrl y AlternateText para un anuncio mediante programación:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Controles de lista incluyen ListBox, DropDownList, CheckBoxList, RadioButtonList y BulletedList. Cada uno de estos controles puede ser datos enlazados a un origen de datos. Se usa un campo del origen de datos como texto para mostrar y, opcionalmente, puede usar un segundo campo como el valor de un elemento. Los elementos también pueden agregar de forma estática en tiempo de diseño y se pueden mezclar elementos estáticos y dinámicos elementos agregados desde un origen de datos.

Para datos de enlazar un control de lista, agregue un control de origen de datos a la página. Especifique un comando SELECT para el control de origen de datos y, a continuación, establecer la propiedad DataSourceID del control de lista para el identificador del control de origen de datos. Use la **DataTextField** y **DataValueField** propiedades para definir el texto para mostrar y el valor para el control. Además, puede usar el **DataTextFormatString** propiedad para controlar la apariencia del texto de presentación como sigue:

| **Expresión** | **Descripción** |
| --- | --- |
| Precio: {0:C} | Para datos numéricos y decimales. Muestra el literal "precio:" seguido de números en formato de moneda. El formato de moneda depende de la configuración de referencia cultural especificada en el atributo de referencia cultural en el **página** directiva o en el archivo Web.config. |
| {0:D4} | Para datos enteros. No se puede usar con números decimales. Números enteros se muestran en un campo con ceros que tiene cuatro caracteres de ancho. |
| {0:N2}% | Para los datos numéricos. Muestra el número con 2 posiciones decimales precisión seguido del literal "%". |
| {0:000.0} | Para datos numéricos y decimales. Los números se redondean a una posición decimal. Los números de menos de tres dígitos se rellenan con ceros. |
| {0:D} | Para los datos de fecha y hora. Formato de fecha larga de muestra ("jueves, 06 de agosto de 1996"). El formato de fecha depende de la configuración de la referencia cultural de la página o del archivo Web.config. |
| {0:d} | Para los datos de fecha y hora. Formato de fecha corta de muestra ("12/31/99"). |
| {0:yy-MM-dd} | Para los datos de fecha y hora. Muestra la fecha en formato numérico de año-mes-día (96-08-06) |

## <a name="gridview"></a>GridView

El control GridView permite mostrar datos tabulares y edición usando un enfoque declarativo y es el sucesor del control DataGrid. Las siguientes características están disponibles en el control GridView.

- Enlazar a datos los controles de origen, como SqlDataSource.
- Funciones de ordenación integradas.
- Actualización y eliminación de funciones integradas.
- Funciones integradas de paginación.
- Funciones de selección de fila integradas.
- Acceso mediante programación al modelo de objetos de GridView para establecer propiedades dinámicamente, controlar los eventos y así sucesivamente.
- Varios campos de clave.
- Varios campos de datos para las columnas de hipervínculo.
- Personalización del aspecto mediante temas y estilos.

**Campos de columna**

Cada columna en el control GridView se representa mediante un objeto DataControlField. De forma predeterminada, se establece la propiedad AutoGenerateColumns en **true**, que crea un objeto AutoGeneratedField para cada campo del origen de datos. Cada campo se representa como una columna en el control GridView en el orden en que cada campo aparece en el origen de datos. Puede controlar manualmente qué campos de columna aparecen en la **GridView** control estableciendo la **AutoGenerateColumns** propiedad **false** y, a continuación, definir las suyas propias colección de campos de columna. Tipos de campo de otra columna determinan el comportamiento de las columnas en el control.

En la tabla siguiente se enumera los tipos de campo de otra columna que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos. Este es el tipo de columna predeterminado del control GridView. |
| ButtonField | Muestra un botón de comando para cada elemento en el control GridView. Esto le permite crear una columna de controles de botón personalizado, como el botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla de verificación para cada elemento en el control GridView. Este tipo de campo de columna se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra los botones de comando para realizar la selección, editar o eliminar operaciones predefinidos. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de columna permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen para cada elemento en el control GridView. |
| TemplateField | Muestra el contenido definido por el usuario para cada elemento en el control GridView según una plantilla especificada. Este tipo de campo de columna permite crear un campo de columna personalizado. |

Para definir una colección de campos de columna mediante declaración, primero agregue de apertura y cierre **&lt;columnas&gt;** etiquetas entre las etiquetas apertura y cierre del control GridView. A continuación, mostrar los campos de columna que se van a incluir entre la apertura y cierre **&lt;columnas&gt;** etiquetas. Las columnas especificadas se agregan a la colección de columnas en el orden indicado. El **columnas** colección almacena todos los campos en el control de la columna y le permite administrar mediante programación los campos de columna en el control GridView.

Campos de columna declarados explícitamente se pueden mostrar en combinación con los campos de columna generada automáticamente. Cuando se utilizan ambos, los campos de columna declarados explícitamente se representan en primer lugar, seguido de los campos de columna generada automáticamente.

## <a name="binding-to-data"></a>Enlace a datos

El control GridView que puede enlazarse a un control de origen de datos (como **SqlDataSource**, **ObjectDataSource**, etc.), así como cualquier otro origen de datos que implementa el System.Collections.IEnumerable interfaz (como System.Data.DataView, System.Collections.ArrayList o System.Collections.Hashtable). Utilice uno de los métodos siguientes para enlazar el control GridView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control GridView en el valor de identificador del control de origen de datos. El control GridView automáticamente enlaza con el control de origen de datos especificado y puede sacar partido del origen de datos funciones del control para realizar la ordenación, actualización, eliminación y paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa la interfaz System.Collections.IEnumerable, establecer mediante programación la propiedad de origen de datos del control GridView al origen de datos y, a continuación, llame al método de enlazar datos. Cuando se usa este método, el control GridView no proporciona ordenación, actualizar, eliminar y funcionalidad de paginación integrada. Debe proporcionar esta funcionalidad por sí mismo.

## <a name="data-operations"></a>Operaciones de datos

El control GridView proporciona muchas funciones integradas que permiten al usuario ordenar, actualizar, eliminar, seleccionar y desplazarse a través de los elementos del control. Cuando el control GridView se enlaza a un control de origen de datos, el control GridView puede aprovechar el origen de datos de las capacidades del control y proporcionar automática ordenar, actualizar y eliminar funciones.

> [!NOTE]
> El control GridView puede proporcionar compatibilidad para ordenar, actualizar y eliminar con otros tipos de orígenes de datos; Sin embargo, debe proporcionar un controlador de eventos adecuado con la implementación de estas operaciones.


Ordenación permite al usuario ordenar los elementos en el control GridView con respecto a una columna específica, haga clic en el encabezado de la columna. Para habilitar la ordenación, establezca la propiedad AllowSorting en **true**.

Las funcionalidades de actualización, eliminación y selección automática se habilitan cuando un botón en un **ButtonField** o **TemplateField** campo de la columna, con un nombre de comando de "Editar", "Delete" y "Select", respectivamente, hacer clic en. El control GridView puede agregar automáticamente una **CommandField** campo de la columna con una edición, eliminación o botón Seleccionar si se establece la propiedad AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton en **true**, respectivamente.

> [!NOTE]
> Insertar registros en el origen de datos no es directamente compatible con el control GridView. Sin embargo, es posible insertar registros mediante el control GridView junto con DetailsView o FormView control.


En lugar de mostrar todos los registros en el origen de datos al mismo tiempo, el control GridView puede automáticamente dividir los registros en las páginas. Para habilitar la paginación, establezca la propiedad AllowPaging **true**.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control GridView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control GridView. Cuando se establece esta propiedad, las filas de datos se muestran alternando la configuración de RowStyle y **AlternatingRowStyle** configuración. |
| EditRowStyle | La configuración de estilo para la fila que se está editando en el control GridView. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el control GridView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control GridView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control GridView. |
| PagerStyle | La configuración de estilo para la fila del localizador del control GridView. |
| RowStyle | La configuración de estilo para las filas de datos en el control GridView. Cuando el **AlternatingRowStyle** también se establece la propiedad, las filas de datos se muestran alternando entre el **RowStyle** configuración y la **AlternatingRowStyle** configuración. |
| SelectedRowStyle | La configuración de estilo para la fila seleccionada en el control GridView. |

También puede mostrar u ocultar las diferentes partes del control. En la tabla siguiente se enumera las propiedades que controlan qué partes se muestran u ocultas.

| **Property** | **Descripción** |
| --- | --- |
| ShowFooter | Muestra u oculta la sección de pie de página del control GridView. |
| ShowHeader | Muestra u oculta la sección de encabezado del control GridView. |

### <a name="events"></a>Eventos

El control GridView proporciona varios eventos que se pueden programar. Esto permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control GridView.

| **Event** | **Descripción** |
| --- | --- |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero después de que el control GridView administra la operación de paginación. Este evento suele utilizarse cuando necesite realizar una tarea después de que el usuario se desplaza a otra página en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero antes de GridView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |
| RowCancelingEdit | Se produce cuando se hace clic en el botón de cancelación de una fila, pero antes de que el control GridView sale del modo de edición. Este evento suele utilizarse para detener la operación de cancelación. |
| RowCommand | Se produce cuando se hace clic en un botón en el control GridView. Este evento se utiliza a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| RowCreated | Se produce cuando se crea una nueva fila en el control GridView. Este evento se utiliza a menudo para modificar el contenido de una fila cuando se creó la fila. |
| RowDataBound | Se produce cuando una fila de datos se enlaza a datos en el control GridView. Este evento se utiliza a menudo para modificar el contenido de una fila cuando la fila está enlazada a datos. |
| RowDeleted | Se produce cuando se hace clic en el botón de eliminación de una fila, pero después de que el control GridView elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| RowDeleting | Se produce cuando se hace clic en el botón de eliminación de una fila, pero antes de GridView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| RowEditing | Se produce cuando se hace clic en el botón de edición de una fila, pero antes de GridView control entra en modo de edición. Este evento se utiliza a menudo para cancelar la operación de edición. |
| RowUpdated | Se produce cuando se hace clic en el botón de actualización de una fila, pero después de que el control GridView actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| RowUpdating | Se produce cuando se hace clic en el botón de actualización de una fila, pero antes de GridView control actualiza la fila. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| SelectedIndexChanged | Se produce cuando se hace clic en el botón de selección de una fila, pero después de que el control GridView administra la operación de selección. Este evento se utiliza a menudo para realizar una tarea después de que se selecciona una fila en el control. |
| SelectedIndexChanging | Se produce cuando se hace clic en el botón de selección de una fila, pero antes de GridView control administra la operación de selección. Este evento se utiliza a menudo para cancelar la operación de selección. |
| Ordenar | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero después de que el control GridView administra la operación de ordenación. Este evento normalmente se usa para realizar una tarea después de que el usuario hace clic en un hipervínculo para ordenar una columna. |
| Ordenar | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero antes de GridView control administra la operación de ordenación. Este evento suele utilizarse para cancelar la operación de ordenación o realizar una rutina de ordenación personalizada. |

## <a name="formview"></a>FormView

El control FormView se utiliza para mostrar un único registro de un origen de datos. Es similar al control DetailsView, exceptuando que muestra plantillas definidas por el usuario en lugar de los campos de fila. Crear sus propias plantillas le ofrece mayor flexibilidad para controlar cómo se muestran los datos. El control FormView admite las siguientes características:

- Enlazar a datos los controles de origen, como SqlDataSource y ObjectDataSource.
- Funciones de inserción integradas.
- Actualización y eliminación de funciones integradas.
- Funciones integradas de paginación.
- Acceso mediante programación al modelo de objetos de FormView para establecer propiedades dinámicamente, controlar los eventos y así sucesivamente.
- Personalización del aspecto mediante plantillas definidas por el usuario, temas y estilos.

## <a name="templates"></a>Plantillas

Para que el control FormView mostrar el contenido, debe crear plantillas para las distintas partes del control. Mayoría de las plantillas es opcional; Sin embargo, debe crear una plantilla para el modo en que se configure el control. Por ejemplo, un control FormView que admite la inserción de registros debe tener definida una plantilla de elemento de inserción. En la tabla siguiente se enumera las diferentes plantillas que puede crear.

| **Tipo de plantilla** | **Descripción** |
| --- | --- |
| EditItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de edición. Esta plantilla normalmente contiene controles de entrada y botones de comando con el que el usuario puede editar un registro existente. |
| EmptyDataTemplate | Define el contenido de la fila de datos vacía mostrada cuando se enlaza el control FormView a un origen de datos que no contiene ningún registro. Normalmente, esta plantilla contiene contenido para alertar al usuario que el origen de datos no contiene ningún registro. |
| FooterTemplate | Define el contenido de la fila de pie de página. Esta plantilla normalmente contiene cualquier contenido adicional que le gustaría mostrar en la fila de pie de página. Como alternativa, puede especificar simplemente texto que se va a mostrar en la fila de pie de página estableciendo la propiedad FooterText. |
| HeaderTemplate | Define el contenido de la fila de encabezado. Esta plantilla normalmente contiene cualquier contenido adicional que le gustaría mostrar en la fila de encabezado. Como alternativa, puede especificar simplemente texto que se va a mostrar en la fila de encabezado estableciendo la propiedad HeaderText. |
| ItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de solo lectura. Normalmente, esta plantilla contiene contenido para mostrar los valores de un registro existente. |
| InsertItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de inserción. Esta plantilla normalmente contiene controles de entrada y botones de comando con el que el usuario puede agregar un nuevo registro. |
| PagerTemplate | Define el contenido de la fila del localizador mostrada cuando está habilitada la característica de paginación (cuando se establece la propiedad AllowPaging en **true**). Esta plantilla normalmente contiene controles con los que el usuario pueda ir a otro registro. |

Controles de entrada en la plantilla de elementos de edición y la plantilla de elemento de inserción se pueden enlazar a los campos de un origen de datos mediante una expresión de enlace bidireccional. Esto permite al control FormView automáticamente extraer los valores del control de entrada para una actualización o la operación de inserción. Expresiones de enlace bidireccionales también permiten controles de entrada en una plantilla de elementos de edición para mostrar automáticamente los valores de campo original.

### <a name="binding-to-data"></a>Enlace a datos

El control FormView puede enlazarse a un control de origen de datos (como **SqlDataSource**, AccessDataSource, **ObjectDataSource** y así sucesivamente), o a cualquier origen de datos que implementa la Interfaz System.Collections.IEnumerable (por ejemplo, System.Data.DataView, System.Collections.ArrayList y System.Collections.Hashtable). Utilice uno de los métodos siguientes para enlazar el control FormView para el tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control FormView para el valor de identificador del control de origen de datos. El control FormView automáticamente enlaza con el control de origen de datos especificado y puede sacar partido del origen de datos funciones del control para realizar Insertar, actualizar, eliminar y funcionalidad de paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** de interfaz, establecer la propiedad de origen de datos del control FormView mediante programación al origen de datos y, a continuación, llame al método de enlazar datos. Cuando se usa este método, el control FormView no proporciona integradas de inserción, actualización, eliminación y funcionalidad de paginación. Debe proporcionar esta funcionalidad mediante el evento adecuado.

## <a name="data-operations"></a>Operaciones de datos

El control FormView proporciona muchas funciones integradas que permiten al usuario actualizar, eliminar, insertar y desplazarse a través de los elementos del control. Cuando el control FormView se enlaza a un control de origen de datos, el control FormView puede aprovechar el origen de datos de las capacidades del control y proporcionar actualizar, eliminar, insertar y funcionalidad de paginación automática. El control FormView puede proporcionar compatibilidad de actualización, eliminación, inserción y las operaciones de paginación con otros tipos de orígenes de datos; Sin embargo, debe proporcionar un controlador de eventos adecuado con la implementación de estas operaciones.

Dado que el control FormView utiliza plantillas, no proporciona una manera de generar automáticamente botones de comando para llevar a cabo la actualización, elimina o las operaciones de inserción. Debe incluir manualmente estos botones de comando en la plantilla adecuada. El control FormView reconoce ciertos botones que tienen sus **CommandName** se establecen en valores específicos. En la tabla siguiente se muestra los botones de comando que reconoce el control de FormView.

| **Button** | **Valor de CommandName** | **Descripción** |
| --- | --- | --- |
| Cancelar | "Cancelar" | Utilizado para actualizar o las operaciones de inserción para cancelar la operación y descartar los valores escritos por el usuario. El control de FormView, a continuación, vuelve al modo especificado por la propiedad DefaultMode. |
| Eliminar | “Eliminar” | Se utiliza en las operaciones de eliminación para eliminar el registro mostrado del origen de datos. Genera los eventos ItemDeleting y ItemDeleted. |
| Editar | "Edit" | Se utiliza en las operaciones de actualización para colocar el control FormView en modo de edición. El contenido especificado en el **EditItemTemplate** propiedad se muestra para la fila de datos. |
| Insertar | "Insertar" | Se utiliza en las operaciones de inserción para intentar insertar un nuevo registro en el origen de datos utilizando los valores proporcionados por el usuario. Genera los eventos ItemInserting y ItemInserted. |
| Nuevo | "Nuevo" | Se utiliza en las operaciones de inserción para colocar el control FormView en modo de inserción. El contenido especificado en el **InsertItemTemplate** propiedad se muestra para la fila de datos. |
| Página | "Página" | Se utiliza en operaciones de paginación para representar un botón de la fila del localizador que realiza la paginación. Para especificar la operación de paginación, establezca la **CommandArgument** propiedad del botón en "Siguiente", "Prev", "First", "Último" o el índice de la página que se va a navegar. Genera los eventos PageIndexChanging y PageIndexChanged. |
| Actualizar | "Actualización" | Se utiliza en las operaciones de actualización para intentar actualizar el registro mostrado en el origen de datos con los valores proporcionados por el usuario. Genera los eventos ItemUpdating y ItemUpdated. |

A diferencia de la eliminación botón (que elimina el registro mostrado inmediatamente), cuando se hace clic en el botón Editar o nuevo, el control pasa a la edición de FormView o respectivamente, el modo de inserción. En el modo de edición, el contenido se incluye en el **EditItemTemplate** propiedad se muestra para el elemento de datos actual. Normalmente, la plantilla de elemento de edición se define tal que el botón de edición se reemplaza con una actualización y un botón de cancelación. Controles de entrada que son adecuados para el tipo de datos del campo (por ejemplo, un cuadro de texto o un control de casilla de verificación) normalmente también se muestran con el valor de un campo para el usuario modificar. Haga clic en el botón de actualización actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios.

Del mismo modo, el contenido de la **InsertItemTemplate** propiedad se muestra para el elemento de datos cuando el control está en modo de inserción. La plantilla de elementos de inserción normalmente se define tal que el botón nuevo se reemplaza por una instrucción Insert y un botón Cancelar y controles de entrada vacíos se muestran para que el usuario especificar los valores para el nuevo registro. Haga clic en el botón Insertar inserta el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios.

El control FormView proporciona una característica de paginación, lo que permite al usuario navegar a otros registros del origen de datos. Cuando se habilita, se muestra una fila del localizador en el control FormView que contiene los controles de navegación de páginas. Para habilitar la paginación, establezca la **AllowPaging** propiedad **true**. Puede personalizar la fila del localizador estableciendo las propiedades de los objetos contenidos en el PagerStyle y la propiedad PagerSettings. En lugar de utilizar la fila del localizador integrada interfaz de usuario, puede crear su propia interfaz de usuario mediante la **PagerTemplate** propiedad.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control FormView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| EditRowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el control FormView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control FormView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control FormView. |
| InsertRowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila del localizador mostrada en el control FormView cuando está habilitada la característica de paginación. |
| RowStyle | La configuración de estilo para la fila de datos cuando el control FormView está en modo de solo lectura. |

## <a name="events"></a>Eventos

El control FormView proporciona varios eventos que se pueden programar. Esto permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control de FormView.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón dentro de un control de FormView. Este evento se utiliza a menudo para realizar una tarea cuando se hace clic en un botón en el control. |
| ItemCreated | Se produce después de que todos los objetos de FormViewRow se crean en el control de FormView. Este evento se utiliza a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando un botón Eliminar (un botón con su **CommandName** propiedad establecida en "Delete") se hace clic, pero después de que el control FormView elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de FormView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando un botón Insertar (un botón con su **CommandName** propiedad establecida en "Insert") se hace clic, pero después de que el control FormView inserta el registro. Este evento se utiliza a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de FormView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando un botón Actualizar (un botón con su **CommandName** propiedad establecida en "Actualización") se hace clic, pero después de que el control FormView actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de FormView control actualiza el registro. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control FormView cambia los modos (para edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para realizar una tarea cuando el control FormView cambia los modos. |
| ModeChanging | Se produce antes de que el control FormView cambia los modos (para edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero después de que el control FormView administra la operación de paginación. Este evento suele utilizarse cuando necesite realizar una tarea después de que el usuario se desplaza a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero antes de FormView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="detailsview"></a>DetailsView

El control DetailsView se utiliza para mostrar un único registro de un origen de datos en una tabla, donde cada campo del registro se muestra en una fila de la tabla. Se puede utilizar en combinación con un control GridView para escenarios de principal-detalle. El control DetailsView admite las siguientes características:

- Enlazar a datos los controles de origen, como SqlDataSource.
- Funciones de inserción integradas.
- Actualización y eliminación de funciones integradas.
- Funciones integradas de paginación.
- Acceso mediante programación al modelo de objetos de DetailsView para establecer propiedades dinámicamente, controlar los eventos y así sucesivamente.
- Personalización del aspecto mediante temas y estilos.

## <a name="row-fields"></a>Campos de fila

Cada fila de datos en el control DetailsView se crea mediante la declaración de un control de campo. Tipos de campos de fila diferente determinan el comportamiento de las filas en el control. Controles de campo se derivan de DataControlField. En la tabla siguiente se enumera los tipos de campos de fila diferente que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos como texto. |
| ButtonField | Muestra un botón de comando en el control DetailsView. Esto permite mostrar una fila con un control de botón personalizado, como una botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla de verificación en el control DetailsView. Este tipo de campo de fila se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra el comando integrado botones para realizar la edición, inserción o eliminación de las operaciones en el control DetailsView. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de fila permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen en el control DetailsView. |
| TemplateField | Muestra el contenido definido por el usuario para una fila en el control DetailsView según una plantilla especificada. Este tipo de campo de fila permite crear un campo de fila personalizado. |

De forma predeterminada, se establece la propiedad AutoGenerateRows en **true**, lo cual genera automáticamente un objeto de campo de fila enlazado para cada campo de tipo enlazable del origen de datos. Los tipos enlazables válidos son String, DateTime, Decimal, Guid y el conjunto de tipos primitivos. Cada campo, a continuación, se muestra en una fila como texto, en el orden en el que cada campo aparece en el origen de datos.

Generación automática de las filas proporciona una manera rápida y sencilla para mostrar cada campo en el registro. Sin embargo, para que use de DetailsView control capacidades avanzadas de que debe declarar explícitamente los campos de fila para incluir en el control DetailsView. Para declarar los campos de fila, establezca primero la **AutoGenerateRows** propiedad **false**. A continuación, agregue la apertura y cierre **&lt;campos&gt;** etiquetas entre las etiquetas apertura y cierre del control DetailsView. Por último, enumerar los campos de fila que se van a incluir entre la apertura y cierre **&lt;campos&gt;** etiquetas. Los campos de fila especificados se agregan a la colección de campos en el orden indicado. El **campos** colección le permite administrar mediante programación los campos de fila en el control DetailsView.

> [!NOTE]
> Campos no se agregan a la colección de campos de fila generados automáticamente.


## <a name="binding-to-data"></a>Enlace a datos

El control DetailsView puede enlazarse a un control de origen de datos, como **SqlDataSource** o AccessDataSource, o a cualquier origen de datos que implementa la interfaz System.Collections.IEnumerable, como System.Data.DataView, System.Collections.ArrayList y System.Collections.Hashtable.

Utilice uno de los métodos siguientes para enlazar el control DetailsView para el tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control DetailsView para el valor de identificador del control de origen de datos. El control DetailsView se enlaza automáticamente al control de origen de datos especificado. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa el **System.Collections.IEnumerable** de interfaz, establecer la propiedad de origen de datos del control DetailsView mediante programación al origen de datos y, a continuación, llame al método de enlazar datos.

## <a name="security"></a>Seguridad

Este control se puede utilizar para mostrar los datos proporcionados por el usuario, que pueden incluir script de cliente malintencionado. Compruebe que cualquier información que se envía desde un cliente de secuencias de comandos ejecutables, instrucciones SQL u otro código antes de mostrarla en la aplicación. ASP.NET proporciona una característica de validación de solicitudes de entrada para bloquear secuencias de comandos y código HTML en proporcionados por el usuario.

## <a name="data-operations"></a>Operaciones de datos

El control DetailsView proporciona funciones integradas que permiten al usuario actualizar, eliminar, insertar y desplazarse a través de los elementos del control. Cuando el control DetailsView se enlaza a un control de origen de datos, el control DetailsView puede aprovechar el origen de datos de las capacidades del control y proporcionar actualizar, eliminar, insertar y funcionalidad de paginación automática.

El control DetailsView puede proporcionar compatibilidad de actualización, eliminación, inserción y las operaciones de paginación con otros tipos de orígenes de datos; Sin embargo, debe proporcionar la implementación para estas operaciones en un controlador de eventos apropiado.

El control DetailsView puede agregar automáticamente una **CommandField** campo de fila con un botón Editar, eliminar o nuevo estableciendo las propiedades AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton a **true**, respectivamente. A diferencia de la eliminación botón (que elimina el registro seleccionado inmediatamente), cuando se hace clic en el botón Editar o nuevo, el control pasa a la edición de DetailsView modo o inserción, respectivamente. En modo de edición, el botón Editar se reemplaza por una actualización y un botón de cancelación. Controles de entrada que son adecuados para el tipo de datos del campo (por ejemplo, un cuadro de texto o un control de casilla de verificación) se muestran con el valor de un campo para el usuario modificar. Haga clic en el botón de actualización actualiza el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar abandona los cambios. Del mismo modo, en modo de inserción, el botón nuevo se reemplaza por una instrucción Insert y un botón Cancelar y controles de entrada vacíos se muestran para que el usuario especificar los valores para el nuevo registro.

El control DetailsView proporciona una característica de paginación, lo que permite al usuario navegar a otros registros del origen de datos. Cuando se habilita, los controles de navegación de página se muestran en una fila del localizador. Para habilitar la paginación, establezca la propiedad AllowPaging **true**. La fila del localizador puede personalizarse mediante las propiedades PagerStyle y PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizar la interfaz de usuario

Puede personalizar la apariencia del control DetailsView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumera las propiedades de estilo diferente.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control DetailsView. Cuando se establece esta propiedad, las filas de datos se muestran alternando la configuración de RowStyle y **AlternatingRowStyle** configuración. |
| CommandRowStyle | La configuración de estilo para la fila que contiene los botones de comando integrados en el control DetailsView. |
| EditRowStyle | La configuración de estilo para las filas de datos cuando el control DetailsView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo para la fila de datos vacía que se muestra en el control DetailsView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo para la fila de pie de página del control DetailsView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control DetailsView. |
| InsertRowStyle | La configuración de estilo para las filas de datos cuando el control DetailsView está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila del localizador del control DetailsView. |
| RowStyle | La configuración de estilo para las filas de datos en el control DetailsView. Cuando el **AlternatingRowStyle** también se establece la propiedad, las filas de datos se muestran alternando entre el **RowStyle** configuración y la **AlternatingRowStyle** configuración. |
| FieldHeaderStyle | La configuración de estilo para la columna de encabezado del control DetailsView. |

## <a name="events"></a>Eventos

El control de DetailsView proporciona varios eventos que se pueden programar. Esto permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumera los eventos admitidos por el control DetailsView. El control DetailsView también hereda estos eventos de sus clases base: enlace de datos, enlace de datos, Disposed, Init, carga, PreRender y representación.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón en el control DetailsView. |
| ItemCreated | Se produce después de que todos los objetos de DetailsViewRow se crean en el control DetailsView. Este evento se utiliza a menudo para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón Eliminar, pero después de que el control DetailsView elimina el registro del origen de datos. Este evento se utiliza a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón Eliminar, pero antes de DetailsView control elimina el registro del origen de datos. Este evento se utiliza a menudo para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón Insertar, pero después de que el control DetailsView inserta el registro. Este evento se utiliza a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de DetailsView control inserta el registro. Este evento se utiliza a menudo para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón Actualizar, pero después de que el control DetailsView actualiza la fila. Este evento se utiliza a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón Actualizar, pero antes de DetailsView control actualiza el registro. Este evento se utiliza a menudo para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control DetailsView cambia el modo (edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para realizar una tarea cuando el control DetailsView cambia los modos. |
| ModeChanging | Se produce antes de que el control DetailsView cambia el modo (edición, inserción o modo de solo lectura). Este evento se utiliza a menudo para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero después de que el control DetailsView administra la operación de paginación. Este evento suele utilizarse cuando necesite realizar una tarea después de que el usuario se desplaza a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de buscapersonas, pero antes de DetailsView control administra la operación de paginación. Este evento se utiliza a menudo para cancelar la operación de paginación. |

## <a name="the-menu-control"></a>El Control de menú

El control de menú en ASP.NET 2.0 está diseñado para ser un sistema de navegación completa. Puede ser enlazado a datos fácilmente con orígenes de datos jerárquicos como SiteMapDataSource.

Una estructura de controles de menú puede definirse mediante declaración o dinámicamente y consta de un único nodo raíz y cualquier número de nodos secundarios. El código siguiente define mediante declaración un menú para el control de menú.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

En el ejemplo anterior, el nodo Home.aspx es el nodo raíz. Todos los demás nodos están anidados dentro del nodo raíz en varios niveles.

Hay dos tipos de los menús que puede representar el control de menú; los menús estáticos y menús dinámicos. Los menús estáticos se componen de elementos de menú siempre están visibles. Menús dinámicos constan de elementos de menú que solo están visibles cuando el usuario mantiene el mouse sobre ellos con el mouse. A menudo pueden confundir a los clientes los menús estáticos con los menús que se definen de forma declarativa y menús dinámicos con los menús que están enlazados a datos en tiempo de ejecución. De hecho, los menús dinámicos y estáticos están relacionadas con el método de llenado. Los términos *estático* y *dinámica* consulte únicamente si el menú está estáticamente muestra de forma predeterminada o solo aparece cuando el usuario realice alguna acción.

El **StaticDisplayLevels** propiedad se utiliza para configurar el número de niveles del menú es estáticos y, por tanto, se muestran de forma predeterminada. En el ejemplo anterior, establecer el **StaticDisplayLevels** propiedad con un valor de 2 provocaría que el menú para mostrar estáticamente el nodo principal, el nodo de música y el nodo de películas. Todos los demás nodos dinámicamente aparecerá cuando el usuario mantiene el mouse sobre el nodo primario.

El **MaximumDynamicDisplayLevels** propiedad configura el número máximo de niveles dinámicos es capaz de mostrar el menú. Los menús dinámicos en un nivel superior al valor especificado por el **MaximumDynamicDisplayLevels** propiedad se descartan.

> [!NOTE]
> Es casi seguro que pueden surgir situaciones donde los menús no aparecen representar debido a la propiedad MaximumDynamicDisplayLevels. En esos casos, asegúrese de que la propiedad está establecida lo suficiente para permitir la visualización de los menús de los clientes.


## <a name="data-binding-the-menu-control"></a>El Control de menú de enlace de datos

El control de menú se puede enlazar a cualquier origen de datos jerárquicos como SiteMapDataSource o XMLDataSource. SiteMapDataSource es el método más usado para el enlace de datos a un control de menú porque lo fuentes de distribución en el archivo Web.sitemap y su esquema proporciona una API conocida para el control de menú. La siguiente lista muestra un archivo Web.sitemap simple.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Observe que hay un único elemento siteMapNode raíz, en este caso, el elemento principal. Varios atributos se pueden configurar para cada siteMapNode. Los atributos más utilizados son:

- **dirección URL** especifica la dirección URL que se muestra cuando un usuario hace clic en el elemento de menú. Si este atributo no está presente, el nodo simplemente registrará cuando se hace clic en.
- **título** especifica el texto que se muestra en el elemento de menú.
- **descripción** utiliza como documentación para el nodo. También se muestra como una información sobre herramientas cuando el mouse sobre el nodo.
- **siteMapFile** permite mapas anidados. Este atributo debe apuntar a un archivo de mapa del sitio ASP.NET con formato correcto.
- **roles** permite que la apariencia de un nodo controlarse mediante la reducción de seguridad ASP.NET.

Tenga en cuenta que aunque estos atributos son opcionales, el comportamiento del menú puede no ser lo que se espera si no se especifican. Por ejemplo, si la *url* se especifica el atributo pero la *descripción* no es el atributo, el nodo no serán visible y no habrá ninguna manera de navegar a la dirección URL especificada.

## <a name="controlling-a-menus-operation"></a>Controlar una operación de menús

Hay varias propiedades que afectan al funcionamiento de un control de menú de ASP.NET; el **orientación** propiedad, el **DisappearAfter** propiedad, el **StaticItemFormatString** propiedad y el **StaticPopoutImageUrl**propiedad son sólo algunas de ellas.

- El **orientación** puede establecerse en *horizontal* o *vertical* y controla si los elementos de menú estático se dispuestos horizontalmente en una fila o verticalmente y apilados en entre sí. Esta propiedad no afecta a los menús dinámicos.
- El **DisappearAfter** propiedad configura el tiempo que debe permanecer visible un menú dinámico después el mouse se haya movido fuera de él. El valor se especifica en milisegundos y el valor predeterminado es 500. Si se establece esta propiedad en un valor de -1 hará que el menú para nunca desaparecen automáticamente. En ese caso, el menú sólo desaparecerá cuando el usuario hace clic fuera del menú.
- El **StaticItemFormatString** propiedad facilita mantener coherente palabras en el sistema de menús. Al especificar esta propiedad, *{0}* debe especificarse en lugar de la descripción que aparece en el origen de datos. Por ejemplo, para que el elemento de menú de ejercicio 1 decir visite nuestra página de productos, etc., especificaría visite nuestra página de la StaticItemFormatString de {0}. En tiempo de ejecución, ASP.NET reemplazará cualquier aparición de {0} con la descripción correcta para el elemento de menú.
- El **StaticPopoutImageUrl** propiedad especifica la imagen que se utiliza para indicar que un nodo determinado de menú tiene nodos secundarios que pueden tener acceso al mantener el mouse sobre él. Menús dinámicos sigue usando la imagen predeterminada.

## <a name="templated-menu-controls"></a>Controles de menú con plantilla

El control de menú es un control con plantilla y permite dos elementos diferentes; el StaticItemTemplate y el DynamicItemTemplate. Mediante estas plantillas, se pueden agregar fácilmente controles de servidor o controles de usuario a los menús.

Para editar las plantillas en Visual Studio. NET, haga clic en el botón de etiqueta inteligente en el menú y elija Editar plantillas. A continuación, puede elegir entre modificar el StaticItemTemplate o el DynamicItemTemplate.

Los controles agregados a la StaticItemTemplate aparecerá en el menú estático cuando se carga la página. Los controles agregados a la DynamicItemTemplate aparecerá en todos los menús emergentes.

## <a name="menu-events"></a>Eventos de menú

El control de menú tiene dos eventos que son únicos el **MenuItemClicked** y **MenuItemDatabound** eventos.

El evento MenuItemClicked se desencadena cuando se hace clic en un elemento de menú. El evento MenuItemDatabound se desencadena cuando un elemento de menú está enlazado a datos. El **MenuEventArgs** que se pasa al evento controlador proporciona acceso al elemento de menú a través de la propiedad del elemento.

## <a name="controlling-a-menus-appearance"></a>Controlar la apariencia de los menús

También puede modificar la apariencia de un control de menú mediante uno o varios de los muchos estilos disponibles para los menús de formato. Entre éstos se encuentran **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**y **DynamicHoverStyle**. Estas propiedades se configuran mediante una cadena de estilo HTML estándar. Por ejemplo, la siguiente afectará el estilo de menús dinámicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si está utilizando alguno de los estilos al mantener el mouse, debe agregar una &lt;head&gt; elemento en la página con el *runat* elemento establecido en *server*.


Controles de menú también admiten el uso de temas de ASP.NET 2.0.

## <a name="the-treeview-control"></a>TreeView (Control)

TreeView (control) muestra los datos en una estructura de árbol. Al igual que con el control de menú, puede ser fácilmente los datos enlazados a cualquier origen de datos jerárquicos como SiteMapDataSource.

La primera pregunta que los clientes están probables que pregunte sobre el control TreeView de ASP.NET 2.0 es o no está relacionado con el WebControl de Internet Explorer de vista de árbol que estaba disponible para ASP.NET 1.x. No es así. El control TreeView de ASP.NET 2.0 se escribió desde el principio y ofrece mejora considerable sobre el TreeView WebControl de Internet Explorer que estaba disponible anteriormente.

No entraré en detalles sobre cómo enlazar un control TreeView a un mapa del sitio tal como se realiza exactamente igual que el control de menú. Sin embargo, el control de vista de árbol tiene algunas diferencias en la manera en que funciona.

De forma predeterminada, un control de vista de árbol aparece totalmente expandido. Para cambiar el nivel de expansión tras la carga inicial, modifique la **ExpandDepth** propiedad del control. Esto es especialmente importante en casos donde la vista de árbol es el enlace de datos al expandir nodos concretos.

## <a name="databinding-the-treeview-control"></a>Enlace de datos del Control TreeView

A diferencia del control de menú, la vista de árbol se presta para administrar grandes volúmenes de datos. Por lo tanto, además de enlace de datos a un SiteMapDataSource o XMLDataSource, la vista de árbol es a menudo los datos enlazados a un conjunto de datos u otros datos relacionales. En casos donde el control de vista de árbol se enlaza a grandes cantidades de datos, es mejor enlazar únicamente a los datos que es visibles en el control. Entonces, puede enlazar datos a datos adicionales como nodos de TreeView se expanden.

En estos casos, el **PopulateOnDemand** debe establecerse la propiedad de la vista de árbol en *true*. A continuación, debe proporcionar una implementación para el **TreeNodePopulate** método.

## <a name="data-binding-without-postback"></a>Enlace sin devolución de datos

Tenga en cuenta que cuando se expande un nodo en el ejemplo anterior por primera vez, la página devuelva los datos y se actualiza. Thats no hay problema en este ejemplo, pero pueden imaginar que es posible en un entorno de producción con una gran cantidad de datos. Un escenario mejor sería aquel en el que la vista de árbol se sigue dinámicamente rellenar sus nodos, pero sin una entrada de blog devuelve al servidor.

Estableciendo la **PopulateNodesFromClient** y **PopulateOnDemand** propiedades en true, el control TreeView de ASP.NET dinámicamente rellenará nodos que no tienen una devolución de datos. Cuando se expande el nodo primario, se realiza un XMLHttpRequest desde el cliente y se desencadena el evento OnTreeNodePopulate. El servidor responde con una isla de datos XML que se usa para datos enlaza los nodos secundarios.

ASP.NET crea dinámicamente el código de cliente que implementa esta funcionalidad. El &lt;script&gt; se generan las etiquetas que contienen la secuencia de comandos que señala a un archivo AXD. Por ejemplo, la siguiente lista muestra los vínculos de script para el código de script que genera el XMLHttpRequest.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Examinar el archivo AXD anteriormente en el explorador y abrirlo, se podrán visualizar el código que implementa el XMLHttpRequest. Este método evita que a los clientes de modificar el archivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlar el funcionamiento del Control TreeView

El control de vista de árbol tiene varias propiedades que afectan al funcionamiento del control. Las propiedades más obvias son el **ShowCheckBoxes**, **ShowExpandCollapse**, y **ShowLines**.

El **ShowCheckBoxes** propiedad afecta a los nodos muestran una casilla de verificación cuando se representan como si no. Los valores válidos para esta propiedad son **ninguno**, **raíz**, **primario**, **hoja**, y **todos los**. Estos afectan a TreeView (control) como se indica a continuación:

| **Valor de propiedad** | **Effect** |
| --- | --- |
| Ninguna | No se muestran casillas de verificación en todos los nodos. Ésta es la configuración predeterminada. |
| Raíz | Solo se muestra una casilla en el nodo raíz. |
| Elemento primario | Solo se muestra una casilla en los nodos que tienen nodos secundarios. Los nodos secundarios pueden ser nodos primarios o nodos de hoja. |
| Hoja | Se muestra una casilla de verificación solo en aquellos nodos que no tienen nodos secundarios. |
| Todas | Se muestra una casilla en todos los nodos. |

Cuando se utilizan las casillas de verificación, el **CheckedNodes** propiedad devolverá una colección de nodos de la vista de árbol que se comprueban tras la devolución.

El **ShowExpandCollapse** propiedad controla el aspecto de la imagen de expandir o contraer junto a los nodos raíz y el elemento primario. Si esta propiedad se establece en **false**, nodos de la vista de árbol se representan como hipervínculos y son expandir/contraer haciendo clic en el vínculo.

El **ShowLines** propiedad controla si se muestran líneas conectando los nodos primarios a los nodos secundarios. Cuando **false** (valor predeterminado), no se muestran líneas. Cuando **true**, el control TreeView usará imágenes de líneas en la carpeta especificada por el **LineImagesFolder** propiedad.

Para personalizar la apariencia de las líneas de TreeView, Visual Studio .NET 2005 incluye una herramienta de diseñador de línea. Puede tener acceso a esta herramienta mediante el botón de etiqueta inteligente en el control de vista de árbol como a continuación.


![](data-bound-controls/_static/image1.jpg)

**Figura 1**


Cuando selecciona el **personalizar imágenes de líneas** opción de menú, se inicia la herramienta Diseñador de línea ya que permiten configurar la apariencia de las líneas de TreeView.

## <a name="treeview-events"></a>Eventos de TreeView

TreeView (control) tiene los siguientes eventos únicos:

- SelectedNodeChanged tiene lugar cuando se selecciona un nodo en función de la **SelectAction** propiedad.
- TreeNodeCheckChanged se produce cuando se cambia un estado de casillas de nodos.
- TreeNodeExpanded se produce cuando se expande un nodo en función de la **SelectAction** propiedad.
- TreeNodeCollapsed se produce cuando se contrae un nodo.
- TreeNodeDataBound se produce cuando un nodo de datos enlazado.
- TreeNodePopulate se produce cuando un nodo se rellena.

El **SelectAction** propiedad le permite configurar qué evento se desencadena cuando se selecciona un nodo. La propiedad SelectAction proporciona las siguientes acciones:

- TreeNodeSelectAction.Expand genera TreeNodeExpanded cuando se selecciona el nodo.
- TreeNodeSelectAction.None no genera ningún evento cuando se selecciona el nodo.
- TreeNodeSelectAction.Select provoca el evento SelectedNodeChanged cuando se selecciona el nodo.
- TreeNodeSelectAction.SelectExpand provoca el evento SelectedNodeChanged y el evento TreeNodeExpanded cuando se selecciona el nodo.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

TreeView (control) proporciona muchas de las propiedades para controlar la apariencia del control con estilos. Las propiedades siguientes están disponibles.

| **Nombre de propiedad** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla el estilo de nodos cuando el mouse sobre ellos. |
| LeafNodeStyle | Controla el estilo de nodos hoja. |
| NodeStyle | Controla el estilo de todos los nodos. Estilos de nodo específico (por ejemplo, LeafNodeStyle) invalidan este estilo. |
| ParentNodeStyle | Controla el estilo de todos los nodos primarios. |
| RootNodeStyle | Controla el estilo para el nodo raíz. |
| SelectedNodeStyle | Controla el estilo del nodo seleccionado. |

Cada una de estas propiedades es de solo lectura. Sin embargo, aparecerán cada devolución un **TreeNodeStyle** objeto y las propiedades de ese objeto pueden modificarse mediante la *propiedad subpropiedad* formato. Por ejemplo, para establecer el **ForeColor** propiedad de la **SelectedNodeStyle**, utilizaría la siguiente sintaxis:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Tenga en cuenta que no se cierra la etiqueta anterior. Eso es porque cuando se usa la sintaxis declarativa que se muestra aquí, también debe incluir los nodos de controles TreeView en el código HTML.

También se pueden especificar las propiedades de estilo en el código mediante el *property.subproperty* formato. Por ejemplo, para establecer el **ForeColor** propiedad de la **RootNodeStyle** en el código, se utilizaría la siguiente sintaxis:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obtener una lista completa de las propiedades de estilo diferente, consulte la documentación de MSDN en el objeto TreeNodeStyle.


## <a name="the-sitemappath-control"></a>El Control SiteMapPath

El control SiteMapPath proporciona un control de navegación de pan detallada para los programadores de ASP.NET. Al igual que los demás controles de navegación, puede ser fácilmente datos enlazan a datos jerárquicos orígenes como XmlDataSource o SiteMapDataSource.

Un control SiteMapPath se compone de objetos SiteMapNodeItem. Hay tres tipos de nodos; el nodo raíz, los nodos primarios y el nodo actual. El nodo raíz es el nodo en la parte superior de la estructura jerárquica. El nodo actual representa la página actual. Todos los demás nodos son los nodos primarios.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlar el funcionamiento del Control SiteMapPath

Las propiedades que controlan el funcionamiento del control SiteMapPath son los siguientes:

| **Property** | **Descripción de propiedad** |
| --- | --- |
| ParentLevelsDisplayed | Controla cuántos nodos primarios se muestran. El valor predeterminado es -1 que no se impone ninguna restricción en el número de nodos primarios mostrados. |
| PathDirection | Controla la dirección de la SiteMapPath. Los valores válidos son RootToCurrent (valor predeterminado) y CurrentToRoot. |
| PathSeparator | Una cadena que controla el carácter que separa los nodos en un control SiteMapPath. El valor predeterminado es:. |
| RenderCurrentNodeAsLink | Un valor booleano que controla si el nodo actual se representa como un vínculo. El valor predeterminado es False. |
| SkipLinkText | Ayuda con la accesibilidad cuando se ve la página por los lectores de pantalla. Esta propiedad permite a los lectores de pantalla omitir el control SiteMapPath. Para deshabilitar esta característica, establezca la propiedad en String.Empty. |

## <a name="templated-sitemappath-controls"></a>Controles con plantilla SiteMapPath

El SiteMapControl es un control con plantilla y, por lo tanto, puede definir varias plantillas para su uso en Mostrar el control. Para editar las plantillas en un control SiteMapPath, haga clic en el botón de etiqueta inteligente en el control y elija Editar plantillas en el menú. Esto muestra el menú de SiteMapTasks tal y como se muestra a continuación donde puede elegir entre las distintas plantillas disponibles.


![](data-bound-controls/_static/image2.jpg)

**Figura 2**


El **NodeTemplate** plantilla hace referencia a cualquier nodo SiteMapPath. Si el nodo es un nodo raíz o el nodo actual y un **RootNodeTemplate** o **CurrentNodeTemplate** está configurado, se invalida la NodeTemplate.

## <a name="sitemappath-events"></a>Eventos de SiteMapPath

El control SiteMapPath tiene dos eventos que no se derivan de la clase del Control; el **ItemCreated** eventos y la **ItemDataBound** eventos. El evento ItemCreated se produce cuando se crea un elemento SiteMapPath. ItemDataBound se genera cuando se llama al método de enlazar datos durante el enlace de datos de un nodo SiteMapPath. A **SiteMapNodeItemEventArgs** objeto proporciona acceso a la SiteMapNodeItem específico a través de la propiedad del elemento.

## <a name="controlling-appearance-with-styles"></a>Controlar la apariencia con estilos

Los estilos siguientes están disponibles para el formato de un control SiteMapPath.

| **Nombre de propiedad** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla el estilo del texto del nodo actual. |
| RootNodeStyle | Controla el estilo del texto para el nodo raíz. |
| NodeStyle | Controla el estilo del texto de todos los nodos, suponiendo que no se aplica un CurrentNodeStyle o RootNodeStyle. |

La propiedad NodeStyle se reemplaza por la CurrentNodeStyle o el RootNodeStyle. Cada una de estas propiedades es de sólo lectura y devuelve un **estilo** objeto. Para que afecte a la apariencia de un nodo usando una de estas propiedades, debe establecer las propiedades del objeto de estilo que se devuelve. Por ejemplo, el código siguiente cambia la propiedad de color de primer plano del nodo actual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propiedad también se puede aplicar mediante programación como sigue:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si se aplica una plantilla, no se aplicará el estilo.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Práctica 1: Configurar un Control de menú de ASP.NET

1. Crear un nuevo sitio Web.
2. Agregue un archivo de mapa del sitio, seleccione archivo, nuevo, archivo y elija mapa del sitio en la lista de plantillas de archivo.
3. Abra el mapa del sitio (Web.sitemap de forma predeterminada) y modifíquelo para que se asemeje al igual que la lista siguiente. Las páginas a la que se están estableciendo vínculos en el archivo de asignación de sitio no existe realmente, pero que no sea un problema para este ejercicio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra el formulario Web de manera predeterminada en la vista Diseño.
5. En la sección de navegación del cuadro de herramientas, agregue un nuevo control de menú a la página.
6. En la sección de datos del cuadro de herramientas, agregue un SiteMapDataSource nueva. SiteMapDataSource utilizará automáticamente el archivo Web.sitemap en su sitio. (El archivo Web.sitemap *debe* en la carpeta raíz del sitio.)
7. Haga clic en el control de menú y, a continuación, haga clic en el botón de etiqueta inteligente para mostrar el cuadro de diálogo de tareas de menú.
8. En la lista desplegable Elegir origen de datos, seleccione SiteMapDataSource1.
9. Haga clic en el vínculo Autoformato y elija un formato para el menú.
10. En el panel Propiedades, establezca la **StaticDisplayLevels** propiedad en 2. El control de menú debe mostrar ahora el nodo principal, productos y servicios en el diseñador.
11. Busque la página en el explorador para usar el menú. (Dado que las páginas se ha agregado a la asignación de sitio no existen en realidad, verá un error cuando intente y llega hasta ellos.)

Probar a cambiar el StaticDisplayLevels y las propiedades de MaximumDynamicDisplayLevels y ver cómo afectan a cómo se representa el menú.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Práctica 2: Enlazar dinámicamente un Control TreeView

Este ejercicio se supone que dispone de SQL Server que se ejecuta de forma local y que la base de datos Northwind esté presente en la instancia de SQL Server. Si no se cumplen estas condiciones, cambie la cadena de conexión en el ejemplo. Tenga en cuenta que también debe especificar la autenticación de SQL Server en lugar de una conexión de confianza.

1. Crear un nuevo sitio Web.
2. Cambie a la vista de código para Default.aspx y reemplace todo el código por el código que aparece a continuación. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Guarde la página como treeview.aspx.
4. Busque la página.
5. Cuando la página se muestra por primera vez, ver el código fuente de la página en el explorador. Tenga en cuenta que sólo los nodos visibles se enviaron al cliente.
6. Haga clic en el signo más situado junto a cualquier nodo.
7. Ver código fuente en la página de nuevo. Observe que ahora están presentes los nodos recién mostrados.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratorio 3: Detalles de vista y edición de datos mediante un GridView y DetailsView

1. Crear un nuevo sitio Web.
2. Agregue un nuevo archivo de web.config para el sitio Web.
3. Agregar una cadena de conexión en el archivo web.config tal y como se muestra a continuación: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Debe cambiar la cadena de conexión en función de su entorno.
4. Guarde y cierre el archivo web.config.
5. Abra el archivo Default.aspx y agregue un nuevo control SqlDataSource.
6. Cambie el identificador del control SqlDataSource para **productos**.
7. En el **tareas SqlDataSource** menú, haga clic en **Configurar origen de datos**.
8. Seleccione **Northwind** en la lista desplegable de la conexión y haga clic en siguiente.
9. Seleccione **productos** desde el **nombre** desplegable y comprobar la **ProductID**, **ProductName**, **UnitPrice**, y **UnitsInStock** casillas de verificación tal y como se muestra a continuación. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Haga clic en **Siguiente**.
11. Haga clic en **Finalizar**.
12. Cambie a la vista de origen y examine el código generado. Observe el **SelectCommand**, **DeleteCommand**, **InsertCommand**, y **UpdateCommand** que se agregaron a la SqlDataSource control. Tenga en cuenta también los parámetros que se han agregado.
13. Cambie a la vista Diseño y agregar un nuevo control GridView a la página.
14. Seleccione **productos** desde el **Elegir origen de datos** lista desplegable.
15. Comprobar **Habilitar paginación** y **Habilitar selección** tal y como se muestra a continuación. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Haga clic en el **Editar columnas** vincular y asegúrese de que **generar automáticamente campos** está activada.
17. Haga clic en **Aceptar**.
18. Con el control GridView seleccionado, haga clic en el botón junto a la **DataKeyNames** propiedad en el panel de propiedades.
19. Seleccione **ProductID** desde el **campos de datos disponibles** lista y haga clic en el **&gt;** botón para agregarlo.
20. Haga clic en Aceptar.
21. Agregar un nuevo control SqlDataSource a la página.
22. Cambie el identificador del control SqlDataSource para **detalles**.
23. En el menú tareas de SqlDataSource, elija **Configurar origen de datos**.
24. Elija **Northwind** desde la lista desplegable y haga clic en **siguiente**.
25. Seleccione <strong>productos</strong> desde el <strong>nombre</strong> lista desplegable y compruebe el <strong> \</ strong > * checkbox en el <strong>columnas</strong> cuadro de lista.
26. Haga clic en el **donde** botón.
27. Seleccione **ProductID** desde el **columna** lista desplegable.
28. Seleccione **=** en la lista desplegable operador.
29. Seleccione **Control** desde el **origen** lista desplegable.
30. Seleccione **GridView1** desde el **Id. de Control** lista desplegable.
31. Haga clic en el **agregar** botón para agregar la cláusula WHERE.
32. Haga clic en **Aceptar**.
33. Haga clic en el **avanzadas** botón y comprobar la **generar instrucciones INSERT, UPDATE y DELETE** casilla de verificación.
34. Haga clic en **Aceptar**.
35. Haga clic en **siguiente** y haga clic en **finalizar**.
36. Agregar un control DetailsView a la página.
37. En el **Elegir origen de datos** de lista desplegable, elija **detalles**.
38. Compruebe el **Habilitar edición** casilla tal y como se muestra a continuación. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Guarde la página y examinar Default.aspx.
40. Haga clic en el **seleccione** vínculo situado junto a diferentes registros para ver la actualización de DetailsView automáticamente.
41. Haga clic en el **editar** vínculo en el control DetailsView.
42. Realiza un cambio en el registro y haga clic en **actualización**.
