---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Mostrar datos con los controles DataList y Repeater (C#) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores hemos usado el control GridView para mostrar los datos. A partir de este tutorial, nos centramos en la creación de patrones comunes de generación de informes con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: ca585c51ae12be2395897044f6d0fd1857d3e357
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382707"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Mostrar datos con los controles DataList y Repeater (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) o [descargar PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> En los tutoriales anteriores hemos usado el control GridView para mostrar los datos. A partir de este tutorial, nos centramos en la creación de patrones comunes de generación de informes con los controles DataList y Repeater, empezando por los aspectos básicos de la visualización de datos con estos controles.


## <a name="introduction"></a>Introducción

En todos los ejemplos a lo largo de los últimos 28 tutoriales, si se necesita mostrar varios registros desde un origen de datos se convierte en el control GridView. El control GridView representa una fila para cada registro en el origen de datos, mostrar los campos de datos de registro s en columnas. Aunque el control GridView hace muy sencillo para mostrar, paginar, ordenación, edición y eliminación de datos, su aspecto es un poco anguloso. Además, el responsable del marcado para la estructura de GridView s se ha corregido incluye un elemento HTML `<table>` con una fila de tabla (`<tr>`) para cada registro y una celda de tabla (`<td>`) para cada campo.

Para proporcionar un mayor grado de personalización de la apariencia y el marcado presentado al mostrar varios registros, ASP.NET 2.0 ofrece los controles DataList y Repeater (que también estaban disponibles en la versión de ASP.NET 1.x). Los controles DataList y Repeater representan su contenido con plantillas en lugar de BoundFields, CheckBoxFields, ButtonFields y así sucesivamente. Al igual que el control GridView, el control DataList se representa como HTML `<table>`, pero permite para los datos de varios registros de origen que se mostrarán por cada fila de tabla. El control Repeater, por otro lado, no representa ningún marcado adicional que lo que explícitamente especifique y es un candidato ideal cuando se necesita un control preciso sobre el marcado que genera.

A través de los tutoriales después docenas o es así, veremos la creación de patrones comunes de generación de informes con los controles DataList y Repeater, a partir de los aspectos básicos de la visualización de datos con estas plantillas de controles. Veremos cómo dar formato a estos controles, cómo modificar el diseño de los registros de origen de datos en DataList, escenarios comunes de detalles y maestras, formas para editar y eliminar datos, cómo paginar los registros y así sucesivamente.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Paso 1: Agregar los controles DataList y Repeater páginas Web del Tutorial

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que necesitaremos para este tutorial y los tutoriales de pocos siguientes trabaja con la presentación de datos mediante los controles DataList y Repeater. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterBasics`. A continuación, agregue las siguientes cinco páginas ASP.NET en esta carpeta, configurado para usar la página principal de mantenerlos todos `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Cree una carpeta DataListRepeaterBasics y agregar las páginas del Tutorial de ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: crear un `DataListRepeaterBasics` carpeta y agregue las páginas del Tutorial de ASP.NET


Abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera el mapa del sitio y los tutoriales se muestra en la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Para tener la visualización de la lista con viñetas los tutoriales de DataList y Repeater que crearemos, es necesario agregarlos al mapa del sitio. Abra el `Web.sitemap` archivo y agregue el siguiente marcado después de las marcas de nodo de mapa de sitio de agregar botones personalizados:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Paso 2: Mostrar la información de producto con el control DataList

Al igual que FormView, el control DataList s salida representada depende de las plantillas en lugar de BoundFields, CheckBoxFields y así sucesivamente. A diferencia de FormView, el control DataList está diseñado para mostrar un conjunto de registros en lugar de a un solitarios. Permiten s empezar este tutorial con una mirada a la información de producto de enlace en un control DataList. Comience abriendo la `Basics.aspx` página en el `DataListRepeaterBasics` carpeta. A continuación, arrastre a un control DataList desde el cuadro de herramientas hasta el diseñador. Como se muestra en la figura 4, antes de especificar las plantillas de DataList s, en el diseñador se muestra como un cuadro gris.


[![Arrastre al control DataList desde el cuadro de herramientas hasta el diseñador](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: arrastre el control DataList desde el cuadro de herramientas en el Diseñador de ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Desde el control DataList s de etiquetas inteligentes, agregue un nuevo origen ObjectDataSource y configurarlo para que use el `ProductsBLL` clase s `GetProducts` método. Puesto que re de la creación de un control DataList de solo lectura en este tutorial, establecemos la lista desplegable en (None) en la s de Asistente para insertar, actualizar y eliminar las fichas.


[![Optar por crear un nuevo origen ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: participar para crear un nuevo origen ObjectDataSource ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Recuperar información acerca de todos los productos con el método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperar información acerca de todos los de los productos usando el `GetProducts` método ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Después de configurar el origen ObjectDataSource y asociarlo con el control DataList a través de su etiqueta inteligente, Visual Studio creará automáticamente un `ItemTemplate` en el control DataList que muestra el nombre y valor de cada campo de datos devuelto por el origen de datos (consulte la a continuación de marcado). Este valor predeterminado `ItemTemplate` apariencia es idéntico de las plantillas se crean automáticamente cuando se enlaza un origen de datos a FormView a través del diseñador.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Recuerde que al enlazar un origen de datos a un control FormView a través de la etiqueta inteligente de FormView s, Visual Studio creó un `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate`. Con el control DataList, sin embargo, sólo un `ItemTemplate` se crea. Esto es porque el control DataList no tiene integrado de la mismo edición e inserción de compatibilidad que ofrece FormView. El control DataList contener eventos relacionados con la edición y la eliminación y edición y eliminación de soporte técnico no se pueden agregar con un poco de código, pero hay s ningún soporte de fábrica simple como con FormView. Veremos cómo incluir la edición y eliminación de soporte técnico con el control DataList en un futuro tutorial.


Permiten s Tómese un momento para mejorar el aspecto de esta plantilla. En lugar de mostrar todos los campos de datos, permiten s Mostrar sólo el nombre del producto, proveedor, categoría, cantidad por unidad y el precio unitario. Además, s permiten mostrar el nombre de un `<h4>` de encabezado y diseñar los campos restantes con un `<table>` bajo el encabezado.

Para realizar estos cambios, puede use características en el diseñador desde el control DataList s smart tag haga clic en el vínculo Editar plantillas o puede modificar la plantilla manualmente mediante la sintaxis declarativa de s de página de edición de plantillas. Si usa la opción de editar plantillas en el diseñador, el marcado resultante puede no coincidir con el siguiente marcado exactamente, pero cuando se ven a través de un explorador debe ser muy similar a la pantalla se muestra en la figura 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> El ejemplo anterior se usa Web Label controles cuya propiedad `Text` propiedad se asigna el valor de la sintaxis de enlace de datos. Como alternativa, podríamos haber omitimos las etiquetas por completo, escribiendo en la sintaxis del enlace de datos. Es decir, en lugar de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` nos podríamos usó en su lugar la sintaxis declarativa `<%# Eval("CategoryName") %>`.


Sin embargo, se deja en los controles Web de la etiqueta, ofrece dos ventajas. En primer lugar, proporciona una forma más fácil para dar formato a los datos según los datos, como veremos en el siguiente tutorial. En segundo lugar, la opción de editar plantillas en la sintaxis de enlace de datos declarativo de diseñador t para mostrar que aparece fuera algo de control Web. En su lugar, la interfaz de edición de plantillas está diseñada para facilitar el trabajo con marcado estático y Web controles y se da por supuesto que se realizará ningún enlace de datos mediante el cuadro de diálogo Editar DataBindings, que es accesible desde las etiquetas inteligentes de controles Web.

Por lo tanto, cuando se trabaja con el control DataList, que proporciona la opción de editar las plantillas a través del diseñador, prefiero usar los controles Web de la etiqueta para que el contenido sea accesible a través de la interfaz de edición de plantillas. Como veremos en breve, el control Repeater requiere que el contenido de plantilla s pueden editar desde la vista del origen. Por lo tanto, al diseñar las plantillas de s Repeater, a menudo a omitir la etiqueta de Web controles a menos que sepa que necesitará para dar formato a la apariencia de los datos enlazados texto basado en lógica de programación.


[![Cada producto s salida es representar utilizando el control DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: cada producto s salida es representar utilizando el control DataList s `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Paso 3: Mejorar la apariencia del control DataList

Al igual que el control GridView, el control DataList ofrece una serie de propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, y así sucesivamente. Cuando se trabaja con los controles GridView y DetailsView, hemos creado los archivos de máscara en el `DataWebControls` tema que predefinidas el `CssClass` las propiedades de estos dos controles y el `CssClass` propiedad para varias de sus subpropiedades (`RowStyle` `HeaderStyle`, y así sucesivamente). Permiten s lo mismo para el control DataList.

Como se describe en el [mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, un archivo de máscara especifica las propiedades de relacionadas con la apariencia predeterminada de un control Web; un tema es una colección de archivos de máscara, imagen, CSS y JavaScript que definen un determinado aspecto de un sitio Web. En el *mostrar datos con el origen ObjectDataSource* tutorial, creamos un `DataWebControls` tema (que se implementa como una carpeta dentro de la `App_Themes` carpeta) en la actualidad, que tiene dos archivos de máscara: `GridView.skin` y `DetailsView.skin`. Permitir s Agregar un tercer archivo de máscara para especificar la configuración de estilo predefinido para el control DataList.

Para agregar un archivo de máscaras, haga doble clic en el `App_Themes/DataWebControls` carpeta, elija Agregar un nuevo elemento y seleccione la opción de archivo de máscara de la lista. Asigne al archivo el nombre `DataList.skin`.


[![Cree un nuevo archivo de máscara denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: crear una nueva máscara de archivo con nombre `DataList.skin` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Utilice el siguiente marcado para el `DataList.skin` archivo:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Esta configuración asigna las mismas clases CSS a las propiedades de DataList apropiadas utilizadas con los controles GridView y DetailsView. Las clases CSS que se usa aquí `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, y así sucesivamente se definen en el `Styles.css` de archivos y se agregaron en los tutoriales anteriores.

Con la adición de este archivo de máscara, se actualiza la apariencia DataList en el diseñador (es posible que deba actualizar la vista de diseñador para ver los efectos del nuevo archivo de máscara; en el menú Ver, elija actualizar). Como se muestra en la figura 10, cada producto alterna tiene un color de fondo de color rosa claro.


[![Cree un nuevo archivo de máscara denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: crear una nueva máscara de archivo con nombre `DataList.skin` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Paso 4: Explorar el control DataList s otras plantillas

Además el `ItemTemplate`, el control DataList admite seis otras plantillas opcionales:

- `HeaderTemplate` Si se proporciona, agrega una fila de encabezado a la salida y se usa para representar esta fila
- `AlternatingItemTemplate` utilizado para representar los elementos alternos
- `SelectedItemTemplate` utilizado para representar el elemento seleccionado; el elemento seleccionado es el elemento cuyo índice se corresponde con el control DataList s [ `SelectedIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilizado para representar el elemento que se está editando
- `SeparatorTemplate` Si se proporciona, se agrega un separador entre cada elemento y se usa para representar este separador
- `FooterTemplate` -Si se proporciona, se agrega una fila de pie de página a la salida y se usa para representar esta fila

Al especificar el `HeaderTemplate` o `FooterTemplate`, el control DataList agrega una fila de encabezado o pie de página adicional a la salida representada. Al igual que con el control GridView s encabezado y pie filas, el encabezado y pie de página en un control DataList no están enlazados a datos. Por lo tanto, cualquier sintaxis de enlace de datos en el `HeaderTemplate` o `FooterTemplate` que intenta obtener acceso a datos enlazados devolverá una cadena vacía.

> [!NOTE]
> Como hemos visto en el [mostrar información de resumen en el pie de página de GridView s](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial, mientras que las filas de encabezado y pie de página don sintaxis de enlace de datos de soporte técnico de t, información específica de datos se puede insertar directamente en estas filas de la GridView s `RowDataBound` controlador de eventos. Esta técnica se puede usar para ambos calculan totales acumulados u otra información de los datos enlazados al control, así como asignar esa información al pie de página. Este mismo concepto se puede aplicar a los controles DataList y Repeater; la única diferencia es que para los controles DataList y Repeater, cree un controlador de eventos para el `ItemDataBound` evento (en lugar de para el `RowDataBound` eventos).


En nuestro ejemplo, permiten s tienen el título del producto información que se muestra en la parte superior de los resultados de DataList s en un `<h3>` encabezado. Para ello, agregue un `HeaderTemplate` con el formato adecuado. En el diseñador, esto puede realizarse haciendo clic en el vínculo Editar plantillas en la etiqueta inteligente de DataList s, elegir la plantilla de encabezado de la lista desplegable y escribir en el texto después de elegir la opción de encabezado 3 de la lista desplegable de estilo de lista (consulte la figura 11).


[![Agregar un elemento HeaderTemplate con la información de producto de texto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: agregar un `HeaderTemplate` con la información de producto de texto ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Como alternativa, se puede agregar mediante declaración, escriba el siguiente marcado dentro de la `<asp:DataList>` etiquetas:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Para agregar un poco de espacio entre cada lista de productos, permiten s Agregar un `SeparatorTemplate` que incluya una línea entre cada sección. La etiqueta a la regla horizontal (`<hr>`), agrega un divisor de este tipo. Crear el `SeparatorTemplate` para que tenga el siguiente marcado:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Al igual que el `HeaderTemplate` y `FooterTemplates`, el `SeparatorTemplate` no está enlazado a todos los registros del origen de datos y, por tanto, no directamente del origen de datos enlazados al control DataList de registros de acceso.


Después de realizar esta adición, al ver la página mediante un explorador debe ser similar a la figura 12. Tenga en cuenta la fila de encabezado y la línea entre cada lista de productos.


[![El control DataList incluye una fila de encabezado y una regla Horizontal entre cada lista de productos](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: el control DataList incluye una fila de encabezado y un Horizontal regla entre cada listado de productos ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Paso 5: Representar marcado específico con el Control Repeater

Si lo hace un origen de la vista desde el explorador cuando se visita el ejemplo DataList de figura 12, verá que el control DataList emite un HTML `<table>` que contiene una fila de tabla (`<tr>`) con una celda de tabla (`<td>`) para cada elemento asociado a la DataList. Esta salida, de hecho, es idéntica a lo que se emitiría desde un control GridView con TemplateField único. Como veremos en un futuro tutorial, el control DataList permitir mayor personalización de la salida, lo que nos permite mostrar varios registros de origen de datos por cada fila de tabla.

¿Qué ocurre si no le t desea emitir un elemento HTML `<table>`, aunque? Para control total y completado sobre el marcado generado por un control Web de datos, debemos usar el control Repeater. Al igual que el control DataList, el control Repeater se construye en función de las plantillas. Sin embargo, el control Repeater, solo ofrece las siguientes cinco plantillas:

- `HeaderTemplate` Si se proporciona, se agrega el marcado especificado antes de los elementos
- `ItemTemplate` usada para representar elementos
- `AlternatingItemTemplate` Si se proporciona, se usa para representar los elementos alternos
- `SeparatorTemplate` Si se proporciona, se agrega el marcado especificado entre cada elemento
- `FooterTemplate` -Si se proporciona, se agrega el marcado especificado después de los elementos

En ASP.NET 1.x, el control Repeater control se utiliza normalmente para mostrar una lista con viñetas cuyos datos provienen de algún origen de datos. En tal caso, el `HeaderTemplate` y `FooterTemplates` contendría la apertura y cierre `<ul>` etiquetas, respectivamente, mientras el `ItemTemplate` contendría `<li>` elementos con la sintaxis de enlace de datos. Este enfoque todavía se puede usar en ASP.NET 2.0 como vimos en los dos ejemplos de la [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial:

- En el `Site.master` página maestra, se usó un control Repeater para mostrar una lista con viñetas del contenido de mapa del sitio de nivel superior (Basic Reporting, Filtering Reports, Customized Formatting etc.); otro repetidor anidado se usó para mostrar las secciones de los elementos secundarios de la secciones de nivel superior
- En `SectionLevelTutorialListing.ascx`, se usó un control Repeater para mostrar una lista con viñetas de las secciones de los elementos secundarios de la sección de asignación de sitio actual

> [!NOTE]
> ASP.NET 2.0 introduce el nuevo [control BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), que se puede enlazar a un control de origen de datos con el fin de mostrar una lista con viñetas. Con el control BulletedList no necesitamos especificar ninguno de lo HTML relacionados con la lista; en su lugar, nos basta con indicar el campo de datos para mostrar como el texto para cada elemento de lista.


El control Repeater actúa como un bloque catch todos los datos de control Web. Si no es un control existente que genera el marcado necesario, puede utilizarse el control Repeater. Para ilustrar el uso del repetidor, permiten s tiene la lista de categorías mostrado sobre el control DataList de información de producto creado en el paso 2. En particular, permiten s tienen las categorías que se muestran en una sola fila HTML `<table>` con cada categoría que se muestra como una columna en la tabla.

Para lograr esto, iniciar, arrastre un control Repeater desde el cuadro de herramientas hasta el diseñador, anteriormente el control DataList de información de producto. Al igual que con el control DataList, el control Repeater muestra inicialmente como un cuadro gris hasta que se han definido sus plantillas.


[![Agregar un control Repeater al diseñador](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: agregar un control Repeater al diseñador ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Sólo una opción de allí s en la etiqueta inteligente del control Repeater s: Elegir origen de datos. Optar por crear un nuevo origen ObjectDataSource y configúrelo para utilizar el `CategoriesBLL` clase s `GetCategories` método.


[![Crear un nuevo origen ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figura 14**: crear un nuevo origen ObjectDataSource ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurar el origen ObjectDataSource para usar la clase CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Recuperar información acerca de todas las categorías mediante el método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperar información acerca de todos los de la categorías mediante el `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


A diferencia de DataList, Visual Studio no crea automáticamente una plantilla ItemTemplate para el control Repeater después de enlazarla a un origen de datos. Además, las plantillas de Repeater s no se puede configurar a través del diseñador y se deben especificar mediante declaración.

Para mostrar las categorías como una sola fila `<table>` con una columna para cada categoría, se necesita el control Repeater para emitir marcado similar al siguiente:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Puesto que el `<td>Category X</td>` texto es la parte que se repite, esta descripción aparecerá en el control Repeater s ItemTemplate. El marcado que aparece antes de que - `<table><tr>` -se colocarán en el `HeaderTemplate` mientras el marcado final - `</tr></table>` -se colocará en el `FooterTemplate`. Para especificar esta configuración de plantilla, vaya a la parte de la página ASP.NET declarativa haciendo clic en el botón de origen en la esquina inferior izquierda y escriba la siguiente sintaxis:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

El control Repeater emite el marcado según lo especificado por sus plantillas, nada más y nada menos preciso. Figura 17 se muestra la salida de s Repeater cuando se ve mediante un explorador.


[![HTML de una sola fila &lt;tabla&gt; enumera cada categoría en una columna independiente](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: HTML de una sola fila `<table>` enumera cada categoría en una columna independiente ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Paso 6: Cómo mejorar la apariencia del repetidor

Puesto que el control Repeater emite con precisión el marcado especificado por sus plantillas, debe proceder como no nos debe sorprender que no hay ninguna propiedad relacionada con el estilo para el control Repeater. Para modificar la apariencia del contenido generado por el control Repeater, debemos agregar manualmente el contenido HTML o CSS necesario directamente en las plantillas de Repeater s.

En nuestro ejemplo, permiten s tiene las columnas de la categoría alternar los colores de fondo, al igual que con las filas alternas en el control DataList. Para lograr esto, necesitamos asignar el `RowStyle` clase CSS para cada elemento del control Repeater y `AlternatingRowStyle` clase CSS para cada elemento alterno de Repeater mediante el `ItemTemplate` y `AlternatingItemTemplate` plantillas, así:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Permitir s también agregar una fila de encabezado a la salida con el texto de las categorías de productos. Dado que no queremos t sabe cuántas columnas nuestro resultante `<table>` constará de, la manera más sencilla de generar una fila de encabezado que se garantiza que abarcan todas las columnas es usar *dos* `<table>` s. La primera `<table>` contendrá dos filas de la fila de encabezado y una fila que contiene la única fila segunda, `<table>` que tiene una columna para cada categoría en el sistema. Es decir, queremos que emita el siguiente marcado:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

La siguiente `HeaderTemplate` y `FooterTemplate` como resultado el formato deseado:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Figura 18 se muestra el control Repeater una vez realizados estos cambios.


[![Las columnas de la categoría alternativo en el Color de fondo e incluye una fila de encabezado](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: la categoría de las columnas alternativas en Color de fondo e incluye una fila de encabezado ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Resumen

Mientras el control GridView facilita la tarea mostrar, editar, eliminar, ordenar y paginar los datos, el aspecto es muy anguloso y cuadrícula. Para obtener más control sobre la apariencia, es necesario activar a los controles DataList o Repeater. Ambos de estos controles muestran un conjunto de registros mediante plantillas en lugar de BoundFields, CheckBoxFields y así sucesivamente.

El control DataList representa como HTML `<table>` que, de forma predeterminada, muestra cada registro del origen de datos en una sola fila de tabla, al igual que un control GridView con un único TemplateField. Sin embargo, como veremos en un futuro tutorial, el control DataList permiten varios registros que se mostrarán por fila de la tabla. El control Repeater, por otro lado, emite estrictamente el marcado especificado en sus plantillas; no agrega ningún marcado adicional y, por tanto, normalmente se usa para mostrar los datos en los elementos HTML que no sea un `<table>` (como en una lista con viñetas).

Si bien los controles DataList y Repeater ofrecen más flexibilidad en su salida representada, carecen de muchas de las características integradas que se encuentra en el control GridView. Como en próximos tutoriales, examinaremos algunas de estas características se pueden conectar en sin demasiado esfuerzo, pero ¿tenga en cuenta que utilizando al control DataList o Repeater en lugar de GridView limitar las características que puede utilizar sin tener que implementar esas características usted mismo.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Yaakov Ellis, Liz Shulok, Randy Schmidt y Stacy Park. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
